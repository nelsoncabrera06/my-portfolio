# AGENTS.md — Nelson Cabrera Portfolio

> **Este archivo es la memoria compartida del proyecto.** Si sos un agente de IA (o un
> humano) retomando este repo, leelo **antes de tocar nada**. No hace falta que adivines
> nada: acá está el mapa, las convenciones y el estado del trabajo.
>
> Al final de cada sesión de trabajo, **agregá una entrada al Log de Sesiones**. Ese log
> es lo que permite que el próximo agente (o el Nelson en 3 meses) sepa qué se hizo y qué
> falta, sin depender de la conversación anterior.

---

## 1. Qué es este proyecto

Portfolio personal de **Nelson Cabrera — Release Deployment Engineer | DevOps | CI/CD Automation**.

- Sitio estático de una sola página, **sin build step, sin package manager, sin framework**.
- Se publica en **GitHub Pages** desde la rama `main`, en la raíz del repo.
- URL pública: <https://nelsoncabrera06.github.io/my-portfolio/>
- Tema: **dark tech / terminal** (`$ whoami`, cursores, prompts, monoespaciada para lo técnico).
- Idioma del contenido: **inglés**. Idioma de las notas internas de este archivo: español.

**No introduzcas frameworks, bundlers, npm ni pasos de build.** Es un sitio estático a
propósito: se abre `index.html` y funciona. Esa simplicidad es una decisión, no una carencia.

---

## 2. Protocolo de arranque (qué leer al empezar una sesión)

En este orden:

1. **Este `AGENTS.md`** — completo, incluidos los acoplamientos críticos y el log.
2. **`git log --oneline -20`** — el historial es corto y descriptivo; es el mejor resumen de
   "qué se estaba haciendo".
3. **`dev_local/`** — carpeta **gitignored**, scratch space de Nelson para ideas, TODOs y
   notas de trabajo. Puede contener cosas que ya se implementaron: **verificá contra el código**
   antes de arrancar algo. Hoy contiene `Cosas para mejorar.txt` con 2 ítems que ya están
   completos (ver §8).
4. **Solo si el trabajo es puntual**, abrir `index.html` / `css/style.css` / `js/main.js`.

Si vas a cambiar el sitio, el flujo correcto es:

```
leer este archivo → revisar el log → hacer el cambio → verificar en browser → commit → actualizar el log
```

---

## 3. Mapa del proyecto

```
my-portfolio/
├── index.html          # TODO el contenido y markup (1187 líneas). Single page.
├── css/
│   └── style.css       # TODO el styling (1781 líneas). Ordenado por bloques commented.
├── js/
│   └── main.js         # Nav, animaciones on-scroll, smooth scroll (229 líneas)
├── assets/
│   └── images/         # laptop.png (favicon), profile.png
├── dev_local/          # gitignored — notas personales, NO es parte del sitio
├── README.md           # público: stack, live demo, contacto
├── .gitignore
└── AGENTS.md           # este archivo
```

### Anclas dentro de `index.html` (números de línea de referencia, pueden derivar)

| Línea | Bloque |
|---|---|
| 1–36   | `<head>`: meta, OG tags, fuentes, CDN, favicon |
| 39–58  | Nav (`<nav class="nav" id="nav">`) con `.nav-menu` / `.nav-link` |
| 61–101 | Hero (`id="home"`) |
| 104    | `section.about` → `#about` (01.) |
| 150    | `section.experience` → `#experience` (02.) |
| 280    | `section.skills` → `#skills` (03.) |
| 454    | `section.ai` → `#AI` (04.) — **id en mayúsculas** |
| 588    | `div.ai-cert-section` → `#ai-certifications` (sub-bloque dentro de AI) |
| 778    | `section.projects` → `#projects` (05.) |
| 1037   | `section.education` → `#education` (06.) — incluye subsection de certificaciones |
| 1135   | `section.contact` → `#contact` (07.) |
| 1174   | `<footer class="footer">` (copyright `© 2024`) |
| ~1185  | `<script src="js/main.js">` |

### Secciones numeradas (el número es manual, hay que renumerar a mano)

`01. About Me` · `02. Experience` · `03. Skills` · `04. AI & Generative AI` ·
`05. Projects` · `06. Education` · `07. Get In Touch`

El hero (`#home`) **no** lleva número. El footer tampoco.

### Bloques de `css/style.css` (delimitados por banners `/* --- ... --- */`)

`:root` variables · Reset & Base · Container · Navigation · Hero · Buttons ·
Section Styles · About · Experience · Skills · AI & Generative AI · Projects ·
Education · Contact · Footer · Scroll Animations · Responsive Design

Las media queries existen en **dos lugares**: algunas específicas dentro de cada sección
(`900px`, `600px`) y un bloque global al final (`1024px`, `768px`, `480px`).
Si tocás responsive, revisá **las dos**.

### `js/main.js` — 4 funciones `init*` que corren en `DOMContentLoaded`

`initNavigation()` · `initScrollAnimations()` · `initSmoothScroll()` · `initActiveNavLink()`

Hay un `initTypingEffect()` comentado al final (no usado: el hero muestra el cursor con CSS).

