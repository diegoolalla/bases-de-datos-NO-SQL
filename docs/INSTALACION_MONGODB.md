# Guía de Instalación de MongoDB

Esta guía te ayudará a instalar y configurar MongoDB en diferentes sistemas operativos.

## 📦 Instalación por Sistema Operativo

### Windows

#### Opción 1: Instalador MSI (Recomendado)

1. **Descargar MongoDB Community Server**
   - Visita: https://www.mongodb.com/try/download/community
   - Selecciona: Windows, MSI Package
   - Descarga la última versión estable

2. **Ejecutar el instalador**
   - Doble clic en el archivo `.msi`
   - Selecciona "Complete" installation
   - Marca "Install MongoDB as a Service"
   - Marca "Install MongoDB Compass" (GUI opcional pero recomendado)

3. **Verificar instalación**
   ```cmd
   mongod --version
   mongosh --version
   ```

4. **Agregar MongoDB al PATH (si es necesario)**
   - Buscar "Variables de entorno" en Windows
   - Agregar: `C:\Program Files\MongoDB\Server\7.0\bin`

#### Opción 2: Usando Chocolatey

```powershell
choco install mongodb
choco install mongodb-compass
```

---

### macOS

#### Opción 1: Usando Homebrew (Recomendado)

1. **Instalar Homebrew** (si no lo tienes):
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

2. **Agregar el tap de MongoDB**:
   ```bash
   brew tap mongodb/brew
   ```

3. **Instalar MongoDB**:
   ```bash
   brew install mongodb-community@7.0
   ```

4. **Iniciar MongoDB**:
   ```bash
   brew services start mongodb-community@7.0
   ```

5. **Verificar instalación**:
   ```bash
   mongosh --version
   ```

#### Opción 2: Descarga Manual

1. Descargar desde: https://www.mongodb.com/try/download/community
2. Seleccionar: macOS, TGZ package
3. Extraer y mover a `/usr/local/mongodb`
4. Agregar al PATH en `~/.zshrc` o `~/.bash_profile`:
   ```bash
   export PATH=/usr/local/mongodb/bin:$PATH
   ```

---

### Linux (Ubuntu/Debian)

#### Ubuntu 22.04 / Debian 11+

1. **Importar la clave pública GPG**:
   ```bash
   curl -fsSL https://pgp.mongodb.com/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg --dearmor
   ```

2. **Crear el archivo de lista de fuentes**:
   
   Para Ubuntu 22.04:
   ```bash
   echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | \
   sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
   ```

3. **Actualizar el índice de paquetes**:
   ```bash
   sudo apt-get update
   ```

4. **Instalar MongoDB**:
   ```bash
   sudo apt-get install -y mongodb-org
   ```

5. **Iniciar MongoDB**:
   ```bash
   sudo systemctl start mongod
   sudo systemctl enable mongod  # Para inicio automático
   ```

6. **Verificar el estado**:
   ```bash
   sudo systemctl status mongod
   ```

#### Fedora/RHEL/CentOS

1. **Crear archivo repo**:
   ```bash
   sudo vim /etc/yum.repos.d/mongodb-org-7.0.repo
   ```

   Contenido:
   ```ini
   [mongodb-org-7.0]
   name=MongoDB Repository
   baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/7.0/x86_64/
   gpgcheck=1
   enabled=1
   gpgkey=https://pgp.mongodb.com/server-7.0.asc
   ```

2. **Instalar**:
   ```bash
   sudo yum install -y mongodb-org
   ```

3. **Iniciar**:
   ```bash
   sudo systemctl start mongod
   sudo systemctl enable mongod
   ```

---

## 🐳 Docker (Todas las plataformas)

### Opción más rápida para desarrollo

1. **Instalar Docker**:
   - Windows/Mac: https://www.docker.com/products/docker-desktop
   - Linux: `sudo apt-get install docker.io`

2. **Ejecutar MongoDB en Docker**:
   ```bash
   docker run -d \
     --name mongodb \
     -p 27017:27017 \
     -v mongodb_data:/data/db \
     mongo:7.0
   ```

3. **Conectar al contenedor**:
   ```bash
   docker exec -it mongodb mongosh
   ```

4. **Detener/Iniciar**:
   ```bash
   docker stop mongodb
   docker start mongodb
   ```

---

## 🔧 Configuración Post-Instalación

### 1. Verificar que MongoDB está corriendo

```bash
# Ver el proceso
ps aux | grep mongod

# Verificar el puerto
netstat -an | grep 27017
# o en Mac/Linux:
lsof -i :27017
```

### 2. Conectar con mongosh

```bash
mongosh
```

Deberías ver:
```
Current Mongosh Log ID: ...
Connecting to: mongodb://127.0.0.1:27017/?directConnection=true
Using MongoDB: 7.0.x
Using Mongosh: 2.0.x
```

### 3. Crear directorio de datos (si es necesario)

```bash
# Windows
mkdir C:\data\db

# Mac/Linux
sudo mkdir -p /data/db
sudo chown -R $USER /data/db
```

### 4. Configurar archivo de configuración

**Linux/Mac:** `/etc/mongod.conf`
**Windows:** `C:\Program Files\MongoDB\Server\7.0\bin\mongod.cfg`

