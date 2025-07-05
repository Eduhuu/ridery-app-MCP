# Documento de Requerimientos del Producto (PRD)

## 1. Resumen del Producto

Desarrollar una aplicación web SPA para la administración de vehículos en una empresa de ride hailing. El sistema permitirá a usuarios administradores autenticados registrar, visualizar y gestionar el estado de los vehículos, así como gestionar usuarios. La solución estará basada en Next.js para el frontend, Supabase como backend (autenticación y base de datos), y Netlify para despliegue.

## 2. Objetivos
- Permitir a administradores gestionar el registro y estado de vehículos.
- Permitir la gestión de usuarios y administradores desde un panel dedicado.
- Garantizar la seguridad y privacidad de los datos mediante autenticación robusta y 2FA.
- Ofrecer una experiencia de usuario fluida, intuitiva, responsive y multilenguaje.

## 3. Stack Tecnológico
- **Frontend:** Next.js (React, SSR/SSG, API routes)
- **Backend y Base de Datos:** Supabase (PostgreSQL, Auth, REST API)
- **Despliegue:** Netlify

## 4. Requerimientos Funcionales

### 4.1. Gestión de Usuarios
- Solo existen dos roles: **administrador** y **usuario**.
- Panel de gestión de usuarios accesible solo para administradores:
  - Listar, agregar, editar y eliminar usuarios y administradores.
  - Visualización del nombre de cada usuario.

### 4.2. Interfaz de Usuario
- **Página de Login:**
  - Autenticación de administradores vía correo y contraseña usando Supabase Auth.
  - Implementar autenticación de dos factores (2FA).
  - Manejo de errores claros (credenciales incorrectas, usuario no registrado).
  - Única página sin protección de autenticación.

- **Página Principal (Dashboard):**
  - Visualización de la lista de vehículos registrados.
  - Información mostrada: marca, modelo, año, estado (disponible, en mantenimiento, en servicio), id único, fecha de creación, última actualización, creado/actualizado por (nombre).
  - Lista paginada y ordenada por fecha de creación (más recientes primero).
  - Tamaño de página configurable, por defecto 10.
  - Mensaje amigable si no hay vehículos registrados.
  - Opciones de filtrado y búsqueda parcial (marca, modelo, año, id).
  - Filtrado combinado opcional.
  - Opción para exportar la lista de vehículos a Excel.

- **Formulario de Registro de Vehículos:**
  - Permite agregar vehículos con campos: marca, modelo, año, estado inicial.
  - Validación de campos en frontend y backend.

- **Cambio de Estado de Vehículos:**
  - Permite modificar el estado del vehículo desde la lista.
  - Confirmaciones visuales de éxito o error.
  - Historial de cambios de estado por vehículo (auditoría).

### 4.3. Backend y API
- Uso de Supabase para:
  - Autenticación JWT con expiración de 1 hora y refresh token.
  - Autenticación de dos factores (2FA) para administradores.
  - Almacenamiento y gestión de usuarios y vehículos (tablas relacionales).
  - API RESTful para operaciones CRUD sobre vehículos y usuarios.
- Protección de rutas mediante middleware de autenticación.
- Validación de datos en endpoints de creación y actualización.
- Registro de logs de acciones administrativas (creación, edición, eliminación de vehículos y usuarios, cambios de estado, etc.) en una sección dedicada.

### 4.4. Base de Datos
- **Supabase/PostgreSQL:**
  - Tabla de usuarios administradores y usuarios (con contraseñas encriptadas por Supabase Auth).
  - Tabla de vehículos con los campos requeridos y relaciones con usuarios.
  - Tabla de historial de cambios de estado de vehículos.
  - Tabla de logs de acciones.
  - Índices para búsquedas rápidas.

## 5. Requerimientos Técnicos
- **Frontend:**
  - Next.js 13+ con app directory y React Server Components.
  - Manejo eficiente del estado (React context, SWR o similar).
  - Componentes modulares y reutilizables.
  - Layouts globales (admin, login, blank, etc).
  - Manejo de estados de carga y errores.
  - Uso de librería de componentes UI (ej: Material UI, Chakra UI, etc).
  - Uso de axios o fetch para peticiones a la API de Supabase.
  - Internacionalización: la app debe ser fácilmente extensible a múltiples idiomas.

