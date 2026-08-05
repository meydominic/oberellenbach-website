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
- `url: <pfad>` – nur auf Seiten, deren URL vom Dateinamen abweicht (alte Permalinks wurden beibehalten, z. B. `/das-dorf/hessischer-demografiepreis`, `/wirtschaft/hausschlachter`)

## Hinweise zur Migration

- Alle alten URLs (Jekyll-Permalinks) bleiben erhalten, inklusive der historischen
  Tippfehler (`/das-dorf/aerztlicher-bereitsschaftsdienst`).
- Nicht mehr veröffentlichte Seiten (`published: false` → `draft: true`) sind weiterhin
  im Content vorhanden, werden aber nicht gebaut: Dorfladen, Töpferei Geißler, Kirchhof oHG,
  Gastwirtschaft Kambach, Zimmervermietung Kambach, Reiner Kothe, Impressionen, Info Neubürger,
  Deutscher Engagement Preis 2013, Historische Fotos.
- Das RSS-Feed `/feed.xml` ist wie im Original leer (die Seite hat keine Posts).
- `<html lang>` ist jetzt `de` (die alte Seite hatte standardmäßig `en`).
- SCSS wird mit Hugo Pipes (`resources.Get … | toCSS`) kompiliert – das nutzt das in
  Hugo Extended eingebaute libsass. Falls Hugo libsass künftig entfernt, `dart-sass`
  (Embedded) installieren, dann funktioniert `toCSS` weiterhin.

## Deployment (GitHub Pages)

Das Deployment-Layout (Branch `gh-pages` vs. Actions-Workflow) ist bewusst offen gelassen –
je nach Wunsch kann ein Workflow mit `peaceiris/actions-hugo` + `actions-gh-pages`
ergänzt werden. Die `static/CNAME` wird beim Build nach `public/` kopiert.
