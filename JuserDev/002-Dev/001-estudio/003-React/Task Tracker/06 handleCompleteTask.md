**handleCompleteTask** es una función controladora definida en el componente `App` que alterna el estado de completado de una tarea específica.

Su propósito es actualizar el estado global de la lista de tareas invirtiendo la propiedad booleana `completed` de la tarea cuyo ID coincide con el argumento proporcionado.

### Definición

La función utiliza el método `.map()` para iterar sobre la lista de tareas, creando una nueva matriz con el estado de la tarea objetivo modificado, asegurando la inmutabilidad de los datos en React.

```javascript
const handleCompleteTask = (id) => {
  // Crea una nueva lista de tareas basada en la actual
  const updatedTasks = allTask.map(task => {
    if (task.id === id) {
      return {
        ...task, // Copia las propiedades existentes
        completed: !task.completed // Invierte el estado de completado
      }
    }
    return task // Retorna la tarea sin cambios si no coincide el ID
  })

  setAllTask(updatedTasks) // Actualiza el estado con la nueva lista
}
```

- **Parámetros**: `id` (Identificador numérico de la tarea a modificar).
- **Efectos secundarios**: Actualiza el estado `allTask` mediante la función `setAllTask`, lo que provoca una nueva renderización del componente.
- **Retorna**: `void` (No devuelve ningún valor).

### Ejemplos de Uso

Esta función se utiliza principalmente dentro del componente `TaskItem` para responder a la interacción del usuario al hacer clic en un botón. Se pasa como prop desde `App` hacia `TaskList` y finalmente hacia `TaskItem`.

En el siguiente ejemplo, tomado de `TaskItem.jsx`, la función se invoca cuando el usuario presiona el botón designado para marcar la tarea:

```javascript
tracker/src/components/TaskItem.jsx
<button className='completed-task' onClick={() => handleCompleteTask(task.id)}>
  {task.completed
    ? 'Descompletar tarea'
    : 'Completar tarea'}
</button>
```

En el código base, `handleCompleteTask` es referenciada en `App.jsx` (línea 72) para ser pasada al componente `TaskList`, y luego en `TaskList.jsx` (línea 10) para ser pasada a cada instancia de `TaskItem`.

### Notas

- La implementación utiliza el operador de propagación (`...task`) para garantizar que se conserven todas las demás propiedades del objeto tarea mientras se modifica únicamente el estado `completed`.
- El uso de `!task.completed` permite que el mismo botón sirva tanto para marcar como para desmarcar la tarea, actuando como un interruptor (toggle).