# DESIGN.md — HiPanda Talleres

## Sistema visual

El sistema visual es el estándar HiPanda adaptado al formato de presentación. Colores OKLCH, tipografía Libre Franklin, asimetría deliberada. Lo que cambia es la estructura de navegación y el comportamiento de slide.

---

## Colores

Sistema basado en OKLCH. Los nombres son semánticos, no descriptivos.

```css
:root {
  /* Fondos */
  --paper: oklch(0.985 0.005 85); /* Fondo principal. Blanco cálido, no puro. */
  --paper-soft: oklch(
    0.969 0.007 82
  ); /* Fondo de secciones secundarias, cards. */
  --paper-strong: oklch(
    0.944 0.008 82
  ); /* Fondo de elementos con más peso visual. */

  /* Texto */
  --ink: oklch(
    0.252 0.012 252
  ); /* Texto principal. Casi negro con tinte azul frío. */
  --muted: oklch(0.53 0.016 250); /* Texto secundario, labels, descripciones. */

  /* Estructura */
  --line: oklch(0.865 0.008 80); /* Bordes, divisores. */
  --slate-soft: oklch(0.95 0.012 245); /* Fondo alternativo con tinte frío. */
  --slate-line: oklch(0.88 0.014 245); /* Borde para elementos sobre slate. */

  /* Acento */
  --accent: oklch(
    0.59 0.074 46
  ); /* Amber cálido. Kickers, números destacados, énfasis. */
  --accent-soft: oklch(0.946 0.022 46); /* Fondo de callouts y conclusiones. */
}
```

### Estrategia de color: Restrained

Un único acento (amber) que aparece en menos del 10% de la superficie. El resto es papel, tinta y estructura.

El acento nunca se usa en fondos grandes ni en headings principales. Solo en:

- Kickers (etiquetas de sección)
- Números de slide o métricas con énfasis especial
- La línea superior del `.contract-shell`
- Textos de conclusión o takeaway

### Fondo de página

No es un color plano. Es una composición de gradientes:

```css
background:
  radial-gradient(circle at top left, oklch(0.96 0.012 50) 0, transparent 28%),
  radial-gradient(
    circle at top right,
    oklch(0.958 0.012 235) 0,
    transparent 30%
  ),
  linear-gradient(180deg, var(--paper) 0%, oklch(0.978 0.006 84) 100%);
```

Dos halos sutiles en las esquinas superiores (uno warm, uno cool) sobre un gradiente vertical casi imperceptible. Le da profundidad al fondo sin que se note.

---

## Tipografía

**Familia única:** Libre Franklin (Google Fonts)

```html
<link
  href="https://fonts.googleapis.com/css2?family=Libre+Franklin:wght@400;500;600;700;800&display=swap"
  rel="stylesheet"
/>
```

No se usa ninguna otra familia. Sin serifs, sin monospace decorativo.

### Escala y roles

| Rol           | Tamaño base                              | Peso            | Tracking | Uso                                |
| ------------- | ---------------------------------------- | --------------- | -------- | ---------------------------------- |
| Kicker        | 0.73rem                                  | 700             | 0.18em   | Etiquetas de sección en mayúsculas |
| H1 (hero)     | 4.6rem desktop / 5xl tablet / 4xl mobile | 800 (extrabold) | -0.04em  | Título principal del taller        |
| H2 (sección)  | 3xl–4xl                                  | 700 (bold)      | -0.04em  | Títulos de cada módulo/tema        |
| H3 (card)     | 2xl                                      | 700 (bold)      | -0.03em  | Subtítulos dentro de slides        |
| Body largo    | lg–xl                                    | 400             | normal   | Párrafos descriptivos principales  |
| Body corto    | base                                     | 400             | normal   | Texto de apoyo, descripciones      |
| Labels        | sm                                       | 600–700         | 0.12em   | Etiquetas de datos en mayúsculas   |
| Datos/valores | base–lg                                  | 600–700         | normal   | Estadísticas, números de módulo    |

### `.kicker`

```css
.kicker {
  letter-spacing: 0.18em;
  text-transform: uppercase;
  font-size: 0.73rem;
  font-weight: 700;
}
```

Siempre en `.accent` o `.muted`. Nunca en `.ink`. Nunca con font-size mayor a 0.8rem.

### `.section-title`

```css
.section-title {
  line-height: 1.02;
  letter-spacing: -0.04em;
}
```

Se aplica a todos los H1 y H2 principales. El tracking negativo aprieta las letras y da el efecto editorial característico.

### Medida máxima de cuerpo

```css
.measure {
  max-width: 72ch;
}
```

Los párrafos largos nunca se estiran al ancho completo del contenedor.

---

## Layout

### Slide como unidad

Cada slide es un `<section>` independiente que ocupa el viewport:

```html
<section class="slide" id="slide-01">
  <!-- contenido -->
</section>
```

```css
.slide {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 2rem;
}
```

Los slides se apilan verticalmente y se navega con scroll suave o botones de siguiente/anterior.

### Contenedor principal

```html
<main
  class="mx-auto max-w-7xl px-5 pb-16 pt-6 sm:px-8 lg:px-10 lg:pt-10"
></main>
```

`max-w-7xl` (1280px). Padding horizontal responsivo. No se usa `container` de Tailwind.

### Grid asimétrico

El layout no es una columna ni un grid simétrico. Los grids varían por sección:

- Hero del taller: `lg:grid-cols-[minmax(0,1.35fr)_minmax(19rem,0.7fr)]`
- Contenido + ejemplo: `lg:grid-cols-[minmax(0,1.05fr)_minmax(0,0.95fr)]`
- Módulos: `lg:grid-cols-3` o asimétrico según contenido
- Casos prácticos: `lg:grid-cols-[minmax(0,1.1fr)_minmax(18rem,0.78fr)]`

La asimetría es deliberada. Crea tensión visual y jerarquía.

### Separación entre slides

Cada slide es independiente. Dentro de un slide: `gap-8` o `gap-10`.

### Bordes y radios

- Slide principal: `rounded-[2rem]`
- Cards internas: `rounded-[1.75rem]`
- Callouts internos: `rounded-[1.5rem]` o `rounded-[1.25rem]`
- Elementos pequeños: `rounded-[1.5rem]`

El radio decrece con el nivel de anidamiento.

---

## Componentes

### `.contract-shell`

Card de contenido con línea accent en la parte superior:

```css
.contract-shell::before {
  content: "";
  display: block;
  height: 2px;
  background: linear-gradient(
    90deg,
    color-mix(in oklab, var(--accent) 95%, transparent),
    color-mix(in oklab, var(--accent) 30%, transparent)
  );
  margin-bottom: 1.25rem;
  border-radius: 999px;
}
```

La línea va de accent sólido a accent transparente, de izquierda a derecha.

### `.stat-line`

Grid de dos columnas para métricas o datos clave:

```css
.stat-line {
  display: grid;
  grid-template-columns: minmax(0, 1fr) auto;
  gap: 1rem;
  align-items: end;
  padding: 0.95rem 0;
  border-top: 1px solid var(--line);
}
.stat-line:first-child {
  border-top: 0;
  padding-top: 0;
}
```

El valor va a la derecha en `.ink` o `.accent`. La descripción va a la izquierda en `.muted`.

### `.flow-index`

Número de paso o módulo:

```css
.flow-index {
  width: 2.2rem;
  height: 2.2rem;
  border-radius: 999px;
  border: 1px solid var(--line);
  background: color-mix(in oklab, var(--paper) 84%, var(--accent-soft));
  color: var(--ink);
  font-size: 0.85rem;
  font-weight: 700;
  flex-shrink: 0;
}
```

Círculo con mezcla de paper y accent-soft.

### Callout / Takeaway

```html
<div
  class="rounded-[1.5rem] border border-line bg-[color:var(--accent-soft)] p-5 sm:p-6"
>
  <p class="text-sm font-semibold uppercase tracking-[0.12em] accent">
    Key takeaway
  </p>
  <p class="mt-3 text-base leading-7 ink">...</p>
</div>
```

Fondo `--accent-soft`, borde `--line`. El label en `.accent`. El cuerpo en `.ink`.

### Callout neutro

```html
<div class="rounded-[1.25rem] border border-line bg-[color:var(--paper)] p-4">
  <p class="text-sm font-semibold uppercase tracking-[0.12em] muted">Ejemplo</p>
  <p class="mt-2 text-sm leading-6 muted">...</p>
</div>
```

Fondo `--paper` (más claro que el padre que es `--paper-soft`). Todo en `.muted`.

---

## Navegación

### Botones de slide

```html
<nav class="slide-nav">
  <button class="nav-btn" id="prev">← Anterior</button>
  <span class="slide-counter">3 / 12</span>
  <button class="nav-btn" id="next">Siguiente →</button>
</nav>
```

```css
.nav-btn {
  padding: 0.75rem 1.5rem;
  border-radius: 999px;
  border: 1px solid var(--line);
  background: var(--paper);
  color: var(--ink);
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
}
.nav-btn:hover {
  background: var(--paper-strong);
  border-color: var(--accent);
}
```

La navegación puede estar fija abajo o al final de cada slide.

### Indicadores de progreso

Barra fina en la parte superior:

```css
.progress-bar {
  height: 2px;
  background: var(--line);
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 50;
}
.progress-fill {
  height: 100%;
  background: var(--accent);
  transition: width 0.3s ease;
}
```

---

## Sombras y elevación

Un solo nivel de sombra para las cards principales:

```css
box-shadow: 0 24px 80px rgba(45, 39, 32, 0.08);
```

En Tailwind: `shadow-paper` (configurado en `tailwind.config`). Solo en las cards de primer nivel sobre el fondo de página.

---

## Stack técnico

- **HTML** semántico (`<main>`, `<section>`, `<article>`, `<aside>`, `<nav>`)
- **Tailwind CSS** via CDN (`https://cdn.tailwindcss.com`)
- **Libre Franklin** via Google Fonts
- **JavaScript** mínimo para navegación entre slides (scroll, teclado, botones)
- Sin dependencias externas adicionales
- Todo el CSS custom va en `<style>` en el `<head>`
- Cada slide puede ser un archivo HTML independiente o una sección en un solo archivo

---

## Lo que no va

- Gradientes de texto (`background-clip: text`)
- `border-left` como acento de color en cards
- Cards idénticas en grid simétrico sin variación
- Glassmorphism
- Animaciones excesivas (transiciones suaves sí, pero nada distractivo)
- Iconografía de terceros (sin Font Awesome, sin Heroicons, sin emojis decorativos)
- `#000` o `#fff` en ningún lado
- Sombras apiladas
- Impresión como objetivo principal (la presentación es digital)
