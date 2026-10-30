# IntroAnimationSkill

Ein Claude-Skill für animierte Logo-Intros auf Websites: Das eigene Logo oder der eigene Schriftzug wird lebendig,
spielt kurze Szenen, zeigt einen Claim und landet am Ende wieder exakt als Logo. Optional mit Musik, Soundeffekten
und Sprecher – und einer Animation, die genau auf dem Beat der Musik liegt.

Der Skill ist eine Anleitung für Claude (`skills/intro-animation/SKILL.md`), kein fertiger Code. Claude baut damit das
Intro für dein Logo – egal ob Figur, Symbol oder Text.

## Was der Skill abdeckt
- **Briefing**: Logo/Text, Szenen, Wörter, Claim, Ton – in einer Frage.
- **Logo zum Leben erwecken**: Figur aus dem Logo (Rig mit Gelenken, Füße bleiben am Boden), Symbol zerlegen oder
  Schriftzug in Buchstaben teilen. Letztes Bild = exaktes Logo.
- **Bewegung**: Keyframes als Funktion der Zeit (springen zu Szenen, Standbilder, kein Auseinanderlaufen mit dem Ton),
  Ausholen, Nachschwingen, Stauchen bei der Landung, Schatten.
- **Ton**: Effekte und Stimme z. B. mit ElevenLabs (eine Variante pro Sound, spart Credits), Musik mit ElevenLabs Music,
  Suno, Udio oder lizenzierter Stock-Musik, Lautstärke angleichen, Abspielen per Web Audio.
- **Regler-Feld** (`?tune`): Musikstart, Lautstärken, Zeitpunkte nach Gehör einstellen und als JSON übernehmen.
- **Beat-Sync**: Beatraster aus dem Track messen (Kick-Drum statt Hi-Hats), Prüf-Klicktrack, oder Beat per Tap einklopfen;
  Tanz der Buchstaben/des Logos als reine Funktion der Zeit (Bounce, Welle, La-Ola, Hands up).
- **Prüfen**: Standbilder jeder Szene, reduzierte Bewegung, Handy-Breite, Ton auf dem Handy-Lautsprecher.

## Installation

### Claude Code
Als Plugin aus diesem Repo:
```
/plugin marketplace add marcohildebrandmh-ops/IntroAnimationSkill
/plugin install intro-animation@intro-animation-skill
```
Oder nur den Ordner kopieren:
- für alle Projekte: `skills/intro-animation` nach `~/.claude/skills/intro-animation`
- für ein Projekt: nach `.claude/skills/intro-animation` im Projekt

### Claude (Chat, claude.ai / Desktop / App)
1. Ordner `skills/intro-animation` als ZIP packen (die ZIP enthält den Ordner mit `SKILL.md`).
2. In Claude: **Einstellungen → Fähigkeiten (Capabilities) → Skills → Skill hochladen**. Code-Ausführung muss aktiv sein.
3. Im Chat z. B.: *„Mach mir ein Logo-Intro für meine Website, hier ist mein Logo als SVG.“*

## Beispiel-Anfragen
- „Animiere unser Logo: Die Figur springt, wirft und läuft, dann kommt unser Claim, dann das Logo.“
- „Lass die Buchstaben unseres Schriftzugs auf dem Beat dieser MP3 tanzen.“
- „Schreib mir einen Suno-Prompt für 20 Sekunden Intro-Musik mit Drop auf dem Logo.“
- „Bau ein Regler-Feld, damit ich den Musikstart selbst einstellen kann.“

## Hinweise
- API-Schlüssel (z. B. ElevenLabs) nie in Dateien schreiben – nur über Umgebungsvariablen oder den Connector.
- Für Musik und Stimmen die Lizenzbedingungen des jeweiligen Tarifs prüfen (kommerzielle Nutzung).
