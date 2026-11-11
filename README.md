# lucronium.github.io · web estática + Google Sheets

Tres piezas:

| Archivo | Dónde va |
|---|---|
| `index.html` | GitHub Pages (la web que ven los visores) |
| `Code.gs` | Dentro de la hoja de cálculo (Extensiones → Apps Script) |
| La hoja de cálculo | Tu Google Sheet, con 3 pestañas (ver abajo) |

Los editores modifican **la hoja de cálculo**. La web solo lee. Quién puede editar y el historial de cambios los gestiona Google.

---

## 1. Estructura de la hoja

La **fila 1 de cada pestaña son las cabeceras** (los nombres de abajo, sin importar mayúsculas ni tildes). Los nombres de las **pestañas** deben ser exactamente `Recursos`, `Minifiguras` y `Eventos`.

### Pestaña `Recursos` (los contadores)

| Nombre | Valor | Unidad | Icono |
|---|---|---|---|
| Dinero | 12450 | € | 💰 |
| Minifiguras | `=COUNTA(Minifiguras!A2:A)` | | 🧍 |
| Materiales | 320 | uds | 🧱 |

- Una fila = una tarjeta en la web, en el mismo orden.
- `Valor` puede ser un número fijo o una fórmula. El ejemplo de «Minifiguras» cuenta solo las filas de la pestaña `Minifiguras`.
- `Unidad` e `Icono` son opcionales (el icono es un emoji).
- Para añadir otro contador, añade una fila.

### Pestaña `Minifiguras`

| Nombre | Foto | Notas |
|---|---|---|
| Capitán Aldric | *(enlace de Drive)* | Líder de la expedición |

- `Foto` acepta un enlace de Drive («Copiar enlace»), el ID suelto del archivo o cualquier URL `https://` de una imagen. Si queda vacía, se muestra la inicial del nombre.
- Sube las fotos a una carpeta de Drive con acceso **«Cualquier persona con el enlace → Lector»**; si no, los visores no las verán.

### Pestaña `Eventos`

| Fecha | Título | Descripción |
|---|---|---|
| 2026-11-20 | Reunión de editores | Repaso de recursos |

- `Fecha` debe ser una fecha real de Sheets (formato `aaaa-mm-dd` recomendado). También se entiende `20/11/2026` escrito como texto.
- `Descripción` es opcional.

### Atajo

En el editor de Apps Script, ejecuta una vez la función **`crearEstructuraEjemplo`**: crea las tres pestañas con cabeceras y datos de ejemplo (no toca las que ya existan).

---

## 2. Poner en marcha la API

1. Abre tu hoja → **Extensiones → Apps Script**. Borra el contenido y pega `Code.gs`.
2. (Opcional) Ejecuta `crearEstructuraEjemplo` y acepta los permisos que pide Google.
3. **Implementar → Nueva implementación → Aplicación web**:
   - *Ejecutar como*: **Yo**
   - *Quién tiene acceso*: **Cualquier usuario**
4. Copia la **URL de la aplicación web** (termina en `/exec`).
5. Abre `index.html`, busca `API_URL` y pega la URL entre las comillas.

Si luego cambias `Code.gs`, usa **Implementar → Administrar implementaciones → ✏️ → Versión: nueva versión**. Así la URL no cambia.

## 3. Publicar en GitHub Pages

1. Crea un repositorio y sube `index.html`.
2. **Settings → Pages → Branch: main / (root)**.
3. Tu web quedará en `https://TU_USUARIO.github.io/NOMBRE_REPO/`.

## 4. Editores

Comparte la hoja de cálculo como **Editor** solo con las 5-10 personas que deban cambiar datos. Los visores no necesitan nada: solo abren la web.

---

## Cosas a tener en cuenta

- **Actualización:** la web consulta cada 20 s (`SEGUNDOS_ENTRE_ACTUALIZACIONES`) y la API cachea 15 s. Un cambio en la hoja tarda entre unos segundos y ~35 s en verse.
- **Privacidad:** la URL de la API queda visible en el código de la web, así que cualquiera que la tenga puede leer las tres pestañas publicadas. No pongas datos sensibles en ellas. El resto del libro no se expone.
- **Cuotas:** Apps Script es gratuito con cuotas diarias. Esta API es muy ligera, pero puedes vigilar «Ejecuciones» en el panel de Apps Script.
- **Modo demostración:** mientras `API_URL` conserve el texto `PEGA_AQUI…`, la web muestra datos de ejemplo.
- **Cambiar nombres de pestañas:** si renombras una pestaña, actualiza `HOJAS_PUBLICAS` en `Code.gs`. Los nombres de cabecera (`Nombre`, `Valor`…) son los que lee la web.
- 
