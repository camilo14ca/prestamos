# Sistema de Préstamos (web app)

App personal para controlar préstamos desde el celular y el PC. Es gratis: se publica con **GitHub Pages**, y los datos se guardan en un **repositorio privado** de GitHub.

## Cómo funciona
- **Repo 1 – `prestamos` (público):** solo tiene el código de la app (estos archivos). Aquí NO van datos.
- **Repo 2 – `prestamos-datos` (privado):** la app guarda ahí el archivo `datos.json` con tus préstamos. Solo tú puedes verlo.

## Pasos de instalación

### 1. Publicar la app
1. En GitHub: **New repository** → nombre `prestamos` → **Public** → Create.
2. **Add file → Upload files** → sube `index.html`, `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png` → Commit.
3. **Settings → Pages** → Source: *Deploy from a branch* → Branch: `main` / `(root)` → Save.
4. En 1–2 minutos queda en: `https://camilo14ca.github.io/prestamos/`

### 2. Crear el repo privado de datos
- **New repository** → nombre `prestamos-datos` → **Private** → marca *Add a README file* → Create.

### 3. Crear el token (la “llave” de la app)
1. Foto de perfil → **Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token**.
2. Nombre: `app-prestamos`. Expiration: la que quieras (p. ej. 1 año).
3. **Repository access → Only select repositories →** `prestamos-datos`.
4. **Permissions → Repository permissions → Contents → Read and write**.
5. Generate token y cópialo (empieza por `github_pat_`).

### 4. Primera vez (configurar dispositivo)
1. Abre la app: sale **Configurar este dispositivo**.
2. Escribe usuario `camilo14ca`, repositorio `prestamos-datos`, el token y una **contraseña de la app** (mínimo 6 caracteres).
3. La primera vez te pide confirmar la contraseña. Se crea `auth.json` en `prestamos-datos` (solo la huella, nunca la contraseña).
4. Si es tu primera carga de datos: **Más → Importar respaldo** → `datos_iniciales.json`.

### 5. En el celular
1. Abre `https://camilo14ca.github.io/prestamos/` en Chrome → menú ⋮ → **Instalar app**.
2. Configura el dispositivo con los mismos datos y **la misma contraseña**.

## Inicio de sesión
- Cada vez que abres la app pide la contraseña. Se cierra sola tras 10 min en segundo plano, o con el candado 🔒.
- La contraseña NO está en el código: en `prestamos-datos/auth.json` solo hay una huella PBKDF2 (no se puede revertir).
- En cada dispositivo el token, los datos y las fotos guardadas quedan cifrados (AES-GCM) con tu contraseña.
- **Cambiar contraseña:** Más → Cambiar contraseña. Sirve para todos los dispositivos (los otros la piden al entrar).
- **Si olvidas la contraseña:** borra `auth.json` en `prestamos-datos` y en la app usa "Configurar de nuevo": crearás una nueva. Tus datos no se pierden.

## Reglas de cálculo (iguales al Excel)
- Valor interés = monto × % interés por cuota × N° cuotas (por defecto 10 %).
- Valor a pagar = monto + interés · Valor cuota = valor a pagar ÷ cuotas.
- Saldo = valor a pagar − pagos · Estado: PAGADO si saldo < 1.
- Disponible para prestar = capital inicial + total cobrado − retiros − total prestado.
- Préstamos con “SE PRESTÓ AL 15 %” en observaciones quedaron con 10 % como en el Excel; edítalos si quieres cambiar el %.

## Fotos
- **Foto del cliente:** pestaña Clientes → toca un cliente → toca el círculo con la cámara.
- **Documentos (cédula, pagaré, contrato…):** abre un préstamo → *Documentos* → **Agregar foto** → elige el tipo.
- **Comprobante de pago:** al registrar un pago, usa *Foto del comprobante (opcional)*.
- Las fotos se comprimen y se guardan en la carpeta `fotos/` del repo privado `prestamos-datos`, así se ven en el celular y en el PC. Si no hay internet, quedan *pendientes por subir* y se suben solas al reconectar.
- El respaldo .json NO incluye las fotos (quedan en GitHub).

## Seguridad
- El token queda guardado solo en tus dispositivos. Si pierdes el celular, revócalo en GitHub y crea otro.
- Nunca subas `datos_iniciales.json` ni respaldos `.json` al repo público.
- Descarga de vez en cuando un respaldo (.json) desde **Más**.
