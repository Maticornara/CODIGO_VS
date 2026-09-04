# Solar Propiedades — Contexto del proyecto

Sitio web inmobiliario de **Solar Propiedades** (Antonella Gutiérrez · Neuquén, Patagonia Argentina).
La idea es entregárselo terminado a Antonella, que NO es técnica: ella administra propiedades desde un panel sin tocar código.

# ⚡ ESTADO ACTUAL (04/09/2026) — LEER ESTO PRIMERO

**El traspaso a Antonella ya se hizo.** Todo lo que sigue mas abajo en este archivo es **historico**
y tiene datos viejos (menciona `Maticornara/CODIGO_VS`, `codigo-vs.vercel.app` y una "cuenta de la
facultad" que nunca existio). **Cuando algo contradiga a esta seccion, gana esta seccion.**

## URLs vigentes
| Que | Donde |
|---|---|
| **Sitio publico** | **https://www.solarprop.com.ar** (el apex redirige 308 al www) |
| **Panel / CMS** | https://unique-kitsune-f448c7.netlify.app/admin/ |
| **Repo** | **https://github.com/solarpropiedades/CODIGO_VS** (rama `main`) |
| Link para compartir una propiedad | `www.solarprop.com.ar/p/<slug>` |

`codigo-vs.vercel.app` quedo en el proyecto viejo de Vercel (cuenta de Mati), sin dominio. Se puede borrar.

## Quien es dueno de que
- ✅ **GitHub `solarpropiedades`** — el repo con TODAS las propiedades y fotos. **Es de Antonella.**
  `Maticornara` quedo como **colaborador Admin** (por eso Mati puede pushear).
- ✅ **Vercel `solarpropiedades-5496`** — proyecto `codigo-vs` + el dominio. **Es de Antonella.**
- ⚠️ **Netlify `archivoscornara@gmail.com`** — el panel. **Sigue siendo de Mati**: Netlify pide plan
  pago para transferir sites. Antonella entra igual con su usuario de Identity; lo que no puede es
  administrar el site. Ver "Como completar la transferencia de Netlify" al final del archivo.

## Cuentas
- **Servicios (GitHub / Vercel / Netlify nueva):** `solarpropiedades@outlook.com.ar`, misma contrasena
  para las tres. La casilla la tienen **Mati y Antonella**. A proposito NO depende del dominio propio,
  que es la pieza mas fragil.
- **Panel (Decap/Identity):** `inmobiliaria@solarprop.com.ar` — **solo sirve para el panel**, no abre
  GitHub ni Vercel ni Netlify. Ese mail SI recibe correo (Microsoft 365).
- **Netlify que administra el panel:** `archivoscornara@gmail.com`.

## Como publica Antonella (el circuito, verificado end-to-end)
Panel → Git Gateway → commit a `solarpropiedades/CODIGO_VS` → Vercel republica **y** el sitio lee los
`.md` en vivo desde la GitHub API. Verificado el 04/09/2026: creo y borro una propiedad ella misma.

## ⚠️ Cosas que NO hay que hacer
- **NO recrear el site de Netlify desde cero.** Git Gateway esta **deprecado**: habilitarlo de nuevo
  es una config nueva de algo que Netlify ya no arregla.
- **NO poner el repo en privado.** El sitio lee los datos con la API publica de GitHub, sin token.
- **NO borrar la cuenta de Netlify `solarpropiedades@outlook.com.ar`** (esta vacia a proposito: es el
  destino si algun dia se transfiere el site).
- **NO usar URLs absolutas con `codigo-vs.vercel.app`.** El canonico es `www.solarprop.com.ar`.
- En Vercel, **no desactivar** el redirect 308 del apex al www ni tocar los TXT `_vercel` del DNS.

## Respaldos (Escritorio\SOLAR PROPIEDADES)
- `CODIGO_VS-backup.git` — clon completo del repo antes de transferirlo.
- `DNS-BACKUP-solarprop.txt` — registros del correo (MX / SPF / autodiscover) por si hay que recrearlos.

## Pendientes menores
- Borrar el proyecto viejo de Vercel (cuenta de Mati, ya sin dominio).
- Que Antonella active 2FA en Netlify (figura como "No 2FA").
- Imagen Open Graph propia de 1200x630 (hoy usa `fotos/ISOLOGO.PNG` como parche).
- La propiedad `lo-615.md` ("Casa Chica Barrio Norte") sigue siendo la de prueba; la reemplaza ella.

---

## Idioma
Hablame y comentá el código en **español** (argentino).

## Stack
- **HTML / CSS / JS puro**, sin frameworks ni build step. Todo vive en archivos `.html`.
- **CMS:** Decap CMS (ex Netlify CMS) con **Netlify Identity + Git Gateway**. Antonella crea/edita propiedades desde el panel; cada cambio se guarda como `.md` en el repo de GitHub.
- Las propiedades son archivos YAML en `content/propiedades/*.md`.

## URLs
- **Sitio público:** codigo-vs.vercel.app (Vercel, sin límite de deploys)
- **Admin / CMS:** unique-kitsune-f448c7.netlify.app/admin/ (Netlify — site nuevo, cuenta de la facultad con créditos renovados). ⚠️ URL vieja (ya NO usar): sunny-blancmange-47869e.netlify.app.
- **Repo:** github.com/Maticornara/CODIGO_VS (rama de trabajo y producción: `main`)

## Deploy ⚠️ IMPORTANTE
- **Producción se despliega en VERCEL al hacer push a `main`.** Para publicar: commit + push a `main`.
- **Los builds de Netlify están PAUSADOS** a propósito (para no gastar créditos). Netlify se usa SOLO como Identity provider + Git Gateway del CMS. NO reactivar los builds de Netlify.
- Hacer commit/push solo cuando el usuario lo pida.

## Archivos clave
- `index.html` — sitio principal (incluye todo el hero animado).
- `propiedades/index.html` — hace doble función: listado (`/propiedades`) con búsqueda/filtros, y página individual (`/propiedades/[slug]`). Detecta el modo por la URL.
- `admin/index.html` + `admin/config.yml` — panel Decap CMS.
- `content/propiedades/*.md` — propiedades en YAML.
- `fotos/` — logos e imágenes (`LOGO.PNG` = pin; `ISOLOGO_VECTOR.svg` = wordmark "SOLAR PROPIEDADES").

## Datos
- **WhatsApp:** 5492994029148
- **Paleta:** verde `#36512B` / verde oscuro `#1e2e17` / fondo hero `#1a2a10` · acento naranja `#EE7A13` · crema `#FFFCEB`.
- **Tipografías:** Archivo (cuerpo) + Archivo Black (display).

## Hero animado (index.html) — cómo funciona
Todo está en un IIFE dentro de `index.html`. Hay 4 ilustraciones line-art que rotan sincronizadas con un typewriter ("El valor justo de tu Casa./Departamento./Terreno./Propiedad.").

- Cada ilustración es un `<svg class="illus-svg" id="illus-0..3">` con grupos:
  - `.ill-stroke` → contornos que se **dibujan** (animación de trazo, Web Animations API).
  - `.ill-fill` → caras que se **rellenan** con el gradiente `#faceGrad` (crema→naranja sutil), vía `fill-opacity`.
  - `.ill-grow` → elementos que **crecen/brotan** (scaleY desde la base, con rebote): árboles y carteles.
- Trazo: usa `pathLen()` (getTotalLength × escala del CTM, por `non-scaling-stroke`) + dasharray con gap grande + overshoot (evita "puntitos" al desdibujar). NO usar un DASH fijo.
- **Escenas:**
  - **Casa (0):** chimenea + humo (`.smoke`) + caminito + árboles que crecen.
  - **Edificio (1):** ventanas que se encienden (`.ill-lights`, crema) + destello/glint en las ventanas + auto y canteros (árboles) abajo.
  - **Terreno (2):** árboles + cartel (plop) + pájaros (`.bird`) que cruzan.
  - **Llave (3):** levita + sombra + destello + una **mano** que entra desde la derecha y la agarra (con recoil).
- Cada ilustración tiene su `viewBox` y, si necesita más lugar, una caja propia (`#illus-N { max-width/height }`) y/o un `transform` de posición. Posicionar suele requerir iteración con capturas del usuario.

## Ilustraciones — flujo de trabajo
- Las ilustraciones las **dibuja y exporta el usuario en Illustrator** (line-art), con grupos `llenos` (rellenos) y `strokes` (contornos). NO dibujarlas a mano en código (desentonan con las pro).
- Los SVG fuente quedan en `ILUSTRACIONES/` (carpeta de trabajo, **ignorada por git** — el sitio no la usa porque los SVG van inline en el HTML).
- `ILUSTRACIONES/_extract.py` extrae los vectores del export de Illustrator, calcula viewBox y genera snippets para pegar en `index.html`.

## Cómo correr en local
Es HTML puro, sin build. Servidor local: `python -m http.server 3000` (o `npx serve .`).

## Preferencias del usuario (Mati) y cómo le gusta trabajar
- **Itera con capturas.** Trabaja muy visual: pide cambios, manda screenshots y se afina de a poco. Cuando algo de posición/escala no se puede "ver", asumir que va a necesitar varias vueltas con capturas. No dar por cerrado sin su OK visual.
- **Idioma español argentino**, tono cercano. Usa expresiones como "mandale", "dale", "está re lindo", "joya", "hermoso".
- **Nada se superpone.** Regla fuerte: ningún elemento debe pisarse con otro (ni árbol con árbol, ni con el edificio, ni el auto). Lo repitió varias veces.
- **Le importa el detalle y la "magia".** Pidió explícitamente "darle más magia", "que destaque", "que se vea un poco más lindo", "agregarle capas y poderes". Valora micro-animaciones y vida en las ilustraciones.

## Decisiones e intenciones de diseño que marcó (su "voz")
- **Tipografía hero más grande** y que cada frase respete **2 líneas** (nunca 3). Palabras del typewriter con **mayúscula inicial** y en **naranja de acento** (Casa./Departamento./Terreno./Propiedad.).
- **Wordmark "SOLAR PROPIEDADES" nunca negro ni blanco puro:** sobre fondo oscuro → crema; sobre crema → verde. (Le molesta el negro y el blanco puro.)
- **Splash:** logo + wordmark en horizontal (igual que el nav), con un "truco" de degradé: glow naranja detrás para que el verde del pin contraste. No le gustaba el verde plano.
- **Relleno de las ilustraciones:** crema con un **degradé sutil hacia naranja** para dar profundidad (no plano). Lo aprobó fuerte ("quedó hermoso").
- **Desdibujado parejo, sin "puntitos".** Le molestaban los restos de trazo al desdibujar; quedó resuelto con gap grande + overshoot. El timing tiene que ser fluido y coordinado con el typewriter.
- **Árboles y cartel: efecto "plop"** — que **crezcan/broten desde la base** con un rebotecito (no que se dibujen). Y que **crezcan, pero NO se "descrezcan"**: al salir se **desdibujan como todo** (no se encogen). En el terreno, el plop del cartel va **antes** del fill.
- **Humo de la casa:** que se mueva **siempre** y se vaya en **fade-out** (no que se congele). Estilo "bocanadas que suben".
- **Pájaros:** silueta en **"V" con perspectiva** (ala izquierda más larga), **pico pronunciado**, **aleteo lento** (~1 por segundo, no achatado/pivote), **vuelo corto** que entra y sale con transparencia. Solo en el **terreno**.
- **Llave:** **levita** suave, con **sombra desenfocada** debajo, **destello/glint** bien **blanco** y **giro sutil**. La **mano entra desde la derecha y la agarra**, con el **brazo cortándose por el viewport** (no se ve el codo). Al agarrar, un **recoil de movimiento** (se va un toque a la izquierda y vuelve, "como mostrando la llave"), NO un scale. La mano al salir **se desdibuja** (no se va).
- **Edificio:** **ventanas que se encienden** en crema (no le gustaba el amarillo cálido), y **destello en las ventanas** que ocurre **después** de que prenden las luces (las ventanas son crema FFFCEB para que el glint blanco contraste). Auto + canteros (árboles en línea, tipo vereda) abajo; los canteros también hacen "plop".

## Notas de memoria
- Este `CLAUDE.md` es la "memoria" del proyecto: se lee automáticamente en cada chat del agente en esta carpeta. Mantenerlo actualizado con decisiones y preferencias nuevas.

---

# Sesión de trabajo — secciones, propiedades, admin y contacto

> Todo lo de abajo se agregó en una sesión larga. **Importante:** muchos cambios todavía requieren **deploy** para verse (ver "Deploy y datos en vivo").

## Deploy y datos en vivo ⚠️ (clave para no confundirse)
- Las propiedades se leen **EN VIVO desde la GitHub API** (`Maticornara/CODIGO_VS`, rama `main`, carpeta `content/propiedades`). Por eso **un cambio local en un `.md` NO se ve hasta hacer push**. Si algo nuevo (ej. características/chips) "no aparece" en local, es porque los datos viven en GitHub.
- **Sitio público → Vercel** (push a `main`). **Admin → Netlify** (builds PAUSADOS; hay que hacer **deploy manual** para ver cambios de `admin/`).

## Routing de fichas (importante)
- La ficha individual ahora se abre con **query param**: `/propiedades/?p=slug`. Funciona en **cualquier server** (localhost incluido), sin depender del rewrite de Vercel. Se mantiene compatibilidad con `/propiedades/slug` (path) por las dudas. Las cards (home y listado) linkean a `?p=`.

## Sección "Lo que nos define" (Valores)
- Animaciones al hacer scroll con **IntersectionObserver** (NO scroll listeners): líneas divisorias verticales que crecen, y columnas con fade+slide escalonado. Threshold 0.7 + pausa de 0.5s al entrar (si no, no se llega a ver).
- La **barra de navegación se vuelve transparente** sobre esta sección (clase `over-dark`), como en el hero. Y la sección tiene el **mismo degradé animado (blobs) del hero** (capa `.hero-pattern` reutilizada, recortada por una capa interna para no cortar dropdowns).

## Sección "Sobre mí"
- Foto real: `fotos/foto_antonella.jpeg` (3/4, sin recuadro naranja desfasado).
- Secuencia con IntersectionObserver: espera ~1s al entrar → foto + eyebrow fade → typewriter del título "Hola, soy Antonella Gutiérrez" → bio → badges escalonados.
- **Typewriter SIN layout shift:** todo el texto ya está renderizado (cada carácter en un `<span class="ch">` transparente) y se "revela" cambiando color, no insertando texto. El bio mantiene `<strong>` (negrita) por carácter. El typing total dura ~3s (revelado por `requestAnimationFrame`, duración exacta).

## FAQ
- Se abre con **click/tap** (antes era hover), clase `.open`. Soporta teclado.
- Hover sobre la pregunta: **reborde naranja de 3px** que aparece/desaparece por **opacidad** (fade 0.5s), solo en items cerrados (`:not(.open)`).

## Navegación
- Orden: **Inicio · Propiedades▾ · Servicios · Nuestra esencia · Sobre mí · FAQ · Contacto▾ · Consultar**.
- **Propiedades** va destacado (naranja + bold, clase `nav-destacado`).
- **Contacto▾** es una bandeja desplegable con WhatsApp / Email / Instagram (por ahora solo en el nav del home).
- Favicon: `fotos/LOGO.PNG`.

## Sistema tipográfico (desktop, consistente en las 3 páginas)
- Referencias "buenas" del usuario: hero-sub (~16px) y bio de Sobre mí (15px).
- **Cuerpo / desarrollo = 16px** (descripciones, FAQ, bio, datos de ficha). **Labels mayúscula = 11px** (eyebrows, ZONA, etc.). Subtítulos/cards 17px, pregunta FAQ 20px. Precios/m² grandes sin cambios. Se subió el contraste de varios textos apagados.

## Propiedades — listado y ficha
- **Buscadores (home + listado) iguales (estilo barra blanca)** y filtran igual. El header del listado quedó en **claro/crema** (no verde).
- **Precio y m² son inputs libres** (no dropdowns). El valor que pone el usuario es una **referencia ±30%** (aparecen propiedades alrededor, no un tope). Placeholder "Sin especificar" en itálica. Label "Precio U$S".
- **Búsqueda sin tildes ni mayúsculas:** función `normTxt()` (minúsculas + saca acentos). Se usa en ubicación y características.
- **Listado:** las **destacadas van al FINAL** (abajo) y muestran badge **"★ Destacada"** (naranja, arriba-der).
- **Ficha (layout desktop):** contenedor **1280px**. Arriba el **título**. Después **galería a la izquierda + card de precio/contacto a la derecha** (`.prop-top`, la card NO se estira: su alto se acomoda al contenido). Debajo (`.prop-body`, max-width 820px para que la descripción tenga aire): datos (Superficie grande arriba, **Zona / Localidad** separadas en un renglón — si solo hay `ubicacion` viejo, se separa por la coma), descripción, reel. Mapa abajo.
- **Galería:** foto principal **16:9** con relleno **verde** si la foto es vertical. **Flechas naranjas** (SVG, con hover scale) + **teclado ← →**. **Fade** al cambiar de foto. Miniaturas con hover scale. Lightbox al click.
- **Mapa:** acepta coordenadas DMS o decimal; `dmsToDecimal()` las convierte para el embed de Google Maps.
- **Sin "Moneda"** en los datos (ya está implícita en el precio).

## Características (hashtags) — feature completo
- Campo nuevo en el admin: **"Características"** = lista de **texto libre** (Antonella agrega: Cochera, Pileta, Vista al lago…).
- Se muestran como **#hashtags**: en la **card verde de la ficha** (clickeables → llevan al listado filtrado, `?tag=`) y en las **cards** (no clickeables porque la card ya es un link).
- **Búsqueda por características** en home y listado: una **fila de chips** que se arma **sola** con las etiquetas existentes. Filtro **OR** (muestra las que tengan **alguna**). Decidido: texto libre (no lista cerrada). La normalización (`normTxt`) agrupa por minúsculas/sin acentos, pero sinónimos distintos ("Cochera" vs "Garage") quedan como chips distintos.

## Contacto
- **Formulario → WhatsApp:** al enviar, abre WhatsApp con los datos (nombre, tel, email, consulta). (Se probó mailto y no es confiable en desktop sin cliente de correo; por eso WhatsApp.)
- Además, visibles como otros medios: **Email** y **Instagram** (`@solar_propiedades` → https://www.instagram.com/solar_propiedades/).
- **Email de contacto: `inmobiliaria@solarprop.com.ar`** (email real de la inmobiliaria; reemplazó al de prueba `matyy.cornara@gmail.com`). Aparece en el form del home, los mailto del nav/contacto/menú mobile, y en la ficha.
- Los mensajes (WhatsApp y mail) dicen explícito **"desde la Página web de Solar Propiedades"**. En la ficha, el mensaje sigue el ejemplo de Antonella: *"¡Hola! Estoy interesado en la propiedad que vi en su web: (TÍTULO) - (CÓDIGO). ¿Me podrían brindar más información? Aquí está el enlace del inmueble: (LINK)"*.

## Admin (Decap CMS)
- **Verde de marca `#36512B`** en header y acentos (antes #1F3216).
- Botón corregido a **"Nueva propiedad"** (era "Nuevo Propiedad").
- **Logo** en login/header: `logo_url: /fotos/LOGO.PNG`.
- **Vista previa en vivo** (`CMS.registerPreviewTemplate` + `registerPreviewStyle`): muestra la propiedad con el estilo real del sitio (foto 16:9 verde, badges, título Archivo Black, superficie, zona/localidad, precio, #hashtags, descripción) mientras Antonella edita.
- Límite honesto: Decap es la "cáscara" de React; reestilizar componentes internos profundos es frágil (clases ofuscadas). Lo robusto: branding (logo/colores/fuente) + preview custom.

## Pendientes (cuando se retome)
- **Mobile:** menú hamburguesa (hoy en celular NO hay menú) + repaso mobile completo (home, listado, ficha).
- **Servicios** (sección nunca revisada), footer.
- ~~Reemplazar email de prueba por el real.~~ ✅ Hecho: `inmobiliaria@solarprop.com.ar`.
- Open Graph / SEO para compartir lindo por WhatsApp/redes.
- Opcional: características estructuradas (dormitorios, baños) como datos, no como tags.

---

# Sesión — Rediseño ficha (Remax) + GRAN FILTRO + campos nuevos del admin

## Modelo de datos / Admin (campos nuevos en `admin/config.yml`)
Además de los ya existentes (codigo, titulo, tipo, precio, moneda, zona, localidad, coordenadas, descripcion, fotos, link_reel, destacada), se agregaron:
- **tipo:** se sumó **Departamento** (opciones: Lote, Casa, Departamento, Alquiler permanente, Alquiler turístico).
- **metros_cuadrados** (= "Superficie total"), **metros_cubiertos**, **metros_semicubiertos**, **metros_terreno**.
- **ambientes** (número) + **ambientes_detalle** (lista de nombres: Living, Cocina…; texto libre con sugerencias en el `hint`).
- **dormitorios**, **banos**, **antiguedad** (años).
- **servicios** (lista texto libre, sugerencias en `hint`: Luz, Gas, Agua, Cloacas…).
- **caracteristicas** (lista texto libre, ya existía; ahora SIN # en las cards).
- ⚠️ Decap no tiene autocompletado real en listas → las "sugerencias" son el `hint`. Para sugerencias clickeables habría que usar checkboxes fijos.

## Ficha de propiedad (`propiedades/index.html`) — estructura tipo Remax (vertical, todo apilado)
Orden: **1)** Hero (breadcrumb + badge + título + ubicación 📍) **2)** Galería **3)** Card de datos (precio + íconos) **4)** Descripción **5)** Detalles **6)** Reel **7)** Mapa **8)** Similares. Todo en `.prop-page` (max-width **1200px**).
- **Galería:** foto **16:9** (siempre) a la izquierda + **carrusel vertical de thumbnails a la derecha** (columna 240px) con **scroll local sin barra**. Degradés arriba/abajo que aparecen/desaparecen por opacity según haya más fotos (clases `show-top`/`show-bottom` en `.gallery-thumbs`, vía `updateThumbFade()`). En mobile el carrusel pasa a fila horizontal. El hero usa `.prop-hero { min-height: calc(100vh - nav) }` para que la card de precio no asome en la 1ª pantalla.
- **Robustez de imágenes:** `keepValidImages()` precarga y descarta fotos rotas (nunca se ve un recuadro roto); en cards, `imgFallback()` reemplaza por placeholder.
- **Card de datos** (`.prop-datacard`, borde naranja): código + precio + grid de stats con íconos SVG (m² totales/cubiertos/terreno, ambientes, dormitorios, baños, antigüedad). Contacto (WhatsApp/Email) debajo.
- **Sección "Detalles de la propiedad"** (recuadro solo-borde, igual que Descripción): subsecciones **Superficies y datos** (label: valor), **Servicios** (✓), **Ambientes** (✓), **Características** (✓, clickeables → `?tag=`). Cada lista aparece solo si hay datos.
- **Propiedades similares:** `renderSimilares(p)` trae todas, puntúa (mismo **tipo** pesa fuerte + precio parecido misma moneda + características compartidas), deduplica por código, muestra top 3 (más parecida a la izquierda). Aparece vacía si solo hay una propiedad.

