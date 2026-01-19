# Referencia Rápida de MongoDB

Guía de referencia rápida con los comandos y operadores más utilizados en MongoDB.

## 🗄️ Comandos Básicos de Base de Datos

```javascript
// Mostrar todas las bases de datos
show dbs

// Usar/crear una base de datos
use movies_db

// Mostrar la base de datos actual
db

// Eliminar la base de datos actual
db.dropDatabase()

// Mostrar colecciones
show collections

// Crear colección
db.createCollection("movies")

// Eliminar colección
db.movies.drop()

// Estadísticas de la base de datos
db.stats()

// Estadísticas de la colección
db.movies.stats()
```

---

## 📝 CRUD Operations

### Create (Insertar)

```javascript
// Insertar un documento
db.movies.insertOne({
  title: "Inception",
  year: 2010,
  rating: 8.8
})

// Insertar múltiples documentos
db.movies.insertMany([
  { title: "Movie 1", year: 2020 },
  { title: "Movie 2", year: 2021 }
])
```

### Read (Leer)

```javascript
// Encontrar todos los documentos
db.movies.find()

// Con formato legible
db.movies.find().pretty()

// Encontrar con filtro
db.movies.find({ year: 2010 })

// Encontrar uno
db.movies.findOne({ title: "Inception" })

// Proyección (solo ciertos campos)
db.movies.find({}, { title: 1, year: 1, _id: 0 })

// Limitar resultados
db.movies.find().limit(5)

// Saltar resultados
db.movies.find().skip(10)

// Ordenar
db.movies.find().sort({ year: -1 })  // -1 descendente, 1 ascendente

// Contar
db.movies.countDocuments()
db.movies.countDocuments({ year: 1994 })
```

### Update (Actualizar)

```javascript
// Actualizar un documento
db.movies.updateOne(
  { title: "Inception" },
  { $set: { rating: 9.0 } }
)

// Actualizar múltiples documentos
db.movies.updateMany(
  { year: 1994 },
  { $set: { decade: "90s" } }
)

// Reemplazar un documento completamente
db.movies.replaceOne(
  { title: "Old Title" },
  { title: "New Movie", year: 2024 }
)

// Upsert (insertar si no existe)
db.movies.updateOne(
  { title: "New Movie" },
  { $set: { year: 2024 } },
  { upsert: true }
)
```

### Delete (Eliminar)

```javascript
// Eliminar un documento
db.movies.deleteOne({ title: "Movie to delete" })

// Eliminar múltiples documentos
db.movies.deleteMany({ year: { $lt: 1950 } })

// Eliminar todos los documentos
db.movies.deleteMany({})
```

---

## 🔍 Operadores de Consulta

### Operadores de Comparación

```javascript
// $eq - igual
db.movies.find({ year: { $eq: 1994 } })
// Equivalente a:
db.movies.find({ year: 1994 })

// $ne - no igual
db.movies.find({ year: { $ne: 1994 } })

// $gt - mayor que
db.movies.find({ rating: { $gt: 8.5 } })

// $gte - mayor o igual que
db.movies.find({ rating: { $gte: 8.5 } })

// $lt - menor que
db.movies.find({ runtime: { $lt: 120 } })

// $lte - menor o igual que
db.movies.find({ runtime: { $lte: 120 } })

// $in - en un array de valores
db.movies.find({ year: { $in: [1994, 1999, 2010] } })

// $nin - no en un array de valores
db.movies.find({ year: { $nin: [1994, 1999, 2010] } })
```

### Operadores Lógicos

```javascript
// $and
db.movies.find({
  $and: [
    { year: 1994 },
    { rating: { $gt: 8.5 } }
  ]
})
// Equivalente a:
db.movies.find({ year: 1994, rating: { $gt: 8.5 } })

// $or
db.movies.find({
  $or: [
    { year: 1994 },
    { year: 1999 }
  ]
})

// $not
db.movies.find({
  rating: { $not: { $gt: 8.5 } }
})

// $nor - ni esto ni aquello
db.movies.find({
  $nor: [
    { year: 1994 },
    { rating: { $lt: 8.0 } }
  ]
})
```

### Operadores de Elemento

```javascript
// $exists - campo existe
db.movies.find({ awards: { $exists: true } })

// $type - tipo de dato
db.movies.find({ rating: { $type: "double" } })
```

### Operadores de Array

```javascript
// Buscar en array
db.movies.find({ genres: "Drama" })

// $all - contiene todos los elementos
db.movies.find({ genres: { $all: ["Drama", "Crime"] } })

// $size - tamaño del array
db.movies.find({ actors: { $size: 3 } })

// $elemMatch - al menos un elemento cumple la condición
db.movies.find({
  awards: {
    $elemMatch: { wins: { $gt: 5 }, oscars: { $gt: 3 } }
  }
})
```

### Operadores de Evaluación

