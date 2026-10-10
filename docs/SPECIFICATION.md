# KI-Dialogstudio Server – Specification (as of 10 Oct 2026)

German translation: `SPECIFICATION-DE.md`.

Status: specification before the talks with prospective business customers. There is no code yet.

Labels used in this document:

| Label | Meaning |
|---|---|
| **D** | decided |
| **O** | open |

## 1. Purpose and scope

| Topic | Content | |
|---|---|---|
| Product | AI courses and AI assistants for plain HTML pages, without WordPress | D |
| Target group | Businesses | D |
| Operation | The customer runs the software on its own server. ReCons operates nothing | D |
| AI costs | The customer uses its own API keys. No request passes through ReCons servers | D |
| Data | All data stays with the customer | D |
| License | Annual subscription | D |
| Copyright holder | ReCons (Einzelgewerbe, i.e. sole proprietorship) | D |
| Relation to the plugins | Separate product with its own repository. No shared code with Lite and Pro (PHP), but the same functional concept | D |
| Priority | After the release of Lite and Pro | D |

## 2. Decisions of 10 Oct 2026

**Foundations**

1. A server run by the customer is a prerequisite.
2. Two access types: login mode and anonymous chat.
3. Login with built-in user management and with a connection to the company login (SSO).
4. Backend in Node.js, because the customer's environment cannot be predicted.
5. Name: KI-Dialogstudio Server. The trademark application "Dialogstudio" (classes 9, 41, 42) covers the product family.
6. Repository: `buzzer111/ki-dialogstudio-server`.

**Structure and technology**

7. Feature scope of 1.0 as in section 4, based on KI-Dialogstudio (Pro) 3.1.0, without selling access.
8. TypeScript, Fastify, PostgreSQL, delivery as a Docker image.
9. SSO via OpenID Connect first; SAML 2.0 only if a customer requires it.
10. License check via a signed license file, without an internet connection.
11. Embedding via a script and a custom HTML element.
12. Knowledge base in 1.0 as full text, as in Pro.
13. No floating chat window in 1.0.
14. The English AI instructions from Pro are the shared basis (reservation: section 11, no. 7).

**Documents**

15. Documents: English is the original, German the translation. Code and user interface in English with a German translation.
16. Copyright holder: ReCons (Einzelgewerbe).

## 3. Components

| Component | Task | Folder |
|---|---|---|
| Server | API, access, sessions, learning progress, AI requests, limits, log, license check | `server/` |
| Chat widget | Embedded into an HTML page by a script; shows the course or assistant | `web/widget/` |
| Administration | Assistants, users, providers, settings, statistics, log, license | `web/admin/` |
| Database | Assistants, users, sessions, usage, log, knowledge base | at the customer |
| Installation | Files for start-up, settings and updates | `deploy/` |

Flow of a request:

1. The customer's HTML page loads the chat widget from the customer's server.
2. The widget signs the participant in (login) or starts an anonymous session.
3. The widget sends the message to the server.
4. The server checks access, limits and consent, adds instructions, learning state and knowledge base, and calls the AI provider with the customer's key.
5. The answer is streamed back. The server stores progress, usage and, if switched on, the log.

API keys and AI instructions never leave the server towards the browser.

## 4. Feature scope

Based on KI-Dialogstudio (Pro) 3.1.0.

| Feature | Pro 3.1.0 | Server 1.0 |
|---|---|---|
| Assistants | unlimited | unlimited |
| Modes | course, knowledge assistant, guided assistant | all three |
| Learning progress per topic | yes | yes (persistent in login mode only) |
| Completion | summary, checklist, exam with PDF certificate | summary, checklist; exam and certificate: O |
| AI providers | OpenAI, Gemini, xAI, Anthropic, OpenRouter | the same; plus a custom OpenAI-compatible endpoint (e.g. Azure OpenAI, local models) |
| Provider choice by participants | yes | yes, can be switched off |
| Streaming | yes | yes |
| Custom instructions per assistant | yes | yes |
| Questions for participants before the chat | yes, 10 fields | yes |
| Knowledge base from documents (PDF, DOCX, TXT, MD) | yes, full text up to 60,000 characters | yes, full text; search in larger collections not in 1.0 |
| Draft a course with AI | from text and documents | yes |
| Conversation log in the administration | yes, off by default | yes, off by default |
| Limits | per user and day, per month, per role | the same; plus per anonymous session and per IP address |
| Statistics | periods, providers, participants, costs | yes |
| Course language per assistant | 22 languages or automatic | yes |
| User interface | English and German | English and German |
| Consent, retention periods, data export and erasure | yes | yes |
| Selling access (WooCommerce) | yes | no |
| Embedding | shortcode, Gutenberg block | script in an HTML page |

New compared to the plugins, because WordPress is missing:

1. Built-in user management (accounts, roles, password reset).
2. Connection to the company login.
3. Anonymous chat with abuse protection.
4. Embedding on external pages (allowed addresses, see section 6).
5. Installation, backup and update without the WordPress backend.

## 5. Access

| Access type | Description | |
|---|---|---|
| Anonymous chat | No sign-in. Session in the browser. Progress applies to this session only | D |
| Built-in user management | Accounts on the server: email and password, roles | D |
| Company login (SSO) | Sign-in through the customer's identity system | D |

Each assistant defines which access types apply.

| Topic | Definition |
|---|---|
| SSO protocol | OpenID Connect first (Microsoft Entra ID, Google Workspace, Keycloak, Okta); SAML 2.0 only if a customer requires it |
| Roles | administrator, editor (maintains assistants), participant |
| Mapping with SSO | Groups of the company login are mapped to roles and assistants |
| Anonymous chat | Limit per session and per IP address, monthly limit for the whole installation, optional captcha |
| Passwords | stored as a hash (Argon2id), never in plain text |