## GRAN FILTRO — en home (`index.html`) y listado (`propiedades/index.html`)
**Misma barra en las dos pages** (ancho **1280px = --max-w**, margen lateral 56px; en el listado el hero/cards quedan en 1100 pero la barra es 1280).
- **Filtros principales (barra):** Tipo (dropdown) · Ubicación (input con **autocompletado** que sugiere localidades/zonas existentes; igual permite texto libre) · **Precio** (dropdown con selector **U$S / $** + inputs **Desde/Hasta**) · **Superficie m²** (dropdown con **Desde/Hasta**).
- **"Más filtros"** (botón que despliega `.more-filters`): Ambientes/Dormitorios/Baños (mín., selects), m² cubiertos/terreno (mín.), Antigüedad (máx.), y **Servicios** + **Características** como **botones colapsables** (`.mf-chips-toggle`; no aparecen de una).
- **Reglas clave (decisiones del usuario):**
  - **Nada es obligatorio:** input vacío = NO filtra. Las etiquetas de Precio/m² quedan **vacías** (no dicen "Cualquiera").
  - **Moneda sin toggle global:** el selector U$S/$ vive dentro del filtro de Precio; solo define en qué moneda se interpreta el rango. Sin cotización/conversión (filtra por `moneda` que coincida).
  - **El filtrado se ejecuta SOLO al tocar "Buscar"** (o Enter), NO en vivo. Los filtros **no se borran** al buscar (solo con "Limpiar filtros").
