**setAllTask** es una función establecedora de estado de React.
Se encarga de actualizar la lista de tareas (`allTask`) dentro del componente principal de la aplicación.

### Definición

`setAllTask` se crea mediante el hook `useState` para gestionar el estado de la lista de tareas. Inicializa el estado como un array vacío.

```javascript
const [allTask, setAllTask] = useState([])
```

- **Parámetros**: Un nuevo valor (array de objetos) para reemplazar el estado actual de `allTask`.
- **Efectos secundarios**: Desencadena un nuevo renderizado del componente `App` para reflejar los cambios en la interfaz de usuario.
- **Devuelve**: `void` (no devuelve ningún valor).

### Ejemplos de Uso

Esta función se utiliza en tres controladores de eventos dentro del componente `App` para realizar operaciones de creación, actualización y eliminación (CRUD) en la lista de tareas.

**1. Agregar una nueva tarea**
En la función `handleOnSubmit`, se utiliza el operador de propagación para crear un nuevo array que incluye la tarea existente más la nueva tarea.

```javascript
const handleOnSubmit = (e) => {
  e.preventDefault()
  setAllTask([...allTask, newTask]) // Añade la nueva tarea al array
}
```

**2. Alternar el estado de completado**
En `handleCompleteTask`, se utiliza el método `map` para crear una copia del array con la propiedad `completed` de la tarea específica invertida.

```javascript
const handleCompleteTask = (id) => {
  const updatedTasks = allTask.map(task => {
    // ... lógica para alternar 'completed' si el ID coincide ...
    return task
  })
  setAllTask(updatedTasks) // Actualiza la lista con la tarea modificada
}
```

**3. Eliminar una tarea**
En `handleDeleteTask`, se utiliza el método `filter` para excluir la tarea cuyo ID coincide con el proporcionado.

```javascript
const handleDeleteTask = (id) => {
  const deleteTasks = allTask.filter(task => task.id !== id)
  setAllTask(deleteTasks) // Actualiza la lista eliminando la tarea
}
```

**Resumen de uso**
`setAllTask` se invoca exclusivamente en el archivo `src/App.jsx` por las funciones `handleOnSubmit`, `handleCompleteTask` y `handleDeleteTask`. Es el mecanismo central para mutar el estado de la aplicación en respuesta a acciones del usuario.

### Notas

- La función sigue el principio de inmutabilidad de React; en lugar de modificar el array `allTask` directamente (ej. push o splice), siempre se le pasa un nuevo array o una copia modificada del mismo.
- El estado inicial es un array vacío `[]`, lo que significa que la aplicación comienza sin tareas hasta que el usuario añade una.