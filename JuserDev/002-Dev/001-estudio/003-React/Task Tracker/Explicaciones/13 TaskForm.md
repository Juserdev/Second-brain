**TaskForm** es un componente funcional de React diseñado para el ingreso de datos de tareas.
Facilita la creación de nuevas tareas mediante un formulario controlado que delega la lógica de estado en el componente padre.

**Definición**

El componente `TaskForm` renderiza un elemento de formulario HTML que contiene un campo de entrada de texto y un botón. No gestiona su propio estado interno; en su lugar, recibe el valor actual de la tarea y los manejadores de eventos a través de `props`.

```jsx
export const TaskForm = ({ handleOnSubmit, handleOnChange, newTask }) => {
  return (
    <form className="form" onSubmit={handleOnSubmit}>
      <input
        type="text"
        className="input-task"
        value={newTask.title}
        onChange={handleOnChange}
      />
      <button className="btn-task">
        Agregar tarea
      </button>
    </form>
  )
}
```

- **Parámetros**:
  - `handleOnSubmit`: Función invocada al enviar el formulario.
  - `handleOnChange`: Función invocada al cambiar el valor del input.
  - `newTask`: Objeto que contiene el estado actual de la tarea (específicamente `newTask.title`).
- **Efectos secundarios**: Ninguno directo en el DOM o estado global; la interacción se maneja mediante las funciones pasadas por `props`.
- **Retorna**: Un elemento JSX que representa un formulario.

**Ejemplos de Uso**

`TaskForm` es utilizado principalmente por el componente `App` para permitir a los usuarios agregar elementos a la lista de tareas. En el siguiente ejemplo, `App` pasa el estado `newTask` y las funciones para manejar los cambios y el envío del formulario.

```jsx
<TaskForm
  handleOnSubmit={handleOnSubmit}
  handleOnChange={handleOnChange}
  newTask={newTask}
/>
```

En el código base, este componente se instancia una vez dentro del componente principal `App` (ubicado en `/Users/juanse/Developer/Practica/Ract/04-task-tracker/src/App.jsx`). Sirve como la interfaz principal para la mutación de datos en la aplicación, permitiendo la expansión del array `allTask`.

**Notas**

- El componente es un "componente controlado", ya que el valor del input (`value={newTask.title}`) es derivado del estado del padre en lugar de mantener un estado local.
- El botón dentro del formulario no tiene un atributo `type` explícito, por lo que actúa como un botón de envío (`submit`) por defecto, activando el evento `onSubmit` del formulario padre.