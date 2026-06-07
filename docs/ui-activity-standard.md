# UI Activity Standard

Policy-Marker: `ACTIVITY_FIRST_UI_V1`

Dieser Standard ist verbindlich fuer alle GSH-internen Apps, Verwaltungswerkzeuge, Controlling-Oberflaechen, Labor-/Dokumentationsoberflaechen und datenverarbeitenden Workflows in diesem Repository.

Activity-first UI ist Pflicht. Eine Anwendung darf bei Importen, Analysen, Berechnungen, Exporten, Bestellungen, Dokumentverarbeitung, KI-/Assistenzfunktionen oder Datenbankjobs nicht im Hintergrund arbeiten, ohne dem Nutzer sichtbar zu zeigen, was gerade passiert.

Keine langen Prozesse ohne sichtbaren Status, Fortschritt, Live-Log und nächste Aktion.

## Zielbild

Die Oberflaeche soll ruhig, professionell und hochwertig wirken, aber lebendig genug sein, dass Arbeit sichtbar bleibt. Nutzer muessen jederzeit erkennen:

- was gerade laeuft,
- welcher Schritt erledigt wurde,
- welcher Schritt als Naechstes kommt,
- wie weit der Prozess ist,
- ob Daten aktuell oder veraltet sind,
- welche Fehler oder Warnungen offen sind,
- welche konkrete Aktion jetzt sinnvoll ist.

## Pflichtkomponenten

Bei jeder relevanten UI oder Workflow-Funktion muss mindestens eine passende Form dieser Komponenten vorgesehen werden:

- Activity Center: zentrale Liste der letzten und laufenden Ereignisse mit Zeit, Status, Quelle und Ergebnis.
- Job Status Panel: aktueller Lauf mit Status, Prozent, aktueller Aktion, Zaehlern, Startzeit, Dauer und Ergebnis.
- Timeline oder Audit-Spur: fachlich wichtige Statuswechsel, Entscheidungen, Importe, Exporte, E-Mail-/Bestellaktionen oder Review-Schritte.
- Live-Log: kurze, strukturierte Meldungen fuer laengere Prozesse; keine sensiblen Rohdaten.
- Status-Chips: klar erkennbare Zustaende wie laeuft, wartet, erledigt, Warnung, Fehler, abgebrochen.
- Naechste Aktion: bei Fehlern, offenen Pruefungen oder abgeschlossenen Laeufen muss sichtbar sein, was der Nutzer als Naechstes tun kann.

## Dashboards und Startseiten

Dashboards muessen eine erste Sicht auf Aktivitaet bieten. Mindestens sichtbar sein sollen, wenn fachlich relevant:

- Datenstand und letzte erfolgreiche Aktualisierung,
- laufende Jobs oder aktive Verarbeitung,
- letzte erfolgreiche Aktion,
- offene Pruefungen oder Entscheidungen,
- Fehler und Warnungen mit Handlungslink,
- Frische/Alter der wichtigsten Datenquellen.

## Lange Prozesse

Fuer Importe, Berechnungen, KI-/Analysejobs, Exporte, PDF-/OCR-Verarbeitung, E-Mail-Batches, Datenbankjobs und Massenaktionen sind verpflichtend:

- Fortschrittsbalken oder Fortschrittsring,
- Prozentanzeige, sofern sinnvoll berechenbar,
- aktuelle Aktion in Klartext,
- Anzahl verarbeiteter und gesamter Datensaetze,
- geschaetzte Restdauer, wenn belastbar moeglich,
- Live-Log oder Ereignisfeed,
- sauberer Fehlerstatus,
- Abbruchmoeglichkeit, wenn fachlich und technisch sinnvoll,
- Ergebniszusammenfassung nach Abschluss.

## Fehlerdarstellung

Fehler duerfen nicht nur als rote Meldung erscheinen. Jede Fehlerdarstellung muss moeglichst enthalten:

- was passiert ist,
- welcher Schritt oder Datensatz betroffen ist,
- ob der Prozess fortgesetzt, abgebrochen oder teilweise abgeschlossen wurde,
- welche Daten gespeichert oder verworfen wurden,
- welche konkrete Nutzeraktion als Naechstes sinnvoll ist,
- Link zu Detailansicht, Log oder Review-Queue, wenn vorhanden.

