---
name: intro-animation
description: Plan and build an animated intro for a website from any logo or wordmark - the logo (or its letters) comes alive, acts out short scenes, shows a claim and settles back into the logo. Covers motion with JS keyframes, optional sound (music, effects, voice e.g. with ElevenLabs, music from Suno/Udio/ElevenLabs Music/stock), a tuning panel to fit the music by ear, and animation that hits the beat exactly (beat grid measured from the track). Use when someone wants a logo intro, splash or hero animation, a mascot from their logo, a game-style voice intro, or an animation synced to music.
---

# Logo intro animation

Goal: a short intro (10-20 s) on top of a web page in which the user's **logo or text** comes alive, plays a few
**scenes**, shows a **claim** and ends **exactly as the logo**. Sound is optional; with music the logo or the
letters can **dance on the beat**.

Build it as plain HTML/CSS/JS (one file or one module). No animation library needed.

## 1. Brief - ask in one message
- Logo as SVG (PNG only if nothing else exists - then trace it) or the text/wordmark, brand colors.
- Story: 3-6 scenes of ~2-3 s each (e.g. the sports, tools or moves of the business) and one word per scene.
- Claim before the logo, wording and language.
- Where it goes: stand-alone page or top of an existing site (keep existing navigation untouched).
- Sound yes/no; music source; voice line text.

## 2. Bring the logo to life
Pick the approach that fits the logo:
- **Figure logo** (shapes that can read as head/arms/legs): build a rig from SVG parts - limbs as stroked paths
  (`M shoulder L elbow L hand`), legs with two-bone IK so feet stay on the ground.
- **Symbol logo** (no body): animate its parts - split paths, let them bounce, rotate, assemble; or give the symbol
  simple legs/arms in the same stroke width and colors.
- **Wordmark / text**: split the text into one `<span>` per letter; letters jump, wave, line up, form shapes.
  For gradient text give every letter its slice of one gradient (`background-size` = word box,
  `background-position` = minus the letter offset), otherwise each letter shows the full gradient.

Rules:
- The **last frame must equal the logo**. If the rig cannot reproduce it exactly, fade the exact logo paths in on
  top in the last ~0.15 s. Never show the logo and a separate figure at the same time.
- Use one `gradientUnits="userSpaceOnUse"` gradient for all parts (bounding-box gradients break on straight lines).

## 3. Motion engine: keyframes as a function of time
Every frame is `paint(t)`. That gives seeking (jump to a scene), exact sync with audio, stills for review, no drift.
```js
// channel = [[time, value, ease], ...]; value: number or array; ease: e smooth, o out, i in, l linear
const EASE = { l: t => t, o: t => 1 - (1 - t) ** 2, i: t => t * t, e: t => t < .5 ? 4 * t ** 3 : 1 - (-2 * t + 2) ** 3 / 2 };
const mix = (a, b, f) => Array.isArray(a) ? a.map((v, i) => v + (b[i] - v) * f) : a + (b - a) * f;
function sample(keys, t) {
  if (t <= keys[0][0]) return keys[0][1];
  for (let i = 1; i < keys.length; i++) if (t < keys[i][0]) {
    const [t0, v0] = keys[i - 1], [t1, v1, e = 'e'] = keys[i];
    return mix(v0, v1, EASE[e]((t - t0) / (t1 - t0)));
  }
  return keys.at(-1)[1];
}
// two-bone IK: knee from hip and foot
function knee(hx, hy, fx, fy, side, L) {
  const vx = fx - hx, vy = fy - hy, d = Math.hypot(vx, vy) || 1;
  let kx = hx + vx / 2, ky = hy + vy / 2;
  if (d < 2 * L) { const h = Math.sqrt(L * L - d * d / 4); kx += side * vy / d * h; ky -= side * vx / d * h; }
  return [kx, ky];
}
```
- Channels per moving part: body offset, feet (world coordinates), arms, squash, head, props, logo reveal.
- Loops (running, dribbling) are generated with a `for` loop, not typed by hand.
- Feel: anticipation before big moves, follow-through after, squash for ~0.1 s on landing, a ground shadow that
  shrinks in the air, props that follow the hand until release.
- One `requestAnimationFrame` loop; animate only SVG attributes, `transform` and `opacity`.

## 4. Words and UX
- One big, light word per scene above the animation; shrink long words to the screen width.
- Claim in the same style; the logo gradient is reserved for the wordmark at the end.
- Progress dots (click = jump to scene), Skip, Replay, Sound on/off.
- `prefers-reduced-motion` → show the end state. Play once per session (`sessionStorage`). Start when visible.

