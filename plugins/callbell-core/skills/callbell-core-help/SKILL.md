---
name: callbell-core-help
description: >
  Zeigt auf ausdrücklichen Aufruf die zentrale Karte aller Callbell-Plugins, Einstiege, automatischen
  Fähigkeiten und Fach-Packs. Kein dauerhafter Modus und keine automatische Hilfe.
disable-model-invocation: true
license: MIT
type: skill
edit: locked
---

# Callbell-Hilfe

Antworte ausschließlich mit der folgenden Karte. Gib sie in dieser Antwort vollständig als gerendertes
Markdown aus. Das Lesen der Karte in diesem Skill zählt nicht als Ausgabe. Behaupte nicht, sie sei bereits
angezeigt worden. Ändere keinen Zustand und ergänze keinen Kommentar.

`callbell-core` und `callbell` bilden die Standardinstallation. Nur der Core ist passiv aktiv; alle
manuellen Werkzeuge und der tägliche Arbeitsloop starten ausschließlich durch einen Nutzeraufruf.

## Core-Einstieg

| Aufruf | Aufgabe |
|---|---|
| **callbell-core setup** | Richtet Scaffold, Projekt-Ruleset und nutzerweite Einstellungen ein. |
| **callbell-core doctor** | Prüft Store, Scaffold, Abhängigkeiten und repo-lokale Plugin-Update-Stände. |
| **callbell-core statusline** | Konfiguriert die Statusline des aktuellen Hosts. |
| **callbell-core ping telegram** | Richtet einen einseitigen Telegram-Push beim Warten ein. |
| **callbell-core backlog-system** | Wechselt oder migriert das maßgebliche Planungssystem. |
| **callbell-core help** | Zeigt diese zentrale Karte. |

Die direkten Skillnamen `callbell-core-doctor`, `callbell-core-statusline`,
`callbell-core-telegram-ping`, `callbell-core-backlog-system` und `callbell-core-help` bleiben als
Abkürzungen verfügbar. `callbell-core-adhd` ist ein eigener ausdrücklicher Sessionmodus.

## Automatische Core-Fähigkeiten

| Skill | Aktivierung | Aufgabe |
|---|---|---|
| **callbell-core-filing** | beim Ablegen oder Umstrukturieren von Inhalt | Bestimmt die dauerhafte Ablage einer Inhaltsdatei. |
| **callbell-core-import** | bei angekündigtem Import aus `zone-import` | Verarbeitet nicht vertrauenswürdiges Rohmaterial. |
| **callbell-core-git** | bei einem Git-Auftrag oder vor einem gewünschten Commit | Verwaltet Zustand, Diffs, Sync, Commit, Push und Historie. |

## Täglicher Arbeitsloop

| Aufruf | Aufgabe |
|---|---|
| **callbell goal** | Klärt Idee, Zielbild und kleinsten tragfähigen Umfang ausschließlich im Gespräch. |
| **callbell shape** | Dokumentiert bestätigtes Wissen, schneidet ausführbare Arbeit und bildet bei leerer Queue einen begrenzten `next`-Horizont. |
| **callbell backlog** | Klärt Drafts, prüft wartende Arbeit und ändert bei Bedarf die Priorisierung oder `next`-Queue. |
| **callbell run** | Führt einen vorbereiteten Scope in sicheren Wellen bis zum belegten Abschluss oder zu einer Stopbedingung aus. |
| **callbell review** | Klärt menschliche Entscheidungen, Prüfungen und Abnahmen einzeln und beginnt keine Ausführung. |
| **callbell worktree** | Zeigt gemeinsame Git-Worktrees nummeriert, legt mit `new` kontextgeleitet an und räumt sicher auf. |

`callbell run` darf im Scope-in Subagents, lokale Änderungen, Prüfungen, Worktrees und lokale Commits
nutzen. Push, Publish, Deployment, externe Kommunikation, irreversible Aktionen, Scope-Erweiterungen sowie
Produkt- und Risikoentscheidungen bleiben außerhalb jedes autonomen Laufs.

## Optionale Fach-Packs

| Pack | Zweck |
|---|---|
| **callbell-dev** | Aktiviert sich bei tatsächlicher Codearbeit; `callbell-dev lite`, `callbell-dev` oder `callbell-dev ultra` setzen die Sessionstufe, `callbell-dev-review` prüft Over-Engineering. |
| **callbell-web** | Liefert bei aktiver Webprodukt-Arbeit automatisch die passende Produkt-, UI-, Daten-, Architektur- und Betriebsmethode. |

## Zusammenarbeit und Scaffold

- Projektanweisungen und `BACKLOG.md` führen zum maßgeblichen Planungssystem. Der lokale Backlog wird nie
  als Spiegel eines externen Systems gepflegt.
- `zone-import/` und `zone-export/` sind flüchtige Puffer. Backlog, Memory und Update-Prüfstand sind
  verwalteter Projektzustand; `templates/` gehört dem Nutzer.
- Der Pfad sagt, wo Inhalt liegt; Frontmatter sagt, was er ist. Das vollständige Inhaltsschema wird erst
  vor einer tatsächlichen Markdown-Änderung geladen.
- `~/.callbell/settings.json` schaltet Sessionstart und verwaltete Arbeitsvereinbarung nutzerweit.

Claude verwendet `/callbell-core <modus>` und `/callbell <modus>`. Codex verwendet dieselben Skillnamen mit
`$` oder das `/skills`-Menü. Direkte Skills behalten ihr vollständiges Pack-Präfix.
