---
Developer:
Language: Markdown
---
# Encabezados
# H1 — Encabezado principal
## H2 — Encabezado secundario
### H3 — Subtítulo
#### H4 — Nivel 4
##### H5 — Nivel 5
###### H6 — Nivel 6

---

# Énfasis en el texto
**Negrita**  
*Cursiva*  
***Negrita + Cursiva***  
~~Tachado~~  
<sub>Subíndice</sub>  
<sup>Superíndice</sup>  

---

# Listas
## Viñetas
- Elemento 1  
- Elemento 2  
  - Sub-elemento  
    - Sub-sub-elemento  

## Numeradas
1. Primer ítem  
2. Segundo ítem  
3. Tercer ítem  

## Tareas
- [x] Hecho  
- [ ] Pendiente  
- [ ] Otro pendiente  

---

# Citas
> Esto es una cita simple  
>> Cita anidada  
>>> Otro nivel

---

# Enlaces
[Texto del enlace](https://ejemplo.com)  
[Enlace con título](https://ejemplo.com "Título opcional")  
<https://ejemplo.com> (autolink)

---

# Imágenes
![Texto alternativo](https://via.placeholder.com/150)  
![Imagen con título](https://via.placeholder.com/150 "Título de la imagen")

---

# Código
## En línea
Texto con `código en línea`

## Bloques
\`\`\`python
def saludar():
    print("Hola Mundo")
\`\`\`

---

# Tablas
| Columna 1 | Columna 2 | Columna 3 |
|-----------|-----------|-----------|
| Fila 1    | Dato 1    | Dato 2    |
| Fila 2    | Dato 3    | Dato 4    |
| Fila 3    | Dato 5    | Dato 6    |

---

# Separadores
---  
***  
___  

---

# Notas y llamadas de atención (no estándar, dependen del render)
> **NOTE:** Esto es una nota  
> **TIP:** Esto es un tip o consejo  
> **IMPORTANT:** Esto es importante  
> **WARNING:** Esto es una advertencia  
> **CAUTION:** Ten cuidado  

---

# Detalles desplegables
<details>
  <summary>Haz clic para desplegar</summary>
  Aquí va el contenido oculto.
</details>

---

# HTML permitido (según el render)
<p style="color:red">Texto rojo usando HTML</p>
<b>Texto en negrita con HTML</b>
<i>Texto en cursiva con HTML</i>
<u>Texto subrayado con HTML</u>

---

# Diagramas y extras (con extensiones)
## Mermaid
\`\`\`mermaid
graph TD
  A[Inicio] --> B[Proceso]
  B --> C[Fin]
\`\`\`

## Matemáticas (LaTeX)
Ecuación en línea: $E = mc^2$  
Bloque:
$$
\int_0^\infty x^2 dx
$$

---
> [!NOTE]
> Useful information that users should know, even when skimming content.

> [!TIP]
> Helpful advice for doing things better or more easily.

> [!IMPORTANT]
> Key information users need to know to achieve their goal.

> [!WARNING]
> Urgent info that needs immediate user attention to avoid problems.

> [!CAUTION]
> Advises about risks or negative outcomes of certain actions.


