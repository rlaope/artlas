# Fermi time-lapse pixel reels ("이거 진짜 해보면?" series)

Format: a 30 s vertical pixel reel. One explorer does an absurd task at one unit per second (a sand grain, a spoonful of seawater). Counters fast-forward hours, days and years until the cosmos ends the job, and the reel loops. There is no voice; chiptune music and SFX carry the audio.

## Series format (keep it across episodes)
- Reuse the same explorer sprite, the HUD pill under the caption, the caption band (UI y ≈ 62–100), one fixed comparison-bubble spot clear of the HUD, a 120 BPM chiptune bed and the iris loop back to frame 0. The episodes should read as one series.
- 30 s beat sheet:
  1. Hook question, on screen from frame 0.
  2. Rule card: "1초에 한 X씩" plus the rule that closes the obvious loophole ("퍼낸 물은 안 돌아와").
  3. Three escalating effort cards, each with a counter spin (1시간 / 1일 / 100일 or 100년). Each reads "…했는데/퍼도 겨우 <familiar object>".
  4. "근데" scale pivot, shown as a comparison (한반도 7개, 롯데타워 6개 반).
  5. "그래서" consequence.
  6. Cosmic twist.
  7. Payoff.
  8. A short punchline that pairs with the earlier episodes' ("…자석 쓸걸", "…그냥 기다릴걸"), then the loop.
- Episodes so far: `needle-desert-20261102` (ep. 1), `ocean-spoon-20261102` (ep. 2) and `password-brute-20261102` (ep. 3, vault room with 3 nested doors, phishing payoff, ending "…그냥 문자 보낼걸"). Start a new episode by copying these:
  - `src/{pix,hero,motion,tl,score,sheet}.py`
  - `assets/galmuri`
  - `tools/build.sh`
  - `research/{llm.py,critique.py,critic_brief.md}`
- Topics offered for later episodes:
  - folding paper 42 times to reach the Moon
  - saving 1원 per second toward a Seoul apartment (KB 2026-08 평균 16억 739만 원; the price rose about 6원 per second over the past year)
  - stacking one Lego brick per second (3.5 km ≈ 4일 8시간, where the bottom brick fails: Open University/BBC 2012)
- A new episode does not need to be another cosmic-time story. Ep. 3 ended on a human shortcut (phishing beats 1억 9천만 년), which gave the payoff a real-world lesson.

## Procedure
1. **Estimate first.** Write `research/estimate.py`. It prints every number with its assumption and source, plus the comparison ratios (object volumes, areas). Use one area/volume basis throughout, because critics flag mixed bases.
2. **Script loop.**
   - Write the draft as a table: time, picture, text.
   - Run `critique.py N draft.md "openai/gpt-6.1-sol,google/gemini-3.1-pro-preview,anthropic/claude-opus-5-5"` in the background. It uses OpenGateway, and `critic_brief.md` lists the owner's dislikes, including zero counts.
   - Merge the changes that two or more critics agree on, plus any arithmetic error any single critic proves.
   - Stop when at least two critics say SHIP, usually after 2–3 rounds.
   - Build sprites while the critics run.
3. **Timing.** `tl.py` is the single source of times. `motion.py` derives the HUD text, years, tool-cycle phase and SFX events from it. `render.py` and `score.py` both import it, so picture and sound cannot drift.
4. **Sprites.** Vision-check a strip of the key poses on a neutral background before building scenes: reach, dip/pinch, lift, release, old with beard, skeleton, holding the prize up.
5. **Stills, then full render.**
   - Render stills at the event times from `tl.py`, not round numbers.
   - Vision-check each one with a precise question, crop to confirm flagged spots, and fix.
   - Run `tools/build.sh vN`. It does the score, the 900-frame render, a static gain to about -14.6 LUFS plus `alimiter` 0.79, and a 2-pass 2200k share copy (about 9 MB). Then it probes, fully decodes and runs ebur128.
6. **Final QA.** Sample 20–30 frames from the encoded share copy.
   - Read every caption and HUD.
   - Count the compared objects at the moment the line naming the count lands.
   - Check the loop frames.
   - Report any leftover the check flags. Do not call the file clean.

