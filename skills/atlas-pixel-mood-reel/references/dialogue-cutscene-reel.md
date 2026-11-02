# Pixel game-cutscene reel (dialogue box + scene transition + voice or song)

For requests like "Skul-style pixel game view, two characters, a dialogue box, then a scene change" as a vertical 9:16 reel, either voiced or cut to a song. Each reel lives in its own scratch project (`src/ refs/ assets/ frames_* audio/ out/ qa/`, `prep.py`, `tl.py`, `render.py`, `mix.py`). Keep the finished project (without stills and raw video refs) alongside the delivered file.

## Pipeline

1. **Story beats first.** List the scene's defining beats in order and pin each to a song time. Include the cause of the emotion (the fatal blow, the sacrifice) as its own beat. Typical 30 s shape: dialogue 0-9 s, action 9-14 s, quiet break with a build, the drop as the climax, then the aftermath, last line and title.
2. **Reference stills.** Try sources in this order:
   - Fandom MediaWiki API: `api.php?action=query&list=search&srsearch=...&srnamespace=6&format=json`, then `prop=imageinfo&iiprop=url`. static.wikia.nocookie.net needs a browser User-Agent plus `Referer: https://<wiki>.fandom.com/`. Some wikis sit behind a Cloudflare challenge ("Just a moment...") that neither curl nor the browser harness passes, so move on.
   - animationscreencaps.com: numbered full-HD caps per film at `caps2.b-cdn.net/<id>/<slug>/full/<slug>-animationscreencaps.com-N.jpg`. They need a browser UA plus `Referer: https://animationscreencaps.com/`. Sample every 60th cap, find the scene on a numbered contact sheet, then sample every 10th around it.
   - Official PV, trailer or music video. Fetch with `yt-dlp -f "bv*[height<=720]"`, extract `fps=1` frames, tile them into numbered contact sheets, and pick seconds with vision. Then grab single frames with `ffmpeg -ss S -frames:v 1`. This works for films the caps site lacks.
   - Official figures or statues of the scene make useful pose references too.
3. **Reference boards for image edits.** image_generate edits take a single PNG input. Build a 1024 px magenta board from cropped film stills (or from approved sprite cells), quantize to 64-96 colours, and keep it under ~450 KB. JPEG inputs fail with "'image' is required", and large inputs return 413.
4. **Assets from the host image tool:**
   - Portrait backgrounds (1024x1536) with the walkable floor line at an explicit % height.
   - Square bust portraits.
   - One 2x2 sprite sheet per character per pose family on flat #FF00FF.
   - Couple or interaction sheets for shared beats.
   - Effect sheets such as fire, a dragon or a slash.
   In every sheet prompt ask for "same scale, same feet baseline, each figure fully inside its cell with a 10-12% margin".
5. **Sprites through sprite-forge.** Run `forge.py process --rows 2 --cols 2 --key FF00FF --tolerance 60` into a new directory and quote body_scale_cv. Then run `pixel_sprite()` from the first reel's prep.py: erode 2 px of magenta spill, BOX-downscale (factor 4-6), quantize to ~40-48 colours, and add a 1 px dark outline. Vision-check every extracted strip on a neutral background before using it. The QC numbers miss anatomy errors such as a duplicated shoe or a missing sword; drop the bad frame rather than editing it.
6. **One grid for everything.** Compose the world on one logical grid and the UI at 360x640 (3 px), then upscale with NEAREST. Pixelize backgrounds the same way (BOX down, MEDIANCUT 72-96 colours). Use 270x480 (4 px) for wide shots, or 216x384 (5 px) when the characters need to read bigger. Make `up()` scale by `W // grid_width`.
7. **Korean pixel font: Galmuri.** Use Galmuri14 @15 px for dialogue and Galmuri11-Bold @12 px for name tags. Draw with `fontmode = "1"`. Galmuri9 @10 px is unreadable at phone size. Galmuri11-Bold has no CJK, and Galmuri14 lacks some kanji (for example 煉), so set Japanese card text in Galmuri11 regular (13-15 px, 1 px fake bold). Check glyph coverage first: render each character and compare its ink against an undefined codepoint's tofu.
8. **Voices (only when wanted):** `edge-tts` in a private venv, ko-KR-SunHiNeural / ko-KR-InJoonNeural, `--rate=-10%` with a slightly negative pitch. Measure segments with `ffmpeg silencedetect=noise=-40dB:d=0.15` and spread the typewriter reveal across each one. Without voices, time the reveal to the song's phrases.
9. **Shared timeline module** (tl.py) imported by the renderer and the audio script. Every cue lives there. For a revision, write tl2.py and render2.py that import and override the previous ones, so earlier exports stay reproducible.
10. **Audio.** Use either a synth score or the user's song (next section). Use one dialogue tick everywhere and duck the song ~6 dB while a box is open. Check the tick onset contrast (median ≥ +3 dB). Add a sub-boom on the drop and short metallic clangs on sword impacts. Finish with `alimiter` and two-pass loudnorm to -14 LUFS / TP -1.5.
11. **Verify the encoded file.** Check ffprobe stream counts and a full `-f null` decode. Run an ebur128 integrated-loudness check. Tile frames decoded from the MP4 for a vision pass that covers every beat. Measure dead air per section: decode gray 135x240, then look at the share of consecutive frames whose mean absolute difference is below 0.25. A talk scene above ~75% still needs idle life (1 px breathing bob, alternating talk poses, drifting smoke, a slow camera move).

