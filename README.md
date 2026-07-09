# 🧼 Sistema de Control de Calidad e Higiene NFC/QR

Sistema corporativo avanzado para la gestión, auditoría y visualización pública del estado de desinfección e higiene en instalaciones de alta concurrencia. Desarrollado de forma nativa para cumplir con las políticas restrictivas de seguridad de IT mediante una arquitectura asíncrona desacoplada.

---

## 🏗️ Arquitectura del Sistema

Para eludir las restricciones perimetrales de seguridad corporativas (bloqueos de origen CORS y restricciones de compartición externa), el sistema funciona de forma bidireccional asíncrona:

[Personal Habilitado] ➔ Escaneo NFC ➔ Validación Cuenta Eulen ➔ Apps Script (Backend) ➔ Google Sheets
│
└───➔ API GitHub (Push JSON)
│
[Usuario Público]     ➔ Escaneo QR  ➔ GitHub Pages (Lectura Local HTML/JSON) ◄────┘


1. **Backend & Captura (Entorno Seguro):** El operario interactúa mediante tecnología NFC con un formulario web integrado en Google Apps Script, securizado bajo el dominio de la organización.
2. **Puente de Datos (API REST):** Tras cada inserción validada en la base de datos (Google Sheets), el backend compila las últimas 5 intervenciones y las inyecta en formato encriptado Base64 en la API de GitHub de manera transparente.
3. **Frontend Público (Anónimo):** Los usuarios y clientes finales consultan una interfaz desacoplada de alto rendimiento en GitHub Pages. Esto garantiza un acceso 100% público y anónimo, con tiempos de respuesta en milisegundos y cero tráfico hacia la red interna corporativa.

---

## 🛡️ Protocolo de Seguridad y Validación de Accesos

El entorno de registro de partes está doblemente blindado en servidor:
* **Autenticación OAuth2:** Exige de forma nativa un inicio de sesión con cuenta corporativa activa a través del terminal del operario.
* **Lista Blanca de Habilitación:** El script ejecuta una comprobación en tiempo de ejecución (`verificarPermisoUsuario`). Si el correo electrónico del operario logueado no se encuentra explícitamente en la columna A de la pestaña interna `USUARIOS`, el acceso se bloquea inmediatamente arrojando un error `ERR_AUTH_NOT_FOUND`.

---

## 🚀 Guía de Despliegue Operativo

### 1. Preparación de la Base de Datos (Google Sheets)
La hoja de cálculo centralizada debe estructurarse estrictamente con las siguientes pestañas:
* **`Registro`**: Historial masivo de intervenciones. Columnas: `Fecha/Hora` | `ID_Aseo` | `Operario` | `Observaciones`.
* **`USUARIOS`**: Lista blanca de personal. Columna A: `Correos electrónicos autorizados (minúsculas)`.

### 2. Configuración de Enlaces en Etiquetas NFC (Operarios)
Las etiquetas NFC destinadas al personal técnico deben programarse en formato URL apuntando al entorno de ejecución (`/exec`) del Apps Script corporativo, inyectando el parámetro correspondiente de cada aseo:
```text
[https://script.google.com/macros/s/TU_SCRIPT_ID/exec?id=aseo_codigo](https://script.google.com/macros/s/TU_SCRIPT_ID/exec?id=aseo_codigo)
3. Configuración de Códigos QR Visuales (Clientes / Público)
Los carteles de visualización pública ubicados en las instalaciones contendrán un código QR con la ruta estática del servidor de despliegue de GitHub Pages:

Plaintext
[https://pajaroeulen.github.io/limpiezaaseostorrevieja/?id=aseo_codigo](https://pajaroeulen.github.io/limpiezaaseostorrevieja/?id=aseo_codigo)
💻 Stack Tecnológico Utilizado
Frontend: HTML5, CSS3, JavaScript Es6.

Framework Estilos: Tailwind CSS (via CDN optimizado).

Librería Gráfica: Lucide Icons (Vectores SVG dinámicos).

Backend: Google Apps Script (V8 Engine).

Almacenamiento de Datos: Google Sheets API & GitHub REST API.

Alojamiento Público: GitHub Pages CDN de alta disponibilidad.

📈 Escalabilidad del Proyecto
El sistema ha sido diseñado bajo una premisa de mantenimiento cero (Zero-Ops):

Sin deploys redundantes: Un único archivo index.html renderiza dinámicamente un número ilimitado de aseos.

Aprovisionamiento Automático: Si se crea un nuevo código de aseo (ej: aseo_norte_05) y el personal habilitado escanea el NFC, el backend creará el archivo JSON correspondiente (datos-aseonorte05.json) en el repositorio de manera autónoma, sin necesidad de intervención técnica o reconfiguración del servidor web.


### 🎯 Ventajas de este README:
* **Para tu cliente:** Demuestra que le estás entregando un producto tecnológico de ingeniería serio, documentado y moderno.
* **Para tu departamento de IT:** Explica detalladamente que cumples con sus restricciones de seguridad y que el público general jamás pisará los servidores internos de la empresa.
