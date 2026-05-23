# Guía de Deploy de Django (sitio estático) en Render

Esta guía es un manual paso a paso para preparar y desplegar un proyecto Django (sin base de datos) en Render, usando la capa gratuita. Incluye qué archivos crear, qué poner dentro de cada uno, cómo configurar `settings.py`, y cómo realizar el deploy desde “New Web Service”.

---

## 1) Estructura mínima del proyecto

Asegúrate de tener una estructura similar a esta (nombres de app y proyecto a modo de ejemplo):

```
juserdev_django/
├─ juserdev/                # carpeta del proyecto Django (settings, urls, wsgi, asgi)
│  ├─ __init__.py
│  ├─ asgi.py
│  ├─ settings.py
│  ├─ urls.py
│  └─ wsgi.py
├─ landing/                 # app Django con estáticos y templates
│  ├─ static/
│  │  ├─ css/
│  │  ├─ img/
│  │  └─ js/
│  └─ templates/
│     ├─ layouts/
│     └─ landing/
├─ manage.py
├─ requirements.txt
├─ Procfile
├─ runtime.txt
└─ render.yaml              # opcional (si usas Blueprint), útil para dejar automatizado
```

---

## 2) Configurar los archivos clave

### 2.1 `requirements.txt`
Incluye Django, Gunicorn y Whitenoise para servir estáticos en producción:

```
Django==5.1.2
gunicorn
whitenoise
```

Si tu proyecto necesita otras librerías, agrégalas aquí.

### 2.2 `Procfile`
Render leerá este archivo para saber cómo arrancar tu app. Debe apuntar a tu módulo WSGI:

```
web: gunicorn juserdev.wsgi:application
```

- Reemplaza `juserdev` por el nombre de tu módulo de proyecto si es distinto.
- Importante: sin punto final.

### 2.3 `runtime.txt`
Fija la versión de Python para que Render use la misma que usas localmente:

```
python-3.12.5
```

Cámbiala si usas otra versión (3.11.x o 3.12.x soportadas). Mantén coherencia con tu entorno local.

### 2.4 `render.yaml` (opcional pero recomendado)
Si prefieres usar “Blueprint” en Render (infra como código) crea este archivo en la raíz:

```yaml
services:
  - type: web
    name: juserdev-django
    env: python
    buildCommand: |
      pip install --upgrade pip
      pip install -r requirements.txt
      python manage.py collectstatic --noinput
    startCommand: gunicorn juserdev.wsgi:application
    autoDeploy: true
```

Con `render.yaml`, Render sabrá cómo construir y arrancar tu servicio automáticamente.

---

## 3) Configurar `settings.py` para producción con Render

### 3.1 Conceptos clave (app estática sin BD)

- Activa Whitenoise para servir archivos estáticos directamente desde tu app (sin Nginx).
- Ajusta dinámicamente `ALLOWED_HOSTS` y `CSRF_TRUSTED_ORIGINS` usando `RENDER_EXTERNAL_HOSTNAME` (variable que Render inyecta con tu dominio generado).
- Respeta HTTPS detrás del proxy de Render con `SECURE_PROXY_SSL_HEADER` para que Django trate las solicitudes como seguras.
- Define `STATIC_ROOT` para que `collectstatic` recopile todos los archivos en producción y `STATICFILES_STORAGE` para compresión y cacheo con hash.

### 3.2 Ejemplo completo y comentado de `juserdev/settings.py` (mínimo para Render)

