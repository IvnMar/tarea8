# Django PostgreSQL App

Una aplicación web moderna construida con Django 4.x, PostgreSQL y Docker que implementa un sistema completo de autenticación de usuarios con interfaz responsive.

## 🚀 Características

- **Autenticación completa**: Login, registro, logout y gestión de sesiones
- **Base de datos robusta**: PostgreSQL con migraciones automatizadas
- **Interfaz moderna**: Bootstrap 5 con diseño responsive y dark mode
- **Usuarios demo**: Cuentas de prueba pre-configuradas
- **Dashboard personalizado**: Panel de control con estadísticas de usuario
- **Gestión de perfil**: Edición de información personal
- **Contenedorización**: Docker y Docker Compose para desarrollo
- **Logging detallado**: Sistema de debug para desarrollo

## 🛠️ Tecnologías

- **Backend**: Django 4.x + Python 3.10
- **Base de datos**: PostgreSQL 13
- **Frontend**: Bootstrap 5 + Font Awesome 6 + CSS moderno
- **Contenedores**: Docker + Docker Compose
- **Timezone**: America/Mexico_City (configurable)

## 📋 Requisitos previos

- Docker y Docker Compose instalados
- Git (para clonar el repositorio)
- Puerto 8000 disponible para la aplicación
- Puerto 5432 disponible para PostgreSQL

## 🚀 Instalación y configuración

### 1. Preparar el proyecto

```bash
# Crear estructura de directorios
mkdir django-postgresql-app
cd django-postgresql-app

# Crear el archivo docker-compose.yml
```

### 2. Crear docker-compose.yml

Crea un archivo `docker-compose.yml` en la raíz del proyecto:

```yaml
version: '3.8'

services:
  db:
    image: postgres:13
    volumes:
      - postgres_data:/var/lib/postgresql/data/
    environment:
      POSTGRES_DB: postgres
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: root
    ports:
      - "5432:5432"
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5

  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/code
    ports:
      - "8000:8000"
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=root
      - DJANGO_SECRET_KEY=tu-clave-secreta-aqui
    depends_on:
      db:
        condition: service_healthy

volumes:
  postgres_data:
```

### 3. Crear Dockerfile

Crea un `Dockerfile` en la raíz del proyecto:

```dockerfile
FROM python:3.10

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

WORKDIR /code

COPY requirements.txt /code/
RUN pip install -r requirements.txt

COPY . /code/
```

### 4. Crear requirements.txt

```txt
Django==4.2.23
psycopg2-binary==2.9.7
```

### 5. Estructura de directorios

Organiza los archivos según esta estructura:

```
django-postgresql-app/
├── docker-compose.yml
├── Dockerfile
├── requirements.txt
├── manage.py
└── app/
    ├── __init__.py
    ├── settings.py
    ├── urls.py
    ├── wsgi.py
    ├── admin.py
    ├── apps.py
    ├── models.py
    ├── views.py
    ├── forms.py
    ├── tests.py
    ├── migrations/
    │   ├── __init__.py
    │   ├── 0001_initial.py
    │   └── 0002_alter_usuario_options.py
    └── templates/
        ├── base.html
        ├── index.html
        ├── login.html
        ├── register.html
        ├── dashboard.html
        └── profile.html
```

### 6. Crear manage.py

```python
#!/usr/bin/env python
import os
import sys

if __name__ == '__main__':
    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'app.settings')
    try:
        from django.core.management import execute_from_command_line
    except ImportError as exc:
        raise ImportError(
            "Couldn't import Django. Are you sure it's installed and "
            "available on your PYTHONPATH environment variable? Did you "
            "forget to activate a virtual environment?"
        ) from exc
    execute_from_command_line(sys.argv)
```

## 🏃‍♂️ Ejecutar la aplicación

### 1. Construir y levantar los contenedores

```bash
# Construir la aplicación
docker-compose build

# Levantar los servicios
docker-compose up -d

# Ver los logs
docker-compose logs -f web
```

### 2. Ejecutar migraciones

