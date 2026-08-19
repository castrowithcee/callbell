---
name: callbell-help
description: >
  Zeigt auf ausdrücklichen Aufruf die zentrale Karte des Callbell-Plugins, seiner Einstiege, automatischen
  Fähigkeiten und optionalen Fach-Packs. Kein dauerhafter Modus und keine automatische Hilfe.
disable-model-invocation: true
license: MIT
type: skill
edit: locked
---

# Callbell-Hilfe

Antworte ausschließlich mit der folgenden Karte. Gib sie in dieser Antwort vollständig als gerendertes
Markdown aus. Das Lesen der Karte in diesem Skill zählt nicht als Ausgabe. Behaupte nicht, sie sei bereits
angezeigt worden. Ändere keinen Zustand und ergänze keinen Kommentar.

Callbell ist die Standardinstallation. Seine Regeln und der Sessionkontext wirken passiv; Einrichtung,
Verwaltungswerkzeuge und Arbeitsloop starten ausschließlich durch einen Nutzeraufruf.

## Callbell-Einstieg

| Aufruf | Aufgabe |
|---|---|
| **callbell setup** | Richtet Scaffold, Projekt-Ruleset und nutzerweite Einstellungen ein. |
| **callbell goal** | Klärt Idee, Zielbild und kleinsten tragfähigen Umfang ausschließlich im Gespräch. |
| **callbell shape** | Dokumentiert bestätigtes Wissen, schneidet ausführbare Arbeit und bildet bei leerem Horizont bis zu fünf `next`-Tasks. |
| **callbell backlog** | Klärt Drafts, prüft wartende Arbeit und ändert bei Bedarf Priorisierung oder `next`-Horizont. |
| **callbell run** | Führt höchstens fünf Tasks seriell aus; der Orchestrator steuert und Subagents setzen jeweils den aktiven Task um. |
| **callbell review** | Klärt menschliche Entscheidungen, Prüfungen und Abnahmen einzeln und beginnt keine Ausführung. |
| **callbell worktree** | Zeigt gemeinsame Git-Worktrees nummeriert, legt mit `new` kontextgeleitet an und räumt sicher auf. |
| **callbell-core doctor** | Prüft Store, Scaffold, Abhängigkeiten und den repo-lokalen Plugin-Update-Stand. |
| **callbell-core statusline** | Konfiguriert die Statusline des aktuellen Hosts. |
| **callbell-core ping** oder **callbell-core ping telegram** | Richtet einen einseitigen Telegram-Push beim Warten ein. |
| **callbell-core backlog-system** | Wechselt oder migriert das maßgebliche Planungssystem. |
| **callbell-mode adhd** | Formt die Zusammenarbeit für den Rest der Session handlungsfreundlich für ADHD. |
| **callbell-help** | Zeigt diese zentrale Karte. |

`callbell`, `callbell-core` und `callbell-mode` sind Router. `callbell-help` bleibt der einzige direkte
Einzweck-Einstieg.

## Automatische Fähigkeiten

| Skill | Aktivierung | Aufgabe |
|---|---|---|
| **callbell-core-filing** | beim Ablegen oder Umstrukturieren von Inhalt | Bestimmt die dauerhafte Ablage einer Inhaltsdatei. |
| **callbell-core-import** | bei angekündigtem Import aus `zone-import` | Verarbeitet nicht vertrauenswürdiges Rohmaterial. |
| **callbell-core-git** | bei einem Git-Auftrag oder vor einem gewünschten Commit | Verwaltet Zustand, Diffs, Sync, Commit, Push und Historie. |

`callbell run` darf im Scope-in Subagents, lokale Änderungen, Prüfungen, Worktrees und lokale Commits
nutzen. Es bearbeitet nur einen Task zur Zeit. Push, Publish, Deployment, externe Kommunikation,
irreversible Aktionen, Scope-Erweiterungen sowie Produkt- und Risikoentscheidungen bleiben außerhalb jedes
autonomen Laufs.

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
- Der Pfad sagt, wo Inhalt liegt; Frontmatter sagt, was es ist. Das vollständige Inhaltsschema wird erst
  vor einer tatsächlichen Markdown-Änderung geladen.
- `~/.callbell/settings.json` schaltet Sessionstart und verwaltete Arbeitsvereinbarung nutzerweit.

Claude verwendet `/callbell <modus>`, `/callbell-core <modus>`, `/callbell-mode <modus>` oder
`/callbell-help`; Codex verwendet die entsprechenden `$…`-Skills oder das `/skills`-Menü.
