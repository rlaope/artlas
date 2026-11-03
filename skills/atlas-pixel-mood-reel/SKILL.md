---
name: atlas-pixel-mood-reel
description: "Use when making a dark pixel-anime mood reel in the Atlas style: focused light, gloom, high-quality dots, a song."
version: 1.1.0
author: rlaope (khope), Friren, Kami
license: MIT
metadata:
  hermes:
    tags: [pixel-art, anime, reel, video, lighting, audio, mood]
    related_skills: [pixel-art]
---

# Atlas pixel mood reel

Short vertical clips (15-30 s, 1080x1920, 30 fps by default; use the requested frame rate). A character moment from an anime is redrawn as a pixel scene. Light, smoke, hair, breathing and expressions move, and a named song plays inside the room. The gallery in this repository's README shows the finished pieces this skill was distilled from.

## The core: these four come first

Every decision is judged against these. Explicit requester instructions take priority; otherwise these guide later defaults and recipes.

1. **High-quality dot art, and the dissonance it creates.** The frame reads as pixel art at a glance, on a phone: the grid is coarse enough to see. Underneath it sits a TV-anime key frame, with smooth anime shading, many colours and clean forms. The tension between the coarse dots and the high-quality drawing is the look. The character uses the same grid and texture as the room; a sharper or flatter character reads as pasted.
2. **Gloom.** A dark room at night, decadent and lonely. No sweet, pastel or candy grading. When a colour word is ambiguous, keep the gloomy reading and say how you read it.
3. **Light staging.** Default to one dominant source (a monitor, a TV, a window). Allow a secondary source when explicitly requested, while keeping one visual focus. The shape the light makes on the face, the hands, the smoke and the floor must be visible, and it changes slowly: a screen that shifts colour, a car's headlights crossing the blinds over ten seconds.
4. **Darkness staging.** Whatever the light does not reach sinks into darkness. The far side of the body, the back of the room and the corners stay near black, while silhouettes stay readable.

## Fixed rules

- **Candidates first.** Post each new base with a stable ID such as A or B, captioned outside the artwork, and let the requester pick before building on it. Record the accepted filename, SHA256 and a distinguishing feature in the spec so display order cannot select a different picture. Code-only fixes and aligned motion plates for that same approved base need no new choice; a redesigned face or composition does.
- **Motion every second.** Something moves at all times: the screen picture and its light, the ember, smoke, hair tips, breathing, a blink, a tear running slowly down the cheek. When the request calls for fluid character animation, the body, head, hair or expression must visibly act at phone size. Changing smoke or grain alone does not meet that request. A screen in the scene may stutter on purpose, like an old TV with a bad signal.
- **The character and the story.** Resolve the named character and their canonical silhouette before prompting. Use the canonical outfit and proportions unless the requester directs otherwise; preserve their choice rather than substituting an original character or adding an unrequested redesign. Anything the request centres on (for example the news report of a death on the TV) must read on a phone. The character is the subject of the frame, not the props.
- **Composition.** No ceiling-down camera. Leave empty dark space above the head. Keep props few. Keep named elements out of the Reels UI: the right-edge button column and the top and bottom bands.
- **A real room.** Dark does not mean empty. The background is a readable interior in the dark (a window with neon blinds, pipes, a shelf, a poster).
- **Text follows the request.** Default to full-frame artwork without added titles, caption bars, lyrics or decorative labels. Add wording only when it belongs to the requested scene or the requester asks for it; a request for no text also removes title framing from alternate exports.
- **Working method.** Check a phone-size still before rendering, then review actual decoded motion using the [feedback loop](references/feedback-loop.md). Fix every weakness that can be fixed before delivering. List as "remaining" only what truly cannot be fixed in this base, with the reason. Read the other collaborator's review posts as well.

## Defaults that change per piece

These worked so far. Treat them as starting points, not rules.

- **Face.** The default is a mouthless dot face: eyes only, drawn on the same grid as the face, irises that reflect the screen rather than glow, matching catchlights in both eyes, gaze toward the screen. A closed eye is a curved lash line with a skin-coloured lid that follows the eye's shape, never a filled rectangle. Draw a mouth when the scene needs it (a canonical death face, a scream).
- **Music.** The named song, embedded in the file. With no requested start point, the cut usually puts the hook about 10 s into the clip. A requested timestamp or phrase determines the start instead; verify it on the actual source file.
- **Space.** The song usually plays from a device inside the room (PC speakers, a TV, a laptop in a closet, a radio behind a door). Pick the treatment that fits the mood: across the room, through a wall, far away. For a muffled wall or karaoke-room effect, roll off the highs while preserving vocal intelligibility; use the spatial-audio measurements in the reference.
- **Sound effects.** Mixed close and separate from the song (lighter, fan, rain, a car passing at the same time as its headlights). Avoid short periodic transients such as clock ticks; under reverb they smear into a recurring patter.
- **Loudness.** -14 LUFS integrated, true peak -1.5 dB, as the default finish.

## Pipeline

1. **Merge the request into one spec.** Requests often arrive as a burst of short messages; later messages override earlier ones. Write the beats and what must read on a phone.
2. **Base still.** Generate a TV-anime key frame (9:16, 1024x1536) that states the camera height, the pose literally, the dominant light and any requested secondary source, where each falls, the room behind, and any screen as a flat `#00FF00` placeholder inside its bezel. Generate two candidates, pixelize both, and pick by a side-by-side read at phone size. Fix framing with outpainting (blurred, dimmed pads as the space to fill), never with mirrored or tiled pads.
3. **Pixelize.** Crop to 9:16, BOX-downscale to the grid (180x320 x6 or 216x384 x5), quantize to about 110-160 colours without dither, and replace isolated pixels. Bring the face, hands and small props through from the source with a sharpened LANCZOS downscale blended in softly. Remove a drawn mouth on the source before the downscale (plane fit of the surrounding skin).
4. **Light.** Build the light map from the drawing's own lit planes and recolour at constant luminance. Darken the base away from the source. Check region means in code, not only by eye.
5. **Motion.** Choose the acting beats, evaluate each output timestamp and render a short proof covering their peaks and transitions. Use the reference's character-motion recipe when stronger acting is requested; keep the pixel grid and static room coherent.
6. **Sound.** Fetch the official upload, verify the requested start against that file's own onsets, build the room path, add close effects separately, and loudness-normalise. When listening is unavailable, report measurement evidence (C50, T30, syllable-envelope correlation, transient scan) and mark listening as not observed.
7. **Verify and deliver.** Complete the feedback loop on the exact final MP4: ffprobe, full decode, ebur128, decoded motion and phone-size inspection. Preserve the approved master and the requester's quality/version rules. Make a smaller share copy only for an explicitly required platform limit, and identify it separately.

## References

- `references/anime-pixel-clips.md`: the full procedure and recipes for these clips: base prompts, pixelize settings, eye, blink, mouth and smoke recipes, light maps, colour swings, headlights, the choppy TV, songs and spatial audio, and delivery checks.
