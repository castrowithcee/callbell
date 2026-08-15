---
description: >
  Interaktive Disposition des maßgeblichen Backlogs von unreifen Paketen über die freigegebene Queue bis
  zu menschlichen oder extern wartenden Aufgaben.
type: playbook
edit: locked
license: MIT
---

# Backlog disponieren

Aus einem vorhandenen Arbeitsvorrat entsteht eine verständliche, ausführbare und bewusst geordnete Queue.
Dieser Modus verändert Planung und Taskverträge, führt aber keine fachliche Arbeit aus.

## Bestand bilden

1. Bestimme den ausdrücklich gewählten Scope und das maßgebliche Planungssystem.
2. Lies zuerst nur Roster beziehungsweise externe Metadaten: Status, Kurzstand, Reihenfolge,
   Abhängigkeiten, Eigentümerschaft und vorhandene Wiedervorlagen.
3. Bilde daraus getrennte Kandidatenmengen für `review`, `draft`, `waiting`, `ready` und `next`.
   Lies nur die Tasks vollständig, die in dieser Sitzung wirklich disponiert werden.
4. Verändere keinen `in-progress`-Task mit laufendem oder unbekanntem Worker.

Bei großen Beständen arbeite in sinnvollen Ausschnitten. Es gibt keine harte Mengenbegrenzung; vollständiges
Lesen aller Task-Dateien ist dennoch kein Review-Verfahren.

## Disponieren

Arbeite in dieser Reihenfolge, soweit der gewählte Scope die Gruppe enthält:

1. **Menschliche Gates sichtbar machen.** Verweise für echte `review`-Übergaben auf `callbell review`; kläre
   sie in diesem Modus nur, wenn der Nutzer das ausdrücklich in denselben Backlog-Aufruf einbezieht.
2. **Drafts reifen.** Stelle nur entscheidungsrelevante Fragen. Arbeite Antworten am fachlich passenden Ort
   in den aktuellen Taskvertrag ein und entferne überholte Varianten. Setze ihn nach dem Statusmodell des
   maßgeblichen Planungssystems erst auf `ready`, wenn keine bekannte Vertragsfrage bleibt.
3. **Ruhende Arbeit prüfen.** Hebe `waiting` nur auf, wenn das Wiederaufnahmesignal belegt eingetreten ist.
4. **Queue bestimmen.** Empfiehl aus `ready` eine fachlich begründete Auswahl und ihre Reihenfolge. Verschiebe
   sie erst nach Nutzerbestätigung nach `next`. Eine Aufgabenmenge ist `next`, wenn vor ihrer Ausführung
   keine weitere Priorisierungsentscheidung nötig ist.
5. **Bestehende Queue konsolidieren.** Entferne Doppelungen, löse widersprüchliche Reihenfolgen und prüfe
   Abhängigkeiten, ohne ausführbare Taskverträge unnötig umzuschreiben.

## Grenzen

- Erzeuge keine künstlichen Tasks nur, um eine ferne Idee vollständig zu nummerieren. Materialisiere
  eigenständige Pakete, wenn ihr Vertrag verstanden ist; halte spätere Horizonte kompakt im Projektwissen.
- Setze nichts auf `in-progress` oder `done` und beginne keine Implementierung.
- Eine bloße Empfehlung setzt keinen Task auf `next`.
- Bewahre keine Review- oder Planungshistorie im Task. Er bleibt der konsolidierte aktuelle Stand.

Beende mit der Anzahl der weiterhin `draft`, `waiting`, `review` und neu oder weiterhin `next`
liegenden Tasks sowie dem ersten ausführbaren Scope. Starte ihn nicht.
