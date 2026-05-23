**App** es el componente funcional principal de una aplicación de seguimiento de tareas ("To-do") construida con React.

Gestiona el estado global de la lista de tareas y orquesta la interacción entre los componentes secundarios encargados de mostrar, añadir y modificar dichas tareas.

### Definición

El componente `App` define la estructura lógica y de estado de la aplicación. Utiliza el hook `useState` para mantener la tarea que se está escribiendo actualmente (`newTask`) y la lista completa de todas las tareas (`allTask`). Además, implementa funciones manejadoras para la creación, finalización y eliminación de tareas.

```javascript
function App() {
  // Estructura base para una nueva tarea
  const task = {
    id: 0,
    title: '',
    completed: false
  }

  // Actualiza el estado de newTask basado en la entrada del usuario
  const handleOnChange = (e) => {
    setNewTask({
      ...newTask,
      id: allTask.length + 1,
      title: e.target.value
    })
  }

  // Añade la nueva tarea a la lista allTask
  const handleOnSubmit = (e) => {
    e.preventDefault()
    setAllTask([...allTask, newTask])
  }

  // Alterna el estado 'completed' de una tarea específica por ID
  const handleCompleteTask = (id) => {
    const updatedTasks = allTask.map(task => {
      if (task.id === id) {
        return { ...task, completed: !task.completed }
      }
      return task
    })
    setAllTask(updatedTasks)
  }

  // Filtra y elimina una tarea de la lista por ID
  const handleDeleteTask = (id) => {
    const deleteTasks = allTask.filter(task => task.id !== id)
    setAllTask(deleteTasks)
  }

  // Declaración de estados
  const [newTask, setNewTask] = useState(task)
  const [allTask, setAllTask] = useState([])

  return (
    <div className="container-general">
      <h1>To-do</h1>
      <CounterTask allTask={allTask} />
      <TaskForm
        handleOnSubmit={handleOnSubmit}
        handleOnChange={handleOnChange}
        newTask={newTask}
      />
      <TaskList
        allTask={allTask}
        handleCompleteTask={handleCompleteTask}
        handleDeleteTask={handleDeleteTask}
      />
    </div >
  )
}
```

- **Tipo**: Componente Funcional de React.
- **Estado**:
    - `newTask`: Objeto que representa la tarea siendo creada.
    - `allTask`: Array de objetos que almacena todas las tareas existentes.
- **Retorna**: Estructura JSX que renderiza los componentes `CounterTask`, `TaskForm` y `TaskList`.

### Ejemplos de Uso

El componente `App` se utiliza como el punto de entrada raíz de la aplicación React. Se importa y monta dentro del DOM en el archivo principal de arranque.

```javascript
import { createRoot } from 'react-dom/client'
import App from './App.jsx'
import './index.css'

createRoot(document.getElementById('root')).render(
  <App />
)
```

En el código base, `App` es el único componente renderizado en el nivel superior. Actúa como el proveedor de estado y lógica para todos los demás componentes de la interfaz de usuario (`CounterTask`, `TaskForm`, `TaskList`), los cuales reciben propiedades directas de este componente.

### Notas

- La generación de `id` para nuevas tareas se basa en la longitud actual del array `allTask` (`allTask.length + 1`). Esto implica que los IDs no son únicos de forma permanente si se eliminan tareas intermedias, aunque es funcional para este contexto específico.
- El estado inicial `task` (líneas 10-14) sirve como plantilla para inicializar `newTask`, asegurando que cada nueva tarea comience con la estructura correcta (`id`, `title`, `completed`).