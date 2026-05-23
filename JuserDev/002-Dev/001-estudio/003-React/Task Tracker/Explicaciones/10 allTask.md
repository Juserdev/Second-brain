**allTask** es una variable de estado de React que almacena el listado de tareas.

Gestiona la colección principal de datos en la aplicación, permitiendo realizar operaciones de creación, actualización y eliminación sobre las tareas.

### Definición

```javascript
const [allTask, setAllTask] = useState([])
```

- **Tipo**: Array (lista).
- **Valor inicial**: `[]` (array vacío).
- **Elementos**: Objetos con la estructura `{ id: number, title: string, completed: boolean }`.
- **Origen**: Definido dentro del componente principal `App` utilizando el hook `useState`.

### Ejemplos de Uso

El símbolo `allTask` se utiliza extensamente dentro del componente `App` para manipular la lista de tareas y se pasa como propiedad ("prop") a componentes hijos.

**Adición de una tarea**
En la función `handleOnSubmit`, se actualiza el estado añadiendo la nueva tarea (`newTask`) al array existente utilizando el operador spread.

```javascript
const handleOnSubmit = (e) => {
  e.preventDefault()
  setAllTask([...allTask, newTask]) // Crea un nuevo array con la tarea añadida
}
```

**Eliminación de una tarea**
En la función `handleDeleteTask`, se utiliza el método `filter` para excluir la tarea cuyo ID coincide con el proporcionado, actualizando el estado con el resultado.

```javascript
const handleDeleteTask = (id) => {
  const deleteTasks = allTask.filter(task => task.id !== id)
  setAllTask(deleteTasks) // Actualiza el estado solo con las tareas que no coinciden con el ID
}
```

**Paso a componentes hijos**
`allTask` se transmite a los componentes `CounterTask` y `TaskList` para que puedan renderizar la información o contar las tareas.

```javascript
<CounterTask allTask={allTask} />
{/* ... */}
<TaskList
  allTask={allTask}
  handleCompleteTask={handleCompleteTask}
  handleDeleteTask={handleDeleteTask}
/>
```

**Resumen de uso**
El símbolo es central en `src/App.jsx`, apareciendo en 7 referencias dentro del archivo. También se consume en `src/components/CounterTask.jsx` para calcular estadísticas y en `src/components/TaskList.jsx` para iterar y renderizar los elementos de la lista.

### Notas

- **Generación de IDs**: El ID para una nueva tarea se calcula dinámicamente como `allTask.length + 1` dentro del manejador de eventos `handleOnChange` (línea 20). Esto significa que el ID se asigna durante la escritura del título y se basa en la longitud actual del array en ese momento.