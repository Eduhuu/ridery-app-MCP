# Plan de Desarrollo para la Aplicación de Administración de Vehículos

## 1. Fase de Preparación y Setup
**Objetivo:** Sentar las bases técnicas y organizativas del proyecto.

- Inicialización del repositorio en GitHub y estructura de ramas.
- Configuración de Next.js 13+ con app directory.
- Integración inicial con Supabase (Auth y DB).
- Configuración de variables de entorno y Netlify para despliegue automático.
- Documentación inicial: README, convenciones de código y estructura de carpetas.

---

## 2. Modelado de Base de Datos y Supabase
**Objetivo:** Crear las tablas y relaciones necesarias en Supabase.

- Creación de tablas: usuarios, marcas, modelos, vehículos, historial de cambios, logs de acciones, historial de activaciones/desactivaciones.
- Definición de relaciones, restricciones (FK, índices compuestos, unicidad, cascada de desactivación).
- Pruebas de integridad y validación de reglas de negocio a nivel de base de datos.

---

## 3. Backend/API (Supabase y Next.js API routes)
**Objetivo:** Exponer y proteger los endpoints necesarios para la app.

- Configuración de Supabase Auth con JWT y 2FA.
- Middleware de autenticación en Next.js para rutas protegidas.
- Endpoints CRUD para usuarios, marcas, modelos, vehículos, historial y logs (solo admin).
- Validación de datos en endpoints, protección de rutas y roles.
- Configuración de CORS y manejo de errores.

---

## 4. Frontend (Next.js)
**Objetivo:** Construir la interfaz de usuario y la lógica de interacción.

- Página de login con Supabase Auth y 2FA, manejo de errores y carga.
- Dashboard principal: listado de vehículos (paginado, filtrado, búsqueda, exportar a Excel).
- Formularios de registro/edición y cambio de estado de vehículos.
- Gestión de marcas, modelos y usuarios (alta, edición, activación/desactivación).
- Visualización de logs y auditoría.
- Internacionalización y UI/UX responsive con librería de componentes y Tailwind CSS.

---

## 5. Testing y Calidad
**Objetivo:** Garantizar la calidad y robustez del sistema.

- Pruebas unitarias y de integración en componentes críticos.
- Pruebas de usuario para validar flujos principales.
- Revisión de seguridad: protección de rutas, roles y datos sensibles.

---

## 6. Despliegue y Entregables
**Objetivo:** Publicar la aplicación y entregar documentación.

- Deploy en Netlify y validación de entorno de producción.
- Actualización de README con instrucciones claras.
- Documentación de endpoints y modelo de datos.
- Entrega de acceso al evaluador y URL pública (opcional).

---

## 7. Mejoras y Extras (Opcional)
**Objetivo:** Agregar valor y robustez al sistema.

- Contenedores Docker para facilitar despliegue.
- Tests E2E automatizados.
- Mejoras de performance y accesibilidad.

---

## Prioridades (MVP)
1. Autenticación y gestión de usuarios.
2. Modelado de datos y CRUD de vehículos.
3. Gestión de marcas/modelos y reglas de negocio principales.
4. Dashboard y listado de vehículos.
5. Logs y auditoría.
6. Exportación a Excel.
7. Internacionalización y detalles de UI/UX. 