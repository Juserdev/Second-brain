---
Developer: Midudev
Language: CSS
Date: 12-04-2026
---
La propiedad outline reemplaza al border en un sentido concreto y es que el border ocupa un espacio en el contenedor mientras que outline no, ejemplo de uso:

`Outline: 1px solid black`

---
El **outline** (contorno) es una línea que se dibuja alrededor de los elementos, por fuera del borde. Aunque se parecen mucho, tienen propósitos y comportamientos distintos.
Aquí tienes el resumen clave:
### ¿Qué es el Outline?
Es una propiedad que dibuja una línea rodeando el elemento para hacerlo resaltar. Se usa mucho para la **accesibilidad** (cuando seleccionas un botón con el teclado, ese recuadro que aparece suele ser un outline).
### Diferencias principales: Outline vs. Border
| Característica | **Border** (Borde)                                          | **Outline** (Contorno)                                                                |
| -------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| **Espacio**    | Ocupa espacio en el diseño (afecta el tamaño del elemento). | **No ocupa espacio**. Es como una "capa" encima; no mueve a los vecinos.              |
| **Lados**      | Puedes definir cada lado por separado (border-left, etc.).  | Es **siempre alrededor** de todo el elemento. No hay outline-left.                    |
| **Forma**      | Sigue el border-radius (curvas).                            | Tradicionalmente era rectangular, aunque hoy ya soporta curvas en muchos navegadores. |
| **Posición**   | Está pegado al contenido/padding.                           | Puedes separarlo del borde usando la propiedad outline-offset.                        |
### Ejemplo rápido de código
Si quieres ver la diferencia en acción, nota cómo el outline-offset despega la línea del cuadro:
```css
.caja {
  border: 2px solid black;   /* El borde físico */
  outline: 3px solid red;    /* El resaltado */
  outline-offset: 5px;       /* Crea un hueco entre el borde y el outline */
}
```
**Regla de oro:** Usa **border** para la estructura y estética básica del diseño, y usa **outline** para estados de enfoque (*focus*) o para resaltar algo sin que se mueva todo el layout de tu página.