---
description: >
  Autonomer Arbeitsloop für eine ausdrücklich gestartete Task-, Projekt- oder Backlog-Queue mit Harness,
  Worker- und Checker-Wellen, lokaler Integration, Beweisen und gesicherten Stopbedingungen.
type: playbook
edit: locked
license: MIT
---

# Arbeit ausführen

Aus einem vorbereiteten Arbeitsumfang entstehen integrierte, belegte Ergebnisse oder eine gesicherte
Review-Schlange. Der Nutzer besitzt Start und Scope. Innerhalb dieses Rahmens besitzt der Hauptagent die
Disposition, Delegation, Prüfung und Integration.

Der Aufruf autorisiert Subagents, lokale Änderungen, Prüfungen, isolierte Worktrees und lokale Commits im
gewählten Scope. Er autorisiert keinen Push, Publish, kein Deployment, keine Nachricht an Dritte und keine
sonstige externe oder irreversible Wirkung.

## Harness festlegen

Bestimme vor der ersten Änderung den vollständigen Laufvertrag:

1. **Ziel:** Welcher beobachtbare Zustand beendet den Lauf erfolgreich?
2. **Scope-in:** Welche Tasks, Repos, Pfade und Systeme gehören zum Aufruf?
3. **Scope-out:** Welche benachbarten Ziele, Pfade und Wirkungen bleiben ausgeschlossen?
4. **Spine:** Welches Planungssystem hält Status, Abhängigkeiten, Befunde und Übergaben dauerhaft fest?
5. **Beweise:** Welche Tests, Zustände oder Artefakte belegen den Abschluss?
6. **Grenzen:** Welche Handlung braucht zwingend den Nutzer?

Nutzer- und Projektvorgaben bestimmen das Planungssystem. Nur ohne andere Vorgabe gilt der lokale
Callbell-Backlog. Ist ein vorgeschriebenes externes System nicht erreichbar, spiegle es nicht in lokale
Dateien; stoppe vor schreibender Arbeit mit dem konkreten Hindernis.

## Scope-in

Zum gewählten Lauf gehören:

- der ausdrücklich gewählte Task, das Projekt oder der Backlog und seine erfüllten Abhängigkeiten,
- lokale Repository-Dateien und ausschließlich dafür nötige Zustandsänderungen,
- lokale Tests, Builds, statische Prüfungen, Worktrees, Task-Branches und Commits,
- Subagents für getrennte Bestandsaufnahme, Umsetzung, Ursachenklärung und Verifikation.

## Scope-out

Außerhalb des gewählten Laufs bleiben immer:

- neue Produktziele, Scope-Erweiterungen und nicht belegte Annahmen über die Nutzerabsicht,
- Push, Publish, Deployment, produktive Infrastruktur und externe Kommunikation,
- Secrets, Zugangsdaten, irreversible Löschungen und sonstige hochriskante Aktionen,
- Entscheidungen über Produktumfang, Risikoakzeptanz und menschliche oder fachliche Abnahme.

Eine technische Schwierigkeit erweitert den Scope nicht. Sichere den Stand und übergib, wenn der Auftrag
ohne eine ausgeschlossene Handlung nicht fortgesetzt werden kann.

## Preflight

1. Lies zuerst nur Roster beziehungsweise externe Metadaten des gewählten Scopes. Ermittle daraus Status,
   Kurzstand, Reihenfolge und Besitzsignale, soweit das System sie anbietet, und bilde eine enge
   Kandidatenmenge für die nächste Welle. Kläre Abhängigkeiten aus Roster, Beziehungsfeldern oder den aktuellen
   Datensätzen dieser Kandidaten und lies danach nur gewählte Tasks und ihre echten Blocker vollständig. Bei
   einem ausdrücklich gewählten einzelnen Task lies dessen aktuellen Datensatz direkt; lade weder andere
   offene Tasks noch Historie vorsorglich.
2. Folge bei einem externen System der Lesepolitik seines Bindings. Öffne Kommentare oder andere Historie
   nur bei einem Widerspruch, fehlender entscheidungsrelevanter Begründung oder einem ausdrücklichen Verweis
   des aktuellen Datensatzes. Historie ersetzt nie den eigenständig ausführbaren Ist-Stand.
