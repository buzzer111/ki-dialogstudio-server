# KI-Dialogstudio Server – Spezifikation (Stand 10.10.2026)

English original: `SPECIFICATION.md`.

Status: Spezifikation vor den Gesprächen mit möglichen Firmenkunden. Es gibt noch keinen Code.

Kennzeichnung in diesem Dokument:

| Kennzeichen | Bedeutung |
|---|---|
| **E** | entschieden |
| **O** | offen |

## 1. Zweck und Abgrenzung

| Thema | Inhalt | |
|---|---|---|
| Produkt | KI-Kurse und KI-Assistenten für normale HTML-Seiten, ohne WordPress | E |
| Zielgruppe | Firmen | E |
| Betrieb | Der Kunde betreibt die Software auf einem eigenen Server. ReCons betreibt nichts | E |
| KI-Kosten | Der Kunde nutzt eigene API-Schlüssel. Keine Anfrage läuft über Server von ReCons | E |
| Daten | Alle Daten liegen beim Kunden | E |
| Lizenz | Jahresabo | E |
| Rechteinhaber | ReCons (Einzelgewerbe) | E |
| Verhältnis zu den Plugins | Eigenes Produkt mit eigenem Repository. Kein gemeinsamer Code mit Lite und Pro (PHP), aber dasselbe Fachkonzept | E |
| Priorität | Nach der Veröffentlichung von Lite und Pro | E |

## 2. Entscheidungen vom 10.10.2026

**Grundlagen**

1. Eigener Server beim Kunden ist Voraussetzung.
2. Zwei Zugangsarten: Login-Modus und anonymer Chat.
3. Login mit eigener Nutzerverwaltung und mit Anbindung an das Firmen-Login (SSO).
4. Backend in Node.js, weil der Einsatz beim Kunden nicht abschätzbar ist.
5. Name: KI-Dialogstudio Server. Die Markenanmeldung „Dialogstudio" (Klassen 9, 41, 42) deckt die Produktfamilie ab.
6. Repository: `buzzer111/ki-dialogstudio-server`.

**Aufbau und Technik**

7. Funktionsumfang 1.0 laut Abschnitt 4, auf Basis von KI-Dialogstudio (Pro) 3.1.0, ohne Verkauf von Zugängen.
8. TypeScript, Fastify, PostgreSQL, Auslieferung als Docker-Image.
9. SSO zuerst über OpenID Connect; SAML 2.0 nur bei Bedarf eines Kunden.
10. Lizenzprüfung über eine signierte Lizenzdatei, ohne Internetverbindung.
11. Einbindung über ein Skript und ein eigenes HTML-Element.
12. Wissensbasis in 1.0 als Volltext wie in Pro.
13. Kein schwebendes Chat-Fenster in 1.0.
14. Die englischen KI-Anweisungen aus Pro sind die gemeinsame Grundlage (Vorbehalt: Abschnitt 11, Nr. 7).

**Dokumente**

15. Dokumente: Englisch ist das Original, Deutsch die Übersetzung. Code und Oberfläche Englisch mit deutscher Übersetzung.
16. Rechteinhaber: ReCons (Einzelgewerbe).

## 3. Bausteine

| Baustein | Aufgabe | Ordner |
|---|---|---|
| Server | API, Zugang, Sitzungen, Lernfortschritt, KI-Anfragen, Limits, Protokoll, Lizenzprüfung | `server/` |
| Chat-Baustein | Wird per Skript in eine HTML-Seite eingebunden und zeigt Kurs oder Assistent | `web/widget/` |
| Verwaltung | Assistenten, Nutzer, Anbieter, Einstellungen, Auswertung, Protokoll, Lizenz | `web/admin/` |
| Datenbank | Assistenten, Nutzer, Sitzungen, Verbrauch, Protokoll, Wissensbasis | beim Kunden |
| Installation | Dateien für Start, Einstellungen und Update | `deploy/` |

Ablauf einer Anfrage:

