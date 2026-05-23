**CounterTask** es un componente de React funcional.

Muestra estadísticas sobre el estado de una lista de tareas, calculando y visualizando cuántas están completadas y cuántas pendientes.

### Definición

El componente recibe un array de objetos `allTask` como propiedad y deriva tres valores: el número total de tareas, el número de tareas completadas y el número de tareas pendientes. Renderiza estos valores dentro de un contenedor `div` con la clase CSS `container-counter`.

```jsx
export const CounterTask = ({ allTask }) => {
  // Calcula la longitud total del array
  const totalTask = allTask.length
  
  // Filtra tareas donde 'completed' es true y cuenta
  const completedTask = allTask.filter(task => task.completed).length
  
  // Filtra tareas donde 'completed' es false y cuenta
  const pendingTask = allTask.filter(task => !task.completed).length

  return (
    <div className="container-counter">
      <h2>Contador de tareas</h2>
      {/* Muestra los valores calculados */}
      <span>Total: {totalTask}</span>
      <span>Completadas: {completedTask}</span>
      <span>Pendientes: {pendingTask}</span>
    </div>
  )
}
```

- **Parámetros**: `allTask` (Array de objetos de tarea, cada uno con una propiedad booleana `completed`).
- **Efectos secundarios**: Ninguno (componente presentacional).
- **Retorna**: Elemento JSX que contiene un título y tres elementos `span` con los contadores.

### Ejemplo de Uso

El componente se utiliza principalmente dentro del componente principal `App`. En este contexto, `App` mantiene el estado de la lista de tareas en la variable `allTask` y se la pasa a `CounterTask` para que renderice las estadísticas actuales.

```jsx
import { CounterTask } from './components/CounterTask'
// ... otras importaciones y lógica de estado ...

return (
  <div className="container-general">
    <h1>To-do</h1>

    {/* Renderiza el contador pasando el estado actual de tareas */}
    <CounterTask allTask={allTask} />
    
    {/* ... resto de la UI ... */}
  </div>
)
```

### Notas

- El componente asume que cada objeto dentro del array `allTask` posee una propiedad booleana llamada `completed` para realizar el filtrado.
- Es un componente "tonto" o presentacional, ya que no gestiona su propio estado interno ni modifica los datos, solo se limita a mostrar la información derivada de las propiedades que recibe.