3. Wähle bei einem Backlog- oder Projektlauf nur `next`. Ein ausdrücklich genannter `ready`-Task gilt durch
   diesen Nutzeraufruf als freigegeben und wechselt vor der Beanspruchung über `next`. Weitere Übergänge
   folgen dem Statusmodell des maßgeblichen Planungssystems.
4. Verändere keinen `in-progress`-Task mit einem laufenden oder unbekannten Worker. Kläre zuerst dessen Eigentümer
   und Arbeitsstand.
5. Prüfe bei Git Root, Branch, Upstream, Worktrees und vollständigen Status. Schreibende Arbeit beginnt nur
   auf einem sauberen, seit dem Preflight unveränderten Steuerbranch.
6. Wende vor dem ersten schreibenden Git-Schritt den vom Einstieg genannten Git-Ablauf an.

Beanspruche jedes Paket unmittelbar vor seiner ersten schreibenden Ausführung im Spine. Im lokalen Backlog
ist das `in-progress`; extern gilt der im Binding festgelegte bedeutungsgleiche Status. Aktualisiere keinen
Status eines Pakets, das noch nicht Teil der nächsten Welle ist.

## Den Arbeitsloop führen

Führe bis zu einer Stopbedingung dieselbe Schleife aus:

### 1. Nächste Welle wählen

- Wähle nur ausführbare Arbeit mit erfüllten Abhängigkeiten.
- Parallelisiere nur getrennte Zielbereiche mit sicherer Isolation. Serialisiere jede mögliche
  Überschneidung.
- Plane nur die nächste Welle. Mehr Worker sind kein Ziel.
- Leite aus der Größe der `next`-Queue keine Parallelität ab. Der vorbereitete Horizont darf vollständig
  seriell ausgeführt werden.

### 2. Rollen besetzen

Der Hauptagent bleibt Eigentümer von Scope, Spine, Workersteuerung, Integration und Abschluss. Gib jedem
Worker genau ein Arbeitspaket mit Pfad oder ID, Arbeitsort, erlaubten Zielen, Abnahmekriterien, Prüfungen und
Rückgabeformat. Worker verändern weder Spine noch Branchverwaltung, committen nicht und spawnen keine
eigenen Subagents.

Nutze Subagents nur, wenn Trennung einen messbaren Vorteil bringt:

- Ein Worker setzt ein hinreichend eigenständiges Paket um.
- Ein unabhängiger Checker prüft bei mittlerem oder hohem Risiko den Diff und die Beweise, ohne die
  Rechtfertigung des Workers als Wahrheit zu übernehmen.
- Kleine Zustands- und Integrationsschritte erledigt der Hauptagent selbst.
- Fehlen Subagents, führe höchstens einen sicher seriellen Task selbst aus und benenne diese Abweichung im
  Abschluss. Simuliere keine unabhängige Prüfung.

Sende unmittelbar nach dem erfolgreichen Start der Worker genau eine gerenderte Markdown-Karte für die
Welle:

> **Welle: 2 von 4**
>
> **Aufgabe:** #84 - Dateizugriff auf SQL umstellen
>
> **Sub-Agent:** gestartet mit terra

Verwende ID und Titel aus dem Spine. Nenne den tatsächlich gewählten oder geerbten Modellnamen in seiner
kurzen, erkennbaren Form, etwa `terra`, `opus`, `sonnet` oder `sol`; ist er dem Hauptagenten nicht bekannt,
schreibe `nicht ausgewiesen`, statt ihn zu erraten. Wiederhole bei mehreren Workern das Paar aus Aufgabe und
Sub-Agent innerhalb derselben Karte. Arbeitet die Welle ohne Subagent, schreibe `Sub-Agent: keiner;
Ausführung durch Hauptagent`.

### 3. Beobachten und korrigieren