1. Die HTML-Seite des Kunden lädt den Chat-Baustein vom Server des Kunden.
2. Der Baustein meldet den Teilnehmer an (Login) oder startet eine anonyme Sitzung.
3. Der Baustein sendet die Nachricht an den Server.
4. Der Server prüft Zugang, Limits und Einwilligung, ergänzt Anweisungen, Lernstand und Wissensbasis und ruft den KI-Anbieter mit dem Schlüssel des Kunden auf.
5. Die Antwort läuft als Datenstrom zurück. Der Server speichert Fortschritt, Verbrauch und, falls eingeschaltet, das Protokoll.

API-Schlüssel und Anweisungen an die KI verlassen den Server nie in Richtung Browser.

## 4. Funktionsumfang

Grundlage ist KI-Dialogstudio (Pro) 3.1.0.

| Funktion | Pro 3.1.0 | Server 1.0 |
|---|---|---|
| Assistenten | beliebig viele | beliebig viele |
| Modi | Kurs, Wissensassistent, geführter Assistent | alle drei |
| Lernfortschritt je Thema | ja | ja (nur im Login-Modus dauerhaft) |
| Abschluss | Zusammenfassung, Checkliste, Prüfung mit PDF-Zertifikat | Zusammenfassung, Checkliste; Prüfung und Zertifikat: O |
| KI-Anbieter | OpenAI, Gemini, xAI, Anthropic, OpenRouter | dieselben; zusätzlich eigener OpenAI-kompatibler Endpunkt (z. B. Azure OpenAI, lokale Modelle) |
| Anbieterwahl durch Teilnehmer | ja | ja, abschaltbar |
| Streaming | ja | ja |
| Eigene Anweisungen je Assistent | ja | ja |
| Fragen an Teilnehmer vor dem Chat | ja, 10 Felder | ja |
| Wissensbasis aus Dokumenten (PDF, DOCX, TXT, MD) | ja, Volltext bis 60.000 Zeichen | ja, Volltext; Suche in größeren Beständen nicht in 1.0 |
| Kurs mit KI entwerfen | aus Text und Dokumenten | ja |
| Gesprächsprotokoll in der Verwaltung | ja, standardmäßig aus | ja, standardmäßig aus |
| Limits | je Nutzer und Tag, je Monat, je Rolle | dieselben; zusätzlich je anonymer Sitzung und je IP-Adresse |
| Auswertung | Zeiträume, Anbieter, Teilnehmer, Kosten | ja |
| Kurssprache je Assistent | 22 Sprachen oder automatisch | ja |
| Oberfläche | Englisch und Deutsch | Englisch und Deutsch |
| Einwilligung, Löschfristen, Datenexport und -löschung | ja | ja |
| Zugang verkaufen (WooCommerce) | ja | nein |
| Einbindung | Shortcode, Gutenberg-Block | Skript in HTML-Seite |

Neu gegenüber den Plugins, weil WordPress fehlt:

1. Eigene Nutzerverwaltung (Konten, Rollen, Passwort zurücksetzen).
2. Anbindung an das Firmen-Login.
3. Anonymer Chat mit Schutz vor Missbrauch.
4. Einbindung auf fremden Seiten (erlaubte Adressen, siehe Abschnitt 6).
5. Installation, Sicherung und Update ohne WordPress-Backend.

## 5. Zugang

| Zugangsart | Beschreibung | |
|---|---|---|
| Anonymer Chat | Ohne Anmeldung. Sitzung im Browser. Fortschritt gilt nur für diese Sitzung | E |
| Eigene Nutzerverwaltung | Konten im Server: E-Mail und Passwort, Rollen | E |
| Firmen-Login (SSO) | Anmeldung über das Anmeldesystem des Kunden | E |

Je Assistent wird festgelegt, welche Zugangsarten gelten.

| Thema | Festlegung |
|---|---|
| SSO-Protokoll | zuerst OpenID Connect (Microsoft Entra ID, Google Workspace, Keycloak, Okta); SAML 2.0 nur bei Bedarf eines Kunden |
| Rollen | Administrator, Redakteur (Assistenten pflegen), Teilnehmer |
| Zuordnung bei SSO | Gruppen des Firmen-Logins werden Rollen und Assistenten zugeordnet |
| Anonymer Chat | Limit je Sitzung und je IP-Adresse, Monatslimit für die ganze Installation, optional Captcha |
| Passwörter | gespeichert als Hash (Argon2id), nie im Klartext |

