# Guía para Capturas de Pantalla

Esta guía te ayudará a tomar las capturas de pantalla correctas para documentar todos los ejercicios del proyecto.

## 📸 Herramientas Recomendadas

### MongoDB Compass (Recomendado)
- Interfaz gráfica intuitiva
- Visualización clara de resultados
- Fácil de capturar y documentar

### MongoDB Shell (mongosh o mongo)
- Para capturas más técnicas
- Muestra el código y resultado juntos
- Ideal para queries complejas

## 🎯 Qué Capturar en Cada Ejercicio

### Ejercicio de Importación
**Capturar:**
- Terminal mostrando el comando de importación
- Mensaje de éxito con número de documentos importados

**Ejemplo de lo que debe verse:**
```
$ mongoimport --db movies_db --collection movies --file data/movies.json --jsonArray
2024-XX-XX ... connected to: mongodb://localhost/
2024-XX-XX ... 20 document(s) imported successfully. 0 document(s) failed to import.
```

---

### Ejercicios de Consulta Básica (find, count)

**Capturar:**
- El código de la query en la parte superior
- Los resultados mostrados claramente
- Si hay muchos resultados, capturar los primeros y últimos con "..." en medio

**Ejemplo:**
```javascript
db.movies.find({ year: 1994 })
```
Resultado: [4 películas mostradas con sus campos]

---

### Ejercicios de Actualización

**Capturar DOS pantallas:**

1. **Antes de la actualización:**
```javascript
db.movies.findOne({ title: "The Dark Knight" })
```
Mostrar el documento con el valor original

2. **Después de la actualización:**
```javascript
db.movies.updateOne(
  { title: "The Dark Knight" },
  { $set: { rating: 9.1 } }
)
// Resultado: { "acknowledged": true, "matchedCount": 1, "modifiedCount": 1 }

db.movies.findOne({ title: "The Dark Knight" })
```
Mostrar el documento con el valor actualizado

---

### Ejercicios de Agregación

**Capturar:**
- El código completo del pipeline
- Los resultados en formato legible
- Si es posible, usar pretty() o formato JSON

**Para agregaciones complejas:**
- Mostrar la query completa
- Mostrar todos los resultados (si son pocos)
- Si hay muchos, mostrar los más relevantes

---

## 📋 Lista de Capturas Necesarias

### Configuración Inicial
- [ ] `00_importacion.png` - Importación del dataset
- [ ] `01_verificacion_import.png` - Verificación con countDocuments()

### Ejercicios Básicos (2-8)
- [ ] `02_count_total.png` - Conteo de documentos
- [ ] `03_find_all.png` - Todas las películas (primeras y últimas)
- [ ] `04_year_1994.png` - Películas de 1994
- [ ] `05_rating_mayor_85.png` - Películas con rating > 8.5
- [ ] `06_genero_drama.png` - Películas del género Drama
- [ ] `07_runtime_menor_120.png` - Películas < 120 minutos
- [ ] `08_proyeccion_titulo_year.png` - Solo título y año

### Manipulación de Arrays (9-12)
- [ ] `09_tom_hanks.png` - Películas con Tom Hanks
- [ ] `10_multiples_generos.png` - Películas con >2 géneros
- [ ] `11_count_actores.png` - Número de actores por película
- [ ] `12_add_actor_antes.png` - The Matrix antes de agregar actor
- [ ] `12_add_actor_despues.png` - The Matrix después de agregar actor

### Inserción y Actualización (13-17)
- [ ] `13_insert_nueva_pelicula.png` - Inserción exitosa
- [ ] `13_verificacion_insert.png` - Verificar película insertada
- [ ] `14_update_rating_antes.png` - Rating antes de actualizar
- [ ] `14_update_rating_despues.png` - Rating después de actualizar
- [ ] `15_increment_nominations.png` - Incremento de nominaciones
- [ ] `16_update_many_nolan.png` - Actualización múltiple
- [ ] `17_unset_field.png` - Eliminación de campo

### Agregaciones Básicas (18-20)
- [ ] `18_avg_rating.png` - Rating promedio
- [ ] `19_count_por_year.png` - Películas por año
- [ ] `20_pelicula_mas_larga.png` - Película más larga

### Agregaciones Avanzadas (21-23)
- [ ] `21_analisis_generos.png` - Análisis completo de géneros
- [ ] `22_top_actores.png` - Top 5 actores
- [ ] `23_analisis_decada.png` - Análisis por década

### Queries Libres (1-3)
- [ ] `libre_01_oscars.png` - Análisis de películas con Oscars
- [ ] `libre_02_directores.png` - Directores prolíficos
- [ ] `libre_03_evolucion.png` - Evolución temporal

### Verificación Final
- [ ] `final_count.png` - Conteo final (21 películas)
- [ ] `final_stats.png` - Estadísticas de la colección

---

## 🎨 Consejos para Mejores Capturas

### 1. Configuración del Entorno

**MongoDB Shell:**
```javascript
// Usar pretty() para mejor formato
db.movies.find().pretty()

// Limitar resultados si son muchos
db.movies.find().limit(3)
```

