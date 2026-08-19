# Callbell

Callbell klärt neue Ziele und führt Ideen sowie vorhandene Arbeit durch wiederholbare Schleifen bis zum
belegten Ergebnis. Eine dauerhaft aktive Infrastruktur hält Kontext, Normen und Projektzustand zusammen.
Core und täglicher Arbeitsloop bilden die Grundausstattung; fachliche Methoden bleiben bewusst
installierbare Packs.

## Das Modell

```text
Nutzerprompt + AGENTS.md + SessionStart
                  ↓
          callbell-core
   Kontext, Regeln, Scaffold, Spine
                  ↓
             callbell
       Goal? → Shape → Run ↔ Review
                  ↕
               Backlog
                  ↓
         fachliche Packs
       Dev · Web · weitere
```

| Baustein | Aufgabe | Aktivierung |
|---|---|---|
| `callbell-core` | Passive Infrastruktur, Projektzustand und sichere Verwaltungswerkzeuge | standardmäßig aktiv |
| `callbell` | Täglicher Arbeitsloop von der Idee bis zur autonomen Ausführung | standardmäßig installiert, bewusst aufgerufen |
| Fach-Packs | Methode für Code, Server, Webprodukte und weitere Domänen | bewusst installiert, bei Relevanz geladen |
| `__callbell__/` | Backlog-Binding, Memory, Zonen und Update-Stand des Projekts | per `callbell-core` |

Nur der Core läuft passiv. Der tägliche Arbeitsloop ist als Grundausstattung installiert, beginnt aber wie
alle Schleifen und systemverändernden Werkzeuge nur durch einen Nutzeraufruf. Fach-Packs liefern Methode
und Prüfperspektive; sie besitzen keinen konkurrierenden Backlog.

## Schnellstart

Callbell benötigt Node im `PATH`. Das gilt auch für nicht interaktive Shells, wie sie etwa bei Nix oder nvm
entstehen können.

### Claude Code

```text
claude plugin marketplace add castrowithcee/callbell
claude plugin install callbell@callbell
```

`callbell` installiert und aktiviert `callbell-core` als Abhängigkeit. Der nächste Sessionstart lädt den
Callbell-Kontext automatisch.

### Codex

```text
codex plugin marketplace add castrowithcee/callbell
```

Der Marketplace markiert `callbell-core` und `callbell` als standardmäßig installiert.

Öffne anschließend unter Codex `/hooks`, prüfe die Callbell-Core-Handler und genehmige sie. Codex führt
Plugin-Hooks erst nach dieser Trust-Freigabe aus; nach einer geänderten Hook-Definition kann eine erneute
Freigabe nötig sein. Ohne sie bleiben die Skills verfügbar, aber SessionStart-Normen, Projektkontext und
Update-Prüfungen fehlen.

Richte danach den gewünschten Arbeitsordner ein:

```text
Claude: /callbell-core setup
Codex:  $callbell-core setup
```

Der ausschließlich vom Nutzer gestartete Core-Einstieg prüft Node, ergänzt nur Fehlendes, konkretisiert
eine neu angelegte minimale `AGENTS.md` aus dem vorhandenen Repo und fragt bei Bedarf, ob Git initialisiert
werden soll. In einem bereits eingerichteten Repo bleibt sein Bericht bei einer Zeile.

## Der tägliche Arbeitsloop

Callbell besitzt einen einzigen Nutzer-Einstieg mit lazy geladenen Modi:

| Aufruf | Ergebnis |
|---|---|
| `callbell goal` | Klärt Idee, Vision, MVP oder nächsten Produktzustand ausschließlich im Gespräch und ohne dauerhafte Änderung. |
| `callbell shape` | Reift Chat, Idee oder Import zu Projektwissen und Arbeitspaketen und bildet bei leerer Queue bis zu fünf sinnvolle `next`-Tasks. |
| `callbell backlog` | Klärt Drafts, prüft ruhende Arbeit und ändert bei Bedarf Priorisierung oder `next`-Queue. |
| `callbell run` | Führt den gewählten vorbereiteten Scope mit sicheren Worker- und Checker-Wellen bis zum Abschluss oder einer Stopbedingung aus. |
| `callbell review` | Klärt menschliche Entscheidungen, Prüfungen und Handlungen einzeln, ohne die Ausführung fortzusetzen. |
| `callbell worktree` | Zeigt gemeinsame Git-Worktrees nummeriert; `new` legt einen für den aktuellen Arbeitskontext an. |

