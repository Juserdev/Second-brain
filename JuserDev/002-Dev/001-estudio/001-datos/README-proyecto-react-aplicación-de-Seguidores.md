---
Developer: Midudev
Language: React
---
# README: Proyecto React - Aplicación de Seguidores

Este README es un instructivo detallado basado en lo aprendido en la clase de React. Cubre desde conceptos básicos hasta detalles específicos del código implementado en este proyecto. El objetivo es proporcionar una guía paso a paso para entender React, sus componentes, hooks, estructura de proyecto y mejores prácticas. Este documento te servirá como referencia futura para crear aplicaciones similares o más avanzadas.

## 1. Introducción al Proyecto
Este proyecto es una aplicación simple en React que simula una lista de tarjetas de usuarios con funcionalidad de "seguir" o "dejar de seguir". Incluye:
- Un componente principal (`App.jsx`) que renderiza una lista de usuarios.
- Un componente reutilizable (`XFollowCard.jsx`) para cada tarjeta de usuario.
- Estilos CSS para la interfaz.
- Uso de estado con hooks (`useState`) para manejar interacciones dinámicas.

**Objetivos de aprendizaje**:
- Entender la estructura básica de un proyecto React.
- Crear componentes funcionales y reutilizables.
- Manejar props (propiedades) para pasar datos entre componentes.
- Usar hooks para gestionar el estado de la aplicación.
- Implementar eventos y cambios dinámicos en la UI.
- Aplicar estilos básicos con CSS.

El proyecto demuestra conceptos clave como JSX, componentes, estado, eventos y renderizado condicional, todo en un contexto práctico.

## 2. Estructura de Carpetas en un Proyecto React
Un proyecto React típico sigue una estructura organizada para mantener el código limpio y escalable. Basado en herramientas como Vite (usado aquí, similar a Create React App), la estructura general es la siguiente:

```
/proyecto-react/
├── public/                  # Archivos públicos (íconos, imágenes estáticas, index.html base)
│   └── index.html           # Archivo HTML principal que carga la app React
├── src/                     # Código fuente principal de la aplicación
│   ├── components/          # (Opcional) Carpeta para componentes reutilizables
│   ├── App.jsx              # Componente raíz de la aplicación
│   ├── index.jsx            # Punto de entrada donde se renderiza la app en el DOM
│   ├── App.css              # Estilos específicos para App.jsx
│   └── index.css            # Estilos globales
├── package.json             # Archivo de configuración con dependencias y scripts
├── README.md                # Este archivo (documentación)
└── node_modules/            # Dependencias instaladas (generadas automáticamente)
```

