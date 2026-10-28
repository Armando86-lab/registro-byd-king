# Registro BYD King — PWA para iPhone

Este paquete es una primera versión funcional de una aplicación web instalable (PWA). Incluye formularios de viajes, cargas eléctricas y gasolina; resumen mensual; configuración de precios; inicio de sesión por correo y contraseña; y exportación manual de respaldo JSON.

## Importante antes de usar

- La app aún no está publicada en internet ni conectada a tu cuenta de Supabase. Para usarla en tu iPhone hay que completar los pasos siguientes.
- Supabase Free es suficiente para probar con un solo usuario y un volumen pequeño de registros, pero los proyectos gratuitos pueden pausarse tras una semana de inactividad y el plan gratuito no incluye copias automáticas de la base de datos. Usa **Descargar respaldo** periódicamente y guarda el archivo fuera de la app.
- No se incluye una función para restaurar/importar automáticamente el JSON todavía; el respaldo permite conservar una copia de los datos.
- La aplicación calcula la carga eléctrica como kWh medidos o, si no se indican, horas conectadas × potencia configurada. El costo eléctrico es kWh × tarifa configurada. El costo de gasolina es litros × precio por litro. No vuelve a cargar como electricidad la gasolina que el motor consume para generar energía.

## 1. Crear el proyecto de datos en Supabase

1. Abre https://supabase.com/ y crea una cuenta/proyecto gratuito.
2. En el proyecto, abre **SQL Editor** → **New query**.
3. Copia todo el contenido de `supabase-schema.sql`, ejecútalo y confirma que terminó sin errores.
4. En **Project Settings** → **API** (o **Connect**), copia la **Project URL** y la clave pública **anon/publishable**.
5. Abre `config.js` con un editor de texto y reemplaza `PEGA_AQUI_LA_URL` y `PEGA_AQUI_LA_CLAVE_PUBLICA` por esos valores.
6. No uses la clave `service_role` ni ninguna clave secreta en el archivo público.

## 2. Publicar la app gratis con GitHub Pages

GitHub Pages puede alojar archivos estáticos en HTTPS, que es necesario para instalar una PWA.

1. Crea/inicia sesión en https://github.com/.
2. Crea un repositorio **público** llamado `registro-byd-king`. No subas tu Excel ni respaldos personales al repositorio.
3. Sube los archivos de esta carpeta a la raíz del repositorio: `index.html`, `styles.css`, `app.js`, `config.js`, `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png`. Puedes dejar `LEEME.md` y `supabase-schema.sql` si quieres documentar el proyecto; no contienen datos personales.
4. En el repositorio, entra a **Settings** → **Pages**. En **Build and deployment**, selecciona **Deploy from a branch**, rama `main`, carpeta `/(root)` y guarda.
5. Espera a que GitHub muestre la URL publicada, normalmente `https://TU-USUARIO.github.io/registro-byd-king/`.
6. En Supabase, abre **Authentication** → **URL Configuration** y configura la URL publicada como **Site URL**. Añádela también a **Redirect URLs** si la pantalla lo solicita.
7. Abre la URL publicada en Safari y crea tu cuenta con correo y contraseña.

La clave anon/publishable es una clave de cliente diseñada para usarse en el navegador; la protección de datos depende de las políticas RLS incluidas en el SQL. No publiques nunca una clave secreta.

## 3. Añadir a la pantalla de inicio del iPhone

1. Abre la URL publicada en **Safari** (no desde un navegador integrado dentro de otra app).
2. Toca **Compartir**.
3. Elige **Añadir a pantalla de inicio**.
4. Deja el nombre `Registro BYD` y toca **Añadir**.

## Datos y cálculos

- **Viajes:** fecha, odómetro final o kilómetros manuales, batería al llegar y notas. Para calcular el kilometraje con precisión, registra el odómetro de forma consistente. Si no se introduce un odómetro anterior, el resumen usa los kilómetros manuales cuando existen.
- **Cargas:** si se conoce, se prioriza kWh medidos; de lo contrario se estiman con horas × potencia configurada.
- **Gasolina:** importe = litros × precio por litro.
- **Resumen mensual:** suma kilómetros, compras de gasolina, cargas eléctricas y costo por kilómetro. La gasolina comprada y la energía doméstica se suman una sola vez cada una; la estimación de kWh del coche no se trata como compra adicional.
- **Respaldo:** desde Resumen → Descargar respaldo se descarga un JSON con los registros de la cuenta. Guárdalo en Archivos/iCloud Drive y hazlo regularmente.

## Limitaciones conocidas de esta primera versión

- No importa automáticamente el historial del Excel; los registros históricos deben migrarse por separado.
- No permite editar/eliminar registros ni restaurar el JSON desde la interfaz todavía.
- El resumen calcula kilómetros manuales y diferencias entre odómetros registrados. Para evitar resultados incompletos, usa odómetros consistentes y registra cada viaje.
- Es una PWA web, no una aplicación distribuida en App Store.
