---
name: intro-animation
description: Design and build an animated intro for a website, app or video from any logo, symbol or wordmark - the logo comes alive, tells a short story and settles back into the exact logo. Starts with a creative interview (idea, story, style, mood, sound), proposes concepts, then builds it in HTML/SVG/JS with keyframes, optional sound (music, effects, voice - e.g. ElevenLabs, Suno, Udio, stock), a tuning panel to fit the music by ear, and motion that lands exactly on the beat of the music. Use for logo intros, splash screens, hero animations, kinetic typography, game-style voice intros, or any animation synced to music.
---

# Intro animation

Turn someone's logo or text into a short animated intro (usually 5-20 s) that ends exactly on the logo.
Every brand and every person has a different idea, so **start by asking**, then propose, then build.

The result is plain HTML/CSS/JS (one file or one module, no framework required). Libraries are optional
(see "Tools" below) - the core techniques here need none.

---

## Phase 1 - Creative interview

Ask in **one message**, grouped and short, so the user can answer in a few lines. Offer examples so nobody has
to start from a blank page. Skip questions the user already answered.

**About the brand**
1. Logo: SVG (best), PNG/JPG (will be traced or rebuilt), or only a name/text? Colors and fonts?
2. What does the brand do, in one sentence? Who should feel addressed (customers, investors, fans, kids…)?
3. Personality in three words (e.g. playful / premium / bold / calm / technical / handmade)?

