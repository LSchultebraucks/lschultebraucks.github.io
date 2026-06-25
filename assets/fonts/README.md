# Self-hosted Web Fonts

Das Redesign nutzt drei self-hosted Schriften (DSGVO-konform, kein externes CDN).
Lege die folgenden **variablen woff2-Dateien** in dieses Verzeichnis (`assets/fonts/`).
Solange die Dateien fehlen, fällt die Seite sauber auf System-Schriften zurück
(`font-display: swap`), bleibt also funktionsfähig.

| Schrift        | Einsatz                | Akzeptierte Dateinamen (einer genügt)                 |
|----------------|------------------------|-------------------------------------------------------|
| Inter          | Fließtext / UI         | `Inter[wght].woff2` **oder** `Inter-Variable.woff2`   |
| Space Grotesk  | Überschriften / Wordmark | `SpaceGrotesk[wght].woff2` **oder** `SpaceGrotesk-Variable.woff2` |
| Fraunces       | Display / Name im Hero | `Fraunces[opsz,wght].woff2` **oder** `Fraunces-Variable.woff2` |

Die `@font-face`-Definitionen stehen in `assets/css/main.css` (ganz oben).

## Bezugsquellen (alle OFL-lizenziert, kostenlos)

- **Inter** — https://github.com/rsms/inter/releases (Datei `InterVariable.woff2` → in `Inter-Variable.woff2` umbenennen)
- **Space Grotesk** — https://github.com/floriankarsten/space-grotesk (Ordner `fonts/variable/`)
- **Fraunces** — https://github.com/undercasetype/Fraunces (Ordner `fonts/variable/`)

Alternativ via [Fontsource](https://fontsource.org/) die `*-variable`-Pakete von
`@fontsource-variable/inter`, `@fontsource-variable/space-grotesk` und
`@fontsource-variable/fraunces` herunterladen und die woff2 hierher kopieren.

Beim Download über Google Fonts heißen die Dateien typischerweise `Inter[wght].woff2`
usw. — diese Namen werden ebenfalls direkt erkannt.
