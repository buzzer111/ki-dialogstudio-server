# KI-Dialogstudio Server

Firmen-Version von KI-Dialogstudio: KI-Kurse und KI-Assistenten für normale HTML-Seiten, ohne WordPress. Der Kunde betreibt die Software auf einem eigenen Server, mit eigenem KI-Schlüssel und eigenen Daten.

**Stand:** Konzept. Es gibt noch keinen lauffähigen Code.

## Produktfamilie

| Produkt | Plattform | Vertrieb |
|---|---|---|
| KI-Dialogstudio Lite | WordPress | kostenlos, WordPress.org |
| KI-Dialogstudio | WordPress | Jahresabo und Lifetime |
| **KI-Dialogstudio Server** | Node.js, selbst gehostet | Jahresabo |

Die beiden WordPress-Plugins liegen im Repository `buzzer111/ki-dialogstudio`.

## Ordner

| Pfad | Inhalt | Stand |
|---|---|---|
| `docs/` | Spezifikation und Entwicklungsdokumente | `SPEZIFIKATION.md` |
| `server/` | Backend (Node.js): API, Zugang, KI-Anbindung, Daten | leer |
| `web/widget/` | Chat-Baustein zum Einbinden in HTML-Seiten | leer |
| `web/admin/` | Verwaltungsoberfläche | leer |
| `deploy/` | Dateien für Installation und Betrieb beim Kunden | leer |

## Dokumente

- [`docs/SPEZIFIKATION.md`](docs/SPEZIFIKATION.md): Entscheidungen, Vorschläge und offene Punkte.

## Lizenz

Proprietär, siehe [`LICENSE`](LICENSE). Kein Open-Source-Projekt.
