# Callbell

Callbell hält Kontext, Regeln und Projektzustand zusammen und führt Ideen sowie vorhandene Arbeit durch
ausdrücklich gestartete Schleifen bis zum belegten Ergebnis. Die gemeinsame Infrastruktur und der
Arbeitsloop leben in einem Plugin; fachliche Methoden bleiben bewusst installierbare Packs.

## Das Modell

```text
Nutzerprompt + AGENTS.md + SessionStart
                  ↓
              callbell
      Kontext · Regeln · Scaffold
      Setup · Goal · Shape · Backlog
      Run · Review · Worktree
                  ↓
         fachliche Packs
             Dev · Web
```

| Baustein | Aufgabe | Aktivierung |
|---|---|---|
| `callbell` | Passive Infrastruktur, Projektzustand, Verwaltungswerkzeuge und täglicher Arbeitsloop | standardmäßig installiert; Schleifen und Verwaltungswerkzeuge nur ausdrücklich, Nutzertool-Operationen nur unter separater Autorität |
| Fach-Packs | Methode für Code, Webprodukte und weitere Domänen | bewusst installiert, bei Relevanz geladen |
| `__callbell__/` | Backlog-Binding, Memory, Zonen und Update-Stand des Projekts | per `callbell setup` |

Sessionkontext und Regeln wirken passiv. Einrichtung, systemverändernde Callbell-Verwaltungswerkzeuge und
der tägliche Arbeitsloop beginnen ausschließlich durch einen Nutzeraufruf. Operationen über die Callbell
CLI oder ihren MCP-Broker brauchen eine separate eindeutige Autorität aus Nutzerauftrag, Taskvertrag oder
geltender Agentendatei. Fach-Packs liefern Methode und Prüfperspektive; sie besitzen keinen
konkurrierenden Backlog.

## Schnellstart

Callbell benötigt Node im `PATH`. Das gilt auch für nicht interaktive Shells, wie sie etwa bei Nix oder nvm
entstehen können.

### Claude Code

```text
claude plugin marketplace add castrowithcee/callbell
claude plugin install callbell@callbell
```

### Codex

```text
codex plugin marketplace add castrowithcee/callbell
```

Der Marketplace installiert `callbell` standardmäßig. Öffne anschließend unter Codex `/hooks`, prüfe die
Callbell-Handler und genehmige sie. Codex führt Plugin-Hooks erst nach dieser Trust-Freigabe aus; nach einer
geänderten Hook-Definition kann eine erneute Freigabe nötig sein. Ohne sie bleiben die Skills verfügbar,
aber SessionStart-Normen, Projektkontext und Update-Prüfungen fehlen.

Richte danach den gewünschten Arbeitsordner ein:

```text
Claude: /callbell setup
Codex:  $callbell setup
```

Der ausschließlich vom Nutzer gestartete Einstieg prüft Node, ergänzt nur Fehlendes, konkretisiert eine
neu angelegte minimale `AGENTS.md` aus dem vorhandenen Repo und fragt bei Bedarf, ob Git initialisiert
werden soll. In einem bereits eingerichteten Repo bleibt sein Bericht bei einer Zeile.

## Der Arbeitsloop

Callbell besitzt einen Nutzer-Einstieg mit lazy geladenen Modi:

| Aufruf | Ergebnis |
|---|---|
| `callbell goal` | Klärt Idee, Vision, MVP oder nächsten Produktzustand ausschließlich im Gespräch und ohne dauerhafte Änderung. |
| `callbell shape` | Reift Chat, Idee oder Import zu Projektwissen und Arbeitspaketen und bildet bei leerem Horizont bis zu fünf sinnvolle `next`-Tasks. |
| `callbell backlog` | Bespricht ohne Scope alle Drafts einzeln bis `ready`, prüft ruhende Arbeit und bildet einen sinnvollen `next`-Horizont. |
| `callbell run` | Führt höchstens fünf ausführbare Tasks seriell aus; ein Orchestrator steuert und Subagents setzen jeweils den aktiven Task um. |
| `callbell review` | Klärt menschliche Entscheidungen, Prüfungen und Handlungen einzeln, ohne die Ausführung fortzusetzen. |
| `callbell worktree` | Zeigt gemeinsame Git-Worktrees nummeriert; `new` legt einen für den aktuellen Arbeitskontext an. |

