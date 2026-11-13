# SMITH • DECK — operating manual and surface contract

`deck.html` is the deck. This file says what each screen is for, how you move between them,
and the rules any new screen or table has to obey.

## Operational diagnostic

| | |
|---|---|
| **Substantives removed** | "integration", "modulation layer", "ecosystem", "experience", "workflow". Each is replaced by the control that does it. |
| **Mental-state verbs removed** | "you will find", "feels like", "intuitive", "understand". Every line of on-screen text is now an instruction or a reading. |
| **The language game** | Every label is a button name or a meter reading. Every sentence on screen has the form *do X → Y happens*. |

## 1. Way

- **Actor:** one player, one or two hands, phone or laptop.
- **Object:** two tables (A, B) and the mix of them.
- **Operation:** touch a table → it sounds. Slide CROSSFADE → the mix moves between A and B.
- **Consequence:** the meter beside each picker moves when that table is in the mix.
- **Completion:** a take exported as WAV, or nothing — playing is allowed to end with no file.

## 2. Obstruction (what the previous host did)

- Mixer controls lived in a long-press menu; the crossfader had no audio to fade.
- Each table showed its own title, help text, readouts and full control panel inside the square.
- Level faders, routing, crate and swap all sat on the main screen and competed with the squares.
- Recording produced a file download with no review, no trim, no stems.
- No way to see the tables before choosing one; names only.

## 3. Keep / cut ledger (DECK screen)

| Element | Claim it makes | Verdict |
|---|---|---|
| Picker A / B | "this table is on this deck; tap to change it" | keep |
| Meter beside picker | "this deck is reaching the mix now" (grey = playing direct) | keep |
| Two squares | "touch here to play" | keep, largest thing on screen |
| ☰ on a square | "this table has controls of its own" | keep, shown only if it has |
| ⛶ on a square | "fill the screen with this table" | keep |
| Two macro sliders | "these two parameters of this table are worth a hand" | keep, shown only if the table exposes any |
| Hint line | "this is the one thing to do next" | keep, empties itself once both decks sound |
| CROSSFADE, A, B | "the mix is here between A and B" | keep |
| PLAY | "start / pause both tables" | keep |
| BPM | "this is the tempo; tap in time to change it" | keep |
| REC | "record from now" / "stop and review" | keep |
| ⋯ | "everything else" | keep, single entry |
| Level faders, filter, delay, limiter, routing | true, but not while playing a table | cut to MIXER |
| Swap, crate, full screen, help | occasional | cut to ⋯ |
| Tap-tempo button, −/+ | duplicates BPM | cut (BPM is the tap button; exact value in MIXER) |
| Table title stamp, help box, readout pills | the table talking about itself | cut (host hides them) |
| Cue buttons | false: one audio output cannot pre-listen | cut |

## 4. State map

```
                 tap A / B                       card + LOAD
      ┌──────────────────────► SURFACES ─────────────────────────┐
      │        ← DECK / Esc / Back                               ▼
    DECK ◄───────────────────────────────────────────────────── DECK (table changed, toast: UNDO)
      │  ⋯
      ├──────► MORE ──► MIXER  (sheet over the deck; DONE / scrim / Esc)
      │           ├──► SAVED PAIRS ── LOAD ──► DECK (toast: UNDO)
      │           ├──► CRATE (sheet) ── → DECK A / B
      │           ├──► LAST TAKE (only if a take exists)
      │           ├──► SWAP A/B ──► DECK
      │           ├──► FULL SCREEN ──► DECK
      │           └──► HOW IT WORKS (coach)
      │  ● REC                    ■ STOP (≥ 0.5 s)
      └──────► DECK·RECORDING ───────────────────► TAKE ── ← DECK ──► DECK
                     │ 8 min reached                │ EXPORT WAV / SHARE / SAVE SESSION stay on TAKE
                     └──────────────────────────────┘
      ⛶ on a square: DECK ⇄ DECK·FOCUS (one table fills the screen; crossfade stays)
```

Every screen is a URL hash (`#surfaces`, `#mixer`, `#pairs`, `#take`, `#crate`), so the phone's Back button
and Esc always return to DECK. `?a=…&b=…` holds the two tables, so any setup is a link.

Deck states and what each shows:

| State | Condition | Hint line | Dominant action |
|---|---|---|---|
| REST | no table has made sound | "Touch a square to make sound. Tap A or B above to change a table." | touch a square |
| DIRECT | a table sounds but the deck's mixer is not running (meter grey) | "Tap PLAY once so the deck can mix both tables." | PLAY |
| ONE LIVE | one table in the mix | "Touch the other square, then slide CROSSFADE." | touch the other square |
| READY | both in the mix | *(empty)* | play |
| RECORDING | REC on | "Recording. Press ■ STOP to trim and export the take." | ■ STOP (shows elapsed time) |
| FOCUS | ⛶ on | — | ⛶ again or Esc |

## 5. Wireframes

```
DECK                                   SURFACES
[A name ▾][▮▮▮▯▯]  [B name ▾][▮▮▯▯▯]   [← DECK] SURFACES [search…] [A name][B name]
┌───────────┐  ┌───────────┐           [ALL][DISCS][PLATES][FAVORITES]
│  table A  │  │  table B  │           DISCS   ▢ ▢ ▢ ▢ ▢ ▢
└───────────┘  └───────────┘           PLATES  ▢ ▢ ▢ ▢ ▢ ▢
 MODE ──●── DRIVE ──●──  …             RING ALBUMS [SHOW ALL 62]
        hint line                                         [LOAD x TO B]
[A] ══════●══════ [B]
[▶ PLAY] [120 BPM] [● REC] [⋯]         TAKE
                                       [← DECK]        decks muted while reviewing
MIXER (sheet)                          TAKE 04                         01:32
[A trim filter delay▮][B …▮]           ruler / MASTER / DECK A / DECK B  ‖ handles ‖
[DELAY RETURN ▮][MASTER limiter ▮]     [▶ PLAY]
ROUTING  [A→B mode amount][B→A …]      [MASTER ▾]  [EXPORT WAV][SAVE SESSION][SHARE]
```

