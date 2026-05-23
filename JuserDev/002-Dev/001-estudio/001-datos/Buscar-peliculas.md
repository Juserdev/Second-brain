---
Developer: Midudev
Language: React
---
Api = http://www.omdbapi.com/?apikey=96c704f&
ApiKye = 96c704f

aqui ets la api que me dan por correo = https://www.omdbapi.com/?i=tt3896198&apikey=96c704f que es como una busqueda rapida para que vemos como funciona la api.

esa es la Api que vamos a estar usando = https://www.omdbapi.com/?apikey=96c704f&s=avengers  

> [!IMPORTANT]
> instalar la extencion "json formatter chrome extension"

---
cree un json desde la api con nombre "with-result.json que es el que contiene la informacion de la peticion que hice y en app.jsx cree lo siguiente al princio

```import responseWithMovies from './mocks/with-result.json'```

segun lo que tiendo aqui es lo siguiente, a resposneWithMovies es algo que cree, que no es una variable ni nada si no que es unombre que le di al documento por decirlo asi, es una varia pero creada como importacion

luego dentro de funcion ``` app(){} ``` creo lo siguiente
  
```
const movies = responseWithMovies.Search
const hasMovies = movies.length > 0
```

aqui lo que entiendo es que creo una vairable movies que tiene la vairable que cree en el import y le di "." para ver si contenido

luego creo hasMovie uqe dice que es true cuando movies.lengt sea mayor a 0

---
## HandleSubmit

la funcion **handleSubmit()** sirve para capturar todos los datos de los imputs de un form al hacer click en el boton de tipo submit de un formulario

```javascript
const handleSubmit = (e) => {
	e.preventDefault()
	const { query } = Object.fromEntries(new window.FormData(e.target))
	console.log(query)
}
```

Aqui es importante tener en cuenta que estamos haciendo **JavaScript** puro y que estamos optimizando el codigo y los recursos.

hacemos uan destructuracion **{ query }** para facilitar el proceso en caso que hayan mas inputs, por ejemplo en el formulario:
```javascript 
<form className="form" onSubmit={handleSubmit}>
	<input name='query' type="text" placeholder='Avengers, Star Wars, The Matrix..' />
	<button type='submit'>Buscar</button>
</form>
```

solamente tenemos un **input** con **name='query'**, pero si tuvieramos otro por ejemplo con **name={apellido}** por ejemplo, en la desctructuracion tendiamos que poner {query, apellido} para capturar los dos inputs.

---
## useRef

```import { useRef } from 'React'```

el useRef no da un valor que persiste en cada renderizado, tambien es util para guardar referencia de elementos del DOM.

--- 
# 📚 Guía Detallada del Proyecto: Buscador de Películas

> Documentación completa del código React utilizado en este proyecto  
> Explicación de cada componente, hook y función con ejemplos reales del código

---

## 📋 Tabla de Contenidos