- **public/**: Contiene archivos estáticos accesibles directamente (ej. favicon.ico). El archivo `index.html` es el template base donde React inyecta la aplicación.
- **src/**: Aquí va todo el código fuente. `index.jsx` es el entry point que importa y renderiza `App.jsx`.
- **package.json**: Define scripts como `npm run dev` (para desarrollo) y `npm run build` (para producción). Incluye dependencias como `react`, `react-dom`, etc.
- **node_modules/**: Carpeta generada por npm con bibliotecas instaladas. Nunca se sube a Git.

En este proyecto específico:
- `src/App.jsx`: Componente principal que maneja la lógica de la lista de usuarios.
- `src/XFollowCard.jsx`: Componente hijo reutilizable para cada tarjeta.
- `src/App.css`: Estilos para la aplicación.

## 3. Cómo Iniciar un Proyecto con React
React se puede configurar de varias formas. Usaremos Vite (como en este proyecto), que es rápido y moderno, similar a Create React App.

### Pasos Detallados:
1. **Instalar Node.js y npm**:
   - Descarga Node.js desde [nodejs.org](https://nodejs.org). npm viene incluido.
   - Verifica instalación: Abre la terminal y ejecuta `node -v` y `npm -v`.

2. **Crear un Nuevo Proyecto con Vite**:
   - Abre la terminal en la carpeta deseada.
   - Ejecuta: `npm create vite@latest nombre-del-proyecto`.
     - Selecciona "React" como framework cuando se pregunte.
     - Ejemplo: `npm create vite@latest mi-app-react`.
     - Esto crea la estructura de carpetas, instala dependencias y configura scripts.
   - Espera a que termine (puede tomar unos minutos).

3. **Navegar al Proyecto**:
   - `cd nombre-del-proyecto`.

4. **Instalar Dependencias**:
   - Ejecuta `npm install` (aunque suele hacerse automáticamente).

5. **Iniciar el Servidor de Desarrollo**:
   - Ejecuta `npm run dev`.
   - Abre el navegador en `http://localhost:5173` (puerto por defecto de Vite) para ver la app corriendo.
   - El servidor se recarga automáticamente al cambiar archivos (hot reload).

6. **Scripts Útiles**:
   - `npm run dev`: Modo desarrollo (con hot reload).
   - `npm run build`: Crea una versión optimizada para producción en la carpeta `dist/`.
   - `npm run preview`: Vista previa de la build.
   - Para detener el servidor: Presiona `Ctrl + C` en la terminal.

**Notas**:
- Vite es más rápido que CRA para desarrollo. Para producción, usa `npm run build`.
- Siempre ejecuta `npm install` si copias un proyecto existente para instalar dependencias.

## 4. Conceptos Básicos de React
React es una biblioteca para construir interfaces de usuario interactivas. Usa componentes como bloques de construcción.

### 4.1. Componentes
- **Definición**: Un componente es una función o clase que devuelve JSX (JavaScript XML, similar a HTML pero con lógica).
- **Tipos**:
  - **Funcionales**: Basados en funciones (como en este proyecto). Más simples y recomendados desde React 16.8+.
  - **De Clase**: Usan clases ES6, pero menos comunes ahora.
- **Ejemplo Básico**:
  ```jsx
  function MiComponente() {
    return <h1>Hola Mundo</h1>;
  }
  ```
- En este proyecto:
  - `App.jsx` es el componente raíz.
  - `XFollowCard.jsx` es un componente hijo reutilizable.

### 4.2. JSX (JavaScript XML)
- JSX permite escribir HTML-like dentro de JavaScript.
- Reglas clave:
  - Usa `{}` para insertar JavaScript (ej. `{variable}`).
  - Las etiquetas deben cerrarse (ej. `<img />` o `<div></div>`).
  - Clases se definen con `className` (no `class`).
- Ejemplo: `<div className="app">{nombre}</div>` renderiza el valor de `nombre`.

### 4.3. Props (Propiedades)
- Props son datos pasados de un componente padre a un hijo (como argumentos de función).
- Son inmutables (no se pueden cambiar dentro del hijo).
- Uso:
  ```jsx
  // Padre
  <Hijo nombre="Juan" edad={25} />

  // Hijo
  function Hijo({ nombre, edad }) {
    return <p>Hola {nombre}, tienes {edad} años</p>;
  }
  ```
- En este proyecto: `App.jsx` pasa `userName`, `inicialIsFollowing` y `children` a `XFollowCard`.

### 4.4. Estado (State)
- El estado permite que los componentes "recuerden" datos y se actualicen dinámicamente.
- Usa el hook `useState` para componentes funcionales.
- Ejemplo:
  ```jsx
  const [contador, setContador] = useState(0);
  const incrementar = () => setContador(contador + 1);
  ```
- En este proyecto: Se usa para rastrear si un usuario está siendo seguido.

### 4.5. Eventos
- Eventos como `onClick` permiten interacciones (ej. clics en botones).
- Siempre usa funciones (ej. `onClick={miFuncion}`) o arrow functions.
- Ejemplo: `<button onClick={() => console.log('Clicado')}>Click</button>`.

### 4.6. Renderizado Condicional
- Muestra contenido basado en condiciones.
- Ejemplo: `{isFollowing ? 'Siguiendo' : 'Seguir'}`.

## 5. Análisis Detallado del Código
Ahora, vamos paso a paso por el código implementado, incluyendo partes comentadas y cómo funcionan.

### 5.1. Archivo `App.jsx` (Componente Principal)
Este es el componente raíz que orquesta la aplicación.

```jsx
// Importaciones
import { useState } from 'react';  // Importa el hook useState para manejar estado.
import './App.css';                // Importa estilos específicos.
import { XFollowCard } from './XFollowCard';  // Importa el componente hijo.

// Array de usuarios (datos simulados)
const users = [
  { userName: "juserdev", name: "Juan Sebastian Rodriguez", isFollowing: true },
  { userName: "mouredev", name: "Brais Moure", isFollowing: false },
  { userName: "midudev", name: "Miguel Angel Duran", isFollowing: true },
  { userName: "vicalf85", name: "Victor Ahumada", isFollowing: false }
];

// Función del componente App
export function App() {
  // Estado local para un ejemplo comentado (ver abajo).
  const [name, setName] = useState('juserdev');
  const handleSetName = () => { setName('juanserdev'); };  // Función para cambiar el estado.

  return (
    <div className="x-app">  // Contenedor principal con clase CSS.
      {/* Código comentado: Ejemplo inicial de renderizado manual.
          - Se usaba para probar componentes individuales con props fijos.
          - 'children' pasa texto como hijo (ej. nombre del usuario).
          - El botón cambia el estado 'name', pero no afecta la lista dinámica.
      */}
      {/* <XFollowCard userName={name} inicialIsFollowing={true}>
            Juan Sebastian Rodriguez
          </XFollowCard>
          ... (más tarjetas similares)
          <button onClick={handleSetName}>cambiar de nombre</button> */}

      {/* Renderizado dinámico con .map()
          - Itera sobre el array 'users'.
          - Para cada usuario, crea un XFollowCard con props dinámicas.
          - 'key' es obligatorio en listas para que React identifique cambios.
          - Pasa 'userName', 'inicialIsFollowing' y 'name' como 'children'.
      */}
      {users.map(({ userName, name, isFollowing }) => (
        <XFollowCard
          key={userName}  // Clave única para optimización.
          userName={userName}
          inicialIsFollowing={isFollowing}
        >
          {name}  // 'children' recibe el nombre como texto.
        </XFollowCard>
      ))}
    </div>
  );
}
```

- **Funcionamiento Paso a Paso**:
  1. **Importaciones**: Carga React, estilos y componentes necesarios.
  2. **Datos**: `users` es un array de objetos con info de usuarios.
  3. **Estado (comentado)**: `useState` inicializa `name` en 'juserdev'. `handleSetName` lo cambia al hacer clic (ejemplo de interacción).
  4. **Renderizado**:
     - El código activo usa `.map()` para generar tarjetas dinámicamente.
     - Cada `<XFollowCard>` recibe props y muestra datos del usuario.
     - El comentario explica el enfoque inicial (estático vs. dinámico).
  5. **Por Qué Dinámico?**: Es escalable; agregar usuarios al array actualiza la UI automáticamente.

### 5.2. Archivo `XFollowCard.jsx` (Componente Hijo)
Este componente representa una tarjeta de usuario reutilizable.

```jsx
// Importa useState para manejar el estado de "seguir".
import { useState } from "react";

