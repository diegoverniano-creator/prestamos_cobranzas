# Préstamos y Cobranzas – app Android

App Android que lleva dentro tu `prestamos_web.py` (Python embebido con Chaquopy) y lo muestra en pantalla.
Los datos quedan en el celular (`prestamos.db`), con los mismos cálculos de siempre.

> Importante: este proyecto **no se pudo compilar ni probar en un celular** donde lo preparé (sin acceso al SDK de Android).
> La parte Python sí se probó (servidor, préstamos, bancos, alquileres, impresiones, copias). Si la compilación falla,
> mandame el mensaje de error completo y lo corrijo.

## Opción A – Generar el APK en la nube con GitHub (sin instalar nada en la PC)

1. Creá una cuenta en github.com y un repositorio **privado** (por ejemplo `prestamos-android`).
2. Subí **todo el contenido de esta carpeta** (incluida la carpeta oculta `.github`). Lo más fácil es GitHub Desktop.
   Si subís por la web y `.github` no aparece, creá a mano el archivo `.github/workflows/apk.yml` (botón *Add file > Create new file*)
   y pegá el contenido del que viene en este zip.
3. **Para poder actualizar la app más adelante sin perder datos**, firmá con tu keystore. En el repositorio:
   *Settings > Secrets and variables > Actions > New repository secret*, y creá estos cuatro:
   - `KEYSTORE_B64`: tu keystore convertido a texto. En Windows (PowerShell):
     `[Convert]::ToBase64String([IO.File]::ReadAllBytes("C:\ruta\tu.jks"))`
   - `KEYSTORE_PASSWORD`, `KEY_ALIAS`, `KEY_PASSWORD`
   Podés usar el mismo keystore de tu otra app. **Nunca subas el archivo .jks al repositorio.**
   Sin estos secretos igual compila, pero con una clave de prueba distinta cada vez: sirve para instalar,
   no para actualizar encima.
4. Pestaña **Actions > Compilar APK > Run workflow**. Tarda entre 5 y 15 minutos.
5. Al terminar, entrá a la ejecución, bajá **PrestamosYCobranzas-apk** (viene en un zip) y adentro está el `.apk`.
6. Pasalo al celular e instalalo (hay que permitir «instalar apps de origen desconocido»).

## Opción B – Android Studio

1. Instalá Android Studio y **Python 3.11** (Chaquopy lo necesita para compilar).
2. *File > Open* y elegí esta carpeta. Esperá a que termine la sincronización.
3. *Build > Generate Signed App Bundle / APK > APK > release*, elegí tu keystore y finalizá.
   El APK queda en `app/build/outputs/apk/release/`.

## Cosas que cambian respecto de la PC

- **Datos de la PC al celular:** copiá tu `prestamos.db` al celular y en la app tocá **⬆ Importar** (arriba).
  Antes de reemplazar, la app guarda una copia de lo que tenía.
- **Copia de seguridad:** botón **⬇ Copia**: guarda toda la base en la carpeta *Descargas*. Hacela seguido
  (si desinstalás la app, los datos se pierden).
- **Imprimir** (comprobantes, estado de cuenta, contrato, resúmenes): abre el diálogo de impresión de Android;
  ahí elegís impresora o *Guardar como PDF*.
- **Exportar a Excel:** guarda el archivo .csv en *Descargas*.
- **WhatsApp:** abre la aplicación con el mensaje listo.
- **A cobrar / A pagar mensual:** el detalle de cada mes se ve **tocando** el mes (en la PC era al pasar el mouse).
- Si ponés contraseña y la errás 3 veces, no se cierra sola: cerrá y volvé a abrir la app.
- Requiere Android 10 o más nuevo.

## Qué se cambió en el código

`app/src/main/python/prestamos_web.py` es tu archivo; solo se tocó una línea (comillas dentro de un f-string, para que
funcione en Python 3.11). Todo lo demás está en `android_main.py` (arranque del servidor y copias),
`MainActivity.java` (pantalla, impresión, descargas) y `assets/inject.js` (ajustes para pantalla chica).
