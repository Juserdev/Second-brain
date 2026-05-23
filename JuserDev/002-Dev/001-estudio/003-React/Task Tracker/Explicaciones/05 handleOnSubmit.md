**handleOnSubmit** es un manejador de eventos de formulario en React.
Su función principal es agregar una nueva tarea al estado de la aplicación y evitar la recarga de la página durante el envío.

**Definición**

```javascript
const handleOnSubmit = (e) => {
  e.preventDefault() // Evita el comportamiento de recarga por defecto del formulario
  setAllTask([...allTask, newTask]) // Actualiza el estado 'allTask' añadiendo la nueva tarea
}
```

- **Parámetros**: `e` (Objeto de evento sintético de React).
- **Efectos secundarios**: Actualiza el estado `allTask` utilizando la función `setAllTask`.
- **Retorna**: `undefined` (implícito).

**Ejemplos de Uso**

El símbolo se define en el componente principal `App` y se pasa como propiedad (`prop`) al componente `TaskForm`.

En `App.jsx`, la función se transfiere al componente hijo:

```javascript
<TaskForm
  handleOnSubmit={handleOnSubmit} // Se pasa la función para manejar el envío
  handleOnChange={handleOnChange}
  newTask={newTask}
/>
```

Dentro de `TaskForm.jsx`, se asigna al evento `onSubmit` del elemento HTML `<form>`:

```javascript
export const TaskForm = ({ handleOnSubmit, ... }) => {
  return (
    <form
      className="form"
      onSubmit={handleOnSubmit} // Se ejecuta al presionar el botón de enviar
    >
```

**Resumen de uso**
`handleOnSubmit` es utilizado exclusivamente en el archivo `App.jsx` y `TaskForm.jsx`. Actúa como el puente entre la interfaz de usuario de entrada de datos (`TaskForm`) y la lógica de estado de la aplicación (`App`) para persistir nuevas tareas en la lista.

**Notas**

- Utiliza el operador de propagación (`...`) para crear una nueva copia del array `allTask` antes de añadir `newTask`, asegurando la inmutabilidad del estado en React.
- Dependiendo de que el estado `newTask` se haya actualizado correctamente (probablemente a través de `handleOnChange`) antes de la ejecución, ya que simplemente toma el valor actual de `newTask` del estado.