// Función del componente con destructuring de props.
// - 'children': Texto hijo (nombre del usuario).
// - 'userName': Nombre de usuario (por defecto 'unknow' si no se pasa).
// - 'inicialIsFollowing': Estado inicial de seguimiento.
export function XFollowCard({ children, userName = 'unknow', inicialIsFollowing }) {
  // Estado interno: 'isFollowing' empieza con 'inicialIsFollowing'.
  const [isFollowing, setIsFollowing] = useState(inicialIsFollowing);

  // Función para alternar el estado al hacer clic.
  const handleClick = () => { setIsFollowing(!isFollowing); };

  // Genera URL de avatar dinámicamente usando el userName.
  const imageSrc = `https://unavatar.io/${userName}`;

  // Renderizado condicional: Texto del botón cambia según estado.
  const text = isFollowing ? 'Siguiendo' : 'Seguir';
  const buttonClaseName = isFollowing ? 'x-followCard-button is-following' : 'x-followCard-button';

  return (
    <article className='x-followCard'>  // Elemento raíz (semántico: representa un artículo).
      <header className='x-followCard-header'>
        <img src={imageSrc} alt={`avatar de ${userName}`} className='x-followCard-avatar' />
        <div className='x-followCard-info'>
          <strong>{children}</strong>  // Muestra el nombre (children).
          <span className="x-followCard-username">@{userName}</span>
        </div>
      </header>
      <aside>
        <button className={buttonClaseName} onClick={handleClick}>
          <span className="x-followCard-text">{text}</span>
          <span className="x-followCard-stopFollow">Dejar de seguir</span>
        </button>
      </aside>
    </article>
  );
}
```

- **Funcionamiento Paso a Paso**:
  1. **Props**: Recibe datos del padre. `children` es el nombre, `userName` para el avatar y username, `inicialIsFollowing` para el estado inicial.
  2. **Estado Interno**: `useState(inicialIsFollowing)` crea un estado local que persiste en el componente.
  3. **Evento**: `handleClick` invierte `isFollowing` al hacer clic, actualizando la UI dinámicamente.
  4. **Renderizado Condicional**:
     - `text` y `buttonClaseName` cambian basado en `isFollowing`.
     - El botón muestra "Siguiendo" o "Seguir"; en hover (CSS), cambia a "Dejar de seguir".
  5. **Estructura JSX**:
     - `<article>`: Contenedor semántico.
     - `<header>`: Cabecera con avatar e info.
     - `<aside>`: Sección lateral para el botón.
     - Usa `{}` para JavaScript en atributos y contenido.
  6. **Reutilización**: Puedes usar este componente múltiples veces con diferentes props.

### 5.3. Archivo `App.css` (Estilos)
CSS básico para la apariencia. Incluye clases para botones con transiciones y hover effects.

- Ejemplo clave: `.x-followCard-button.is-following:hover .x-followCard-text { display: none; }` oculta el texto y muestra "Dejar de seguir" en hover.

## 6. Hooks y Funciones Específicas
- **useState**: Hook para estado. Toma valor inicial y devuelve [estado, setter]. Ej: `const [count, setCount] = useState(0);`.
- **.map()**: Método de arrays para iterar y generar JSX dinámicamente.
- **Event Handlers**: Funciones como `onClick` responden a interacciones del usuario.

## 7. Mejores Prácticas Aprendidas
- Usa `key` en listas para optimización.
- Separa lógica en componentes pequeños.
- Comenta código para claridad (como en el ejemplo).
- Usa props por defecto (ej. `userName = 'unknow'`).
- Maneja estado localmente donde sea posible.
- Prueba cambios en el navegador con `npm run dev`.

## 8. Próximos Pasos y Expansión
- Agrega más componentes (ej. un formulario para agregar usuarios).
- Usa otros hooks como `useEffect` para efectos secundarios.
- Integra APIs reales para datos dinámicos.
- Aprende routing con React Router para múltiples páginas.
- Despliega en plataformas como Netlify o Vercel.

Este README cubre todos los detalles del proyecto. Si necesitas agregar más funcionalidades o aclaraciones, ¡házmelo saber!

