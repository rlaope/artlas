---
name: atlas-pixel-mood-reel
description: "Use when making a dark Atlas pixel mood reel with focused light, gloom and music, using either anime illustration or the requested game's native sprites."
version: 1.2.0
author: rlaope (khope), Friren, Kami
license: MIT
metadata:
  hermes:
    tags: [pixel-art, anime, reel, video, lighting, audio, mood]
    related_skills: [pixel-art]
---

# Atlas pixel mood reel

Short vertical clips (15-30 s, 1080x1920, 30 fps by default; use the requested frame rate). A character moment becomes a pixel scene with motion and a named song. The gallery in this repository's README shows the finished pieces this skill was distilled from.

## Choose the visual basis first

Use the requester's latest style instruction to choose the source workflow. The default anime recipe below applies to anime illustration beneath a visible pixel grid. For **original-game dots, native sprites or the actual game's small characters**, read [native game pixel scenes](references/native-game-pixel.md) before choosing assets. Start from native game frames and their scale, palette and origins; generate only missing poses in that style. This branch replaces the anime base-generation and pixelization steps, including mouthless-face, anatomy and palette defaults. Do not restart an anime-illustration loop or shrink an earlier illustration after the requester has chosen native game pixels.

## The core: these four come first

Every decision is judged against these. Explicit requester instructions take priority; otherwise these guide later defaults and recipes.

1. **High-quality dot art.** The frame reads as pixel art at a glance, on a phone. In the default anime branch, a visible coarse grid sits over a TV-anime key frame with smooth shading, many colours and clean forms. In the native-game branch, quality comes from the game's deliberate pixel clusters, limited palette and coherent sprite scale. The character and environment must belong to the chosen visual basis.
2. **Gloom.** A dark room at night, decadent and lonely. No sweet, pastel or candy grading. When a colour word is ambiguous, keep the gloomy reading and say how you read it.
3. **Light staging.** Default to one dominant source (a monitor, a TV, a window). Allow a secondary source when explicitly requested, while keeping one visual focus. The shape the light makes on the face, the hands, the smoke and the floor must be visible, and it changes slowly: a screen that shifts colour, a car's headlights crossing the blinds over ten seconds.
4. **Darkness staging.** Whatever the light does not reach sinks into darkness. The far side of the body, the back of the room and the corners stay near black, while silhouettes stay readable.

## Fixed rules

- **Candidates first.** Post each new base with a stable ID such as A or B, captioned outside the artwork, and let the requester pick before building on it. Record the accepted filename, SHA256 and a distinguishing feature in the spec so display order cannot select a different picture. Code-only fixes and aligned motion plates for that same approved base need no new choice; a redesigned face or composition does.
- **Motion every second.** Something moves at all times: the screen picture and its light, the ember, smoke, hair tips, breathing, a blink, a tear running slowly down the cheek. When the request calls for fluid character animation, the body, head, hair or expression must visibly act at phone size. Changing smoke or grain alone does not meet that request. A screen in the scene may stutter on purpose, like an old TV with a bad signal.
- **The character and the story.** Resolve the named character and their canonical silhouette before prompting. Compare the whole head, visor/eyes and jaw against native references at equal pixel scale before approving individual mouth or expression features. Use the canonical outfit and proportions unless the requester directs otherwise; preserve their choice rather than substituting an original character or adding an unrequested redesign. Anything the request centres on (for example the news report of a death on the TV) must read on a phone. The character is the subject of the frame, not the props.
- **Composition.** No ceiling-down camera. Leave empty dark space above the head. Keep props few. Keep named elements out of the Reels UI: the right-edge button column and the top and bottom bands.
- **A readable environment.** In the anime branch, dark does not mean empty: keep a readable interior (a window with neon blinds, pipes, a shelf, a poster). In the native-game branch, preserve the requested game's room, tiles and intentional black negative space.
- **Text follows the request.** Default to full-frame artwork without added titles, caption bars, lyrics or decorative labels. Add wording only when it belongs to the requested scene or the requester asks for it; a request for no text also removes title framing from alternate exports. Physical wording follows the real surface and its natural angle/occlusion. Preserve canonical face geometry rather than enlarging or redesigning it to fit a font, and distinguish the source word from the glyphs actually visible.
- **One completed MP4.** Deliver one canonical master by default. For Atlas, this takes precedence over generic workflows that create a normal/shorts pair; additional formats or delivery exports require an explicit request. Library copies may preserve that same master's bytes without becoming separate versions. Keep its full quality and text policy, with no unrequested title wrapper or re-encoding.
- **Respect the requested scope.** A diagnostic-only or skill-only request changes evidence, procedures and review gates, not the current scene, source or delivery. Use the feedback loop's isolated-proof and production-hash guard before running a renderer. Isolated diagnostic renders are allowed outside production with all QA/output paths overridden; do not write or regenerate production assets or exports, sync a vendored skill or replace a delivery in this mode.
- **Working method.** Compare the source, logical-grid render and encoded result at matching crops, then check a phone-size still and actual decoded motion using the [feedback loop](references/feedback-loop.md). Within the requested scope, fix every weakness that can be fixed before delivering. List as "remaining" only what truly cannot be fixed in this base, with the reason. Read the other collaborator's review posts as well.

