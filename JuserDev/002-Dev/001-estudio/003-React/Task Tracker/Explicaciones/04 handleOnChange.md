**handleOnChange** es una función de controlador de eventos definida en el componente principal `App`.

Gestiona la actualización del estado de una nueva tarea en tiempo real mientras el usuario escribe en un campo de texto.

### Definición

```javascript
const handleOnChange = (e) => {
  setNewTask({
    ...newTask, // Mantiene las propiedades existentes de la tarea
    id: allTask.length + 1, // Calcula un nuevo ID basado en la longitud actual de la lista
    title: e.target.value // Actualiza el título con el valor ingresado
  })
}
```

- **Parámetros**: `e` (Objeto de evento sintético de React que representa el evento del DOM).
- **Efectos secundarios**: Invoca a la función de estado `setNewTask` para modificar el objeto `newTask`.
- **Retorna**: `undefined` (La función no devuelve ningún valor explícito).

### Ejemplos de Uso

El símbolo `handleOnChange` se utiliza principalmente para conectar el componente hijo `TaskForm` con el estado del padre `App`.

En el componente `App`, la función se pasa como una propiedad (`prop`) al componente `TaskForm`:

```javascript
<TaskForm
  handleOnSubmit={handleOnSubmit}
  handleOnChange={handleOnChange} // Se pasa el manejador al componente de formulario
  newTask={newTask}
/>
```

Dentro del componente `TaskForm`, esta función se asigna al evento `onChange` de un elemento de entrada de texto:

```javascript
<input
  type="text"
  className="input-task"
  value={newTask.title}
  onChange={handleOnChange} // Se ejecuta cada vez que el usuario escribe
/>
```

**Resumen de uso**: Este símbolo es crucial para el flujo de datos unidireccional en la aplicación. Permite que el componente hijo (`TaskForm`) notifique al padre (`App`) sobre los cambios en la entrada del usuario, actualizando el estado local de la nueva tarea antes de que se envíe oficialmente.

### Notas

- El cálculo del `id` (`allTask.length + 1`) se realiza en cada pulsación de tecla. Esto significa que el ID se recalcula dinámicamente mientras se escribe, asegurando que esté basado en el número de tareas existentes en ese momento.
- El uso del operador de propagación (`...newTask`) es esencial para preservar las propiedades predeterminadas del objeto tarea (como `completed: false`) que podrían perderse si solo se actualizara el `title`.