- **Backend:**
  - Supabase como backend-as-a-service (Auth, DB, API).
  - Middleware de autenticación en API routes de Next.js si se usan funciones personalizadas.
  - Configuración de CORS adecuada.

- **Despliegue:**
  - Deploy automático en Netlify (solo entorno de producción).
  - Variables de entorno seguras para claves de Supabase.

- **Estilizado:**
  - Se utilizará **Tailwind CSS** para el estilizado de la aplicación.
  - La aplicación debe contar con **modo claro** y **modo oscuro**.
  - La paleta de colores será **monocromática**, utilizando blancos, grises y negros.
  - El **color principal** será el **amarillo** y el **color secundario** será el **morado**.

## 6. Criterios de Éxito y Evaluación
- Cumplimiento de todos los requerimientos funcionales.
- Seguridad: autenticación JWT, contraseñas encriptadas, protección de rutas, 2FA.
- Código claro, modular y bien documentado.
- Interfaz intuitiva, responsive, coherente y multilenguaje.
- Instrucciones claras en README para clonar, configurar y ejecutar el proyecto.
- (Deseable) Uso de contenedores y despliegue en producción.
- Exportación de vehículos a Excel.
- Registro y visualización de logs de acciones.
- Historial de cambios de estado de vehículos.

## 7. Entregables
- Repositorio en GitHub con el código fuente.
- Archivo README.md con instrucciones de instalación y despliegue.
- Acceso al evaluador para revisión.
- (Opcional) URL de la app desplegada en Netlify.

## 8. Modelado de Datos y Reglas de Negocio

### Entidades principales

#### Usuario
- id (UUID, PK)
- nombre (string)
- email (string, único)
- telefono (string)
- estado (string, 'activo'/'inactivo')
- rol (string, 'administrador'/'usuario')
- fecha_creacion (timestamp)

#### Marca
- id (UUID, PK)
- nombre (string, único)
- is_active (boolean, default TRUE)
- fecha_creacion (timestamp)

#### Modelo
- id (UUID, PK)
- nombre (string)
- marca_id (UUID, FK -> marca.id)
- is_active (boolean, default TRUE)
- fecha_creacion (timestamp)
- Restricción: el nombre del modelo debe ser único por marca (índice compuesto: nombre + marca_id)

#### Vehículo
- id (UUID, PK)
- marca_id (UUID, FK -> marca.id)
- modelo_id (UUID, FK -> modelo.id)
- año (integer)
- estado (string: 'disponible', 'en mantenimiento', 'en servicio')
- creado_por (UUID, FK -> usuario.id)
- actualizado_por (UUID, FK -> usuario.id)
- fecha_creacion (timestamp)
- fecha_actualizacion (timestamp)
- is_active (boolean, default TRUE)

#### Historial de Cambios de Estado de Vehículo
- id (UUID, PK)
- vehiculo_id (UUID, FK -> vehiculo.id)
- estado_anterior (string)
- estado_nuevo (string)
- usuario_id (UUID, FK -> usuario.id)
- fecha_cambio (timestamp)

#### Log de Acciones
- id (UUID, PK)
- usuario_id (UUID, FK -> usuario.id)
- accion (string: login, logout, creación/edición/eliminación de usuario, creación/edición/eliminación de vehículo, cambio de estado, exportación)
- entidad (string: usuario, vehiculo, marca, modelo, etc.)
- entidad_id (UUID, opcional)
- fecha_hora (timestamp)

#### Historial de Activaciones/Desactivaciones
- id (UUID, PK)
- entidad (string: 'marca', 'modelo', 'vehiculo')
- entidad_id (UUID)
- is_active_anterior (boolean)
- is_active_nuevo (boolean)
- usuario_id (UUID, FK -> usuario.id)
- fecha_cambio (timestamp)

### Reglas de Negocio
- Solo los administradores pueden crear, editar, activar/desactivar marcas, modelos y vehículos.
- Al desactivar una marca o modelo, todos los vehículos asociados deben desactivarse automáticamente (cascada).
- Se debe permitir reactivar marcas, modelos y vehículos en cualquier momento.
- Se debe guardar un historial de activaciones/desactivaciones para auditoría.
- No se pueden registrar, listar ni editar vehículos asociados a marcas/modelos inactivos.
- El nombre del modelo puede repetirse en diferentes marcas, pero no dentro de la misma marca. 