## Defaults that change per piece

These worked so far. Treat them as starting points, not rules. Face and drawing recipes in this section belong to the anime branch; a native-game face follows its original design and the current request.

- **Face.** The default is a mouthless dot face: eyes only, drawn on the same grid as the face, irises that reflect the screen rather than glow, matching catchlights in both eyes, gaze toward the screen. A closed eye is a curved lash line with a skin-coloured lid that follows the eye's shape, never a filled rectangle. Draw a mouth when the scene needs it (a canonical death face, a scream).
- **Music.** The named song, embedded in the file. With no requested start point, the cut usually puts the hook about 10 s into the clip. A requested timestamp or phrase determines the start instead; verify it on the actual source file.
- **Space.** The song usually plays from a device inside the room (PC speakers, a TV, a laptop in a closet, a radio behind a door). Pick the treatment that fits the mood: across the room, through a wall, far away. For a muffled wall or karaoke-room effect, roll off the highs while preserving vocal intelligibility; use the spatial-audio measurements in the reference.
- **Sound effects.** Mixed close and separate from the song (lighter, fan, rain, a car passing at the same time as its headlights). Avoid short periodic transients such as clock ticks; under reverb they smear into a recurring patter.
- **Loudness.** -14 LUFS integrated, true peak -1.5 dB, as the default finish.

## Pipeline

1. **Merge the request into one spec.** Requests often arrive as a burst of short messages; later messages override earlier ones. Record the chosen anime or native-game basis, the beats and what must read on a phone.
2. **Base assets.** For native-game scenes, use the [native workflow](references/native-game-pixel.md) for original sources and any missing poses. For anime scenes, generate a TV-anime key frame (9:16, 1024x1536) that states the camera height, the pose literally, the dominant light and any requested secondary source, where each falls, the room behind, and any screen as a flat `#00FF00` placeholder inside its bezel. Generate two candidates, pixelize both, and pick by a side-by-side read at phone size. Fix framing with outpainting (blurred, dimmed pads as the space to fill), never with mirrored or tiled pads.
3. **Grid.** Native-game assets retain their native cells, palette, common scale and integer placement. For anime illustration, crop to 9:16 and choose the logical grid and palette by iris, lash, hair and dark-ramp retention alongside the phone-size pixel read. Grids such as 180x320 or 216x384 and 110-160 colours are starting examples, not fixed quality targets. Resample from the source once, preserve local dark ramps and compare the logical result before encoding; a larger export cannot restore detail already collapsed into the grid. Use the anime reference's feature-preservation recipes only where needed. Remove an unrequested drawn mouth on the source before downscaling.
4. **Light.** In anime scenes, build the light map from the drawing's own lit planes and recolour at constant luminance. For native-game scenes, preserve the original palette and use the requested game's layer/effect treatment. Darken away from the source while retaining readable silhouettes. Check region means in code, not only by eye.
5. **Motion.** Choose the acting beats, evaluate each output timestamp and render a short proof covering their peaks and transitions. Use the chosen branch's motion recipe; keep the pixel grid and static room coherent. Native pose cadence is separate from container FPS: a 60 fps export does not establish 60 new actor poses. Review hair and clothing lines for temporal crawl in real playback: progressive metadata, unique frames and a sharp still do not establish stable fine edges.
6. **Sound.** Fetch the official upload, verify the requested start against that file's own onsets, build the room path, add close effects separately, and loudness-normalise. When listening is unavailable, report measurement evidence (C50, T30, syllable-envelope correlation, transient scan) and mark listening as not observed.
7. **Verify and deliver.** Complete the feedback loop on the exact final MP4: ffprobe, full decode, ebur128, decoded motion and phone-size inspection. Deliver the single approved master under the requester's quality/version rules. Make an additional smaller share copy only when explicitly requested for a platform limit, and identify it separately.

## References

- `references/anime-pixel-clips.md`: the full procedure and recipes for these clips: base prompts, pixelize settings, eye, blink, mouth and smoke recipes, light maps, colour swings, headlights, the choppy TV, songs and spatial audio, and delivery checks.
- [references/native-game-pixel.md](references/native-game-pixel.md): original-game sources, missing native poses, palette and chroma cleanup, shared sampling phase, physical pixel text, native effect alpha and encoded-motion checks. Read when the requester chooses the actual game's dot style.