## 5. Sound (optional, always opt-in)
Browsers block autoplay with sound - start only from a "Sound on" button. On iPhone set
`navigator.audioSession.type = 'playback'` so it plays in silent mode.

- **Playback**: Web Audio, `source.start(when, offset)` - sample-exact and can start mid-way (dots, tuning).
  Picture clock = audio clock: start the animation at the time `play()` schedules the first sample.
  A `DynamicsCompressor` on the master catches peaks.
- **Effects and voice** (e.g. ElevenLabs Sound Effects / Text to Speech): one effect per hit (kick, whoosh, landing,
  logo impact, crowd), one short voice line (game-announcer style, deep and calm). Prompt pattern for effects:
  *what + material + distance + length + "no music, no voice"*. Generate **one take per sound** to save credits; reuse a
  sound with `playbackRate` instead of generating variants. Cue list: `[name, time, volume, rate]` at the frame of contact.
  Never write API keys into project files - read them from the environment.
- **Music**: ElevenLabs Music (API), Suno or Udio (web apps - Claude writes the prompt, the user generates and uploads),
  or a licensed stock track. Prompt pattern: *instrumental, genre, BPM, build-up, drop after N bars, clean ending hit*.
  Check that the plan's licence allows commercial use.
- Choose the best 20-40 s of the track: build-up under the scenes, the drop on the logo. `musicStart` = that second
  of the track. Fade out or let it end naturally.
- Normalize loudness (e.g. ffmpeg `loudnorm`), mp3 128 kbit/s, effects mono, load files only after "Sound on".

## 6. Fit the sound by ear - tuning panel
Add a panel behind a URL switch (e.g. `?tune`): sliders for music start (second in track), volumes, fade, voice time,
dance end and beat offset; buttons "play from start / from scene"; changes re-schedule the audio immediately; values are
kept in `localStorage` and shown as JSON. The user listens, adjusts, copies the JSON into a `config.json` that the page
loads. Remove the panel when the user has final values.

## 7. Animate on the beat
**Measure the beat grid** (tempo + position of the beats) of the part of the track that plays during the dance:
- With ffmpeg + numpy: decode to mono, compute spectral flux per frame (STFT, log magnitude, positive differences),
  get the tempo from the autocorrelation of the full-band flux, then fit period and offset on the **kick band
  (< 150 Hz)** - hi-hats often sit between the beats and would put the grid on the off-beat. Write a click track
  (a click on every measured beat over the music) and listen before using the numbers.
- Without tools (e.g. in chat): a **Tap beat** button in the tuning panel - the user taps along 8+ times; a straight
  line fit through the tap times gives `beatPeriod` and `beatFirst`.
- `librosa.beat.beat_track` is fine too if available.

**Keep the grid in track time** (`beatFirst` = seconds in the music file). Animation time `t` → track time
`t + musicStart`, so the dance stays on the beat when the music start changes.

**Dance as a pure function of time** (never `setInterval` per beat - it drifts):
```js
const s = (t + musicStart - beatFirst) / beatPeriod;   // beats since the first downbeat
const k = Math.floor(s), u = s - k;                     // beat index, phase in the beat 0..1
const down = k % beatsPerBar === 0;                     // the "1" of the bar
const hop = u => (u < .62 ? Math.sin(Math.PI * u / .62) : 0);   // up on the beat, rest before the next
```
Ideas: all letters jump on the "1", otherwise one after another (`k % letters.length`); squash on landing in the first
10 % of the beat; a wave `sin(2π·s/bar − i·0.9)`; a la-ola on the last beat of each bar (letter `i` delayed by
`i·0.15` beats); the logo raises its arms on every beat; a feature word changes every 2 beats. Multiply everything by a
gain that fades to 0 before the dance ends. If the big accents feel one or two beats off, shift `beatFirst` by whole
`beatPeriod`s - the "1" of the bar is the only guess of the measurement.

## 8. Review
- Debug hook (e.g. `?debug` → `window.intro.at(t)`) to render any frame; take stills with Playwright and check them as a
  contact sheet: feet planted, knees bend sensibly, nothing passes through the head, props in hand, last frame = logo.
- No console errors; works without audio files; reduced motion shows the logo; fits 375 px width.
- Sound: effects on contact, voice ends with the logo hit, dance checked by ear on a phone speaker
  (Bluetooth adds latency).

## Output
- Claude Code: integrate into the project, audio files in one folder, explain the tuning switch and the debug hook.
- Claude chat: one HTML file (artifact); list the sound prompts and file names for the user to generate and place.