```bash
# Aplicar migraciones (esto también crea usuarios demo)
docker-compose exec web python manage.py migrate

# Crear superusuario (opcional)
docker-compose exec web python manage.py createsuperuser
```

### 3. Acceder a la aplicación

- **Aplicación principal**: http://localhost:8000
- **Panel de administración**: http://localhost:8000/admin/

## 👥 Usuarios de prueba

La aplicación crea automáticamente usuarios demo después de ejecutar las migraciones:

| Usuario | Contraseña | Tipo |
|---------|------------|------|
| admin | admin123 | Administrador |
| usuario1 | password123 | Usuario normal |
| test | test123 | Usuario de prueba |
| demo | demo123 | Demostración |
| invitado | invitado123 | Invitado |
| supervisor | supervisor123 | Supervisor |

## 🗺️ Rutas disponibles

| URL | Vista | Descripción |
|-----|-------|-------------|
| `/` | index | Página de inicio |
| `/login/` | login_view | Formulario de inicio de sesión |
| `/register/` | register_view | Formulario de registro |
| `/dashboard/` | dashboard_view | Panel de control del usuario |
| `/profile/` | profile_view | Perfil del usuario |
| `/logout/` | logout_view | Cerrar sesión |
| `/admin/` | Django Admin | Panel de administración |

## 🎨 Características de la interfaz

### Página de inicio
- Diseño landing page moderno
- Cards de acción para login/registro
- Sección de características
- Responsive design

### Sistema de autenticación
- **Login**: Formulario con usuarios demo visibles
- **Registro**: Validación en tiempo real de contraseñas
- **Dashboard**: Estadísticas, información personal, acciones rápidas
- **Perfil**: Edición de datos, historial, configuración de seguridad

### Funcionalidades técnicas
- **Validaciones**: Frontend y backend
- **Sesiones**: Manejo seguro de sesiones de usuario
- **Mensajes**: Sistema de notificaciones con Bootstrap alerts
- **Responsive**: Compatible con móviles y tablets
- **Debug**: Logging detallado para desarrollo

## 🔧 Desarrollo

### Comandos útiles

```bash
# Ver logs en tiempo real
docker-compose logs -f

# Ejecutar comandos Django
docker-compose exec web python manage.py <comando>

# Acceder al shell de Django
docker-compose exec web python manage.py shell

# Ejecutar tests
docker-compose exec web python manage.py test

# Recolectar archivos estáticos
docker-compose exec web python manage.py collectstatic

# Crear nueva migración
docker-compose exec web python manage.py makemigrations

# Aplicar migraciones
docker-compose exec web python manage.py migrate
```

### Estructura de la base de datos

```sql
-- Tabla usuarios
CREATE TABLE usuarios (
    id BIGSERIAL PRIMARY KEY,
    usuario VARCHAR(50) UNIQUE NOT NULL,
    contrasena VARCHAR(255) NOT NULL,
    correo VARCHAR(254) UNIQUE NOT NULL,
    fecha_creacion TIMESTAMP WITH TIME ZONE NOT NULL,
    activo BOOLEAN NOT NULL DEFAULT true
);
```

## 🛠️ Personalización

### Cambiar zona horaria
En `settings.py`:
```python
TIME_ZONE = 'America/Mexico_City'  # Cambiar según necesidades
```

### Modificar usuarios demo
En `models.py`, función `crear_usuarios_por_defecto()`:
```python
usuarios_por_defecto = [
    # Agregar o modificar usuarios aquí
]
```

### Configurar email (opcional)
En `settings.py`:
```python
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_PORT = 587
EMAIL_USE_TLS = True
EMAIL_HOST_USER = 'tu-email@gmail.com'
EMAIL_HOST_PASSWORD = 'tu-password'
```

![image](https://github.com/user-attachments/assets/36a3d417-06cd-4190-8e31-23697640c100)
![image](https://github.com/user-attachments/assets/2004a6f6-4afc-4cf3-90be-911b065f7599)
![image](https://github.com/user-attachments/assets/15e06cba-9be2-4860-a89d-7e37521be728)


