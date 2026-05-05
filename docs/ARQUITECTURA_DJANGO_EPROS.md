# Arquitectura de Software para EPROS con Django

## 📋 Índice
1. [Arquitecturas Recomendadas](#arquitecturas-recomendadas)
2. [Comparativa](#comparativa-de-arquitecturas)
3. [Arquitectura Propuesta para EPROS](#arquitectura-propuesta-para-epros)
4. [Estructura de Carpetas](#estructura-de-carpetas)
5. [Flujo de Datos](#flujo-de-datos)
6. [Patrones de Diseño](#patrones-de-diseño)
7. [Stack Tecnológico](#stack-tecnológico)
8. [Implementación Step-by-Step](#implementación-step-by-step)

---

## 🎯 Arquitecturas Recomendadas

### 1. **MVC (Model-View-Controller)** ⭐ Estándar Django

```
┌─────────────────────────────────────────────┐
│              USUARIO (Browser)              │
└────────────────┬────────────────────────────┘
                 │
    ┌────────────┴────────────┐
    │                         │
┌───▼────────┐         ┌──────▼───────┐
│   VIEW     │         │  CONTROLLER  │
│            │         │              │
│ Templates  │◄────────┤ Logic        │
│ (HTML)     │         │ (Django View)│
└────────────┘         └──────┬───────┘
                              │
                       ┌──────▼───────┐
                       │   MODEL      │
                       │              │
                       │ Django ORM   │
                       │ (Database)   │
                       └──────────────┘
```

**Ventajas:**
- ✅ Estándar Django
- ✅ Fácil de implementar
- ✅ Bueno para MVPs
- ✅ Menor complejidad inicial

**Desventajas:**
- ❌ Views muy grandes
- ❌ Difícil de testear
- ❌ No escalable

---

### 2. **MVT (Model-View-Template)** - Django Variant

Similar a MVC pero Django lo llama MVT (Template en lugar de View).

**Diferencia clave:**
- Django maneja automáticamente URLs → View → Template

---

### 3. **MVC + Servicios (Recomendado para EPROS)** ⭐⭐⭐

```
┌──────────────────────────────────────────────────┐
│                   VIEWS LAYER                    │
│    (URLs, Vistas, API Endpoints, Serializers)   │
└────────────────┬─────────────────────────────────┘
                 │
┌────────────────▼─────────────────────────────────┐
│              SERVICES LAYER                      │
│  (Lógica de Negocio, Casos de Uso, Validación) │
└────────────────┬─────────────────────────────────┘
                 │
┌────────────────▼─────────────────────────────────┐
│            REPOSITORY LAYER                      │
│      (Acceso a Datos, Queries, ORM)             │
└────────────────┬─────────────────────────────────┘
                 │
┌────────────────▼─────────────────────────────────┐
│              MODELS LAYER                        │
│         (Django Models, Entidades)              │
└────────────────┬─────────────────────────────────┘
                 │
        ┌────────▼────────┐
        │   DATABASE      │
        │   PostgreSQL    │
        └─────────────────┘
```

**Ventajas:**
- ✅ Separación clara de responsabilidades
- ✅ Fácil de testear
- ✅ Escalable
- ✅ Reutilizable
- ✅ Ideal para sistemas complejos

**Desventajas:**
- ❌ Más carpetas y archivos
- ❌ Curva de aprendizaje mayor

---

### 4. **Clean Architecture** 🏆

```
┌─────────────────────────────────────────────────────┐
│              PRESENTACIÓN LAYER                     │
│  (Views, Serializers, Endpoints, Response Format) │
└────────────────┬────────────────────────────────────┘
                 │
┌────────────────▼────────────────────────────────────┐
│            APPLICATION LAYER                       │
│   (Use Cases, DTOs, Orquestación, Lógica)         │
└────────────────┬────────────────────────────────────┘
                 │
┌────────────────▼────────────────────────────────────┐
│            DOMAIN LAYER                            │
│   (Entidades, Interfaces, Reglas de Negocio)      │
└────────────────┬────────────────────────────────────┘
                 │
┌────────────────▼────────────────────────────────────┐
│         INFRASTRUCTURE LAYER                       │
│  (Repositorios, Database, APIs Externas)          │
└─────────────────────────────────────────────────────┘
```

**Ventajas:**
- ✅ Máxima escalabilidad
- ✅ Independencia tecnológica
- ✅ Excelente testabilidad
- ✅ Código limpio

**Desventajas:**
- ❌ Muy compleja para empezar
- ❌ Mucho "boilerplate"
- ❌ Overhead inicial

---

### 5. **Arquitectura en Capas + DDD (Domain-Driven Design)**

```
┌────────────────────────────────────────────────────┐
│            PRESENTATION LAYER                      │
│  Django Views, Serializers, URL Routing          │
└────────────────┬─────────────────────────────────┘
                 │
┌────────────────▼─────────────────────────────────┐
│          APPLICATION LAYER                       │
│  Use Cases, Commands, Queries, DTOs             │
└────────────────┬─────────────────────────────────┘
                 │
┌────────────────▼─────────────────────────────────┐
│           DOMAIN LAYER                           │
│  Entities, Value Objects, Services Dominios    │
└────────────────┬─────────────────────────────────┘
                 │
┌────────────────▼─────────────────────────────────┐
│       INFRASTRUCTURE LAYER                       │
│  Repositories, QueryObjects, ExternalAPIs      │
└────────────────┬─────────────────────────────────┘
                 │
          DATABASE LAYER
```

**Ventajas:**
- ✅ Enfoque en el dominio (incapacidades)
- ✅ Fácil de evolucionar
- ✅ Ubiquitous Language
- ✅ Ideal para sistemas financiero-legales

---

## 📊 Comparativa de Arquitecturas

| Aspecto | MVC Simple | MVC+Servicios | Clean Arch | DDD |
|---------|-----------|---------------|-----------|-----|
| **Complejidad** | Baja | Media | Alta | Alta |
| **Escalabilidad** | Baja | Media-Alta | Alta | Alta |
| **Testabilidad** | Baja | Media-Alta | Excelente | Excelente |
| **Tiempo inicio** | Muy rápido | Rápido | Lento | Lento |
| **Mantenibilidad** | Baja | Media | Alta | Alta |
| **Para EPROS** | ❌ No | ✅ Sí | ⭐ Posible | ⭐⭐ Ideal |
| **Curva aprendizaje** | Baja | Media | Alta | Muy Alta |

---

## ✅ Arquitectura Propuesta para EPROS

### **MVC + Servicios (Arquitectura en Capas)**

**Por qué esta arquitectura:**

1. ✅ Equilibrio entre **simplicidad e innovación**
2. ✅ Separación clara de **responsabilidades**
3. ✅ **Fácil de testear** (crucial para finanzas)
4. ✅ **Escalable** para nuevas funcionalidades
5. ✅ Estándar en **industria Django**
6. ✅ Ideal para **proyectos medianos-grandes**
7. ✅ Soporta **futura evolución a DDD**

---

## 📁 Estructura de Carpetas Propuesta

```
epros_project/
│
├── manage.py
├── requirements.txt
├── README.md
├── .env.example
├── .gitignore
│
├── config/                          # Configuración Django
│   ├── __init__.py
│   ├── settings/
│   │   ├── base.py                 # Configuración base
│   │   ├── development.py          # Dev settings
│   │   ├── production.py           # Prod settings
│   │   └── test.py                 # Test settings
│   ├── urls.py                     # URLs principales
│   ├── wsgi.py
│   └── asgi.py                     # Para WebSockets futuros
│
├── apps/                            # Aplicaciones Django
│   │
│   ├── usuarios/                   # App: Gestión de usuarios
│   │   ├── migrations/
│   │   ├── models.py              # Usuario, Rol, Permiso
│   │   ├── views.py               # Vistas/Endpoints
│   │   ├── serializers.py         # DRF Serializers
│   │   ├── urls.py
│   │   ├── services.py            # Lógica de negocio
│   │   ├── repositories.py        # Acceso a datos
│   │   ├── forms.py
│   │   ├── admin.py
│   │   ├── tests.py
│   │   └── __init__.py
│   │
│   ├── incapacidades/             # App: Gestión de incapacidades
│   │   ├── migrations/
│   │   ├── models.py              # Incapacidad, Estado, Historial
│   │   ├── views.py               # APIs REST
│   │   ├── serializers.py         # Serialización
│   │   ├── urls.py
│   │   ├── services/              # Lógica de negocio
│   │   │   ├── __init__.py
│   │   │   ├── registro_service.py       # CU-01: Registrar
│   │   │   ├── validacion_service.py    # CU-02: Validar
│   │   │   ├── transcripcion_service.py # CU-03: Transcribir
│   │   │   ├── radicacion_service.py    # CU-04: Radicar
│   │   │   ├── estado_service.py        # CU-05: Gestionar estado
│   │   │   ├── seguimiento_service.py   # CU-06: Seguimiento
│   │   │   ├── cobro_service.py         # CU-07: Cobro
│   │   │   ├── pago_service.py          # CU-08: Pago
│   │   │   ├── rechazo_service.py       # CU-12: Rechazo
│   │   │   ├── conciliacion_service.py  # CU-14: Conciliación
│   │   │   └── cobro_juridico_service.py# CU-13: Cobro jurídico
│   │   ├── repositories/          # Acceso a datos
│   │   │   ├── __init__.py
│   │   │   ├── incapacidad_repository.py
│   │   │   └── historial_repository.py
│   │   ├── admin.py
│   │   ├── tests/
│   │   │   ├── test_models.py
│   │   │   ├── test_views.py
│   │   │   ├── test_services.py
│   │   │   └── test_integration.py
│   │   └── __init__.py
│   │
│   ├── consultas/                 # App: Consultas y reportes
│   │   ├── models.py              # Vistas de lectura
│   │   ├── views.py               # APIs de consulta
│   │   ├── serializers.py
│   │   ├── urls.py
│   │   ├── services/
│   │   │   ├── historial_service.py    # CU-09
│   │   │   └── reportes_service.py     # CU-10
│   │   └── __init__.py
│   │
│   ├── eps_arl/                   # App: Gestión de entidades externas
│   │   ├── models.py
│   │   ├── views.py
│   │   ├── services.py
│   │   └── __init__.py
│   │
│   └── auditorias/                # App: Auditoría y logs
│       ├── models.py
│       ├── views.py
│       ├── services.py
│       └── __init__.py
│
├── core/                           # Utilitarios compartidos
│   ├── __init__.py
│   ├── exceptions.py              # Excepciones personalizadas
│   ├── enums.py                   # Estados, tipos, etc
│   ├── decorators.py              # Decoradores
│   ├── permissions.py             # Permisos personalizados
│   ├── pagination.py              # Paginación
│   ├── filters.py                 # Filtros comunes
│   └── constants.py               # Constantes
│
├── shared/                        # Código compartido
│   ├── __init__.py
│   ├── utils/
│   │   ├── __init__.py
│   │   ├── validators.py         # Validadores (CIE-10, fechas, etc)
│   │   ├── formatters.py         # Formatos (dinero, fechas, etc)
│   │   ├── calculators.py        # Cálculos (días, valores, etc)
│   │   └── helpers.py
│   ├── interfaces/
│   │   ├── __init__.py
│   │   └── repository_interface.py  # Interfaces comunes
│   └── dto/                       # Data Transfer Objects
│       ├── __init__.py
│       ├── incapacidad_dto.py
│       └── usuario_dto.py
│
├── static/                        # Archivos estáticos (CSS, JS)
│   ├── css/
│   ├── js/
│   └── images/
│
├── templates/                     # Templates HTML
│   ├── base.html
│   ├── dashboard.html
│   └── ...
│
├── tests/                         # Tests globales
│   ├── __init__.py
│   ├── conftest.py               # Fixtures pytest
│   ├── factories.py              # Factory Boy
│   └── test_integration.py
│
├── docs/                          # Documentación
│   ├── api/
│   │   └── openapi.yaml         # Especificación OpenAPI
│   ├── architecture/
│   ├── diagrams/                 # Diagramas UML
│   └── README.md
│
└── docker/                        # Configuración Docker
    ├── Dockerfile
    ├── docker-compose.yml
    └── .dockerignore
```

---

## 🔄 Flujo de Datos

### Ejemplo: Registrar Incapacidad (CU-01)

```
1. PRESENTACIÓN (Request)
   ┌────────────────────────────────┐
   │ POST /api/incapacidades/       │
   │ {                              │
   │   "numero": "INC-001",         │
   │   "colaborador_id": 123,       │
   │   "fecha_inicio": "2025-04-20" │
   │ }                              │
   └────────────┬───────────────────┘
                │
2. VIEW LAYER
   ┌────────────▼────────────────────┐
   │ incapacidades/views.py          │
   │ - RegistroIncapacidadView       │
   │ - Valida request                │
   │ - Deserializa datos             │
   └────────────┬────────────────────┘
                │
3. SERIALIZER (DRF)
   ┌────────────▼────────────────────┐
   │ incapacidades/serializers.py    │
   │ - RegistroIncapacidadSerializer │
   │ - Valida datos entrada          │
   │ - Transforma a DTO              │
   └────────────┬────────────────────┘
                │
4. SERVICE LAYER (Lógica de Negocio)
   ┌────────────▼────────────────────┐
   │ incapacidades/services/         │
   │ registro_service.py             │
   │ - RegistroIncapacidadService    │
   │ - Validaciones complejas        │
   │ - Cálculos                      │
   │ - Orquestación                  │
   └────────────┬────────────────────┘
                │
5. REPOSITORY LAYER (Acceso a datos)
   ┌────────────▼────────────────────┐
   │ incapacidades/repositories/     │
   │ IncapacidadRepository           │
   │ - Crea registro                 │
   │ - Guarda en BD                  │
   │ - Retorna entidad               │
   └────────────┬────────────────────┘
                │
6. DATABASE
   ┌────────────▼────────────────────┐
   │ PostgreSQL                      │
   │ INSERT INTO incapacidades ...   │
   └────────────┬────────────────────┘
                │
7. RESPONSE (JSON)
   ┌────────────▼────────────────────┐
   │ {                               │
   │   "id": 1,                      │
   │   "numero": "INC-001",          │
   │   "estado": "Registrada",       │
   │   "fecha_creacion": "2025-04-20"│
   │ }                               │
   └────────────────────────────────┘
```

---

## 🎯 Patrones de Diseño

### 1. **Repository Pattern**
Abstrae acceso a datos

```python
# repositories.py
class IncapacidadRepository:
    def crear(self, data):
        return Incapacidad.objects.create(**data)
    
    def obtener_por_id(self, id):
        return Incapacidad.objects.get(id=id)
    
    def obtener_por_estado(self, estado):
        return Incapacidad.objects.filter(estado=estado)
```

### 2. **Service Layer Pattern**
Encapsula lógica de negocio

```python
# services/registro_service.py
class RegistroIncapacidadService:
    def __init__(self, repository, validador):
        self.repository = repository
        self.validador = validador
    
    def registrar(self, data):
        # Validar
        self.validador.validar(data)
        
        # Crear
        incapacidad = self.repository.crear(data)
        
        # Registrar auditoría
        self._registrar_auditoria(incapacidad)
        
        return incapacidad
```

### 3. **Dependency Injection**
Inyectar dependencias (loose coupling)

```python
# views.py
class RegistroIncapacidadView(APIView):
    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.repository = IncapacidadRepository()
        self.service = RegistroIncapacidadService(self.repository)
    
    def post(self, request):
        serializer = RegistroSerializer(data=request.data)
        if serializer.is_valid():
            incapacidad = self.service.registrar(serializer.validated_data)
            return Response(RegistroSerializer(incapacidad).data)
```

### 4. **Strategy Pattern**
Para manejar diferentes tipos de incapacidades

```python
# services/
class EstrategiaCobroEPS:
    def calcular_valor(self, incapacidad):
        return incapacidad.dias * (incapacidad.salario / 30) * 0.6667

class EstrategiaCobroARL:
    def calcular_valor(self, incapacidad):
        return incapacidad.dias * (incapacidad.salario / 30) * 1.0
```

### 5. **Factory Pattern**
Para crear servicios

```python
# services/factory.py
class ServicioFactory:
    @staticmethod
    def crear_servicio_cobro(tipo):
        if tipo == "EPS":
            return EstrategiaCobroEPS()
        elif tipo == "ARL":
            return EstrategiaCobroARL()
```

---

## 💻 Stack Tecnológico Recomendado

### Backend
```yaml
Framework:
  - Django 4.2+ (LTS)
  - Django REST Framework 3.14+
  - djangorestframework-simplejwt (Autenticación)

Base de Datos:
  - PostgreSQL 14+ (Producción)
  - SQLite (Desarrollo)

Utilidades:
  - Celery + Redis (Tareas asincrónicas)
  - Dramatiq (Alternativa a Celery)
  - django-filter (Filtrado de querysets)
  - django-cors-headers (CORS)
  - python-decouple (Variables de entorno)
  - Pillow (Procesamiento de imágenes)
  - reportlab (PDFs)

Testing:
  - pytest + pytest-django
  - factory-boy (Fixtures)
  - faker (Datos fake)
  - coverage (Cobertura de tests)

Validación:
  - pydantic (Validación de datos)
  - django-extensions (Comandos útiles)
  - black (Code formatting)
  - flake8 (Linting)
  - mypy (Type checking)

Documentación:
  - drf-spectacular (OpenAPI/Swagger)
  - mkdocs (Documentación)
```

### Frontend
```yaml
Framework:
  - React 18+ o Vue 3+ (Para futuro)
  - Django Templates (Para MVP)

Herramientas:
  - Bootstrap 5 o Tailwind CSS
  - HTMX (Para interactividad sin JS)
  - Webpack o Vite

Gestión de Estado:
  - Redux o Pinia
```

### DevOps
```yaml
Containerización:
  - Docker
  - Docker Compose

CI/CD:
  - GitHub Actions
  - GitLab CI

Monitoreo:
  - Sentry (Error tracking)
  - New Relic o Datadog
  - ELK Stack (Logs)

Deployment:
  - AWS EC2 o RDS
  - DigitalOcean
  - Azure
  - Heroku (MVP)
```

---

## 🚀 Implementación Step-by-Step

### Fase 1: Setup Inicial (Semana 1)

```bash
# 1. Crear proyecto Django
django-admin startproject config .

# 2. Crear apps
python manage.py startapp usuarios
python manage.py startapp incapacidades
python manage.py startapp consultas
python manage.py startapp eps_arl
python manage.py startapp auditorias

# 3. Estructura de carpetas
mkdir -p apps/{usuarios,incapacidades,consultas,eps_arl,auditorias}
mkdir -p core shared/utils shared/interfaces shared/dto
mkdir -p tests docs

# 4. Instalar dependencias
pip install -r requirements.txt

# 5. Configurar settings
cp config/settings.py config/settings/base.py
```

### Fase 2: Modelos Base (Semana 1-2)

```python
# apps/incapacidades/models.py

from django.db import models

class EstadoIncapacidad(models.TextChoices):
    REGISTRADA = 'registrada', 'Registrada'
    EN_VALIDACION = 'en_validacion', 'En Validación'
    TRANSCRITA = 'transcrita', 'Transcrita'
    RADICADA = 'radicada', 'Radicada'
    EN_REVISION_EPS = 'en_revision_eps', 'En Revisión EPS'
    APROBADA = 'aprobada', 'Aprobada'
    RECHAZADA = 'rechazada', 'Rechazada'
    EN_COBRO = 'en_cobro', 'En Cobro'
    PAGADA = 'pagada', 'Pagada'
    EN_CONCILIACION = 'en_conciliacion', 'En Conciliación'
    COBRO_JURIDICO = 'cobro_juridico', 'Cobro Jurídico'

class Incapacidad(models.Model):
    numero = models.CharField(max_length=50, unique=True)
    colaborador = models.ForeignKey(Usuario, on_delete=models.PROTECT)
    estado = models.CharField(
        max_length=20,
        choices=EstadoIncapacidad.choices,
        default=EstadoIncapacidad.REGISTRADA
    )
    fecha_inicio = models.DateField()
    fecha_fin = models.DateField()
    diagnostico_cie10 = models.CharField(max_length=10)
    valor_cobrado = models.DecimalField(max_digits=12, decimal_places=2, null=True)
    valor_pagado = models.DecimalField(max_digits=12, decimal_places=2, null=True)
    creada_en = models.DateTimeField(auto_now_add=True)
    actualizada_en = models.DateTimeField(auto_now=True)
    
    class Meta:
        ordering = ['-creada_en']
        indexes = [
            models.Index(fields=['estado']),
            models.Index(fields=['colaborador']),
        ]

class HistorialEstado(models.Model):
    incapacidad = models.ForeignKey(Incapacidad, on_delete=models.CASCADE)
    estado_anterior = models.CharField(max_length=20, choices=EstadoIncapacidad.choices)
    estado_nuevo = models.CharField(max_length=20, choices=EstadoIncapacidad.choices)
    fecha_cambio = models.DateTimeField(auto_now_add=True)
    usuario = models.ForeignKey(Usuario, on_delete=models.SET_NULL, null=True)
    justificacion = models.TextField(blank=True)
```

### Fase 3: Services (Semana 2-3)

```python
# apps/incapacidades/services/registro_service.py

class RegistroIncapacidadService:
    def __init__(self, repository, validador, auditor):
        self.repository = repository
        self.validador = validador
        self.auditor = auditor
    
    def registrar_incapacidad(self, data):
        try:
            # Validar datos
            self.validador.validar_datos_entrada(data)
            
            # Validar colaborador existe
            self.validador.validar_colaborador(data['colaborador_id'])
            
            # Validar sin duplicados
            self.validador.validar_no_duplicado(
                data['numero'],
                data['colaborador_id']
            )
            
            # Crear incapacidad
            incapacidad = self.repository.crear(data)
            
            # Registrar auditoría
            self.auditor.registrar_creacion(incapacidad)
            
            return incapacidad
            
        except ValidationError as e:
            raise IncapacidadRegistroError(str(e))
```

### Fase 4: Views/Serializers (Semana 3)

```python
# apps/incapacidades/views.py
from rest_framework import status
from rest_framework.views import APIView
from rest_framework.response import Response

class RegistroIncapacidadView(APIView):
    def post(self, request):
        serializer = RegistroIncapacidadSerializer(data=request.data)
        
        if serializer.is_valid():
            try:
                service = RegistroIncapacidadService(
                    repository=IncapacidadRepository(),
                    validador=ValidadorIncapacidad(),
                    auditor=AuditorService()
                )
                
                incapacidad = service.registrar_incapacidad(
                    serializer.validated_data
                )
                
                return Response(
                    RegistroIncapacidadSerializer(incapacidad).data,
                    status=status.HTTP_201_CREATED
                )
            except IncapacidadRegistroError as e:
                return Response(
                    {'error': str(e)},
                    status=status.HTTP_400_BAD_REQUEST
                )
        
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

# apps/incapacidades/serializers.py
from rest_framework import serializers

class RegistroIncapacidadSerializer(serializers.Serializer):
    numero = serializers.CharField(max_length=50)
    colaborador_id = serializers.IntegerField()
    fecha_inicio = serializers.DateField()
    fecha_fin = serializers.DateField()
    diagnostico_cie10 = serializers.CharField(max_length=10)
    observaciones = serializers.CharField(required=False)
```

### Fase 5: Tests (Semana 4)

```python
# tests/test_incapacidades.py
import pytest
from apps.incapacidades.services import RegistroIncapacidadService

@pytest.mark.django_db
class TestRegistroIncapacidad:
    
    def test_registrar_incapacidad_exitosa(self):
        # Arrange
        data = {
            'numero': 'INC-001',
            'colaborador_id': 1,
            'fecha_inicio': '2025-04-20',
            'fecha_fin': '2025-04-25',
            'diagnostico_cie10': 'M79.3'
        }
        
        # Act
        service = RegistroIncapacidadService(...)
        incapacidad = service.registrar_incapacidad(data)
        
        # Assert
        assert incapacidad.numero == 'INC-001'
        assert incapacidad.estado == 'registrada'
```

---

## 🔐 Consideraciones de Seguridad

### 1. Autenticación y Autorización
```python
# core/permissions.py
from rest_framework.permissions import BasePermission

class EsAuxiliarTH(BasePermission):
    def has_permission(self, request, view):
        return request.user.has_perm('incapacidades.ver_incapacidades')

# En views:
class RegistroIncapacidadView(APIView):
    permission_classes = [IsAuthenticated, EsAuxiliarTH]
```

### 2. Auditoría Completa
```python
# apps/auditorias/models.py
class Auditoria(models.Model):
    ACCIONES = [
        ('crear', 'Crear'),
        ('actualizar', 'Actualizar'),
        ('eliminar', 'Eliminar'),
    ]
    
    usuario = models.ForeignKey(Usuario, on_delete=models.SET_NULL, null=True)
    accion = models.CharField(max_length=20, choices=ACCIONES)
    tabla = models.CharField(max_length=100)
    registro_id = models.IntegerField()
    datos_antes = models.JSONField(null=True)
    datos_despues = models.JSONField()
    fecha = models.DateTimeField(auto_now_add=True)
    ip = models.GenericIPAddressField()
```

### 3. Validación de Datos
```python
# shared/utils/validators.py
from pydantic import BaseModel, validator

class IncapacidadValidator(BaseModel):
    numero: str
    fecha_inicio: date
    fecha_fin: date
    
    @validator('fecha_fin')
    def validar_fecha_fin(cls, v, values):
        if 'fecha_inicio' in values and v < values['fecha_inicio']:
            raise ValueError('fecha_fin debe ser >= fecha_inicio')
        return v
```

### 4. Encriptación de Datos Sensibles
```python
# settings.py
ENCRYPTION_ENABLED = True
SENSITIVE_FIELDS = ['identificacion', 'email', 'telefono']

# En modelos:
from cryptography.fernet import Fernet

class Usuario(models.Model):
    identificacion = models.CharField(max_length=20)
    
    def save(self, *args, **kwargs):
        if ENCRYPTION_ENABLED:
            cipher = Fernet(settings.ENCRYPTION_KEY)
            self.identificacion = cipher.encrypt(
                self.identificacion.encode()
            )
        super().save(*args, **kwargs)
```


---

## ✅ Checklist Inicial

- [ ] Crear proyecto Django
- [ ] Configurar PostgreSQL
- [ ] Crear apps por dominio
- [ ] Configurar Django REST Framework
- [ ] Crear modelos base
- [ ] Implementar autenticación JWT
- [ ] Crear servicios para CU-01
- [ ] Crear tests unitarios
- [ ] Configurar CI/CD (GitHub Actions)
- [ ] Documentar API (drf-spectacular)
- [ ] Configurar logging
- [ ] Preparar Docker/Compose

---

**Conclusión:**

Para EPROS, recomiendo **MVC + Servicios (Arquitectura en Capas)** porque:

✅ Es práctica y escalable
✅ Mantiene código limpio y testeable
✅ Permite evolucionar a Clean Architecture después
✅ Es estándar en la industria Django
✅ Maneja bien la complejidad del dominio

¿Necesitas que profundice en algún aspecto específico?
