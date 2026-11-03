# DELTARUNE native assets, poses and staging

Use this reference while selecting assets, creating a missing pose and building the visual scene. Python with Pillow/NumPy is useful for deterministic inspection and extraction; FFmpeg handles proofs and final encoding. No particular one-off renderer or personal workspace is required.

## Pin bytes and inspect the role

Prefer available original game frames. The study below uses [TeamBlossomDevs/DeltaruneDecomp_beta, revision `154f9a97b8f18fa6974e917c4c4e774bde6b7eba`](https://github.com/TeamBlossomDevs/DeltaruneDecomp_beta/tree/154f9a97b8f18fa6974e917c4c4e774bde6b7eba), a **third-party decompilation**, not an official-hosted release. Treat it as one pinned study, not proof that every chapter or requested action is present. Inspect the selected revision and record repository/release, revision, path, URL, Git blob SHA, SHA256, dimensions, alpha and opaque palette. A wiki icon or enlarged preview is not a native asset.

This executable example fetches one pinned ordinary-Queen source into a project-local source cache and verifies both hashes before use:

```python
from pathlib import Path
import hashlib
import urllib.request

repo = "TeamBlossomDevs/DeltaruneDecomp_beta"
revision = "154f9a97b8f18fa6974e917c4c4e774bde6b7eba"
relative = "sprites/spr_queen_left/38d056af-b5cf-4610-8a22-520c9bee23db.png"
url = f"https://raw.githubusercontent.com/{repo}/{revision}/{relative}"
payload = urllib.request.urlopen(url).read()
git_blob = hashlib.sha1(b"blob " + str(len(payload)).encode() + b"\0" + payload).hexdigest()
sha256 = hashlib.sha256(payload).hexdigest()
assert git_blob == "4effdd46bb06788fb6340da1604a6a1a45b87db4"
assert sha256 == "969ea00fee3e449df17f37b54f48a13967a80d6c67808524b49879d9e9073908"
destination = Path("sources/native") / relative
destination.parent.mkdir(parents=True, exist_ok=True)
destination.write_bytes(payload)
print(destination, git_blob, sha256)
```

Read the associated sprite `.yy` and object Create/Step/Draw events. Sequence keyframes establish UUID order and holds; sorting PNG filenames does not. Walk the `sequence.tracks` tree for the `frames` track, sort enabled `keyframes.Keyframes` by `Key`, and resolve `Channels["0"].Id.name` to each PNG. Keep each keyframe's `Length`, canvas dimensions and `sequence.xorigin/yorigin`. If trimming a source bbox `(x0,y0,x1,y1)`, change its origin to `(origin_x-x0, origin_y-y0)`; do not erase origin information by trimming.

Record `.yy` preview `playbackSpeed` and its type separately from object `image_speed`, room timing and the fan scene's chosen cadence. The [unhappy-walk sequence](https://github.com/TeamBlossomDevs/DeltaruneDecomp_beta/blob/154f9a97b8f18fa6974e917c4c4e774bde6b7eba/sprites/spr_queen_walk_left_unhappy/spr_queen_walk_left_unhappy.yy) orders `cb851de7…`, `456820e6…`, `91019dfb…`, `108f43d4…`; its first and third PNGs have identical bytes. Preserve intentional repetition.

### Useful candidates and misleading names

These are candidates to inspect at the pinned revision, not substitutions for a requested action:

| Source | Useful role or trap |
| --- | --- |
| `spr_queen_left` / `spr_queen_down` | Ordinary Queen identity; canvases 33×63 / 34×65, origins (4,0) / (0,0). |
| `spr_queen_walk_left_unhappy` | Four-frame walk, not a kneeling offering. |
| [`spr_cutscene_25_queen_chair_look_down`](https://github.com/TeamBlossomDevs/DeltaruneDecomp_beta/tree/154f9a97b8f18fa6974e917c4c4e774bde6b7eba/sprites/spr_cutscene_25_queen_chair_look_down) | Ordinary seated Queen looking down; inspect the full chair/actor crop before reuse. |
| [`spr_cutscene_27_queen_look_down`](https://github.com/TeamBlossomDevs/DeltaruneDecomp_beta/tree/154f9a97b8f18fa6974e917c4c4e774bde6b7eba/sprites/spr_cutscene_27_queen_look_down) | A 206×160 GIGA-scale head, not the ordinary chair pose. A matching phrase in the name is insufficient. |
| `spr_krisb_idle` / `spr_krisb_defeat` / `spr_kris_fallen_dark` | Dark World reference / seated defeat / another seated-looking silhouette. None establishes a flat inert body merely by its name. |
| `spr_queen_banana` | Banana object, not Queen's holding action. |
| `spr_ralsei_defeat` / `spr_ralseib_defeat` | Inspect world, costume and alpha; choose the actual requested discarded costume/defeat reading. |
| [`spr_cutscene_26_laser_knife`](https://github.com/TeamBlossomDevs/DeltaruneDecomp_beta/tree/154f9a97b8f18fa6974e917c4c4e774bde6b7eba/sprites/spr_cutscene_26_laser_knife) | Green outlined attack/effect geometry, not a small discarded floor dagger. |
| `spr_cyber_fountain` | A different fountain asset; it does not supply the Dark Fountain's full draw stack. |

## Author missing poses without losing identity

List available sources and the genuinely missing actions. Generate raw art with the image model using visible native references; code may extract, map palette, align, composite and stage light, but must not draw replacement actor anatomy, faces or tears. Use one action family per compact multi-row sheet, fixed standing-equivalent scale, safe margins and common feet/root anchors. A 2×3 six-pose sheet is one option, not a requirement for every scene. Generate a coherent prone or crouched pose if the native library only supplies standing/seated frames.

Compare the integrated actor at equal source-pixel scale before local features: whole head silhouette, visor/eye width and angle, white/dark balance, jaw, collar and body proportions. Do not normalize each head bbox to hide enlargement. An unchanged native head crop may be attached to a generated body if that head actually expresses the action; record its crop, palette, scale and neck anchor and check all pose seams. A source plan does not prove pixel identity: verify the cropped region if claiming it stayed unchanged.

For a downward look, head pitch, visible top plane, neck/collar and shoulders must aim toward the actual target in the composed scene. A horizontal unhappy face shifted down one pixel does not establish gaze. Review hands and literal story cues too: receiving versus floor-resting hands, an offering versus a joke, or an unresponsive partner versus one looking back. Tears and facial wording appear only when requested or supported by the current scene; a prior Queen tragedy does not set the next character's expression.

### Palette, alpha and deterministic processing

Extract semantic opaque palettes from the native references. Ordinary Queen's studied palette is black `(0,0,0)`, blue `(18,99,188)`, cyan `(111,209,255)`, periwinkle `(114,138,237)` and white `(255,255,255)`. A banana adds its own dark outline and yellow tones. Do not assign that five-colour palette to every actor or indiscriminately include colours belonging only to a weapon.

Actual PNG transparency comes from alpha. Hidden RGB under alpha 0 may be cyan or magenta and is not a chroma-key instruction; retain real opaque robe/scarf colours and opaque black. Generated magenta sheets are a separate case: remove the demonstrated `#FF00FF` background/blends before nearest-palette mapping, protect actual costume colours, and inspect raw/keyed/processed edges. Generated actors use binary alpha `{0,255}`. Preserve fractional alpha only for a pinned native effect that already uses it, with that exception recorded per layer.

If Generate2dsprite is available, use its processor **unchanged** for key cleanup, splitting, QC and transparent exports; record its actual file hash and command. Inspect the installed CLI rather than assuming a `preserve` flag avoids filtering. Feed a pre-reduced common native grid and verify `source_to_output_scale == 1` before accepting any internal smoothing stage as harmless. One studied wrapper used 64-cell input, trim 4 and fit scale 0.875 so the remaining 56 pixels were preserved at 1:1; these values depend on that processor's implementation and are not global settings. If that tool is unavailable, implement equivalent deterministic extraction under these invariants and report the method; do not depend on a personal script path or claim unrun tool QC.

Use one common nearest-neighbour sample over each complete raw cell. With square raw cell size `S` and native cell size `N`, a candidate phase samples `floor((i+0.5)*S/N + phase)` on each axis. Use the same scale and phase for every cell, keep indices in bounds and retain the original. Natural pose bboxes may change; never fit each pose separately or use a different phase only for its mouth. Align grounded frames by integer translation to a shared feet baseline and compare opaque cell counts before/after to detect clipping. A prone actor uses a floor/root anchor, not a forced standing-feet rule. Regenerate within scope when the source lacks enough cells; do not hand-paint a missing facial cue.

Semantic props need visual review after sampling and occlusion. For a torn SOUL, verify two ragged halves of **one** heart, with crooked asymmetric inner edges and uneven separation, at native size and in the actual phone composite. Component count and unchanged source hashes do not establish a tear. Keep enough native cells or regenerate if the cut becomes a straight slit. A pack's dagger, blood smears and heart are separate static props unless the current request says they animate; pack slots are not animation frames.

## Native scene layers and timing

Store each asset's canvas, source origin, runtime anchor, integer position/scale, z-order and pose durations separately. Position top-left as `runtime_anchor - source_origin * integer_scale`. Use the same native grid for floor, actors and props; final enlargement is an integer nearest-neighbour scale without a fractional camera pan. Do not force all scenes onto a fixed 180×320 grid if required identity or prop details cannot survive there.

Keep actor pose cadence distinct from export FPS. Evaluate layer state for each output timestamp but hold authored/native poses for their intended duration. Log per-layer pose indices and anchors: an animated fountain can make every full-frame hash unique while an actor intentionally holds still. Reject high-frequency edge crawl inside a held pose; do not synthesize blurred intermediate actors to satisfy a 60 fps container.

For a requested static corpse or floor prop, guard pose index, anchor and transformed RGBA hash throughout the relevant interval. Also compare its designated visible opaque region **after compositing**, so moving light or an overlay cannot simulate a response despite an unchanged source. Use explicit protected regions where other layers legitimately occlude it; distinguish intentional occlusion from a failed stillness condition.

### Dark Fountain: port the draw stack

Inspect [`obj_darkfountain/Create_0.gml`](https://github.com/TeamBlossomDevs/DeltaruneDecomp_beta/blob/154f9a97b8f18fa6974e917c4c4e774bde6b7eba/objects/obj_darkfountain/Create_0.gml) and [`Draw_0.gml`](https://github.com/TeamBlossomDevs/DeltaruneDecomp_beta/blob/154f9a97b8f18fa6974e917c4c4e774bde6b7eba/objects/obj_darkfountain/Draw_0.gml), plus the actual room instance and related events. At this revision Draw's Git blob SHA is `f4ba498ff2022e82108d4e5b3a1c2294a5582a0b`, SHA256 `02722c75992b0ce69a834fc75fc1f87dc5759cf0555ac4e6d7eb33d2654ecada`.

The stack tiles `bg_fountain1` twice with opposing offsets and different alpha, masks the exterior black, repeats the edge sprite vertically with lateral echoes, draws three `spr_fountainbottom` passes, then covers the floor region. Preserve layer origins, blend/alpha and relative draw order. Convert room-scale coordinates to a declared native basis before adapting portrait placement. Make any changed hue, scroll period or looping behaviour explicit; do not describe an adapted port as byte-identical gameplay. A green cyber fountain PNG, a mesh or a new shader is not this draw-stack port.

### Light and physical wording follow the scene

For requested quiet melancholy, use a restrained cold ray and floor pool that reach the actors without a global blue wash. Derive contact shadows from actual contacts and cast shadows from caster silhouettes and a coherent source direction. Keep native palette families readable. Avoid a healing-like halo or time-varying exposure on an explicitly unresponsive body; stage ambient motion elsewhere. Other requested moods may use different light and colour.

No added text is the Atlas default. If in-world wording is requested, measure the actual native surface and natural occlusion, contain glyphs within it and preserve head/visor geometry. A later instruction removing a word such as “Kris” overrides earlier physical-text permission. Partial source-word visibility is not full-word legibility. Check the native actor and actual phone composite after every wording change.