## Technische Umsetzung

Django/PostgreSQL:

- eigenes `JobStatus`-, `ActivityEvent`- oder aehnliches Modell fuer laengere Jobs und fachliche Ereignisse,
- Statuswerte wie `queued`, `running`, `waiting`, `done`, `warning`, `failed`, `cancelled`,
- strukturierte Felder fuer Fortschritt, aktuelle Aktion, Zaehler, Ergebnis, Fehlercode und Zeitstempel,
- `select_related` / `prefetch_related` fuer Activity-Feeds,
- serverseitige Zugriffskontrolle fuer alle Activity-Daten.

Flask/HTMX:

- dedizierte Status-Endpunkte fuer laufende Prozesse,
- HTMX-Polling fuer einfache Statusupdates,
- Server-Sent Events oder WebSockets fuer echte Live-Logs, wenn noetig,
- strukturierte Logs pro Lauf statt unlesbarer Textbloecke.

React/Vite:

- eigene Activity-/Timeline-Komponenten,
- klare Loading-, Running-, Empty-, Done-, Warning- und Failed-States,
- optimistic UI nur, wenn Serverzustand sauber abgeglichen wird,
- Activity-State nicht nur lokal halten, wenn er auditrelevant ist.

## Datenmodell-Mindestfelder

Ein langlebiger Job oder fachliches Activity-Event soll, soweit passend, enthalten:

- ID / Lauf-ID,
- Typ / Kategorie,
- Status,
- aktuelle Aktion,
- Fortschritt in Prozent,
- verarbeitete Datensaetze,
- Gesamtzahl,
- Quelle / Datei / Modul,
- verantwortlicher Benutzer oder Systemakteur,
- Startzeit,
- letzte Aktualisierung,
- Ende,
- Ergebniszusammenfassung,
- Fehlercode und sichere Fehlermeldung,
- Link zum Ergebnis oder zur Detailansicht.

## Visueller Stil

- Ruhig, dicht und professionell; keine spielerische Gamification.
- Aktivitaet darf sichtbar pulsieren oder animieren, aber nur dezent und nur fuer wirklich laufende Vorgaenge.
- Keine dekorativen Daueranimationen ohne Informationswert.
- Statusfarben sparsam und eindeutig nutzen: Erfolg, Warnung, Fehler, Info, Wartet.
- Texte muessen auf Desktop und Mobile passen und duerfen keine Inhalte ueberlagern.
- `prefers-reduced-motion` respektieren.
- Activity-Feeds muessen tastatur- und screenreader-tauglich bleiben.

## Sicherheits- und Datenschutzregeln

- Keine Passwoerter, Tokens, personenbezogenen Rohdaten, Gehaltsdetails, vertraulichen Inhalte oder internen Geheimnisse in Activity-Logs schreiben.
- Logs fuer normale Nutzer fachlich verstaendlich und datensparsam formulieren.
- Detaillierte technische Fehler nur fuer berechtigte Admin-/Developer-Rollen anzeigen.
- Sensible Dateipfade nur anzeigen, wenn es fachlich noetig und berechtigt ist.

## Tests und Abnahme

Bei Activity-UI-Aenderungen pruefen:

- leerer Zustand,
- laufender Zustand,
- erfolgreicher Abschluss,
- Warnung / Teilabschluss,
- Fehlerfall,
- Abbruch, falls vorhanden,
- lange Texte und viele Events,
- Mobile-Layout,
- Barrierefreiheit und reduzierte Bewegung,
- keine sensiblen Daten im sichtbaren Log.

## Nicht akzeptabel

Nicht akzeptabel sind:

- reine Spinner ohne Kontext bei laengeren Prozessen,
- stumme Hintergrundjobs,
- abgeschlossene Prozesse ohne Ergebniszusammenfassung,
- Fehler ohne naechste Aktion,
- Live-Logs mit sensiblen Rohdaten,
- Activity-Anzeigen, die nur dekorativ sind und keinen echten Systemzustand zeigen.