## 6. Einbindung in HTML-Seiten

```html
<script src="https://dialog.kunde.example/widget.js" defer></script>
<ki-dialogstudio assistant="einarbeitung"></ki-dialogstudio>
```

| Thema | Festlegung |
|---|---|
| Technik | eigenes HTML-Element, gekapseltes Design, ohne Abhängigkeit zur Seite des Kunden |
| Erlaubte Seiten | Liste der Adressen, die den Baustein einbinden dürfen, in der Verwaltung |
| Aussehen | Farben und Schrift über CSS-Variablen |
| Darstellung | eingebettet in die Seite; kein schwebendes Fenster in 1.0 |
| Login im Baustein | Weiterleitung zum Login des Servers, danach zurück zur Seite |

## 7. Daten und Datenschutz

| Thema | Inhalt | |
|---|---|---|
| Speicherort | ausschließlich beim Kunden | E |
| Übertragung an Dritte | nur an den vom Kunden gewählten KI-Anbieter | E |
| Übertragung an ReCons | keine Inhalte, keine Nutzungsdaten, keine Lizenzabfrage | E |
| API-Schlüssel | verschlüsselt gespeichert, Schlüssel dazu in den Einstellungen des Servers | E |
| Einwilligung | vor der ersten Anfrage, standardmäßig an | E |
| Löschfristen | inaktive Verläufe nach 90 Tagen, einstellbar | E |
| Auskunft und Löschung | Export und Löschung je Nutzer in der Verwaltung | E |
| Protokoll | standardmäßig aus; Hinweis für Teilnehmer, wenn eingeschaltet | E |
| Rolle von ReCons | Softwarehersteller, kein Auftragsverarbeiter, weil ReCons keine Daten verarbeitet | O, rechtlich prüfen |

## 8. Technik

| Thema | Festlegung | Bei Kundenwunsch |
|---|---|---|
| Laufzeit | Node.js, jeweils aktuelle LTS-Version | – |
| Sprache | TypeScript | – |
| Server-Bibliothek | Fastify | – |
| Datenbank | PostgreSQL | SQLite für kleine Installationen; MySQL/MariaDB |
| Auslieferung | Docker-Image mit Docker-Compose-Datei | Archiv für Installation direkt unter Node.js |
| Einstellungen | Umgebungsvariablen und Verwaltungsoberfläche | – |
| HTTPS | über den Reverse-Proxy des Kunden | – |
| Dateien der Wissensbasis | Text in der Datenbank, wie in Pro | – |
| PDF-Zertifikat | nur falls Prüfung in 1.0 enthalten ist | – |
| Tests | Komponenten- und Ende-zu-Ende-Tests mit nachgebildeten KI-Anbietern, wie bei den Plugins | – |

Die Spalte „Bei Kundenwunsch" nennt Erweiterungen, die nicht Teil von 1.0 sind.

## 9. Lizenz und Vertrieb

| Thema | Inhalt | |
|---|---|---|
| Modell | Jahresabo | E |
| Verkäufer | ReCons | E |
| Lizenzprüfung | signierte Lizenzdatei mit Ablaufdatum, Prüfung ohne Internetverbindung. ReCons braucht keinen Lizenzserver | E |
| Updates | Kunde lädt neue Version und installiert sie selbst | E |
| Preis | – | O |
| Bemessung | je Installation, je Nutzerzahl oder je Firma | O |
| Verhalten nach Ablauf | – | O |
| Support | Umfang und Reaktionszeiten | O |
| Zahlungsabwicklung | Rechnung durch ReCons oder Zahlungsanbieter | O |

Zur Lizenzprüfung: Die Plugins nutzen das WordPress-SDK von Freemius; die Prüfung dort braucht eine Verbindung zu Freemius. Server in Firmennetzen haben oft keinen Internetzugang, deshalb die signierte Lizenzdatei. Ob Freemius als Zahlungsabwicklung für den Server in Frage kommt, ist nicht geprüft.