```yaml
# mongod.conf
storage:
  dbPath: /data/db
  journal:
    enabled: true

systemLog:
  destination: file
  path: /var/log/mongodb/mongod.log
  logAppend: true

net:
  port: 27017
  bindIp: 127.0.0.1
```

---

## 🧪 Probar la Instalación

### 1. Conectar a MongoDB

```bash
mongosh
```

### 2. Crear una base de datos de prueba

```javascript
use test_db
```

### 3. Insertar un documento

```javascript
db.test_collection.insertOne({ name: "Test", value: 123 })
```

### 4. Consultar el documento

```javascript
db.test_collection.find()
```

### 5. Eliminar la base de datos de prueba

```javascript
db.dropDatabase()
```

Si todo funciona correctamente, ¡estás listo! ✅

---

## 📱 MongoDB Compass (GUI)

### Instalación

1. **Descargar**:
   - https://www.mongodb.com/try/download/compass

2. **Instalar**:
   - Windows: Ejecutar el instalador `.exe`
   - Mac: Arrastrar a Applications
   - Linux: `sudo dpkg -i mongodb-compass_*_amd64.deb`

3. **Conectar**:
   - URI de conexión: `mongodb://localhost:27017`
   - Click en "Connect"

### Ventajas de Compass

- ✅ Visualización gráfica de datos
- ✅ Construcción visual de queries
- ✅ Análisis de esquema
- ✅ Índices y rendimiento
- ✅ Perfecto para principiantes

---

## 🔐 Seguridad Básica (Opcional para desarrollo local)

### Crear usuario administrador

```javascript
use admin

db.createUser({
  user: "admin",
  pwd: "tu_password_seguro",
  roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
})
```

### Habilitar autenticación

En `mongod.conf`:
```yaml
security:
  authorization: enabled
```

Reiniciar MongoDB:
```bash
sudo systemctl restart mongod
```

Conectar con autenticación:
```bash
mongosh -u admin -p tu_password_seguro --authenticationDatabase admin
```

---

## 🛠️ Comandos Útiles

### Control del servicio

```bash
# Linux
sudo systemctl start mongod
sudo systemctl stop mongod
sudo systemctl restart mongod
sudo systemctl status mongod

# macOS (Homebrew)
brew services start mongodb-community
brew services stop mongodb-community
brew services restart mongodb-community

# Windows (PowerShell como Admin)
net start MongoDB
net stop MongoDB
```

### Logs

```bash
# Linux/Mac
tail -f /var/log/mongodb/mongod.log

# Windows
Get-Content "C:\Program Files\MongoDB\Server\7.0\log\mongod.log" -Wait
```

### Backup

```bash
# Exportar colección
mongoexport --db movies_db --collection movies --out movies_backup.json

# Exportar base de datos completa
mongodump --db movies_db --out /backup/dir
```

### Restore

```bash
# Importar colección
mongoimport --db movies_db --collection movies --file movies_backup.json

# Restaurar base de datos
mongorestore --db movies_db /backup/dir/movies_db
```

---

## ❓ Solución de Problemas Comunes

### MongoDB no inicia

**Linux:**
```bash
# Verificar logs
sudo tail -50 /var/log/mongodb/mongod.log

# Verificar permisos del directorio de datos
sudo chown -R mongodb:mongodb /var/lib/mongodb
sudo chown mongodb:mongodb /tmp/mongodb-27017.sock

# Reintentar
sudo systemctl restart mongod
```

**Windows:**
```powershell
# Verificar el servicio
Get-Service MongoDB

# Iniciar manualmente
mongod --dbpath "C:\data\db"
```

### Puerto 27017 en uso

```bash
# Encontrar el proceso
sudo lsof -i :27017
# o
sudo netstat -nlp | grep 27017

# Matar el proceso
sudo kill -9 <PID>
```

### Error de conexión "Connection refused"

1. Verificar que MongoDB está corriendo:
   ```bash
   ps aux | grep mongod
   ```

2. Verificar el bind IP en la configuración:
   ```yaml
   net:
     bindIp: 127.0.0.1,::1
   ```

3. Verificar el firewall (si aplica)

### Falta de espacio en disco

```bash
# Verificar espacio
df -h

# Limpiar logs viejos
sudo rm /var/log/mongodb/mongod.log.*

# Compactar base de datos
mongosh
use movies_db
db.runCommand({ compact: 'movies' })
```

---

## 📚 Recursos Adicionales

- **Documentación oficial**: https://docs.mongodb.com/manual/
- **MongoDB University**: https://university.mongodb.com/ (cursos gratuitos)
- **Tutoriales**: https://www.mongodb.com/docs/manual/tutorial/
- **Foro de la comunidad**: https://www.mongodb.com/community/forums/
- **Stack Overflow**: https://stackoverflow.com/questions/tagged/mongodb

---

## ✅ Checklist de Instalación Completa

- [ ] MongoDB instalado y versión verificada
- [ ] Servicio MongoDB iniciado
- [ ] Conexión con mongosh exitosa
- [ ] Directorio de datos creado y con permisos correctos
- [ ] MongoDB Compass instalado (opcional)
- [ ] Prueba de inserción/consulta exitosa
- [ ] Comandos básicos funcionando
- [ ] mongoimport disponible y funcionando

Una vez completado este checklist, estás listo para comenzar con los ejercicios del proyecto. 🚀