- **Listado:** filtra sobre `allProps` y **lee parámetros de la URL** (`tipo, ubic, cur, pmin, pmax, mmin, mmax, amb, dorm, banos, cub, ter, ant, tags, serv`). Si vienen chips por URL, abre "Más filtros" y el grupo correspondiente.
- **Home:** la barra **no filtra ahí** (las destacadas son vidriera). "Buscar" (`irABusqueda`) **junta todo y redirige a `/propiedades?...`**. Las destacadas siguen con `mostrarDestacadas()`.
- Se quitó el slider de dos perillas (el usuario lo encontró limitante) → inputs Desde/Hasta.

## Cards (home + listado)
- Muestran las **características principales como píldoras** (`60 m²` · `3 amb.` · `2 dorm.` · `1 baño`), **sin #** (`cardStats(p)` / `statsStr`). Ubicación arriba en texto plano.

## Home (`index.html`) — ajustes varios
- **Nav blanca/sólida sobre la sección de esencia** (antes se transparentaba): `navDarkSections = []`.
- Bloques 1/2/3 de "Lo que nos define" **subidos** (menos margen del título y padding de los items).
- Botón **"Ver todas"** de destacadas **destacado** (relleno naranja, `.btn-ver-todas-dest`).

## Deploy de esta sesión
- Push a `main` → Vercel (sitio) ✅. Para el **admin** (campos nuevos) hace falta **deploy de Netlify** (estaba pausado; el usuario lo habilitó para publicar).