## Cutting to a song

1. **Lyric timing:** `curl -A Mozilla/5.0 "https://lrclib.net/api/search?q=..."`. It sometimes answers 503 ServerOverloaded, so retry with a short sleep. Use entries whose duration matches the downloaded file (movie edit vs album cut, or a re-arranged "MOVIE version" with different timing), and require two entries to agree within ~0.3 s.
2. **Audio source:** run the latest yt-dlp from the project venv (`pip install -U yt-dlp`, because a stale build gets 403). Use `yt-dlp -x --audio-format wav`, and prefer the label or official upload whose length matches the lrclib entry.
3. **Map the structure on the file itself.** Print full-band RMS in 0.25-0.5 s steps to find the quiet break before the last chorus. Then run low-band (<150 Hz) onset detection in 10-50 ms hops to place the drop, which can trail the lyric stamp by ~0.4 s. Get the beat period from spectral-flux autocorrelation. Choose the song offset so the drop lands on the climax cut, and shift every lyric-tied cue by the same delta.
4. **Cut and normalize:** `ffmpeg -ss OFFSET -t DUR -i song.wav -af "afade=t=in:st=0:d=0.3,afade=t=out:st=DUR-1.4:d=1.4"`, then loudnorm, then mux with `-c:a aac -b:a 192k -ar 48000`.
5. **Deliver** with the song embedded. Mention the copyright-mute risk once and give the in-app start point as the fallback.
6. **Discord attachment:** a CRF 23 master is ~14 MB for 30 s at 1080x1920. Re-encode the share copy at CRF 26 (~8-10 MB).

## Sprite sheets and staging

- Keep one pose family per 2x2 sheet: standing, kneeling, running, flying, combat. Mixing families fails QC (body_scale_cv 0.08-0.36). A 4-frame run needs contact, passing, opposite contact and passing named explicitly, or the model draws the same stride four times.
- To QC a single row of a mixed sheet, paste its two cells into a padded 2x2 grid (each cell placed in both rows). The extracted frames then come out A, B, A, B, so the distinct poses are indices 0 and 1, not 0 and 2.
- When a blade or flame crosses the cell midline (forge refuses for bleed), move whole connected components into a wider grid and keep each figure with its own small components. Never edit pixels; a redraw belongs to a new sheet.
- Anchor interacting sprites by a feature point measured on the asset, such as the reaching hand or the clasped hands, not by the sprite centre or feet. That is what makes "hands meet" land on the exact frame.
- Two dark-haired characters touching heads merge into one blob at phone size. Tint one character's hair (teal) and keep a 1 px dark outer ring around both.
- Tears, sparks and impact marks must be at least 2x2 logical px with a dark backing, or they vanish at phone size. Check them on a crop of the encoded frame, not on the world layer.
- Keep the hero readable through an effect. Draw flames and auras behind him and clip foreground effects to rows below the feet line. Draw hit flashes under the fighters and redraw the fighters on top.
- A fleeing or leaping character needs its own airborne sprite, an arc that leaves the frame, and a dark-backed motion trail. Sliding a standing sprite reads as a teleport.

## Effects