Ohne Modus liest Callbell nur den bereits geladenen Projektzustand und empfiehlt die nächste sinnvolle
Schleife. Es beginnt keine Arbeit. `worktree` verwaltet nur Isolation und ist selbst keine Arbeitsschleife.
`goal` ist eine optionale Vorstufe; ein direkter Einstieg mit `shape` bleibt der Normalfall. `goal` schreibt
weder Dateien noch Tasks und darf nicht selbst zu `shape` wechseln. Der Nutzer startet `shape` erst mit
einem neuen ausdrücklichen Aufruf. Auch ein Wechsel zu `run` braucht immer einen neuen Aufruf.
`backlog` ist keine Pflichtschleuse zwischen `shape` und `run`; der Modus dient der bewussten Änderung eines
vorhandenen Arbeitsvorrats oder Ausführungshorizonts.

### Statusmodell

Der normale Arbeitsfluss lautet:

```text
draft → ready → next → in-progress → done
```

- `draft`: Der Taskvertrag hat eine konkrete offene Frage.
- `ready`: Eigenständig ausführbar, aber nicht Teil des nächsten Ausführungshorizonts.
- `next`: Für den kommenden Ausführungshorizont disponiert und im Roster geordnet. Bei leerer Queue wählt
  `shape` automatisch höchstens fünf Tasks seines Scopes und überführt ihren Status von `ready` nach
  `next`; der Nutzer darf die Queue ändern oder erweitern.
- `in-progress`: Von einer laufenden Ausführung beansprucht.
- `review`: Wartet auf eine konkrete Entscheidung, Prüfung oder Abnahme des Nutzers.
- `waiting`: Wartet auf eine externe Voraussetzung mit erkennbarem Wiederaufnahmesignal.
- `done`: Beobachtbar abgeschlossen.

`review` und `waiting` sind Abzweigungen, keine Pflichtstationen. Eine interne Task-Abhängigkeit allein
macht einen Task nicht zu `waiting`; Reihenfolge und Abhängigkeiten bleiben im Roster. Bewusst später
eingeplante Arbeit bleibt `ready` und wird nicht nach `next` übernommen.
Eine bestehende `next`-Queue wird von einem späteren `shape` nicht still verändert. `next` bestimmt weder
Parallelität noch Ausführungsautorisierung; erst ein ausdrückliches `callbell run` startet die Arbeit.
Jeder Task trägt genau einen Status; `next` ist kein zusätzliches Queue-Feld neben `ready`.

### Autonome Ausführung

`callbell run` erhält vor der ersten Änderung einen vollständigen Harness aus Ziel, Scope-in, Scope-out,
Spine, Beweisen und Grenzen. Der Aufruf autorisiert im gewählten Scope Subagents, lokale Änderungen,
Prüfungen, isolierte Worktrees und lokale Commits. Er autorisiert keinen Push, Publish, kein Deployment,
keine Nachricht an Dritte und keine sonstige externe oder irreversible Wirkung.

Der Hauptagent disponiert immer nur die nächste Welle, gibt jedem Worker ein enges Paket und prüft dessen
Rückgabe gegen echten Diff und Beweise. Bei mittlerem oder hohem Risiko kann ein unabhängiger Checker
hinzukommen. Menschliche Entscheidungen werden gesichert nach `review` übergeben; externe Blockaden nach
`waiting`. Unabhängige Arbeit darf bis zu einer definierten Stopbedingung weiterlaufen.

## SessionStart-Kontext

Der Core schreibt seine Pluginregeln nicht in `CLAUDE.md` oder `AGENTS.md`. Ein gemeinsames Hook-Script
injiziert unabhängige Blöcke; ihre Reihenfolge ist unerheblich:

- `CALLBELL.md` und `FILES.md` gelten in jedem Ordner.
- `FRONTMATTER.md` enthält nur die immer nötigen Such-, Schutz- und Ladeinvarianten. Das vollständige
  Inhaltsschema wird erst vor einer tatsächlichen Markdown-Änderung aus dem Core-Store gelesen.
- Die schaltbare Arbeitsvereinbarung kommt aus `~/.callbell/rules/RULESET.md`.
- Bei vorhandenem `__callbell__/` kommen `SCAFFOLD.md`, `BACKLOG.md` sowie die Projektindizes für Memory und
  Backlog hinzu.
- `IDEAS.md` wird nur zum Erfassen, Sichten oder Ausarbeiten von Ideen geöffnet.
- Ein eigener Handler meldet relevante projektbezogene Plugin-Updates.

Jeder Regelblock nennt seine versionsgebundene Quelle. Callbell begrenzt einen einzelnen Block auf 9.000
Zeichen; mehrere kleine Handler sind dennoch kein Freibrief für unnötigen Sessionkontext.

### Nutzerweite Schaltung

`callbell-core` legt `~/.callbell/settings.json` an. Ohne abweichende Einstellung gelten:

```json
{
  "format": 1,
  "sessionStart": {
    "enabled": true,
    "ruleset": true
  },
  "mute": []
}
```