## 6. Embedding into HTML pages

```html
<script src="https://dialog.kunde.example/widget.js" defer></script>
<ki-dialogstudio assistant="einarbeitung"></ki-dialogstudio>
```

| Topic | Definition |
|---|---|
| Technology | custom HTML element, encapsulated design, no dependency on the customer's page |
| Allowed pages | list of addresses allowed to embed the widget, in the administration |
| Appearance | colors and font via CSS variables |
| Presentation | embedded in the page; no floating window in 1.0 |
| Login in the widget | redirect to the server's login, then back to the page |

## 7. Data and privacy

| Topic | Content | |
|---|---|---|
| Storage location | exclusively at the customer | D |
| Transfer to third parties | only to the AI provider chosen by the customer | D |
| Transfer to ReCons | no content, no usage data, no license request | D |
| API keys | stored encrypted; the key for this is in the server settings | D |
| Consent | before the first request, on by default | D |
| Retention periods | inactive conversations after 90 days, adjustable | D |
| Access and erasure | export and erasure per user in the administration | D |
| Log | off by default; notice for participants when switched on | D |
| Role of ReCons | software vendor, not a data processor, because ReCons processes no data | O, legal review |

## 8. Technology

| Topic | Definition | On customer request |
|---|---|---|
| Runtime | Node.js, the current LTS version at the time | – |
| Language | TypeScript | – |
| Server library | Fastify | – |
| Database | PostgreSQL | SQLite for small installations; MySQL/MariaDB |
| Delivery | Docker image with a Docker Compose file | archive for installation directly under Node.js |
| Settings | environment variables and administration interface | – |
| HTTPS | via the customer's reverse proxy | – |
| Knowledge base files | text in the database, as in Pro | – |
| PDF certificate | only if the exam is part of 1.0 | – |
| Tests | component and end-to-end tests with mocked AI providers, as for the plugins | – |

The column "On customer request" lists extensions that are not part of 1.0.

## 9. License and sales

| Topic | Content | |
|---|---|---|
| Model | annual subscription | D |
| Seller | ReCons | D |
| License check | signed license file with an expiry date, checked without an internet connection. ReCons needs no license server | D |
| Updates | the customer downloads a new version and installs it | D |
| Price | – | O |
| Basis of pricing | per installation, per number of users or per company | O |
| Behavior after expiry | – | O |
| Support | scope and response times | O |
| Payment processing | invoice by ReCons or a payment provider | O |

On the license check: the plugins use the Freemius WordPress SDK; its check needs a connection to Freemius. Servers in company networks often have no internet access, hence the signed license file. Whether Freemius is an option for payment processing for the server has not been examined.

## 10. Not in version 1.0

1. Selling access to end customers.
2. Operation by ReCons (hosted version).
3. Several separate companies in one installation.
4. AI performs actions (function calling, MCP).
5. Voice input and read-aloud.
6. File upload by participants in the chat.
7. Connection to learning platforms (SCORM, LTI).
8. Floating chat window.
9. Search in large document collections.

## 11. Open decisions

| # | Question | Depends on |
|---|---|---|
| 1 | Price and basis of pricing | customer talks |
| 2 | Behavior after the subscription expires | contract design |
| 3 | Exam with PDF certificate in 1.0 | customer talks |
| 4 | Support: scope and response times | customer talks |
| 5 | Payment processing | – |
| 6 | License terms (the file `LICENSE` is a placeholder) | legal review |
| 7 | Rights to the AI instructions from Pro (`prompts/course.php`, `guided.php`, `knowledge.php`) | clarification of rights, which is also pending for Pro before sales start |
| 8 | Role of ReCons under data protection law | legal review |

## 12. Questions for the customer talks

1. Which identity system does the company use (Microsoft Entra ID, Google, Keycloak, other)?
2. Where does the server run: own data center, the company's cloud account, with or without internet access?
3. Is Docker permitted? Which databases does IT already operate?
4. Which AI providers are approved? Is there a custom endpoint (Azure OpenAI, local models)?
5. Purpose: onboarding, training, knowledge assistant, customer advice on the public website?
6. How many participants, how many assistants?
7. Is proof required (exam, certificate, attendance list)?
8. How large are the document collections for the knowledge base?
9. Which requirements come from data protection, works council and IT security (log, retention periods, software review)?
10. Which pricing model is customary for the company (per installation, per user)?

Answers to 1 to 3 may extend the definitions in section 8 by the column "On customer request".

## 13. Order of implementation

Start after the release of Lite and Pro.

| Step | Content |
|---|---|
| 1 | Project setup: TypeScript, Fastify, PostgreSQL, Docker, tests |
| 2 | Server basis: database, settings, encrypted keys, AI connection with streaming |
| 3 | Course mode with learning progress, anonymous chat, chat widget |
| 4 | Built-in user management, administration interface, limits, statistics |
| 5 | Company login (OpenID Connect) |
| 6 | Knowledge assistant, guided assistant, knowledge base, course draft |
| 7 | Log, privacy functions, license check |
| 8 | Installation package, guide in English and German, test with a pilot customer |

## 14. Sources

Documents in the repository `buzzer111/ki-dialogstudio`, folder `docs/`:

1. `ki-dialogstudio-lite-free-pro-schnitt.md` (feature split and default values, as of 10 Oct 2026)
2. `ki-dialogstudio-entwicklung-lite-1.3-pro-3.1.md`
3. `ki-dialogstudio-feature-vergleich.md`