Ohne Modus liest Callbell nur den bereits geladenen Projektzustand und empfiehlt den nächsten sinnvollen
Einstieg. Es beginnt keine Arbeit. `goal` ist optional; dauerhafte Ausarbeitung und jede Ausführung brauchen
einen eigenen ausdrücklichen Aufruf. `backlog` ist keine Pflichtschleuse, sondern dient der bewussten Pflege
des Arbeitsvorrats.

### Statusmodell

Der normale Arbeitsfluss lautet:

```text
draft → ready → next → in-progress → done
```

- `draft`: Der Taskvertrag hat eine konkrete offene Frage.
- `ready`: Ohne bekannte Vertragsfrage eigenständig ausführbar.
- `next`: Ebenfalls vollständig ausführbar und zusätzlich für den kommenden Horizont disponiert.
- `in-progress`: Von der laufenden Ausführung beansprucht.
- `review`: Wartet auf eine konkrete Entscheidung, Prüfung oder Abnahme des Nutzers.
- `waiting`: Wartet auf eine externe Voraussetzung mit erkennbarem Wiederaufnahmesignal.
- `done`: Beobachtbar abgeschlossen.

`review` und `waiting` sind Abzweigungen, keine Pflichtstationen. Eine interne Task-Abhängigkeit allein
macht einen Task nicht zu `waiting`; Reihenfolge und Abhängigkeiten bleiben im Roster. Jeder Task trägt
genau einen Status: `next` ist kein zusätzliches Feld neben `ready`, impliziert aber dieselbe vollständige
Ausführungsreife.

Eine bestehende `next`-Menge bleibt in Nutzerreihenfolge erhalten und darf beliebig groß sein. Ihre Größe
bestimmt weder Parallelität noch den Umfang eines Laufs. Ein Run nimmt zuerst `next`; bleiben von seinen
höchstens fünf Plätzen welche frei, wählt er selbstständig sinnvolle `ready`-Tasks desselben Scopes. Dadurch
ist ein leerer `next`-Horizont kein Blocker und keine vorgeschaltete Backlog-Sitzung nötig.

### Serielle Ausführung

Der initiale Agent eines Runs ist der Orchestrator. Er hält Ziel, Scope, Spine, Taskauswahl, Subagents,
Beweise, Integration und Stopbedingungen. Die Subagents setzen die fachliche Arbeit um.

Es ist immer höchstens ein Task `in-progress`. Braucht dieser eine legitime Trennung nach Rollen oder
Zielbereichen, darf der Orchestrator mehrere Subagents für denselben Task einsetzen. Ein zweiter Task beginnt
erst nach Abschluss oder gesicherter Übergabe des aktiven Tasks. Nach jedem Task werden Abhängigkeiten und
Reihenfolge neu bewertet.

Ein initialer Umsetzungsversuch darf nach einem belegten Lösungsfehler höchstens zweimal gezielt korrigiert
werden. Jede Korrektur braucht neue Evidenz; ein anderer Subagent oder eine neue Formulierung setzt das
Budget nicht zurück. Scheitert auch die zweite Korrektur, stoppt die Aufgabe, hält Ursache und Restbefund
kompakt fest und wechselt nach `review`. Blockiert sie weitere Tasks, endet der Lauf und der Nutzer
entscheidet das weitere Vorgehen.

