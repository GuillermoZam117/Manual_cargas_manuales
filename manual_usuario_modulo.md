# Manual de Usuario: Sistema de Administración - Sustentabilidad .NET 8

Este documento detalla el uso de las funcionalidades implementadas hasta la fecha en el sistema web del proyecto "Sustentabilidad - .NET 8 Cargas Manuales + Guías Estándar". Abarca desde el acceso al sistema hasta la administración de parámetros por niveles.

---

## 1. Inicio de sesión

### Pantalla de login
- El usuario accede al sistema mediante una pantalla que solicita:
  - **Usuario** (nombre de usuario)
  - **Contraseña**
- Si los datos son correctos, el sistema genera un JWT y redirige al dashboard.
- Si son incorrectos, se muestra un mensaje de error.
- El backend valida contra la base de datos y las contraseñas están encriptadas.

---

## 2. Gestión de Usuarios

### Pantalla principal
- Muestra el listado de usuarios existentes.
- Acciones disponibles:
  - **Agregar usuario**: abre un modal con campos requeridos.
  - **Editar usuario**: rellena los campos del modal con los valores actuales.
  - **Eliminar usuario**: solicita confirmación.

### Modal de alta/edición
- Campos:
  - Nombre de usuario
  - Correo
  - Rol
  - Contraseña
  - Confirmación de contraseña

---

## 3. Gestión de Clientes

### Pantalla de clientes
- Tabla con clientes actuales (Nombre, División, Teléfono).
- Acciones:
  - **Agregar/Editar**: abre modal con validación.
  - **Eliminar**: sólo si no hay dependencias.

### Relación con otros módulos
- Al crear un cliente nuevo, se insertan automáticamente sus parámetros predeterminados.

---

## 4. Catálogos Globales

Estos se gestionan mediante tablas editables con acciones CRUD. Algunos ejemplos:

### Ejemplo: WMS - Unidad de Medida
- Campos: Código, Descripción
- Acciones:
  - **Agregar/Editar**: abre modal para captura.
  - **Eliminar**: elimina el registro si no hay uso.

### Pantalla principal
- Selección de sistema (WMS/TMS).
- Selección de catálogo.
- Grid con filtros y paginación.

---

## 5. Catálogos TMS

Cada catálogo incluye filtros específicos y su relación con cliente y sitio/depositario.

### Ejemplos:

#### TMS - Country Code
- Campos: Country Code, TMS Code, Descripción
- Operaciones CRUD mediante modales.

#### TMS - Document Type
- Campos: Document Code, Descripción

#### TMS - Unit Measure
- Filtros: Cliente, Site Depositor
- Campos: Operation Number, Customer Code, Company Code, Unit Code, Peso, Volumen, Moneda

#### TMS - Status Code (por tipo)
- Cada uno se asocia con cliente + sitio + interfaz

---

## 6. Módulo de Parámetros

El sistema permite administrar configuraciones personalizadas a 3 niveles:

### 6.1. Global (SMTP / Correo)
- Selector: Tipo = Correo
- Se muestran parámetros como `SMTP_SERVER`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASSWORD`, etc.
- Todos son editables mediante modal.

### 6.2. Por Cliente
- Selector: Tipo = Cliente
- Filtro adicional: Cliente
- Botón "Actualizar parámetros predeterminados" para generar si están vacíos.

### 6.3. Por Interfaz (Cliente + Sitio + Interfaz)
- Selectores: Cliente, Sitio, Interfaz
- Muestra parámetros técnicos como rutas de archivos, flags de correo, listas de distribución, etc.

### Modal de edición de parámetro
- Siempre muestra:
  - Nombre del parámetro (sólo lectura)
  - Campo para capturar nuevo valor
- Al guardar, se actualiza mediante SP `spSetParameterById`

---

## Relaciones entre pantallas y flujos

- **Catálogos TMS y WMS** dependen del sistema seleccionado en la vista principal.
- **Parámetros Cliente** y **Parámetros Interfaz** requieren selección previa de cliente (y sitio/interfaz en su caso).
- La edición de cada fila en tablas se realiza mediante modales uniformes.
- Todas las tablas tienen paginación, validación de campos y controles de estado.

---

Este manual cubre hasta el momento todas las funcionalidades implementadas en el sistema y refleja las buenas prácticas de diseño modular, consistencia UX/UI y separación clara de responsabilidades entre frontend, backend y base de datos.