## 10. Nicht in Version 1.0

1. Verkauf von Zugängen an Endkunden.
2. Betrieb durch ReCons (gehostete Version).
3. Mehrere getrennte Firmen in einer Installation.
4. KI führt Aktionen aus (Function Calling, MCP).
5. Spracheingabe und Vorlesen.
6. Datei-Upload durch Teilnehmer im Chat.
7. Anbindung an Lernplattformen (SCORM, LTI).
8. Schwebendes Chat-Fenster.
9. Suche in großen Dokumentbeständen.

## 11. Offene Entscheidungen

| # | Frage | Hängt ab von |
|---|---|---|
| 1 | Preis und Bemessung | Kundengespräche |
| 2 | Verhalten nach Ablauf des Abos | Vertragsgestaltung |
| 3 | Prüfung mit PDF-Zertifikat in 1.0 | Kundengespräche |
| 4 | Support: Umfang und Reaktionszeiten | Kundengespräche |
| 5 | Zahlungsabwicklung | – |
| 6 | Lizenzbedingungen (Datei `LICENSE` ist ein Platzhalter) | rechtliche Prüfung |
| 7 | Rechte an den KI-Anweisungen aus Pro (`prompts/course.php`, `guided.php`, `knowledge.php`) | Rechteklärung, die auch für Pro vor dem Verkaufsstart ansteht |
| 8 | Rolle von ReCons im Datenschutz | rechtliche Prüfung |

## 12. Fragen für die Kundengespräche

1. Welches Anmeldesystem nutzt die Firma (Microsoft Entra ID, Google, Keycloak, anderes)?
2. Wo läuft der Server: eigenes Rechenzentrum, Cloud-Konto der Firma, mit oder ohne Internetzugang?
3. Ist Docker zugelassen? Welche Datenbanken betreibt die IT bereits?
4. Welche KI-Anbieter sind freigegeben? Gibt es einen eigenen Endpunkt (Azure OpenAI, lokale Modelle)?
5. Einsatzzweck: Einarbeitung, Schulung, Wissensassistent, Kundenberatung auf der öffentlichen Website?
6. Wie viele Teilnehmer, wie viele Assistenten?
7. Wird ein Nachweis gebraucht (Prüfung, Zertifikat, Teilnahmeliste)?
8. Wie groß sind die Dokumentbestände für die Wissensbasis?
9. Welche Anforderungen stellen Datenschutz, Betriebsrat und IT-Sicherheit (Protokoll, Löschfristen, Prüfung der Software)?
10. Welches Preismodell ist für die Firma üblich (je Installation, je Nutzer)?

Antworten zu 1 bis 3 können die Festlegungen in Abschnitt 8 um die Spalte „Bei Kundenwunsch" erweitern.

## 13. Reihenfolge der Umsetzung

Beginn nach der Veröffentlichung von Lite und Pro.

| Schritt | Inhalt |
|---|---|
| 1 | Projektaufbau: TypeScript, Fastify, PostgreSQL, Docker, Tests |
| 2 | Server-Grundlage: Datenbank, Einstellungen, verschlüsselte Schlüssel, KI-Anbindung mit Streaming |
| 3 | Kursmodus mit Lernfortschritt, anonymer Chat, Chat-Baustein |
| 4 | Eigene Nutzerverwaltung, Verwaltungsoberfläche, Limits, Auswertung |
| 5 | Firmen-Login (OpenID Connect) |
| 6 | Wissensassistent, geführter Assistent, Wissensbasis, Kursentwurf |
| 7 | Protokoll, Datenschutzfunktionen, Lizenzprüfung |
| 8 | Installationspaket, Anleitung Englisch und Deutsch, Test bei einem Pilotkunden |

## 14. Quellen

Dokumente im Repository `buzzer111/ki-dialogstudio`, Ordner `docs/`:

1. `ki-dialogstudio-lite-free-pro-schnitt.md` (Funktionsschnitt und Standardwerte, Stand 10.10.2026)
2. `ki-dialogstudio-entwicklung-lite-1.3-pro-3.1.md`
3. `ki-dialogstudio-feature-vergleich.md`