1. [Estructura del Proyecto](#estructura-del-proyecto)
2. [main.jsx - Punto de Entrada](#mainjsx---punto-de-entrada)
3. [App.jsx - Componente Principal](#appjsx---componente-principal)
4. [Custom Hook: useSearch](#custom-hook-usesearch)
5. [Custom Hook: useMovies](#custom-hook-usemovies)
6. [Components: Movies](#components-movies)
7. [Services: movies.js](#services-moviesjs)
8. [Hooks Utilizados en el Proyecto](#hooks-utilizados-en-el-proyecto)
9. [Flujo de Datos Completo](#flujo-de-datos-completo)
10. [Conceptos Clave Aplicados](#conceptos-clave-aplicados)

---

## Estructura del Proyecto

```
src/
├── main.jsx              # Punto de entrada de la aplicación
├── App.jsx               # Componente principal
├── App.css               # Estilos del componente principal
├── components/
│   └── Movies.jsx        # Componente para mostrar películas
├── hooks/
│   └── useMovies.js      # Custom Hook para manejar películas
└── services/
    └── movies.js         # Servicio para llamadas a la API
```

---

## main.jsx - Punto de Entrada

### 📄 Código Completo

```jsx
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.jsx'

const root = createRoot(document.getElementById('root'))
root.render(
    <App />
)
```

### 🔍 Explicación Detallada

#### ¿Qué hace este archivo?

Este es el **punto de entrada** de toda la aplicación React. Es el primer archivo que se ejecuta cuando cargas la aplicación.

#### Línea por línea:

**Línea 1:** `import { createRoot } from 'react-dom/client'`
- Importa la función `createRoot` de React 18+
- Esta función es la nueva forma de montar aplicaciones React (reemplaza a `ReactDOM.render`)
- Permite el uso de las nuevas características de React 18 como Concurrent Rendering

**Línea 2:** `import './index.css'`
- Importa los estilos globales de la aplicación
- Se aplican a toda la aplicación

**Línea 3:** `import App from './App.jsx'`
- Importa el componente principal `App`
- Este componente es la raíz de toda la aplicación

**Línea 5:** `const root = createRoot(document.getElementById('root'))`
- Busca el elemento HTML con id "root" en index.html
- Crea un root de React en ese elemento
- Este es el punto donde React se "conecta" al DOM real

**Líneas 6-8:** `root.render(<App />)`
- Renderiza el componente `App` dentro del root
- A partir de aquí, React toma el control y maneja todo el contenido

### 💡 Concepto Clave: Renderizado

El **renderizado** es el proceso donde React toma tus componentes (JSX) y los convierte en elementos del DOM que el navegador puede mostrar.

---

## App.jsx - Componente Principal

### 📄 Código Completo

```jsx
import { useState, useEffect, useRef } from 'react'
import { Movies } from './components/Movies'
import { useMovies } from './hooks/useMovies'
import './App.css'

function useSearch() {
  const [search, updateSearch] = useState('')
  const [error, setError] = useState(null)
  const isFirstInput = useRef(true)

  useEffect(() => {

    if (isFirstInput.current) {
      isFirstInput.current = search !== ''
      return
    }

    if (search === '') {
      setError('No se puede buscar una película sin una consulta')
      return
    }

    if (search.match(/^\d+$/)) {
      setError('No se puede buscar una película con un número')
      return
    }

    if (search.length < 3) {
      setError('La consulta debe tener al menos 3 caracteres')
      return
    }
    setError(null)
  }, [search])

  return { search, updateSearch, error }

}


function App() {
  const [sort, setSort] = useState(false)

  const { search, updateSearch, error } = useSearch()
  const { movies: mappedMuvies, getMovies, loading } = useMovies({ search, sort })

  const handleSubmit = (e) => {
    e.preventDefault()
    getMovies({ search })
  }

  const handleSort = () => {
    setSort(!sort)
  }

  const handleChange = (e) => {
    const newSearch = e.target.value
    updateSearch(newSearch)
  }

  return (

    <div className='page'>
      <header>
        <h1>Buscador de películas</h1>
        <form className="form" onSubmit={handleSubmit}>
          <input onChange={handleChange} value={search} name='query' type="text" placeholder='Avengers, Star Wars, The Matrix..' />
          <input type="checkbox" onChange={handleSort} checked={sort} />
          <button type='submit'>Buscar</button>
        </form>
        {error && <p style={{ color: 'red' }}>{error}</p>}
      </header>

      <main>
        {
          loading ? <p>Cargando...</p> : <Movies movies={mappedMuvies} />
        }

      </main>
    </div>

  )
}

export default App
```

### 🔍 Análisis Completo del Componente

---

## Custom Hook: useSearch

Este es un **Custom Hook** que maneja toda la lógica de validación de búsqueda.

### 📄 Código del Hook

```jsx
function useSearch() {
  const [search, updateSearch] = useState('')
  const [error, setError] = useState(null)
  const isFirstInput = useRef(true)

  useEffect(() => {

    if (isFirstInput.current) {
      isFirstInput.current = search !== ''
      return
    }

    if (search === '') {
      setError('No se puede buscar una película sin una consulta')
      return
    }

    if (search.match(/^\d+$/)) {
      setError('No se puede buscar una película con un número')
      return
    }

    if (search.length < 3) {
      setError('La consulta debe tener al menos 3 caracteres')
      return
    }
    setError(null)
  }, [search])

  return { search, updateSearch, error }
}
```

### 🔍 Explicación Línea por Línea

#### **Línea 7:** `const [search, updateSearch] = useState('')`

**¿Qué es `useState`?**
- Es un Hook que permite agregar **estado** a un componente funcional
- El estado es información que puede cambiar con el tiempo
- Cuando el estado cambia, React re-renderiza el componente

**¿Qué hace en este código?**
- `search`: Variable que guarda el texto de búsqueda actual (ej: "Avengers")
- `updateSearch`: Función para actualizar el valor de `search`
- `''`: Valor inicial (cadena vacía)

**Ejemplo de uso:**
```jsx
// Al cargar: search = ''
updateSearch('Avengers')  // Ahora: search = 'Avengers'
updateSearch('Matrix')    // Ahora: search = 'Matrix'
```

---

#### **Línea 8:** `const [error, setError] = useState(null)`

**¿Qué hace?**
- `error`: Guarda el mensaje de error (si existe)
- `setError`: Función para cambiar el error
- `null`: Valor inicial (sin error)

**Estados posibles:**
```jsx
error = null  // Sin error
error = 'No se puede buscar una película sin una consulta'  // Con error
```

---

#### **Línea 9:** `const isFirstInput = useRef(true)`

**¿Qué es `useRef`?**
- Es un Hook que permite guardar un valor que **persiste entre renders**
- A diferencia de `useState`, cambiar un `useRef` NO causa un re-render
- Se accede al valor con `.current`

**¿Por qué usar `useRef` aquí?**
- Para rastrear si es la primera vez que el usuario escribe
- Evita mostrar errores cuando el usuario aún no ha interactuado con el input

**Ejemplo:**
```jsx
// Al cargar la app:
isFirstInput.current = true   // Primera vez

// Usuario escribe "A":
isFirstInput.current = false  // Ya no es la primera vez
```

**Diferencia con `useState`:**
```jsx
// Con useState - Causa re-render ❌
const [isFirstInput, setIsFirstInput] = useState(true)
setIsFirstInput(false)  // ⚠️ El componente se re-renderiza

// Con useRef - NO causa re-render ✅
const isFirstInput = useRef(true)
isFirstInput.current = false  // ✅ Solo cambia el valor, sin re-render
```

---

#### **Líneas 11-33:** El `useEffect` de Validación

```jsx
useEffect(() => {
  // ... código de validación
}, [search])
```

**¿Qué es `useEffect`?**
- Es un Hook para **efectos secundarios**
- Se ejecuta después de que el componente se renderiza
- El array `[search]` son las **dependencias**: el efecto se ejecuta cuando `search` cambia

**Flujo de ejecución:**
1. El usuario escribe en el input
2. Se llama a `updateSearch(nuevoValor)`
3. `search` cambia
4. React re-renderiza el componente
5. Se ejecuta el `useEffect` (porque `search` está en las dependencias)

---

#### **Líneas 13-16:** Validación de Primera Entrada

```jsx
if (isFirstInput.current) {
  isFirstInput.current = search !== ''
  return
}
```

**¿Qué hace?**
- **Si es la primera vez** (`isFirstInput.current === true`)
- Actualiza `isFirstInput.current` a `false` solo si el usuario escribió algo
- Termina la ejecución con `return` (no valida aún)

**¿Por qué es importante?**
- Evita mostrar errores cuando el input está vacío al cargar la página
- El usuario ve el error solo después de empezar a escribir

**Ejemplo paso a paso:**
```jsx
// 1. App se carga
search = ''
isFirstInput.current = true
// ✅ No muestra error (es la primera vez)

// 2. Usuario escribe "A"
search = 'A'
isFirstInput.current = false  // Ya no es la primera vez
// ⚠️ Ahora sí puede mostrar errores

// 3. Usuario borra todo
search = ''
isFirstInput.current = false  // Ya escribió antes
// ⚠️ Muestra error: "No se puede buscar sin consulta"
```

---

#### **Líneas 18-21:** Validación de Campo Vacío

```jsx
if (search === '') {
  setError('No se puede buscar una película sin una consulta')
  return
}
```

**¿Qué hace?**
- Si el campo está vacío, establece un mensaje de error
- El `return` detiene las validaciones siguientes

**Flujo:**
```jsx
search = ''
→ setError('No se puede buscar una película sin una consulta')
→ error = 'No se puede buscar una película sin una consulta'
→ React re-renderiza
→ Se muestra el error en pantalla
```

---

#### **Líneas 23-26:** Validación de Solo Números

```jsx
if (search.match(/^\d+$/)) {
  setError('No se puede buscar una película con un número')
  return
}
```

**¿Qué hace?**
- `search.match(/^\d+$/)` es una **expresión regular** (regex)
- Verifica si el texto contiene **solo números**

**¿Cómo funciona la regex?**
- `^`: Inicio de la cadena
- `\d+`: Uno o más dígitos (0-9)
- `$`: Fin de la cadena

**Ejemplos:**
```jsx
'123'.match(/^\d+$/)      // ✅ true → Solo números
'Avengers'.match(/^\d+$/) // ❌ null → Tiene letras
'123abc'.match(/^\d+$/)   // ❌ null → Mezcla números y letras
```

---

#### **Líneas 28-31:** Validación de Longitud Mínima

```jsx
if (search.length < 3) {
  setError('La consulta debe tener al menos 3 caracteres')
  return
}
```

**¿Qué hace?**
- Verifica que la búsqueda tenga al menos 3 caracteres
- Evita búsquedas muy cortas que darían demasiados resultados

**Ejemplos:**
```jsx
'A'.length < 3          // true → Error
'Av'.length < 3         // true → Error  
'Ave'.length < 3        // false → OK ✅
'Avengers'.length < 3   // false → OK ✅
```

---

#### **Línea 32:** Limpiar Error

```jsx
setError(null)
```

**¿Qué hace?**
- Si pasa todas las validaciones, elimina cualquier error previo
- `error` vuelve a ser `null`

**Flujo completo de validación:**
```jsx
// Usuario escribe "12"
→ search = '12'
→ useEffect se ejecuta
→ No es primera vez ✅
→ No está vacío ✅
→ Son solo números ❌
→ setError('No se puede buscar una película con un número')

// Usuario escribe "Av"
→ search = 'Av'
→ useEffect se ejecuta
→ No está vacío ✅
→ No son solo números ✅
→ Longitud < 3 ❌
→ setError('La consulta debe tener al menos 3 caracteres')

// Usuario escribe "Avengers"
→ search = 'Avengers'
→ useEffect se ejecuta
→ No está vacío ✅
→ No son solo números ✅
→ Longitud >= 3 ✅
→ setError(null) ✅ Sin errores
```

---

#### **Línea 35:** Return del Hook

```jsx
return { search, updateSearch, error }
```

**¿Qué devuelve?**
Un objeto con tres propiedades:
- `search`: El valor actual de búsqueda
- `updateSearch`: Función para cambiar el valor
- `error`: El mensaje de error (o `null`)

**¿Por qué devolver un objeto?**
Permite usar destructuring en el componente:

```jsx
const { search, updateSearch, error } = useSearch()

// Ahora puedes usar:
console.log(search)        // 'Avengers'
updateSearch('Matrix')     // Cambiar el valor
console.log(error)         // null o mensaje de error
```

---

## Componente App - Análisis del Cuerpo Principal

### 📄 Código del Componente

```jsx
function App() {
  const [sort, setSort] = useState(false)

  const { search, updateSearch, error } = useSearch()
  const { movies: mappedMuvies, getMovies, loading } = useMovies({ search, sort })

  const handleSubmit = (e) => {
    e.preventDefault()
    getMovies({ search })
  }

  const handleSort = () => {
    setSort(!sort)
  }

  const handleChange = (e) => {
    const newSearch = e.target.value
    updateSearch(newSearch)
  }

  return (
    <div className='page'>
      <header>
        <h1>Buscador de películas</h1>
        <form className="form" onSubmit={handleSubmit}>
          <input onChange={handleChange} value={search} name='query' type="text" placeholder='Avengers, Star Wars, The Matrix..' />
          <input type="checkbox" onChange={handleSort} checked={sort} />
          <button type='submit'>Buscar</button>
        </form>
        {error && <p style={{ color: 'red' }}>{error}</p>}
      </header>

      <main>
        {
          loading ? <p>Cargando...</p> : <Movies movies={mappedMuvies} />
        }
      </main>
    </div>
  )
}
```

### 🔍 Análisis Línea por Línea

#### **Línea 41:** `const [sort, setSort] = useState(false)`

**¿Qué hace?**
- Crea un estado para controlar si las películas deben ordenarse alfabéticamente
- `sort`: `true` = ordenar, `false` = no ordenar
- Valor inicial: `false` (sin ordenar)

**Uso:**
```jsx
sort = false  // No ordenar
setSort(true) // Ordenar alfabéticamente
```

---

#### **Línea 43:** `const { search, updateSearch, error } = useSearch()`

**¿Qué hace?**
- Llama al Custom Hook `useSearch()`
- Obtiene: el valor de búsqueda, la función para actualizarlo, y el error

**Es equivalente a:**
```jsx
const resultado = useSearch()
const search = resultado.search
const updateSearch = resultado.updateSearch
const error = resultado.error
```

---

#### **Línea 44:** `const { movies: mappedMuvies, getMovies, loading } = useMovies({ search, sort })`

**¿Qué hace?**
- Llama al Custom Hook `useMovies` pasándole `search` y `sort`
- Obtiene:
  - `movies` (renombrado a `mappedMuvies`): Array de películas
  - `getMovies`: Función para buscar películas
  - `loading`: Estado de carga (true/false)

**Sintaxis especial:** `movies: mappedMuvies`
- Toma la propiedad `movies` y la renombra a `mappedMuvies`

**Equivalente sin renombrar:**
```jsx
const { movies, getMovies, loading } = useMovies({ search, sort })
// Usar: movies
```

**Con renombrado:**
```jsx
const { movies: mappedMuvies, getMovies, loading } = useMovies({ search, sort })
// Usar: mappedMuvies
```

---

#### **Líneas 46-49:** Función `handleSubmit`

```jsx
const handleSubmit = (e) => {
  e.preventDefault()
  getMovies({ search })
}
```

**¿Qué hace?**
Esta función se ejecuta cuando el usuario hace clic en el botón "Buscar" o presiona Enter en el formulario.

**Línea por línea:**

**`e.preventDefault()`**
- `e` es el **evento** del formulario
- Por defecto, los formularios HTML recargan la página al enviarse
- `preventDefault()` **previene** ese comportamiento
- Sin esto, la página se recargaría y perderíamos el estado

**`getMovies({ search })`**
- Llama a la función `getMovies` del hook `useMovies`
- Le pasa el valor actual de `search`
- Esto inicia la búsqueda de películas en la API

**Flujo:**
```jsx
// 1. Usuario hace clic en "Buscar"
→ Se ejecuta handleSubmit(e)
→ e.preventDefault() // Evita recargar la página
→ getMovies({ search: 'Avengers' }) // Busca las películas
```

---

#### **Líneas 51-53:** Función `handleSort`

```jsx
const handleSort = () => {
  setSort(!sort)
}
```

**¿Qué hace?**
- Alterna el estado de `sort` entre `true` y `false`
- Se ejecuta cuando el usuario hace clic en el checkbox

**Operador `!` (negación):**
```jsx
!true  = false
!false = true
```

**Ejemplo paso a paso:**
```jsx
// Estado inicial
sort = false

// Usuario hace clic en checkbox
handleSort() → setSort(!false) → setSort(true) → sort = true

// Usuario vuelve a hacer clic
handleSort() → setSort(!true) → setSort(false) → sort = false
```

---

#### **Líneas 55-58:** Función `handleChange`

```jsx
const handleChange = (e) => {
  const newSearch = e.target.value
  updateSearch(newSearch)
}
```

**¿Qué hace?**
- Se ejecuta cada vez que el usuario escribe en el input
- Actualiza el estado `search` con el nuevo valor

**Línea por línea:**

**`e.target.value`**
- `e`: El evento del input
- `target`: El elemento que disparó el evento (el input)
- `value`: El valor actual del input

**Ejemplo:**
```jsx
// Usuario escribe "A"
e.target.value = 'A'
newSearch = 'A'
updateSearch('A')
→ search = 'A'
→ useEffect en useSearch() se ejecuta
→ Valida la búsqueda

// Usuario escribe "Av"
e.target.value = 'Av'
newSearch = 'Av'
updateSearch('Av')
→ search = 'Av'
→ useEffect se ejecuta nuevamente
```

**¿Por qué no usar directamente `updateSearch(e.target.value)`?**
```jsx
// Se podría escribir así:
const handleChange = (e) => {
  updateSearch(e.target.value)
}

// Pero usar una variable intermedia es más legible
const handleChange = (e) => {
  const newSearch = e.target.value
  updateSearch(newSearch)
}
```

---

### 📄 El Return del Componente - La Interfaz

```jsx
return (
  <div className='page'>
    <header>
      <h1>Buscador de películas</h1>
      <form className="form" onSubmit={handleSubmit}>
        <input onChange={handleChange} value={search} name='query' type="text" placeholder='Avengers, Star Wars, The Matrix..' />
        <input type="checkbox" onChange={handleSort} checked={sort} />
        <button type='submit'>Buscar</button>
      </form>
      {error && <p style={{ color: 'red' }}>{error}</p>}
    </header>

    <main>
      {
        loading ? <p>Cargando...</p> : <Movies movies={mappedMuvies} />
      }
    </main>
  </div>
)
```

#### **El Input de Búsqueda**

```jsx
<input onChange={handleChange} value={search} name='query' type="text" placeholder='Avengers, Star Wars, The Matrix..' />
```

**Propiedades:**
- `onChange={handleChange}`: Se ejecuta al escribir
- `value={search}`: El valor del input (controlado por React)
- `name='query'`: Nombre del campo
- `type="text"`: Tipo de input
- `placeholder`: Texto de ayuda

**Concepto: Input Controlado**

Este es un **input controlado** porque React controla su valor:

```jsx
// ❌ Input NO controlado (mal)
<input type="text" />
// El valor lo maneja el DOM, no React

// ✅ Input controlado (bien)
<input value={search} onChange={handleChange} />
// React controla el valor en todo momento
```

**Flujo:**
```jsx
// 1. Input muestra el valor de 'search'
<input value={search} />  // search = ''

// 2. Usuario escribe "A"
→ onChange={handleChange} se dispara
→ handleChange actualiza search = 'A'
→ React re-renderiza
→ Input muestra "A"
```

---

#### **El Checkbox de Ordenamiento**

```jsx
<input type="checkbox" onChange={handleSort} checked={sort} />
```

**Propiedades:**
- `type="checkbox"`: Es un checkbox
- `onChange={handleSort}`: Se ejecuta al hacer clic
- `checked={sort}`: El estado del checkbox (controlado por React)

**Comportamiento:**
```jsx
// sort = false → Checkbox desmarcado
<input type="checkbox" checked={false} />

// Usuario hace clic
→ onChange={handleSort}
→ setSort(!false) → setSort(true)
→ sort = true

// sort = true → Checkbox marcado
<input type="checkbox" checked={true} />
```

---

#### **Renderizado Condicional del Error**

```jsx
{error && <p style={{ color: 'red' }}>{error}</p>}
```

**¿Cómo funciona?**

Esto usa el operador `&&` (AND lógico) para renderizado condicional:

```jsx
condición && elemento
```

**Si `condición` es verdadera:** renderiza `elemento`  
**Si `condición` es falsa:** no renderiza nada

**Ejemplos:**
```jsx
// Caso 1: Sin error
error = null
null && <p>Error</p>  // → No renderiza nada

// Caso 2: Con error
error = 'La consulta debe tener al menos 3 caracteres'
'La consulta...' && <p style={{ color: 'red' }}>La consulta...</p>
// → Renderiza el párrafo con el mensaje
```

**Equivalente con if:**
```jsx
// Con &&
{error && <p style={{ color: 'red' }}>{error}</p>}

// Equivalente con if
{error ? <p style={{ color: 'red' }}>{error}</p> : null}

// Equivalente con if tradicional
let errorElement = null
if (error) {
  errorElement = <p style={{ color: 'red' }}>{error}</p>
}
return errorElement
```

---

#### **Renderizado Condicional del Loading**

```jsx
{
  loading ? <p>Cargando...</p> : <Movies movies={mappedMuvies} />
}
```

**¿Cómo funciona?**

Esto usa el **operador ternario** (`? :`):

```jsx
condición ? valorSiVerdadero : valorSiFalso
```

**Flujo:**
```jsx
// Caso 1: Cargando
loading = true
true ? <p>Cargando...</p> : <Movies />
// → Muestra "Cargando..."

// Caso 2: Películas cargadas
loading = false
false ? <p>Cargando...</p> : <Movies movies={mappedMuvies} />
// → Muestra el componente Movies con las películas
```

**Equivalente con if:**
```jsx
// Con operador ternario
{loading ? <p>Cargando...</p> : <Movies movies={mappedMuvies} />}

// Equivalente con if
let content
if (loading) {
  content = <p>Cargando...</p>
} else {
  content = <Movies movies={mappedMuvies} />
}
return content
```

---

## Custom Hook: useMovies

Este hook maneja toda la lógica de obtención y ordenamiento de películas.

### 📄 Código Completo

```jsx
import { useState, useRef, useMemo } from 'react'
import { searchMovies } from '../services/movies'

export function useMovies({ search, sort }) {
  const [movies, setMovies] = useState([])
  const [loading, setLoading] = useState(false)
  const [error, setError] = useState(null)
  const previousSearch = useRef(search)

  const getMovies = useMemo(() => {
    return async ({ search }) => {
      if (search === previousSearch.current) return

      try {

        setLoading(true)
        setError(null)
        previousSearch.current = search
        const newMovies = await searchMovies({ search })
        setMovies(newMovies)

      } catch (e) {

        setError(e.message)

      } finally {

        setLoading(false)

      }

    }
  }, [])


  const sortedMovies = useMemo(() => {
    return sort
      ? [...movies].sort((a, b) => a.title.localeCompare(b.title))
      : movies
  }, [sort, movies])

  return { movies: sortedMovies, getMovies, loading, error }

}
```

### 🔍 Análisis Detallado

#### **Línea 4:** Parámetros del Hook

```jsx
export function useMovies({ search, sort }) {
```

**¿Qué recibe?**
- `search`: El término de búsqueda actual
- `sort`: Si debe ordenar alfabéticamente (true/false)

**Destructuring de parámetros:**
```jsx
// Esto:
function useMovies({ search, sort }) {

// Es lo mismo que:
function useMovies(params) {
  const search = params.search
  const sort = params.sort
}
```

---

#### **Líneas 5-7:** Estados del Hook

```jsx
const [movies, setMovies] = useState([])
const [loading, setLoading] = useState(false)
const [error, setError] = useState(null)
```

**Tres estados independientes:**

1. **`movies`**: Array de películas (inicial: array vacío `[]`)
2. **`loading`**: Si está cargando (inicial: `false`)
3. **`error`**: Mensaje de error (inicial: `null`)

**Estados posibles:**
```jsx
// Inicial
movies = []
loading = false
error = null

// Buscando
movies = []
loading = true
error = null

// Con resultados
movies = [{...}, {...}, {...}]
loading = false
error = null

// Con error
movies = []
loading = false
error = 'Error searching movies'
```

---

#### **Línea 8:** `const previousSearch = useRef(search)`

**¿Para qué sirve?**
- Guarda la búsqueda anterior
- Evita hacer la misma búsqueda dos veces seguidas
- No causa re-render cuando cambia

**Ejemplo:**
```jsx
// Primera búsqueda
previousSearch.current = ''
search = 'Avengers'
// → Hace la búsqueda
previousSearch.current = 'Avengers'

// Usuario hace clic en "Buscar" otra vez sin cambiar el texto
search = 'Avengers'
previousSearch.current = 'Avengers'
// → NO hace la búsqueda (son iguales)
```

---

#### **Líneas 10-33:** `getMovies` con `useMemo`

```jsx
const getMovies = useMemo(() => {
  return async ({ search }) => {
    // ... código
  }
}, [])
```

**¿Qué es `useMemo`?**
- Memoriza (cachea) un valor calculado
- Solo lo recalcula cuando las dependencias cambian
- `[]` = array vacío = nunca recalcula

**¿Por qué usar `useMemo` aquí?**
- Para mantener la misma referencia de la función `getMovies`
- Evita que se cree una nueva función en cada render
- Similar a `useCallback`, pero devuelve el resultado de la función

**Equivalencia:**
```jsx
// Con useMemo
const getMovies = useMemo(() => {
  return async ({ search }) => { ... }
}, [])

// Con useCallback (equivalente)
const getMovies = useCallback(
  async ({ search }) => { ... }
, [])
```

---

#### **Línea 11:** Función Asíncrona

```jsx
return async ({ search }) => {
```

**¿Qué es `async`?**
- Marca la función como **asíncrona**
- Permite usar `await` dentro de ella
- Siempre devuelve una Promise

**¿Por qué asíncrona?**
- Porque hace una llamada a una API (operación que toma tiempo)
- Necesitamos esperar la respuesta con `await`

---

#### **Línea 12:** Prevenir Búsquedas Duplicadas

```jsx
if (search === previousSearch.current) return
```

**¿Qué hace?**
- Si la búsqueda es igual a la anterior, no hace nada
- Evita llamadas innecesarias a la API

**Ejemplo:**
```jsx
// Primera vez
search = 'Avengers'
previousSearch.current = ''
'Avengers' === '' // false → Continúa

// Usuario hace clic en "Buscar" de nuevo
search = 'Avengers'
previousSearch.current = 'Avengers'
'Avengers' === 'Avengers' // true → return (no hace nada)
```

---

#### **Bloque try-catch-finally**

```jsx
try {
  // Código que puede fallar
} catch (e) {
  // Manejo del error
} finally {
  // Código que siempre se ejecuta
}
```

**Estructura:**
1. **`try`**: Intenta ejecutar el código
2. **`catch`**: Si hay un error, lo captura
3. **`finally`**: Se ejecuta siempre (haya o no error)

---

#### **Líneas 16-20:** Bloque try

```jsx
setLoading(true)
setError(null)
previousSearch.current = search
const newMovies = await searchMovies({ search })
setMovies(newMovies)
```

**Paso a paso:**

**1. `setLoading(true)`**
```jsx
loading = true  // Muestra "Cargando..." en la UI
```

**2. `setError(null)`**
```jsx
error = null  // Limpia cualquier error anterior
```

**3. `previousSearch.current = search`**
```jsx
previousSearch.current = 'Avengers'  // Guarda la búsqueda actual
```

**4. `const newMovies = await searchMovies({ search })`**

**¿Qué es `await`?**
- Pausa la ejecución hasta que la Promise se resuelva
- Solo funciona con funciones `async`
- Obtiene el resultado de la Promise

**Sin await:**
```jsx
const newMovies = searchMovies({ search })
// newMovies = Promise { <pending> } ❌
```

**Con await:**
```jsx
const newMovies = await searchMovies({ search })
// newMovies = [{...}, {...}, {...}] ✅
```

**5. `setMovies(newMovies)`**
```jsx
movies = newMovies  // Actualiza el estado con las películas
```

---

#### **Líneas 22-24:** Bloque catch

```jsx
catch (e) {
  setError(e.message)
}
```

**¿Qué hace?**
- Si hay un error en el `try`, se captura aquí
- `e` es el objeto de error
- `e.message` es el mensaje del error
- Actualiza el estado `error`

**Ejemplo:**
```jsx
// Si la API falla:
Error: Error searching movies
→ catch (e)
→ setError('Error searching movies')
→ error = 'Error searching movies'
```

---

#### **Líneas 26-28:** Bloque finally

```jsx
finally {
  setLoading(false)
}
```

**¿Qué hace?**
- **Siempre** se ejecuta (con o sin error)
- Establece `loading = false`
- Oculta el mensaje "Cargando..."

**Flujos:**

**Caso exitoso:**
```jsx
try {
  setLoading(true)           // loading = true
  // ... búsqueda exitosa
  setMovies(newMovies)       // movies = [...]
} finally {
  setLoading(false)          // loading = false
}
```

**Caso con error:**
```jsx
try {
  setLoading(true)           // loading = true
  // ... error en la API
} catch (e) {
  setError(e.message)        // error = 'Error...'
} finally {
  setLoading(false)          // loading = false (se ejecuta igual)
}
```

---

#### **Líneas 36-40:** `sortedMovies` con `useMemo`

```jsx
const sortedMovies = useMemo(() => {
  return sort
    ? [...movies].sort((a, b) => a.title.localeCompare(b.title))
    : movies
}, [sort, movies])
```

**¿Qué hace este `useMemo`?**
- Memoriza el array de películas ordenadas
- Solo recalcula cuando `sort` o `movies` cambian
- Evita ordenar en cada render innecesariamente

**Sin `useMemo` (problema):**
```jsx
// Se ordena en CADA render (incluso si no cambió nada)
const sortedMovies = sort
  ? [...movies].sort((a, b) => a.title.localeCompare(b.title))
  : movies
```

**Con `useMemo` (optimizado):**
```jsx
// Solo se ordena cuando sort o movies cambian
const sortedMovies = useMemo(() => {
  return sort ? [...movies].sort(...) : movies
}, [sort, movies])
```

---

#### **El Operador Ternario**

```jsx
sort ? [...movies].sort(...) : movies
```

**Lógica:**
```jsx
// Si sort = true → Ordena
sort = true
→ [...movies].sort((a, b) => a.title.localeCompare(b.title))

// Si sort = false → No ordena
sort = false
→ movies
```

---

#### **El Spread Operator `...`**

```jsx
[...movies]
```

**¿Qué hace?**
- Crea una **copia** del array
- No modifica el array original

**¿Por qué es importante?**
- El método `.sort()` modifica el array original
- React necesita que los estados sean inmutables
- Copiamos el array antes de ordenarlo

**Ejemplo:**
```jsx
// ❌ INCORRECTO - Modifica el estado directamente
movies.sort(...)

// ✅ CORRECTO - Crea una copia y ordena la copia
[...movies].sort(...)
```

**Sin spread:**
```jsx
const movies = [{ title: 'B' }, { title: 'A' }]
movies.sort()
// movies se modifica directamente ❌
```

**Con spread:**
```jsx
const movies = [{ title: 'B' }, { title: 'A' }]
const sorted = [...movies].sort()
// movies no cambia ✅
// sorted es un nuevo array ordenado
```

---

#### **El Método `.sort()` y `localeCompare`**

```jsx
.sort((a, b) => a.title.localeCompare(b.title))
```

**¿Qué es `.sort()`?**
- Método de arrays que ordena los elementos
- Recibe una función comparadora

**¿Qué es `localeCompare`?**
- Compara dos strings alfabéticamente
- Respeta el idioma local (español, inglés, etc.)
- Devuelve:
  - Número negativo si `a` va antes que `b`
  - 0 si son iguales
  - Número positivo si `a` va después de `b`

**Ejemplo:**
```jsx
'Avengers'.localeCompare('Batman')
// → -1 (Avengers va primero)

'Batman'.localeCompare('Avengers')
// → 1 (Batman va después)

'Matrix'.localeCompare('Matrix')
// → 0 (son iguales)
```

**Ordenamiento completo:**
```jsx
const movies = [
  { title: 'Matrix' },
  { title: 'Avengers' },
  { title: 'Batman' }
]

const sorted = [...movies].sort((a, b) => 
  a.title.localeCompare(b.title)
)

// Resultado:
// [
//   { title: 'Avengers' },
//   { title: 'Batman' },
//   { title: 'Matrix' }
// ]
```

---

#### **Línea 42:** Return del Hook

```jsx
return { movies: sortedMovies, getMovies, loading, error }
```

**¿Qué devuelve?**
Un objeto con:
- `movies`: Las películas (ordenadas o no)
- `getMovies`: Función para buscar películas
- `loading`: Estado de carga
- `error`: Mensaje de error

**Uso en App.jsx:**
```jsx
const { movies: mappedMuvies, getMovies, loading } = useMovies({ search, sort })

// mappedMuvies → sortedMovies
// getMovies → función para buscar
// loading → true/false
```

---

## Components: Movies

Este componente se encarga de mostrar las películas o un mensaje si no hay resultados.

### 📄 Código Completo

```jsx
function ListOfMovies({ movies }) {
  return (
    <ul className='movies'>
      {
        movies.map(movie => (
          <li className='movie' key={movie.id}>
            <h3>{movie.title}</h3>
            <p>{movie.year}</p>
            <img src={movie.poster} alt={movie.title} />
          </li>
        ))
      }
    </ul>
  )
}

function NoMoviesResults() {
  return (
    <p>No se encontraron películas relacionadas</p>
  )
}

export function Movies({ movies }) {
  const hasMovies = movies.length > 0

  return (
    hasMovies
      ? <ListOfMovies movies={movies} />
      : <NoMoviesResults />
  )
}
```

### 🔍 Análisis Detallado

#### **Componente `ListOfMovies`**

```jsx
function ListOfMovies({ movies }) {
  return (
    <ul className='movies'>
      {
        movies.map(movie => (
          <li className='movie' key={movie.id}>
            <h3>{movie.title}</h3>
            <p>{movie.year}</p>
            <img src={movie.poster} alt={movie.title} />
          </li>
        ))
      }
    </ul>
  )
}
```

**¿Qué hace?**
- Recibe un array de películas como prop
- Renderiza una lista (`<ul>`) con todas las películas

---

#### **El Método `.map()`**

```jsx
movies.map(movie => ( ... ))
```

**¿Qué es `.map()`?**
- Método de arrays que transforma cada elemento
- Devuelve un nuevo array
- No modifica el array original

**En React:**
- Se usa para convertir datos en elementos JSX
- Cada elemento debe tener una `key` única

**Ejemplo:**
```jsx
const movies = [
  { id: '1', title: 'Avengers', year: '2012', poster: '...' },
  { id: '2', title: 'Matrix', year: '1999', poster: '...' }
]

movies.map(movie => (
  <li key={movie.id}>
    <h3>{movie.title}</h3>
    <p>{movie.year}</p>
    <img src={movie.poster} alt={movie.title} />
  </li>
))

// Resultado (array de elementos JSX):
[
  <li key="1"><h3>Avengers</h3><p>2012</p><img .../></li>,
  <li key="2"><h3>Matrix</h3><p>1999</p><img .../></li>
]
```

---

#### **La Prop `key`**

```jsx
<li className='movie' key={movie.id}>
```

**¿Qué es `key`?**
- Una prop especial que React usa para identificar elementos
- Debe ser única entre hermanos
- Ayuda a React a optimizar el renderizado

**¿Por qué es importante?**
Sin `key`, React no sabe qué elementos cambiaron:

```jsx
// Sin key ❌
<li><h3>Avengers</h3></li>
<li><h3>Matrix</h3></li>

// React no puede distinguirlos eficientemente

// Con key ✅
<li key="1"><h3>Avengers</h3></li>
<li key="2"><h3>Matrix</h3></li>

// React sabe exactamente qué elemento es cada uno
```

**Ejemplo de optimización:**
```jsx
// Estado inicial
<li key="1">Avengers</li>
<li key="2">Matrix</li>

// Nuevo estado (se agregó Batman al principio)
<li key="3">Batman</li>
<li key="1">Avengers</li>
<li key="2">Matrix</li>

// Con key, React sabe que:
// - key="3" es NUEVO → crea el elemento
// - key="1" y key="2" ya existen → no los re-crea
```

---

#### **Componente `NoMoviesResults`**

```jsx
function NoMoviesResults() {
  return (
    <p>No se encontraron películas relacionadas</p>
  )
}
```

**¿Qué hace?**
- Componente simple que muestra un mensaje
- Se usa cuando no hay resultados

---

#### **Componente `Movies` (Principal)**

```jsx
export function Movies({ movies }) {
  const hasMovies = movies.length > 0

  return (
    hasMovies
      ? <ListOfMovies movies={movies} />
      : <NoMoviesResults />
  )
}
```

**¿Qué hace?**
- Decide qué mostrar basándose en si hay películas
- Usa renderizado condicional

**Lógica:**
```jsx
// Si hay películas
hasMovies = true
→ <ListOfMovies movies={movies} />

// Si no hay películas
hasMovies = false
→ <NoMoviesResults />
```

**Ejemplo paso a paso:**
```jsx
// Caso 1: Sin películas
movies = []
hasMovies = [].length > 0  // false
→ Muestra <NoMoviesResults />

// Caso 2: Con películas
movies = [{...}, {...}, {...}]
hasMovies = 3 > 0  // true
→ Muestra <ListOfMovies movies={movies} />
```

---

## Services: movies.js

Este archivo contiene la lógica para llamar a la API de películas.

### 📄 Código Completo

```jsx
const API_KEY = '4287ad07'

export const searchMovies = async ({ search }) => {
  if (search === '') return null
  if (search.length < 3) return null
  try {
    const response = await fetch(`https://omdbapi.com/?apikey=${API_KEY}&s=${search}`)
    const json = await response.json()

    const movies = json.Search

    return movies.map(movie => ({
      id: movie.imdbID,
      title: movie.Title,
      year: movie.Year,
      poster: movie.Poster
    }))

  } catch (e) {
    throw new Error('Error searching movies', e)
  }

}
```

### 🔍 Análisis Detallado

#### **Línea 1:** API Key

```jsx
const API_KEY = '4287ad07'
```

**¿Qué es?**
- Clave de acceso a la API de OMDB
- Necesaria para hacer peticiones a la API
- En producción debería estar en variables de entorno

---

#### **Línea 3:** Función `searchMovies`

```jsx
export const searchMovies = async ({ search }) => {
```

**Características:**
- `export`: Se puede importar desde otros archivos
- `async`: Función asíncrona (permite `await`)
- `{ search }`: Recibe un objeto con la propiedad `search`

---

#### **Líneas 4-5:** Validaciones Tempranas

```jsx
if (search === '') return null
if (search.length < 3) return null
```

**¿Qué hacen?**
- Evitan hacer la petición si la búsqueda es inválida
- `return null`: Devuelven `null` inmediatamente

**Ejemplo:**
```jsx
await searchMovies({ search: '' })
// → return null (sin llamar a la API)

await searchMovies({ search: 'Av' })
// → return null (sin llamar a la API)

await searchMovies({ search: 'Avengers' })
// → Continúa con la petición
```

---

#### **Línea 7:** Fetch a la API

```jsx
const response = await fetch(`https://omdbapi.com/?apikey=${API_KEY}&s=${search}`)
```

**¿Qué es `fetch`?**
- Función del navegador para hacer peticiones HTTP
- Devuelve una Promise
- Se usa con `await` para esperar la respuesta

**Template Strings (backticks):**
```jsx
`https://omdbapi.com/?apikey=${API_KEY}&s=${search}`
```

**Interpolación:**
```jsx
// Si:
API_KEY = '4287ad07'
search = 'Avengers'

// Resultado:
'https://omdbapi.com/?apikey=4287ad07&s=Avengers'
```

**¿Qué devuelve `fetch`?**
```jsx
const response = await fetch(url)
// response = Response {
//   ok: true,
//   status: 200,
//   json: Function,
//   ...
// }
```

---

#### **Línea 8:** Parsear la Respuesta

```jsx
const json = await response.json()
```

**¿Qué hace `.json()`?**
- Convierte la respuesta de texto a objeto JavaScript
- También es asíncrono (necesita `await`)

**Ejemplo:**
```jsx
// response.json() devuelve:
{
  "Search": [
    {
      "Title": "Avengers: Endgame",
      "Year": "2019",
      "imdbID": "tt4154796",
      "Poster": "https://..."
    },
    ...
  ],
  "totalResults": "100",
  "Response": "True"
}
```

---

#### **Línea 10:** Extraer las Películas

```jsx
const movies = json.Search
```

**¿Qué hace?**
- Accede a la propiedad `Search` del JSON
- `Search` es un array de películas

**Ejemplo:**
```jsx
const json = {
  Search: [
    { Title: "Avengers", ... },
    { Title: "Avengers: Endgame", ... }
  ]
}

const movies = json.Search
// movies = [{ Title: "Avengers", ... }, ...]
```

---

#### **Líneas 12-16:** Mapear y Transformar los Datos

```jsx
return movies.map(movie => ({
  id: movie.imdbID,
  title: movie.Title,
  year: movie.Year,
  poster: movie.Poster
}))
```

**¿Qué hace?**
- Transforma cada película de la API al formato que necesitamos
- Usa `.map()` para crear un nuevo array
- Cambia los nombres de las propiedades

**Transformación:**

**Antes (datos de la API):**
```jsx
{
  "Title": "Avengers: Endgame",
  "Year": "2019",
  "imdbID": "tt4154796",
  "Poster": "https://..."
}
```

**Después (nuestro formato):**
```jsx
{
  id: "tt4154796",
  title: "Avengers: Endgame",
  year: "2019",
  poster: "https://..."
}
```

**¿Por qué transformar?**
- Los nombres de la API (Title, Year) son inconsistentes
- Preferimos nombres en minúscula y en español
- Simplifica el uso en componentes

**Sintaxis de retorno implícito:**
```jsx
// Con paréntesis → retorno implícito
movies.map(movie => ({
  id: movie.imdbID
}))

// Sin paréntesis → necesitas return explícito
movies.map(movie => {
  return {
    id: movie.imdbID
  }
})
```

---

#### **Líneas 18-20:** Manejo de Errores

```jsx
catch (e) {
  throw new Error('Error searching movies', e)
}
```

**¿Qué hace?**
- Si hay un error en el `try`, se captura aquí
- `throw new Error()`: Lanza un nuevo error
- El error se propaga al código que llamó a `searchMovies`

**Flujo:**
```jsx
// En useMovies.js:
try {
  const newMovies = await searchMovies({ search })
} catch (e) {
  // El error de movies.js llega aquí
  setError(e.message)  // 'Error searching movies'
}
```

---

## Hooks Utilizados en el Proyecto

### Resumen de Hooks

| Hook | Dónde se usa | Para qué |
|------|--------------|----------|
| `useState` | App.jsx, useSearch, useMovies | Manejar estado que cambia con el tiempo |
| `useEffect` | useSearch | Validar la búsqueda cuando cambia |
| `useRef` | useSearch, useMovies | Guardar valores sin causar re-render |
| `useMemo` | useMovies | Optimizar cálculos costosos (ordenamiento) |

---

### useState - Explicación Profunda

**¿Qué es el Estado?**

El estado es información que:
1. Puede cambiar con el tiempo
2. Afecta lo que se muestra en pantalla
3. Causa re-renders cuando cambia

**Uso en el proyecto:**

```jsx
// App.jsx
const [sort, setSort] = useState(false)

// useSearch
const [search, updateSearch] = useState('')
const [error, setError] = useState(null)

// useMovies
const [movies, setMovies] = useState([])
const [loading, setLoading] = useState(false)
const [error, setError] = useState(null)
```

**Reglas de useState:**
1. Solo se llama en el nivel superior (no en loops o ifs)
2. El estado es inmutable (no modificar directamente)
3. Actualizar el estado causa un re-render

---

### useEffect - Explicación Profunda

**¿Qué son los Efectos?**

Efectos son operaciones que:
1. Interactúan con el mundo exterior
2. Se ejecutan después del render
3. Pueden necesitar limpieza

**Uso en el proyecto:**

```jsx
// useSearch
useEffect(() => {
  // Validar search cada vez que cambia
  if (search === '') {
    setError('...')
  }
}, [search])  // Se ejecuta cuando search cambia
```

**Los 3 casos de useEffect:**

```jsx
// 1. Sin dependencias → Se ejecuta en cada render
useEffect(() => {
  console.log('Cada render')
})

// 2. Array vacío → Se ejecuta una sola vez
useEffect(() => {
  console.log('Solo al montar')
}, [])

// 3. Con dependencias → Se ejecuta cuando cambian
useEffect(() => {
  console.log('Cuando search cambia')
}, [search])
```

---

### useRef - Explicación Profunda

**¿Qué es una Ref?**

Una ref es:
1. Un valor mutable
2. Persiste entre renders
3. NO causa re-render cuando cambia

**Uso en el proyecto:**

```jsx
// useSearch
const isFirstInput = useRef(true)

// useMovies
const previousSearch = useRef(search)
```

**Diferencia con useState:**

```jsx
// useState → Causa re-render
const [count, setCount] = useState(0)
setCount(1)  // Re-render ✅

// useRef → NO causa re-render
const count = useRef(0)
count.current = 1  // No re-render ❌
```

**¿Cuándo usar cada uno?**

```jsx
// useState → Si afecta la UI
const [search, setSearch] = useState('')
// Cambiar search → Actualizar el input

// useRef → Si NO afecta la UI
const isFirstInput = useRef(true)
// Cambiar isFirstInput → No afecta la UI directamente
```

---

### useMemo - Explicación Profunda

**¿Qué es la Memorización?**

Memorizar es:
1. Guardar el resultado de un cálculo
2. Reutilizarlo si las dependencias no cambiaron
3. Optimización de rendimiento

**Uso en el proyecto:**

```jsx
// useMovies - Memorizar la función getMovies
const getMovies = useMemo(() => {
  return async ({ search }) => { ... }
}, [])

// useMovies - Memorizar el array ordenado
const sortedMovies = useMemo(() => {
  return sort
    ? [...movies].sort((a, b) => a.title.localeCompare(b.title))
    : movies
}, [sort, movies])
```

**Sin useMemo:**

```jsx
// Se ordena en CADA render (ineficiente)
function useMovies({ search, sort }) {
  const [movies, setMovies] = useState([])
  
  // Se crea una nueva función en cada render
  const getMovies = async ({ search }) => { ... }
  
  // Se ordena en cada render (aunque movies y sort no cambien)
  const sortedMovies = sort 
    ? [...movies].sort((a, b) => a.title.localeCompare(b.title))
    : movies
    
  return { movies: sortedMovies, getMovies }
}
```

**Con useMemo:**

```jsx
// Solo se recalcula cuando las dependencias cambian
function useMovies({ search, sort }) {
  const [movies, setMovies] = useState([])
  
  // La función se crea una sola vez
  const getMovies = useMemo(() => {
    return async ({ search }) => { ... }
  }, [])
  
  // Solo se ordena cuando sort o movies cambian
  const sortedMovies = useMemo(() => {
    return sort 
      ? [...movies].sort((a, b) => a.title.localeCompare(b.title))
      : movies
  }, [sort, movies])
    
  return { movies: sortedMovies, getMovies }
}
```

**Beneficios:**
1. Evita cálculos costosos innecesarios
2. Mantiene referencias estables
3. Mejora el rendimiento

---

## Flujo de Datos Completo

### Flujo: Usuario Busca una Película

```
1. Usuario escribe "Avengers" en el input
   ↓
2. Se ejecuta handleChange(e)
   ↓
3. updateSearch('Avengers')
   ↓
4. search = 'Avengers' (useState)
   ↓
5. React re-renderiza App
   ↓
6. useEffect en useSearch() se ejecuta
   ↓
7. Valida 'Avengers' → Sin errores
   ↓
8. Usuario hace clic en "Buscar"
   ↓
9. Se ejecuta handleSubmit(e)
   ↓
10. e.preventDefault() (evita recargar)
   ↓
11. getMovies({ search: 'Avengers' })
   ↓
12. setLoading(true) → loading = true
   ↓
13. React re-renderiza → Muestra "Cargando..."
   ↓
14. await searchMovies({ search: 'Avengers' })
   ↓
15. fetch a la API de OMDB
   ↓
16. API responde con películas
   ↓
17. Mapea y transforma los datos
   ↓
18. setMovies(newMovies) → movies = [...]
   ↓
19. setLoading(false) → loading = false
   ↓
20. React re-renderiza
   ↓
21. sortedMovies se calcula (useMemo)
   ↓
22. <Movies movies={sortedMovies} />
   ↓
23. movies.map() renderiza cada película
   ↓
24. Usuario ve las películas en pantalla ✅
```

---

### Flujo: Usuario Ordena las Películas

```
1. Usuario hace clic en el checkbox
   ↓
2. Se ejecuta handleSort()
   ↓
3. setSort(!sort) → sort = true
   ↓
4. React re-renderiza App
   ↓
5. useMovies recibe sort = true
   ↓
6. useMemo detecta que sort cambió
   ↓
7. Ejecuta: [...movies].sort((a, b) => a.title.localeCompare(b.title))
   ↓
8. sortedMovies = películas ordenadas alfabéticamente
   ↓
9. <Movies movies={sortedMovies} />
   ↓
10. Se muestran las películas ordenadas ✅
```

---

## Conceptos Clave Aplicados

### 1. Componentes Funcionales

Todos los componentes son funciones:

```jsx
function App() { ... }
function Movies({ movies }) { ... }
function ListOfMovies({ movies }) { ... }
```

---

### 2. Props (Propiedades)

Datos que se pasan de padre a hijo:

```jsx
// Padre
<Movies movies={mappedMuvies} />

// Hijo
function Movies({ movies }) {
  // movies es la prop
}
```

---

### 3. Estado (State)

Información que cambia con el tiempo:

```jsx
const [search, updateSearch] = useState('')
// search cambia → Componente se re-renderiza
```

---

### 4. Custom Hooks

Funciones que encapsulan lógica reutilizable:

```jsx
function useSearch() {
  // Lógica de validación de búsqueda
  return { search, updateSearch, error }
}

function useMovies({ search, sort }) {
  // Lógica de obtención y ordenamiento
  return { movies, getMovies, loading, error }
}
```

---

### 5. Renderizado Condicional

Mostrar diferentes cosas según condiciones:

```jsx
// Con operador ternario
{loading ? <p>Cargando...</p> : <Movies movies={mappedMuvies} />}

// Con operador &&
{error && <p style={{ color: 'red' }}>{error}</p>}

// Con función
{hasMovies ? <ListOfMovies /> : <NoMoviesResults />}
```

---

### 6. Listas y Keys

Renderizar arrays con `.map()`:

```jsx
movies.map(movie => (
  <li key={movie.id}>
    <h3>{movie.title}</h3>
  </li>
))
```

---

### 7. Funciones Asíncronas

Trabajar con operaciones que toman tiempo:

```jsx
const getMovies = async ({ search }) => {
  const response = await fetch(url)
  const json = await response.json()
  return json.Search
}
```

---

### 8. Manejo de Errores

Capturar y manejar errores:

```jsx
try {
  const movies = await searchMovies({ search })
} catch (e) {
  setError(e.message)
}
```

---

### 9. Optimización

Evitar cálculos innecesarios:

```jsx
// useMemo para ordenamiento
const sortedMovies = useMemo(() => {
  return sort ? [...movies].sort(...) : movies
}, [sort, movies])

// useRef para prevenir búsquedas duplicadas
if (search === previousSearch.current) return
```

---

### 10. Separación de Responsabilidades

Cada archivo tiene una responsabilidad:

- **main.jsx**: Punto de entrada
- **App.jsx**: Lógica principal y UI
- **Movies.jsx**: Presentación de películas
- **useMovies.js**: Lógica de películas
- **movies.js**: Llamadas a la API

---

## 🎯 Resumen del Proyecto

### Tecnologías y Conceptos

- ✅ React 19
- ✅ Hooks (useState, useEffect, useRef, useMemo)
- ✅ Custom Hooks (useSearch, useMovies)
- ✅ Componentes Funcionales
- ✅ Renderizado Condicional
- ✅ Manejo de Estado
- ✅ Llamadas a APIs
- ✅ Async/Await
- ✅ Manejo de Errores
- ✅ Optimización con useMemo

---

### Flujo General de la Aplicación

1. **Usuario escribe en el input** → Se valida en tiempo real
2. **Usuario hace clic en "Buscar"** → Se llama a la API
3. **API responde** → Se transforman y guardan los datos
4. **Películas se muestran** → Renderizadas con `.map()`
5. **Usuario puede ordenar** → Se reordenan con `useMemo`

---

## 📚 Para Estudiar Más

### Orden Recomendado

1. **Conceptos básicos:** useState, props, componentes
2. **Efectos:** useEffect y su array de dependencias
3. **Referencias:** useRef y cuándo usarlo
4. **Optimización:** useMemo y cuándo optimizar
5. **Custom Hooks:** Cómo extraer lógica reutilizable
6. **Async/Await:** Trabajar con APIs
7. **Manejo de errores:** try-catch-finally

---

### Ejercicios Sugeridos

1. Agrega un botón para limpiar la búsqueda
2. Implementa paginación de resultados
3. Agrega un filtro por año
4. Guarda las búsquedas recientes en localStorage
5. Implementa modo oscuro/claro

---

¡Éxito en tu aprendizaje! 🚀
