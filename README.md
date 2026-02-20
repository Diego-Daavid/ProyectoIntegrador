[README.md](https://github.com/user-attachments/files/25447582/README.md)
# Sistema de Control de Materiales

Sistema web desarrollado con **Flask** y **SQL Server** para gestionar materiales, préstamos, devoluciones y pagos de multas dentro de un entorno académico o institucional. Incluye integración con la API de YouTube, geolocalización, clima y pagos en línea con PayPal.

---

## Descripción del Sistema

El sistema permite llevar un control completo del ciclo de vida de los materiales disponibles en un área:

- **Materiales**: Registro, consulta y control de stock de los materiales disponibles.
- **Usuarios**: Alta de usuarios con roles definidos (Alumno, Maestro, Admin).
- **Préstamos**: Registro de préstamos de materiales a usuarios, con validación de stock.
- **Devoluciones**: Registro de devoluciones que restauran el inventario automáticamente.
- **Pagos de Multas**: Cobro de multas por atraso o pérdida de material a través de PayPal.
- **Geolocalización**: Consulta de ubicación por IP o dirección, y clima actual por coordenadas.
- **Video Tutorial**: En la pantalla de materiales se muestra automáticamente un video tutorial del primer material registrado, usando la API de YouTube.

---

## Requisitos previos

Antes de instalar, asegúrate de contar con lo siguiente:

- **Python 3.10 o superior**
- **SQL Server** (Express o superior) con la instancia `localhost\SQLEXPRESS` disponible
- **ODBC Driver 17 for SQL Server** instalado ([descargar aquí](https://learn.microsoft.com/en-us/sql/connect/odbc/download-odbc-driver-for-sql-server))
- **Node.js** (opcional, solo si se desea manipular archivos `.docx`)
- Una cuenta de **PayPal Developer** para obtener credenciales sandbox o live
- Una **API Key de YouTube Data v3** (opcional, para el video tutorial)

---

## Instalación

### 1. Clonar el repositorio

```bash
git clone https://github.com/tu-usuario/tu-repo.git
cd tu-repo
```

### 2. Crear y activar un entorno virtual

```bash
# Windows
python -m venv .venv
.venv\Scripts\activate

# Linux / macOS
python -m venv .venv
source .venv/bin/activate
```

### 3. Instalar dependencias de Python

```bash
pip install -r requirements.txt
```

### 4. Configurar la base de datos

1. Abre **SQL Server Management Studio (SSMS)** o cualquier cliente de SQL Server.
2. Crea la base de datos `Integradora`:
   ```sql
   CREATE DATABASE Integradora;
   ```
3. Ejecuta el script de inicialización:
   ```
   Integradora.sql
   ```
   Este script crea las tablas `materiales`, `usuarios`, `prestamos`, `devoluciones` y `pagos_multa`, e inserta datos de ejemplo.

### 5. Configurar variables de entorno

Copia el archivo de ejemplo y edítalo con tus credenciales:

```bash
cp .env.example .env
```

Edita el archivo `.env`:

```env
PAYPAL_MODE=sandbox           # Usa "live" para producción
PAYPAL_CLIENT_ID=tu_client_id_aqui
PAYPAL_CLIENT_SECRET=tu_secret_aqui
PAYPAL_CURRENCY=MXN
```

> Si necesitas cambiar la conexión a la base de datos (servidor, nombre de BD, autenticación), puedes agregar también estas variables al `.env`:
> ```env
> DB_DRIVER=ODBC Driver 17 for SQL Server
> DB_SERVER=localhost\SQLEXPRESS
> DB_NAME=Integradora
> DB_TRUSTED_CONNECTION=yes
> ```

---

## Ejecución

### Verificar conexión a la base de datos

Antes de levantar el servidor, puedes probar la conexión ejecutando:

```bash
python test_db.py
```

Si la conexión es exitosa, verás listadas las tablas de la base de datos.

### Iniciar el servidor Flask

```bash
python app.py
```

Por defecto, la aplicación estará disponible en:

```
http://127.0.0.1:5000
```

---

## Estructura del Proyecto

```
├── app.py                  # Aplicación principal Flask y rutas
├── database.py             # Conexión a SQL Server con pyodbc
├── models/
│   ├── material.py         # Lógica de materiales
│   ├── prestamo.py         # Lógica de préstamos
│   ├── devolucion.py       # Lógica de devoluciones
│   └── usuario.py          # Lógica de usuarios
├── templates/
│   ├── layout.html         # Plantilla base con navegación
│   ├── index.html          # Página de inicio
│   ├── materiales.html     # Lista de materiales + video tutorial
│   ├── registrar_material.html  # Formulario de nuevo material
│   ├── usuarios.html       # Gestión de usuarios
│   ├── prestamos.html      # Gestión de préstamos
│   ├── pagos.html          # Pagos de multas con PayPal
│   └── geolocalizacion.html     # Herramientas de geolocalización y clima
├── static/                 # Archivos estáticos (CSS, imágenes)
├── Integradora.sql         # Script de creación e inicialización de la BD
├── test_db.py              # Script para verificar la conexión a la BD
├── requirements.txt        # Dependencias de Python
├── .env.example            # Plantilla de variables de entorno
└── .gitignore
```

---

## APIs Integradas

| API | Uso | Requiere clave |
|-----|-----|---------------|
| YouTube Data v3 | Video tutorial de materiales | Sí |
| ip-api.com | Geolocalización por IP | No |
| Nominatim (OpenStreetMap) | Geocodificación por dirección | No |
| Open-Meteo | Clima actual por coordenadas | No |
| PayPal REST API | Cobro de multas en línea | Sí |

---

## Endpoints de la API REST

| Método | Ruta | Descripción |
|--------|------|-------------|
| GET | `/api/v1/geolocalizacion/ip` | Geolocalización por IP (param: `?ip=`) |
| POST | `/api/v1/geolocalizacion/direccion` | Geocodificación por dirección (body JSON) |
| GET | `/api/v1/clima/actual` | Clima actual (params: `?lat=&lon=`) |
| POST | `/pagos/crear-orden` | Crear orden de pago en PayPal |
| POST | `/pagos/capturar/<order_id>` | Capturar pago aprobado por PayPal |

---

## Credenciales de prueba (PayPal Sandbox)

Para probar el flujo de pagos en modo sandbox, utiliza las cuentas de prueba disponibles en el [Dashboard de PayPal Developer](https://developer.paypal.com/dashboard/accounts).

---

## Autores

- **Jorge Alberto** — Desarrollador
- **Diego David** — Desarrollador
- **Cristian Uriel** — Desarrollador

Proyecto Integrador II — 7EE2