Der Run-Aufruf allein autorisiert im gewählten Scope Subagents, lokale Änderungen, Prüfungen, isolierte
Worktrees und lokale Commits. Er autorisiert keinen Push, Publish, kein Deployment, keine Nachricht an
Dritte und keine sonstige externe oder irreversible Wirkung. Eine konkrete Callbell-Tooloperation ist nur
unter der separat und eindeutig festgelegten Autorität aus Auftrag, Taskvertrag oder geltender
Agentendatei zulässig. Eine Abschlussmeldung sendet ausschließlich der primäre Orchestrator.

## SessionStart-Kontext

Callbell schreibt seine Pluginregeln nicht in `CLAUDE.md` oder `AGENTS.md`. Ein gemeinsames Hook-Script
injiziert unabhängige Blöcke; ihre Reihenfolge ist unerheblich:

- `CALLBELL.md` und `FILES.md` gelten in jedem Ordner.
- `FRONTMATTER.md` enthält nur die immer nötigen Such-, Schutz- und Ladeinvarianten. Das vollständige
  Inhaltsschema wird erst vor einer tatsächlichen Markdown-Änderung aus der bedingten Referenz gelesen.
- Die schaltbare Arbeitsvereinbarung kommt aus `~/.callbell/rules/RULESET.md`.
- Bei vorhandenem `__callbell__/` kommen `SCAFFOLD.md`, `BACKLOG.md` sowie die Projektindizes für Memory und
  Backlog hinzu.
- `IDEAS.md` wird nur zum Erfassen, Sichten oder Ausarbeiten von Ideen geöffnet.
- Ein eigener Handler meldet relevante projektbezogene Plugin-Updates.

Jeder Regelblock nennt seine versionsgebundene Quelle. Callbell begrenzt einen einzelnen Block auf 9.000
Zeichen; mehrere kleine Handler sind dennoch kein Freibrief für unnötigen Sessionkontext.

### Nutzerweite Schaltung

`callbell setup` legt `~/.callbell/settings.json` an. Ohne abweichende Einstellung gelten:

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

Im Ambient-Modus stehen Callbell-Regeln und Skills in jedem Ordner bereit, ohne Dateien anzulegen. Der
Projektmodus entsteht erst durch `callbell setup` und bündelt unter `__callbell__/`:

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

## Fähigkeiten und Fach-Packs

`callbell` bündelt Setup und den Arbeitsloop. `callbell-core` routet ausdrücklich gestartete Diagnose,
Statusline, Telegram-Ping und Planungssystemwechsel; `callbell-mode` hält sessionweite Zusammenarbeitsmodi
wie `adhd`. `callbell-help` bleibt der direkte Einzweck-Einstieg zur Übersicht. Filing, Import und Git
werden bei passendem Gegenstand automatisch als `callbell-core-filing`, `callbell-core-import` und
`callbell-core-git` gewählt. `callbell-core-cli-mcp` ist immer der automatische Einstieg für Installation,
Nutzung und Diagnose der Callbell CLI oder ihres MCP-Brokers sowie für ausdrücklich autorisierte
Operationen über konfigurierte Nutzertools.

`callbell-dev` ist das Pack für tatsächliche Codearbeit. `callbell-web` ergänzt aktive Webprodukt-Arbeit um
Produktprofil, Fähigkeiten, UI, Daten- und Berechtigungsgrenzen, Stack, Architektur, Sicherheit und Betrieb.
Beide benötigen `callbell` und besitzen keinen eigenen Intake, Backlog oder Task-Workflow.

```text
Claude: claude plugin install callbell-dev@callbell
Claude: claude plugin install callbell-web@callbell
Codex:  codex plugin add callbell-dev@callbell
Codex:  codex plugin add callbell-web@callbell
```

Claude ruft einen Skill mit `/` auf, beispielsweise `/callbell run`, `/callbell-core doctor` oder
`/callbell-mode adhd`. Codex verwendet `$`, beispielsweise `$callbell run`, oder das `/skills`-Menü.
Explizite Skills starten nie automatisch.

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