Prüfe Rückgaben gegen den tatsächlichen Diff, die Umwelt und die vereinbarten Beweise. Eine Erfolgsmeldung
des Workers ist kein Abschlussbeleg. Klassifiziere einen Fehlschlag vor jedem Follow-up:

- **Ausführungsfehler:** Ein Tippfehler, falscher Pfad, Quoting oder ungeeigneter Flag hat die Methode noch
  nicht geprüft. Korrigiere ihn direkt und führe den beabsichtigten Schritt erneut aus. Bleibt derselbe
  Fehler nach der Korrektur bestehen, behandle ihn nicht durch Umformulieren als immer neuen Fehler.
- **Fehlende Voraussetzung:** Runtime, Werkzeug, Berechtigung, Ressource oder Nutzereingabe fehlen. Baue
  keine materiell andere Ersatzlösung und installiere nichts außerhalb des Laufvertrags. Sichere den Task
  sofort für `review` oder `waiting`; ein neuer Worker ist kein Lösungsversuch.
- **Lösungsfehler:** Der Ansatz wurde tatsächlich ausgeführt, erfüllt aber ein Abnahmekriterium nicht. Gib
  genau ein enges Follow-up, wenn eine konkrete Diagnose oder neue Evidenz eine bestimmte Korrektur trägt.
  Fehlt sie oder scheitert die Korrektur, ist das Fehlerbudget dieses Kriteriums ausgeschöpft.

Nach ausgeschöpftem Budget übernimmt kein Ersatz-Worker denselben Lösungsauftrag. Ein unabhängiger Checker
darf vorhandene Beweise prüfen, aber die Umsetzung nicht als neue Runde übernehmen. Sichere die Übergabe;
andere unabhängige Tasks dürfen weiterlaufen.

### 4. Integrieren und abschließen

Integriere einzeln in Abhängigkeitsreihenfolge und führe gemeinsame Prüfungen am Wellenende aus. Löse
fachlich offene Konflikte nicht automatisch. Pflege Task, Abschlussbericht und Spine nach dem maßgeblichen
Vertrag. Setze einen Task erst auf `done`, wenn alle Kriterien auf dem Steuerbranch belegt sind.

Muss der Nutzer entscheiden, prüfen oder handeln, setze den Task auf `review`. Halte die dort verlangte
aktuelle Übergabe fest. Ein schmutziger Worktree ist keine Übergabe.

Verhindert eine externe Voraussetzung, Ressource oder Umgebung die Fortsetzung, setze den Task nach dem
maßgeblichen Statusmodell auf `waiting`. Eine unerfüllte Abhängigkeit zu einem anderen Task verändert den
Status nicht; die Reihenfolge im Spine genügt.

Ist das Fehlerbudget ausgeschöpft, setze den Task auf `review`. Formuliere daraus eine konkrete Entscheidung
über Voraussetzung, Ansatzwechsel, Scope oder Zurückstellung. Erzeuge kein Versuchsprotokoll.

Die Wellenkarte ist im Normalverlauf die einzige sichtbare Zwischenmeldung. Nicht blockierende Fragen
kommen in die Review-Schlange, während unabhängige Arbeit weiterläuft.

## Stopbedingungen

Beende den Lauf, sobald eine dieser Bedingungen gilt:

- Der gewählte Scope ist vollständig und belegt abgeschlossen.
- Der gewählte `next`-Horizont ist ausgeschöpft; außerhalb davon verbleibendes `ready` wird nicht still
  nachgezogen.
- Es bleibt nur Arbeit mit unerfüllten Abhängigkeiten, `waiting` oder menschlicher Übergabe.
- Eine Scope-, Risiko-, Außenwirkungs- oder Berechtigungsgrenze ist erreicht.
- Der Steuerbranch wurde seit dem Preflight fremd verändert.
- Das Fehlerbudget eines verbleibenden Tasks ist ausgeschöpft.

Sichere vor dem Ende Spine, Abschlussberichte und erlaubte lokale Commits. Bei offenen Übergaben melde nur
deren Anzahl und verweise auf `callbell review`. Ohne Übergaben schließe mit Ergebnis und maßgeblichen
Beweisen knapp ab. Pushe nichts.
