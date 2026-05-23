---
Developer: personal
Language: React
Date: 22-05-2026
---
# Descripción General del Proyecto - Task Tracker

##  Propósito del Reto

Este proyecto es un ejercicio práctico de React diseñado para enseñar los conceptos fundamentales del desarrollo de aplicaciones web modernas. El objetivo principal es construir una aplicación de gestión de tareas (To-Do List) que permita a los usuarios crear, completar y eliminar tareas de manera interactiva.

##  Conceptos Aprendidos

### React Core
- **Componentes**: Creación de componentes reutilizables y modulares
- **Props**: Comunicación entre componentes padre e hijo
- **useState**: Manejo de estado local y global
- **Eventos**: Manejo de eventos de formulario (onSubmit, onChange)

### Manipulación de Arrays
- **map()**: Renderizado dinámico de listas de elementos
- **filter()**: Filtrado de tareas para eliminación y contadores
- **Spread operator**: Creación de copias inmutables de arrays

### Arquitectura
- **Organización de archivos**: Estructura de carpetas components/
- **Separación de responsabilidades**: Cada componente tiene una función específica

##  Cómo Funciona

### Estructura de Componentes

1. **App.jsx** (Componente Principal)
   - Maneja el estado global de la aplicación
   - Contiene la lógica de negocio principal
   - Estado: `newTask` (tarea actual) y `allTask` (lista de todas las tareas)

2. **TaskForm.jsx**
   - Formulario para ingresar nuevas tareas
   - Recibe props: [handleOnSubmit](cci:1://file:///Users/juanse/Developer/Practica/Ract/04-task-tracker/src/App.jsx:24:2-27:3), [handleOnChange](cci:1://file:///Users/juanse/Developer/Practica/Ract/04-task-tracker/src/App.jsx:16:2-22:3), `newTask`
   - Renderiza un input y un botón de agregar

3. **TaskList.jsx**
   - Renderiza la lista completa de tareas
   - Usa `.map()` para iterar sobre el array de tareas
   - Pasa props a cada TaskItem individual

4. **TaskItem.jsx**
   - Representa una tarea individual
   - Muestra el título y estado de completado
   - Contiene botones para completar y eliminar

5. **CounterTask.jsx** (Bonus)
   - Muestra estadísticas en tiempo real
   - Calcula: total, completadas, pendientes
   - Usa `.filter()` para contar tareas por estado

### Flujo de Datos

Usuario escribe tarea 
→ handleOnChange 
→ actualiza newTask Usuario hace submit 
→ handleOnSubmit 
→ agrega a allTask Usuario completa tarea 
→ handleCompleteTask 
→ toggle completed Usuario elimina tarea 
→ handleDeleteTask 
→ filtra array

### Modelo de Datos

Cada tarea tiene la estructura:

```javascript
{
  id: number,        // Identificador único
  title: string,     // Nombre de la tarea
  completed: boolean // Estado de completado
}
```

### Funcionalidades Implementadas

- ✅ **Agregar tareas**: Validación de formulario y actualización de estado
- ✅ **Completar tareas**: Toggle del estado completed con feedback visual
- ✅ **Eliminar tareas**: Filtrado del array para remover elementos
- ✅ **Contador dinámico**: Estadísticas en tiempo real (Bonus 2)
- 🔄 **Filtros**: Pendiente de implementar (Bonus)
- 💾 **localStorage**: Pendiente de implementar (Bonus 3)
- 
## Tecnologías Utilizadas

- **React 19.2.6**: Framework principal
- **Vite 8.0.12**: Herramienta de build y desarrollo
- **JavaScript ES6+**: Sintaxis moderna
- **CSS**: Estilos personalizados
    

##  Valor Educativo

Este proyecto es ideal para principiantes porque:
- Cubre los conceptos esenciales de React
- Tiene una dificultad progresiva (core + bonuses)
- Permite practicar patrones comunes de React
- Tiene resultados visuales inmediatos
- Es extensible para agregar más funcionalidades

## Solución

1. [[01 Readme|01 Readme]]
2. [[02 App]]
3. [[03 task]]
4. [[04 handleOnChange]]
5. [[05 handleOnSubmit]]
6. [[06 handleCompleteTask]]
7. [[07 handleDeleteTask]]
8. [[08 newStask]]
9. [[09 setNewTask]]
10. [[10 allTask]]
11. [[11 setAllTask]]
12. [[12 CounterTask]]
13. [[13 TaskForm]]
14. [[14 TaskList]]
