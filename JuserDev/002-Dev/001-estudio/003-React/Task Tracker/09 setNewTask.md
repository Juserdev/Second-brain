**setNewTask** es una función de actualización de estado de React.

Gestiona los datos temporales de la tarea que el usuario está escribiendo actualmente en el formulario.

### Definición

```javascript
const [newTask, setNewTask] = useState(task)
```

- **Parámetros**: Un objeto `task` (con `id`, `title`, `completed`) para actualizar el estado.
- **Efectos secundarios**: Provoca el re-renderizado del componente `App` al cambiar el estado.
- **Retorna**: Vacío (void).

### Ejemplos de Uso

El uso principal de `setNewTask` ocurre dentro de la función `handleOnChange`, que se ejecuta cada vez que el usuario interactúa con el campo de entrada del formulario. Aquí se actualiza el título de la tarea y se calcula un nuevo ID basado en la longitud actual de la lista de tareas.

```javascript
const handleOnChange = (e) => {
  setNewTask({
    ...newTask,
    id: allTask.length + 1,
    title: e.target.value
  })
}
```

En el código base, `setNewTask` se define en el componente principal `App` y se invoca únicamente a través de `handleOnChange` para mantener la sincronización entre la entrada del usuario y el estado local de la nueva tarea.

### Notas

- Se inicializa utilizando el hook `useState` con un objeto `task` predeterminado que tiene `id: 0`, `title: ''` y `completed: false`.
- Al llamar a `setNewTask`, se utiliza el operador de propagación (`...newTask`) para copiar las propiedades anteriores y evitar la pérdida de datos (como el estado `completed`) si se añadieran más campos al objeto en el futuro.