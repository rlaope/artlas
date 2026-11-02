# Guide: making an Atlas reel

This guide walks through one piece from request to delivery, using the Lucy clip (`docs/gallery/lucy-chair.jpg`) as the worked example. The rules behind every step are in [`skills/atlas-pixel-mood-reel/SKILL.md`](../skills/atlas-pixel-mood-reel/SKILL.md).

## 0. Install the skill

With Hermes Agent:

```bash
git clone https://github.com/rlaope/atlas.git
mkdir -p ~/.hermes/skills/creative
cp -r atlas/skills/atlas-pixel-mood-reel ~/.hermes/skills/creative/
```

Then ask for a reel in plain words. The skill loads on requests like "make a dark pixel clip of Lucy in her armchair, with I Really Want to Stay at Your House".

Tools the procedure uses: Python 3 with numpy, scipy and Pillow; ffmpeg and ffprobe; yt-dlp in its own venv; an image generator that can do text-to-image and image edits (outpainting).

## 1. Write the spec

Turn the request into a short spec before generating anything. For Lucy:

- **Scene:** Lucy, after David's death, alone in her apartment at 3 a.m. Head thrown back, forearm over her eyes, a cigarette in that hand, legs crossed.
- **Must read on a phone:** Lucy as the subject. The TV playing the news of David's death. Two crumpled moon-trip tickets on the side table.
- **Light:** the TV is the one light, cold white-blue. The window neon is only a rim.
- **Darkness:** everything the TV does not reach.
- **Motion:** the news on the TV (stuttering), its light on her, smoke, ember, breathing, hair tips, a slow tear, a car passing outside.
- **Song:** "I Really Want to Stay at Your House", the hook at about 10 s.

## 2. Make the base still

Generate a TV-anime key frame at 1024x1536. Name the camera height ("armrest height, oblique side view"), the pose word for word, the light and where it falls, the room behind her, and the TV screen as a flat `#00FF00` placeholder. Generate two candidates, pixelize both, and compare them at phone size.

Common fixes:

- **The TV is cut by the frame edge:** pad the canvas with blurred, dimmed copies and outpaint ("the blurred areas are empty space; keep everything inside the sharp picture").
- **A prop is the wrong size:** scale its own pixels about its contact point and feather it back. Do not paste a separately drawn sprite.
- **A mouth is drawn:** refill it from a plane fit of the surrounding skin, on the source, before downscaling.

## 3. Pixelize

Crop to 9:16 and BOX-downscale to 216x384 (x5 on output). Quantize to about 160 colours with no dither, then replace isolated pixels. Bring the face and the tickets through with a sharpened LANCZOS downscale blended softly, so features survive without looking pasted.

Check: the dots are visible at phone size, and the face still reads.

## 4. Light and darkness

- Build the TV light from the drawing's own lit planes, and fade it with distance from the screen.
- Darken the base away from the source, and darken the far side of her body one step.
- Pull the green placeholder's spill back to neutral.
- Measure region means in code (face, legs, wall, floor) so the light is visible on her and absent on the back wall.

## 5. Motion

- **TV:** the broadcast is a generated still with a slow push-in and cuts. Make it stutter: 12 fps, short freezes, a row tear, a vertical roll with a black bar, a flash of snow. The room light dims with each glitch.
- **Car:** a passing car over 10 s. Amber light slides along the blind slats, and the slatted light shape crosses the side wall and the floor in opposite directions, with soft edges.
- **Hair:** sway the tips with a sub-cell displacement weighted from 0 at the scalp to 1 at the tips.
- **Tear:** a drop forms under the forearm, slides down a measured cheek path over 8 s, leaves a faint wet trail, fades at the jaw and repeats.
- **Also:** particle smoke from the ember, a slow drag on the ember, and a 1-cell breathing rise.

## 6. Sound

Fetch the official upload. Find the hook on the file itself (lyric times, then the vocal-band onset), and cut so it lands near 10 s. Build the room path for the chosen space: speaker character, reflections, pre-delay, late tail and air. Keep the song clearly above the bed. Add close effects separately. Finish with a limiter and two-pass loudnorm to -14 LUFS.

## 7. Check and deliver

- ffprobe the streams and frame counts, run a full decode, and run ebur128.
- Look at a decoded frame strip, and at one phone-size frame against the four core rules.
- Fix what can be fixed. Report as remaining only what this base cannot fix, with the reason.
- Keep the share copy under 10 MB.

## Feedback this style was built from

- "The TV became the subject": make the character the subject, the TV smaller and turned toward her.
- "Where is the background?": a dark room still has a readable interior.
- "The tickets look too big": draw props at their natural size.
- "Make the TV stutter, let the car light pass very slowly, make the hair move softly, the tear should run slowly": every one of these is now motion that changes slowly and continuously.
- "Not sweet, gloomy": one cold source, deep shadows, one warm accent at most.