## 6. Void contract

1. The squares are the largest things on the DECK screen at every size.
2. A control appears only while it can act: ☰ only on tables with controls, macro sliders only on tables
   with parameters, LOAD only after a card is selected, LAST TAKE only after a take exists, SHARE only where
   the device can share files, SEND only toward a table with an audio input.
3. Every replace is undoable for six seconds: loading a table, loading a pair, deleting a pair.
4. The one destructive act, recording over an unexported take, asks first.
5. Nothing moves unless something changed: meters move with audio, the playhead with playback.
6. Text on screen is an instruction or a reading. No adjectives.
7. A table that cannot do something says which table can.

## 7. Failure

| Fault | What stays | What is shown | Next action |
|---|---|---|---|
| Deck mixer cannot start (no gesture yet) | tables keep sounding through their own output | meter grey, hint "Tap PLAY once…" | PLAY |
| Table on another site | it plays direct | no ☰, no macros, meter stays dark | load a same-site table to mix it |
| Crate file cannot be decoded | previous record / sample | "COULD NOT LOAD … · previous record kept" on the table | pick another track |
| Target table has no input for a track | nothing changes | "… cannot take a track. Use VINYL, SAMPLER, or a Smith / Puppet / Deco table." | change table |
| GRAB with a silent source | previous sample | "Deck A is silent. Play it, then GRAB." | play, then GRAB |
| Take under 0.5 s | previous take | "Take too short. Nothing kept." | record again |
| Take reaches 8 min | the take | "Take stopped at 8 minutes." → TAKE screen | trim / export |
| moto not served beside smith-deck | everything else | "Crates load when smith-deck and moto are served side by side." | use built-in loops |

## 8. Implementation

- **Capture.** The host patches `AudioNode.prototype.connect` inside each same-origin table frame: anything
  sent to that frame's speakers goes to a bus that feeds a monitor gain and a `MediaStreamDestination`.
  The host mixes the stream and mutes the monitor once its own context is running.
- **Per-deck chain.** in → routing gain → routing filter → trim → low-pass → high-pass → meter → crossfade → mix;
  post-crossfade send → tempo delay (¾ beat) → return → mix; mix → limiter (switchable) → out.
- **Routing.** RING: source audio into the target's gain. GATE / DUCK / FILTER: source loudness (rectify,
  14 Hz low-pass, clamp 0..1) into the target's gain or filter cutoff. SEND: the target frame's
  `getUserMedia` returns a clone of the source deck's stream, and the host presses the table's mic button.
- **Tape.** One `AudioWorklet` reads master, post-fader A and B, and pre-fader A and B. Pre-fader audio fills a
  12-second ring per deck (what GRAB reads). While recording, master (stereo) and both stems (mono) are
  stored as 16-bit blocks; the TAKE screen trims and writes WAV from them.
- **Tables.** Listed in the `T` array in `deck.html`: key, name, disc/plate, URL, design size, preferred macros.
  Thumbnails are `thumbs/<key>.jpg` (240 px).

### Changes after the first phone test

- **☰ opens the table's own controls full screen**, at real size, with ← DONE. Inside a small square they could not be used.
- **LINK** is on the deck's main row. It names what each link does in one sentence and shows the active link on the button
  (`A→B GATE`). The same routing is still in MIXER.
- **Both squares take fingers at the same time.** A browser gives the second finger no touch events when it lands in a
  different frame. A transparent layer in the deck now receives every finger and re-dispatches pointer, touch and (for an
  unclaimed tap) click events into the table underneath. Tables that need real touches for native widgets
  (SMITH MORPH, MORPH FIELD) opt out; any table gets real touches while its controls are open.
- **The bottom row always fits.** The deck measures its real box (visual viewport minus safe-area padding, every grid gap
  counted) instead of assuming one. A phone on its side gets a one-row control bar.
- Focus (⛶) keeps PLAY / BPM / LINK / REC visible.

### Changes after the second phone test: two decks, and OPEN

- The deck screen is **only the two squares**, the crossfade and one row of buttons. Macro sliders and the focus button are gone.
- **OPEN** on a square shows that table whole: full screen, unscaled, nothing hidden, real touches, with all of its own
  menus and controls. The other deck keeps playing. The crossfade stays pinned underneath. **← BOTH** returns;
  **OPEN A / OPEN B** jumps straight to the other deck.
- While a table is open, a tall control panel is capped at 46 % of the height and scrolls, so the chart is never squeezed out.

### Table contract (to add a table)

Minimum: one page whose playing surface is a canvas that follows the window `resize` event and sends its
sound to `audioContext.destination`. Then add a row to `T` and a thumbnail.

Optional, each unlocks one deck feature:

| Provide | Deck feature |
|---|---|
| controls inside `#controls-container` | ☰ opens them; the first two range inputs become macro sliders |
| `window.scionDeck.manifest()` / `set(id, v)` / `get(id)` with params in 0..1 | macro sliders bound to named params |
| `scionDeck.start()` / `stop()` / `setBpm(bpm)` | PLAY and tempo reach the table |
| `scionDeck.loadUrl(url, name)` or a `#radioSource` select | CRATE can put a track on it |
| `scionDeck.loadAudio(channels, sampleRate, name)` and `parent.postMessage({type:'scion-grab', bars:4})` | GRAB the other deck into it |
| a `#micInputToggle` button that calls `getUserMedia` | it can receive SEND |
