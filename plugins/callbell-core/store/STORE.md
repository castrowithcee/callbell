---
description: >
  Index der versionierten Callbell-Assets und -Vorlagen: was im Store liegt und wann es gezielt verwendet
  werden darf.
type: meta
edit: locked
---

# Store: Assets und Vorlagen nur bei Bedarf

Der Store enthält ausschließlich Assets und Vorlagen, die mit dem Plugin aktualisiert und für ein konkretes
Ziel kopiert oder als Ausgangspunkt verwendet werden. Er ist kein Ort für Regeln, Schemata oder andere
Verhaltensreferenzen. Lade ihn weder beim Sessionstart noch auf Vorrat. Öffne diesen Index nur bei einem
konkreten Bedarf, wähle einen passenden Eintrag und verwende danach ausschließlich die dort genannten
Dateien.

## Bibliothek

| Bedarf | Lies nur | Verwendung |
|---|---|---|
| Aufgabe im lokalen Callbell-Backlog | `backlog/task.md` | Als `__callbell__/backlog/task-<id>-<slug>.md` oder im betreffenden Projekt anlegen und nach der Backlog-Referenz ausfüllen. |
| Projekt im lokalen Callbell-Backlog | `backlog/project-index.md` | Als `__callbell__/backlog/<project>/INDEX.md` anlegen und nach der Backlog-Referenz ausfüllen. |
| Wiederkehrende Ablage pro Kunde oder Fall | `customers/customers-framework.md` und `customers/customer-index.md` | Als `<area>-customers/FRAMEWORK.md` und pro Kunde als `<area>-customers/<id>/INDEX.md` kopieren und anpassen. |
| Statusline auf hellen und dunklen Terminalhintergründen | `statusline/dual-theme.json` | Über `~/.callbell/statusline.json` kopieren. |

Der installierte Store ist schreibgeschützt, weil sein versionsgebundener Pfad beim Update ersetzt wird.
Kopiere benötigtes Material an sein Ziel und ändere es dort. `__callbell__/templates/` ist ausschließlich
die versionierte Vorlagenbibliothek des Nutzers. Callbell legt dort nichts ab. Eigene Vorlagen gehören
dorthin, niemals in den Store.
