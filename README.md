# KI-Dialogstudio Server

German translation: `README-DE.md`.

Business edition of KI-Dialogstudio: AI courses and AI assistants for plain HTML pages, without WordPress. The customer runs the software on its own server, with its own AI key and its own data.

**Status:** specification. There is no runnable code yet.

## Product family

| Product | Platform | Sales |
|---|---|---|
| KI-Dialogstudio Lite | WordPress | free, WordPress.org |
| KI-Dialogstudio | WordPress | annual subscription and lifetime |
| **KI-Dialogstudio Server** | Node.js, self-hosted | annual subscription |

The two WordPress plugins live in the repository `buzzer111/ki-dialogstudio`.

## Technology

TypeScript, Fastify, PostgreSQL, delivery as a Docker image. Details: `docs/SPECIFICATION.md`, section 8.

## Folders

| Path | Content | Status |
|---|---|---|
| `docs/` | Specification and development documents | `SPECIFICATION.md`, `SPECIFICATION-DE.md` |
| `server/` | Backend (Node.js): API, access, AI connection, data | empty |
| `web/widget/` | Chat widget for embedding into HTML pages | empty |
| `web/admin/` | Administration interface | empty |
| `deploy/` | Files for installation and operation at the customer | empty |

## Documents

English is the original, German the translation. Changes go into the English original first, then into the translation.

| English original | German translation |
|---|---|
| `README.md` | `README-DE.md` |
| `docs/SPECIFICATION.md` | `docs/SPECIFICATION-DE.md` |

## License

Proprietary, see [`LICENSE`](LICENSE). Not an open-source project.
