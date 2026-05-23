---
Developer:
Language: Django
---
# Tutorial Django - Aplicación de Gestión de Proyectos y Tareas

![logo django](https://static.djangoproject.com/img/logos/django-logo-negative.png)

## Tabla de Contenidos
1. [Configuración Inicial](#configuración-inicial)
2. [Creación del Proyecto](#creación-del-proyecto)
3. [Configuración de la Aplicación](#configuración-de-la-aplicación)
4. [Modelos de Datos](#modelos-de-datos)
5. [Panel de Administración](#panel-de-administración)
6. [Vistas y URLs](#vistas-y-urls)
7. [Templates](#templates)
8. [Formularios](#formularios)
9. [Funcionalidades Avanzadas](#funcionalidades-avanzadas)
10. [Comandos Útiles](#comandos-útiles)

---

## Configuración Inicial

### 1. Instalación de Virtualenv
Primero instalamos virtualenv para crear un entorno virtual aislado:

```bash
pip install virtualenv
```

### 2. Creación del Entorno Virtual
Creamos una máquina virtual para nuestro proyecto:

```bash
virtualenv venv
```

### 3. Activación del Entorno Virtual
Activamos el entorno virtual (en macOS/Linux):

```bash
source venv/bin/activate
```

### 4. Instalación de Django
Con el entorno virtual activado, instalamos Django:

```bash
pip install django
```

---

## Creación del Proyecto

### 5. Crear el Proyecto Django
Creamos un nuevo proyecto Django llamado `mysite`:

```bash
django-admin startproject mysite
```

### 6. Iniciar el Servidor de Desarrollo
Navegamos al directorio del proyecto y iniciamos el servidor:

```bash
cd mysite
python3 manage.py runserver
```

Para usar un puerto específico (ejemplo: puerto 3000):

```bash
python3 manage.py runserver 3000
```

### 7. Crear una Aplicación
Creamos una aplicación dentro del proyecto llamada `myapp`:

```bash
python3 manage.py startapp myapp
```

---

## Configuración de la Aplicación

### 8. Registrar la Aplicación
En `mysite/settings.py`, agregamos nuestra aplicación a `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'myapp'  # ← Agregamos nuestra aplicación
]
```

### 9. Configurar URLs Principales
En `mysite/urls.py`, incluimos las URLs de nuestra aplicación:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path("", include("myapp.urls"))  # Incluye las URLs de myapp
]
```

---

## Modelos de Datos

### 10. Crear Modelos
En `myapp/models.py`, definimos nuestros modelos de datos:

```python
from django.db import models

class Project(models.Model):
    name = models.CharField(max_length=100)

    def __str__(self):
        return self.name

class Task(models.Model):
    title = models.CharField(max_length=100)
    description = models.TextField(max_length=200)
    project = models.ForeignKey(Project, on_delete=models.CASCADE)
    done = models.BooleanField(default=False)

    def __str__(self):
        return f"{self.title} --> proyecto: {self.project.name}"
```

**Explicación de los modelos:**
- **Project**: Modelo simple con un campo `name`
- **Task**: Modelo con título, descripción, relación con Project y estado (done)
- **ForeignKey**: Relación de muchos a uno (muchas tareas pueden pertenecer a un proyecto)
- **on_delete=CASCADE**: Si se elimina un proyecto, se eliminan todas sus tareas

### 11. Crear y Aplicar Migraciones
Creamos las migraciones para nuestros modelos:

```bash
python3 manage.py makemigrations
```

Aplicamos las migraciones a la base de datos:

```bash
python3 manage.py migrate
```

---

## Panel de Administración

### 12. Crear Superusuario
Creamos un usuario administrador:

```bash
python3 manage.py createsuperuser
```

### 13. Registrar Modelos en el Admin
En `myapp/admin.py`, registramos nuestros modelos:

```python
from django.contrib import admin
from .models import Project, Task

# Configuración personalizada para el modelo Task
class TaskAdmin(admin.ModelAdmin):
    readonly_fields = ("created", )  # Campos de solo lectura

# Registramos los modelos
admin.site.register(Project)
admin.site.register(Task, TaskAdmin)
```

---

## Vistas y URLs

### 14. Crear Vistas
En `myapp/views.py`, definimos nuestras vistas:

```python
from django.shortcuts import render, get_object_or_404, redirect
from django.http import HttpResponse
from .models import Project, Task
from .forms import Create_new_project, Create_new_task

# Vista básica
def hello(request):
    return HttpResponse("<h2>Hello World</h2>")

# Vista con template
def index(request):
    title = "Welcome to Django Course!!"
    return render(request, "index.html", {
        "title": title
    })

# Vista con parámetros
def hello_params(request, username):
    return HttpResponse(f"<h2>Hello World {username}</h2>")

# Vistas para mostrar datos
def projects(request):
    projects = Project.objects.all()
    return render(request, "projects/projects.html", {
        "projects": projects
    })

def tasks(request):
    tasks = Task.objects.all()
    return render(request, "tasks/tasks.html", {
        "tasks": tasks
    })

def tasks_id(request, id):
    task = get_object_or_404(Task, id=id)
    return render(request, "tasks/task_detail.html", {
        "task": task
    })

# Vistas para crear datos
def create_project(request):
    if request.method == 'GET':
        return render(request, "projects/create_project.html", {
            "form": Create_new_project()
        })
    else:
        Project.objects.create(name=request.POST['name'])
        return redirect('projects')

def create_task(request):
    if request.method == 'GET':
        return render(request, "tasks/create_task.html", {
            "form": Create_new_task()
        })
    else:
        project = Project.objects.get(id=request.POST["project"])
        task = Task.objects.create(
            title=request.POST['title'],
            description=request.POST['description'],
            project=project
        )
        return redirect('tasks')
```

### 15. Configurar URLs de la Aplicación
En `myapp/urls.py`, definimos las rutas:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index),
    path('hello/', views.hello),
    path('hello/<str:username>', views.hello_params),
    path('hello_id/<int:id>', views.hello_params_id),
    path('projects/', views.projects),
    path('tasks/', views.tasks),
    path('tasks/<int:id>', views.tasks_id),
    path('create_task/', views.create_task),
    path('create_project/', views.create_project),
]
```

**Tipos de parámetros en URLs:**
- `<str:username>`: Parámetro de tipo string
- `<int:id>`: Parámetro de tipo entero

---

## Templates

### 16. Estructura de Templates
Creamos la carpeta `myapp/templates/` con la siguiente estructura:

```
templates/
├── index.html
├── about.html
├── layouts/
│   └── base.html
├── projects/
│   ├── projects.html
│   └── create_project.html
└── tasks/
    ├── tasks.html
    └── create_task.html
```

### 17. Template Base
En `templates/layouts/base.html`:

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}Django App{% endblock %}</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
        <div class="container">
            <a class="navbar-brand" href="/">Django App</a>
            <div class="navbar-nav">
                <a class="nav-link" href="/projects/">Proyectos</a>
                <a class="nav-link" href="/tasks/">Tareas</a>
            </div>
        </div>
    </nav>
    
    <div class="container mt-4">
        {% block content %}
        {% endblock %}
    </div>
</body>
</html>
```

### 18. Templates Específicos
Template para mostrar proyectos (`templates/projects/projects.html`):

```html
{% extends 'layouts/base.html' %}

{% block content %}
<div class="row">
    <div class="col-md-4">
        <h3>Proyectos</h3>
        <a href="/create_project/" class="btn btn-primary mb-3">Crear Proyecto</a>
        
        {% for project in projects %}
        <div class="card mb-2">
            <div class="card-body">
                <h5 class="card-title">{{ project.name }}</h5>
            </div>
        </div>
        {% endfor %}
    </div>
</div>
{% endblock %}
```

---

## Formularios

### 19. Crear Formularios
En `myapp/forms.py`, definimos nuestros formularios:

```python
from django import forms

class Create_new_task(forms.Form):
    title = forms.CharField(label="Título de tarea", max_length=200)
    description = forms.CharField(
        label="Descripción de tarea", 
        widget=forms.Textarea
    )

class Create_new_project(forms.Form):
    name = forms.CharField(label="Nombre del proyecto", max_length=200)
```

### 20. Templates de Formularios
Template para crear proyectos (`templates/projects/create_project.html`):

```html
{% extends 'layouts/base.html' %}

{% block content %}
<div class="row">
    <div class="col-md-4">
        <h3>Crear Proyecto</h3>
        
        <form method="POST">
            {% csrf_token %}
            {{ form.as_p }}
            <button type="submit" class="btn btn-primary">Guardar</button>
        </form>
        
        <a href="/projects/" class="btn btn-secondary mt-2">Volver</a>
    </div>
</div>
{% endblock %}
```

---

## Funcionalidades Avanzadas

### 21. Manejo de Formularios con Validación
Ejemplo de vista con validación:

```python
def create_task(request):
    if request.method == 'GET':
        return render(request, "tasks/create_task.html", {
            "form": Create_new_task(),
            "projects": Project.objects.all()
        })
    else:
        form = Create_new_task(request.POST)
        if form.is_valid():
            project = Project.objects.get(id=request.POST["project"])
            Task.objects.create(
                title=form.cleaned_data['title'],
                description=form.cleaned_data['description'],
                project=project
            )
            return redirect('tasks')
        else:
            return render(request, "tasks/create_task.html", {
                "form": form,
                "projects": Project.objects.all()
            })
```

### 22. Filtros y Búsquedas
Ejemplo de vista con filtros:

```python
def tasks_by_project(request, project_id):
    project = get_object_or_404(Project, id=project_id)
    tasks = Task.objects.filter(project=project)
    return render(request, "tasks/tasks.html", {
        "tasks": tasks,
        "project": project
    })
```

### 23. Actualizar Estado de Tareas
Vista para marcar tareas como completadas:

```python
def complete_task(request, task_id):
    task = get_object_or_404(Task, id=task_id)
    task.done = not task.done
    task.save()
    return redirect('tasks')
```

---

## Comandos Útiles

### Comandos de Desarrollo
```bash
# Activar entorno virtual
source venv/bin/activate

# Iniciar servidor
python3 manage.py runserver
python3 manage.py runserver 3000  # Puerto específico

# Migraciones
python3 manage.py makemigrations
python3 manage.py migrate

# Crear superusuario
python3 manage.py createsuperuser

# Shell interactivo de Django
python3 manage.py shell

# Recopilar archivos estáticos
python3 manage.py collectstatic
```

### Comandos de Base de Datos
```bash
# Ver SQL de las migraciones
python3 manage.py sqlmigrate myapp 0001

# Verificar problemas
python3 manage.py check

# Resetear migraciones (cuidado en producción)
python3 manage.py migrate myapp zero
```

---

## Estructura Final del Proyecto

```
Django/
├── venv/                    # Entorno virtual
├── db.sqlite3              # Base de datos SQLite
├── manage.py               # Script de gestión de Django
├── mysite/                 # Configuración del proyecto
│   ├── __init__.py
│   ├── settings.py         # Configuración principal
│   ├── urls.py            # URLs principales
│   ├── wsgi.py
│   └── asgi.py
└── myapp/                  # Aplicación principal
    ├── __init__.py
    ├── admin.py           # Configuración del admin
    ├── apps.py
    ├── models.py          # Modelos de datos
    ├── views.py           # Lógica de vistas
    ├── urls.py            # URLs de la aplicación
    ├── forms.py           # Formularios
    ├── tests.py           # Pruebas
    ├── migrations/        # Migraciones de BD
    └── templates/         # Templates HTML
        ├── index.html
        ├── about.html
        ├── layouts/
        ├── projects/
        └── tasks/
```

---

## Próximos Pasos

1. **Autenticación**: Implementar sistema de usuarios
2. **API REST**: Crear endpoints con Django REST Framework
3. **Archivos estáticos**: Configurar CSS, JS e imágenes
4. **Deployment**: Preparar para producción
5. **Testing**: Escribir pruebas unitarias
6. **Optimización**: Mejorar rendimiento con caché

---

## Recursos Adicionales

- [Documentación oficial de Django](https://docs.djangoproject.com/)
- [Django REST Framework](https://www.django-rest-framework.org/)
- [Bootstrap para estilos](https://getbootstrap.com/)

---

**¡Felicidades!** Has completado el tutorial básico de Django. Este proyecto incluye:

✅ Configuración completa del entorno  
✅ Modelos de datos con relaciones  
✅ Panel de administración  
✅ Vistas y templates  
✅ Formularios funcionales  
✅ Sistema de URLs  
✅ CRUD completo para proyectos y tareas  

¡Ahora puedes expandir la aplicación con nuevas funcionalidades!

---

# Explicación detallada por archivo y por función

Esta sección describe, en lenguaje claro, qué hace cada parte del proyecto. Úsala como referencia rápida.

## Archivos principales del proyecto

- `manage.py`
  - Script de utilidad de Django para ejecutar comandos de gestión: ejecutar el servidor (`runserver`), crear migraciones (`makemigrations`), aplicarlas (`migrate`), crear superusuario (`createsuperuser`), etc.
  - Recibe argumentos de línea de comandos y delega en el sistema de comandos de Django.

- `mysite/settings.py`
  - Configuración global del proyecto: apps instaladas (`INSTALLED_APPS`), base de datos (`DATABASES`), middlewares (`MIDDLEWARE`), plantillas (`TEMPLATES`), zona horaria, archivos estáticos, etc.
  - Puntos clave de este proyecto:
    - `INSTALLED_APPS` incluye `myapp` para que Django detecte modelos, vistas, templates y migraciones de tu app.
    - `TEMPLATES[0]['APP_DIRS'] = True` permite que Django busque templates dentro de `myapp/templates/`.
    - `DATABASES` usa SQLite por defecto con archivo `db.sqlite3` en la raíz del proyecto.

- `mysite/urls.py`
  - Enrutador principal del proyecto. Incluye las rutas del admin y, con `include("myapp.urls")`, todas las rutas definidas en tu aplicación.
  - Ventaja de `include`: separa y organiza rutas por aplicación, manteniendo limpio el archivo principal.

- `myapp/apps.py`
  - Configuración de la aplicación. Generalmente no requiere cambios, pero define metadatos de la app y su nombre interno.

---

## `myapp/models.py`: modelos de datos

- `class Project(models.Model)`
  - `name = models.CharField(max_length=100)`: nombre corto del proyecto.
  - `__str__(self)`: devuelve `self.name` para mostrar un nombre legible en admin, shell y templates.

- `class Task(models.Model)`
  - `title = models.CharField(max_length=100)`: título de la tarea.
  - `description = models.TextField(max_length=200)`: descripción breve.
  - `project = models.ForeignKey(Project, on_delete=models.CASCADE)`: relación muchos-a-uno; si se elimina el proyecto, se eliminan sus tareas.
  - `done = models.BooleanField(default=False)`: estado de completado.
  - `__str__(self)`: muestra título y nombre del proyecto asociado para facilitar la identificación.

Notas prácticas:
- Cambios en modelos requieren ejecutar `makemigrations` y luego `migrate`.
- Para añadir timestamps, puedes agregar `created = models.DateTimeField(auto_now_add=True)` y `updated = models.DateTimeField(auto_now=True)`.

---

## `myapp/admin.py`: panel de administración

- Registro actual:
  - `admin.site.register(Project)` y `admin.site.register(Task)` muestran ambos modelos en `/admin/`.
- Opcional (mejora): definir una clase `ModelAdmin` para personalizar columnas, filtros, búsqueda, campos de solo lectura, etc.
  - Ejemplo (opcional):
    - `class TaskAdmin(admin.ModelAdmin): list_display = ("title", "project", "done")`
    - `admin.site.register(Task, TaskAdmin)`
  - Nota: en tu repositorio actual, no hay campos `created/updated`, por lo que si usas `readonly_fields` para esos campos primero debes crearlos en el modelo.

---

## `myapp/forms.py`: formularios

- `class Create_new_task(forms.Form)`
  - `title`: campo de texto.
  - `description`: `Textarea` para texto largo.
  - Uso: se instancia en la vista `create_task()` y se renderiza con `{{ form.as_p }}` en el template.

- `class Create_new_project(forms.Form)`
  - `name`: campo de texto para nombre del proyecto.

Buenas prácticas:
- Para crear y validar instancias de modelos más fácilmente, puedes usar `ModelForm`. Facilita `form.save()` y validaciones.

---

## `myapp/views.py`: vistas (lógica de negocio)

Flujo común en vistas:
1) Recibir `request` (GET/POST).  
2) Consultar/crear datos en modelos.  
3) Devolver respuesta: `render()` (HTML), `HttpResponse` (texto), `JsonResponse` (JSON) o `redirect()` (redirección).

Funciones:
- `hello(request)`
  - Devuelve un `HttpResponse` simple con "Hello World". Útil para probar que el routing funciona.

- `about(request)`
  - Define una variable `username` y renderiza `about.html` pasando ese valor al contexto. Demuestra cómo inyectar datos en templates.

- `hello_params(request, username)` y `hello_params_id(request, id)`
  - Ejemplos de rutas con parámetros dinámicos (`<str:username>`, `<int:id>`). Devuelven texto interpolado.

- `index(request)`
  - Crea un título y renderiza `index.html` con esa variable. Muestra cómo pasar contexto a la vista principal.

- `projects(request)`
  - Obtiene todos los `Project` con `Project.objects.all()` y los envía al template `projects/projects.html`.  
  - En el template se recorre con `{% for project in projects %}`.

- `tasks(request)`
  - Obtiene todas las `Task` y las envía al template `tasks/tasks.html`.

- `tasks_id(request, id)`
  - Usa `get_object_or_404(Task, id=id)` para obtener una tarea por id o devolver 404 si no existe.  
  - En tu código actual devuelve `HttpResponse` con el título; también es común renderizar un detalle en `tasks/task_detail.html`.

- `tasks_name(request, title)`
  - Usa `Task.objects.get(title=title)` para buscar por título.  
  - Nota: actualmente retorna `render(request, "tasks/task.html")` sin pasar contexto y ese template podría no existir; es una zona a mejorar (ver sección de mejoras).

- `create_task(request)`
  - GET: renderiza `tasks/create_task.html` con `Create_new_task()`.
  - POST: crea `Task` con datos del formulario. En el repo actual se fija `project_id=2` como ejemplo; lo ideal es permitir elegir el proyecto desde el formulario y validar que existe. Al finalizar, redirige a `/tasks/`.

- `create_project(request)`
  - GET: muestra formulario `Create_new_project()` en `projects/create_project.html`.
  - POST: crea un `Project` con `request.POST['name']` y vuelve a renderizar el formulario. Alternativamente, puedes `redirect` a la lista de proyectos.

Funciones y utilidades clave usadas:
- `render(request, template, context)`: devuelve HTML renderizado con contexto.
- `HttpResponse(...)`: devuelve texto plano/HTML sin template.
- `JsonResponse(data, safe=False)`: devuelve JSON; útil para APIs simples o pruebas.
- `get_object_or_404(Model, **kwargs)`: atajo que devuelve 404 si no encuentra el objeto.
- `redirect(url_or_name)`: redirecciona a otra vista o URL.

---

## `myapp/urls.py`: enrutamiento de la aplicación

Rutas definidas y su propósito:
- `path('', views.index)`: página principal.
- `path('hello/', views.hello)`: prueba rápida del servidor.
- `path('about/', views.about)`: página con variable de contexto `username`.
- `path('hello/<str:username>', views.hello_params)`: parámetro tipo string en la URL.
- `path('hello_id/<int:id>', views.hello_params_id)`: parámetro tipo entero.
- `path('projects/', views.projects)`: lista de proyectos.
- `path('tasks/', views.tasks)`: lista de tareas.
- `path('tasks/<int:id>', views.tasks_id)`: detalle de tarea por id.
- `path('tasks/<str:title>', views.tasks_name)`: búsqueda por título (posible colisión con la ruta anterior si no se ordena bien).
- `path('create_task/', views.create_task)`: formulario de creación de tarea.
- `path('create_project/', views.create_project)`: formulario de creación de proyecto.

Consejo: asigna `name` a cada ruta para usar `reverse()`/`url` en templates y `redirect('nombre_de_ruta')` en vistas.

---

## Templates: cómo funcionan

Conceptos clave:
- Herencia de templates: `base.html` define bloques `{% block title %}` y `{% block content %}`; las páginas concretas los extienden con `{% extends 'layouts/base.html' %}`.
- Contexto: variables pasadas desde vistas se usan con `{{ variable }}`.
- Estructuras de control: bucles `{% for ... %}` y condicionales `{% if ... %}`.
- Seguridad CSRF: formularios POST deben incluir `{% csrf_token %}` para proteger contra ataques CSRF.

Ejemplos en el proyecto:
- `templates/projects/projects.html`: recorre `projects` y los muestra en tarjetas.
- `templates/projects/create_project.html`: muestra el formulario con `{{ form.as_p }}` y botón de enviar.
- `templates/tasks/...`: idem para tareas (lista, creación, detalle si se implementa).

---

## Flujo de datos típico (request-response)

1) El navegador solicita una URL (por ejemplo, `/projects/`).
2) `mysite/urls.py` delega en `myapp/urls.py` por medio de `include`.
3) La ruta `path('projects/', views.projects)` invoca `views.projects`.
4) La vista consulta la base de datos con `Project.objects.all()`.
5) Se llama a `render(request, 'projects/projects.html', { 'projects': projects })`.
6) Django resuelve el template, lo renderiza con el contexto y devuelve HTML al navegador.

---

## Buenas prácticas y mejoras recomendadas

- Validación y manejo de errores
  - Validar formularios con `form.is_valid()` y mostrar errores en templates.
  - Manejar posibles `DoesNotExist` al buscar proyectos/tareas.

- No hardcodear IDs
  - Evitar `project_id=2`. Ofrecer un `<select>` de proyectos o usar un `ModelChoiceField` en el formulario.

- Nombres de rutas
  - Añadir `name='projects'`, `name='tasks'`, etc., para usar `redirect('projects')` y `{% url 'projects' %}` en templates.

- `ModelForm`
  - Sustituir `forms.Form` por `forms.ModelForm` para `Project` y `Task`: permite validación automática y `form.save()`.

- Templates faltantes
  - Implementar `tasks/task_detail.html` y/o completar `tasks_name()` pasando contexto y manejando inexistencias.

- Mensajería al usuario
  - Usar `django.contrib.messages` para mostrar mensajes de éxito/error tras crear/editar entidades.

- Seguridad y despliegue
  - Revisar `DEBUG=False` en producción y configurar `ALLOWED_HOSTS`.
  - Configurar archivos estáticos con `collectstatic` y un servidor de archivos estáticos (o CDN) en producción.

---

## Glosario rápido de utilidades Django usadas

- `render(request, template, context)`: devuelve respuesta HTML desde un template.
- `redirect(to)`: redirige a una URL o a una vista por nombre.
- `HttpResponse(content)`: respuesta HTTP simple.
- `JsonResponse(data, safe=False)`: respuesta en JSON.
- `get_object_or_404(Model, **filters)`: retorna objeto o 404.
- `QuerySet`: colección perezosa de objetos del ORM. Métodos útiles: `.all()`, `.filter()`, `.get()`, `.create()`.

---

Con estas explicaciones tendrás a mano el “por qué” y el “cómo” de cada pieza del proyecto. Si quieres, puedo aplicar de inmediato algunas de las mejoras sugeridas (por ejemplo, reemplazar `project_id=2` por un selector de proyecto en el formulario de tareas y nombrar las rutas) y dejar el código listo.

---

# Anotaciones detalladas: imports y funciones línea por línea

Esta sección analiza los archivos más importantes, explicando cada importación y función para que entiendas qué hace y por qué se usa.

## `myapp/views.py` — imports explicados

```python
from django.shortcuts import render, get_object_or_404, redirect
```
- `render(request, template, context)`: compone una respuesta HTML combinando un template con un diccionario de datos (contexto).
- `get_object_or_404(Model, **filtros)`: intenta recuperar un objeto; si no existe, levanta un error 404 automáticamente.
- `redirect(to)`: devuelve una redirección HTTP hacia una URL o el nombre de una ruta.

```python
from django.http import HttpResponse, JsonResponse
```
- `HttpResponse`: para devolver texto/HTML simple sin template.
- `JsonResponse`: para APIs simples, devuelve datos serializados a JSON (útil para pruebas o endpoints mínimos).

```python
from .models import Project, Task
```
- Importa los modelos propios de la app, necesarios para consultar/crear registros en la base de datos.

```python
from .forms import Create_new_project, Create_new_task
```
- Importa formularios que encapsulan validación y representación de inputs HTML.

## `myapp/views.py` — funciones explicadas

- `hello(request)`: devuelve una cadena HTML fija para verificar el enrutamiento básico.

- `about(request)`: define una variable `username` y la pasa al template `about.html`. Ejemplo de cómo enviar datos a un template.

- `hello_params(request, username)` y `hello_params_id(request, id)`: muestran cómo capturar parámetros de la URL y usarlos dentro de la respuesta.

- `index(request)`: construye un título y lo envía a `index.html`. Es la “landing page” del sitio.

- `projects(request)`: consulta todos los `Project` (`Project.objects.all()`) y los pasa al template `projects/projects.html` para listarlos.

- `tasks(request)`: consulta todas las `Task` y las pasa al template `tasks/tasks.html`.

- `tasks_id(request, id)`: busca una `Task` por `id` (recomendado con `get_object_or_404`) y devuelve una respuesta. Es común renderizar un detalle en un template dedicado, por ejemplo `tasks/task_detail.html`.

- `tasks_name(request, title)`: busca por título. Debe manejar el caso en que no exista (`Task.DoesNotExist`) y pasar contexto al template.

- `create_task(request)`: patrón GET/POST. GET renderiza el formulario; POST crea la tarea y redirige a la lista. Mejora recomendada: no “hardcodear” `project_id`, sino elegir el proyecto con un `<select>` o `ModelChoiceField`.

- `create_project(request)`: patrón GET/POST para crear proyectos. En POST, crea el proyecto y redirige a la lista (`redirect('projects')`).

- `projects_detail(request, id)`: muestra el detalle de un proyecto y filtra sus tareas relacionadas con `Task.objects.filter(project_id=id)`. Renderiza `projects/detail.html` con `project` y `tasks`.

## `myapp/urls.py` — imports y rutas

```python
from django.urls import path
from . import views
```
- `path`: define patrones de URL y los asocia a vistas.
- `views`: referencia a las funciones de `views.py`.

Rutas con `name` (buena práctica):
- Permiten usar `{% url 'nombre' %}` en templates y `redirect('nombre')` en vistas.
- Facilita cambios de path sin romper enlaces internos.

Ejemplos clave:
- `path('', views.index, name="index")`: página principal.
- `path('projects/', views.projects, name="projects")`: lista de proyectos.
- `path('projects/<int:id>', views.projects_detail, name="projects_detail")`: detalle de proyecto.
- `path('tasks/', views.tasks, name="tasks")`: lista de tareas.

## `mysite/settings.py` — puntos clave

- `INSTALLED_APPS`: incluye `django.contrib.staticfiles` (sirve estáticos en desarrollo) y `myapp`.
- `TEMPLATES[0]['APP_DIRS'] = True`: busca templates dentro de cada app (por ejemplo, `myapp/templates/`).
- `DATABASES`: usa SQLite con archivo `db.sqlite3` en la raíz del proyecto.
- `DEBUG = True`: activo en desarrollo; en producción debe ser `False` y configurar `ALLOWED_HOSTS`.
- `STATIC_URL = 'static/'`: prefijo de URL para archivos estáticos.
- Opcional global: `STATICFILES_DIRS = [BASE_DIR / 'static']` si tienes una carpeta `static/` global (además de los estáticos por app).

## Templates y estáticos — cómo encajan

En `myapp/templates/layouts/base.html`:

```django
{% load static %}
<link rel="stylesheet" href="{% static 'style/main.css' %}">
```

- `{% load static %}` habilita la etiqueta `static` para resolver rutas de `/static/...`.
- El archivo CSS está en `myapp/static/style/main.css`, por eso usas `'{% static 'style/main.css' %}'`.
- En desarrollo, `django.contrib.staticfiles` lo sirve automáticamente con `DEBUG=True`.
- Consejo de cache busting durante desarrollo: `?v=1` al final del href.

Bloques y navegación con rutas por nombre:

```django
<a href="{% url 'projects' %}">Projects</a>
```

- Si cambias el path real en `urls.py`, los templates no se rompen porque usan el nombre de ruta.

## `myapp/forms.py` — widgets y estilos

```python
class Create_new_task(forms.Form):
    title = forms.CharField(..., widget=forms.TextInput(attrs={'class': 'input'}))
    description = forms.CharField(..., widget=forms.Textarea(attrs={'class': 'input'}))
```

- Los `attrs` agregan clases CSS al `<input>`/`<textarea>` generado (aquí, `class="input"`).
- En `main.css` defines `.input { ... }` para estilizar esos campos.

## CSS (`myapp/static/style/main.css`) — notas y trucos

- Cárgalo después de otras hojas (como Bootstrap) si quieres que sus reglas tengan prioridad.
- Para diagnosticar si una regla no “agarra”:
  - Revisa en DevTools si la regla aparece en “Styles”.
  - Usa `!important` temporalmente o incrementa especificidad (`body .container { ... }`).
  - Verifica que el archivo descargado incluya la regla (pestaña “Network”).

## Flujo CRUD resumido

- Crear Project: `GET` muestra formulario → `POST` valida/crea → `redirect('projects')`.
- Crear Task: `GET` muestra formulario → `POST` valida/crea → `redirect('tasks')`.
- Detalle Project: `GET /projects/<id>` → consulta `Project` + `Task` relacionadas → template `projects/detail.html`.

## Errores comunes y cómo evitarlos

- “Hardcodear” IDs (p. ej., `project_id=2`): usa un campo de selección (`ModelChoiceField`) o `ModelForm` con relación.
- No nombrar rutas: añade `name=` y usa `{% url %}`/`redirect()`.
- Falta de `csrf_token` en formularios POST: añade `{% csrf_token %}`.
- Template sin `{% load static %}`: los archivos estáticos no se resuelven.

## Mejoras sugeridas (siguientes pasos prácticos)

- Convertir formularios a `ModelForm` para `Project` y `Task`.
- Añadir `created`/`updated` en modelos y mostrarlos en admin.
- Implementar `tasks/task_detail.html` y completar `tasks_name()` con manejo de errores.
- Añadir mensajes de éxito/error con `django.contrib.messages`.
- Separar estilos por componentes o usar un framework CSS si lo prefieres.

---

Con esto, el tutorial queda como una guía de referencia detallada para reutilizar en futuros proyectos.

---

# Cambios finales implementados y explicación detallada

Esta sección resume y explica los cambios que añadiste al finalizar el curso, con referencias a archivos y motivos de diseño.

## Rutas con nombre en `myapp/urls.py`

Archivo: `myapp/urls.py`

- Se añadieron `name` a cada `path`, por ejemplo:
  - `path('', views.index, name="index")`
  - `path('projects/', views.projects, name="projects")`
  - `path('projects/<int:id>', views.projects_detail, name="projects_detail")`
  - `path('tasks/', views.tasks, name="tasks")`

Ventajas:
- Usar `{% url 'nombre' %}` en templates y `redirect('nombre')` en vistas evita romper enlaces si el path cambia.

## Vista de detalle de proyecto y template

Archivos: `myapp/views.py` y `myapp/templates/projects/detail.html`

- Vista `projects_detail(request, id)`:
  - Recupera el `Project` por id y sus `Task` relacionadas con `Task.objects.filter(project_id=id)`.
  - Renderiza `projects/detail.html` con el contexto `{ "project": project, "tasks": tasks }`.

- Template `projects/detail.html`:
  - Muestra el nombre del proyecto (`{{ project.name }}`) y recorre las tareas con `{% for task in tasks %}`.

- Enlace desde el listado:
  - En `templates/projects/projects.html` agregaste:
    ```django
    <a href="{% url 'projects_detail' project.id %}"> {{ project.name }} </a>
    ```
  - Esto navega al detalle correspondiente.

## Layout base con estáticos y navegación

Archivo: `myapp/templates/layouts/base.html`

- Añadiste `{% load static %}` al inicio para poder referenciar archivos en `myapp/static/`.
- Incluiste tu CSS principal:
  ```django
  <link rel="stylesheet" href="{% static 'style/main.css' %}">
  ```
- Estructuraste un `<nav class="navbar container">` con enlaces usando rutas por nombre (`{% url 'index' %}`, `{% url 'projects' %}`, etc.).
- Agregaste un contenedor principal:
  ```html
  <div class="container">
    {% block content %}{% endblock %}
  </div>
  ```

## Uso de archivos estáticos en templates

Archivo: `myapp/templates/index.html`

- Cargaste `static` y mostraste una imagen ubicada en `myapp/static/`:
  ```django
  {% load static %}
  <img src="{% static "icon_github.jpg" %}" alt="" width="50px">
  ```
- Nota: coloca `icon_github.jpg` dentro de `myapp/static/` (o subcarpetas) para que se resuelva correctamente.

## Formularios con widgets estilizados

Archivo: `myapp/forms.py`

- Añadiste widgets con clases CSS:
  ```python
  title = forms.CharField(..., widget=forms.TextInput(attrs={'class': 'input'}))
  description = forms.CharField(..., widget=forms.Textarea(attrs={'class': 'input'}))
  name = forms.CharField(..., widget=forms.TextInput(attrs={'class': 'input'}))
  ```
- Esto permite estilizar inputs desde `main.css` con la clase `.input`.

## Hoja de estilos principal

Archivo: `myapp/static/style/main.css`

- Estructura y estilos clave:
  - `.navbar` y elementos de navegación.
  - `.container` con `max-width`, `margin: auto` y `padding`.
  - `.card` para presentar items (proyectos, etc.).
  - `.input` para campos de formulario generados por `forms.py`.

Buenas prácticas:
- Cargar `main.css` después de cualquier framework (como Bootstrap) para sobrescribir estilos cuando sea necesario.
- Durante desarrollo, bust cache con un query param: `{% static 'style/main.css' %}?v=2`.

## Notas de despliegue para estáticos

- En desarrollo (`DEBUG=True`), Django sirve archivos estáticos automáticamente con `django.contrib.staticfiles`.
- En producción (`DEBUG=False`):
  - Configura `STATIC_ROOT` (por ejemplo, `STATIC_ROOT = BASE_DIR / 'staticfiles'`).
  - Ejecuta `python manage.py collectstatic` para recopilar todos los estáticos.
  - Sirve `STATIC_ROOT` con tu servidor web (Nginx/Apache) o un CDN.

---

Con esto, el documento refleja no solo el estado final del proyecto sino también el razonamiento detrás de cada cambio, dejando una guía sólida para replicar o extender la solución en el futuro.