---

# Sesión — Admin simplificado, favicon, listas como texto, y presupuesto de deploys

## ⚠️ DEPLOYS DE NETLIFY (actualizado)
- **Site nuevo:** `unique-kitsune-f448c7.netlify.app` (cuenta de la facultad, **créditos renovados** → ya NO hay escasez de deploys como antes). El site viejo (`sunny-blancmange-47869e`) quedó obsoleto.
- **Builds PAUSADOS a propósito** igual (el usuario los dejó así: "Netlify está re loco"). Netlify se usa SOLO como Identity + Git Gateway del CMS; el sitio público lo sirve Vercel. ⇒ Cuando se toca código de `admin/` (config.yml o index.html) **hay que hacer deploy manual de Netlify** ("Trigger deploy"). NO reactivar los builds salvo que el usuario lo pida.
- **Las ediciones de propiedades de Antonella NO gastan ni dependen de Netlify** (commitean a GitHub → publica Vercel; el sitio lee los datos en vivo desde la GitHub API).
- Ya no hace falta batchear con tanto cuidado por escasez, pero igual conviene avisar al usuario para que haga el deploy manual cuando cambie el `admin/`.

## Favicon
- `LOGO.PNG` como favicon en `index.html`, `propiedades/index.html` y `admin/index.html` (ruta absoluta `/fotos/LOGO.PNG`).

## Listas (características / servicios / ambientes) → TEXTO LIBRE
- Problema: el widget `list` de Decap cortaba en "tags" (confuso) y dejaba ítems vacíos (`- ""` → en el sitio salía `# ""`).
- Solución: en `admin/config.yml` esos 3 campos son **`widget: text`** (cuadro de texto; se escribe con coma y espacios, ej. "Gas natural, Luz").
- En el sitio se parsea con **`toList(v)`** (en `propiedades/index.html` e `index.html`): acepta string ("a, b") o lista YAML vieja, separa por coma/salto de línea y **descarta vacíos**. Está aplicado en TODOS los consumidores (ficha Detalles, filtros, chips, similares, home).
- **`parseYAML`** (ambos archivos): ignora ítems de lista vacíos y saca comillas → no más `# ""`.
- Datos migrados a texto en `content/propiedades/lo-615.md` (CA-001) y `lo-615-1.md` (ALP-001).
- ⚠️ Tras deployar el nuevo `config.yml`, abrir cada propiedad una vez para que esos campos queden en formato texto (si quedó alguno en lista vieja, se ve raro hasta re-guardar).

## Fotos en el admin
- `fotos` → **`widget: image, multiple: true`** (se eligen/arrastran varias). Sigue guardando lista de paths; el sitio lee `p.fotos` igual.
- `admin/index.html` fuerza `multiple` en los `input[type=file]` (para elegir varias en el explorador).
- ⚠️ Las fotos recién subidas tardan **1-2 min en verse en el sitio** porque el archivo lo sirve **Vercel** (rebuild). El `.md` se lee en vivo pero la imagen necesita el build. `keepValidImages()` oculta las que aún no están listas.

## Admin (`admin/index.html`) — branding/UX por JS (Decap no se reestiliza fácil)
- Oculta pestaña **"Medios"** y botón **"Añadir rápido"** (matcheo ESPECÍFICO `añadir rápido`/`quick add` para NO ocultar otros botones con "Añadir").
- Botón **"Nueva Propiedad"** (corrige "Nuevo"→"Nueva") y **"Subir nuevo"** en **naranja**. "Confirmar selección" y el resto, sin tocar.
- **Preview en vivo** reescrito: ubicación (zona · localidad), stats (m² total/cubiertos/terreno, ambientes, dorm, baños, antigüedad), y grupos Servicios/Ambientes/Características. Helper `toArr()` parsea string o lista Immutable.

## Ficha
- Las **características** dentro de la propiedad ya **NO son clickeables al filtro** (se descartó esa idea por ahora); quedan como texto con ✓.

## Barra de búsqueda
- Mismo ancho en home y listado: ambas `max-width: var(--max-w)` (1280px).

---

# Sesión — Traducción ES/EN (toggle propio, SIN Google Translate)

## Por qué NO Google Translate
- Google Translate rompía las animaciones de tipografía (typewriter del **hero** y de **Sobre mí**) porque reemplaza nodos de texto en vivo. Se descartó. Se hizo un **toggle propio ES/EN**.

## Cómo funciona el i18n (en `index.html` y `propiedades/index.html`)
- **Estado:** `window.SOLAR_LANG` se define en el `<head>` (antes de cualquier animación) leyendo `localStorage['solar-lang']` (default `'es'`). También setea `document.documentElement.lang`.
- **Helper para JS:** `window.T(es, en)` devuelve el string según el idioma. Se usa en TODO el texto generado por JS (cards, stats de la ficha, labels de detalle, status de resultados, navBackLabel, etc.).
- **Texto estático en HTML:** atributo **`data-en`** (se swapea el `innerHTML`) y **`data-en-ph`** (se swapea el `placeholder` de inputs). `solarApplyLang()` guarda el original en `data-es`/`data-es-ph` la primera vez y luego intercambia según idioma.
- **Toggle:** botón `#langToggle` (clase `.nav-lang`, dice "EN" o "ES") → `solarToggleLang()` guarda el próximo idioma en localStorage y hace **`location.reload()`**. **Recargar es a propósito:** así el hero y Sobre mí se re-inicializan limpios en el idioma elegido y no se ven cortes/saltos de las animaciones.
- **Texto animado:** el hero usa `window.__setHeroLang(lang)` que reasigna `BASE`/`WORDS` del typewriter; el título de Sobre mí lee el atributo `data-text-en` (además de `data-text`). La bio/badges usan `data-en` (se swapean antes de que corra la animación de reveal).
- **Fallback seguro:** lo que no tenga `data-en`/`T()` queda en español (no rompe nada; permite traducir de a poco).

