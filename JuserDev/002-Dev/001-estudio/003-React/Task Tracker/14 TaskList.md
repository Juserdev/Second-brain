**TaskList** es un componente de React encargado de renderizar una lista de tareas.

Itera sobre el array de tareas y genera un componente `TaskItem` para cada una.

**Definición**

El componente `TaskList` se define en `src/components/taskList.jsx`. Recibe tres propiedades: `allTask` (el array de tareas), `handleCompleteTask` (función para marcar como completada) y `handleDeleteTask` (función para eliminar).

```javascript
export const TaskList = ({ allTask, handleCompleteTask, handleDeleteTask }) => {
  return (
    allTask.map(task => {
      return (
        <TaskItem
          key={task.id}
          task={task}
          handleCompleteTask={handleCompleteTask}
          handleDeleteTask={handleDeleteTask}
        />
      )
    })
  )
}
```

- **Params**:
  - `allTask`: Array de objetos de tarea.
  - `handleCompleteTask`: Función para alternar el estado de completado de una tarea.
  - `handleDeleteTask`: Función para eliminar una tarea del array.
- **Side effects**: Ninguno directo (renderizado UI).
- **Returns**: Un array de componentes `TaskItem` generado por `.map()`.

### Ejemplo de Usos

El componente `TaskList` es utilizado principalmente en el componente principal `App` para mostrar la lista de tareas actualizada.

```javascript
      <TaskList
        allTask={allTask}
        handleCompleteTask={handleCompleteTask}
        handleDeleteTask={handleDeleteTask}
      />
```

En este contexto, `App` pasa el estado `allTask` y las funciones de manejo de eventos (`handleCompleteTask`, `handleDeleteTask`) como *props* para que `TaskList` pueda delegar estas acciones a cada `TaskItem` individual. Es el único lugar donde se utiliza este componente en el códigobase proporcionado.

### Notas

- El componente utiliza el método `.map()` para transformar el array de datos en elementos de React, una práctica estándar para renderizar listas.
- La responsabilidad de `TaskList` es puramente de presentación y delegación; no contiene lógica de estado local, sino que recibe todo a través de *props*.