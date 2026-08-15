---
name: callbell-core-doctor
description: >
  Nur ausdrücklich aufgerufene, read-only Diagnose eines Callbell-Projekts: prüft Abhängigkeiten,
  Store, Scaffold, Ruleset und den repo-lokalen Update-Stand des Cores. Verwenden bei callbell-core-doctor oder
  wenn der Nutzer den Callbell-Zustand gezielt untersuchen lassen will; repariert und bestätigt nichts von
  selbst.
disable-model-invocation: true
license: MIT
type: skill
edit: locked
---

# callbell-core-doctor

Diagnostiziere den aktuellen Ordner, ohne ihn zu verändern. Der Nutzer besitzt das Timing dieses Skills.

`<plugin-root>` ist der Ordner, aus dem dieser Skill geladen wurde. Der Session-Kontext nennt ihn als
`CALLBELL PLUGIN ROOT`; andernfalls liegt er zwei Ebenen über dieser `SKILL.md`. Verwende keinen festen Pfad.

## Prüfen

Führe beide Befehle ohne `--apply` und ohne `ack` aus:

```sh
node <plugin-root>/scripts/callbell-core-doctor.js
node <plugin-root>/scripts/callbell-update.js status
```

Der erste Befehl prüft Node, Git, Git-Identität, optionale Git-LFS-Unterstützung, nutzerweiten Store,
Scaffold, `.gitignore` und Ruleset. Der zweite vergleicht den für dieses Repo gespeicherten Core-Prüfstand
mit der installierten Core-Version. Die optionalen Packs prüfen ihre eigene Version jeweils über ihren
SessionStart-Hook; leite aus dem Core-Befehl keinen Gesamtstand aller Plugins ab.

Sind Update-Anweisungen offen, lies nur die ausgegebenen Dateien und prüfe ihre Punkte gegen das aktuelle
Repo. Vorhandene Repo-Dateien sind primär. Berichte, ob und was anwendbar ist, aber bestätige den Prüfstand
nicht im Diagnose-Lauf. Ist etwas anwendbar, nenne pro Punkt knapp den konkreten Befund, die vorgeschlagene
Änderung und ihre praktische Folge oder ihren Grund. Eine bloße Liste aus Dateinamen, Mengen oder
Schlagwörtern reicht nicht. Erst wenn der Nutzer danach ausdrücklich über vollständige, teilweise oder keine
Übernahme entscheidet, darfst du Änderungen und `ack` ausführen.

## Berichten

Fasse Befunde nach Priorität zusammen: Blocker, fehlende Projektbestandteile, Hinweise und Update-Stand.
Erfinde keine Reparatur und melde nicht pauschal Drift, nur weil Plugin-Vorlage und vorhandene Repo-Datei
inhaltlich verschieden sind.

Will der Nutzer Fehlendes ergänzen, verwende anschließend `callbell-core setup` oder führe den Doctor nach
der Freigabe mit `--apply` aus. Beide Verfahren kopieren nur fehlende Dateien und ergänzen `.gitignore`.
Bestätige Update-Stände niemals als Reparatur eines ungeprüften Befunds.
