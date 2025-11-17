**Resumen Completo y Estructurado de la Clase: "Creación de Rutas RESTful"**  
*(Quinta clase del curso de Node.js y React.js)*

---

### **Tema Principal del Video**
La clase se centra en la **implementación de la lógica de negocio** en una API RESTful mediante **controladores**, utilizando una **base de datos simulada en memoria** (arreglo de JavaScript). Se cubren operaciones CRUD completas (Crear, Leer, Actualizar, Eliminar) para el recurso `users`, incluyendo validación de datos con la librería **Joi** y manejo de errores mediante `try...catch`.

---

### **Puntos Clave Explicados (Con Detalle)**

#### **1. Separación de Responsabilidades: Handlers vs. Controladores**
- **Handlers**: Se encargan únicamente de:
  - Recibir la solicitud HTTP (`req`).
  - Extraer datos de `params`, `query` o `body`.
  - Llamar al controlador correspondiente.
  - Enviar la respuesta (`res`) o manejar errores.
- **Controladores**: Contienen la **lógica de negocio**:
  - Interactúan con la "base de datos" (en este caso, un arreglo en memoria).
  - Validan y transforman datos.
  - Devuelven resultados al handler.

> ✅ **Beneficio**: Código modular, mantenible y escalable.

#### **2. Simulación de Base de Datos**
Dado que aún no se trabaja con una base de datos real (como MongoDB o PostgreSQL), se crea una **base de datos en memoria** usando un arreglo:

```javascript
// src/database/database.js
const users = [
  { id: 1, name: "Martín", username: "martin", email: "martin@mail.com" },
  { id: 2, name: "Norman", username: "norman", email: "norman@mail.com" }
];

module.exports = users;
```

> ⚠️ **Advertencia**: Los datos **no persisten** entre reinicios del servidor.

#### **3. Implementación del CRUD para Usuarios**

##### **a) Crear Usuario (`POST /api/users`)**
- **Controlador**:
  - Genera un `id` único (basado en `users.length + 1`).
  - Crea un nuevo objeto `user` con los datos del `body`.
  - Agrega el usuario al arreglo con `users.push()`.
  - Retorna el nuevo usuario.
- **Validación previa**: Se verifica que `name`, `username` y `email` estén presentes.

##### **b) Obtener Todos los Usuarios (`GET /api/users`)**
- **Controlador**: Retorna todo el arreglo `users`.

##### **c) Filtrar por Nombre (`GET /api/users?name=Norman`)**
- **Controlador**: Usa `Array.filter()` para buscar coincidencias (case-sensitive).
- **Handler**: Si no se pasa `name` en la query, devuelve todos los usuarios.

##### **d) Obtener un Usuario por ID (`GET /api/users/:id`)**
- **Controlador**: Usa `Array.find()` para buscar por `id`.
- **Conversión de tipo**: El `id` de la URL es un string, por lo que se compara como número (`Number(id)`) o con igualdad débil (`==`).

##### **e) Actualizar Usuario (`PUT /api/users/:id`)**
- **Controlador**:
  - Busca el usuario por `id`.
  - Usa `Object.assign()` para sobrescribir sus propiedades con los nuevos datos del `body`.
  - Retorna el usuario actualizado.

##### **f) Eliminar Usuario (`DELETE /api/users/:id`)**
- **Controlador**:
  - Usa `Array.findIndex()` para obtener el índice del usuario.
  - Si existe (`index !== -1`), elimina el elemento con `Array.splice(index, 1)`.
  - Retorna el usuario eliminado.

#### **4. Manejo de Errores con `try...catch`**
- En los **handlers**, se envuelve la llamada al controlador en un bloque `try...catch`.
- Si el controlador lanza un error (por ejemplo, datos incompletos), el `catch` responde con un código de estado `400` y un mensaje descriptivo.

```javascript
// Ejemplo en handler
try {
  const response = createUserController(req.body);
  res.status(201).json(response);
} catch (error) {
  res.status(400).json({ error: error.message });
}
```

