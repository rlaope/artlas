# Native game pixel scenes

Use this branch when the requester wants the original game's dots, small sprites or an in-game scene. The latest request determines the visual basis. Native quality means coherent silhouettes, deliberate flat colour clusters and the game's scale; it does not mean an anime illustration with fewer colours. Keep the shared mood, story, sound and single-master delivery rules in [SKILL.md](../SKILL.md), and use the [feedback loop](feedback-loop.md) to review the exact encoded result.

## Establish the actual sources

Inspect the original-resolution PNGs before prompting. A library's 200×200 icon, search thumbnail, enlarged contact sheet or wiki preview is a discovery aid, not a native asset. Record the source repository or release, pinned revision, file path, URL, dimensions, SHA256, alpha values and opaque palette. Label third-party extraction/decompilation as such; a native game asset hosted by a third party is not an official-hosted release. Keep native files separate from generated extensions.

For GameMaker sources, read the sprite `.yy` and the relevant object's Create/Step/Draw code. Preserve frame order from sequence keyframes, canvas dimensions and origin; UUID filename order is not animation order. Record preview playback speed/type separately from object `image_speed`, room timing and the fan scene's chosen cadence. Inspect the actual pose and complete effect draw stack before using an asset name as evidence.

Measured examples from the DELTARUNE source study are pinned to [TeamBlossomDevs/DeltaruneDecomp_beta at `154f9a97b8f18fa6974e917c4c4e774bde6b7eba`](https://github.com/TeamBlossomDevs/DeltaruneDecomp_beta/tree/154f9a97b8f18fa6974e917c4c4e774bde6b7eba), a third-party decompilation. These are source facts, not dimensions to impose on another game:

| Sprite | Native canvas | `.yy` origin | Source use or trap |
| --- | --- | --- | --- |
| `spr_queen_left` | 33×63 | (4,0) | Ordinary Queen side silhouette and palette. |
| `spr_queen_down` | 34×65 | (0,0) | Ordinary front silhouette. |
| `spr_queen_walk_left_unhappy` | 33×63, 4 frames | (4,0) | Walking reference, not a crouched offering pose. |
| `spr_krisb_idle` | 36×38, 6 frames | (3,0) | Dark World identity and palette reference. |
| `spr_krisb_defeat` | 63×44 | (6,4) | Seated defeat; does not establish a flat, unresponsive body. |
| `spr_kris_fallen_dark` | 32×38 | (3,0) | Inspect the seated silhouette despite the name. |
| `spr_queen_banana` | 52×54, 8 frames | (26,27) | A banana object, not Queen holding one. |
| `spr_cyber_fountain` | 140×140, 6 frames | (0,0) | Inspect its scene role; it is not the whole Dark Fountain draw stack. |
| `spr_fountainbottom` / `spr_fountainedge` | 120×140 / 100×140 | (10,0) / (0,0) | Separate native effect layers; retain their relative origins. |
| `bg_fountain1` | 120×120 | (0,0) | Native animated-room treatment is established by draw code, not this image alone. |

For example, the [unhappy walk sequence](https://github.com/TeamBlossomDevs/DeltaruneDecomp_beta/blob/154f9a97b8f18fa6974e917c4c4e774bde6b7eba/sprites/spr_queen_walk_left_unhappy/spr_queen_walk_left_unhappy.yy) orders its frame UUIDs as `cb851de7…`, `456820e6…`, `91019dfb…`, `108f43d4…`. The first and third PNGs have identical bytes in this revision. Preserve that intentional repeated frame rather than sorting names or deduplicating the sequence. Its `.yy` preview speed is 1 with speed type 1; that alone does not establish the object's in-game cadence.

## Generate only the missing poses

List what the pinned assets already supply and what the scene actually lacks. Use original frames for available rooms, props and effects. For a missing special action, make the actual native reference visible to the image model and generate a coherent action sheet in that design. Do not call an existing seated/standing frame a requested prone/crouching action merely to avoid creating it.

Use the available sprite-generation workflow for new raw art and deterministic extraction. Ask for one action family, a compact multi-row grid, fixed standing-equivalent scale, shared body root/feet line, safe margins and the exact limited palette. A six-pose 2-column×3-row sheet is one useful example, not a universal frame count. Keep costume, head form, native facial conventions and pixel clusters stable. A requested face change belongs to that character and shot; never turn it into the next scene's default face.

Validate the **whole canonical head before local features**. Compare the original and candidate at one source pixel to the same number of display pixels, without normalizing their head bboxes. Check head size, silhouette, visor/eye width and angle, white/dark area balance, jaw length and connection to the collar. A correctly closed lip, clean palette or contained word does not pass identity when the narrow native face has become a large rectangular display and hanging jaw. Recheck the whole head after every local feature or typography revision; a previous local-feature PASS cannot carry forward as character approval.

When the existing native head already supplies the required identity, a modular action can use that **actual unchanged head crop** over a generated body-only pose. Make this asset plan explicit and preserve the crop's pixels, palette and scale; record its source crop and the body collar/neck anchor. Review the attachment in every pose for seams, doubled collars, detached jaws and unintended occlusion. Do not repaint or enlarge the head during extraction or let the new body force a different head scale. This is an available reuse method, not a requirement to split every character.

Approve the **integrated actor and story direction** before local facial details. When a character must look down at a fallen partner, read head pitch, neck/collar compression, shoulder posture and the visible top plane toward that partner's actual floor position. A horizontal unhappy head translated down one cell, or a file named "look down", does not establish that pose. If an unchanged head crop cannot express the requested direction, obtain a coherent missing pose within the current scope rather than forcing the body around it. Preserve canonical head scale and its connection to the whole actor.

Judge literal story cues on the resulting native-size frames: whether hands rest on the floor or receive an object, whether a body is inert or looking up, and whether an offering reads as denial rather than a joke. More pixels or more generated poses do not correct an inaccurate pose. Keep raw sources and held alternatives until the chosen result passes independent review.

## Stage quiet melancholy

Use a quiet cold key that visibly reaches the floor and actors while retaining their native palette and dark surroundings. Derive contact shadows from each actor's ground contact and cast shadows from the same caster silhouette and light direction. Check the integrated floor, bodies and shadows at phone width; a detached oval beneath a pose cannot establish all its ground contacts. For a scene with two ragged halves of one SOUL, keep them small relative to the actors and hands, grounded and readable without becoming the visual centre.

Avoid a full blue wash, healing-like halo or animated exposure over a body meant to remain dead and unresponsive. Place restrained environmental movement away from that body unless the current request calls for an effect there. Check the actual timed decoded arrival, hold and withdrawal of any light effect rather than approving the mood from one still.

## Palette and transparent edges

Extract the allowed opaque palette from actual native pixels and record intentional additions for a new prop. A palette is a set of permitted colours; every colour in a reference need not occur in a new pose. Choose semantic subsets when a reference includes colours for an unrelated item. For example, the studied Kris idle frame includes two pink weapon colours; including them indiscriminately mapped a pale knife to pink and changed its reading.

The studied ordinary Queen has five opaque colours: black `(0,0,0)`, blue `(18,99,188)`, cyan `(111,209,255)`, periwinkle `(114,138,237)` and white `(255,255,255)`. Its banana reference adds dark outline `(11,11,59)` and yellows `(248,173,8)` and `(248,216,8)`. These measured palettes are examples tied to the pinned source, not generic pixel-art limits.

For actual native PNGs, honor alpha and inspect opaque RGB: fully transparent pixels may hold cyan or magenta RGB, which is not evidence of a chroma key. Do not remove real robe or scarf colours based on those hidden values.

When raw generation uses `#FF00FF`, remove the key before it becomes a nearest-palette colour. Inspect RGB samples around dark hair and bright costume edges: faint key blends can map to a legitimate cape colour and become stray pink cells. Broad pink key thresholds can erase a real magenta costume. Preserve exact native costume colours explicitly; classify only demonstrated background blends and compare the raw edge, keyed native frame and integer enlargement. Record removed sample counts and retained costume-colour counts. Do not use blur, dithering, grain, extra shading or invented outline pixels to hide a contaminated edge.

Actor output must have alpha in `{0,255}` and opaque black must stay opaque. An exception is allowed for a **pinned original native effect** that already has fractional alpha. Document that layer's source and original alpha values and preserve its intended compositing. This effect-only exception does not permit softened actor edges or an arbitrary translucent filter on generated sprites.

## Common grid, scale and sampling phase

Use one whole-cell nearest-neighbour reduction for a generated sheet and one shared anatomical scale across its poses. Natural pose bboxes may differ. Do not fit each frame's bbox independently, stretch a face or enlarge a head to conceal detail loss. Inspect processor behaviour rather than assuming that a flag named `preserve` avoids filtering; if an internal resize uses a smoothing filter, make that stage a verified identity operation or choose a native-safe deterministic path. Keep the original processor intact and record any wrapper settings and source hashes.

If a thin lip, finger or outline exists in the raw art but disappears on the native grid, compare a few **common whole-cell NN sampling phases**. For cell size `S` reduced to `N`, a reproducible sample is `floor((i+0.5)*S/N + phase)` on each axis. Apply the same phase to every cell and keep the original output. This changes which source pixels are selected; it adds no feature. Check the whole poses, palette, text area and every frame transition after changing it. Per-frame origins, feature-specific sampling, code-drawn facial corrections and repeated resizes defeat this test.

The Queen face study compared common raw y offsets −3 through +3 at 512→64; y −2 retained a thin closed-lip notch more clearly to the author. The face was subsequently rejected for overall head/visor/jaw identity drift. The sampling observation remains a processing lesson, not a universal offset or facial approval. A feature that still lacks enough native cells needs a source revision within the current request, rather than a claim that export resolution restored it.

Verify a prop's **visual meaning after sampling**, not only component counts or unchanged source hashes. A torn SOUL must retain a crooked, asymmetric cut with stepped inner edges and uneven separation; two disconnected blobs alone cannot pass that requirement. Inspect the native result and actual phone-size composite after resampling and occlusion. If the defining tear collapses, retain enough native cells to preserve it or regenerate within scope, then review again rather than accepting reduced readability from a two-component count.

Store native cell dimensions, source origin, runtime anchor and integer placement separately. A grounded action may use a common feet baseline; a prone actor needs an explicit floor/root anchor. Keep translations and final scale integer, disable smoothing, and avoid subpixel camera movement. Verify unclipped limbs/props, stable anatomical scale and the intended ground contact across every frame. NN enlargement preserves the cells; it does not repair a lost silhouette.

## Physical text has a pixel budget

Apply the latest wording instruction first. If a later request removes "Kris", remove those glyphs from the actor and scene even if earlier instructions allowed that physical text; do not preserve it merely because it fits the surface.

Wording requested on a visor or in-world display belongs to that surface, at its true native angle and scale. Measure its usable native black region before promising full visibility. A 12×5 glyph mask cannot be established by a visor's overall bbox if the dark region is triangular or sheared. Transform the word onto the physical surface with integer/stepped placement and its natural perspective/occlusion, then contain all visible glyph cells in that surface's opaque display mask for every actor frame. Save the source word, unoccluded glyph mask, placement, surface/occlusion masks and visible-cell counts; no visible red cell may spill onto skin, outline or background.

Preserve the whole canonical face, head and body scale. **Never enlarge or redesign the visor, face or jaw to fit the font.** A side-turned face may naturally show only part of a correctly spelled source word. Report those separately: source spelling can be exact while the visible glyphs are partial. Natural surface occlusion is not evidence that the whole word is readable. If the request requires full-word legibility and the true surface cannot provide it, report that unresolved constraint rather than silently changing character geometry. Do not paste a floating label, stretch the head, scale up the actor alone or claim that clipping made unreadable marks readable. Phone-size identity and visible text need visual review in addition to per-pixel containment.

## Review the encoded scene

Document export FPS and each layer's actual pose cadence separately. Sixty container frames per second may repeat a deliberately sparse native action; it does not create sixty new actor poses. Use native frame order and measured holds, with requested acting visible at phone size. Intentional pose holds differ from high-frequency line crawl inside a held pose; inspect the affected actor region rather than relying on whole-frame uniqueness. A fountain's motion does not establish fluid character acting. Do not invent motion interpolation that blurs or warps native cells to satisfy an FPS number.

Render a short encoded proof covering every pose transition, the acting pause and the loop seam. Review original native frames, decoded native-size frames, integer enlargements and phone-width playback, with the gaze directed toward the actual partner throughout the relevant beats. Inspect neighboring decoded frames around timed transitions for one-cell flicker, line crawl, anchor jumps, scarf/key fringes, clipped props and physical-text escape. Record per-layer frame indices as well as decoded frame hashes: fountain changes can make every full-frame hash unique while the actor remains static. Claim pixel-identical head crops or feet only after a matching region comparison; a source plan or anchored appearance alone does not prove unchanged pixels.

For the exact final MP4, complete full decode, dimensions/FPS/duration/colour checks, requested music-window verification, loudness/true-peak measurements, and actual playback when available. Review the distant-music treatment with the close effects separately; mark listening as unobserved if only measurements were possible. A proof, a sharp still or technical sprite QC does not approve the final story, facial acting or sound.

Deliver one Atlas master under the requester's current delivery rules. Library/dashboard playback and download copies must match that master byte-for-byte, verified with SHA256; do not reduce quality or re-encode for synchronization. Check real play, seek and end/loop behaviour before marking registration complete. Record the final file/hash reviewed and any remaining unobserved checks. Current native case studies in this document describe source facts and processing lessons, not an approved final video.
