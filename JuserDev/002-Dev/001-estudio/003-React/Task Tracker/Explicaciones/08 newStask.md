**Introducción de dos líneas**
`newTask` es una variable de estado React que representa una tarea individual en proceso de creación.
Actúa como un almacenamiento temporal para los datos ingresados por el usuario en el formulario antes de ser guardados en la lista principal de tareas.

**Definición**

El símbolo se define utilizando el hook `useState` dentro del componente principal `App`. Se inicializa con un objeto `task` predeterminado que contiene la estructura base de una tarea.

```javascript
const task = {
  id: 0,
  title: '',
  completed: false
}
```

```javascript
const [newTask, setNewTask] = useState(task)
```

- **Tipo**: Objeto (estado de React).
- **Valores iniciales**: `{ id: 0, title: '', completed: false }`.
- **Propósito**: Mantener el estado del formulario de entrada para nuevas tareas.

**Ejemplos de Uso**

`newTask` se utiliza principalmente para controlar el componente `TaskForm` y para gestionar la lógica de adición de tareas.

1.  **Actualización en tiempo real**:
    Dentro de la función `handleOnChange`, `newTask` se actualiza cada vez que el usuario escribe en el campo de entrada. Se utiliza el operador spread para preservar las propiedades existentes y se calcula un nuevo `id` basado en la longitud actual de la lista de tareas.

    ```javascript
    const handleOnChange = (e) => {
      setNewTask({
        ...newTask,
        id: allTask.length + 1,
        title: e.target.value
      })
    }
    ```

2.  **Envío del formulario**:
    En la función `handleOnSubmit`, el objeto `newTask` actual se añade al array `allTask` para persistir la tarea.

    ```javascript
    const handleOnSubmit = (e) => {
      e.preventDefault()
      setAllTask([...allTask, newTask])
    }
    ```

3.  **Renderizado**:
    Se pasa como propiedad (`prop`) al componente hijo `TaskForm` para enlazar el valor del campo de texto (`input`) con el estado.

    ```javascript
    <TaskForm
      handleOnSubmit={handleOnSubmit}
      handleOnChange={handleOnChange}
      newTask={newTask}
    />
    ```

**Resumen de uso**
En el código base, `newTask` es central para la funcionalidad de creación de tareas. Se leen y escriben sus valores en `App.jsx` y se consume en `TaskForm.jsx` para mostrar el título actual de la tarea en el input.

**Notas**

- El cálculo del `id` (`id: allTask.length + 1`) ocurre durante el evento de cambio (`onChange`) en lugar de en el envío del formulario. Esto significa que el ID se asigna dinámicamente mientras se escribe, basándose en el número de tareas existentes en ese momento.
- Aunque el objeto inicial define `id: 0`, este valor se sobrescribe inmediatamente en `handleOnChange` antes de que la tarea se agregue a la lista final.
