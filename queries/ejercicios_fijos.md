# Ejercicios Fijos - Análisis de Dataset de Películas con MongoDB

Este documento contiene los 23 ejercicios fijos para el análisis del dataset de películas utilizando MongoDB.

## Configuración Inicial

### 0. Crear la base de datos y seleccionarla
```javascript
use movies_db
```

### 1. Importar el dataset de películas
```bash
mongoimport --db movies_db --collection movies --file data/movies.json --jsonArray
```

**Descripción**: Este comando importa el archivo JSON con el dataset de películas a la colección `movies` en la base de datos `movies_db`.

---

## Ejercicios Básicos (1-8)

### 2. Contar el número total de documentos en la colección
```javascript
db.movies.countDocuments()
```

**Descripción**: Cuenta todos los documentos (películas) en la colección.

**Resultado esperado**: 20

---

### 3. Mostrar todas las películas
```javascript
db.movies.find().pretty()
```

**Descripción**: Recupera y muestra todos los documentos de la colección de forma legible.

---

### 4. Encontrar películas de un año específico (1994)
```javascript
db.movies.find({ year: 1994 })
```

**Descripción**: Filtra películas lanzadas en 1994.

**Resultado esperado**: The Shawshank Redemption, Pulp Fiction, Forrest Gump, The Lion King

---

### 5. Encontrar películas con rating mayor a 8.5
```javascript
db.movies.find({ rating: { $gt: 8.5 } })
```

**Descripción**: Filtra películas con una calificación superior a 8.5.

---

### 6. Encontrar películas del género "Drama"
```javascript
db.movies.find({ genres: "Drama" })
```

**Descripción**: Busca películas que incluyan "Drama" en su array de géneros.

---

### 7. Encontrar películas con duración menor a 120 minutos
```javascript
db.movies.find({ runtime: { $lt: 120 } })
```

**Descripción**: Filtra películas con una duración inferior a 120 minutos.

---

### 8. Proyección: Mostrar solo título y año de todas las películas
```javascript
db.movies.find({}, { title: 1, year: 1, _id: 0 })
```

**Descripción**: Muestra únicamente los campos `title` y `year`, excluyendo el `_id`.

---

## Manipulación de Arrays (9-12)

### 9. Encontrar películas donde actúa "Tom Hanks"
```javascript
db.movies.find({ actors: "Tom Hanks" })
```

**Descripción**: Busca películas que tengan a "Tom Hanks" en el array de actores.

**Resultado esperado**: Forrest Gump, Saving Private Ryan, The Green Mile

---

### 10. Encontrar películas con múltiples géneros (más de 2)
```javascript
db.movies.find({ 
  $expr: { $gt: [{ $size: "$genres" }, 2] }
})
```

**Descripción**: Filtra películas que tengan más de 2 géneros en su array.

---

### 11. Contar cuántos actores tiene cada película
```javascript
db.movies.find({}, { 
  title: 1, 
  numberOfActors: { $size: "$actors" },
  _id: 0 
})
```

**Descripción**: Muestra el título y el número de actores por película.

---

### 12. Agregar un actor a una película específica
```javascript
db.movies.updateOne(
  { title: "The Matrix" },
  { $push: { actors: "Hugo Weaving" } }
)
```

**Descripción**: Añade "Hugo Weaving" al array de actores de "The Matrix". Este ejercicio demuestra el uso del operador `$push` para agregar elementos a un array. (Nota: Hugo Weaving interpretó al Agente Smith en la película original, este es un ejemplo con fines demostrativos).

---

## Operaciones de Inserción y Actualización (13-17)

### 13. Insertar una nueva película
```javascript
db.movies.insertOne({
  title: "The Godfather Part II",
  year: 1974,
  genres: ["Crime", "Drama"],
  directors: ["Francis Ford Coppola"],
  actors: ["Al Pacino", "Robert De Niro", "Robert Duvall"],
  rating: 9.0,
  runtime: 202,
  plot: "The early life and career of Vito Corleone in 1920s New York City is portrayed, while his son, Michael, expands and tightens his grip on the family crime syndicate.",
  country: "USA",
  awards: {
    nominations: 11,
    wins: 6,
    oscars: 6
  }
})
```