```python
"""
Configuración mínima para desplegar un sitio estático en Render con Django.
Explicaciones línea a línea incluidas.
"""

import os
from pathlib import Path

# 1) Rutas base del proyecto
BASE_DIR = Path(__file__).resolve().parent.parent

# 2) Clave secreta
# - En desarrollo puedes dejar una clave local.
# - En producción (Render) debes definirla como variable de entorno SECRET_KEY.
SECRET_KEY = os.environ.get(
    "SECRET_KEY",
    "insegura-solo-desarrollo-cambia-esto-localmente"
)

# 3) DEBUG por entorno
# - En Render define DEBUG=False en el panel de variables de entorno.
# - En local, si no está definida, asumimos True para facilitar desarrollo.
DEBUG = os.environ.get("DEBUG", "True") == "True"

# 4) Hosts permitidos y orígenes confiables para CSRF
# - Render expone RENDER_EXTERNAL_HOSTNAME con tu dominio del servicio.
# - Si existe, limitamos ALLOWED_HOSTS a ese host y configuramos CSRF.
# - En local, permitimos todos los hosts y dejamos CSRF_TRUSTED_ORIGINS vacío.
RENDER_EXTERNAL_HOSTNAME = os.environ.get("RENDER_EXTERNAL_HOSTNAME")
if RENDER_EXTERNAL_HOSTNAME:
    ALLOWED_HOSTS = [RENDER_EXTERNAL_HOSTNAME]
    CSRF_TRUSTED_ORIGINS = [f"https://{RENDER_EXTERNAL_HOSTNAME}"]
else:
    ALLOWED_HOSTS = ["*"]
    CSRF_TRUSTED_ORIGINS = []

# 5) Aplicaciones instaladas
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # Tus apps de proyecto
    'landing',
]

# 6) Middleware
# - WhiteNoise debe ir inmediatamente después de SecurityMiddleware.
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

# 7) URL de enrutamiento principal
ROOT_URLCONF = 'juserdev.urls'

# 8) Templates (por defecto)
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],  # usa plantillas por-app (landing/templates/...)
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

# 9) WSGI: punto de entrada para Gunicorn en producción
WSGI_APPLICATION = 'juserdev.wsgi.application'

# 10) Base de datos
# - Para un sitio estático no necesitas BD en Render.
# - Mantén SQLite para desarrollo local si lo necesitas.
from pathlib import Path  # redundante si ya importaste arriba, aquí sólo por claridad del bloque
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

# 11) Internacionalización (ajústalo a tu preferencia)
LANGUAGE_CODE = 'en-us'
TIME_ZONE = 'UTC'
USE_I18N = True
USE_TZ = True

# 12) Archivos estáticos
# - STATIC_URL: prefijo público de los estáticos.
# - STATIC_ROOT: carpeta destino donde collectstatic reúne archivos para producción.
# - STATICFILES_STORAGE: WhiteNoise con compresión + cache busting por hash.
STATIC_URL = '/static/'
STATIC_ROOT = BASE_DIR / 'staticfiles'
STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'

# 13) HTTPS detrás del proxy de Render
# - Hace que request.is_secure() funcione correctamente y ciertas redirecciones/securizaciones operen bien.
SECURE_PROXY_SSL_HEADER = ("HTTP_X_FORWARDED_PROTO", "https")

# 14) Tipo de clave primaria por defecto
DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'
```

Notas importantes sobre estáticos:
- No necesitas `STATICFILES_DIRS` si colocas archivos estáticos por app en `landing/static/...`.
- En producción, `python manage.py collectstatic --noinput` moverá los archivos a `STATIC_ROOT` (`staticfiles/`).
- WhiteNoise servirá los archivos con compresión y nombres con hash (manifest) para cacheo eficiente.

-

## 4) Buenas prácticas con archivos estáticos

- Ubica los archivos dentro de la app: `landing/static/` con subcarpetas `css/`, `img/`, `js/`.
- En tus templates, usa `{% load static %}` y referencia sin el prefijo del nombre del app:

```html
{% load static %}
<link rel="stylesheet" href="{% static 'css/style.css' %}">
<script src="{% static 'js/miscript.js' %}"></script>
<img src="{% static 'img/logo.png' %}" alt="logo">
```

- En producción, `python manage.py collectstatic --noinput` copiará todo a `STATIC_ROOT` y Whitenoise los servirá.

---

## 5) Preparación del repositorio

1. Asegúrate de tener estos archivos en la raíz del repo: `requirements.txt`, `Procfile`, `runtime.txt`, y opcionalmente `render.yaml`.
2. Verifica `.gitignore` para ignorar archivos locales:
   - `.venv/`, `venv/`, `env/`
   - `*.env`, `*.local`
   - `db.sqlite3` (si no quieres subirla)
   - `staticfiles/`
3. Commit y push de todos los cambios.

---

## 6) Deploy en Render (capa gratuita) usando “New Web Service”

