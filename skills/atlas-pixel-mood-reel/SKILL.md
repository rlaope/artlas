---
name: atlas-pixel-mood-reel
description: "Use when making a dark pixel-anime mood reel in the Atlas style: one light, gloom, high-quality dots, a song."
version: 1.0.0
author: rlaope (khope), Friren, Kami
license: MIT
metadata:
  hermes:
    tags: [pixel-art, anime, reel, video, lighting, audio, mood]
    related_skills: [pixel-art]
---

# Atlas pixel mood reel

Short vertical clips (15-30 s, 1080x1920, 30 fps). A character moment from an anime is redrawn as a still pixel scene. Light, smoke, a screen, hair and tears move, and a named song plays inside the room. The gallery in this repository's README shows the finished pieces this skill was distilled from.

## The core: these four come first

Every decision is judged against these. When a later rule seems to conflict with one of them, the core wins.

1. **High-quality dot art, and the dissonance it creates.** The frame reads as pixel art at a glance, on a phone: the grid is coarse enough to see. Underneath it sits a TV-anime key frame, with smooth anime shading, many colours and clean forms. The tension between the coarse dots and the high-quality drawing is the look. The character uses the same grid and texture as the room; a sharper or flatter character reads as pasted.
2. **Gloom.** A dark room at night, decadent and lonely. No sweet, pastel or candy grading. When a colour word is ambiguous, keep the gloomy reading and say how you read it.
3. **Light staging.** One light source (a monitor, a TV, a window). The shape the light makes on the face, the hands, the smoke and the floor must be visible, and it changes slowly: a screen that shifts colour, a car's headlights crossing the blinds over ten seconds.
4. **Darkness staging.** Whatever the light does not reach sinks into darkness. The far side of the body, the back of the room and the corners stay near black, while silhouettes stay readable.

## Fixed rules

- **Motion every second.** Something moves at all times: the screen picture and its light, the ember, smoke, hair tips, breathing, a blink, a tear running slowly down the cheek. A screen in the scene may stutter on purpose, like an old TV with a bad signal.
- **The character and the story.** Use the canonical outfit, exposure included, for adult characters. Anything the request centres on (for example the news report of a death on the TV) must read on a phone. The character is the subject of the frame, not the props.
- **Composition.** No ceiling-down camera. Leave empty dark space above the head. Keep props few. Keep named elements out of the Reels UI: the right-edge button column and the top and bottom bands.
- **A real room.** Dark does not mean empty. The background is a readable interior in the dark (a window with neon blinds, pipes, a shelf, a poster).
- **Working method.** Before rendering, check one phone-size still against every rule above. Fix every weakness that can be fixed before delivering. List as "remaining" only what truly cannot be fixed in this base, with the reason. In a feedback loop, read the other collaborator's review posts as well.

## Defaults that change per piece

These worked so far. Treat them as starting points, not rules.

- **Face.** The default is a mouthless dot face: eyes only, drawn on the same grid as the face, irises that reflect the screen rather than glow, matching catchlights in both eyes, gaze toward the screen. A closed eye is a curved lash line with a skin-coloured lid that follows the eye's shape, never a filled rectangle. Draw a mouth when the scene needs it (a canonical death face, a scream).
- **Music.** The named song, embedded in the file. The cut usually puts the hook about 10 s into the clip.
- **Space.** The song usually plays from a device inside the room (PC speakers, a TV, a laptop in a closet, a radio behind a door). Pick the treatment that fits the mood: across the room, through a wall, far away. Keep the song clearly audible and do not shave off the top end.
- **Sound effects.** Mixed close and separate from the song (lighter, fan, rain, a car passing at the same time as its headlights). Avoid short periodic transients such as clock ticks; under reverb they smear into a recurring patter.
- **Loudness.** -14 LUFS integrated, true peak -1.5 dB, as the default finish.

## Pipeline

1. **Merge the request into one spec.** Requests often arrive as a burst of short messages; later messages override earlier ones. Write the beats and what must read on a phone.
2. **Base still.** Generate a TV-anime key frame (9:16, 1024x1536) that states the camera height, the pose literally, the one light source and where it falls, the room behind, and any screen as a flat `#00FF00` placeholder inside its bezel. Generate two candidates, pixelize both, and pick by a side-by-side read at phone size. Fix framing with outpainting (blurred, dimmed pads as the space to fill), never with mirrored or tiled pads.
3. **Pixelize.** Crop to 9:16, BOX-downscale to the grid (180x320 x6 or 216x384 x5), quantize to about 110-160 colours without dither, and replace isolated pixels. Bring the face, hands and small props through from the source with a sharpened LANCZOS downscale blended in softly. Remove a drawn mouth on the source before the downscale (plane fit of the surrounding skin).
4. **Light.** Build the light map from the drawing's own lit planes and recolour at constant luminance. Darken the base away from the source. Check region means in code, not only by eye.
5. **Motion.** Screen content (real footage or a generated broadcast mapped into the screen quad), room light following the picture, particle smoke, sub-cell hair sway pinned at the scalp, breathing, a tear sliding along a measured cheek path, slow headlight passes.
6. **Sound.** Fetch the official upload, time the hook from lyrics and the file's own onsets, build the room path, add the close effects, and loudness-normalise. You cannot listen, so prove intelligibility with measurements (C50, T30, syllable-envelope correlation, transient scan).
7. **Verify.** ffprobe, a full decode, ebur128, a decoded frame strip and one phone-size frame. Keep the share copy under 10 MB.

## References

- `references/anime-pixel-clips.md`: the full procedure and recipes for these clips: base prompts, pixelize settings, eye, blink, mouth and smoke recipes, light maps, colour swings, headlights, the choppy TV, songs and spatial audio, and delivery checks.
- `references/dialogue-cutscene-reel.md`: game-cutscene reels with dialogue boxes, sprites, fights and a song.
- `references/fermi-timelapse-reel.md`: the "absurd task, one per second" chiptune series.
- `references/ascii-glyph-scene-recreation.md`: recreating an ASCII-glyph night scene with a pixel sprite.