```javascript
// $regex - expresión regular
db.movies.find({ title: { $regex: /^The/ } })
db.movies.find({ title: { $regex: "Godfather", $options: "i" } })

// $expr - usar expresiones
db.movies.find({
  $expr: { $gt: ["$rating", 8.5] }
})

// Comparar dos campos
db.movies.find({
  $expr: { $gt: ["$awards.wins", "$awards.nominations"] }
})
```

---

## 🔄 Operadores de Actualización

```javascript
// $set - establecer valor
db.movies.updateOne(
  { title: "Inception" },
  { $set: { rating: 9.0, updated: new Date() } }
)

// $unset - eliminar campo
db.movies.updateOne(
  { title: "Inception" },
  { $unset: { updated: "" } }
)

// $inc - incrementar
db.movies.updateOne(
  { title: "Inception" },
  { $inc: { "awards.nominations": 2 } }
)

// $mul - multiplicar
db.movies.updateOne(
  { title: "Inception" },
  { $mul: { rating: 1.1 } }
)

// $min - actualizar si el nuevo valor es menor
db.movies.updateOne(
  { title: "Inception" },
  { $min: { rating: 8.5 } }
)

// $max - actualizar si el nuevo valor es mayor
db.movies.updateOne(
  { title: "Inception" },
  { $max: { rating: 9.0 } }
)

// $currentDate - fecha actual
db.movies.updateOne(
  { title: "Inception" },
  { $currentDate: { lastModified: true } }
)

// $rename - renombrar campo
db.movies.updateMany(
  {},
  { $rename: { "rating": "imdbRating" } }
)
```

### Operadores de Array (Actualización)

```javascript
// $push - agregar elemento al array
db.movies.updateOne(
  { title: "Inception" },
  { $push: { genres: "Mystery" } }
)

// $push con $each (múltiples elementos)
db.movies.updateOne(
  { title: "Inception" },
  { $push: { actors: { $each: ["Actor 4", "Actor 5"] } } }
)

// $addToSet - agregar si no existe
db.movies.updateOne(
  { title: "Inception" },
  { $addToSet: { genres: "Thriller" } }
)

// $pop - eliminar primer (-1) o último (1) elemento
db.movies.updateOne(
  { title: "Inception" },
  { $pop: { actors: 1 } }
)

// $pull - eliminar elemento específico
db.movies.updateOne(
  { title: "Inception" },
  { $pull: { genres: "Mystery" } }
)

// $pullAll - eliminar múltiples elementos
db.movies.updateOne(
  { title: "Inception" },
  { $pullAll: { genres: ["Mystery", "Thriller"] } }
)

// $ - actualizar primer elemento que coincide
db.movies.updateOne(
  { title: "Inception", "actors": "Old Name" },
  { $set: { "actors.$": "New Name" } }
)
```

---

## 📊 Aggregation Framework

### Estructura Básica

```javascript
db.movies.aggregate([
  { $match: { /* filtro */ } },
  { $group: { /* agrupación */ } },
  { $sort: { /* ordenar */ } },
  { $project: { /* proyección */ } },
  { $limit: 10 }
])
```

### Etapas del Pipeline

```javascript
// $match - filtrar documentos
{ $match: { year: { $gte: 2000 } } }

// $project - proyectar/transformar campos
{
  $project: {
    title: 1,
    year: 1,
    decade: { $floor: { $divide: ["$year", 10] } }
  }
}

// $group - agrupar y agregar
{
  $group: {
    _id: "$year",
    count: { $sum: 1 },
    avgRating: { $avg: "$rating" },
    maxRating: { $max: "$rating" },
    minRating: { $min: "$rating" },
    movies: { $push: "$title" }
  }
}

// $sort - ordenar
{ $sort: { year: -1, rating: -1 } }

// $limit - limitar resultados
{ $limit: 10 }

// $skip - saltar resultados
{ $skip: 5 }

// $unwind - descomponer array
{ $unwind: "$genres" }

// $lookup - join con otra colección
{
  $lookup: {
    from: "directors",
    localField: "director_id",
    foreignField: "_id",
    as: "director_info"
  }
}

// $addFields - agregar campos
{
  $addFields: {
    totalAwards: { $add: ["$awards.wins", "$awards.nominations"] }
  }
}

// $count - contar documentos
{ $count: "totalMovies" }

// $facet - múltiples pipelines en paralelo
{
  $facet: {
    "byYear": [
      { $group: { _id: "$year", count: { $sum: 1 } } }
    ],
    "byGenre": [
      { $unwind: "$genres" },
      { $group: { _id: "$genres", count: { $sum: 1 } } }
    ]
  }
}
```

### Operadores de Acumulación

```javascript
// $sum - sumar
{ $sum: 1 }                    // Contar
{ $sum: "$field" }             // Sumar valores de un campo

// $avg - promedio
{ $avg: "$rating" }

// $min - mínimo
{ $min: "$year" }

// $max - máximo
{ $max: "$rating" }

// $push - crear array con valores
{ $push: "$title" }
{ $push: { title: "$title", year: "$year" } }

// $addToSet - array sin duplicados
{ $addToSet: "$genre" }

// $first - primer valor del grupo
{ $first: "$title" }

// $last - último valor del grupo
{ $last: "$title" }
```