1. Inicia sesión en Render → botón "New" → "Web Service".
2. Conecta tu repositorio (GitHub/GitLab) y elige la rama principal.
3. Configura:
   - Name: el que prefieras (p. ej., `juserdev-django`).
   - Region: la más cercana a tus usuarios.
   - Instance Type: `Free`.
   - Environment: `Python`.
4. Build Command:
   ```
   pip install --upgrade pip && pip install -r requirements.txt && python manage.py collectstatic --noinput
   ```
5. Start Command:
   ```
   gunicorn juserdev.wsgi:application
   ```
6. Variables de entorno (Environment → Add Environment Variable):
   - `DEBUG` = `False`
   - `SECRET_KEY` = una clave segura (no uses la de desarrollo). Puedes generar una en local:
     ```
     python -c "import secrets; print(secrets.token_urlsafe(50))"
     ```
   - Render inyecta `RENDER_EXTERNAL_HOSTNAME` automáticamente.
7. Activa Auto Deploy si quieres que cada push redeploye.
8. Crea el servicio con “Create Web Service”.

Verificación:
- Revisa `Logs` para confirmar que `pip install`, `collectstatic` y `gunicorn` corrieron sin errores.
- Abre la URL del servicio y valida que carguen CSS/JS/imágenes. Forzar recarga (Cmd/Ctrl + Shift + R) si ves cacheos.

---

## 7) Flujo de trabajo local recomendado

- Usa Python 3.12 (coherente con `runtime.txt`).
- Crea y activa un virtualenv en la raíz del proyecto:
  ```bash
  python3.12 -m venv .venv
  source .venv/bin/activate
  python -m pip install --upgrade pip
  python -m pip install -r requirements.txt
  ```
- Ejecuta el servidor:
  ```bash
  python manage.py runserver
  ```
- Al agregar paquetes nuevos:
  ```bash
  pip install <paquete>
  pip freeze > requirements.txt
  ```

---

## 8) Solución de problemas comunes

- 404 al cargar JS/CSS/imagenes en producción:
  - Verifica que usas `{% load static %}` y rutas como `{% static 'js/archivo.js' %}` sin prefijo del app.
  - Asegúrate de que `collectstatic` se ejecuta en el build.
  - Forzar recarga del navegador (Whitenoise usa hash/manifest y caché agresiva).

- Error WSGI en local (no arranca `runserver`):
  - Asegúrate de usar Python 3.12 y de instalar dependencias en el venv (incluido `whitenoise`).
  - Ejecuta `python manage.py check` para obtener diagnósticos.

- Dominio personalizado:
  - Puedes seguir usando la lógica con `RENDER_EXTERNAL_HOSTNAME` o agregar tu dominio en `ALLOWED_HOSTS` y `CSRF_TRUSTED_ORIGINS` si lo ves necesario.

---

## 9) Checklist final antes del deploy

- [ ] `requirements.txt` contiene `Django`, `gunicorn`, `whitenoise`.
- [ ] `Procfile` apunta al módulo WSGI correcto, p. ej.: `web: gunicorn juserdev.wsgi:application`.
- [ ] `runtime.txt` fija una versión de Python soportada por Render, igual a tu entorno local.
- [ ] `settings.py` configurado con `WhiteNoiseMiddleware`, `STATIC_ROOT`, `STATICFILES_STORAGE`, `SECURE_PROXY_SSL_HEADER`, y lógica de `RENDER_EXTERNAL_HOSTNAME`.
- [ ] Estáticos referenciados con `{% static %}` y rutas correctas.
- [ ] `.gitignore` ignora `staticfiles/` y archivos locales.
- [ ] Variables en Render: `DEBUG=False` y `SECRET_KEY` segura.

---

## 10) Extensiones futuras (opcional)

- Base de datos Postgres en Render:
  - Agregar `psycopg2-binary` y/o `dj-database-url` en `requirements.txt`.
  - Configurar `DATABASES` con variables de entorno.
  - Añadir `python manage.py migrate` al `buildCommand` o realizar migraciones manualmente.

- Tareas programadas o servicios adicionales: usar más entries en `render.yaml` o servicios separados.

---

Con esta guía deberías poder repetir el proceso de preparación y deploy de forma consistente en tus próximos proyectos Django estáticos en Render. Si quieres, puedo adaptar este documento como plantilla con variables (nombre del proyecto/app) para reutilizarlo aún más rápido.