- Fire: use a 4-frame flame sprite sheet (sprite-forge PASS) at the same pixel density as the characters. Dither-fade the bottom ~22% of each flame so it roots into the ground, then stack flames in rings and rows with per-flame frame offsets. Numpy per-pixel fire simulations cost several rounds and still read as horizontal streaks, flat slabs or a stamped grid at this scale.
- Big signature attacks (a dragon, a beam): generate them as an effect sprite on magenta, key it, pixelize to ~200 px wide, and sweep it with a 2x integer upscale. Add a short white-hot flash on the drop and a white-out into the next shot.
- Light (bloom, god rays, rim) goes in as quantized light fields through a 4x4 Bayer (`floor(g*levels + bayer)/levels`), not as alpha-blended polygons. Use at least 8 levels for large soft glows, because 4 levels show a hard ellipse. Never tint the whole frame with a flat dither, which reads as a grey or beige wash.
- Mask rays and flare spikes to the sky; anything crossing a planet or face reads as a grey wedge or a reticle.
- Vision QA flags these as cheap: thin 1 px shockwave rings, hexagon lens-flare rows, a uniform `rimmed()` glow on a busy background (sticker outline), and a light source parked behind the faces.
- Gate every effect by its section's time window. A state function that keeps returning values after its section leaked a hit-flash into a later shot.
- Transitions: grow an 8 px-grid iris from the target (a moon, a torii) to white, then open from white. A full-screen dither reads as noise and white-mix fades look grey. When time of day changes at the same place, composite the new sky onto the same plate with a luminance mask above the treeline, and keep landmarks such as the wreck. Swapping plates reads as a new location.

## Making a chorus drop feel grand

- Give the drop its own shot with extreme scale contrast (an earthrise, a dragon across the frame, a burst out of the clouds), and keep the motion going at the cut. Easing to a stop reads as a stall.
- Keep the energy holding or swelling through the shot. Peaking on frame 1 and decaying reads as a deflating drop.
- Put a short sub-boom (72 to 38 Hz sweep, ~0.55 s decay) on the drop kick, under the song.

## Pitfalls

- Extending a backdrop downward or upward for a camera move: extend with a mirrored, darkening copy of the edge rows, and blend the seam per column. Otherwise a hard horizontal cut shows.
- In a rush zoom, keep sprites in screen space and shrink them gradually with ease_in. An ease_out shrink pops in a single frame.
- Keep empty foreground bands under ~15% of frame height. Lower the camera, or fill the band with foreground scenery, rather than leaving a dark ledge.
- Tinted sprite echoes on a slow vertical drift pile up into a smoke-like cloud. Slow motion reads through a slow camera, glitter with a slower twinkle, soft light beams and a particle stream.
- Sample QA stills at computed event times (impacts, catches, flee) from tl.py. Stills taken at round numbers can all land in the same phase of a periodic action and miss every impact.
- Give every dialogue box at least ~1 s of hold after the last character types, and finish typing before a cut.
- Do not leave the fight, special-move or escape beats silent (user feedback: "싸울때나 필살기 도망갈때 대사없네"). Put a short line on each:
  - Mid-fight taunt and retort.
  - A vow during the build-up, typed out before the skill-name card appears.
  - A cry from the loser as he breaks free.
  - A shout from the witness as the enemy flees.
- Lines over action should fit one 15 px row (under 208 px), type in 0.4-0.7 s and hold about 0.5-0.8 s. Give each box its own song-duck depth: about 2 dB over the chorus so the drop keeps its energy, about 6 dB over quiet talk.
- Set the blip level per box by blip-to-mix ratio (the ticks-only track against the rest of the mix, 300-2500 Hz, 30 ms after each character), not by the onset jump. The onset jump under-reads on a loud chorus.
- The write_file tool refuses to overwrite a file that a terminal heredoc patched since the last read. Read it (all pages) first, then rewrite.

## Fight choreography (user feedback: "too clang-clang, stuck together, unnatural motion")

