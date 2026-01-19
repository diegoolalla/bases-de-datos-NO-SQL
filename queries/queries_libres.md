# Queries Libres - Análisis Avanzado del Dataset de Películas

Este documento contiene 3 queries libres que realizan análisis adicionales sobre el dataset de películas.

---

## Query Libre 1: Películas Ganadoras de Múltiples Oscars

**Objetivo**: Encontrar películas que hayan ganado 3 o más Oscars y analizar sus características.

```javascript
db.movies.aggregate([
  {
    $match: {
      "awards.oscars": { $gte: 3 }
    }
  },
  {
    $project: {
      title: 1,
      year: 1,
      genres: 1,
      rating: 1,
      oscars: "$awards.oscars",
      nominations: "$awards.nominations",
      winRate: {
        $multiply: [
          { $divide: ["$awards.wins", "$awards.nominations"] },
          100
        ]
      },
      _id: 0
    }
  },
  {
    $sort: { oscars: -1, rating: -1 }
  }
])
```

**Descripción**: 
- Filtra películas con 3 o más Oscars ganados
- Calcula el porcentaje de victorias sobre nominaciones (win rate)
- Ordena por número de Oscars y rating
- Permite identificar las películas más premiadas y su calidad

**Utilidad**: Este análisis ayuda a identificar películas de alta calidad reconocidas por la Academia y entender qué características tienen en común las películas más premiadas.

---

## Query Libre 2: Análisis de Directores Prolíficos y su Rendimiento

**Objetivo**: Identificar directores con múltiples películas en el dataset y analizar su desempeño promedio.

```javascript
db.movies.aggregate([
  {
    $unwind: "$directors"
  },
  {
    $group: {
      _id: "$directors",
      numberOfMovies: { $sum: 1 },
      avgRating: { $avg: "$rating" },
      avgRuntime: { $avg: "$runtime" },
      totalOscars: { $sum: "$awards.oscars" },
      movies: { 
        $push: { 
          title: "$title", 
          year: "$year", 
          rating: "$rating" 
        } 
      },
      genres: { $addToSet: "$genres" }
    }
  },
  {
    $match: {
      numberOfMovies: { $gte: 2 }
    }
  },
  {
    $project: {
      director: "$_id",
      numberOfMovies: 1,
      avgRating: { $round: ["$avgRating", 2] },
      avgRuntime: { $round: ["$avgRuntime", 0] },
      totalOscars: 1,
      movies: 1,
      uniqueGenres: { $size: { $reduce: {
        input: "$genres",
        initialValue: [],
        in: { $setUnion: ["$$value", "$$this"] }
      }}},
      _id: 0
    }
  },
  {
    $sort: { numberOfMovies: -1, avgRating: -1 }
  }
])
```

**Descripción**:
- Descompone el array de directores
- Agrupa por director contando películas y calculando promedios
- Filtra directores con al menos 2 películas en el dataset
- Calcula el número de géneros únicos que ha trabajado cada director
- Muestra la lista de películas de cada director con sus ratings

**Utilidad**: Este análisis permite identificar directores consistentes en términos de calidad, versatilidad (géneros trabajados), y reconocimiento (Oscars ganados).

---

## Query Libre 3: Análisis de Evolución Temporal - Calidad vs Duración

**Objetivo**: Analizar la relación entre la duración de las películas y su calidad a lo largo de las décadas, identificando tendencias.

```javascript
db.movies.aggregate([
  {
    $project: {
      title: 1,
      year: 1,
      rating: 1,
      runtime: 1,
      decade: {
        $concat: [
          { $toString: { $multiply: [
            { $floor: { $divide: ["$year", 10] } },
            10
          ]}},
          "s"
        ]
      },
      runtimeCategory: {
        $switch: {
          branches: [
            { case: { $lt: ["$runtime", 90] }, then: "Corta (< 90 min)" },
            { case: { $lt: ["$runtime", 120] }, then: "Media (90-120 min)" },
            { case: { $lt: ["$runtime", 150] }, then: "Larga (120-150 min)" },
            { case: { $gte: ["$runtime", 150] }, then: "Épica (>= 150 min)" }
          ],
          default: "Desconocida"
        }
      }
    }
  },
  {
    $group: {
      _id: {
        decade: "$decade",
        category: "$runtimeCategory"
      },
      count: { $sum: 1 },
      avgRating: { $avg: "$rating" },
      avgRuntime: { $avg: "$runtime" },
      movies: { 
        $push: { 
          title: "$title", 
          year: "$year",
          runtime: "$runtime",
          rating: "$rating"
        }
      }
    }
  },
  {
    $sort: { "_id.decade": 1, avgRating: -1 }
  },
  {
    $project: {
      decade: "$_id.decade",
      runtimeCategory: "$_id.category",
      count: 1,
      avgRating: { $round: ["$avgRating", 2] },
      avgRuntime: { $round: ["$avgRuntime", 0] },
      movies: 1,
      _id: 0
    }
  }
])
```

**Descripción**:
- Clasifica las películas por década y categoría de duración
- Categorías: Corta (< 90 min), Media (90-120 min), Larga (120-150 min), Épica (>= 150 min)
- Calcula el rating promedio y la duración promedio por categoría y década
- Permite visualizar tendencias: ¿Las películas más largas tienen mejor rating? ¿Ha cambiado esto con el tiempo?

**Utilidad**: Este análisis permite identificar:
- Si existe correlación entre duración y calidad
- Cómo han evolucionado las preferencias de duración a lo largo del tiempo
- Qué categorías de duración tienden a tener mejor recepción por década

---

## Análisis Complementario: Resumen Ejecutivo

Para obtener un resumen general del dataset que puede ser útil como introducción:

```javascript
db.movies.aggregate([
  {
    $facet: {
      "generalStats": [
        {
          $group: {
            _id: null,
            totalMovies: { $sum: 1 },
            avgRating: { $avg: "$rating" },
            avgRuntime: { $avg: "$runtime" },
            totalOscars: { $sum: "$awards.oscars" },
            earliestYear: { $min: "$year" },
            latestYear: { $max: "$year" }
          }
        }
      ],
      "topRated": [
        { $sort: { rating: -1 } },
        { $limit: 3 },
        { $project: { title: 1, rating: 1, year: 1, _id: 0 } }
      ],
      "mostAwarded": [
        { $sort: { "awards.oscars": -1 } },
        { $limit: 3 },
        { $project: { title: 1, oscars: "$awards.oscars", year: 1, _id: 0 } }
      ],
      "countriesCount": [
        { $group: { _id: "$country", count: { $sum: 1 } } },
        { $sort: { count: -1 } }
      ]
    }
  }
])
```

**Descripción**: Utiliza `$facet` para ejecutar múltiples agregaciones en paralelo y obtener un resumen completo del dataset en una sola query.

---

## Notas Adicionales

Estas queries libres demuestran:
1. **Análisis de correlación**: Entre premios y calidad
2. **Análisis de productores/directores**: Identificación de talento consistente
3. **Análisis temporal y categórico**: Tendencias a lo largo del tiempo

Cada query puede ser modificada o extendida según las necesidades específicas del análisis.
