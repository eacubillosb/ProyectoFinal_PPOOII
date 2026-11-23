# Proyecto Final PPOOII: plataforma de vehículos, conductores y trayectos

API REST en **Spring Boot** con un frontend en **React** para gestionar vehículos, sus
documentos, conductores y trayectos en Ibagué (Tolima). El proyecto se construyó en cuatro
entregas de clase:

| Entrega | Qué incluye |
|---|---|
| 1 | CRUD de vehículos y documentos, restricciones `CHECK` y consultas de vehículos |
| 2 | Personas y usuarios, seguridad con **JWT + API key**, carga de documentos PDF (Base64) y servicios públicos |
| 3 | Trayectos con paradas, **tareas programadas** (`@Scheduled`), geocodificación con Google Maps y mapa con Leaflet |
| 4 | Cargue masivo de trayectos desde **Excel** con procedimientos almacenados (`trayecto_tmp`) |

También incluye documentación con **Swagger** y alertas con **RabbitMQ**.

## Estructura

```
ProyectoFinal_PPOOII/
├── proyecto/      # Backend: Spring Boot 4 (Java 17, Maven)
│   ├── src/                                   # Código fuente y pruebas
│   ├── PPOOII_Proyecto.sql                    # Base de datos (MySQL / MariaDB)
│   ├── Plantilla_Cargue_Trayectos_Ibague.xlsx # Plantilla para el cargue masivo (Entrega 4)
│   ├── Coleccion_Postman_Proyecto_E1_a_E4.postman_collection.json
│   └── Bateria_Completa_Pruebas_Postman_E1_a_E4.md
└── frontend/      # Frontend: React 18 + Vite + Tailwind
```

## Requisitos

- **Java 17** o superior (Maven no hace falta instalarlo: el proyecto trae `mvnw`).
- **MySQL o MariaDB** (por ejemplo, con XAMPP).
- **Node.js** (versión LTS) para el frontend.
- **RabbitMQ** (opcional): solo para el módulo de alertas. Sin él, la aplicación arranca pero las alertas no se envían.

## Cómo ejecutarlo

### 1. Base de datos

1. Inicia MySQL desde XAMPP.
2. En phpMyAdmin, crea la base de datos `PPOOII_Proyecto` e importa `proyecto/PPOOII_Proyecto.sql`.

La conexión está en `proyecto/src/main/resources/application.properties` (usuario `root` sin contraseña, el valor por defecto de XAMPP).

### 2. Backend (puerto 9001)

En Windows:

```powershell
cd proyecto
.\mvnw.cmd spring-boot:run
```

En macOS o Linux: `./mvnw spring-boot:run`.

- API: `http://localhost:9001`
- Documentación Swagger: `http://localhost:9001/swagger-ui.html`

### 3. Frontend (puerto 5173)

```bash
cd frontend
npm install
npm run dev
```

Abre `http://localhost:5173`. El backend tiene que estar corriendo.

`frontend/build_standalone.py` genera una versión del frontend en un solo archivo HTML y la copia en `proyecto/src/main/resources/static/`, para que el backend también la sirva en `http://localhost:9001`.

## Clave de Google Maps

La clave de Google Maps **no está en el repositorio**. Para usar la geocodificación con Google, crea la variable de entorno `GOOGLE_MAPS_API_KEY` antes de iniciar el backend:

```powershell
$env:GOOGLE_MAPS_API_KEY = "tu-clave"
.\mvnw.cmd spring-boot:run
```

Sin la clave, el proyecto usa su geocodificador de respaldo (OpenStreetMap Nominatim y coordenadas de Ibagué) y todo lo demás funciona igual.

## Credenciales de prueba

La base de datos crea un usuario administrador **solo para pruebas locales**:

| Campo | Valor |
|---|---|
| Login | `cg1001234567` |
| Contraseña | `Admin123*` |
| API key | `api-key-admin-token-secret-12345` |

Son datos de demostración que también usan el frontend y la colección de Postman. No los uses si despliegas el proyecto en un servidor real.

## Pruebas

- **Postman:** importa `proyecto/Coleccion_Postman_Proyecto_E1_a_E4.postman_collection.json`. La guía de cada prueba está en `proyecto/Bateria_Completa_Pruebas_Postman_E1_a_E4.md`.
- **JUnit:** con MySQL encendido, `cd proyecto` y luego `.\mvnw.cmd test`.

## Créditos

Proyecto final de la asignatura PPOOII, desarrollado en equipo:

- Erick Cubillos
- Alejandro Morales
- Miguel 
