---
name: callbell-core
description: >
  Steuert auf ausdrücklichen Aufruf die manuellen Core-Werkzeuge für Setup, Diagnose, Statusline,
  Telegram-Ping, Planungssystem und die zentrale Plugin-Hilfe. Ohne eindeutigen Modus nur die verfügbaren
  Modi nennen und nichts ändern. Niemals automatisch bei einem neuen Ordner oder fehlendem Scaffold starten.
disable-model-invocation: true
argument-hint: "[setup|doctor|statusline|ping telegram|backlog-system|help] [args]"
license: MIT
type: skill
edit: locked
---

# Callbell Core

Der Nutzer besitzt den Start jedes manuellen Core-Werkzeugs. Wähle genau den genannten Modus; ein Modus
autorisiert keinen weiteren.

## Modus wählen

- **`setup`:** Fahre mit dem Abschnitt **Setup** fort.
- **`doctor [args]`:** Lies vollständig [den Doctor-Skill](../callbell-core-doctor/SKILL.md) und führe ihn
  mit den restlichen Argumenten aus.
- **`statusline [args]`:** Lies vollständig [den Statusline-Skill](../callbell-core-statusline/SKILL.md) und
  führe ihn mit den restlichen Argumenten aus.
- **`ping`** oder **`ping telegram [args]`:** Lies vollständig
  [den Telegram-Ping-Skill](../callbell-core-telegram-ping/SKILL.md) und führe ihn mit den restlichen
  Argumenten aus.
- **`backlog-system [quelle] [ziel]`:** Lies vollständig
  [den Planungssystem-Skill](../callbell-core-backlog-system/SKILL.md) und führe ihn mit den restlichen
  Argumenten aus.
- **`help`:** Lies vollständig [die zentrale Hilfe](../callbell-core-help/SKILL.md) und zeige deren Karte.

Ohne Modus oder bei einem unbekannten Modus nenne `setup`, `doctor`, `statusline`, `ping telegram`,
`backlog-system` und `help` jeweils knapp und ändere nichts. Frage bei einem unbekannten Modus nach genau
einem davon.

## Setup

Richte Callbell ein und halte die sichtbare Übergabe kurz. Beschreibe weder die Hook-Abläufe noch die
internen Prüfungen des Scripts.

### 1. Node

Führe `node --version` aus. Scheitert der Aufruf, nenne Node als einzigen Blocker, verweise auf
[nodejs.org](https://nodejs.org) und stoppe. Unter Windows braucht ein neu installiertes Node gegebenenfalls
ein neues Terminal. Schließe auch dann mit `Mehr: callbell-core help`.

### 2. Einrichten

`<plugin-root>` ist der im Session-Kontext genannte `CALLBELL PLUGIN ROOT`, andernfalls der Ordner zwei
Ebenen über dieser `SKILL.md`.

```text
node <plugin-root>/scripts/callbell-core-doctor.js --apply
```

Nutze den Output als Befund. Wiederhole nicht, was geprüft wurde, und nenne nichts, das bereits vorhanden
war. Das Script darf vorhandene Projektdateien nicht ersetzen.

Hat Doctor `AGENTS.md` neu aus `scaffold/agents-template.md` angelegt, konkretisiere sie vor der Übergabe:

1. Ermittle Repo-Name und Zweck aus Verzeichnis, README, Manifesten und vorhandenem Inhalt.
2. Ersetze die beiden Platzhalter durch konkrete Aussagen. Entferne keinen sicheren Standard.
3. Ergänze nur tatsächlich bekannte projektspezifische Grenzen. Frage nach Sichtbarkeit, Scope oder
   Agentenrolle ausschließlich dann, wenn die Antwort eine anstehende Handlung wesentlich verändert.
4. Lass keine Platzhalter oder Einrichtungs-Kommentare zurück.

Eine bereits vorhandene `AGENTS.md` gehört dem Nutzer und wird von diesem Einstieg nicht umgeschrieben.

### 3. Git

Meldet Doctor ein fehlendes Git-Repo, frage kurz, ob du `git init` ausführen sollst. Handle erst nach der
Antwort.

Bei Zustimmung:

1. Führe `git init` aus.
2. Lies `user.name`, `user.email` und `init.defaultBranch` mit `git config --global --get <schlüssel>`.
3. Nenne die vorhandenen globalen Werte, die Git für das Repo beziehungsweise künftige Commits verwendet.
   Schreibe sie nicht zusätzlich in die lokale Config.
4. Fehlen Name oder E-Mail-Adresse, erfinde nichts und ändere die globale Config nicht ohne gesonderte
   Zustimmung. Melde den fehlenden Wert knapp.

### 4. Übergabe

Fasse nur tatsächlich Angelegtes und offene Blocker zusammen. Bei der ersten Einrichtung genügen diese
Orientierungspunkte:

- `AGENTS.md` trägt die Projektanweisungen; `CLAUDE.md` bindet sie für Claude ein.
- `__callbell__/` trägt den Wegweiser zum maßgeblichen Planungssystem, Memory und die beiden Zonen.
- `~/.callbell/` trägt nutzerweite Einstellungen und die von Callbell verwaltete `rules/RULESET.md`.

Schließe immer mit `Mehr: callbell-core help`. War nichts zu tun, antworte nur:

```text
Callbell ist eingerichtet. Mehr: callbell-core help
```
