---
Date: 20-05-2026
project: Menu
category: tasks
---
#  Fase 2: Arquitectura de la Información y Estructura de Prompts

Este documento contiene las tareas específicas que debemos resolver de forma escrita. Las respuestas a estas tareas se utilizarán como especificaciones técnicas definitivas para estructurar los prompts de desarrollo modular.

---

## Bloque 1: Definición del Core y la Estructura de Datos (Base del Proyecto)

La resolución de este bloque generará el contexto para el "Prompt 1: Inicialización del Proyecto y Arquitectura de Datos".

- [/] **Tarea 1.1: Diseño del Schema del Archivo JSON**
  Debemos definir la estructura exacta del archivo `menuData.json`. Es necesario determinar cómo se estructurarán los objetos para que la plantilla sea reutilizable. 
  * Especificar los campos obligatorios para la información del restaurante (nombre, logo, colores primarios y secundarios, redes sociales).
  * Especificar la estructura de los productos (id, nombre, descripción corta, descripción larga, precio, ruta de imagen pequeña, ruta de imagen grande, etiqueta de recomendado).
  * Definir cómo se agruparán los productos dentro de sus respectivas categorías.

- [ ] **Tarea 1.2: Definición del Setup Inicial de Desarrollo**
  Determinar las especificaciones del entorno local que se le indicarán al modelo para que configure el proyecto base sin conflictos.
  * Confirmar el uso de Vite.js con React y JavaScript gestionado a través de pnpm.
  * Definir la estructura de carpetas inicial (components, hooks, context, mockData, assets).
  * Establecer la lógica de consumo del archivo JSON (si se leerá mediante un estado global con Context API o mediante un hook personalizado).

---

## Bloque 2: Arquitectura de Componentes de la Interfaz (Estructura Mobile)

La resolución de este bloque generará las especificaciones para los prompts de UI/UX paso a paso.

- [ ] **Tarea 2.1: Comportamiento del Layout General y Header**
  * Definir si el Header será estático o fijo (sticky) en la parte superior al hacer scroll.
  * Especificar la posición del logotipo y si se incluirán botones de acción inmediata (como un botón flotante o directo para contacto/redes sociales).

- [ ] **Tarea 2.2: Lógica del Navegador de Categorías (UX de Navegación)**
  * Determinar el tipo de componente para cambiar de categoría en dispositivos móviles (menú horizontal con scroll lateral o sistema de pestañas).
  * Definir si se implementará la lógica de Scroll Espía (ScrollSpy), donde la categoría activa cambia automáticamente según la posición de la pantalla del usuario.

- [ ] **Tarea 2.3: Estructura de las Tarjetas de Producto (Vista General)**
  * Diseñar la distribución visual en el espacio móvil: definir si se organizarán en una sola columna con tarjetas anchas o en dos columnas con tarjetas cuadradas.
  * Establecer el comportamiento de interacción: definir si toda la tarjeta es cliqueable para abrir el detalle o si se requiere un botón específico de acción.

- [ ] **Tarea 2.4: Comportamiento del Modal de Detalle (Vista de Plato Abierto)**
  * Definir la distribución del contenido dentro del detalle: jerarquía de la foto grande, el título, el precio destacado y la descripción de los ingredientes.
  * Especificar los métodos de cierre de la vista de detalle para garantizar una experiencia móvil nativa (botón de cierre visible, clic fuera del contenedor o gesto de deslizar).

---

## Bloque 3: Especificaciones de la Landing Page de Venta

La resolución de este bloque generará el prompt para la vitrina comercial del servicio.

- [ ] **Tarea 3.1: Estructura y Secciones de la Landing Page**
  * Definir la secuencia de secciones de la página web principal (Hero, Características del producto, Demostración visual, Sección de beneficios frente al PDF tradicional, Formulario de contacto o llamado a la acción).

>[!TIP]
>La mejor estructura para IA en terminal como OpenCode es la estructure ROTE (Rol, Objetivo, Tarea, Entorno)

---
# Tareas resueltas

-  [[1.1 Diseño del Schema del Archivo JSON]]
