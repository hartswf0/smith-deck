# SMITH • DECKS — README

A touch-first, browser-based dual-deck playground for sound. Each “deck” is an interactive Smith Chart interface that generates or processes audio. The top bar provides a DJ-style control surface for blending, transport, and recording.

This repo is pure HTML/CSS/JS. No build step required.

## Contents

- Root pages
  - `index.html` — Dual-deck “mixer.” Hosts two deck iframes and the top-bar controls:
    - Crossfader (equal‑power), instant cut A/B
    - Start ⎯ Pause/Resume both
    - Record the mixed output
  - `smith-968.html`, `smith-137.html`, `smith-255.html` — Individual “decks.” Each draws a Smith Chart canvas, maps touch/drag to synth and FX, and can play radio or local audio.
  - `double-smith.html`, `dub-base.html`, `sg-00.html`, `sg-01.html`, `ai_studio_code (90).html`, `ai_studio_code (92).html`, `smith-radio.html` — Experimental or earlier single-deck explorations.
  - `notes.md` — Scratchpad for issues/logs (e.g., CORS errors).

- Misc
  - `.vscode/settings.json` — Local IDE settings (useful for Live Server workflow).
  - `smith_decks_*.webm` — Example recorded output(s).

## The deck (index.html)

Two equal square slots, A and B. Any table goes in either slot; the mixer and routing sit around them.

- **Tables.** Pickers list Rings (every moto album as a mandalla table, plus ring·smith), Puppet, Smith,
  Deco and Morph (FIELD, SEQUENCER, DEEP FIELD), or any URL. `?a=…&b=…` makes a setup a link.
- **Clean squares.** The host hides each table's own chrome and scales it to the square. ☰ on a pad
  opens that table's own controls. No table file is edited.
- **Mixer.** Level fader + meter per deck, crossfade, cut A / B, SWAP A/B (no reload), PLAY, BPM ± / TAP
  (sent to tables that follow tempo), REC of the mix.
- **Audio capture.** The host reroutes whatever a same-origin table sends to its speakers into its own mix.
  (The old `request-audio-stream` handshake could never work: a `MediaStream` cannot be posted between windows.)
- **Routing, A → B and B → A, each with an amount:**
  - `SEND → MIC IN`: the source table's sound replaces the target's microphone, so a puppet / vocoder /
    sampler table processes the other table. Needs a target with a mic input (Smith, Puppet, Deco, Morph sampler).
  - `RING MOD`: target × source at audio rate.
  - `GATE`: target is heard only while the source is sounding.
  - `DUCK`: source pushes the target down.
  - `FILTER`: source loudness opens a low-pass on the target.
  The last four work on any pair of tables, because they act on the captured audio in the host.
- **Crate.** A moto album track into a Smith / Puppet / Deco table's radio input; from there it can be
  sent to or modulate the other deck like any other source.
- **To make a new table plug in:** one canvas that follows the window `resize` event, and all controls inside
  `id="controls-container"`. Optional: a `#micInputToggle` button that calls `getUserMedia` (to receive SEND),
  a `#radioSource` select (to receive crate tracks), `window.scionDeck.setBpm` (to follow tempo).

The earlier host is kept as `index-min-alt.html`.

## Quick Start

1) Serve the folder with a local HTTP server.
   - VS Code “Live Server” works great.
   - Or via Python:

   ```bash
   # macOS / Linux
   python3 -m http.server 5503
   # then open:
   # http://127.0.0.1:5503/index.html
   ```

2) Open `index.html`.

3) On mobile: rotate to landscape for more room.

4) Tap inside each deck once to initialize audio (browser requirement). Once initialized, small A/B LEDs next to the crossfader will light when the parent mixer has the streams.

## Using the Mixer (index.html)

- Top bar controls
  - Start ▶︎ — Starts both decks (resumes audio in each iframe, pulls streams into the parent mixer, and mutes local monitors for a clean blend).
  - Pause ⏯ — Pauses/resumes both decks’ AudioContexts.
  - Record ● / Stop ■ — Records the parent mix and downloads as a file.
  - Crossfader — Equal‑power A↔B blend. Instant cuts via A/B buttons.
  - A/B LEDs — Indicate that the parent page has captured each deck’s stream.

