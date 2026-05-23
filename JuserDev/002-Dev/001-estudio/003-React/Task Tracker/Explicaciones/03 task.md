**task** es un objeto constante que define la estructura de datos base para una tarea en la aplicación.
Sirve como plantilla para inicializar el estado del formulario de creación de tareas, estableciendo los valores predeterminados para los campos de identificación, título y estado de completado.

### Definición

El símbolo `task` se define dentro del componente principal `App` y establece el esquema inicial para los elementos de la lista de tareas.

```javascript
const task = {
  id: 0,
  title: '',
  completed: false
}
```

- **Tipo**: Objeto (constante).
- **Propiedades**:
    - `id`: Identificador numérico (inicializado en 0).
    - `title`: Cadena de texto para el nombre de la tarea (inicializada como vacía).
    - `completed`: Estado booleano que indica si la tarea está terminada (inicializado en `false`).
- **Uso principal**: Valor inicial para el estado de React `newTask`.

### Ejemplos de Uso

El uso principal de `task` ocurre al inicializar el estado local `newTask` utilizando el hook `useState`. Esto asegura que el formulario comience con campos vacíos o en su estado por defecto.

```javascript
const [newTask, setNewTask] = useState(task)
const [allTask, setAllTask] = useState([])
```

En este contexto, `task` proporciona la forma inicial que será modificada por el usuario a través de eventos como `handleOnChange`. Aunque el objeto `task` tiene un `id` estático de 0, la lógica de la aplicación sobrescribe este valor dinámicamente al generar nuevas tareas para evitar duplicados.

### Notas

- El valor de `id` en la constante `task` es 0, pero esto es solo un marcador de posición. La lógica real de asignación de ID se maneja en la función `handleOnChange`, donde se calcula como `allTask.length + 1` antes de agregar la tarea a la lista.