- Do not drive a fight with one repeating dash/impact/recoil cycle per two beats. Write a keyframe track per fighter in a shared module (fight.py: time, torso x, pose, ease, hop, dy) plus event lists (HITS with hit-stop length, SLASHES, DASHES, SKIDS, LANDS). Use it for both render and mix so every sound lands on its frame.
- Place sprites by measured torso x (the median column of the 40-65% body band), not the sprite centre. Lunge and punch sprites are asymmetric, so centre placement makes bodies overlap. Choose contact spacing so the weapon tip just reaches the other body. Check it every frame with a per-row silhouette gap test (min row gap >= -3 px) and an on-screen edge test.
- Make every exchange different: hops, a leaping strike with real air height (dy -20 or more), a whiff, a big clash, a two-hit flurry. Give both fighters knock-back (20-30 px skids with dust), not only the defender of the moment.
- Use mild acceleration on dashes (u**1.6 or linear). A cubic ease-in puts most of the travel in the last two frames and reads as a teleport. Add speed lines and 3 afterimages once speed passes ~60 px/s.
- Hits: 2-frame opaque white flash on the defender only, a shudder through hit-stop, a warm star with a dark backing offset onto the contact edge, and arcing 2x2 sparks with gravity. For the big clash, dim the world (not a grey wash), ease the light back over ~0.4 s, then cut to a recoil pose before the push-apart.
- Sound: replace the metallic clang (inharmonic 1-4 kHz partials) with a layered impact: a pitch-dropping body thump (~120 to 50 Hz), a short band-passed noise crack, a low-mid body thud and only a faint dull ring. Dip the song 3-5 dB for ~0.2 s under each hit, and add whooshes on dashes, a fire whoosh on slashes, skid scrapes and landing thuds.

## Fight SFX quality loop (user feedback: "효과음 더 고퀄리티, 피드백 루프 돌려")

- Synthesized hits are not enough. Build them from recorded CC0 one-shots:
  - Body: a punch thud (OpenGameArt "37 hits/punches", Kenney impactPunch).
  - Blade: a high-passed katana contact transient sliced from medieval weapon-on-weapon recordings, not a ringing clang.
  - Sub: a short pitch-dropping sine.
  - Tail: a short noise-IR reverb.
  - Dashes use recorded swishes pitched down. Slashes use a swish plus a fire-staff burst. The drop uses a recorded fire explosion plus sub.
- Sources that worked: Kenney zips need a signed link from `https://kenney.nl/download-link/<data-download-id>` (fetch it with the page cookie and Referer). OpenGameArt pages state the license and expose `sites/default/files/*.zip|7z` (bsdtar unpacks 7z). Openverse timed out.
- OpenGateway chat models refused every audio input shape with HTTP 400, so there is no remote listening judge. Use local judges in a separate venv (torch + transformers):
  - AST (`MIT/ast-finetuned-audioset-10-10-0.4593`) gives AudioSet labels.
  - CLAP (`laion/clap-htsat-unfused`) gives similarity to role prompts versus "cheap synthesized beep", "white noise" and "metal pan clanging". It ranked the v1 clang as Ding/Clang/cheap.
  - `get_*_features` can return a ModelOutput, so take `.pooler_output`.
- Loop structure:
  1. Audition the candidates.
  2. Random-search the layer choice, pitch, levels, filter and drive, scored both isolated and placed into the song.
  3. Match the official PV's hit band profile (sub/low/mid/presence) and transient crest (~8.6 dB).
  4. Sweep SFX level and song dip on full renders, with an onset-jump and music-masking guard.
- Gain-stage before the limiter (loudnorm-measured pre-gain, limit -2 dBFS, attack 2 ms, release 80 ms). Otherwise the hit crest gets flattened from 8.7 to 7 dB.
- Rotate body and blade samples per hit so seven hits are not one copy.

## Wording in these requests

- In a game-cutscene request, "보이스폰트" means the text blip sound ("띠디디딕") that plays as dialogue types out. It does not mean a TTS voice. "음성은 빼고" removes spoken voices only, and the blip is still wanted.
- Use one blip sound everywhere, at one level, with no per-character pitch and no random pitch steps. Varied pitch was rejected as unnatural. Leave out box open/close/swap chirps unless asked.
- "보이스폰트가 귀가 아프다" means the blip is too harsh. A square wave at ~1.5 kHz with a 2 ms attack spikes the 2-5 kHz band, where hearing is most sensitive: +2.7 dB on every character, up to +7.6 dB.
  - Default to a triangle at ~587 Hz (D5) with a smooth 4 ms S-curve attack, a 14 ms decay and a 2.2 kHz low-pass. It keeps the "띠디딕" character.
  - Set its level by A-weighted loudness, then trim it until audibility in the mix matches the old blip.
  - Result: the 2-5 kHz spike dropped to about 0 dB, and the blip stayed audible (+3.3 dB vs +2.6 dB).
  - Measure it in the mix at the real character times, not on the isolated blip.
- "음악 넣고" means mixing the actual song into the file, not leaving it for the user to add in the app.
- "애니명장면 추천" wants a short list of famous scenes with the iconic line and a matching song for each, grouped by mood. Lead with the one that fits the previous reel's structure, then ask which one to make.