**MongoDB Compass:**
- Usar vista "JSON" para documentos individuales
- Usar vista "Table" para múltiples resultados
- Ajustar el zoom para mejor legibilidad

### 2. Calidad de Imagen

✅ **Hacer:**
- Usar resolución alta (1920x1080 o superior)
- Asegurar que el texto sea legible
- Usar fondo oscuro para mejor contraste (opcional)
- Capturar solo el área relevante
- Guardar en formato PNG (mejor calidad)

❌ **Evitar:**
- Capturas borrosas o pixeladas
- Incluir información irrelevante (otras ventanas, barra de tareas)
- Texto demasiado pequeño para leer
- Formato JPG con compresión alta

### 3. Organización

**Nombrar archivos claramente:**
```
screenshots/
├── 01_config/
│   ├── 00_importacion.png
│   └── 01_verificacion.png
├── 02_basicos/
│   ├── 02_count.png
│   ├── 03_find_all.png
│   └── ...
├── 03_arrays/
│   ├── 09_tom_hanks.png
│   └── ...
├── 04_crud/
│   ├── 13_insert.png
│   └── ...
├── 05_agregaciones/
│   ├── 18_avg_rating.png
│   └── ...
└── 06_libres/
    ├── libre_01_oscars.png
    └── ...
```

### 4. Contenido de las Capturas

**Elementos a incluir:**
1. **El comando/query** ejecutado
2. **El resultado** completo o representativo
3. **Timestamp/fecha** (si es relevante)
4. **Nombre de la BD/colección** (visible en el prompt)

**Ejemplo de buena captura:**
```
movies_db> db.movies.countDocuments()
20

movies_db> 
```

---

## 🖥️ Pasos para Capturar en Diferentes Sistemas

### Windows
1. **Snipping Tool** (recomendado)
   - Buscar "Recortes" en el menú inicio
   - Seleccionar área rectangular
   - Guardar como PNG

2. **Windows + Shift + S**
   - Atajo de teclado rápido
   - Copiar al portapapeles
   - Pegar en Paint y guardar

### macOS
1. **Command + Shift + 4**
   - Seleccionar área a capturar
   - Se guarda automáticamente en Escritorio

2. **Command + Shift + 3**
   - Captura toda la pantalla

### Linux
1. **Flameshot** (recomendado)
   ```bash
   sudo apt update && sudo apt install flameshot
   flameshot gui
   ```

2. **Gnome Screenshot**
   ```bash
   gnome-screenshot -a
   ```

---

## ✅ Checklist de Calidad

Antes de usar cada captura, verificar:

- [ ] El texto es completamente legible
- [ ] Se ve el código/query completo
- [ ] Se ven los resultados relevantes
- [ ] No hay información sensible visible
- [ ] El formato es PNG
- [ ] El nombre del archivo es descriptivo
- [ ] La resolución es adecuada (>1200px ancho)
- [ ] No hay elementos distractores

---

## 📝 Documentar las Capturas

Para cada captura en el PDF, incluir:

```markdown
**Figura X: [Título descriptivo]**

[IMAGEN]

*Descripción*: [Breve explicación de lo que muestra la captura]
*Resultado*: [Interpretación del resultado]
```

**Ejemplo:**

```markdown
**Figura 4: Películas del año 1994**

[IMAGEN mostrando 4 películas]

*Descripción*: Query que filtra películas lanzadas en 1994 usando el 
operador de igualdad en el campo year.

*Resultado*: Se encontraron 4 películas: The Shawshank Redemption, 
Pulp Fiction, Forrest Gump y The Lion King.
```

---

## 🎯 Ejemplo de Secuencia Completa

### Para el Ejercicio 14 (Actualizar rating):

**1. Captura del estado inicial:**
```javascript
db.movies.findOne({ title: "The Dark Knight" }, { title: 1, rating: 1 })
```
📸 `14_rating_antes.png`

**2. Captura de la actualización:**
```javascript
db.movies.updateOne(
  { title: "The Dark Knight" },
  { $set: { rating: 9.1 } }
)
```
📸 `14_update_command.png`

**3. Captura del estado final:**
```javascript
db.movies.findOne({ title: "The Dark Knight" }, { title: 1, rating: 1 })
```
📸 `14_rating_despues.png`

**En el PDF mostrarías las 3 capturas en secuencia** para demostrar el cambio completo.

---

## 🚀 Flujo de Trabajo Recomendado

1. **Preparar el entorno:**
   - MongoDB en ejecución
   - Cliente MongoDB abierto
   - Carpeta de screenshots creada
   - Herramienta de captura lista

2. **Ejecutar y capturar:**
   - Abrir el archivo de ejercicios
   - Copiar el primer ejercicio
   - Ejecutar en MongoDB
   - Capturar el resultado
   - Guardar con nombre descriptivo
   - Repetir para cada ejercicio

3. **Verificar:**
   - Revisar todas las capturas
   - Confirmar que son legibles
   - Verificar que no falta ninguna
   - Renombrar si es necesario

4. **Organizar:**
   - Ordenar por número de ejercicio
   - Agrupar por categoría
   - Crear subcarpetas si es necesario

---

¡Con esta guía deberías poder documentar perfectamente todos los ejercicios! 📸✨