## Qué se traduce y qué NO
- **Se traduce toda la UI:** nav, hero, buscador + "Más filtros", secciones (Servicios, esencia, Sobre mí, FAQ, Contacto, footer), y en `propiedades/`: hero del listado, barra de búsqueda, labels de la ficha (Descripción, Detalles, Ubicación, Similares), botones de contacto, breadcrumb, stats, etc.
- **Los DATOS de cada propiedad SÍ se traducen automáticamente** (ver abajo). Antonella los carga **solo en español**; no carga nada dos veces.

## Traducción AUTOMÁTICA de los datos de las propiedades (MyMemory)
- **Por qué automática:** se descartó pedirle a Antonella cargar todo dos veces (ES/EN). En su lugar, los datos se traducen al vuelo SOLO cuando el visitante pone la página en inglés.
- **Motor:** **MyMemory** (`api.mymemory.translated.net`), **gratis y sin API key**, con **CORS abierto** → se llama directo desde el navegador (no hace falta función serverless). Límite ~5.000 palabras/día por IP (suficiente con el caché). Si algún día la calidad no alcanza, se migra a Google Cloud Translation cambiando solo la función `tr()`.
- **Módulo `window.solarTr`** (definido en el `<head>` de `index.html` y `propiedades/index.html`, idéntico):
  - `tr(text)` → Promise con la traducción (o el original si falla / si idioma ≠ en).
  - `el(node)` → traduce el `textContent` de un elemento (campos cortos: título, ubicación).
  - `html(container)` → traduce los **nodos de texto** dentro de un contenedor **preservando el HTML** (para la descripción con `<strong>`/`<ul>` y las listas con ✓).
  - `cards(scope)` → traduce `.prop-card-titulo` y `.prop-card-ubic` de las cards dentro de `scope`.
  - `window.solarTrTipo(t)` → **mapa fijo** de tipos (Lote→Lot, Casa→House, Departamento→Apartment, etc.). No se manda a MyMemory porque "Lote" lo traduce mal.
- **Solo on-demand + caché:** solo traduce cuando `SOLAR_LANG==='en'` y SOLO lo que el visitante está mirando (la propiedad/cards en pantalla, no todo el catálogo). Cada traducción se **cachea en `localStorage['solar-tr-cache']`** → la 2ª vez es instantánea y no gasta cuota. Se ve el español un instante y se reemplaza (sin saltos).
- **Dónde se engancha:** cards del listado y similares (`solarTr.cards`), y en la ficha: `propTitulo` (`el`), `propTipo` (mapa), `propUbicTxt` / `propDesc` / listas de Servicios/Ambientes/Características (`html`). En el home: cards de destacadas (`solarTr.cards`) + tipo (mapa). Badge "★ Destacada"→"★ Featured" con `T()`.

## Deploy
- Todo es del sitio público → **push a `main` → Vercel**. NO toca el admin, así que **no gasta deploys de Netlify**.

---

# Sesión — VERSIÓN MOBILE (prácticamente COMPLETA) ✅

> El sitio estaba pensado **solo para desktop**. Se hizo el pasaje a **mobile** (`@media (max-width: 768px)` en el home; `600px` en `propiedades/`). Quedó funcionando entero en celular. Esta sección documenta cómo está armado y lo poco que falta.

## Concepto central (decisión del usuario)
- En mobile **NO existe el `:hover`**, así que cada efecto hover de desktop se traduce a un **trigger por Intersection Observer**: clase `.is-visible` cuando el elemento entra al viewport, y **se remueve al salir** (se repite al volver a scrollear).
- Implementación: los estilos que en desktop están en `:hover`, en mobile van **dentro del media query** con el selector duplicado `.elemento.is-visible { ... }` (el `&.is-visible` de SCSS NO existe en CSS puro).
- Los **observers se activan solo en mobile** (con `window.matchMedia('(max-width:768px)')`) para no ensuciar desktop.
- **REGLA DURA:** no romper desktop. TODO el CSS mobile va dentro del media query.

## ✅ Lo que se hizo (todo pusheado a `main` → Vercel)

### Splash (carga)
- Vertical en mobile (pin arriba + wordmark abajo): `#splash { flex-direction:column }`. Posición/tamaño afinados con el usuario (`#splash-logo { height:148px; left:6px }`, `#splash-wordmark { height:62px }`). El `transform:translateX` del grupo terminó en `none` (centrado).

### Nav + menú hamburguesa
- **Home:** nav vuelve al comportamiento de desktop (transparente sobre el hero, sólida crema al scrollear). Logo+isologo **centrados** en la barra (`.nav-logo { position:absolute; left:50%; transform:translate(-50%,-50%) }`), hamburguesa a la derecha, isologo visible (28px). El override viejo de "nav siempre sólida" se **eliminó**.
- **Menú overlay `#mobileMenu`:** fondo con el **degradé animado del hero** (se reusa `.hero-pattern` con clase extra `.mm-bg`; sus blobs `blobA/blobB` ya existen). Tipografía liviana (`font-body 20-21px/500`, NO Archivo Black). **Fade-in escalonado** de los items al abrir (`.mobile-menu.open .mobile-menu-links > *:nth-child(n)` con `transition-delay`).
- **Acordeones** (`.mm-acc`): Propiedades y Contacto despliegan **suave** (max-height + opacity, no `display:none`) con flecha naranja `▾` que rota. JS de abrir/cerrar + acordeón en el IIFE final.
- **`propiedades/index.html` (listado + ficha) AHORA TAMBIÉN tiene hamburguesa** (se replicó todo: `.hero-pattern/.mm-bg`, blobs, `.mobile-menu`, `.nav-hamb`, JS). En mobile el nav queda: **flecha "volver" a la izquierda + logo centrado + hamburguesa a la derecha**; el botón EN se mueve adentro del menú. Los links del menú apuntan a las secciones del home (`/#servicios`, etc.) + `/propiedades`.

### Hero (home)
- Las **4 ilustraciones SÍ se muestran** en mobile (ya no `display:none`), **intercaladas entre el typewriter y el subtítulo** (truco `display:contents` en `.hero-content` + `order`). El typewriter/SVG anima igual (no hay corte mobile en el JS).
- Tamaños/posiciones afinados por card: `#illus-0` (casa) 250×240 + más a la izq, `#illus-1` (edificio) 262×285, `#illus-2` (terreno) 245×235. **Trazo más fino**: `.illus-svg { stroke-width:3 }` (el stroke es non-scaling → 4.5 se veía grueso al achicar).
- **Fix llave que se trababa:** el recoil de `handIn()` usaba `translateX(-50px)` fijo y saltaba contra la base mobile. Ahora usa **`composite:'add'`** (delta relativo) → anda igual en desktop y mobile.

### Hover→scroll-trigger (lo que pedía el plan)
- **Servicios** (`.servicio-card.is-visible`): fondo + línea naranja + número naranja + `scale(1.03)`. El observer usa `rootMargin: '-25% 0px -55% 0px'` → se activa la card que cruza la **franja alta-central** (no todas juntas).
- **Esencia/Valores:** en mobile el observer arranca antes (threshold `0.25`, pausa `150ms` vs `0.7`/`500ms` en desktop) y el escalonado entre columnas es más rápido.
- **Sobre mí:** orden mobile **eyebrow → título → foto → bio → badges** (`display:contents` + `order`). La foto fadea **junto con el título** (no antes); el resto de la secuencia (typewriter, badges) intacta. Branch por `matchMedia`.

### WhatsApp flotante (home)
- En mobile: más chico (50px), **sin pulso**, y **aparece recién al scrollear** ~50% del hero (clase `.wa-show` que toggea `updateNav` con `scrollY > innerHeight*0.5`).

### Buscador (home + listado)
- Causa del look "raro": en columna heredaba `align-items:center` → filtros centrados y achicados. Fix: **`align-items:stretch`** (full-width, estilo formulario) + **separadores `.search-sep` horizontales full-width**.
- **Dropdowns abren para ABAJO** (antes para arriba, pisaban el hero) y ocupan el ancho del filtro sin desbordar (`left:0; right:0; min-width:0`; `rng-dropdown` sin `min-width:300`). Mismos arreglos en `propiedades/` (su breakpoint es 600px) para que queden idénticos.

