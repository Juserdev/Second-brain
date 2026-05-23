**handleDeleteTask** es una función de controlador de eventos diseñada para eliminar tareas del estado de la aplicación.
Filtra la lista de tareas existente para excluir el elemento con el identificador especificado y actualiza el estado para reflejar este cambio.

### Definición

La función utiliza el método `filter` sobre el array `allTask` para crear un nuevo array que no contiene la tarea con el `id` proporcionado, y luego actualiza el estado de React.

```javascript
const handleDeleteTask = (id) => {
  // Crea un nuevo array excluyendo la tarea cuyo ID coincide con el argumento
  const deleteTasks = allTask.filter(task => task.id !== id)
  setAllTask(deleteTasks)
}
```

- **Parámetros**: `id` (número o identificador único de la tarea a eliminar).
- **Efectos secundarios**: Actualiza el estado `allTask` invocando `setAllTask`, lo que provoca que la interfaz de usuario se vuelva a renderizar.
- **Retorna**: `undefined` (la función no devuelve ningún valor).

### Ejemplos de Uso

La función se define en el componente principal `App` y se pasa hacia abajo a través de los componentes hijos hasta llegar al elemento de la interfaz que activa la acción.

En el componente `TaskItem`, se invoca cuando el usuario hace clic en el botón de eliminar:

```javascript
tracker/src/components/TaskItem.jsx
<button className='delete-task' onClick={() => handleDeleteTask(task.id)} >eliminar tarea</button>
```

En el componente `App`, la función se transmite al componente `TaskList` como una propiedad (prop):

```javascript
<TaskList
  allTask={allTask}
  handleCompleteTask={handleCompleteTask}
  handleDeleteTask={handleDeleteTask}
/>
```

**Resumen de uso**: `handleDeleteTask` es central en la gestión de estado de la aplicación. Se utiliza para mantener la lista de tareas sincronizada con las acciones del usuario, siguiendo el flujo de datos de React donde la lógica de modificación de datos reside en el componente padre (`App`) y se ejecuta mediante callbacks en componentes descendientes (`TaskList` y `TaskItem`).

### Notas

- La implementación utiliza el método `filter` para asegurar la inmutabilidad del estado; en lugar de mutar el array original, se crea uno nuevo, lo cual es una práctica recomendada en React.
- Dependiendo del cierre (closure) de la función, tiene acceso a las variables de estado `allTask` y `setAllTask` definidas en el componente `App`.