- Deck selection
  - Each side (A/B) has a dropdown to choose a deck page:
    - `smith-968.html`
    - `smith-137.html`
    - `smith-255.html`
  - Fullscreen button per deck iframe.

- Under the hood
  - Parent page owns a single AudioContext for mixing/recording.
  - Each deck exposes an internal mixed stream to the parent via `postMessage` and a `MediaStreamDestination`.
  - The parent mutes deck “monitor” gains so only the mixed signal is heard when recording/blending.

## Decks (smith-968 / smith-137 / smith-255)

All three are variations on an interactive Smith Chart instrument:
- Canvas shows a Smith Chart and HUD.
- Touch/drag inside the chart changes timbre/filters/envelopes.
- Rotary “ring” controls around the chart trim synth/radio levels.
- UI dashboard at the bottom for FX and routing.

Common behaviors:
- Tap to initialize audio (first gesture).
- Radio source select (Off, presets, Custom URL, Local File…).
- For compatible ones, drag‑and‑drop an audio file onto the canvas to load it locally (bypasses CORS).
- “Monitor” gain can be turned off by `index.html` when mixing to avoid double audio.

Notes:
- `smith-968.html` includes the most elaborate FX/analysis chain and visual overlays.
- `smith-137.html` is a more compact synth-focused variation.
- `smith-255.html` focuses on HUD clarity and grouped controls.

## Local File Audio vs URLs

- Local file support
  - Choose “Local File…” in the deck’s Radio selector, or drop an audio file onto the canvas.
  - This uses `createMediaElementSource(new Audio(objectURL))` inside the deck’s AudioContext, so it does not hit CORS.

- URLs (what works)
  - Direct audio file or icecast/shoutcast MP3/AAC streams that allow CORS.
  - Example: SomaFM stations typically work.

- Not supported directly
  - YouTube/Spotify/Apple Music and many commercial radio endpoints do NOT expose CORS-enabled direct audio URLs and/or use DRM. They cannot be loaded directly by the browser’s Web Audio pipeline. A server-side proxy/transcoder would be required.

## Recording

- From `index.html`, click “● Record” to start capturing the parent mix bus, then “■ Stop” to download.
- Format: `audio/webm` (falls back when needed).
- Tip: Start ▶︎ first to ensure both deck streams are available.

## Mobile UX

- The top bar is intentionally minimal and touch-friendly.
- Crossfader thumb is larger, and haptics fire on center/cuts (where supported).
- Decks resize charts and HUD to maintain aspect on narrow screens.

## Known Limitations

- Browser gestures: You must interact (tap) inside each deck to start audio (required by browser autoplay policies).
- CORS for remote audio: Many external stations block cross-origin access; use local files or stations with CORS enabled.
- Stream capture timing: If the crossfader doesn’t affect sound immediately, nudge the fader, press Start ▶︎ again, or tap inside the deck to reinitialize.

## Troubleshooting

- No sound from a deck in the mixer:
  - Tap inside that deck iframe.
  - Check the A/B LED next to the crossfader.
  - If using Radio, try a known CORS-friendly station or use Local File.

- Recording file is silent or very short:
  - Ensure both deck streams are captured (A/B LEDs lit).
  - Hit Start ▶︎ first, then Record.
  - Keep the page in the foreground during recording (mobile power saving can suspend audio).

- BBC/YouTube URLs fail:
  - This is expected due to CORS/DRM. Use Local File or a station that provides direct audio and CORS.

## Roadmap ideas

- Scratch platter gesture per deck (jog velocity/pitch bend or buffered playback-rate emulation).
- Per-deck meters in the top bar.
- “Radio ON/OFF both” buttons in the mixer.
- Deck-local mic recorder with a persistent “Sample Roster” (IndexedDB).
- Parent-only audio engine option (move synth/radio processing to the parent for uniform latency and mixing) while decks become UI-only.

## Development Tips

- Recommend using VS Code with Live Server.
- For changes in deck files, a hard refresh may be needed.
- If you see odd header text or styles, clear cache and reload to avoid stale markup.

## License

- Add your preferred license here.