### Carrusel destacadas (home) + swipe galería (ficha)
- **Destacadas:** `.propiedades-grid` en mobile pasa a **flex + scroll-snap horizontal** (card `flex:0 0 82%`, se asoma la siguiente). La card centrada recibe `.is-visible` (elevación) vía IntersectionObserver con `rootMargin:'0px -38% 0px -38%'`. Se engancha en `setupCardCarousel(grid)` dentro de `renderGrid`.
- **Ficha:** **swipe en la foto principal** (`mainEl` touchstart/touchend; distingue swipe de tap con `swMoved` para no abrir el lightbox). **Swipe también en el lightbox** (`#lightboxImg`). Los thumbnails ya eran scroll horizontal en mobile.

### Varios
- **`overflow-x:hidden` en `html`** (faltaba; `body` ya lo tenía) → mata el scroll lateral.
- **`<meta name="theme-color" content="#1e2e17">`** → tiñe la barra del navegador del celu con el verde de marca. ⚠️ La **barra de navegación del SISTEMA** (botones atrás/home de Android) NO se controla desde la web.

## Lo único que queda pendiente (menor)
- **FAQ:** mostrar el borde naranja del `:hover` cuando el item está `.open` en mobile (`.faq-item.open .faq-q-text::before { opacity:1 }`). El click/abrir ya funciona. Es lo último del plan original.

## Cómo prueba el usuario
- En el celu real → **codigo-vs.vercel.app** (por eso pide pushear; ahora **push directo** — ver memoria `feedback-push-directo`). **Trabaja con capturas**: mostrar, esperar OK visual, seguir. Itera mucho en posiciones/tamaños (px finos).

---

# Sesión — Valor del dólar configurable + conversión de monedas en el filtro

## Qué se agregó
- **Nueva colección en el admin "Configuración General"** (file collection, un solo archivo): `admin/config.yml` → colección `configuracion` con el archivo `content/config/general.json`. Campos:
  - `valor_dolar` (number/float, **obligatorio**) — cuántos pesos vale 1 dólar.
  - `fecha_actualizacion` (datetime, opcional, `DD/MM/YYYY`) — para que Antonella sepa cuándo lo actualizó.
- **Archivo inicial:** `content/config/general.json` (default `valor_dolar: 1350`).

## Cómo lo usa el sitio (`propiedades/index.html`)
- Se lee **EN VIVO desde la GitHub API** (`CONFIG_URL` → `content/config/general.json`), igual que las propiedades → si Antonella cambia el dólar, **se actualiza al toque sin esperar build de Vercel**.
- `var VALOR_DOLAR` + `cargarValorDolar()` (se llama al cargar el listado, en paralelo a las propiedades).
- `precioEnMoneda(precio, monedaProp, monedaObjetivo)` convierte: `USD→ARS = ×valor_dolar`, `ARS→USD = ÷valor_dolar`. Devuelve `null` si no se puede (sin cotización y monedas distintas).
- **Filtro de precio (`filtrarListing`):** antes **excluía** las propiedades cuya `moneda` ≠ la elegida. Ahora **convierte y compara** (una propiedad en USD aparece aunque busques en pesos, y viceversa). Esto es el "doble entrada para precio en cada moneda" que pidió el usuario.
- **Red de seguridad:** si `VALOR_DOLAR` no cargó o falla, `precioEnMoneda` devuelve `null` para monedas distintas → el filtro vuelve al comportamiento viejo (compara por moneda). Nada se rompe.
- En el **filtro** se convierte para comparar. En la **ficha** el precio queda en su moneda original.

## Precio convertido visible en las cards del listado
- En las **cards del listado** (`propiedades/index.html`, `renderCard`) se muestra el precio original y **debajo el equivalente aproximado en la otra moneda** con `≈` (línea `.prop-card-precio-conv`, más chica/gris). Helper `fmtPrecioConv(p)`.
- **Antonella decide por propiedad:** campo nuevo en el admin **`mostrar_precio_convertido`** (`widget: boolean`, **default `true`**). Si lo apaga → solo el precio original (ej: alquileres en pesos). Como las propiedades viejas no tienen el campo, `undefined !== false` → se muestra igual (default ON).
- La conversión usa `precioEnMoneda` + `VALOR_DOLAR`. Si no hay cotización → no muestra la línea ≈ (no rompe). Redondeo lindo: pesos al millar, dólares a la centena. Respeta el período de alquiler.
- Va **solo en las cards del listado** (no en la ficha por ahora, ni en el home).

## Botón "+ Propiedad" en naranja
- En `admin/index.html` el matcher de "Nueva Propiedad" se amplió para agarrar también **"+ Propiedad"** / "Propiedad" / "+ Property" y pintarlo de `#EE7A13` (Decap cambió el botón y el `data-testid="new-button"` ya no alcanzaba).

## Videos
- Se decidió **NO tocar** (queda el campo único "Link Reel / Video" actual).

## Período del precio (alquileres)
- Antes el precio mostraba **"/ mes" fijo** para cualquier alquiler. El turístico suele ser por noche/semana/quincena → se agregó un campo libre.
- **Admin (`config.yml`):** campo nuevo `periodo` (`widget: string`, texto libre, opcional, con ejemplos en el hint: mes, noche, día, semana, quincena, temporada). Va después de "Moneda".
- **Sitio (`index.html` + `propiedades/index.html`):** helper `periodoTxt(p)` → solo aplica a alquileres; limpia "/" y "por " inicial; si está vacío → "mes" (fallback = comportamiento viejo); traduce los términos comunes al inglés con un mapa (mes→month, noche→night, etc.). `fmtPrecio` muestra `precio / <periodo>` (ej: `U$S 80 / noche`). En la ficha, `sidebarPrecioSub` muestra `por <periodo>`.
- **Admin preview (`admin/index.html`):** su `fmtPrecio` también recibe `periodo` (solo español, sin mapa EN).
- ⚠️ El campo `periodo` aparece SIEMPRE en el admin (Decap no tiene campos condicionales simples); el hint aclara que es solo para alquileres y en ventas el sitio lo ignora.

## ⚠️ Deploy de esta sesión
- `admin/config.yml` + `admin/index.html` → necesitan **deploy manual de Netlify** (un solo "Trigger deploy"; quedan POCOS). `content/config/general.json` y `propiedades/index.html` → **Vercel** (push a `main`).

---

# Sesion — Traspaso a Antonella (01/09/2026): correcciones + precio convertido en todo el sitio

## ⚠️ CORRECCION de datos de este archivo — leer ANTES que las secciones de arriba
- **NO existe ninguna "cuenta de la facultad".** El site de Netlify (`unique-kitsune-f448c7`) esta en la
  **cuenta personal de Mati**. Las menciones de mas arriba a *"cuenta de la facultad con creditos renovados"*
  quedaron por historia y son **incorrectas**. No hay riesgo de que esa cuenta caduque, asi que **no hace
  falta migrar ni transferir nada** para entregarle el sitio a Antonella.
- Lo que SI sigue vigente de arriba: los **builds de Netlify estan pausados** a proposito, y por eso todo
  cambio en `admin/` necesita un **Trigger deploy manual**.

## Estado verificado el 01/09/2026 (comparando lo publicado contra el local)
- **Vercel (sitio publico): AL DIA.** `index.html` servido == local.
- **`admin/index.html`: AL DIA.** Las unicas diferencias son 5 lineas de `<meta>` que **Netlify inyecta sola**
  en el deploy (hosting-provider / netlify-deploy). No son cambios del proyecto.
- **`admin/config.yml`: DESFASADO.** Le faltaba unicamente el campo `mostrar_precio_convertido`
  (6 lineas). Se arregla con **un solo Trigger deploy** de Netlify.
  ⚠️ Importante: el **Manual PDF de Antonella ya documenta ese campo**, asi que hasta que se deploye
  ella lo busca en el formulario y **no lo encuentra**.

