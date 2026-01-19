# Análisis de Dataset de Películas con MongoDB

Proyecto de análisis y manipulación de un dataset de películas utilizando MongoDB. Este repositorio contiene ejercicios básicos y avanzados sobre bases de datos NoSQL, incluyendo operaciones de inserción, actualización, filtros y agregaciones.

## 📋 Descripción del Proyecto

Este proyecto incluye:
- **23 ejercicios fijos** que cubren desde operaciones básicas hasta agregaciones avanzadas
- **3 queries libres** con análisis adicionales del dataset
- Dataset de películas en formato JSON con 20 películas clásicas
- Documentación completa con todas las consultas MongoDB

## 🗂️ Estructura del Proyecto

```
bases-de-datos-NO-SQL/
├── data/
│   └── movies.json              # Dataset de películas (20 películas)
├── queries/
│   ├── ejercicios_fijos.md      # 23 ejercicios fijos con soluciones
│   └── queries_libres.md        # 3 queries libres con análisis avanzados
├── docs/
│   └── (aquí se guardará el PDF final)
├── screenshots/
│   └── (capturas de pantalla de resultados)
└── README.md                    # Este archivo
```

## 🚀 Requisitos Previos

- **MongoDB** instalado (versión 4.0 o superior)
- **mongoimport** (incluido con MongoDB)
- Cliente MongoDB (mongo shell, MongoDB Compass, o similar)

## 📦 Instalación y Configuración

### 1. Clonar el repositorio

```bash
git clone https://github.com/diegoolalla/bases-de-datos-NO-SQL.git
cd bases-de-datos-NO-SQL
```

### 2. Iniciar MongoDB

Asegúrate de que MongoDB esté ejecutándose:

```bash
# En sistemas Unix/Linux/Mac
sudo systemctl start mongod

# O si usas el servicio directamente
mongod --dbpath /ruta/a/tu/directorio/de/datos
```

### 3. Importar el Dataset

Desde el directorio raíz del proyecto, ejecuta:

```bash
mongoimport --db movies_db --collection movies --file data/movies.json --jsonArray
```

**Resultado esperado:**
```
2024-XX-XX ... imported 20 documents
```

### 4. Conectar a MongoDB

```bash
mongosh movies_db
```

O si usas la versión antigua de mongo shell:
```bash
mongo movies_db
```

## 📚 Ejercicios Incluidos

### Ejercicios Básicos (1-8)
- Importación de datos
- Conteo de documentos
- Consultas básicas con filtros
- Proyecciones de campos específicos

### Manipulación de Arrays (9-12)
- Búsqueda en arrays
- Contar elementos de arrays
- Agregar elementos a arrays
- Consultas con tamaño de arrays

### Operaciones de Inserción y Actualización (13-17)
- Inserción de nuevos documentos
- Actualización de campos simples
- Operadores de incremento
- Actualizaciones múltiples
- Eliminación de campos

### Agregaciones Básicas (18-20)
- Cálculo de promedios
- Agrupación por campos
- Ordenamiento y límites

### Agregaciones Avanzadas (21-23)
- Análisis de géneros con $unwind
- Top actores más frecuentes
- Análisis por década con operaciones matemáticas

### Queries Libres (1-3)
- Análisis de películas ganadoras de Oscars
- Análisis de directores prolíficos
- Evolución temporal: calidad vs duración

## 🎯 Cómo Usar Este Proyecto

### Opción 1: Ejecutar Ejercicios Paso a Paso

1. Abre el archivo `queries/ejercicios_fijos.md`
2. Copia y ejecuta cada query en tu cliente MongoDB
3. Observa y analiza los resultados
4. Toma capturas de pantalla para tu documentación

### Opción 2: Ejecutar Queries Libres

1. Abre el archivo `queries/queries_libres.md`
2. Ejecuta las queries avanzadas para análisis adicionales
3. Interpreta los resultados obtenidos

## 📊 Dataset de Películas

El dataset incluye 20 películas clásicas con los siguientes campos:

```javascript
{
  "title": "Título de la película",
  "year": 1994,
  "genres": ["Drama", "Crime"],
  "directors": ["Director"],
  "actors": ["Actor 1", "Actor 2", "Actor 3"],
  "rating": 9.3,
  "runtime": 142,
  "plot": "Descripción de la trama",
  "country": "País",
  "awards": {
    "nominations": 7,
    "wins": 3,
    "oscars": 2
  }
}
```

### Películas Incluidas

El dataset contiene películas icónicas como:
- The Shawshank Redemption (1994)
- The Godfather (1972)
- The Dark Knight (2008)
- Pulp Fiction (1994)
- Forrest Gump (1994)
- Y 15 películas más...

## 📸 Capturas de Pantalla

Para completar el proyecto, se deben capturar screenshots de:

1. Proceso de importación del dataset
2. Resultados de cada uno de los 23 ejercicios fijos
3. Resultados de las 3 queries libres
4. Estadísticas finales de la colección

Guardar las capturas en el directorio `screenshots/`.

## 📄 Documentación Final

El proyecto requiere un **archivo PDF** que incluya:

1. **Portada**: Título del proyecto, autor, fecha
2. **Introducción**: Descripción del proyecto y objetivos
3. **Dataset**: Descripción del dataset utilizado
4. **Ejercicios Fijos**: Las 23 consultas con:
   - Código de cada query
   - Descripción de lo que hace
   - Captura de pantalla del resultado
5. **Queries Libres**: Las 3 consultas adicionales con:
   - Código completo
   - Explicación del análisis
   - Resultados y capturas
6. **Conclusiones**: Aprendizajes y observaciones

## 🔍 Conceptos de MongoDB Cubiertos

- **CRUD Operations**: Create, Read, Update, Delete
- **Operadores de consulta**: `$gt`, `$lt`, `$gte`, `$lte`, `$in`, `$nin`
- **Operadores de array**: `$push`, `$addToSet`, `$size`, `$unwind`
- **Operadores de actualización**: `$set`, `$unset`, `$inc`
- **Aggregation Framework**:
  - `$match`: Filtrado de documentos
  - `$group`: Agrupación y operaciones de acumulación
  - `$project`: Proyección y transformación de campos
  - `$sort`: Ordenamiento
  - `$limit`: Limitación de resultados
  - `$unwind`: Descomposición de arrays
  - `$facet`: Múltiples agregaciones en paralelo
- **Funciones de agregación**: `$sum`, `$avg`, `$min`, `$max`, `$push`
- **Operaciones matemáticas**: `$multiply`, `$divide`, `$floor`

## 🛠️ Comandos Útiles de MongoDB

```javascript
// Mostrar bases de datos
show dbs

// Usar una base de datos
use movies_db

// Mostrar colecciones
show collections

// Estadísticas de la colección
db.movies.stats()

// Eliminar la colección (si necesitas reiniciar)
db.movies.drop()

// Contar documentos
db.movies.countDocuments()

// Ver índices
db.movies.getIndexes()
```

## 🧪 Validación de Resultados

Después de completar todos los ejercicios, verifica:

1. ✅ Total de documentos: 21 (20 originales + 1 insertado en ejercicio 13)
2. ✅ Todas las queries ejecutan sin errores
3. ✅ Los resultados son consistentes con las descripciones
4. ✅ Las agregaciones producen resultados lógicos

## 📚 Recursos Adicionales

- [Documentación oficial de MongoDB](https://docs.mongodb.com/)
- [MongoDB Aggregation Pipeline](https://docs.mongodb.com/manual/core/aggregation-pipeline/)
- [MongoDB Query Operators](https://docs.mongodb.com/manual/reference/operator/query/)
- [MongoDB University (cursos gratuitos)](https://university.mongodb.com/)

## 👨‍💻 Autor

Diego Olalla

## 📝 Licencia

Este proyecto es de uso académico para el curso de Bases de Datos NO SQL.

---

## ⚡ Quick Start

```bash
# 1. Importar datos
mongoimport --db movies_db --collection movies --file data/movies.json --jsonArray

# 2. Conectar a MongoDB
mongosh movies_db

# 3. Verificar importación
db.movies.countDocuments()

# 4. Ejecutar primer ejercicio
db.movies.find().pretty()
```

¡Listo para comenzar! 🎬
