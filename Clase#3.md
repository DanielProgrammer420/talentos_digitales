**Resumen Completo y Estructurado de la Clase: "Introducción a Node.js"**  
*(Cuarta clase del curso de Node.js y React.js)*

---

### **Tema Principal del Video**
La clase introduce **Node.js** como entorno de ejecución de JavaScript del lado del servidor, explicando sus características fundamentales, su arquitectura asíncrona y no bloqueante, y cómo organizar el código mediante **módulos**. Además, se cubre la **inicialización de un proyecto**, la instalación y uso de **dependencias de terceros** (como `nodemon` y `axios`), y buenas prácticas para la gestión de archivos sensibles y el control de versiones.

---

### **Puntos Clave Explicados (Con Detalle)**

#### **1. ¿Qué es Node.js?**
- **Definición**: Entorno de ejecución que permite ejecutar **JavaScript fuera del navegador**, en el servidor.
- **Creado por**: Ryan Dahl en 2009.
- **Motor**: Utiliza **V8**, el motor de JavaScript de Google Chrome.
- **Características clave**:
  - **Asíncrono y no bloqueante**: Maneja múltiples operaciones simultáneamente sin bloquear el hilo principal.
  - **Basado en eventos**: Usa un bucle de eventos para gestionar operaciones de E/S.
  - **Multiplataforma**: Funciona en Windows, macOS y Linux.
  - **Ideal para aplicaciones en tiempo real**: APIs, servidores web, chats, juegos en línea.

#### **2. Arquitectura de Node.js**
- **Mono-hilo**: JavaScript se ejecuta en un solo hilo principal.
- **Operaciones asíncronas**: Las tareas de E/S (como leer archivos o hacer peticiones HTTP) se delegan al sistema operativo y se resuelven mediante callbacks, promesas o `async/await`.
- **Escalabilidad**: A pesar de ser mono-hilo, su modelo no bloqueante permite manejar miles de conexiones simultáneas eficientemente.

#### **3. Módulos en Node.js**
Los módulos permiten organizar el código en archivos reutilizables y autocontenidos. Hay tres tipos:

| Tipo | Descripción | Ejemplos |
|------|-------------|----------|
| **Módulos core (nativos)** | Integrados en Node.js. No requieren instalación. | `http`, `fs` (file system), `path` |
| **Módulos de terceros** | Paquetes instalados desde **npm** (Node Package Manager). | `express`, `nodemon`, `axios` |
| **Módulos locales** | Archivos creados por el desarrollador. | `operaciones.js`, `utils.js` |

- **Sistema de módulos CommonJS** (usado en esta clase):
  - **Exportar**: `module.exports = { funcion1, funcion2 }`
  - **Importar**: `const modulo = require('./ruta/al/modulo')`

> **Nota**: También existe el sistema **ES Modules** (`import`/`export`), pero requiere configuración adicional (como `"type": "module"` en `package.json`).

#### **4. Inicialización de un Proyecto con `npm init`**
- **Comando**: `npm init`
- **Propósito**: Crea el archivo `package.json`, que contiene metadatos del proyecto y sus dependencias.
- **Pasos interactivos**:
  1. Nombre del proyecto.
  2. Versión inicial (`1.0.0`).
  3. Descripción.
  4. Punto de entrada (ej: `index.js`).
  5. Comandos de prueba (`test`).
  6. Repositorio, palabras clave, autor, licencia.
- **Versión rápida**: `npm init -y` (usa valores por defecto).

#### **5. Scripts en `package.json`**
Permiten definir comandos personalizados para el proyecto.

- **Ejemplo**:
  ```json
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js"
  }
  ```
- **Ejecución**:
  - `npm start` → Ejecuta el script `start`.
  - `npm run dev` → Ejecuta cualquier otro script (requiere `run`).

#### **6. Instalación y Uso de Dependencias**
- **Dependencias de producción**: `npm install nombre-paquete`
- **Dependencias de desarrollo**: `npm install nombre-paquete --save-dev`
  - Solo se usan durante el desarrollo (ej: `nodemon`).

**Ejemplos prácticos**:

1. **`nodemon`**:
   - Reinicia automáticamente el servidor al detectar cambios.
   - Instalación: `npm install nodemon --save-dev`
   - Uso en script: `"dev": "nodemon index.js"`

2. **`axios`**:
   - Biblioteca para realizar peticiones HTTP (alternativa a `fetch`).
   - Instalación: `npm install axios`
   - Uso:
     ```javascript
     const axios = require('axios');
     axios.get('https://jsonplaceholder.typicode.com/users/1')
       .then(response => console.log(response.data))
       .catch(error => console.error(error));
     ```

