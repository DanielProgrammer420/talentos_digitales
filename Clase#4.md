**Resumen Completo y Estructurado de la Clase: "Express y Middleware"**  
*(Cuarta clase del curso de Node.js y React.js)*

---

### **Tema Principal del Video**
La clase introduce **Express.js**, un framework minimalista y flexible para Node.js, utilizado para crear servidores web y APIs de forma rápida y eficiente. Se abordan conceptos clave como **middleware**, **rutas**, **manejo de solicitudes HTTP**, **organización modular del código** y el uso de herramientas como **Postman** para probar endpoints.

---

### **Puntos Clave Explicados (Con Detalle)**

#### **1. ¿Qué es Express.js?**
- **Definición**: Framework minimalista y flexible para aplicaciones web en Node.js.
- **Propósito**: Facilita la creación de servidores, APIs y aplicaciones web con menos código.
- **Características**:
  - Routing avanzado (rutas dinámicas, anidadas).
  - Soporte para middleware.
  - Organización modular del código.
  - Compatible con plantillas de renderizado (aunque no se usa en APIs REST).

#### **2. Configuración Inicial del Proyecto**
Se sigue un flujo estándar para iniciar un proyecto con Express:

1. **Crear archivo de entrada**: `index.js`.
2. **Inicializar el proyecto**: `npm init` (o `npm init -y`).
3. **Instalar dependencias**:
   - `express`: Framework principal.
   - `nodemon`: Reinicio automático del servidor en desarrollo (`--save-dev`).
   - `morgan`: Middleware para registrar solicitudes (logs).
   - `dotenv`: Para manejar variables de entorno (`.env`).
4. **Configurar scripts en `package.json`**:
   ```json
   "scripts": {
     "dev": "nodemon src/server.js"
   }
   ```

#### **3. Creación de un Servidor Básico con Express**
```javascript
// src/server.js
const express = require('express');
const app = express();
const PORT = process.env.PORT || 3000;

app.get('/', (req, res) => {
  res.send('Hello World');
});

app.listen(PORT, () => {
  console.log(`Servidor escuchando en http://localhost:${PORT}`);
});
```

- **`app.listen()`**: Inicia el servidor en un puerto específico.
- **`app.get()`**: Define una ruta para el método HTTP GET.

#### **4. Middleware: Concepto y Tipos**
- **Definición**: Funciones que se ejecutan **durante el ciclo de solicitud-respuesta** entre el cliente y el servidor.
- **Propósito**: Procesar solicitudes, modificar `req`/`res`, finalizar la solicitud o pasar al siguiente middleware.
- **Tipos**:
  - **De terceros**: `morgan`, `cors`, `helmet`.
  - **Propios de Express**: `express.json()`, `express.urlencoded()`.
  - **Personalizados**: Creados por el desarrollador.

- **Ejemplo de middleware personalizado**:
  ```javascript
  app.use((req, res, next) => {
    console.log('Pasando por mi middleware');
    next(); // ¡Importante! Permite continuar al siguiente middleware o ruta.
  });
  ```

> **¡Atención!**: El orden de los middleware **importa**. Deben definirse **antes** de las rutas que los usan.

#### **5. Organización Modular del Código**
Para mantener el proyecto escalable y legible, se estructura en carpetas:

```
src/
├── server.js          # Punto de entrada del servidor
├── routes/
│   ├── main.routes.js # Rutas principales
│   ├── user.routes.js # Rutas de usuarios
│   └── product.routes.js
└── handlers/
    └── user.handlers.js # Lógica de las rutas
```

- **`main.routes.js`**: Ruta maestra que agrupa rutas específicas.
  ```javascript
  const { Router } = require('express');
  const userRoutes = require('./user.routes');
  const mainRouter = Router();

  mainRouter.use('/users', userRoutes);
  mainRouter.use('/products', productRoutes);

  module.exports = mainRouter;
  ```

- **`user.routes.js`**: Define endpoints para usuarios.
  ```javascript
  const { Router } = require('express');
  const {
    getAllUsersHandler,
    getOneUserHandler,
    createUserHandler
  } = require('../handlers/user.handlers');

  const router = Router();

  router.get('/', getAllUsersHandler);
  router.get('/:id', getOneUserHandler);
  router.post('/', createUserHandler);

  module.exports = router;
  ```

#### **6. Handlers: Separación de Responsabilidades**
- **Propósito**: Encapsular la lógica de cada ruta (validaciones, llamadas a base de datos, etc.).
- **Ventaja**: Mantiene las rutas limpias y enfocadas solo en el enrutamiento.
- **Ejemplo**:
  ```javascript
  // handlers/user.handlers.js
  const getAllUsersHandler = (req, res) => {
    const { name } = req.query; // Obtiene parámetros de la URL
    if (name) {
      return res.send(`Trae al usuario de nombre ${name}`);
    }
    res.send('Trae todos los usuarios');
  };

  module.exports = { getAllUsersHandler, /* ... */ };
  ```

#### **7. Métodos HTTP y Tipos de Parámetros**
Express permite manejar diferentes métodos HTTP y extraer datos de tres formas:

| Tipo | Uso | Ejemplo en URL | Cómo acceder en Express |
|------|-----|----------------|--------------------------|
| **Params** | Identificadores en la ruta | `/users/123` | `req.params.id` |
| **Query** | Filtros opcionales | `/users?name=Juan` | `req.query.name` |
| **Body** | Datos en el cuerpo de la solicitud (POST/PUT) | — | `req.body` *(requiere `express.json()`)* |

> **¡Importante!**: Para usar `req.body`, se debe incluir el middleware `express.json()`:
> ```javascript
> app.use(express.json());
> ```

#### **8. Pruebas con Postman**
- **Propósito**: Probar endpoints sin depender del frontend.
- **Configuración**:
  - Método HTTP (GET, POST, PUT, DELETE).
  - URL del endpoint (ej: `http://localhost:3000/api/users`).
  - Body en formato JSON (para POST/PUT).
