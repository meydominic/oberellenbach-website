# oberellenbach-website

Website für Oberellenbach (Alheim, Hersfeld-Rotenburg) — **migriert von Jekyll auf Hugo**.

Die alte Seite lag im `gh-pages`-Branch und wurde mit Jekyll + dem minima-Theme gebaut.
Diese Migration übernimmt Design und Inhalte 1:1, aufgebaut mit [Hugo](https://gohugo.io/) (getestet mit v0.164.0+extended).

## Bauen & Lokal Entwickeln

```sh
hugo server -D   # Entwicklungsserver inkl. Entwürfen (draft: true)
hugo             # statische Seite nach public/
```

## Struktur

```
content/          Seiten (eine Sektion pro Navigations-Kategorie)
  _index.md             Startseite (/)
  termine/              Termine (/termine, Google-Calendar-Iframe)
  das-dorf/             Das Dorf (/das-dorf/…)
  geschichte/           Geschichte (/geschichte/…)
  wirtschaft/           Wirtschaft (/wirtschaft/…)
  impressum/            Impressum (/impressum)
  contact/, societies/, lost-items/, tourism/   → alte, unverlinkte Seiten
assets/scss/      SCSS-Quellen (1:1 aus dem minima-Theme übernommen, via Hugo Pipes)
data/categories.yml   Navigationsstruktur (wie in Jekyll _data/categories.yml)
layouts/          Basis-Templates (baseof, Navigation, Head, 404)
static/           Bilder, Uploads, CNAME (wird unverändert nach public/ kopiert)
```

## Front Matter-Konventionen

- `category: <kategorie>` – steuert die Zuordnung in der Navigations-Sidebar
- `category-index: true` – kennzeichnet die Kategorieseite selbst (z. B. `das-dorf/_index.md`)
- `draft: true` – entspricht `published: false` der alten Seite (Seite wird nicht gebaut)
- `weight: <n>` – Reihenfolge der Unterpunkte (entspricht der alten alphabetischen Sortierung)
- `url: <pfad>` – nur auf Seiten, deren URL vom Dateinamen abweicht (bewusst
  kürzere Slugs aus der alten Seite, z. B. `/das-dorf/hessischer-demografiepreis`,
  `/wirtschaft/hausschlachter`)

## Hinweise zur Migration

- Alle alten URLs (Jekyll-Permalinks) bleiben erhalten. Die Tippfehler in URLs wurden
  korrigiert; für die alte, fehlerhafte URL existiert jeweils eine Redirect-Seite
  (Hugo `aliases`):
  - `/das-dorf/aerztlicher-bereitsschaftsdienst` (doppeltes s) →
    `/das-dorf/aerztlicher-bereitschaftsdienst` (mit Redirect)
  - `imoressionen.md` → `impressionen.md`, URL `…/impressions` → `…/impressionen`
    (Draft, war nie online)
  - URL `…/german-commitment-award` → `…/deutscher-engagementpreis-2013`
    (Draft, war nie online)
  - Bewusst beibehaltene Kurz-Slugs (keine Tippfehler):
    `/das-dorf/hessischer-demografiepreis`, `/wirtschaft/hausschlachter`,
    `/das-dorf/landeswettbewerb-unser-dorf-hat-zukunft`
- Nicht mehr veröffentlichte Seiten (`published: false` → `draft: true`) sind weiterhin
  im Content vorhanden, werden aber nicht gebaut: Dorfladen, Töpferei Geißler, Kirchhof oHG,
  Gastwirtschaft Kambach, Zimmervermietung Kambach, Reiner Kothe, Impressionen, Info Neubürger,
  Deutscher Engagement Preis 2013, Historische Fotos.
- Das RSS-Feed `/feed.xml` ist wie im Original leer (die Seite hat keine Posts).
- `<html lang>` ist jetzt `de` (die alte Seite hatte standardmäßig `en`).
- SCSS wird mit Hugo Pipes (`resources.Get … | toCSS`) kompiliert – das nutzt das in
  Hugo Extended eingebaute libsass. Falls Hugo libsass künftig entfernt, `dart-sass`
  (Embedded) installieren, dann funktioniert `toCSS` weiterhin.

## Fonts

- Selbst gehostete **Variable Fonts** (woff2, in `assets/fonts/`):
  - **Lora** (SIL OFL) für Überschriften (`h1–h6`)
  - **Source Sans 3** (SIL OFL) für Fließtext (`$base-font-family`)
- Beide als latin-Subset (deckt Deutsch inkl. Umlauten/ß ab), normal + italic,
  Gewichtsachse 400–700, `font-display: swap`
- Kein externer Font-CDN → DSGVO-konform; OFL-Lizenzen liegen in `assets/fonts/`
- URLs werden beim Build via `resources.ExecuteAsTemplate` injiziert und per
  `resources.Fingerprint` gehasht; die beiden normalen Fonts werden im `<head>`
  mit `preload` geladen
- Zum Austauschen: woff2-Dateien in `assets/fonts/` ersetzen und die
  `@font-face`-Blöcke in `assets/scss/main.scss` anpassen

## Deployment (GitHub Pages)

Das Deployment-Layout (Branch `gh-pages` vs. Actions-Workflow) ist bewusst offen gelassen –
je nach Wunsch kann ein Workflow mit `peaceiris/actions-hugo` + `actions-gh-pages`
ergänzt werden. Die `static/CNAME` wird beim Build nach `public/` kopiert.