#### **7. Módulos Nativos: Ejemplos Prácticos**
- **`http`**: Para crear servidores web.
  ```javascript
  const http = require('http');
  const server = http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify({ message: 'Hello World' }));
  });
  server.listen(8000, () => console.log('Servidor en puerto 8000'));
  ```

- **`fs` (File System)**: Para gestionar archivos.
  ```javascript
  const fs = require('fs');
  // Escribir un archivo
  fs.writeFile('texto.txt', 'Contenido para mi archivo', (err) => {
    if (err) throw err;
    console.log('Archivo creado');
  });
  // Leer un archivo
  fs.readFile('texto.txt', 'utf8', (err, data) => {
    if (err) throw err;
    console.log('Contenido:', data);
  });
  // Eliminar un archivo
  fs.unlink('texto.txt', (err) => {
    if (err) throw err;
    console.log('Archivo eliminado');
  });
  ```

#### **8. Gestión de Archivos Sensibles y Control de Versiones**
- **`.gitignore`**: Archivo que especifica qué archivos/carpetas **no** deben subirse a Git.
  - **Contenido típico**:
    ```
    node_modules/
    package-lock.json
    .env
    ```
- **`.env`**: Archivo para variables de entorno sensibles (contraseñas, claves API).
  - **Ejemplo**:
    ```
    PASSWORD=mi_contraseña_secreta
    ```
  - **Uso seguro**: Nunca se debe subir a GitHub. Se ignora con `.gitignore`.

- **Inicializar repositorio Git**: `git init`

---

### **Conceptos Técnicos Definidos**

| Concepto | Definición |
|--------|----------|
| **Entorno de ejecución** | Plataforma que permite ejecutar código (ej: Node.js para JavaScript). |
| **Asíncrono y no bloqueante** | Modelo donde las operaciones de E/S no detienen la ejecución del programa. |
| **Módulo** | Archivo que encapsula funcionalidad y puede exportar/importar código. |
| **npm (Node Package Manager)** | Gestor de paquetes para instalar librerías de terceros. |
| **package.json** | Archivo que describe el proyecto y sus dependencias. |
| **Dependencia de desarrollo** | Librería usada solo en desarrollo (no en producción). |
| **Variables de entorno** | Datos sensibles o configuraciones que se mantienen fuera del código fuente. |

---

### **Ejemplos Utilizados en el Video**

1. **Creación de módulo local**:
   - Archivo `operaciones.js` con funciones `suma` y `resta`.
   - Importación en `index.js` con `require('./operaciones')`.

2. **Servidor HTTP básico**:
   - Uso del módulo `http` para crear un servidor que responde con JSON.

3. **Gestión de archivos con `fs`**:
   - Crear, leer y eliminar un archivo `texto.txt`.

4. **Peticiones HTTP con `axios`**:
   - Obtener datos de un usuario de la API fake `jsonplaceholder`.

5. **Configuración de `nodemon`**:
   - Automatizar el reinicio del servidor durante el desarrollo.

---

### **Conclusiones y Recomendaciones**

- **Organización del código**: Usa módulos para mantener el código limpio, reutilizable y escalable.
- **Gestión de dependencias**: Distingue entre dependencias de producción y desarrollo. Usa `npm install` correctamente.
- **Seguridad**: Nunca subas `node_modules`, `package-lock.json` ni archivos `.env` a GitHub. Usa `.gitignore`.
- **Desarrollo eficiente**: Utiliza `nodemon` para evitar reiniciar manualmente el servidor.
- **Documentación**: Consulta la documentación oficial de Node.js y de las librerías que uses (ej: MDN, npmjs.com).
- **Práctica constante**: Experimenta con los módulos nativos (`fs`, `http`, `path`) para entender su funcionamiento.

---

### **Pasos a Seguir (Checklist para el Estudiante)**

1. [ ] Instalar Node.js (versión LTS).
2. [ ] Crear una carpeta para el proyecto y abrir VS Code.
3. [ ] Inicializar el proyecto: `npm init` (responder preguntas o usar `-y`).
4. [ ] Crear un archivo `index.js` y probar con `console.log("Hola Mundo")`.
5. [ ] Ejecutar con `node index.js` y luego con `npm start` (tras configurar el script).
6. [ ] Instalar `nodemon` como dependencia de desarrollo: `npm install nodemon --save-dev`.
7. [ ] Agregar script `"dev": "nodemon index.js"` y probar con `npm run dev`.
8. [ ] Crear un módulo local (ej: `matematicas.js`) y usarlo en `index.js`.
9. [ ] Experimentar con módulos nativos (`fs`, `http`).
10. [ ] Crear archivos `.gitignore` y `.env`, y configurarlos correctamente.
11. [ ] Inicializar repositorio Git: `git init`.

Este resumen proporciona una base sólida para comenzar a trabajar con Node.js, entendiendo tanto los conceptos teóricos como las prácticas esenciales del desarrollo moderno.

