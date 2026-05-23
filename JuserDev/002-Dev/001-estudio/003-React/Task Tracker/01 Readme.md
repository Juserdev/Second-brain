# React Task Tracker

Proyecto práctico para aprender React trabajando con:

- Componente
- Props
- useState
- Eventos
- Arrays en estado
- Renderizado dinámico
- Organización de archivos
 
---
# Objetivo

Crear una aplicación para administrar tareas.
La aplicación debe permitir:

- agregar tareas
- marcar tareas como completadas
- eliminar tareas
---
# Requisitos
## Debes crear estos componentes

### TaskForm.jsx

Formulario para crear nuevas tareas.

Debe tener:

- input
- botón agregar
---
### TaskItem.jsx

Tarjeta individual de tarea.

Debe mostrar:

- nombre de la tarea
- estado completada/no completada
- botón completar
- botón eliminar
---
### TaskList.jsx

Componente encargado de renderizar todas las tareas.
Debe recorrer el array de tareas usando `.map()`.
---
# Estado principal

Debes manejar el estado principal en App.jsx.
Ejemplo:

```jsx

const [tasks, setTasks] = useState([])

```

---
# Funcionalidades
## Agregar tarea

Cuando el usuario escriba una tarea y haga click en agregar:

- debe guardarse en el estado
- debe renderizarse automáticamente
---
## Completar tarea

Cuando el usuario haga click en completar:

- debe cambiar el estado de la tarea
- debe verse visualmente diferente

Ejemplo:

- texto tachado
- color diferente
- ícono
---
## Eliminar tarea  

Cuando el usuario haga click en eliminar:

- la tarea debe desaparecer
- el estado debe actualizarse
---
# Datos de cada tarea

Cada tarea debe tener:

- id
- title
- completed

Ejemplo:

```js
{
id: 1,
title: 'Aprender React',
completed: false
}
```

---
# Requisitos técnicos

Debes usar:
## useState

Para guardar:

- lista de tareas
- valor del input  

---
# Estructura sugerida

```txt
src/
components/
├── TaskForm.jsx
├── TaskItem.jsx
├── TaskList.jsx
App.jsx
App.css
```

---
# Meta de aprendizaje

Debes comprender:
## useState con arrays

Cómo actualizar listas dinámicamente.
## map

Cómo renderizar elementos dinámicos.
## filter

Cómo eliminar elementos.
## props

Cómo enviar funciones y datos entre componentes.
## eventos

Cómo manejar clicks e inputs.

---
# Bonus

Cuando termines:
Agregar filtro de tareas.
Debes permitir:

- ver todas
- ver completadas
- ver pendientes
- 
Ejemplo:
- All
- Completed
- Pending
---
# Bonus 2

Agregar contador dinámico.
Ejemplo:

```txt
Total: 10
Completed: 4
Pending: 6
```

---
# Bonus 3

Guardar tareas en localStorage.
Al recargar la página:

- las tareas deben mantenerse

---
# Importante

No copiar solución.
Intentar resolver primero.
Equivocarse es parte del aprendizaje.
Después revisar y refactorizar.