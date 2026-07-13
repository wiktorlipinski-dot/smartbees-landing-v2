# SmartBees LP — konwencje budowania

**Czym jest ten system:** 12 gotowych, pełnoszerokościowych sekcji landing page'a
(dark-mode, marka SmartBees — agencja UX). Buduje się z nich całe strony przez
składanie sekcji w pionie — nie są to drobne kontrolki, tylko bloki strony.

## Setup strony

Żaden provider nie jest wymagany. Ustaw tło strony na `#1c1c24` (`var(--surface)`)
i składaj sekcje bezpośrednio, bez paddingu wokół — każda sekcja sama zarządza
swoim tłem (prop `tone`) i wewnętrznym kontenerem 1310px. Design jest wyłącznie
ciemny — nie buduj jasnych wariantów stron.

```jsx
<div style={{ background: 'var(--surface)' }}>
  <Hero variant="a" />
  <TrustBar />
  <Benefits />
  <Stats tone="ember" />
  <Feature />
  <Pricing tone="panel" />
  <Process />
  <Testimonials />
  <CtaBanner variant="b" />
  <Faq />
  <FinalCta />
  <FooterLp />
</div>
```

## Idiom stylowania

Sekcje konfiguruje się **propsami, nie klasami CSS**:
- `variant="a" | "b"` — kąt perswazji: `a` = benefit-led (korzyść), `b` = pain-led (problem/strata). Mają go: `Hero`, `Benefits`, `Testimonials`, `CtaBanner`, `FinalCta`.
- `tone="base" | "dim" | "panel" | "ember"` — kuratorowany wariant tła (granat / ciemniejszy / szklany gradient / poświata akcentu). **Sąsiednie sekcje powinny różnić się tonem** — to celowy rytm strony.
- Wszystkie treści (tytuły, bullety, plany cenowe, opinie) mają polskie wartości domyślne z prawdziwego landingu SmartBees; nadpisuj tylko to, czego wymaga kampania.

Do własnego kodu-kleju (nagłówki stron, drobne wstawki) używaj tokenów i klas
z `styles.css`:
- kolory: `var(--surface)`, `var(--surface-dim)`, `var(--accent)` (#eb5627 — tylko CTA/akcenty), `var(--accent-btn)` (#d74114 — wypełnienie przycisków), `var(--text)`, `var(--text-secondary)`, `var(--outline-dark)`
- kształty: `var(--radius-pill)` (przyciski = zawsze pill), `var(--radius-card)` (karty = asymetryczne `0 8px 0 30px`), `var(--card-gradient)` (szklany gradient kart)
- typografia: `var(--font-display)` (Funnel Display, nagłówki, **waga 500 — nigdy 700+**), `var(--font-body)` (Inter)
- klasy: `.wrap` (kontener 1310px), `.btn.btn-primary` / `.btn.btn-ghost` (+`.btn-lg`, `.btn-block`), `.card`, `.kicker` (nadtytuł z pomarańczową kreską), `.sec-head` (+`.center`), `.sec-sub`

## Zasady marki (nie łam ich)

- Jeden pomarańczowy akcent na widok — CTA i liczby; reszta spokojna.
- Przyciski wyłącznie pill-shaped; kwadratowe rogi na przyciskach = off-brand.
- Nagłówki zawsze wagą 500 (medium) — hierarchia idzie z rozmiaru, nie z grubości.
- CRO: jedno `Hero` na stronę (ma formularz `#kontakt`); wszystkie CTA sekcji
  linkują do `#kontakt`. Powtarzaj CTA: hero → `CtaBanner` w środku → `FinalCta` na końcu.

## Gdzie leży prawda

Przed stylowaniem przeczytaj `styles.css` (importuje `_ds_bundle.css` z pełnym
CSS sekcji i wszystkimi tokenami w `:root`). API każdej sekcji: `components/general/<Name>/<Name>.d.ts`
i przykłady w `<Name>.prompt.md`.

# SmartbeesLpBlocks (smartbees-lp-blocks@0.1.0)

This design system is the published smartbees-lp-blocks React library, bundled as a single
browser global. All 12 components are the real upstream code.

## Where things are

- `_ds_bundle.js` — the whole-DS bundle at the project root; loads every component to `window.SmartbeesLpBlocks`. First line is a `/* @ds-bundle: … */` metadata header.
- `styles.css` — the single stylesheet entry: it `@import`s the tokens, fonts, and component styles (`_ds_bundle.css`). Link this one file.
- `components/<group>/<Name>/<Name>.prompt.md` (example JSX + variants), `<Name>.d.ts` (types), `<Name>.html` (variant grid).
- `tokens/*.css` — CSS custom properties, names verbatim from upstream.
- `fonts/` — `@font-face` files + `fonts.css` (when the package ships fonts).

For a specific component, `read_file("components/<group>/<Name>/<Name>.prompt.md")`.

## Loading

Add these two lines to your page once (React must be on the page first):

```html
<link rel="stylesheet" href="styles.css">
<script src="_ds_bundle.js"></script>
```

Components are then available at `window.SmartbeesLpBlocks.*`. Mount into a dedicated child node (e.g. `<div id="ds-root">`), not the host page's own React root, so the two trees don't collide:

```jsx
const { Benefits } = window.SmartbeesLpBlocks;
ReactDOM.createRoot(document.getElementById('ds-root')).render(<Benefits />);
```

## Tokens

25 CSS custom properties from smartbees-lp-blocks. Names are
preserved verbatim from upstream. They are declared inside `_ds_bundle.css` (this DS ships one compiled stylesheet rather than separate token files).

- **color** (8): `--surface`, `--surface-dim`, `--surface-bright`, …
- **spacing** (2): `--pad-mobile`, `--pad-desktop`
- **typography** (2): `--font-display`, `--font-body`
- **radius** (3): `--radius`, `--radius-card`, `--radius-pill`
- **other** (10): `--ink-black`, `--accent`, `--accent-btn`, …

## Components

### general
- `Benefits` — Siatka korzyci 23: karty z du cyfr porzdkow w kolorze akcentu,
- `CtaBanner` — Banner CTA rdstronny: krtki wyrniony pasek z nagwkiem, jednym
- `Faq` — FAQ w wariancie skrconym: 4 pytania jako karty-akordeony (native details)
- `Feature` — Feature/rozwizanie ze zdjciem: nagwek + bullety wartoci z jednej strony,
- `FinalCta` — CTA kocowe: nagwek + krtka lista risk-reducerw z jednej strony,
- `FooterLp` — Stopka minimalna LP: logo, tagline, dane firmy i NIP, chipy trust
- `Hero` — Hero landing page'a: pasek grny z logo i telefonem, kicker, piguki trust,
- `Pricing` — Cennik: 3 karty planw z list bulletw zakresu i CTA pod kad.
- `Process` — Proces w 3 krokach: due numerowane karty (numer jako wyeksponowany element
- `Stats` — Rzd 3-4 duych liczb z podpisami  samodzielny wizualny trust signal
- `Testimonials` — Karuzela opinii klientw: 2 cytaty na slajd, strzaki nawigacyjne
- `TrustBar` — Pasek zaufania: ciana realnych logotypw klientw w siatce