**Descripción**: Inserta un nuevo documento de película en la colección.

---

### 14. Actualizar el rating de una película
```javascript
db.movies.updateOne(
  { title: "The Dark Knight" },
  { $set: { rating: 9.1 } }
)
```

**Descripción**: Actualiza el rating de "The Dark Knight" a 9.1.

---

### 15. Incrementar las nominaciones de una película
```javascript
db.movies.updateOne(
  { title: "Inception" },
  { $inc: { "awards.nominations": 2 } }
)
```

**Descripción**: Incrementa en 2 el número de nominaciones de "Inception".

---

### 16. Actualizar múltiples películas del mismo director
```javascript
db.movies.updateMany(
  { directors: "Christopher Nolan" },
  { $set: { featured_director: true } }
)
```

**Descripción**: Añade un campo `featured_director` a todas las películas dirigidas por Christopher Nolan.

---

### 17. Eliminar un campo de todas las películas
```javascript
db.movies.updateMany(
  {},
  { $unset: { featured_director: "" } }
)
```

**Descripción**: Elimina el campo `featured_director` de todas las películas.

---

## Agregaciones Básicas (18-20)

### 18. Calcular el rating promedio de todas las películas
```javascript
db.movies.aggregate([
  {
    $group: {
      _id: null,
      averageRating: { $avg: "$rating" }
    }
  }
])
```

**Descripción**: Calcula el promedio de ratings de todas las películas.

---

### 19. Contar películas por año
```javascript
db.movies.aggregate([
  {
    $group: {
      _id: "$year",
      count: { $sum: 1 }
    }
  },
  {
    $sort: { _id: 1 }
  }
])
```

**Descripción**: Agrupa las películas por año y cuenta cuántas hay en cada año, ordenadas cronológicamente.

---

### 20. Encontrar la película más larga
```javascript
db.movies.aggregate([
  {
    $sort: { runtime: -1 }
  },
  {
    $limit: 1
  },
  {
    $project: {
      title: 1,
      runtime: 1,
      _id: 0
    }
  }
])
```

**Descripción**: Encuentra la película con mayor duración (runtime).

---

## Agregaciones Avanzadas (21-23)

### 21. Análisis de géneros: Contar películas por género
```javascript
db.movies.aggregate([
  {
    $unwind: "$genres"
  },
  {
    $group: {
      _id: "$genres",
      count: { $sum: 1 },
      avgRating: { $avg: "$rating" }
    }
  },
  {
    $sort: { count: -1 }
  }
])
```

**Descripción**: Descompone el array de géneros y cuenta cuántas películas hay por género, incluyendo el rating promedio por género.

---

### 22. Análisis de actores: Top 5 actores más frecuentes
```javascript
db.movies.aggregate([
  {
    $unwind: "$actors"
  },
  {
    $group: {
      _id: "$actors",
      movieCount: { $sum: 1 },
      movies: { $push: "$title" }
    }
  },
  {
    $sort: { movieCount: -1 }
  },
  {
    $limit: 5
  }
])
```

**Descripción**: Descompone el array de actores y encuentra los 5 actores que aparecen en más películas, con la lista de sus películas.

---

### 23. Análisis por década: Películas y rating promedio
```javascript
db.movies.aggregate([
  {
    $project: {
      title: 1,
      year: 1,
      rating: 1,
      decade: {
        $multiply: [
          { $floor: { $divide: ["$year", 10] } },
          10
        ]
      }
    }
  },
  {
    $group: {
      _id: "$decade",
      count: { $sum: 1 },
      avgRating: { $avg: "$rating" },
      movies: { $push: { title: "$title", year: "$year" } }
    }
  },
  {
    $sort: { _id: 1 }
  }
])
```

**Descripción**: Agrupa las películas por década, calcula el número de películas y el rating promedio por década.

---

## Verificación Final

### Verificar el número total de películas (después de inserción)
```javascript
db.movies.countDocuments()
```

**Resultado esperado**: 21 (si se insertó la nueva película en el ejercicio 13)

---

### Ver estadísticas de la colección
```javascript
db.movies.stats()
```

**Descripción**: Muestra estadísticas detalladas de la colección incluyendo tamaño, número de documentos, índices, etc.
