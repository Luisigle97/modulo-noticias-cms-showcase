# 📰 Módulo Editorial y CMS de Noticias

> Sistema integral de gestión y publicación editorial, hemeroteca interactiva y panel administrativo de alta seguridad, desarrollado por **[LoNet Solutions](https://lonetsolutions.com)**. Construido con **Node.js nativo (Zero External Dependencies)**, arquitectura modular, control de acceso basado en roles (**RBAC**), modo pánico/mantenimiento preventivo, módulo de auditoría histórica, empaquetado de producción con **Terser** y blindaje de código fuente contra ingeniería inversa y robo de propiedad intelectual.

---

## 🌟 Características Principales

### 🛡️ Seguridad Criptográfica y Blindaje Multicapa
- **Zero-Dependencies en Ejecución:** Corre de forma nativa en Node.js usando módulos del núcleo (`http`, `fs`, `path`, `crypto`, `url`).
- **Almacenamiento Criptográfico:** Hasheo de contraseñas mediante **PBKDF2 + SHA-512** con salt aleatorio de 128 bits (100.000 iteraciones).
- **Tokens de Sesión Opacos (256 bits CSPRNG):** Tokens criptográficamente seguros con validación estricta en servidor.
- **Expiración Automática por Inactividad (30 Minutos):** Monitoreo activo de tiempo de inactividad con temporizador visible, advertencia modal anticipada e invalidación inmediata de tokens en memoria y cookies.
- **Protección de Archivos y Blindaje `.htaccess`:** Bloqueo absoluto de descargas directas vía HTTP a archivos de base de datos `.json`, variables `.env`, repositorios `.git`, volcados `.sql` y archivos de servidor.
- **Prevención de Ataques Web:** Sanitización anti *Path Traversal*, validación de tipos MIME y prevención de inyecciones de código.

### 👥 Control de Acceso Basado en Roles (RBAC) con Jerarquía Protegida
- **Super Administrador (LoNet Solutions):**
  - Control maestro sobre la plataforma y base de datos.
  - Blindaje institucional: Ningún otro rol puede editar, desactivar o eliminar la cuenta de Super Administrador.
  - Generador y asignador de contraseñas directas con copiado al portapapeles y despacho de credenciales al correo institucional.
  - Panel de auditoría global con visión de todas las acciones del sistema.
- **Administrador:**
  - Gestión integral de noticias, categorías y autores (docentes y estudiantes).
  - Acceso al panel de auditoría institucional (con restricción automática para no visualizar acciones del Super Administrador).
  - Activación/desactivación de autores y reenvío de enlaces de verificación.
- **Docente / Periodista / Autor Senior:**
  - Redacción, edición y publicación inmediata de sus propios comunicados y artículos.
  - Gestión de fotos de portada, galerías fotográficas múltiples y videos.
  - Moderación pedagógica de publicaciones enviadas por estudiantes.
- **Estudiante Autor:**
  - Redacción escolar colaborativa con envío automático a revisión pedagógica (*Borrador / Por Aprobar*).

### 🚨 Protocolo de Modo Pánico y Mantenimiento Preventivo
- Interruptor de emergencia accesible para el **Super Administrador** y **Administrador**.
- Al activarse:
  - **Super Administrador y Administrador:** Mantienen capacidad completa de edición, corrección y publicación.
  - **Docentes, Autores y Estudiantes:** El panel conmuta automáticamente a **Modo Solo Lectura**, impidiendo cualquier creación, modificación o borrado de noticias.
  - **Portal Público:** Despliega de inmediato una franja institucional de mantenimiento preventivo y desactiva la interacción de comentarios o envíos externos.

### 📋 Módulo de Control de Auditoría e Historial de Actividad
- Registro histórico persistente (`data/activity_logs.json`) de todas las operaciones clave:
  - Inicios de sesión (exitosos y fallidos).
  - Creación, edición, eliminación y cambio de estado de noticias.
  - Modificación de usuarios, asignación de contraseñas y activaciones.
  - Activación y desactivación del Modo Pánico.
- **Filtros Inteligentes:** Búsqueda por palabra clave, filtro por acción (*Login, News, Users, Panic, Security*) y selector por rol.
- **Blindaje de Privacidad:** El rol Administrador no puede visualizar los registros correspondientes al Super Administrador.

### 📱 Experiencia Responsive y Móvil
- **Login Centrado en Viewport Dinámico:** Tarjeta de inicio de sesión (`.login-card`) perfectamente centrada vertical y horizontalmente en teléfonos y tablets mediante `100dvh` y Flexbox adaptativo.
- **Prevención de Zoom en iOS/Android:** Tamaños tipográficos ajustados a 16px en campos interactivos para evitar zooms intrusivos del navegador al escribir.
- **Hemeroteca Dinámica:** Buscador predictivo en vivo, cintillo de *Última Hora (Breaking News)* y selector de categorías (*pills*) sin scrolls forzados.

### 🌙 Modo Oscuro / Noche (Dark Mode) Integrado
- **Alternancia Fluida en Tiempo Real:** Botón con selector iconográfico (`🌙 Modo Oscuro` / `☀️ Modo Claro`) en la pantalla de inicio de sesión (`#btnLoginThemeToggle`), barra superior del panel administrativo (`#btnThemeToggle`) y barra de navegación de la hemeroteca (`#btnArchiveThemeToggle`).
- **Detección Automática & Zero-FOUC:** Identificación inmediata de la preferencia del sistema operativo (`prefers-color-scheme: dark`) con ejecución anti-parpadeo instantánea en el `<head>`.
- **Persistencia en LocalStorage:** Almacenamiento local transparente bajo la clave `nieto_editorial_theme` que mantiene la coherencia entre el panel de administración, el login y la hemeroteca pública.
- **Paleta Cromática Ergonómica:** Diseñada para largas jornadas de redacción y lectura periodística, con fondos profundos (`#0B1120`, `#141E30`), modales en `#1E293B`, contrastes accesibles y bordes sutiles en `#334155`.

### 🗑️ Papelera de Reciclaje de Noticias con Retención de 30 Días
- **Borrado Seguro (Soft-Delete):** Al eliminar un artículo, este se traslada a la papelera segura (`data/news-trash.json`) registrando fecha y usuario responsable.
- **Restauración con 1 Clic (`POST /api/news/trash/:id/restore`):** Recupera la noticia intacta al panel con sus imágenes y galerías.
- **Indicador Dinámico de Días Restantes:** Insignia que calcula el tiempo restante (`⏳ Quedan X días`).
- **Eliminación Definitiva & Vaciado:** Opciones de eliminación permanente individual o vaciado completo de papelera para administradores.
- **Purga Autónoma del Servidor:** Algoritmo que elimina irreversiblemente cualquier noticia que supere los 30 días de antigüedad sin requerir tareas manuales.

---

## 📂 Estructura del Proyecto

```text
modulo-noticias-cms/
├── server.js                   # Servidor HTTP nativo y API REST completa
├── package.json                # Metadatos, dependencias de desarrollo y scripts
├── package-lock.json           # Árbol de dependencias bloqueado
├── .gitignore                  # Exclusiones de Git (node_modules, logs, temporales)
├── .htaccess                   # Reglas de endurecimiento y seguridad para Apache/LiteSpeed
├── LICENSE                     # Licencia de Propietario LoNet Solutions (All Rights Reserved)
├── README.md                   # Documentación técnica y guía de despliegue seguro
├── scripts/
│   ├── build.js                # Compilador con Terser (3-pass mangling, 0 comentarios)
│   └── clean_html_comments.js  # Purga de comentarios HTML en vistas y modales
├── admin/                      # Panel Editorial y Administrativo (/admin)
│   ├── index.html              # Interfaz limpia de comentarios con carga de scripts ofuscados
│   ├── admin.css               # Estilos responsivos con centrado móvil/tablet
│   ├── admin.js                # ✏️ Código fuente base del panel (legible y comentado)
│   └── admin.min.js            # 🛡️ Archivo de producción (ofuscado, minificado, 0 comentarios)
├── assets/                     # Recursos estáticos del portal público
│   ├── js/
│   │   ├── news-archive.js     # ✏️ Código fuente hemeroteca (legible)
│   │   ├── news-archive.min.js # 🛡️ Producción ofuscada
│   │   ├── news-client.js      # ✏️ Código fuente cliente web (legible)
│   │   ├── news-client.min.js  # 🛡️ Producción ofuscada
│   │   ├── main.js             # ✏️ Lógica general del portal
│   │   └── main.min.js         # 🛡️ Producción ofuscada
│   └── img/
│       └── noticias/           # Almacén de fotos, portadas y videos subidos
├── data/                       # Base de datos JSON (bloqueada contra descargas directas)
│   ├── news.json               # Artículos y comunicados publicados
│   ├── users.json              # Cuentas de usuarios y hashes PBKDF2
│   ├── activity_logs.json      # Registros de auditoría histórica
│   ├── sessions.json           # Sesiones activas (CSPRNG)
│   ├── password-resets.json    # Códigos temporales de restablecimiento
│   └── system_settings.json    # Estado de mantenimiento y pánico
├── index.html                  # Portal público principal
└── noticias.html               # Hemeroteca pública institucional
```

---

## 🚀 Inicio Rápido en Desarrollo Local

### Requisitos
- **Node.js** v16.0.0 o superior instalado en el equipo.

### Ejecución
1. Abre una terminal en la carpeta del proyecto:
   ```bash
   npm start
   ```
   *(O si prefieres depuración con auto-recarga: `npm run dev`)*
2. Accede en el navegador a:
   - **Portal Público:** [http://localhost:3000/](http://localhost:3000/)
   - **Panel Administrativo:** [http://localhost:3000/admin/](http://localhost:3000/admin/)

---

## 🔐 Credenciales Iniciales de Acceso

| Rol | Correo Electrónico | Contraseña | Capacidades |
| :--- | :--- | :--- | :--- |
| **Super Administrador** | `solutionslonet@gmail.com` | Configurada en servidor | Control maestro blindado, auditoría total, contraseñas directas |
| **Administrador** | `admin@cmsnoticias.com` | `Admin123*` | Gestión de noticias, autores y auditoría acotada |
| **Docente Autor** | `autor@cmsnoticias.com` | `Autor123*` | Publicar artículos, fotos, videos y moderación escolar |
| **Estudiante** | `estudiante@cmsnoticias.com` | `Estudiante123*` | Redacción de artículos en borrador para revisión pedagógica |

> [!IMPORTANT]
> Cambia siempre las contraseñas al realizar el primer despliegue a través del panel de administración.

---

## 🛡️ GUÍA PASO A PASO: CÓMO DESPLEGAR CON PROTECCIÓN TOTAL DEL CÓDIGO FUENTE

Para garantizar que nadie pueda copiar, robar o interpretar el código de tu plataforma en internet, sigue estrictamente este procedimiento de despliegue profesional:

### Paso 1: Programa siempre en los archivos fuente originales
En tu equipo de desarrollo local, trabajas con total comodidad en los archivos `.js` legibles, documentados y formateados:
- `admin/admin.js`
- `assets/js/news-archive.js`
- `assets/js/news-client.js`
- `assets/js/main.js`

Estos archivos se mantienen en tu repositorio privado de Git para tu trabajo continuo.

### Paso 2: Ejecuta la compilación y ofuscación automática
Antes de subir cualquier cambio a producción, ejecuta en tu terminal:
```bash
npm run build
```
Este comando (`node scripts/build.js`) ejecuta el motor **Terser** con las siguientes directivas de máxima seguridad:
1. **Compresión en 3 pasadas (`passes: 3`):** Reduce la lógica a operaciones binarias y operadores ternarios compactos.
2. **Mangling agresivo:** Renombra todas las funciones y variables internas a letras sin sentido (`e`, `t`, `n`, `a`, `o`, `i`...).
3. **Erradicación de comentarios (`format: { comments: false }`):** Elimina el 100% de los comentarios de línea (`//`), comentarios de bloque (`/* */`), docstrings y cabeceras.
4. **Eliminación de código muerto y debuggers:** Remueve instrucciones `debugger` y sentencias inalcanzables.
5. **Genera los archivos protegidos:**
   - `admin/admin.min.js`
   - `assets/js/news-archive.min.js`
   - `assets/js/news-client.min.js`
   - `assets/js/main.min.js`

### Paso 3: Verifica que el HTML cargue únicamente las versiones `.min.js`
Los archivos HTML del proyecto están configurados para solicitar los scripts minificados:
```html
<!-- En admin/index.html -->
<script src="admin.min.js?v=20261018_26"></script>

<!-- En noticias.html -->
<script src="assets/js/news-archive.min.js?v=20261018_26"></script>

<!-- En index.html -->
<script src="assets/js/main.min.js?v=20261018_26"></script>
<script src="assets/js/news-client.min.js?v=20261018_26"></script>
```

### Paso 4: Limpieza de comentarios HTML en las vistas
Para evitar que un tercero inspeccione el código HTML en el navegador (*F12*) y descubra qué modales o vistas existen, ejecuta:
```bash
node scripts/clean_html_comments.js
```
Esto purga los 67 comentarios HTML (`<!-- Modal ... -->`), dejando la estructura del DOM limpia y opaca para curiosos.

### Paso 5: Endurecimiento del Servidor (`.htaccess`)
Asegúrate de que el archivo `.htaccess` esté presente en la raíz de tu servidor web (Hostinger, cPanel, Apache, LiteSpeed). Este archivo bloquea todo intento de descarga directa de tus datos y archivos fuente:
```apache
# Bloqueo total de bases de datos JSON, archivos de entorno y fuentes directas
<FilesMatch "\.(json|env|sql|log|md|yml|yaml|git|lock)$">
    Order allow,deny
    Deny from all
</FilesMatch>

# Proteger la descarga directa de archivos fuente sin compilar si se desea
<FilesMatch "^(admin\.js|news-archive\.js|news-client\.js)$">
    Order allow,deny
    Deny from all
</FilesMatch>

# Ocultar banners de servidor y listado de directorios
Options -Indexes
ServerSignature Off
```

### Paso 6: Matriz de Subida al Servidor de Producción (Hostinger / FTP)

| Archivo / Carpeta | ¿Se sube al servidor? | Estado / Protección |
| :--- | :---: | :--- |
| `admin/admin.min.js` | **SÍ** | 🛡️ Ofuscado y comprimido (0 comentarios) |
| `admin/admin.css` | **SÍ** | Estilos responsivos con centrado móvil/tablet |
| `admin/index.html` | **SÍ** | HTML limpio sin comentarios descriptivos |
| `admin/admin.js` | **NO** *(Opcional)* | Solo necesario en tu entorno de desarrollo local |
| `assets/js/*.min.js` | **SÍ** | 🛡️ Scripts públicos del cliente ofuscados |
| `assets/js/*.js` (originales) | **NO** *(Opcional)* | Mantener únicamente en Git |
| `data/*.json` | **SÍ** | Base de datos protegida por `.htaccess` |
| `server.js` o `api/` | **SÍ** | Motor de backend que atiende la API REST |
| `.htaccess` | **SÍ (OBLIGATORIO)** | Escudo perimetral de seguridad del servidor |
| `node_modules/` | **NO** | Solo se usa localmente para compilar con Terser |

Con este flujo, cualquier persona que haga clic en **"Inspeccionar elemento"** o **"Ver código fuente de la página"** en el navegador solo verá scripts de una sola línea incomprensibles y código HTML sin pistas internas.

---

## 🌐 Referencia de la API REST

### Públicos
- `GET /api/news`: Lista de artículos publicados (admite filtros `?category=...&search=...`).
- `GET /api/news/:id`: Detalle de una publicación con contador automático de lecturas.
- `GET /api/news/banner`: Artículos activos en el cintillo de última hora.
- `GET /api/system/maintenance`: Consulta el estado de activación del Modo Pánico.

### Autenticación y Sesiones
- `POST /api/auth/login`: Autentica credenciales, crea sesión CSPRNG de 256 bits y devuelve token.
- `POST /api/auth/logout`: Invalida inmediatamente la sesión en memoria y borra cookies.
- `GET /api/auth/me`: Retorna los datos del usuario de la sesión actual.
- `POST /api/auth/forgot-password`: Genera código numérico seguro de recuperación (expira en 15 min).
- `POST /api/auth/reset-password`: Aplica nueva contraseña tras validar código.
- `POST /api/auth/setup-password`: Activa la cuenta y asigna contraseña inicial para nuevos autores.

### Publicaciones (Requiere Cabecera `Authorization: Bearer <token>`)
- `POST /api/news`: Crea un artículo (registra autor de la sesión).
- `PUT /api/news/:id`: Modifica una publicación (valida autoría o rol administrativo).
- `DELETE /api/news/:id`: Elimina permanentemente una noticia.
- `POST /api/upload`: Recibe y sanitiza imágenes y videos en base64 para portada o galería.

### Gestión Administrativa, Auditoría y Pánico (Requiere Rol `admin` o `super_admin`)
- `GET /api/users`: Lista todos los autores y estados de verificación.
- `POST /api/users`: Registra un nuevo autor y despacha correo de bienvenida.
- `PUT /api/users/:id`: Edita rol, nombre o activa/desactiva una cuenta.
- `DELETE /api/users/:id`: Da de baja la cuenta (blindaje: no permite eliminar Super Admin).
- `POST /api/users/:id/super-password`: Asignación directa de clave por Super Admin con opción de envío por correo.
- `GET /api/audit`: Consulta registros históricos de auditoría con filtrado por rol.
- `POST /api/system/panic`: Conmuta el estado de Modo Pánico y Mantenimiento Preventivo.

---

## 🚀 Estrategia de Comercialización: Showcase y Demo en Vivo (Sandbox)

Para exhibir y comercializar este software ante potenciales clientes institucionales sin exponer el código fuente privado ni comprometer datos reales de clientes en producción, LoNet Solutions implementa una arquitectura dual:

### 1. 🐙 Repositorio Público Showcase (GitHub)
- **Repositorio:** [https://github.com/Luisigle97/modulo-noticias-cms-showcase](https://github.com/Luisigle97/modulo-noticias-cms-showcase)
- **Propósito:** Funciona como ficha técnica pública y portafolio de ingeniería de software.
- **Protección:** No contiene código ejecutable, lógica de backend ni bases de datos; únicamente diagramas de arquitectura, especificaciones de seguridad criptográfica y enlaces a demostraciones.

### 2. ⚡ Entorno Live Demo Interactivo (Sandbox en LoNet Solutions)
- **URL en Producción:** [https://lonetsolutions.com/demo/](https://lonetsolutions.com/demo/)
- **Acceso 1-Click con 4 Roles Preconfigurados:**
  - 👑 **Super Admin (`superadmin@nieto.edu.co`):** Control maestro, gestión total de noticias y usuarios, generador directo de contraseñas de docentes, interruptor de Modo Pánico y auditoría histórica completa.
  - 🛡️ **Administrador (`rectoria@nieto.edu.co`):** Gestión editorial, aprobación pedagógica de artículos, gestión de autores y auditoría institucional.
  - ✍️ **Docente Autor (`docente@nieto.edu.co`):** Redacción, edición y publicación directa con galerías fotográficas múltiples y videos de YouTube.
  - 🎓 **Estudiante Autor (`estudiante@nieto.edu.co`):** Redacción escolar colaborativa en borrador sujeta a moderación administrativa.
- **Arquitectura Sandbox (`demo.js`):** Emula todas las llamadas a la API REST (`/api/auth/login`, `/api/news`, `/api/users`, `/api/audit`, `/api/system/panic`) en el almacenamiento local del navegador (`localStorage`).
- **Seguridad y Velocidad:**
  - Latencia imperceptible ($<50\text{ms}$).
  - Cero riesgo para la base de datos real del colegio.
  - Botón integrado para **Restablecer Datos Demo** en cualquier momento.

### 3. 🌐 Sección Showcase en el Sitio Web Corporativo
- Visible en: [https://lonetsolutions.com/#showcase](https://lonetsolutions.com/#showcase) con botones de acción directa a la hemeroteca pública, el panel interactivo y el repositorio showcase.

---

## 🏢 Desarrollo y Créditos

Este sistema editorial y CMS institucional fue diseñado, construido y protegido por **[LoNet Solutions](https://lonetsolutions.com)**:

- **Empresa Desarrolladora:** [LoNet Solutions](https://lonetsolutions.com)
- **Sitio Web Oficial:** [https://lonetsolutions.com](https://lonetsolutions.com)
- **Contacto y Soporte:** `solutionslonet@gmail.com`

---

## 📄 Licencia de Propietario

Copyright (c) 2026 **[LoNet Solutions](https://lonetsolutions.com)**. Todos los derechos reservados.

Este módulo, su código fuente, arquitectura, diseño y documentación son propiedad intelectual exclusiva y confidencial de **LoNet Solutions**. Queda estrictamente prohibida la copia no autorizada, redistribución, modificación, descompilación o uso comercial sin autorización previa, expresa y por escrito de LoNet Solutions. Consulta el archivo [LICENSE](LICENSE) para los términos legales completos.