**About the idea**
4. What should happen with the logo? Do you already have an idea or story? If not, here are directions
   to pick, mix or ignore (they are only examples - the user's own idea always wins):
   - **Assembly** - parts fly in, snap together, or particles/dots form the logo
   - **Drawing** - the logo is drawn line by line like a pen or neon sign
   - **Morph** - one shape transforms into the next and finally into the logo
   - **Kinetic type** - words and letters move, stack, bounce, form the claim, then the wordmark
   - **Reveal** - mask, light sweep, liquid, paper fold, glitch, shutter
   - **Journey** - a camera move through a small scene that ends on the logo
   - **Character** - the logo gets a face or limbs and acts something out

   Never assume a form, a theme or a story. Whether the logo moves as a shape, as letters or as a character,
   and what it does, is the user's decision.
5. Scenes or messages to show (e.g. 3-6 words: products, values, activities)? A claim at the end?
6. Length (short splash 3-5 s, story 10-20 s) and pace (calm, punchy, playful)?

**About the setting**
7. Where does it run: top of a website, splash screen of an app, social video, presentation?
   Must anything stay untouched (navigation, existing layout)?
8. Plays once per visit, every time, or on click? Skip button wanted?
9. Light or dark background? Phone first?

**About sound** (optional)
10. With sound? Music: own track, generated (Suno, Udio, ElevenLabs Music), or stock? Sound effects?
    A voice line (e.g. game-announcer style) - which words and language?
11. Should something move exactly on the beat (letters dancing, logo pulsing, words changing per beat)?

If the user says "surprise me", choose based on the brand personality and say what you chose.

## Phase 2 - Concepts

Propose **2-3 short concepts** before building, each in 3-5 lines:
- the story beat by beat with rough timing (`0-2 s …, 2-4 s …, … logo at 9.5 s`),
- the visual style (flat, line, gradient, 3D-ish, hand-drawn, neon…),
- sound idea (music mood/BPM, effects, voice line),
- what makes it memorable (the one moment people will remember).

Let the user pick or mix, then write a short **storyboard** (table: time, what moves, word on screen, sound) and
confirm it. Changing a storyboard is cheap - changing finished keyframes is not.

## Proven order of work (reference run)

This is the order in which a finished intro usually comes together. Follow it, show the result after every step
(screenshots or a short screen recording), and let the user react before going on.

1. **Rebuild the logo exactly as SVG.** From a PNG/JPG: trace or rebuild the shapes, overlay the original at 50 %
   opacity and correct until they match. Derive the parts that will move (shapes, lines, letters, pieces) from this SVG.
2. **First motion pass** of the chosen idea: the scenes (with a word each, if wanted), then the claim, then the
   settle into the logo. Logo centered, wordmark fully visible (gradient text needs a little padding so the last letter is not clipped).
3. **Iterate on feedback.** Typical notes: a move looks wobbly, a word is too dark or too small, two scenes are too
   similar, a word should be replaced. Change only what was asked; everything the user approved stays as it is.
4. **Ask for the music.** "Do you have a track? Send the file. Which part should play?" If the user has none, offer to
   write a prompt for a music generator or suggest stock. Never assume a track.
5. **Build the tuning panel right away** (Phase 5), so the user can set the music start, volumes and voice timing by
   ear on their own device. Ask them to send the JSON values; put them into the config and hide the panel. If they
   later bring a new track, show the panel again.
6. **Effects and voice.** Generate one take per sound (Phase 4), place each on its frame of contact.
7. **Ask about the beat.** "Should the logo or the letters move on the beat at the end?" Measure the beat grid of
   exactly that section (Phase 6), start with a simple bounce, then offer more layers one at a time
   (wave, la-ola, logo pulse or gestures, words changing on the beat).
8. **Ask before adding content.** If words should appear (features, values, products), propose a list and let the
   user choose and order them - do not invent final copy on your own.
9. **Timing of the rest of the page.** Decide with the user what appears when: e.g. the text below the logo only
   after the music ends, a final small detail (sparkle, drop, wink) on the last accent.
10. **Optional extension.** The animation can continue while scrolling (the logo reacts to the page, e.g. pushes,
    pulls or reveals the next content), or the page can scroll itself so the last move lands on the final accent of the music.
11. **Polish pass.** Shadows, small details, easing, smoother transitions - list the ideas, let the user pick.
12. **Final check** (Phase 7) and hand-over with the values the user can still tune.

Respect the user's creative decisions: do not shorten, hide or tone down the intro unless they ask for it.

## Phase 3 - Build the motion

### Prepare the logo
- Clean the SVG: one `viewBox`, no transforms on groups if possible, paths named or grouped by part.
- Raster logo: rebuild the main shapes as SVG paths (circles, rects, simple paths) or trace them; keep colors exact.
- Text: use the real font (web font) or convert the wordmark to paths if the font is not available on the web.
- **The last frame must equal the logo.** If the animated version cannot reproduce it exactly, fade the exact logo
  paths in on top during the last ~0.15 s (same position, same size). Never show the logo and a second "copy" side by side.
- Gradients across moving parts: one `gradientUnits="userSpaceOnUse"` gradient shared by all parts
  (bounding-box gradients break on perfectly straight lines).

### Choose the technique per idea
| idea | technique |
|---|---|
| assembly | each part has a start transform (offset, rotation, scale) and flies to identity; stagger by part |
| particles forming the logo | sample points inside the logo shape (draw it to a canvas, read pixels), move particles from random positions to those points |
| drawing | `pathLength="1"`, `stroke-dasharray="1 1"`, animate `stroke-dashoffset` 1 → 0; fill fades in after the line |
| morph | paths with the same number and order of points, interpolate coordinates; or a morph library for arbitrary shapes |
| kinetic type | one `<span>` per letter or word; transforms per letter with stagger; for gradient text give every letter its slice of one gradient (`background-size` = word box, `background-position` = minus the letter offset) |
| reveal | `clip-path` / SVG mask animated over time; light sweep = a moving gradient inside the logo clip |
| camera move | wrap the scene in a group and animate its `transform` (pan, zoom) |
| character (only if chosen) | rig from SVG parts: limbs as stroked paths with joints, two-bone IK (below) so end points stay in place |

### Engine: everything is a function of time
Every frame is `paint(t)`. That gives seeking (jump to a scene), exact sync with audio, stills for review and no drift.
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
const step = (keys, t) => keys.reduce((v, k) => (t >= k[0] ? k[1] : v), keys[0][1]);   // on/off states

// only for characters - two-bone IK: joint position from root and end point
function joint(rx, ry, ex, ey, side, L) {
  const vx = ex - rx, vy = ey - ry, d = Math.hypot(vx, vy) || 1;
  let jx = rx + vx / 2, jy = ry + vy / 2;
  if (d < 2 * L) { const h = Math.sqrt(L * L - d * d / 4); jx += side * vy / d * h; jy -= side * vx / d * h; }
  return [jx, jy];
}

let start = 0;
function loop(now) {
  const t = (now - start) / 1000;
  paint(Math.min(t, END));
  if (t < END) requestAnimationFrame(loop); else finish();
}
```
- One channel per moving property (position, rotation, scale, opacity, path progress, color, mask…).
- Repeating motion (pulsing, rotating, bouncing) is generated with a `for` loop.
- Springs for organic overshoot: `x += v; v += (target - x) * k - v * damping` per frame - but sample them in
  advance into keyframes if the intro must be seekable.
- Animate only SVG attributes, `transform`, `opacity` (and `clip-path`); one `requestAnimationFrame` loop.

### Make it feel alive (the 12 principles, applied)
- **Anticipation**: a small move in the opposite direction before a big one (pull back, then shoot forward).
- **Follow-through and overlap**: secondary parts arrive slightly after the main shape and settle with a small overshoot.
- **Squash and stretch**: short squash on impact, stretch in fast moves; keep the area roughly constant.
- **Arcs**: things in motion travel on curves, not straight lines.
- **Timing**: fast in, slow out for impacts; hold key poses ~0.2 s so the eye can read them.
- **Staging**: one idea at a time; the word on screen names it.
- Grounding (if something lands or floats): a soft shadow that shrinks and fades with height.

### Words, claim, wordmark
- One big word per scene (light color, large, above or behind the animation); shrink long words to the screen width.
- The claim in the same style; reserve the brand gradient/color for the logo and wordmark at the end.
- The wordmark enters after the logo lands (scale/fade or letter by letter).

### UX and accessibility
- Progress dots (click = jump to scene), Skip, Replay, Sound on/off.
- `prefers-reduced-motion: reduce` → show the final logo without motion.
- Play once per session (`sessionStorage`) unless the user wants otherwise; start when visible (IntersectionObserver).
- Split text keeps an `aria-label` with the full word; decorative SVG gets `aria-hidden`.
- No layout shift: reserve the space the intro needs.

## Phase 4 - Sound (optional, always opt-in)

Browsers block autoplay with sound - start audio only from a "Sound on" button. On iPhone set
`navigator.audioSession.type = 'playback'` (where supported) so it plays in silent mode.

### Playback
- Web Audio: decode files into buffers, schedule with `source.start(when, offset)` - sample-exact and able to start
  in the middle of the intro (dots, tuning).
- Picture clock = audio clock: when `play(from)` schedules the first sample at audio time `t0`, start the animation clock
  at that same moment.
- Cue list for effects: `[name, timeInSeconds, volume, playbackRate]`, time = the frame of contact.
- A `DynamicsCompressor` on the master output catches peaks when music, effects and voice coincide.
- Load audio only after "Sound on"; mp3 ~128 kbit/s, effects mono. Normalize loudness (e.g. ffmpeg `loudnorm`):
  music and voice around -14 LUFS, effects around -16, then fine-tune with volumes.

### Sources
- **Effects** (e.g. ElevenLabs Sound Effects): prompt = *what + material + distance + length + "no music, no voice"*,
  e.g. "single soft pop, close, dry, no music". Typical set: whoosh, impact/landing,
  pop, click, sparkle, crowd, one big logo hit.
- **Voice** (e.g. ElevenLabs Text to Speech): one or two short lines; let the user choose the voice from the library,
  never hard-code voice IDs from memory.
- **Music**: ElevenLabs Music (API), Suno or Udio (web apps - write the prompt, the user generates and uploads the file),
  stock libraries, or the user's own track. Prompt pattern:
  `Instrumental, <genre>, <BPM> BPM, <mood>, short build-up, drop after 8 bars, clean ending hit, no vocals.`
- Choose the best 20-40 s: build-up under the scenes, the drop or accent on the logo. `musicStart` = that second of
  the track. Fade out, or let it end naturally if the ending fits.
- **Save the user's credits**: one take per sound, reuse a sound with a different `playbackRate` instead of generating
  variants, never retry silently in a loop. API keys only from environment variables or the connector - never in files.
- Remind the user to check the licence of their plan for commercial use (music, voices).

### In Claude chat vs. Claude Code
- With an audio connector (e.g. ElevenLabs) available, generate the sounds directly (one take each).
- Without it, give the user a table: file name, prompt, length - and where to put the files.
- In Claude Code, scripts that call an API read the key from the environment and skip files that already exist.

## Phase 5 - Tuning panel (fit the sound by ear)

Timing by ear beats timing by calculation. Add a panel behind a URL switch (e.g. `?tune`):
- sliders: music start (second in the track, show mm:ss), music volume, fade start and length, effects volume,
  voice volume and time, dance end, beat offset;
- buttons: play from start, play from a scene, stop, copy JSON, reset;
- every change re-schedules the audio from the current time, values persist in `localStorage`, JSON is shown;
- the page loads the final values from a `config.json`, so the panel can be removed after tuning.

Tell the user how to use it: listen, move a slider, listen again, copy the JSON when it feels right.

## Phase 6 - Motion on the beat

### Measure the beat grid
Analyse only the part of the track that plays while things move on the beat.
- **With ffmpeg + numpy** (write a small script): decode to mono (~22 kHz), STFT (window ~1024, hop ~256),
  log magnitude, spectral flux (positive frame-to-frame differences).
  - Tempo: autocorrelation of the full-band flux, score candidate BPMs (e.g. 70-180) and their multiples.
  - Beat position: fit period and offset on the **kick band (< 150 Hz)** - hi-hats often sit between the beats and
    would put the grid on the off-beat.
  - Output `beatFirst` (seconds in the track), `beatPeriod`, `bpm`, `beatsPerBar`.
  - Write a **click track** (the music with a click on every measured beat) and let the user listen before using it.
- **With librosa**: `librosa.beat.beat_track` and check the result with a click track the same way.
- **Without tools** (e.g. in chat): a **Tap beat** button in the tuning panel; the user taps along 8+ times;
  a least-squares line through the tap times gives `beatPeriod` (slope) and `beatFirst` (intercept).
- If the tempo comes out half or double, ask the user or pass a BPM hint. Which beat is the "1" of the bar is the
  only real guess - if accents feel early or late, shift `beatFirst` by whole `beatPeriod`s.

### Keep the grid in track time
`beatFirst` is a second **in the music file**. Animation time `t` maps to track time `t + musicStart`.
Then the dance stays on the beat even when the user moves the music start in the tuning panel.

### Beat motion as a pure function of time
Never schedule one timer per beat (`setInterval` drifts). Compute from the clock every frame:
```js
const s = (t + musicStart - beatFirst) / beatPeriod;   // beats since the first downbeat
const k = Math.floor(s), u = s - k;                     // beat index, phase inside the beat 0..1
const down = k % beatsPerBar === 0;                     // the "1" of the bar
const hop = u => (u < .62 ? Math.sin(Math.PI * u / .62) : 0);   // up on the beat, rest before the next
```
Building blocks (combine, multiply all by a gain that fades out before the end):
- **Bounce**: all letters jump on the "1", otherwise one letter after another (`k % letters.length`).
- **Landing squash**: `scale(1.05, .92)` during the first 10 % of the beat.
- **Wave**: `translateY(sin(2π · s / bar − i · 0.9))` - one gentle pass per bar.
- **La-ola**: on the last beat of each bar, letter `i` rises with a delay of `i · 0.15` beats.
- **Pulse**: logo scale `1 + 0.04 · hop(u)`, glow or light sweep on the "1".
- **Shape gestures**: parts of the logo open, rotate or stretch on every beat; a bigger move on the "1".
- **Words on the beat**: change a word every 1-2 beats, pulse it on the beat.
- **Cuts**: scene changes on bar starts feel intentional.

## Phase 7 - Review before handing over

- **Stills**: a debug switch (e.g. `?debug` → `window.intro.at(t)` and `window.intro.dance(t)`) renders any frame.
  Take stills with Playwright (or ask the user for screenshots) and check them as a contact sheet: poses readable,
  nothing intersects in a wrong way, transitions are smooth, words match, **last frame = logo**.
- **Function**: no console errors; works without audio files; Skip/Replay/dots work; reduced motion shows the logo;
  fits a 375 px wide phone; no layout shift.
- **Sound**: effects on contact, voice ends with or before the logo hit, music start and fade feel right,
  beat motion checked by ear on a phone speaker (Bluetooth adds latency).
- **Performance**: smooth on a mid-range phone; audio only loaded after "Sound on".

## Deliver
- **Claude Code**: integrate into the project (keep existing layout and navigation), put audio in one folder,
  explain the tuning and debug switches, and how to change words, timing and colors.
- **Claude chat**: one self-contained HTML file (artifact) plus the list of sound files and prompts.
- In both cases, summarise the storyboard as built and the values the user can tune.

## Tools (optional)
The engine above needs no library. Use one when it clearly helps and the user agrees:
- GSAP (timelines, MorphSVG), anime.js, Motion One - timeline-based animation in the browser.
- Lottie (After Effects export) when a designer animates; still add the sound and beat layer from this skill.
- flubber - morphing between arbitrary SVG shapes.
- Rive - interactive state-machine animations.
- Three.js - when the intro should be 3D.
