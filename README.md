# Intro Animation Skill

A Claude skill for animated intros made from any logo, symbol or wordmark: the logo comes alive, tells a short story
and lands exactly on the logo at the end - optionally with music, sound effects, a voice line and motion that hits
the beat of the music.

The skill is a guide for Claude, not finished code. Claude first asks about your idea, brand and style,
suggests concepts, and then builds the intro for you.

## What it covers
- **Creative interview**: logo, brand personality, idea or story, scenes, claim, length, where it runs, sound.
- **Concepts and storyboard**: 2-3 directions based on your answers (e.g. assembly, line drawing, morph, kinetic type,
  reveal, camera journey, character; the form is always your choice), then a timed storyboard to confirm before building.
- **Motion**: keyframes as a function of time, inverse kinematics if the logo becomes a character, line drawing, morphing,
  particles, per-letter text animation, animation principles (anticipation, follow-through, squash, arcs).
- **Sound**: Web Audio playback in sync with the picture; effects and voice (e.g. ElevenLabs), music
  (ElevenLabs Music, Suno, Udio, stock or your own track), loudness, licences, saving credits.
- **Tuning panel**: adjust music start, volumes and timing by ear and copy the values as JSON.
- **Beat sync**: measure the beat grid of the track (or tap it in), keep it in track time, and drive bounces,
  waves, pulses and word changes from the beat without drift.
- **Review**: stills of every frame, accessibility, phone layout, sound check.

## Install

### Claude Code
Copy the `intro-animation` folder into your skills folder:
- for all projects: `~/.claude/skills/intro-animation/`
- for one project: `.claude/skills/intro-animation/` inside the project

Claude picks the skill up automatically when you ask for a logo intro or a beat-synced animation.

### Claude (claude.ai, desktop and mobile apps)
1. Zip the `intro-animation` folder (the zip contains the folder with `SKILL.md`).
2. In Claude go to **Settings → Capabilities → Skills** and upload the zip. Code execution must be enabled.
3. Ask, for example: *"Make an animated intro for my website, here is my logo as SVG."*

## Example prompts
- "Let our logo assemble from small pieces, then show our claim, then the logo."
- "Let the letters of our wordmark dance on the beat of this MP3."
- "Draw our logo line by line like a neon sign, with a short electric sound."
- "Write a music prompt for a 20-second intro with the drop on the logo."
- "Add a panel so I can set the music start myself."

## Notes
- Never put API keys (e.g. for ElevenLabs) into files - use environment variables or a connector.
- Check the licence terms of your music and voice tools for commercial use.

## License
MIT - see [LICENSE](LICENSE).
