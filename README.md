# QTurn — Sistema de turnos médicos

Plataforma full stack para gestionar turnos médicos con experiencias diferenciadas para administradores, médicos y pacientes. QTurn combina una aplicación móvil desarrollada con Expo y React Native, una API REST con Spring Boot, autenticación JWT y persistencia en PostgreSQL.

<p align="center">
  <img src="front/qturn/assets/images/logo.png" alt="Logo de QTurn" width="180" />
</p>

## Qué demuestra este proyecto

- Desarrollo mobile con React Native, Expo Router y TypeScript.
- Diseño de una API REST con Spring Boot y arquitectura por capas.
- Autenticación sin estado mediante Spring Security y JWT.
- Persistencia relacional con PostgreSQL, JPA y claves compuestas.
- Flujos diferenciados para los roles `ADMIN`, `DOCTOR` y `PATIENT`.
- Entorno local reproducible mediante Docker Compose.
- Almacenamiento seguro del token en el dispositivo con Expo SecureStore.

## Funcionalidades implementadas

### Pacientes

- Inicio de sesión y gestión de datos personales.
- Consulta de horarios disponibles.
- Creación, visualización, reprogramación y cancelación de turnos.

### Médicos

- Consulta de la agenda diaria de turnos.
- Configuración y administración de horarios de atención.
- Acceso a la información de turnos y pacientes.

### Administradores

- Listado, creación, edición y eliminación de usuarios.
- Gestión de usuarios y roles desde la aplicación móvil.

### Plataforma

- Autenticación JWT con tokens de una hora de duración.
- Rutas protegidas y tratamiento centralizado de errores de autenticación.
- Persistencia de usuarios, horarios y turnos en PostgreSQL.
- Compatibilidad con temas claro y oscuro en la aplicación móvil.

## Arquitectura

```text
Aplicación Expo / React Native
              |
              | HTTPS + JSON + Bearer JWT
              v
       API REST Spring Boot
              |
              | Spring Data JPA
              v
          PostgreSQL
```

El repositorio utiliza una estructura monorepo:

```text
.
├── back/qturn/       # API con Java 17 y Spring Boot
├── front/qturn/      # Aplicación móvil Expo y React Native
├── compose.yml       # Servicios de PostgreSQL y la API
└── .env.example      # Plantilla de configuración local
```

## Tecnologías

| Área | Tecnologías |
| --- | --- |
| Mobile | React Native, Expo, Expo Router, TypeScript |
| Backend | Java 17, Spring Boot 3, Spring Security, Spring Data JPA |
| Autenticación | JWT, BCrypt, Expo SecureStore |
| Base de datos | PostgreSQL |
| Mapeo y herramientas | MapStruct, Lombok, Maven |
| Infraestructura | Docker, Docker Compose |
| Pruebas | JUnit, Spring Boot Test, Jest, React Test Renderer |

## Ejecución local

### Requisitos

- Docker Engine con Docker Compose.
- Node.js 20 o superior y npm.
- Expo Go, un emulador Android/iOS o un navegador web.

### 1. Iniciar PostgreSQL y la API

```bash
cp .env.example .env
```

Modificar `POSTGRES_PASSWORD` y asignar a `JWT_SECRET` un valor aleatorio de al menos 32 caracteres. Luego ejecutar:

```bash
docker compose up --build
```

La API estará disponible en `http://localhost:8080`. El estado del servicio puede consultarse mediante `GET /actuator/health`.

### 2. Iniciar la aplicación móvil

```bash
cd front/qturn
cp .env.example .env
npm ci
npm start
```

Configurar `EXPO_PUBLIC_API_URL` según el entorno utilizado:

- Navegador o simulador iOS: `http://localhost:8080`.
- Emulador Android: `http://10.0.2.2:8080`.
- Dispositivo físico: la dirección de red local de la computadora, por ejemplo `http://192.168.1.20:8080`.

Al utilizar Expo Go, el dispositivo y la computadora que ejecuta la API deben encontrarse en la misma red.

## Backend sin Docker

```bash
cd back/qturn
export DB_URL=jdbc:postgresql://localhost:5432/qturn_db
export DB_USERNAME=qturn
export DB_PASSWORD=tu-contraseña
export JWT_SECRET=reemplazar-por-al-menos-32-caracteres-aleatorios
./mvnw spring-boot:run
```

También pueden configurarse las variables `SERVER_PORT`, `JPA_DDL_AUTO` y `JPA_SHOW_SQL`.

## Rutas principales de la API

Todas las rutas, excepto el inicio de sesión y las comprobaciones de estado, requieren el encabezado `Authorization: Bearer <token>`.

| Método | Ruta | Descripción |
| --- | --- | --- |
| `POST` | `/login` | Autenticar un usuario y emitir un JWT. |
| `POST` | `/register` | Registrar un usuario desde un flujo autenticado. |
| `GET/PUT/DELETE` | `/users/...` | Administrar usuarios. |
| `POST` | `/appointments` | Crear un turno. |
| `GET` | `/appointments/available-times` | Consultar horarios disponibles. |
| `GET` | `/appointments/doctor/{doctorId}/appointments` | Consultar la agenda diaria de un médico. |
| `PUT/DELETE` | `/appointments/{appointmentId}` | Reprogramar o cancelar un turno. |
| `GET/POST/PUT/DELETE` | `/work-schedules/...` | Administrar horarios de atención. |

## Validación del proyecto

```bash
# Backend
cd back/qturn
JWT_SECRET=secreto-de-prueba-con-al-menos-32-caracteres ./mvnw test

# Aplicación móvil
cd front/qturn
npm ci
npx tsc --noEmit
npm test
```

## Estado y próximos pasos

QTurn cuenta con los flujos principales de usuarios, autenticación, turnos y horarios de atención. Para avanzar hacia un entorno productivo, las siguientes mejoras recomendadas son:

- reforzar la autorización por rol y la propiedad de cada recurso;
- ampliar la cobertura de pruebas de integración y extremo a extremo;
- incorporar migraciones de base de datos en lugar de actualización automática del esquema;
- completar los módulos de lista de espera, historial de turnos y notificaciones;
- agregar observabilidad, limitación de solicitudes y un perfil de despliegue productivo.

El código actual no incluye credenciales productivas ni contraseñas de demostración. Si una versión anterior del proyecto fue compartida con credenciales locales, deben rotarse antes de volver a utilizarlas.

## Autor

Desarrollado por **Emiliano Muñoz**.