`sessionStart.enabled: false` unterdrückt SessionStart-Kontext und Update-Hinweise.
`sessionStart.ruleset: false` schaltet nur die zusätzliche Arbeitsvereinbarung ab. Eigene dauerhafte
Entscheidungen gehören in die globale oder projektlokale Agentendatei, nicht in die von Callbell verwaltete
Kopie unter `~/.callbell/rules/`.

## Ambient- und Projektmodus

Im Ambient-Modus stehen Core-Regeln und Skills in jedem Ordner bereit, ohne Dateien anzulegen. Der
Projektmodus entsteht erst durch `callbell-core` und bündelt unter `__callbell__/`:

```text
__callbell__/
├── backlog/       lokaler Backlog oder Binding an genau ein externes System
├── memory/        dauerhaftes repo-gebundenes Agenten-Memory
├── docs/          kompaktes Projektwissen für den Agenten
├── templates/     optionale Vorlagenbibliothek des Nutzers
├── updates/       zuletzt geprüfter Plugin-Stand
├── zone-import/   flüchtige Eingaben für den Agenten
└── zone-export/   ausdrücklich angeforderte Ergebnisse
```

Die beiden Zonen bleiben unversioniert. Ein externes Planungssystem wird nie in einen lokalen Backlog
gespiegelt. `BACKLOG.md` bleibt der dauerhaft geladene Wegweiser zur einzigen operativen Autorität.

## Installierbare Plugins

### `callbell-core`

Der Core liefert SessionStart, Regeln und Scaffold. `callbell-core` bündelt `setup`, `doctor`, `statusline`,
`ping telegram`, `backlog-system` und die zentrale Hilfe. Filing, Import und Git werden bei
passendem Gegenstand automatisch gewählt; die vollständigen direkten Skillnamen bleiben verfügbar.

### `callbell`

Der standardmäßig installierte tägliche Arbeitsloop mit optionaler Zielklärung, Shape, Backlog, Run und
Review. `callbell worktree` zeigt die gemeinsamen Git-Worktrees nummeriert und legt mit `new` einen zentralen
Worktree für den aktuellen Arbeitskontext an. Das Plugin aktiviert unter Claude `callbell-core` als
Abhängigkeit.

### `callbell-dev`

Das Pack für tatsächliche Codearbeit. Es bevorzugt die einfachste vollständig funktionierende Lösung und
bietet ein ausdrückliches Review auf unnötige Komplexität.

```text
Claude: claude plugin install callbell-dev@callbell
Codex:  codex plugin add callbell-dev@callbell
```

### `callbell-web`

Die fachliche Capability für Websites, Web-Apps und SaaS-Produkte. Sie ergänzt aktive Callbell-Loops um
Produktprofil, Fähigkeiten, UI, Daten- und Berechtigungsgrenzen, Stack, Architektur, Sicherheit und Betrieb.
Sie besitzt keinen eigenen Intake, Backlog oder Task-Workflow.

```text
Claude: claude plugin install callbell-web@callbell
Codex:  codex plugin add callbell-web@callbell
```

## Skills verwenden

- Claude ruft einen Skill mit `/` auf, beispielsweise `/callbell run` oder `/callbell-core doctor`.
- Codex verwendet `$`, beispielsweise `$callbell run`, oder das `/skills`-Menü.
- Explizite Skills starten nie automatisch. Passive Skills wie Filing, Import, Git und Fach-Capabilities
  werden nur bei passendem Gegenstand gewählt.

`callbell-core help` zeigt die zentrale Karte für Core, täglichen Arbeitsloop und alle Fach-Packs. Der
direkte Alias `callbell-core-help` bleibt verfügbar; einzelne Packs führen keine eigene Hilfe mehr.

Skill-Details werden erst nach der Auswahl geladen. Callbell bündelt seine täglichen Schleifen deshalb in
einer Beschreibung und hält ihre vollständigen Verfahren als bedingte Referenzen.

## Projektbezogene Plugin-Updates

Jedes Plugin mit projektbezogenen Update-Anweisungen prüft nur seinen eigenen Stand gegen
`__callbell__/updates/state.json`. Eine Veröffentlichung enthält nur dann eine Projektanweisung, wenn
bestehende Repos tatsächlich etwas prüfen oder zusammenführen müssen. Reine Runtime-, Dokumentations- oder
Skill-Änderungen bleiben still.

Bei einer relevanten Änderung gleicht der Agent die Anweisung mit dem geöffneten Repo ab. Nicht anwendbare
Punkte werden ohne Rückfrage als geprüft markiert. Echte Änderungen erklärt er mit ihren konkreten Folgen
und holt die Entscheidung des Nutzers ein.

## Sprache

Ausgelieferter Callbell-Text ist Deutsch. Im Deutschen etablierte englische Fachbegriffe bleiben stehen,
wenn eine Übersetzung künstlich oder ungenauer wäre.

## Lizenz

MIT. Drittanbieterhinweise stehen beim jeweiligen Plugin.
