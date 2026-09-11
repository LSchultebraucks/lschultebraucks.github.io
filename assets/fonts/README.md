# Self-hosted Web Fonts

Das Redesign nutzt drei self-hosted variable Schriften (DSGVO-konform, kein
externes CDN). Die Dateien liegen bereits in diesem Verzeichnis:

| Datei                         | Schrift       | Einsatz                  | Achsen      |
|-------------------------------|---------------|--------------------------|-------------|
| `Inter-Variable.woff2`        | Inter         | Fließtext / UI           | `wght`      |
| `SpaceGrotesk-Variable.woff2` | Space Grotesk | Überschriften / Wordmark | `wght`      |
| `Fraunces-Variable.woff2`     | Fraunces      | Display / Name im Hero   | `opsz,wght` |

Alle drei sind **latin-Subsets** und OFL-lizenziert. Die `@font-face`-Definitionen
stehen ganz oben in `assets/css/main.css`; `font-display: swap` sorgt dafür, dass
die Seite auch dann lesbar bleibt, wenn eine Datei fehlt.

## Aktualisieren

Die Dateien stammen aus den Fontsource-Paketen (via jsDelivr):

```bash
curl -L -o Inter-Variable.woff2 \
  "https://cdn.jsdelivr.net/npm/@fontsource-variable/inter/files/inter-latin-wght-normal.woff2"
curl -L -o SpaceGrotesk-Variable.woff2 \
  "https://cdn.jsdelivr.net/npm/@fontsource-variable/space-grotesk/files/space-grotesk-latin-wght-normal.woff2"
curl -L -o Fraunces-Variable.woff2 \
  "https://cdn.jsdelivr.net/npm/@fontsource-variable/fraunces/files/fraunces-latin-full-normal.woff2"
```

Originalquellen:

- **Inter** — https://github.com/rsms/inter (OFL)
- **Space Grotesk** — https://github.com/floriankarsten/space-grotesk (OFL)
- **Fraunces** — https://github.com/undercasetype/Fraunces (OFL)

Wenn eine Datei umbenannt wird, müssen `assets/css/main.css` (`@font-face`) und
die `<link rel="preload">`-Zeilen in `_includes/head.html` mitgezogen werden.
