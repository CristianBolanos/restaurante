# Sitio web del restaurante

Landing page profesional para un restaurante, orientada a **convertir visitas en reservas**
(formulario que abre WhatsApp con la solicitud lista) y construida con **Astro + TypeScript +
Tailwind CSS**, lista para publicarse en **GitHub Pages**.

> **Estado del contenido:** el diseño, el código y los textos persuasivos están terminados.
> Los datos propios del negocio (nombre, dirección, horario, carta, precios…) aparecen como
> placeholders entre corchetes, por ejemplo `[Nombre del restaurante]`. Ver
> [Contenido pendiente](#contenido-pendiente).

---

## Características

- **Flujo de conversión:** Impacto → interés → deseo → confianza → reserva, con CTA de reserva y
  WhatsApp en cabecera, hero, carta, experiencia, formulario, CTA final, pie y botón flotante.
- **Reservas sin backend:** el formulario valida nombre, fecha (no pasada), hora y personas, y abre
  WhatsApp con un mensaje redactado. Si el navegador bloquea la ventana, ofrece un enlace alternativo.
- **Secciones:** cabecera fija con menú móvil accesible, hero, propuesta de valor, platos
  destacados, carta por pestañas, experiencia, galería con visor, testimonios (solo si hay reseñas
  reales), reservas, ubicación con mapa bajo demanda, CTA final y pie de página.
- **Rendimiento:** HTML estático, ~9 KB de JavaScript en total (≈5 KB comprimido), fuentes
  autoalojadas y precargadas con métricas de respaldo, imágenes AVIF/WebP responsivas con dirección
  de arte, `content-visibility` en secciones fuera de pantalla (con saltos a anclas exactos) y
  Google Analytics cargado después del evento `load`.
- **SEO técnico:** title, description, canonical, Open Graph, Twitter Cards, sitemap, robots.txt,
  manifest y datos estructurados Schema.org generados automáticamente (solo con datos reales).
- **Accesibilidad:** HTML semántico, navegación por teclado, foco visible, contraste AA verificado,
  etiquetas y mensajes de error accesibles, `prefers-reduced-motion` y enlace "Saltar al contenido".

## Stack

| Herramienta | Uso |
| --- | --- |
| [Astro 7](https://astro.build) | Framework, generación estática, optimización de imágenes y fuentes |
| TypeScript (modo estricto) | Datos tipados y scripts del cliente |
| [Tailwind CSS 4](https://tailwindcss.com) | Sistema de diseño (tokens en `src/styles/global.css`) |
| `@astrojs/sitemap` | Sitemap automático |
| `sharp` | Procesamiento de imágenes en el build |
| GitHub Actions + Pages | Despliegue continuo |

Sin React, Vue ni librerías de UI: componentes `.astro` y JavaScript mínimo.

## Requisitos

- **Node.js 22.12 o superior**
- **pnpm 10** (el proyecto fija la versión en `package.json` → `packageManager`)

```bash
corepack enable pnpm
```

## Inicio rápido

```bash
pnpm install
```

```bash
pnpm dev
```

```bash
pnpm build
```

```bash
pnpm preview
```

| Script | Descripción |
| --- | --- |
| `pnpm dev` | Servidor de desarrollo en `http://localhost:4321` |
| `pnpm build` | Verifica tipos (`astro check`) y genera el sitio en `dist/` |
| `pnpm preview` | Sirve el contenido de `dist/` para revisarlo antes de publicar |
| `pnpm check` | Solo la verificación de TypeScript y plantillas |

## Estructura

```text
.
├── .github/workflows/deploy.yml   # Publicación automática en GitHub Pages
├── public/                        # Favicon, iconos y archivos servidos tal cual
├── src/
│   ├── assets/
│   │   ├── fonts/                 # Fraunces y Manrope (woff2, licencia OFL)
│   │   └── images/                # Fotografías (se optimizan en el build)
│   ├── components/
│   │   ├── ui/                    # Botón, icono, logo, encabezados, imagen con dirección de arte…
│   │   ├── Header.astro           # Cabecera fija + menú móvil (<dialog>)
│   │   ├── Hero.astro
│   │   ├── Features.astro         # Propuesta de valor
│   │   ├── FeaturedDishes.astro   # Platos destacados (DishCard.astro)
│   │   ├── Menu.astro             # Carta con pestañas accesibles
│   │   ├── Experience.astro
│   │   ├── Gallery.astro          # Cuadrícula bento + visor
│   │   ├── Testimonials.astro     # Solo se muestra con reseñas reales
│   │   ├── Reservation.astro      # Sección de reservas (ReservationForm.astro)
│   │   ├── Location.astro         # Datos de contacto + MapEmbed.astro
│   │   ├── FinalCTA.astro
│   │   ├── Footer.astro
│   │   ├── WhatsAppFloat.astro
│   │   ├── SEO.astro              # Meta etiquetas
│   │   └── Analytics.astro        # Google Analytics 4
│   ├── data/                      # ← Toda la información editable del negocio
│   │   ├── restaurant.ts          # Nombre, contacto, dirección, horario, redes, reservas
│   │   ├── menu.ts                # Carta y platos destacados
│   │   ├── content.ts             # Textos e imágenes de cada sección
│   │   └── testimonials.ts        # Reseñas reales (vacío por defecto)
│   ├── layouts/Layout.astro
│   ├── lib/                       # Lógica: enlaces, formatos, Schema.org, placeholders
│   ├── pages/                     # index, privacidad, 404, robots.txt, site.webmanifest
│   ├── scripts/analytics.ts       # Medición de eventos
│   └── styles/global.css          # Tokens de diseño y estilos base
├── astro.config.mjs
├── .env.example
└── package.json
```

## Personalización

### 1. Datos del negocio — `src/data/restaurant.ts`

Es el archivo principal. Reemplaza cada placeholder `[...]` por la información real:

| Campo | Ejemplo de formato |
| --- | --- |
| `name`, `cuisine`, `tagline` | `"Casa Brasa"`, `"Cocina de autor"`, `"Cocina de autor en Medellín"` |
| `description` | Frase de 140–160 caracteres para Google y redes sociales |
| `priceRange`, `currency`, `locale` | `"$$"`, `"COP"`, `"es-CO"` |
| `contact.phone`, `contact.email` | `"+57 300 000 0000"`, `"reservas@turestaurante.com"` |
| `address.*`, `geo` | Dirección completa y coordenadas (mejoran el mapa y el SEO local) |
| `hours` | Horas en formato 24 h (`"12:00"`); `closed: true` para días cerrados |
| `hoursSummary` | `"Martes a domingo · 12:00 – 23:00"` |
| `social[].url` | URL completa del perfil; mientras sea placeholder no se enlaza |
| `links.maps` / `links.mapEmbed` | Opcionales: ficha de Google Maps y URL de "Insertar un mapa" |
| `links.menuPdf` | Opcional: carta en PDF (colócala en `public/` y escribe `"carta.pdf"`) |
| `reservation.timeSlots` | Opcional: franjas horarias, p. ej. `["13:00", "13:30", "20:00"]` |

### 2. Carta — `src/data/menu.ts`

Categorías, platos y platos destacados. `price` acepta un número (`32000` → `$ 32.000` con la
moneda configurada) o un texto (`"Según mercado"`). Las categorías por defecto son Entradas,
Platos fuertes, Postres y Bebidas: añade, elimina o renombra según la carta real.

### 3. Textos — `src/data/content.ts`

Copys de cada sección. Completa los textos entre corchetes (historia, ambiente, propuesta
gastronómica e indicaciones para llegar).

### 4. Testimonios — `src/data/testimonials.ts`

Agrega solo reseñas reales y verificables. Mientras la lista esté vacía, la sección no aparece.

### 5. Fotografías — `src/assets/images/`

Las fotos actuales son de stock: **reemplázalas por fotos reales del restaurante** para generar
confianza (y no mostrar platos que no se sirven). Conserva el nombre del archivo o actualiza el
import en `src/data/`. Astro genera automáticamente las versiones AVIF/WebP en varios tamaños.

| Archivo | Uso | Proporción recomendada |
| --- | --- | --- |
| `hero-escritorio.jpg` / `hero-movil.jpg` | Fondo del hero (horizontal / vertical) | 3:2 (≥ 2400 px) / ~5:8 (≥ 1000 px) |
| `cta-escritorio.jpg` / `cta-movil.jpg` | Fondo del CTA final | 2:1 / 4:5 |
| `plato-1…4.jpg` | Platos destacados | 4:5 (≥ 900 px de ancho) |
| `carta-*.jpg` | Imagen de cada categoría de la carta | 4:5 |
| `experiencia-cocina.jpg` / `experiencia-salon.jpg` | Composición de la sección Experiencia | 4:5 / 3:2 |
| `galeria-*.jpg` | Galería (las verticales ocupan dos filas) | Libre |

### 6. Colores y tipografías

- **Colores:** tokens en `src/styles/global.css` (`@theme`). La paleta actual cumple contraste AA.
- **Tipografías:** Fraunces (títulos) y Manrope (texto), autoalojadas en `src/assets/fonts/` y
  configuradas en `astro.config.mjs` → `fonts`.

### 7. Logo e iconos

- El logotipo es tipográfico (`src/components/ui/Logo.astro`); puedes sustituir el emblema por tu logo.
- Reemplaza `public/favicon.svg`, `favicon.ico`, `apple-touch-icon.png`, `icon-192.png` e
  `icon-512.png` por los de tu marca.

## Contenido pendiente

Cualquier texto entre corchetes `[...]` es un dato pendiente:

- Se muestra tal cual en la página para que sea fácil detectarlo.
- **Nunca** se publica en los datos estructurados ni se convierte en enlace.
- Al ejecutar `pnpm build`, la consola lista todos los pendientes y las variables sin configurar:

```text
[contenido] 81 dato(s) pendiente(s) por completar (texto entre corchetes):
  • src/data/restaurant.ts (24)
      - restaurant.name
      - restaurant.contact.phone
      …
```

## Variables de entorno

Copia `.env.example` como `.env` para desarrollo local:

| Variable | Descripción |
| --- | --- |
| `PUBLIC_WHATSAPP_NUMBER` | Número para reservas, formato internacional solo con dígitos (p. ej. `57XXXXXXXXXX`). Sin número, WhatsApp abre el mensaje y permite elegir el contacto. |
| `PUBLIC_GA_ID` | ID de medición de Google Analytics 4 (`G-XXXXXXXXXX`). Vacío = sin analítica. |
| `SITE_URL`, `BASE_PATH` | Solo para builds manuales; en GitHub Actions se calculan automáticamente. |

En GitHub, define `PUBLIC_WHATSAPP_NUMBER` y `PUBLIC_GA_ID` en
**Settings → Secrets and variables → Actions → Variables**.

## Reservas por WhatsApp

1. El visitante completa nombre, fecha, hora, personas y comentarios.
2. El sitio valida los datos y abre `wa.me` con el mensaje predefinido
   (`Hola, quiero realizar una reserva en el restaurante.`) más los detalles en negrita.
3. El restaurante confirma por el mismo chat.

Los datos no se almacenan en el sitio. Si en el futuro se incorpora un sistema de reservas o un
backend, basta con reemplazar el manejador `submit` de `src/components/ReservationForm.astro`.

## Google Analytics 4

Con `PUBLIC_GA_ID` configurado se miden estos eventos (con el parámetro `location` para saber qué
botón se usó):

| Evento | Cuándo |
| --- | --- |
| `reserve_click` | Clic en cualquier botón "Reservar" |
| `whatsapp_click` | Clic en un enlace de WhatsApp |
| `reservation_submit` | Envío válido del formulario de reserva (`party_size`) |
| `menu_click` | Clic en "Ver menú" / "Ver la carta completa" |
| `menu_category_view` | Cambio de categoría en la carta |
| `phone_click`, `email_click`, `directions_click`, `social_click` | Contacto, mapa y redes |
| `gallery_open`, `map_load`, `menu_pdf_click`, `reviews_click` | Interacciones de contenido |

Recomendado: marca `reservation_submit` y `whatsapp_click` como **eventos clave** (conversiones)
en GA4 → Administrar → Eventos clave. Para medir nuevos botones, añade
`data-track="nombre_evento"` y opcionalmente `data-track-location="zona"`.

> Si tu público está en la Unión Europea u otra jurisdicción que exija consentimiento previo para
> cookies analíticas, añade un banner de consentimiento antes de activar GA4.

## SEO

- Metadatos completos en `src/components/SEO.astro` y `src/layouts/Layout.astro`.
- Imagen para compartir (1200 × 630) generada a partir de la foto del hero.
- Schema.org `Restaurant` (que hereda de `LocalBusiness` y `Organization`) con `PostalAddress`,
  `GeoCoordinates`, `OpeningHoursSpecification`, `Menu`/`MenuItem`/`Offer` y `sameAs`, generado
  en `src/lib/schema.ts` **solo con datos reales**: mientras el nombre sea un placeholder no se publica.
- Después de publicar: verifica el sitio en Google Search Console, envía `sitemap-index.xml` y
  crea o actualiza tu ficha de Google Business Profile con los mismos datos.

> En un sitio de proyecto (`usuario.github.io/repositorio`) los buscadores solo leen el
> `robots.txt` de la raíz del dominio; con un dominio propio el `robots.txt` del sitio se usa
> directamente.

## Despliegue en GitHub Pages

1. Crea un repositorio en GitHub y sube el proyecto:

```bash
git add .
```

```bash
git commit -m "Sitio web del restaurante"
```

```bash
git remote add origin https://github.com/USUARIO/REPOSITORIO.git
```

```bash
git push -u origin main
```

2. En el repositorio: **Settings → Pages → Build and deployment → Source: GitHub Actions**.
3. (Opcional) Define las variables `PUBLIC_WHATSAPP_NUMBER` y `PUBLIC_GA_ID`.
4. Cada push a `main` ejecuta `.github/workflows/deploy.yml`: verifica tipos, compila y publica.
   La URL (`site`) y la subruta (`base`) se calculan automáticamente, por lo que no hay rutas rotas
   ni con `usuario.github.io/repositorio` ni con un dominio propio.

**Dominio propio:** configúralo en Settings → Pages → Custom domain. El workflow detecta el nuevo
origen y la base vacía en el siguiente despliegue.

## Calidad verificada

Auditoría realizada sobre el build de producción con datos de prueba completos y la subruta de
GitHub Pages:

| Prueba | Resultado |
| --- | --- |
| `astro check` (TypeScript + plantillas) | 0 errores, 0 advertencias |
| Lighthouse móvil (4G lento simulado, CPU ×4) | Rendimiento 96–97 · Accesibilidad 100 · Buenas prácticas 100 · SEO 100 |
| Lighthouse escritorio | Rendimiento 100 · Accesibilidad 100 · Buenas prácticas 100 · SEO 100 |
| Métricas de laboratorio (móvil) | FCP 1,2 s · LCP 2,3 s · TBT 0–20 ms · CLS 0 |
| Responsive 320 / 375 / 390 / 414 / 768 / 1024 / 1280 / 1440 px | Sin scroll horizontal, imágenes rotas ni objetivos táctiles pequeños |
| Interacción (menú móvil, foco, pestañas por teclado, formulario, visor) | 20 de 20 pruebas superadas |
| Saltos a anclas (clic, enlace profundo, movimiento reducido, sin *scroll anchoring*) | Desvío 0 px y sin saltos al volver arriba |
| Rutas con base `/repositorio/` (assets, sitemap, robots, 404) | Sin rutas rotas |
| Contraste de color (WCAG 2.2) | Todos los pares de texto ≥ 4,5:1 |

## Créditos y licencias

- **Fotografías:** [Unsplash](https://unsplash.com/license) (uso comercial gratuito). Originales:
  `images.unsplash.com/photo-{id}` con los siguientes identificadores:

| Archivo | ID de Unsplash |
| --- | --- |
| hero-escritorio / hero-movil | `1414235077428-338989a2e8c0` |
| cta-escritorio / cta-movil | `1578474846511-04ba529f0b88` |
| plato-1 | `1558030006-450675393462` |
| plato-2 | `1519708227418-c8fd9a32b7a2` |
| plato-3 | `1563379926898-05f4575a45d8` |
| plato-4 | `1551024506-0bccd828d307` |
| carta-entradas | `1540189549336-e6e99c3679fe` |
| carta-fuertes | `1615937657715-bc7b4b7962c1` |
| carta-postres | `1565958011703-44f9829ba187` |
| carta-bebidas | `1536935338788-846bb9981813` |
| experiencia-salon | `1517248135467-4c7edcad34c4` |
| experiencia-cocina | `1581349485608-9469926a8e5e` |
| galeria-barra | `1543007630-9710e4a00a20` |
| galeria-emplatado | `1595257841889-eca2678454e2` |
| galeria-brindis | `1510812431401-41d2bd2722f3` |
| galeria-ensalada | `1514516345957-556ca7d90a29` |
| galeria-cocina | `1577219491135-ce391730fb2c` |
| galeria-coctel | `1514362545857-3bc16c4c7d1b` |
| galeria-comensales | `1592861956120-e524fc739696` |
| galeria-salon | `1537047902294-62a40c20a6ae` |

- **Tipografías:** Fraunces y Manrope, SIL Open Font License 1.1 (licencias en `src/assets/fonts/`).
- **Iconos:** basados en [Lucide](https://lucide.dev) (ISC) y [Simple Icons](https://simpleicons.org) (CC0).

## Notas de mantenimiento

- `pnpm audit` reporta un aviso en `http-cache-semantics` (dependencia interna de Astro) sin versión
  corregida publicada. Afecta a cachés HTTP compartidas en servidores; no aplica a un sitio estático.
- Si cambias imágenes, el primer build tarda algo más (genera las variantes); los siguientes usan
  la caché de `node_modules/.astro` (también se reutiliza en GitHub Actions).