## Script rules
- Each "그래서" card's number must follow from the fact on the card right before it. The time to lower the sea 1 mm depends on the ocean's area, not its depth. So say "바다가 워낙 넓어서" before the 1 mm card, and put depth before the total-time card.
- Reading time per card must be at least syllables/7 + 0.4 s, and that includes any competing HUD line. When a readout competes with a punchline, fold it into the caption's second line, revealed on a beat, and hide the HUD.
- Use the same verb in all three effort cards (rule of three). Switch "퍼서" to "퍼도" so each card reads as "even so", not "and then".
- Uncertain science gets "쯤" on screen and the model range in the post caption. The Sun drying the oceans is about 10–20억 years; red giant about 50억 years; the stellar era ends around 10^14 years.
- The post caption holds the rules, assumptions, ranges and sources. It follows the same no-zero-count rule as the screen.
- Topics that IT-literate viewers know (passwords, hacking) need the conditions that make the claim true: no lockout, no repeats, the exact character set (94 = printable ASCII minus space), worst case vs average. Cut any card whose number only holds under a condition that does not fit on screen (the GPU-cracking card was cut by all three critics).
- Show every actor the caption names. "도둑은 문자 한 통 보내지" needed the skeleton visibly sending it (phone in hand, envelope flying out) before the victim's phone appears; otherwise critics flagged the sender as missing.
- The input device must be able to type what the scene claims: a numeric keypad cannot enter `aB3$x9!Q`. Swap it for a wide keyboard (coloured key rows plus an "ABC 123 !@#" tag) when the password set changes, and make any fake login page copy that device.
- The ending line must name what the character actually did on screen. "…그냥 물어볼걸" was read as polite asking when the picture showed phishing; "…그냥 문자 보낼걸" fixed it.

## Picture rules
- Draw compared objects so the number can be counted on screen. Draw a half tub as visibly half. Draw 6½ towers as six plus a seventh cut flat, with a dark base line between stacked towers. Finish any pop-in count before, or exactly when, the line that states it appears.
- A map silhouette under about 20 px does not read as the country. The Korean peninsula at true Sahara-map scale is about 11×16 px, and polygon downsampling turned it into a blob or squiggle. Hand-draw the pixel shape and tag one copy with its name ("한반도").
- Keep the sun or moon arc, the bubbles and the insets out of the caption and HUD band, and pin the bubble to one spot. A low sun passing behind the caption is the usual collision.
- Land a day/night time-lapse on the palette of the scene that follows the cut (noon before a daylight cross-section). Otherwise the cut reads as a pasted card.
- When the scale changes between two differently drawn views (beach close-up and ocean cross-section), use an 8-px block iris. It closes on the explorer and opens on the matching marker in the other view, and the caption swaps at the iris midpoint. A shrinking inset card, or a camera zoom composited from both renders, left card edges and giant-pixel seams after several attempts.
- For the loop, run the iris on the world layer only, then draw the UI on top, switching to the frame-0 caption once the iris passes halfway. Mixing two fully composed frames cuts caption glyphs.
- Never let a HUD counter stop at a value far below the caption's claim ("300년차" under "1억 9천만 년"). Spin it as "N년째 찍는 중", then stamp the claim itself ("남은 시간 · 1억 9천만 년").
- A spinning try counter and the on-screen code must agree: the display shows try count − 1 when it starts at 0000, and counts are floored, never rounded up ("47만 번" at 465,198 tries drew a nitpick).
- A thought bubble must not share the position, size and round shape of a round prop (the vault door read as a porthole). Use a cloud outline beside the head, dots leading to the head, and dim the world behind it.
- Keep dithered glows inside an existing shape. A large-radius `pix.glow` clipped by the frame edge read as a dark dithered box on the phone.
- Small props in front of the character (keypad, keyboard) sit to the right of the helmet, and only the columns the arm reaches comfortably get tapped; a far target turns the kneel into a near-horizontal lunge.
- Phone-size effects need contrast: spray drops at 2×2 px, a gold sparkle with a dark backing, and a red "!" with a dark outline on a bright sky. Even so, a thin water arc behind the character barely reads; let the sparkle carry the "it vanished" beat.

## Rendering setup that worked
- World grid 135×240 (8 px) and UI grid 216×384 (5 px), both NEAREST-upscaled to 1080×1920. Text uses Galmuri11-Bold 12 px for captions and HUD and Galmuri9 10 px for small labels, with `fontmode = "1"` and an 8-way dark outline.
- Sprites are procedural (`Spr` canvas plus `outline()`). The explorer is a hip bend plus a 2-bone IK arm, so a reach to the ground reads as bending over. The tool path is pluggable (tweezers, spoon), and a `cheer_pose()` handles holding the prize up. The tweezers have two prongs, and the needle is 2 px with a dark edge and an eye.
- Dunes are skewed-sine ridges with a cast-shadow line. Shade only a surface band, or vertical colour columns appear.
- Tools: a Python with numpy and Pillow, ffmpeg, and the Galmuri pixel fonts. A 900-frame render takes about 1–2 min on CPU.
- Apply multi-replacement edits as `patch_vN.py` scripts that assert each match occurs exactly once and print a success line. Check that line before rendering: a failed assert aborts before the write, and rendering right after renders the stale file.
- Audio is synthesized in `score.py`: sections follow `tl.py`, SFX follow `motion.events()`, the music hard-stops on the punchline, and the loop pickup starts at 29.5 s. Measured -14.8 LUFS integrated and -1.5 dBFS peak on the share copy.