#### **5. Validación de Esquemas con Joi**
- **Joi** es una librería para validar la estructura y el tipo de datos de los objetos.
- Se define un **esquema** para el usuario:

```javascript
// src/controllers/usersControllers.js
const Joi = require('joi');

const userSchema = Joi.object({
  name: Joi.string().min(3).required(),
  username: Joi.string().min(3).required(),
  email: Joi.string().email().required()
});
```

- En el controlador de creación, se valida el `req.body`:

```javascript
const { error } = userSchema.validate(req.body);
if (error) {
  throw new Error(error.details[0].message);
}
```

> ✅ **Ventaja**: Evita errores por datos mal formateados y mejora la seguridad.

---

### **Conceptos Técnicos Definidos**

| Concepto | Definición |
|--------|----------|
| **Controlador** | Función que contiene la lógica de negocio de una operación (ej: crear un usuario). |
| **Handler** | Función que actúa como intermediaria entre la ruta y el controlador, gestionando `req` y `res`. |
| **CRUD** | Operaciones básicas en una API: **C**reate, **R**ead, **U**pdate, **D**elete. |
| **Base de datos en memoria** | Almacenamiento temporal de datos en un arreglo o variable de JavaScript (no persistente). |
| **Joi** | Librería de validación de esquemas para asegurar que los datos de entrada cumplan con reglas definidas. |
| **try...catch** | Estructura para capturar y manejar errores de forma controlada. |
| **Object.assign()** | Método para copiar propiedades de un objeto a otro (útil para actualizar). |
| **Array.splice()** | Método para eliminar o reemplazar elementos en un arreglo. |

---

### **Ejemplos Utilizados en el Video**

1. **Crear un usuario**:
   - **Solicitud**: `POST /api/users` con body `{ "name": "Elena", "username": "elena", "email": "elena@mail.com" }`.
   - **Respuesta**: `{ "id": 3, "name": "Elena", ... }` (código `201`).

2. **Filtrar usuarios por nombre**:
   - **Solicitud**: `GET /api/users?name=Norman`.
   - **Respuesta**: Arreglo con los usuarios que coincidan.

3. **Manejo de error por datos incompletos**:
   - **Solicitud**: `POST /api/users` sin `email`.
   - **Respuesta**: `{ "error": "email is required" }` (código `400`).

4. **Validación con Joi**:
   - Intentar crear un usuario con `name: "Al"` (menos de 3 caracteres) → error de validación.

---

### **Conclusiones y Recomendaciones**

- **Persistencia de datos**: La base de datos en memoria es solo para desarrollo. En producción, se usará MongoDB o PostgreSQL.
- **Validación es clave**: Siempre validar los datos de entrada para evitar errores y ataques.
- **Códigos de estado HTTP**: Usar los adecuados:
  - `200`: Éxito en lectura.
  - `201`: Recurso creado.
  - `400`: Solicitud incorrecta (datos inválidos).
  - `404`: Recurso no encontrado.
- **Modularidad**: Mantener handlers y controladores separados facilita las pruebas y el mantenimiento.
- **Pruebas con Postman**: Esencial para verificar cada endpoint antes de conectar el frontend.

---

### **Pasos a Seguir (Checklist para el Estudiante)**

1. [ ] Crear la carpeta `src/database` y el archivo `database.js` con el arreglo `users`.
2. [ ] Crear la carpeta `src/controllers` y el archivo `usersControllers.js`.
3. [ ] Implementar las funciones de CRUD en el controlador.
4. [ ] Actualizar los handlers en `src/handlers/usersHandlers.js` para usar los controladores.
5. [ ] Instalar Joi: `npm install joi`.
6. [ ] Agregar validación de esquema en el controlador de creación.
7. [ ] Envolver las llamadas a controladores en `try...catch` en los handlers.
8. [ ] Probar todos los endpoints con Postman.

---

Este resumen proporciona una base sólida para implementar una API RESTful completa con lógica de negocio, validación y manejo de errores, lista para ser conectada a una base de datos real en las próximas clases.