---

## 4. Dependencias externas (CDN, sin package.json)

Cargadas en el `<head>`:

- **Google Fonts** — `Inter` (300–700) y `JetBrains Mono` (400, 500)
- **Font Awesome 6.5.1** — `cdnjs.cloudflare.com` → íconos `fas` / `fab`
- **Devicon 2.15.1** — `cdn.jsdelivr.net` → íconos de tecnologías (`devicon-git-plain`, etc.)

**Convención de íconos:** genéricos/UI usan `fas fa-*`, marcas usan `fab fa-*`,
tecnologías usan `devicon-*-plain`. Siempre dentro de `<i class="...">`.

Al agregar un ícono nuevo, primero verificá que exista en la versión fijada del CDN.

---

## 5. Convenciones de código

### HTML
- Indentación de **4 espacios**. Un `<div class="container">` por sección.
- Cada sección: `<section class="section <tema>" id="<id>">` → `.container` →
  `<h2 class="section-title"><span class="section-number">NN.</span> Título</h2>` → contenido.
- Links externos: siempre `target="_blank" rel="noopener noreferrer"`.
- Comentarios `<!-- ... -->` para marcar sub-bloques (ej. `<!-- AI-Assisted Engineering -->`).
- Las cards repetidas se numeran en comentario: `<!-- 1. Building with the Claude API -->`.
- Un solo `<h1>` (el nombre en el hero). Todo lo demás `h2`/`h3`/`h4`.

### CSS
- **Nunca valores hardcodeados** cuando existe una variable en `:root` (colores, spacing,
  transiciones, sombras, radios, fuentes). Agregá la variable primero.
- Colores del tema: `--bg-primary`, `--bg-secondary`, `--bg-card`, `--accent-primary` (cian
  `#00d4ff`), `--accent-secondary` (violeta `#7c3aed`), `--text-primary/secondary/muted`.
- Nombres de clase **BEM-ish con un solo guion**: `bloque-elemento` y `bloque-elemento__variante`
  (ej. `ai-card`, `ai-card-header`, `ai-card-icon.purple`, `nav-link.nav-link-cta`).
- Orden: las **variantes** (`.purple`) se declaran junto a su bloque, no al final del archivo.
- Cada bloque nuevo arranca con el banner:
  ```css
  /* --------------------------------------------------------------------------
     Nombre del Bloque
     -------------------------------------------------------------------------- */
  ```
- Transiciones siempre con `var(--transition-fast|normal|slow)`.
- Responsive en dos tiers: por bloque (900px/600px) y global (1024px/768px/480px).

### JavaScript
- Vanilla JS, IIFE-free, todo dentro de `DOMContentLoaded`.
- Una función `initX()` por funcionalidad + un comentario JSDoc arriba.
- 4 espacios, `const`/`let` (nunca `var`), `forEach` en vez de `for`.
- Animaciones de entrada: clase `.fade-in` (la pone JS) + `.visible` (la pone el
  IntersectionObserver). **Los `transitionDelay` se setean inline desde JS**, no en CSS.

---

## 6. Acoplamientos críticos ⚠️

Esto es lo que rompe silenciosamente la navegación si se ignora:

1. **Nav ↔ section id.** Cada `<li><a href="#X" class="nav-link">` necesita un elemento con
   `id="X"`. `initActiveNavLink()` matchea por `href === '#' + sectionId`, y
   `initSmoothScroll()` también. Si no existen, el link no hace nada y no se marca activo.

2. **Alta de una sección = 5 pasos** (ver §7). El más olvidado es el paso 5.

3. **Registrar clases nuevas en `js/main.js`.** `initScrollAnimations()` tiene un selector
   hardcodeado:
   ```js
   '.timeline-item, .skill-category, .ai-card, .ai-cert-section, .project-card,
    .education-card, .education-certifications, .about-content, .contact-content'
   ```
   Si creás cards nuevas con clases propias y no las agregás ahí, **no se animan** (quedan
   visibles pero sin fade-in) — no se rompe nada visible, por eso es fácil no notarlo.

4. **Offset de 80px sincronizado en 3 lugares:**
   - `css/style.css` → `html { scroll-padding-top: 80px; }`
   - `js/main.js` → `const headerOffset = 80;`
   - `js/main.js` → `const scrollPosition = window.pageYOffset + 150;`
   Si cambia la altura del nav, actualizá los tres.

5. **`.ai-cert-card` se reusa en Education.** La clase se define en la sección AI pero se usa
   también en `#education` (líneas 1071–1128). Cambiar sus estilos afecta **las dos secciones**.

6. **Anclas cruzadas.** `#education` linkea a `#ai-certifications` (dentro de AI) con
   `&uarr;` y flecha. Si movés o renombrás ese div, actualizá el link.

---

## 7. Checklist: agregar una sección nueva

1. `index.html`: `<section class="section <tema>" id="<id>">` con `.container` y
   `section-title` + `section-number`.
2. **Renumerar** los `<span class="section-number">` de todas las secciones siguientes.
3. Nav: agregar `<li><a href="#<id>" class="nav-link">Texto</a></li>` en el orden correcto
   (el CTA de Contact va último y lleva `nav-link-cta`).