## Netlify: Git Gateway esta DEPRECADO (dato verificado en la doc oficial)
- Netlify **Identity NO** esta deprecado; **Git Gateway SI**. Textual: *"continua funcionando para los sites
  que ya lo tienen habilitado, pero no se recomiendan configuraciones nuevas"*, y ya **no arreglan bugs**
  (solo agujeros de seguridad graves).
- Consecuencia practica: **nunca recrear el site de Netlify desde cero** (habria que habilitar Git Gateway
  de nuevo = config nueva = deprecada). Si algun dia hay que cambiarlo de cuenta, **TRANSFERIR el site**,
  que se lleva Git Gateway con el: `Project configuration > General > Project information > Transfer project`.
  Requiere ser Team Owner en el origen, Owner/Developer en el destino, y un Owner/Developer compartido.
- Si alguna vez Git Gateway muere del todo, el plan B es Decap con **backend GitHub + OAuth** (OAuth App +
  dos funciones serverless `/api/auth` y `/api/callback` en el mismo Vercel). Eso ademas eliminaria Netlify
  y los deploys manuales. NO se hizo: no hace falta hoy.

## Precio convertido (≈) — ahora en TODO el sitio
Antes la linea "≈ <precio en la otra moneda>" salia **solo en las cards del listado**. Ahora esta en los
**tres** lugares. La logica ya era simetrica y no se toco: da igual si Antonella carga en USD o en ARS,
siempre muestra **la otra** moneda (`destino = origen === 'ARS' ? 'USD' : 'ARS'`), y se apaga por propiedad
con el interruptor `mostrar_precio_convertido` (default: encendido).

- **Ficha** (`propiedades/index.html`): nuevo `<div id="sidebarPrecioConv">` debajo del precio + CSS
  `.prop-precio-conv` (con `:empty { display:none }`).
  ⚠️ **Bug que se arreglo de paso:** `cargarValorDolar()` se llamaba **solo dentro de `if (isListing)`**,
  asi que en la ficha `VALOR_DOLAR` quedaba `null` y no se podia convertir nada. Ahora la ficha la pide
  y pinta el ≈ cuando resuelve.
- **Home** (`index.html`): no tenia **nada** de la maquinaria de cotizacion. Se porto igual que el listado
  (`CONFIG_URL`, `VALOR_DOLAR`, `cargarValorDolar()`, `precioEnMoneda()`, `fmtPrecioConv()`) + CSS
  `.prop-card-precio-conv`. La cotizacion arranca **en paralelo** al fetch de propiedades y se espera
  recien antes de renderizar (`dolarReady`), para que las cards salgan ya con el ≈ y no haya reflow.
- ⚠️ **OJO al editar precios en `index.html`:** el home usa **espacios no separables (NBSP, U+00A0)**
  dentro de los strings de precio (`'$ '`, `' / '`). El listado usa espacios normales.
  Si se hace un search/replace con espacios comunes **no matchea**.
- Si no hay cotizacion cargada (o falla el fetch), `fmtPrecioConv` devuelve `''` y simplemente no se
  muestra la linea. Nada se rompe.

## Manual PDF de Antonella (ya hecho por Mati)
- Cubre: entrar al panel, cargar/editar/eliminar propiedades, valor del dolar, coordenadas de Google Maps,
  demoras de publicacion y errores. Usuario: `inmobiliaria@solarprop.com.ar`.
- **Pendiente sugerido:** el PDF trae la contrasena en texto plano. Conviene que diga *"contrasena
  provisoria — cambiala al primer ingreso"* en vez del valor fijo.
- El PDF imprime la URL del admin (`unique-kitsune-f448c7.netlify.app/admin/`). Si alguna vez se transfiere
  el site, **verificar que el subdominio siga igual antes de reimprimir** el manual.

## Pendiente concreto para cerrar la entrega
1. **Trigger deploy de Netlify** → para que aparezca `mostrar_precio_convertido` en el panel.
2. Push a `main` → Vercel publica el ≈ en home y ficha.
3. Decidir que hacer con la propiedad de prueba (`content/propiedades/lo-615.md`, "Casa Chica Barrio Norte",
   codigo `000101`). **No se borro** por pedido de Mati.

## ⚠️ CUENTAS DE NETLIFY — dato clave (03/09/2026)
Hay **DOS sites de Netlify**, los dos conectados a `Maticornara/CODIGO_VS` y los dos con Identity +
Git Gateway funcionando:
- **`unique-kitsune-f448c7`** ← **ESTE es el del manual de Antonella. El que se usa.**
- **`sunny-blancmange-47869e`** ← el "viejo". NO esta muerto: tambien funciona y quedo al dia.

**La cuenta de Netlify que administra los sites es `archivoscornara@gmail.com` (Matias Cornara).**
Se perdio un rato el acceso por no recordar con que cuenta se habia creado `unique-kitsune`.
NO es "cuenta de la facultad" (ese dato del archivo es erroneo).

- El 03/09/2026 se deployo `unique-kitsune` y quedo **al dia** (config.yml = 6376 bytes, con
  `mostrar_precio_convertido`). Verificado contra GitHub.
- Sintoma a reconocer: si se deploya y el contenido publicado no cambia, chequear que se este
  deployando **el site correcto** (son parecidos y apuntan al mismo repo).

## ⚠️ PENDIENTE DE SEGURIDAD — registro abierto
Los **dos** sites tienen `"disable_signup": false`, o sea **cualquiera que conozca la URL del panel
puede registrarse solo** y, via Git Gateway, editar las propiedades. La URL esta impresa en el manual.
**Cerrar en AMBOS:** Identity > Settings and usage > Registration preferences > **Invite only**.

---

# Sesion — Open Graph (compartir por WhatsApp) + footer unificado + fixes

## Open Graph: /p/<slug> (PRIMERA pieza de backend del proyecto)
**El problema:** WhatsApp/Instagram/Facebook **no ejecutan JS**. Como las fichas se arman en el
navegador leyendo GitHub, el robot veia una pagina vacia y mostraba el link pelado, sin foto.

**La solucion:** ruta nueva **`/p/<slug>`** servida por **`api/p.js`** (funcion serverless de Vercel,
la unica del proyecto). Lee el `.md` de la propiedad de `raw.githubusercontent.com` y devuelve el
**mismo HTML de siempre** con `<title>`, `description`, `og:*` y `twitter:*` ya escritos.

- **`vercel.json`:** `{"source":"/p/:slug","destination":"/api/p?slug=:slug"}` **antes** del rewrite
  de `/propiedades/`. ⚠️ **No se puede interceptar `/propiedades/`**: los rewrites de Vercel se
  aplican DESPUES del filesystem, y ese path ya resuelve a un archivo real. Por eso la ruta aparte.
- **El navegador recibe el HTML normal** y el JS del cliente arma la ficha solo, porque `pathSlug`
  ya sabia leer el slug del path (`/p/casa-x` -> `casa-x`). No hubo que tocar esa logica.
- **`history.replaceState`** en la ficha cambia la barra a `/p/<slug>`, asi lo que Antonella copia
  del navegador (y el link del mensaje de WhatsApp, que usa `location.href`) ya es el compartible.
- **Cache:** `s-maxage=300, stale-while-revalidate=86400`. Importante: **no usar la GitHub API**
  desde el servidor (60 req/hora por IP, y las IPs de Vercel son compartidas). Por eso `raw.`.
- **RED DE SEGURIDAD:** si GitHub falla, la propiedad no existe o el `.md` viene raro, devuelve la
  pagina tal cual. Si ni siquiera consigue el HTML base, redirige 302 a `/propiedades/?p=<slug>`.
  La ficha nunca se rompe.
- El parser de frontmatter esta **duplicado** en `api/p.js` (no hay build step ni modulos
  compartidos). Si cambia el formato del `.md`, hay que tocarlo en los dos lados.

