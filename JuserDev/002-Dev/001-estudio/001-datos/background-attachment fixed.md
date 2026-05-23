---
Developer: personal
Language: CSS
Date: 13-04-2026
---
## ¿Qué hace `background-attachment: fixed;`?

Normalmente, cuando haces scroll en una página, la imagen de fondo se mueve hacia arriba junto con el contenido. Al usar `fixed`, le dices al navegador: **"Deja la imagen quieta en la pantalla, que solo se mueva el texto"**.

### En términos simples:

- **Sin fixed (scroll):** El fondo está "pegado" al contenido. Si el contenido sube, el fondo sube.
    
- **Con fixed:** El fondo está "pegado" a la ventana del navegador (el cristal de tu pantalla). El contenido pasa por encima como si fuera una ventana transparente.
    

---

### Ejemplo de Código

Para que funcione bien, suele usarse junto con otras propiedades de fondo:

CSS

```
.seccion-pro {
  background-image: url('tu-imagen.jpg');
  background-repeat: no-repeat;
  background-size: cover;      /* Para que cubra toda la pantalla */
  background-attachment: fixed; /* El truco de magia */
}
```

---

### Puntos clave para recordar:

- **Efecto Visual:** Crea una sensación de profundidad. Parece que el contenido flota sobre la imagen.
    
- **Rendimiento:** Ten cuidado, ya que a los navegadores les cuesta un poco más de trabajo procesar el scroll con imágenes fijas muy pesadas.
    
- **Móviles:** **Dato importante:** Muchos navegadores de móviles (como Safari en iPhone) a veces ignoran esta propiedad o dan problemas de visualización para ahorrar batería y procesador.
    

> **💡 Tip de pro:** Si ves que en móvil no se ve como quieres, puedes usar un _Media Query_ para cambiarlo a `background-attachment: scroll;` solo en pantallas pequeñas.

---

### Comparación rápida

| **Propiedad**          | **Comportamiento**                                                                                   |
| ---------------------- | ---------------------------------------------------------------------------------------------------- |
| `scroll` (por defecto) | El fondo se desplaza al ritmo del texto.                                                             |
| `fixed`                | El fondo se queda congelado en su sitio.                                                             |
| `local`                | El fondo se mueve dentro de un elemento que tiene su propio scroll (como un div con scroll interno). |