### Operadores de Expresión

```javascript
// Aritméticos
{ $add: ["$field1", "$field2"] }
{ $subtract: ["$field1", "$field2"] }
{ $multiply: ["$field1", "$field2"] }
{ $divide: ["$field1", "$field2"] }
{ $mod: ["$field1", 10] }

// Comparación
{ $eq: ["$field1", "$field2"] }
{ $ne: ["$field1", "$field2"] }
{ $gt: ["$field1", "$field2"] }
{ $gte: ["$field1", "$field2"] }
{ $lt: ["$field1", "$field2"] }
{ $lte: ["$field1", "$field2"] }

// Lógicos
{ $and: [expr1, expr2] }
{ $or: [expr1, expr2] }
{ $not: expr }

// Condicionales
{
  $cond: {
    if: { $gte: ["$rating", 8.5] },
    then: "Excelente",
    else: "Buena"
  }
}

{
  $switch: {
    branches: [
      { case: { $gte: ["$rating", 9] }, then: "Excelente" },
      { case: { $gte: ["$rating", 8] }, then: "Muy Buena" },
      { case: { $gte: ["$rating", 7] }, then: "Buena" }
    ],
    default: "Regular"
  }
}

// String
{ $concat: ["$firstName", " ", "$lastName"] }
{ $substr: ["$title", 0, 10] }
{ $toLower: "$title" }
{ $toUpper: "$title" }

// Array
{ $size: "$actors" }
{ $arrayElemAt: ["$actors", 0] }
{ $slice: ["$actors", 2] }

// Conversión de tipos
{ $toString: "$year" }
{ $toInt: "$ratingString" }
{ $toDouble: "$ratingString" }

// Fecha
{ $year: "$releaseDate" }
{ $month: "$releaseDate" }
{ $dayOfMonth: "$releaseDate" }
```

---

## 💾 Importar/Exportar

```bash
# Importar JSON
mongoimport --db movies_db --collection movies --file movies.json --jsonArray

# Importar CSV
mongoimport --db movies_db --collection movies --type csv --headerline --file movies.csv

# Exportar JSON
mongoexport --db movies_db --collection movies --out movies.json --jsonArray

# Exportar CSV
mongoexport --db movies_db --collection movies --type csv --fields title,year,rating --out movies.csv

# Backup completo
mongodump --db movies_db --out /backup/path

# Restore
mongorestore --db movies_db /backup/path/movies_db
```

---

## 🔎 Índices

```javascript
// Crear índice
db.movies.createIndex({ title: 1 })

// Índice compuesto
db.movies.createIndex({ year: 1, rating: -1 })

// Índice único
db.movies.createIndex({ title: 1 }, { unique: true })

// Ver índices
db.movies.getIndexes()

// Eliminar índice
db.movies.dropIndex("title_1")

// Eliminar todos los índices (excepto _id)
db.movies.dropIndexes()

// Analizar query performance
db.movies.find({ year: 1994 }).explain("executionStats")
```

---

## 🎯 Ejemplos Prácticos

### Buscar películas de un actor específico con buen rating

```javascript
db.movies.find({
  actors: "Tom Hanks",
  rating: { $gte: 8.5 }
})
```

### Películas de múltiples géneros de la década de los 90

```javascript
db.movies.find({
  year: { $gte: 1990, $lt: 2000 },
  $expr: { $gt: [{ $size: "$genres" }, 2] }
})
```

### Top 5 películas mejor valoradas por década

```javascript
db.movies.aggregate([
  {
    $project: {
      title: 1,
      rating: 1,
      decade: { $multiply: [{ $floor: { $divide: ["$year", 10] } }, 10] }
    }
  },
  { $sort: { rating: -1 } },
  {
    $group: {
      _id: "$decade",
      topMovies: { $push: { title: "$title", rating: "$rating" } }
    }
  },
  {
    $project: {
      topMovies: { $slice: ["$topMovies", 5] }
    }
  },
  { $sort: { _id: 1 } }
])
```

### Contar películas por país y género

```javascript
db.movies.aggregate([
  { $unwind: "$genres" },
  {
    $group: {
      _id: { country: "$country", genre: "$genres" },
      count: { $sum: 1 },
      avgRating: { $avg: "$rating" }
    }
  },
  { $sort: { "_id.country": 1, count: -1 } }
])
```

---

## 🚀 Tips y Mejores Prácticas

1. **Usa proyecciones** para devolver solo los campos necesarios
2. **Crea índices** en campos que consultas frecuentemente
3. **Usa $match temprano** en el pipeline de agregación
4. **Limita resultados** cuando sea posible
5. **Usa explain()** para analizar performance
6. **Evita $where** (usa $expr en su lugar)
7. **Valida datos** antes de insertar
8. **Usa bulkWrite()** para operaciones masivas

---

Esta guía de referencia rápida debería cubrir la mayoría de operaciones comunes en MongoDB. ¡Guárdala para consultas rápidas! 📖✨
