# Motos MAER

Aplicación web para registrar y consultar las **motos en espera de repuestos (MAER)** de Casa Pellas. Funciona desde el navegador del celular o de la computadora, sin instalar nada.

## ¿Qué permite hacer?

- **Registrar** una moto con chasis, marca, modelo, color y la lista numerada de detalles pendientes.
- **Consultar** un chasis para ver sus datos y detalles.
- **Cambiar el estado** de una moto: *En espera*, *Defectuosa* o *Rehabilitada*. Cada cambio guarda la fecha y quién lo realizó, y queda en un historial.
- **Lista** de todas las motos, con filtros por texto y por estado.
- **Excel**: exportar todos los datos (hojas MAER, Detalles e Historial) e importar motos desde un archivo.
- **Ajustes**: el administrador mantiene las listas de modelos por marca y de colores.

Marcas incluidas: Yamaha, Genesis, Hero y Yadea.

## Cómo funciona

| Parte | Tecnología |
|---|---|
| Página web | Un solo archivo, `index.html` (HTML, CSS y JavaScript) |
| Publicación | GitHub Pages |
| Base de datos | Supabase (PostgreSQL) |

La página no tiene servidor propio: se comunica con Supabase mediante funciones que verifican un **código de acceso** antes de leer o guardar datos. Las tablas están cerradas y no se pueden consultar directamente.

## Archivos

- `index.html`: la aplicación.
- `supabase_setup.sql`: crea las tablas y las funciones en Supabase.
- `README.md`: este documento.

## Puesta en marcha

### 1. Base de datos (Supabase)

1. Crear un proyecto en [supabase.com](https://supabase.com).
2. Abrir `supabase_setup.sql`, **cambiar los dos códigos** (`acceso` y `admin`) por códigos propios, largos y con letras y números.
3. Pegar el contenido en el editor SQL del proyecto y ejecutarlo.
4. En los ajustes del proyecto, sección API, copiar la **URL del proyecto** y la **clave pública (anon)**.

### 2. Conexión en la página

Abrir `index.html` y completar la configuración al inicio del script:

```js
const CFG = { url: 'URL_DEL_PROYECTO', key: 'CLAVE_ANON' };
```

La clave `anon` es pública por diseño. **Nunca** poner aquí la clave `service_role`.

### 3. Publicar en GitHub Pages

1. Subir los archivos a este repositorio.
2. En **Settings → Pages**, elegir *Deploy from a branch*, la rama `main` y la carpeta `/ (root)`.
3. Esperar un par de minutos: GitHub muestra el enlace público de la aplicación.

## Uso

1. Abrir el enlace y escribir el **código de acceso**.
2. El personal usa el código `acceso`. El administrador usa el código `admin`, que además permite guardar las listas en *Ajustes*.
3. El navegador recuerda el código en ese dispositivo. El botón **Salir** (al final de la página) lo borra.

## Seguridad

- Reparte los códigos solo a quien deba usar la app: quien los tenga puede ver y modificar las motos.
- Para cambiar un código, ejecutar en Supabase:

  ```sql
  update public.app_codigos set valor = 'NUEVO-CODIGO' where nombre = 'acceso';
  ```

- No publiques los códigos en este repositorio.

## Importar desde Excel

La primera hoja debe tener estos encabezados: `CHASIS`, `MARCA`, `MODELO`, `COLOR` y, opcionalmente, `DETALLES` (uno por línea o separados con punto y coma), `ESTADO`, `FECHA REHABILITACIÓN` y `REHABILITADA POR`. Si un chasis ya existe, se actualiza con los datos del archivo.
