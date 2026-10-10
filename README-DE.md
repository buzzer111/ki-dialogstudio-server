# KI-Dialogstudio Server

English original: `README.md`.

Firmen-Version von KI-Dialogstudio: KI-Kurse und KI-Assistenten für normale HTML-Seiten, ohne WordPress. Der Kunde betreibt die Software auf einem eigenen Server, mit eigenem KI-Schlüssel und eigenen Daten.

**Stand:** Spezifikation. Es gibt noch keinen lauffähigen Code.

## Produktfamilie

| Produkt | Plattform | Vertrieb |
|---|---|---|
| KI-Dialogstudio Lite | WordPress | kostenlos, WordPress.org |
| KI-Dialogstudio | WordPress | Jahresabo und Lifetime |
| **KI-Dialogstudio Server** | Node.js, selbst gehostet | Jahresabo |

Die beiden WordPress-Plugins liegen im Repository `buzzer111/ki-dialogstudio`.

## Technik

TypeScript, Fastify, PostgreSQL, Auslieferung als Docker-Image. Einzelheiten: `docs/SPECIFICATION-DE.md`, Abschnitt 8.

## Ordner

| Pfad | Inhalt | Stand |
|---|---|---|
| `docs/` | Spezifikation und Entwicklungsdokumente | `SPECIFICATION.md`, `SPECIFICATION-DE.md` |
| `server/` | Backend (Node.js): API, Zugang, KI-Anbindung, Daten | leer |
| `web/widget/` | Chat-Baustein zum Einbinden in HTML-Seiten | leer |
| `web/admin/` | Verwaltungsoberfläche | leer |
| `deploy/` | Dateien für Installation und Betrieb beim Kunden | leer |

## Dokumente

Englisch ist das Original, Deutsch die Übersetzung. Änderungen zuerst im englischen Original, dann in der Übersetzung.

| Englisches Original | Deutsche Übersetzung |
|---|---|
| `README.md` | `README-DE.md` |
| `docs/SPECIFICATION.md` | `docs/SPECIFICATION-DE.md` |

## Lizenz

Proprietär, siehe [`LICENSE`](LICENSE). Kein Open-Source-Projekt.