## Open Graph estatico (home y listado)
`index.html` y `propiedades/index.html` tienen `description` + `og:*` + `twitter:*` fijos.
Imagen: **`fotos/ISOLOGO.PNG` (1465x585)** — es un parche. **Lo ideal es una imagen 1200x630**
disenada; si se agrega, guardarla como `fotos/og-image.png` y cambiar la constante en los 3 lugares
(los dos HTML y el `IMG_FALLBACK` de `api/p.js`).

## Footer unificado + ano dinamico
- El footer de `/propiedades` ahora es **igual al del home** (copy izquierda + links derecha). Los
  links apuntan al home (`/#servicios`, etc.) porque es otra pagina.
- **El ano ya no esta hardcodeado** (decia 2025 en 2026): `<span class="footer-year">` que llena
  `new Date().getFullYear()` en las dos paginas.

## Otros fixes
- **FAQ mobile:** el reborde naranja ahora se ve cuando el item esta `.open` (en mobile no hay
  hover). Era el ultimo pendiente del plan mobile.
- **Boton de WhatsApp:** ya no tapa el footer. IntersectionObserver sobre `footer`; cuando entra,
  el JS sube el `bottom` del boton y despues lo devuelve. Transicion suave por CSS.
- **Propiedades similares:** NO habia nada que arreglar. `renderSimilares` ya ordena por parecido
  y muestra las 3 primeras **sin exigir un minimo**. Hoy no aparecen porque hay **una sola**
  propiedad cargada; con dos o mas salen solas.
- **Seccion Servicios:** revisada, esta completa y traducida (01 Venta de Casas, 02 Venta de Lotes,
  03 Alquileres, 04 Tasaciones). El "nunca revisada" de las notas viejas ya no aplica.

## Estado de la entrega a Antonella (cerrada)
- Sitio (Vercel) y panel (`unique-kitsune`) **al dia y verificados**.
- **Registro cerrado (Invite only) en los DOS sites de Netlify.** Ya no hay riesgo de que un
  desconocido se anote.
- **`inmobiliaria@solarprop.com.ar` YA RECIBE MAILS** (estaba a medio configurar en Microsoft 365).
- Cuentas nuevas creadas para el traspaso, todas con **`solarpropiedades@outlook.com.ar`**
  (a proposito: NO depende del dominio, que es la pieza mas fragil). GitHub: **`solarpropiedades`**.
  La casilla de Outlook la tienen **Mati y Antonella**.
- ⚠️ **La transferencia NO se hizo todavia**, y se decidio dejarla para despues de la entrega:
  transferir el repo obliga a **reconectar Git Gateway**, que es lo deprecado. No hacerlo el dia
  antes de mostrarle el sitio.

## ⚠️ DOMINIO PROPIO — el sitio vive en www.solarprop.com.ar
- **`https://www.solarprop.com.ar`** es el dominio **canonico y publico**. `solarprop.com.ar` (sin
  www) redirige 308 al www. `codigo-vs.vercel.app` **sigue respondiendo** y sirve lo mismo.
- Como los dos dominios sirven el mismo contenido, hay que ser prolijo con el canonico o Google lo
  toma como contenido duplicado. Por eso: `og:url`, `og:image`, `twitter:image` y un
  **`<link rel="canonical">`** en las dos paginas, todos al dominio propio.
- **`api/p.js` sobreescribe el canonical por ficha** (`/p/<slug>`), porque el html base trae el de
  `/propiedades` y si no todas las fichas se pisarian entre si.
- Para el fetch del html base, `api/p.js` usa el **host real de la request**, no el dominio fijo:
  asi funciona igual en el dominio propio, en el .vercel.app y en los previews.
- ⚠️ Si se agrega una URL absoluta nueva en algun lado, usar **www.solarprop.com.ar**.
- El **manual de Antonella** deberia decir el dominio propio, no la URL de Vercel.

---

# Sesion — TRASPASO EJECUTADO (04/09/2026)

## Estado final: quien es dueno de que
- **GitHub `solarpropiedades/CODIGO_VS`** ← el repo se TRANSFIRIO. Aca viven las propiedades y las
  fotos. `Maticornara` quedo como **colaborador Admin**. La URL vieja redirige, pero solo para
  LECTURA: las escrituras al nombre viejo fallan (asi se rompio Git Gateway).
- **Vercel `solarpropiedades-5496`** ← proyecto `codigo-vs` + dominio `solarprop.com.ar`.
- **Netlify `archivoscornara@gmail.com`** ← ⚠️ **NO se pudo transferir**: Netlify (y Vercel) piden
  **plan pago** para agregar miembros a un team, y sin eso no dejan transferir. El panel sigue en la
  cuenta de Mati. Antonella entra igual con su usuario de Identity; lo que no puede es administrar
  el site.
- Cuentas nuevas: mail `solarpropiedades@outlook.com.ar` (lo tienen Mati y Antonella).

## Como se hizo Vercel (no se puede transferir el proyecto entre cuentas Hobby)
1. Crear el proyecto NUEVO importando el repo desde la cuenta destino.
2. ⚠️ **Desactivar "Vercel Authentication"** (Deployment Protection): viene ENCENDIDA en proyectos
   nuevos y hace que el sitio pida login a los visitantes.
3. Mover el dominio: Domains > ⋯ > **Move** > escribir el **slug** a mano (`solarpropiedades-5496`).
   El buscador dice "No results" porque solo lista teams propios — **igual hay que darle Continue**.
4. Quitar el dominio del proyecto viejo y conectarlo al nuevo.
5. ⚠️ Al conectarlo pide **verificar la propiedad**: hay que agregar registros TXT `_vercel`
   (uno para el apex y otro para el `www`, con valores distintos). Se agregan en DNS Records.
6. Dejar `www` como Production y el apex redirigiendo a www con **308**.

## Como se arreglo Git Gateway (esto es lo importante)
Al transferir el repo, el panel dejo de publicar: **"API_ERROR: Requires authentication"**.
- Cambiar el token NO alcanza: el campo `Repository` de Git Gateway **no es editable** y seguia
  apuntando al repo viejo.
- La secuencia que SI funciono:
  1. Netlify > el site > **relinkear el repo** al nuevo (`solarpropiedades/CODIGO_VS`),
     instalando la app de Netlify en la cuenta de GitHub `solarpropiedades`.
  2. Identity > Services > **Disable Git Gateway** y volver a **habilitarlo**. Recien ahi toma el
     repo nuevo (no se actualiza solo).
  3. Volver a **pausar los builds** de Netlify.
- Verificado: el commit `Create Propiedad "54"` lo hizo `inmobiliaria@solarprop.com.ar`.

## Respaldos que quedaron en el Escritorio
- `CODIGO_VS-backup.git` — clon completo del repo antes de transferir.
- `DNS-BACKUP-solarprop.txt` — los registros del correo (MX/SPF/autodiscover). **Sobrevivieron
  al Move**, pero conviene tenerlos.

## Pendientes
- **Borrar la propiedad de prueba "54"** que quedo de la verificacion.
- El proyecto viejo de Vercel (cuenta de Mati) quedo sin dominio; se puede borrar.
- Si algun dia se quiere cerrar del todo: cambiar el mail de la cuenta de Netlify de Mati a
  `solarpropiedades@outlook.com.ar` (hay que liberar antes ese mail borrando la cuenta de Netlify
  vacia), o migrar Decap a **backend GitHub + OAuth** y sacar Netlify del proyecto.

## Como completar la transferencia de Netlify el dia de manana
La cuenta de Netlify `solarpropiedades@outlook.com.ar` **se deja creada y vacia a proposito**: es el
destino si alguna vez se transfiere el site. NO borrarla.

Netlify solo permite transferir sites entre cuentas si se pueden agregar miembros al team, y eso
requiere plan pago. Camino barato para cerrarlo:
1. Pagar **un mes** de Netlify Pro (~19 USD) en cualquiera de las dos cuentas.
2. Invitar al otro como **Owner** del team.
3. Transferir el site (`Project configuration > General > Transfer project`).
4. Cancelar el plan y volver a gratis. El site ya queda del otro lado.

⚠️ Ojo: al transferir hay que **rehacer Git Gateway** del mismo modo que el 04/09/2026
(relinkear repo > Disable > Enable > pausar builds). Ver esa seccion.
