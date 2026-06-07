# AGENTS.md – Arbeitsweise für Codex

Du arbeitest an einer Django/PostgreSQL-Web-App für GSH-interne Verwaltungs-, Controlling- und Analyseprozesse.

## Grundprinzip
Arbeite ruhig, gründlich und schrittweise. Nichts überstürzen. Bei größeren Änderungen zuerst analysieren, dann einen Plan erstellen, dann erst Code ändern.

## Pflicht: Plugins, Skills und verfügbare Tools nutzen
Nutze aktiv alle in Codex verfügbaren und installierten Plugins, Skills, MCP-Server und Integrationen, sofern sie zur Aufgabe passen.
Vor größeren Aufgaben:
1. Prüfe, welche Plugins/Skills relevant sind.
2. Nutze sie aktiv für Analyse, UI, Datenbank, Tests, Refactoring, Dokumentation und Projektstruktur.
3. Dokumentiere kurz, welche Plugins/Skills verwendet wurden und wofür.

## Technologie-Stack
- Backend: Django
- Datenbank: PostgreSQL
- Frontend: moderne, ruhige, intuitive UI
- Fokus: Performance, saubere Datenmodelle, stabile Imports, Auswertungen, Drilldowns, Fortschrittsanzeigen

## UI-Zielbild
Die UI soll nüchtern, professionell und klar sein, aber hochwertig und modern wirken.
Keine überladene bunte Oberfläche.
Gewünscht sind:
- klare Dashboards
- Karten, Tabellen, Filter, Drilldowns
- dezente Animationen
- sichtbare Aktivität
- Fortschrittsanzeigen bei längeren Berechnungen
- Live-Logs
- Statusanzeigen
- laufende Prozessanzeige
- klare Hinweise: was läuft, wie weit ist es, was wurde erledigt, was kommt als Nächstes

Nichts ist schlechter als eine App, die im Hintergrund arbeitet, ohne dass der Nutzer sieht, was passiert.

## Lange Prozesse
Für Importe, Berechnungen, Analysen und Datenbankjobs immer vorsehen:
- Fortschrittsbalken
- Prozentanzeige
- aktuelle Aktion
- Anzahl verarbeiteter Datensätze
- geschätzte Restdauer, wenn möglich
- Live-Log im Frontend
- saubere Fehlerausgabe
- Abbruchmöglichkeit, wenn sinnvoll

Technisch bevorzugt:
- Celery/RQ oder Django Background Tasks für lange Jobs
- WebSocket/SSE/Polling für Statusupdates
- eigenes JobStatus-Modell in PostgreSQL
- strukturierte Logs pro Lauf

## Datenbank
PostgreSQL sauber nutzen:
- sinnvolle Models
- Foreign Keys statt Freitext, wenn möglich
- Indizes für häufige Filter
- Migrationen sauber halten
- keine unnötigen N+1 Queries
- select_related / prefetch_related nutzen
- große Datenmengen paginieren
- Import- und Prüfprotokolle speichern

## Coding-Regeln
- Vor Änderungen immer vorhandene Struktur analysieren.
- Keine unnötigen Komplettumbauten.
- Kleine, nachvollziehbare Schritte.
- Bestehende Funktionen nicht zerstören.
- Nach Änderungen Tests/Checks ausführen.
- Bei UI-Änderungen Screens/Komponenten konsistent halten.
- Code lesbar, wartbar und robust schreiben.

## Antwortstil
Erkläre knapp:
1. was analysiert wurde,
2. was geändert wurde,
3. welche Dateien betroffen sind,
4. wie getestet wurde,
5. was als nächster sinnvoller Schritt empfohlen wird.

## Pflicht: Activity-First UI und Prozesssichtbarkeit

Policy-Marker: `ACTIVITY_FIRST_UI_V1`

Activity-first UI ist Pflicht. Bei jeder UI-, Import-, Analyse-, Export-, Bestell-, Dokument-, Datenbank- oder Hintergrundjob-Funktion muss sichtbar sein, was gerade läuft, was erledigt ist, was offen ist, ob Fehler vorliegen und was als nächste Aktion sinnvoll ist.

Keine langen Prozesse ohne sichtbaren Status, Fortschritt, Live-Log und nächste Aktion. Arbeiten im Hintergrund ohne erkennbare Aktivität sind nicht zulässig.

Bei jeder UI-/Workflow-Änderung ist [docs/ui-activity-standard.md](docs/ui-activity-standard.md) verbindlich zu beachten. Wenn ein Projekt noch keine passende technische Grundlage besitzt, muss sie im Rahmen der Änderung ergänzt oder ausdrücklich als blockierender Projektmangel dokumentiert werden.
---

# GitHub Copilot Projektregeln

Diese Datei beginnt mit dem verbindlichen AGENTS.md-Pflichtblock. Projektspezifische Regeln koennen darunter ergaenzt werden.
