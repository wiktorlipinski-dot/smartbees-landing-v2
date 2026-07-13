# Handoff: SmartBees — Landing Page „Audyt UX i UI" (V2)

## Overview
Jednostronicowy landing page agencji UX **SmartBees**, sprzedający usługę **audytu UX i UI**.
Ciemny motyw, jeden pomarańczowy akcent na widok, budowa z 12 pełnoszerokościowych
sekcji ułożonych pionowo (hero → trust → korzyści → statystyki → feature → cennik →
proces → opinie → CTA → FAQ → CTA końcowe → stopka). Cel konwersyjny: skierować
użytkownika do formularza kontaktowego `#kontakt` (w sekcji Hero).

## About the Design Files
Pliki w tej paczce to **referencje projektowe stworzone w HTML** — prototypy pokazujące
docelowy wygląd i zachowanie, **nie** kod produkcyjny do wklejenia 1:1. Zadaniem jest
**odtworzenie tych designów w środowisku docelowego repozytorium** (React, Vue, Next.js,
itp.) zgodnie z jego wzorcami i biblioteką komponentów. Jeśli środowisko jeszcze nie
istnieje — wybrać najodpowiedniejszy framework (rekomendacja: React, bo design system
jest biblioteką React `smartbees-lp-blocks`) i tam zaimplementować.

Design zbudowany jest na gotowej bibliotece **`smartbees-lp-blocks`** (12 sekcji React,
eksponowane jako `window.SmartbeesLpBlocks.*`). W prototypie sekcje osadzane są przez
custom-element `<x-import component-from-global-scope="SmartbeesLpBlocks.Hero" …>`.
W docelowym repo należy zainstalować/zaimportować pakiet `smartbees-lp-blocks` i renderować
komponenty natywnie (`<Hero variant="a" tone="base" />`), a customizacje trzymać jako
override'y CSS (patrz sekcja „Custom overrides").

## Fidelity
**High-fidelity (hifi).** Finalne kolory, typografia, spacing i interakcje. Odtwarzać
pixel-perfect, korzystając z komponentów `smartbees-lp-blocks` + poniższych override'ów.

## Screens / Views

### Pojedynczy widok — Landing (scroll pionowy)
Kontener: `<div id="smartbees-lp-v2" style="background: var(--surface)">`, tło `#1c1c24`.
Sekcje bez paddingu między sobą; każda sekcja sama zarządza tłem przez prop `tone`.
Rytm tonów sąsiednich sekcji jest celowo różny.

Kolejność sekcji i propsy (z prototypu):
1. `Hero` — `variant="a"` `tone="base"` — nagłówek, pigułki trust, formularz `#kontakt`, logo w topbarze.
2. `TrustBar` — ściana logotypów klientów.
3. `Benefits` — `variant="a"` — siatka korzyści; ostatni kafel przerobiony na CTA (klasa `.ben-cta`).
4. `Stats` — `tone="ember"` — rząd dużych liczb.
5. `Feature` — feature ze zdjęciem + bullety.
6. `Pricing` — `tone="panel"` — 3 plany cenowe.
7. `Process` — 3 kroki.
8. `Testimonials` — `variant`/karuzela; w prototypie nadpisana na ciągły przewijany pas kart.
9. `CtaBanner` — `variant="b"` `tone="base"` (owinięte w `.theme-orange`).
10. `Faq` — 4 pytania (native `<details>`).
11. `FinalCta` — `variant="a"` `tone="ember"`.
12. `FooterLp` — `tone="dim"`.

## Custom overrides (nadpisania względem czystej biblioteki)
Te reguły w prototypie zmieniają domyślne zachowanie biblioteki — odtworzyć w docelowym CSS:

- **Logo zamiast wordmarku tekstowego:** `.logo` → `background-image: url(logo.svg)`, `132×30px`; ukryte `.logo i` i `.kicker`.
- **Przyciski bardziej kwadratowe:** `#smartbees-lp-v2 .btn { border-radius: 10px; }` — **uwaga:** to celowe odejście od brandowego `--radius-pill`; klient poprosił o bardziej kwadratowe CTA. Utrzymać spójnie na wszystkich CTA.
- **Motyw heksagonu:** bullety (`.hero-bullets li::before`, `.feat-bullets li::before`, `.price-list li::before`) rysowane jako heksagon (symbol SmartBees).
- **Topbar bez numeru telefonu:** `.topbar .phone-link { display:none }`.
- **Przyklejony pasek / sticky bar po scrollu (DO ZAIMPLEMENTOWANIA):** wg ostatniej prośby klienta — po przewinięciu (>620px) ma pojawiać się pełnoszerokościowy przyklejony pasek u góry z: logo SmartBees + separator + tekst „Audyt UX i UI" po lewej, oraz przyciskiem CTA „Zamów wycenę" po prawej. CTA ma być **wizualnie identyczne przed i po scrollu** (ten sam kwadratowawy `border-radius: 10px`, kolor `--accent-btn`). Tło paska: półprzezroczyste `rgba(28,28,36,.85)` + `backdrop-filter: blur(14px)`, dolna krawędź `1px var(--outline-dark)`, wjazd `transform: translateY(-100%)` → `0`. (W obecnym prototypie jest tylko pływająca pigułka `#sb-float-cta` — należy ją zastąpić pełnym paskiem.)
- **Testimonials:** nadpisane z karuzeli na ciągły, poziomo przewijany pas kart z podglądem następnej (`scroll-snap-type: x mandatory`, ukryty scrollbar).
- **Marquee statystyk / trust:** auto-scroll z przyspieszeniem przy scrollu strony.
- **Hero trust row:** pokazane tylko Clutch (biały) + złota odznaka; reszta logotypów ukryta.

## Interactions & Behavior
- **Smooth-scroll do `#kontakt`:** wszystkie CTA przewijają do formularza z offsetem (~40–80px od góry).
- **Sticky bar:** klasa `.is-visible` dodawana gdy `window.pageYOffset > 620`, usuwana poniżej; przejście `transform .34s cubic-bezier(.22,.61,.36,1)`.
- **Hover CTA:** tło `--accent-btn` (#d74114) → `#eb5627`, `translateY(-1px)`, strzałka SVG `translateX(3px)`.
- **Kopiowanie e-maila:** klik w e-mail kopiuje `kontakt@smartbees.pl` do schowka (krótki feedback kolorem).
- **Testimonials:** strzałki przewijają pas o szerokość karty + gap.

## Design Tokens
Źródło prawdy: `styles.css` → `_ds_bundle.css` (`:root`). Kluczowe:
- `--surface: #1c1c24` (tło strony), `--surface-dim` (ciemniejsze), `--surface-bright`
- `--accent: #eb5627` (akcent — liczby, hover), `--accent-btn: #d74114` (wypełnienie przycisków)
- `--text`, `--text-secondary` (w prototypie nadpisane na `#ffffff`), `--outline-dark`
- `--radius-pill` (brandowo przyciski = pill), `--radius-card: 0 8px 0 30px` (asymetryczne rogi kart), `--card-gradient`
- `--font-display` (Funnel Display, nagłówki, **waga 500 — nigdy 700+**), `--font-body` (Inter)
- spacing: `--pad-mobile`, `--pad-desktop`
- **Override lokalny:** przyciski `border-radius: 10px` (nie pill).

## Zasady marki (nie łamać)
- Jeden pomarańczowy akcent na widok — CTA i liczby; reszta spokojna.
- Nagłówki wagą 500 (medium) — hierarchia z rozmiaru, nie z grubości.
- Jedno `Hero` na stronę (zawiera formularz `#kontakt`); powtarzać CTA: hero → CtaBanner (środek) → FinalCta (koniec).
- Design wyłącznie dark-mode — nie budować jasnych wariantów.

## Screenshots
Folder `screenshots/` zawiera 9 zrzutów całej strony (kolejne pozycje przewinięcia,
`01` → `09`) pokazujących docelowy wygląd wszystkich sekcji. **Uwaga:** samodzielnie
otwarty `SmartbeesLandingV2.dc.html` renderuje tylko część sekcji, bo `<x-import>`
wymaga runtime środowiska projektowego — traktuj zrzuty jako źródło prawdy wizualnej.

## Assets
- `logo.svg` — logo SmartBees (w topbarze i stopce; w sticky barze).
- Odznaki Hero (Clutch, złota odznaka) — wstrzykiwane jako data-URI w skrypcie (base64 w `SmartbeesLandingV2.dc.html`).
- Biblioteka sekcji: `_ds/smartbees-lp-…/` (`_ds_bundle.js`, `_ds_bundle.css`, `styles.css`, `fonts/`).

## Files
- `SmartbeesLandingV2.dc.html` — główny prototyp (struktura, wszystkie override'y CSS, skrypty interakcji).
- `logo.svg` — logo.
- `ds-base.js`, `support.js` — skrypty pomocnicze runtime prototypu (x-import itd.).
- `_ds/smartbees-lp-bca2a5fc-a45e-44ad-ad7c-02b1cce8540c/` — biblioteka `smartbees-lp-blocks` (komponenty, tokeny, style, fonty). W docelowym repo zastąpić instalacją pakietu NPM `smartbees-lp-blocks`.