- **Organización**: Crear colecciones y carpetas por recurso (usuarios, productos, etc.).

---

### **Conceptos Técnicos Definidos**

| Concepto | Definición |
|--------|----------|
| **Framework** | Conjunto de herramientas y reglas que facilitan el desarrollo de aplicaciones. |
| **Middleware** | Función intermedia que procesa solicitudes antes de llegar a la ruta final. |
| **Routing** | Asociación de URLs y métodos HTTP con funciones que manejan esas solicitudes. |
| **Handler** | Función que contiene la lógica de negocio de una ruta (procesa `req` y devuelve `res`). |
| **Params** | Parámetros obligatorios en la ruta (ej: `/users/:id`). |
| **Query** | Parámetros opcionales en la URL (ej: `?page=1&limit=10`). |
| **Body** | Datos enviados en el cuerpo de la solicitud (usado en POST/PUT). |
| **Modularización** | División del código en módulos reutilizables y autocontenidos. |

---

### **Ejemplos Utilizados en el Video**

1. **Middleware de registro con `morgan`**:
   ```javascript
   const morgan = require('morgan');
   app.use(morgan('dev')); // Muestra logs en consola
   ```

2. **Rutas para usuarios**:
   - `GET /api/users` → Obtiene todos los usuarios.
   - `GET /api/users/:id` → Obtiene un usuario por ID.
   - `POST /api/users` → Crea un nuevo usuario.
   - `PUT /api/users/:id` → Actualiza un usuario.
   - `DELETE /api/users/:id` → Elimina un usuario.

3. **Uso de `req.query` para filtrar**:
   ```javascript
   // GET /api/users?name=Juan
   const { name } = req.query;
   if (name) return res.send(`Usuario: ${name}`);
   ```

4. **Extracción de `req.params`**:
   ```javascript
   // GET /api/users/123
   const { id } = req.params;
   res.send(`Trae el usuario con ID ${id}`);
   ```

5. **Manejo de `req.body`**:
   ```javascript
   // POST /api/users con body: { "name": "Juan" }
   const { name } = req.body;
   res.status(201).send(`Usuario ${name} creado`);
   ```

---

### **Conclusiones y Recomendaciones**

- **Organización es clave**: Usa una estructura modular (`routes`, `handlers`) desde el principio para evitar código desordenado.
- **Middleware obligatorio**: No olvides `express.json()` para procesar JSON en el body.
- **Pruebas con Postman**: Esencial para validar endpoints antes de conectar el frontend.
- **Separación de responsabilidades**: Las rutas deben solo enrutar; la lógica va en los handlers.
- **Variables de entorno**: Usa `.env` para el puerto y otras configuraciones sensibles.
- **Evita el código repetido**: Reutiliza rutas y handlers para múltiples recursos (usuarios, productos, etc.).

---

### **Pasos a Seguir (Checklist para el Estudiante)**

1. [ ] Crear proyecto con `npm init`.
2. [ ] Instalar `express`, `nodemon`, `morgan`, `dotenv`.
3. [ ] Configurar `package.json` con script `"dev": "nodemon src/server.js"`.
4. [ ] Crear estructura de carpetas: `src/`, `src/routes/`, `src/handlers/`.
5. [ ] Implementar servidor básico en `src/server.js`.
6. [ ] Agregar middleware: `express.json()`, `morgan('dev')`.
7. [ ] Crear rutas modulares (`main.routes.js`, `user.routes.js`).
8. [ ] Desarrollar handlers para cada operación (GET, POST, etc.).
9. [ ] Probar endpoints con Postman.
10. [ ] Configurar `.gitignore` para excluir `node_modules/`, `.env`, etc.

Este resumen proporciona una base sólida para construir APIs RESTful con Express.js, siguiendo buenas prácticas de organización y escalabilidad.
