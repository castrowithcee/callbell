---
name: callbell
description: >
  Steuert auf ausdrücklichen Aufruf Callbells täglichen Arbeitsloop: eine Idee oder Eingabe mit `shape`
  ausarbeiten und dokumentieren, den Arbeitsvorrat mit `backlog` disponieren, eine freigegebene Queue mit
  `run` autonom ausführen, menschliche Übergaben mit `review` klären oder gemeinsame Git-Worktrees mit
  `worktree` verwalten. Ohne Modus nur den nächsten sinnvollen Loop empfehlen. Niemals automatisch starten.
disable-model-invocation: true
argument-hint: "[shape|backlog|run|review|worktree] [context]"
license: MIT
type: skill
edit: locked
---

# Callbell

Callbell führt eine Absicht durch getrennte Schleifen bis zum belegten Ergebnis. Der Nutzer besitzt den
Start jeder Schleife. Ein Modus autorisiert nie still den nächsten.

Für `shape`, `backlog`, `run` und `review` bestimme zuerst das maßgebliche Planungssystem. Ist der lokale
Callbell-Backlog maßgeblich, lies vor der ersten Taskauswahl oder -änderung vollständig
`<callbell-core-root>/rules/references/backlog.md`. `<callbell-core-root>` ist der im Sessionkontext genannte
`CALLBELL PLUGIN ROOT`; ohne Hook leite ihn aus dem installierten `callbell-core`-Skillpfad ab. Kannst du den
Core nicht auflösen, schreibe nicht in den lokalen Backlog. Bei einem externen System gilt stattdessen nur
dessen Binding.

## Modus wählen

- **Kein Modus:** Lies nur den bereits geladenen Projektzustand, nenne knapp den nächsten sinnvollen Modus
  und ändere nichts. Fehlt ein Callbell-Scaffold, verweise auf `callbell-core`.
- **`shape [Idee oder Quelle]`:** Lies vollständig [Ideen ausarbeiten](references/shape.md). Aus Chat,
  Import oder bestehender Arbeit entstehen bestätigtes Projektwissen und ausführbare Arbeitspakete. Keine
  Umsetzung.
- **`backlog [Scope]`:** Lies vollständig [Backlog disponieren](references/backlog.md). Kläre Drafts,
  bestimme die nächste freigegebene Queue und prüfe ruhende Arbeit. Keine Umsetzung.
- **`run [Task, Projekt oder Backlog]`:** Lies vollständig [Arbeit ausführen](references/run.md). Dieser
  Modus verwendet die autonome Worker-, Checker-, Integrations- und Stoplogik. Lies vor dem ersten
  schreibenden Git-Schritt zusätzlich vollständig [den Git-Ablauf](references/git-workflow.md).
- **`review [Scope]`:** Lies vollständig [Übergaben klären](references/review.md). Kläre ausschließlich
  Entscheidungen, Prüfungen und Nutzerhandlungen. Beginne danach keine Ausführung.
- **`worktree`:** Lies vollständig [Git-Worktrees verwalten](references/worktree.md). Ohne weitere Angabe
  zeige nur die nummerierte Übersicht. `worktree new [Auftrag]` legt für den aktuellen Kontext einen
  Worktree an; jeder andere Rest ist eine natürliche Auswahl oder Aufräumanweisung. Namen und Pfade bestimmt
  immer der Agent.
Ist der genannte Modus nicht eindeutig, erkläre die verfügbaren Modi in je einem Satz und frage nach genau
einem. Deute eine normale Unterhaltung nie als Laufautorisierung.

## Gemeinsamer Vertrag

Nutzer- und Projektvorgaben bestimmen das Planungssystem. Der lokale Callbell-Backlog gilt nur ohne andere
Autorität. Spiegle ein externes System nie in lokale Tasks und behaupte keine dortige Änderung, wenn es
nicht erreichbar ist.

Nutze installierte Fach-Packs, wenn ihre Methode zum Gegenstand passt. Der gewählte Callbell-Modus behält
Eigentum an Gespräch, dauerhaftem Projektwissen, Spine, Status und Übergabe; ein Fach-Pack liefert Methode
und Prüfperspektive, keinen konkurrierenden Workflow.

Behandle Delegation nie als Retry-Strategie. Das Fehlerbudget gehört zum Task und Abnahmekriterium,
unabhängig von Hauptagent, Worker, Follow-up, Worktree oder Session. Ein anderer Agent und eine neue
Formulierung setzen es nicht zurück. Wiederhole eine gescheiterte Methode nur mit konkreter neuer Evidenz
und einer daraus abgeleiteten Änderung; sichere andernfalls den Stand und übergib ihn im gewählten Modus.

Jede Schleife endet mit ihrem eigenen Ergebnis:

- `shape` endet mit bestätigtem Wissen und `draft`- oder `ready`-Arbeit.
- `backlog` endet mit einem konsolidierten Arbeitsvorrat und einer geordneten `next`-Queue.
- `run` endet an seinem belegten Ziel oder einer definierten Stopbedingung.
- `review` endet nach den gewählten menschlichen Übergaben.

`worktree` ist keine Arbeitsschleife und startet keine Umsetzung. Der Modus verwaltet nur die gemeinsame
Isolation, die ein späterer oder bereits autorisierter Lauf verwenden kann.

Ein Übergang zu `run` braucht immer einen neuen ausdrücklichen Nutzeraufruf.