4. `css/style.css`: bloque nuevo con banner + responsive propio; registrar las clases nuevas
   en el `animatedElements` de `js/main.js`.
5. Verificar en browser: ancla, scroll con offset, activo del nav, fade-in, mobile.

---

## 8. Gotchas conocidos / deuda técnica

- `id="AI"` está en **mayúsculas** (todos los demás en minúsculas) y el nav usa `#AI`.
  Es inconsistente, pero **no lo "arregles"** sin actualizar el href del nav también.
- `dev_local/Cosas para mejorar.txt` contiene 2 ítems que **ya están implementados**
  (sección AI y subsection de certificaciones en Education, commits `5a33385`, `61fd65d`,
  `7bdce94`). No los retrabajes: verificá si falta algo y actualizá la nota.
- El footer dice `© 2024` mientras las certificaciones son de 2026. Inconsistencia conocida,
  no corregida a propósito (no tocar sin avisar).
- `<head>` tiene un `<link rel="icon">` commented-out de `/favicon.png` que quedó al lado
  del activo. Ruido, inofensivo.
- El favicon **es un PNG con extensión SVG** (`assets/images/laptop.png` declarado como
  `type="image/svg+xml"`). Funciona, pero es raro.
- Responsive con breakpoints duplicados y solapados entre bloques y bloque global.
- El hero muestra un cursor `|` con blinking (CSS `@keyframes blink`), no hay efecto de
  typewriter real — `initTypingEffect()` está comentado en `main.js` a propósito.
- `main.js` loguea un saludo en consola con `%c`. Es intencional ( Easter egg de developer).

---

## 9. Deploy

- **GitHub Pages sirviendo la rama `main` desde la raíz.** No hay workflow de GitHub Actions,
  no hay rama `gh-pages`, no hay CNAME. **Un `git push` a `main` es un deploy.**
- Por eso: nunca subas un commit roto a `main` sin verificar en browser primero.
- Verificá el resultado en <https://nelsoncabrera06.github.io/my-portfolio/> (puede tardar
  ~1 min en propagar).

### Verificación local (no hay tooling, es manual)

```bash
open index.html              # o: python3 -m http.server 8000
```

Checklist de verificación visual: nav desktop + mobile, cada ancla, scroll offset bajo el nav,
highlight del link activo, animaciones fade-in, lightbox inexistente (no hay), links externos.

---

## 10. Convenciones de git

- Rama única: `main`. Commits directo, sin PRs ni branches de trabajo.
- Mensajes en **inglés, imperativo, corto**, sin prefijo ni cuerpo. Ejemplos reales:
  `more projects`, `new AI section added`, `favicon added`, `All my certifications`,
  `Update hero with whoami command`.
- Un commit = una unidad de cambio coherente (aunque sea "todo el HTML de la sección").

---

## 11. Log de Sesiones

> Agregá una entrada nueva arriba de la más reciente. Formato:
> `### YYYY-MM-DD — Título` + bullets con qué se hizo y qué queda pendiente.

### 2026-09-30 — Creación de AGENTS.md (memoria entre agentes)

**Contexto:** Nelson venía improvisando el portfolio con Antigravity y se quedó sin cuota.
Pidió un archivo de memoria para no perder el contexto y poder seguir con otro agente.

**Se hizo:**
- Reconocimiento completo del repo: estructura, historial de los 12 commits, `main.js`,
  paleta de `:root`, convenciones de markup y los patrones de las secciones AI y Education.
- Documentado este `AGENTS.md`: mapa del proyecto, convenciones, acoplamamientos críticos,
  checklist de alta de sección, gotchas, deploy y git.

**Hallazgos del historial** (reconstrucción de lo que se venía haciendo):

| Commit | Qué hizo |
|---|---|
| `5a33385` | Sección `#AI` completa: cards de AI-Assisted Engineering, ecosystem de frameworks, subsection `#ai-certifications` (+486 líneas de CSS) |
| `61fd65d` | Refinó la sección AI y movió/agregó certificaciones de AI |
| `7bdce94` | Subsection de certificaciones dentro de `#education`, con link a `#ai-certifications` |
| `2464f9a` | Link a Festival Match (proyecto en Vercel) |
| `1a7c7a9` | Favicon (laptop.png) |
| `7e6a3f3`, `63c42f9`, `87c1894` | tres rondas de ampliación de `#projects` |
| `cabf1c6` | Acomodó la sección de educación |
| `63c59de` | "Helsinki" → "Buenos Aires" en el hero y el README |
| `737e81f` | Hero con estética de terminal (`$ whoami`) |
| `166a95a` | primer commit |

**Estado:** sitio completo y funcional, deploy en GitHub Pages desde `main`.
Los 2 ítems de `dev_local/Cosas para mejorar.txt` ya están implementados.
Sin pendientes conocidos.

**Pendiente para la próxima sesión:** ninguno abierto. Si Nelson tiene ideas nuevas, bajalas
a `dev_local/` y agregalas al log de acá cuando se implementen.
