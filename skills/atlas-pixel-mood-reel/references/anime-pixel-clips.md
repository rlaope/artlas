# Anime pixel clips: a still scene with ambient motion and a song

Scope: short vertical clips (15-30 s, 1080x1920). A famous anime scene or a character moment is pixelized, and the picture holds still. Only light, wind, smoke or an on-screen image moves, while a named song plays with an effect (behind a door, under water, out of a monitor). The always-on rules live in SKILL.md; this file holds the procedure and recipes.

## 1. Picture source

- **Named scene: find the canonical moment first.**
  - Use the Fandom MediaWiki API: `api.php?action=query&list=search&srsearch=...&srnamespace=6&format=json`, then `prop=imageinfo&iiprop=url|size`.
  - static.wikia.nocookie.net needs a browser User-Agent plus `Referer: https://<wiki>.fandom.com/`.
  - Use the panel for composition, head angle, expression and blood placement.
- **Base still for a scene.** Use a text-only image_generate prompt for an "official <studio> anime screenshot" of that moment, in 9:16 portrait. List the on-model traits explicitly: hair and bangs, lashes, eye colour, half-lidded or not, expression, and where the blood is. Do not use image edits of the B/W manga panel: they redrew the face off-model, with a bare forehead and small realistic eyes.
- **Original situation** (a character on a sofa at night, for example):
  - Write one prompt that names the single light source and says it faces the character.
  - Name the props at a readable size: "a big tall can with an amber label", "a lit cigarette with a thick ribbon of smoke".
  - For a held drink, write the hold into the prompt too: the near arm resting on the stomach, a large tall 500 ml can about a tenth of the frame height, fingers wrapped around it, and the forearm and sleeve visible and connected. That can read naturally at 180 px; a can about 11 art px wide held against the chest did not.
  - Dress a named character in their canonical outfit with its original silhouette (Lucy: white cropped jacket, black netrunner leotard with red trim, white shorts, grey stockings, boots). Do not substitute a covered-up outfit for an adult character; that substitution was rejected outright. Ask for modest clothing only for original characters, who have no canonical outfit.
  - For an alternate-colour version of a character ("검은 머리 퇴폐 미쿠"), keep every silhouette cue and change only the colours named: twin tails and hair ties, the tie, detached sleeves. Recolour the outfit to match the new hair. Ask for the twin tails to fall outside any blanket or hood and into the screen light. Under the dark grade they read only where lit, and when they were buried, only the eye colour was left as a character cue.
  - Grade prompt for the decadent night mood: "pitch-dark room, the only light is the cold blue-white glow of the monitor, deep near-black shadows, desaturated, gloomy", with one warm accent (the ember) and eyes described as "dull teal irises catching the screen light". A candy-pink and lilac grade read as cute and got the whole cut rejected.
  - For a pose the user describes ("담배 든 손으로 턱 받치고 컴 보는"), write it literally: the elbow on the desk, the chin on the hand that holds the cigarette between two fingers, the tip pointing away from the face, and "no smoke in the picture". The smoke is animated later, and a painted plume has to be masked out and leaves a ghost. A blanket goes over the head like a hood. Frame it medium-close so the face is about a third of the frame width. The monitor can show only its back, as long as its screen faces her.
  - A "forearm laid across her eyes" pose often comes back as an arm resting on the crown, with the lower face open, and an image edit of the base did not move the arm. Check the pose on a 2x source crop before building. If it missed, name it as a limit rather than drawing an arm over the face.
  - For the "high-quality anime under dot pixels" look, prompt a "high-quality TV anime key frame" with chiaroscuro from the one source: a soft falloff across the face, glossy hair highlights, a near-black background, and the screen light described as neutral pale white. The scene's light colour is applied in code, so a colour cast in the base fights the colour swing.
  - Generate two candidates, pixelize both, and pick by a side-by-side vision read of the pixelized versions. The brighter source blew the face out after quantizing. Score eyes at phone size first: a sleepy close-eyed face scored about 2 points lower than an open half-lidded one.
- **"Hiding in a closet" ("벽장 느낌").**
  - Frame the closet from the dark room, through a half-open fusuma gap. Put door panels with round pulls on both sides, folded futons on the upper shelf, and the character on the lower shelf among clutter. Let the screen light spill out of the gap onto the tatami.
  - With the camera inside the closet, the scene read as a generic dark corner.
  - For a withdrawn mood, and when the user allows it, bow the head so the bangs hide the face. That removes the face-legibility edits entirely.
- **Dark props need contrast, not just size.** A black guitar on a dark lap was big enough and still vanished at 180 px. When picking the base, check on the pixelized candidate that each named prop has a lit edge or sits against a lighter surface.
- **Keep the character's own colours.** A style-reference image supplies grid, palette discipline and mood only. Recolouring the character into the reference's monochrome ramp turned an iconic character into a generic one.
- **Underage characters.** When a canonically underage character drinks or smokes, draw them as an adult in modest clothing with no fanservice framing, and say so once in the report.
- **Code-drawn effects.** Draw an exact physical effect in code (for example, halves offset along a cut) only when the user asked for that effect itself, not as your reading of a named scene.

## 2. Pixelize, then edit at grid level

- **Pipeline.** Crop to 9:16, BOX-downscale to 180x320, apply `MedianFilter(3)`, quantize to about 40-44 colours (MEDIANCUT, no dither), then upscale x6 nearest to 1080x1920.
- **Low-key bases** (one small light source, mostly shadow):
  - Skip the median filter.
  - Raise contrast 1.25x before the downscale.
  - Quantize to about 64 colours, and lift the mid-tones with gamma 0.8.
  - With the median filter and 44 colours, hair, clothes and skin merged into one dark mass at phone size.
  - Crop so the figure fills the frame. A dark, empty third reads as mud.
- **Dark grade on a low-key base** (one cold screen, the decadent night mood):
  - Darken away from the light softly, `1 - 0.38*d^1.4` with d the normalised distance from the face, then apply gamma 1.04. A −10 offset with 0.55 periphery darkening crushed the blanket, twin tails and sleeve into black. Gamma 0.92 looked milky.
  - Quantize to about 110 colours, and replace isolated pixels (more than 90 off their 3x3 median). On lit skin, blend 65% toward a median-3 plus 0.6 gaussian smooth. At 64 colours the lit skin banded into blotches that vision called crazed porcelain.
  - To keep the source's full width in 9:16, resize it to 864 wide. Pad top and bottom with a blurred copy of the picture stretched to 864x1536 and dimmed to 0.55, joined through a 90 px alpha ramp. A flat or hard-edged pad read as black bars with a visible seam. When the figure is small in a full scene (a whole armchair and a room), crop x 40-904 of the 1024x1536 source instead. The crop has no pad, enlarges her about 1.19x, and avoids the dead dark bands at the top and bottom.
- **Measure before editing.** Get feature coordinates (irises, mouth corner, nostrils, hand, prop, monitor quad) with vision, either on the plain downscale or on a brightened half-size source mapped through the exact crop transform. Coordinates taken from a different crop land edits in the wrong place, and they show up as pasted rectangles.
- **Porting a renderer to a new base still:** re-measure every coordinate (screen quad, ember, face, prop) on the new base, and find small features such as the ember numerically (for example bright orange pixels). Drop the old scene's hotspot weights, then compare the plain pixelization with the lit frame. Face and prop gaussians carried over from another base washed the character out to a pale ghost, even though the plain pixelization read well. Keep fills on the face and the prop at 0.2 or less, and build the screen spill with the room-light recipe in section 3.
- **Edit recipes:**
  - **Eyes (hand-placed pixel eyes):** a BOX downscale of anime eyes leaves a dark lash smear and a pink band, and vision scored it about 3.5/10. Measure each eye on a 2x source crop: the lash line (outer end, top point, inner end), the iris centre and radii, and the lower lid. Then draw the eye on the grid:
    - an iris oval clipped below the lash curve, with a top-to-bottom ramp from dark to the glowing iris colour
    - a deep shadow band in the first ~2 cells under the lash, and darker side rims where the oval edge is beyond 0.62
    - a near-black pupil (about 26,4,24) of 2x3 cells at the iris centre; a plum pupil merged with the lash
    - a white catchlight up-left of the pupil (2 cells plus 1 below), at the same offset in both eyes, and a small pale dot low-right
    - a lash line 2 cells thick along the interpolated curve, ending in the outer flick
    - a short soft lower-lid line
    A catchlight placed by iris bbox floated on the lash, and one level with the pupil read as a domino. This recipe scored about 7.5/10. In a dark cold-light scene the eyes must reflect the screen, not glow. Ramp the iris from about (12,30,42) to (58,124,140). Make the catchlight a 2x2 cold bar (200,228,255) on the screen side, with one white cell. First pull the source's own bright cyan, in a ring 3 cells around each iris, toward the local skin mean. Bright cyan irises read as self-lit anime glow. On bases with a cool tint, recolouring only the bluish pixels in the iris to a three-stop ramp plus one catchlight is the lighter option.
  - **Eyes on a high-quality anime base with large drawn eyes:** keep the drawing's own eyes, and do not redraw them. Downscale the eye boxes with LANCZOS after an unsharp mask, and blend them into BASE through a soft elliptical mask. Then touch up only four things: darken the top third of the iris about 0.6x for the lid shadow, darken its rim, put in a 2x2 navy pupil and one reflection cell on the screen side, and drop the shaded far eye one step. Fully redrawn iris ovals on that base read as flat, glowing doll eyes and drew "눈 좀 이상하고"; the touched-up originals scored about 7-7.5/10.
  - **Redrawn round anime eyes (when the base's eyes must be replaced):** the eye has two colours only, a near-white sclera and one solid black pupil disc about 60% of the eye width, with no grey iris ring and no catchlight. A grey ring around a black pupil reads as a target, and a catchlight brighter than the sclera reads as a white inside the white. Repaint the old eye area by sampling outward from the eye centre, so the surrounding skin planes continue inward and no ellipse of another colour is left behind.
  - **Redraw eyes with the image model, not code layers:** when a base's eyes are rejected, edit the approved base with the image model and change only the eyes. Code painted over an existing drawing kept leaving nested outlines. If the edit endpoint refuses separate reference images, paste the reference frame as a small framed inset in a corner of the input and tell the model to paint it out. Write the eye spec into the prompt: heavy black upper eyelid sitting directly on the white; lower lid thin and broken; separate dark eye-bag crescent below with a sliver of skin between; small black pupil about 30-35% of eye width, slightly off-center; no outline ring around the eye. Make the closed and mid-blink plates the same way from the chosen open plate, and use code only to switch plates. Show numbered candidates with eye zooms before building.
  - **Edit the picked candidate, change only what was named:** once a candidate is picked and one change is asked for (bigger pupils), edit that picked image with the image model and keep its composition. Then snap the result to the clip's dot grid before showing it. A smooth unsnapped render reads as "not dot style". Pupils read as part of the eye only when they hang from the heavy upper lid in the same darkest ink, cut flat on top, with white below and at the sides. A full disc with white all around reads as a separate sticker circle, even at the right size.
  - **Eye close from generated plates:** generate each closing level (about 20, 33, 50, 65, 75, 88 and 100 % closed) as its own image-model edit of the approved open plate, and snap every plate to the grid. The model's % is unreliable, so measure the eye white left in each plate and pick the cleanest plate per eye per level. Take only the eye zone from each plate, so the face, hair and light never shift. Let the white only shrink from level to level, and keep one lid tone through the close. Swap plates per frame and never crossfade: a blink is 3 frames closing, 2 shut and 4 opening, and the final close goes one plate deeper every 0.5 s and stays shut. On the lit eye, avoid a closed lid darker than the skin around it, which reads as an empty socket.
  - **Pupils move, not only blink:** a pupil that stays in one place between blinks reads as a static sticker circle. Make gaze plates from the open plate: shift the pupil a few cells sideways and one cell up, clip it to the eye opening below the lid, and keep every pupil column joined to the lid. Refill the cells it leaves with the eye white beside them. Script a slow look-around: one cell every two frames, then hold 1-2 s. Return to centre before each blink and before the final close.
  - **Dark circles, not socket rings:** around a redrawn eye, repaint the base's dark socket mass as one flat skin tone, keeping only wide hair masses and thin strands that cross from above. The dark circle is one 2-step crescent under the eye only: the darker step touches the lower lid, then one step lighter. Make it wider than the eye and drooping a little at the outer end, never above the eye or up its sides, and never black. A dark socket, a lighter band and the eye outline stacked around the white read as "an eye inside an eye inside an eye". Black goes only on the upper lid line and the pupil. Check the open eye at the same zoom as the viewer's crop, and count the closed eye-shaped outlines: only the upper lid should read as one.
  - **Whole-eye close:** the upper lid starts at the eye's outer top line and paints skin down to its leading edge. The black lash travels on that edge, and nothing of the open outline, socket ring or crease stays above it. The lower lid rises a little to meet it, and the lower lid line merges into the lash past about 60%. The pupil disc is never moved or squashed, only covered. A lid that moves inside a smaller inner eye while the outer outline stays open reads as "only the inner eye closes".
  - **Shut eye:** one lash curve spans the full eye width, bowed down and levelled to the screen. A stroke following the head tilt reads as a smirk. Skin sits under it, with one soft bag arc below.
  - **Blink timing:** a held face blinks about twice per face section, never on a cut. Use 9 frames: 3 closing (lid 0.25, 0.55, 0.85), 2 shut using the same drawing as the final shut eye, and 4 opening. For a slow final close, pick each frame's lid state from a table of visible eye cells, so the visible eye shrinks linearly in time rather than holding and then snapping shut.
  - **Blink:** close the eye with the eye's own shape. Fill only the cells between the lash curve and the descending lid edge, inside the eye outline (the iris/sclera mask), and sample each column's lid colour from the skin just above the lash. Draw the lash line at the lid's leading edge. A rectangle filled with one skin colour across the eye box read as a square box over the eye.
  - **Painted smoke in the base** (the generator draws a frozen plume, and animated smoke goes on top). Avoid this by prompting the base without smoke. Use this recipe only when the chosen base already has a plume:
    - **Mask:** the largest connected component of `lum - median31(lum) > 6` and low saturation inside a box around the plume, closed, dilated about 14 px. Add a hand-traced centreline through measured points, from the tip to the top curls, stamped as 15 px discs to take the haze. Protect hair, hood and hand, and the lit tip, by rules. A threshold mask on its own caught book edges and hair strands, and it missed the plume's base and top.
    - **Fill:** copy each masked pixel from the nearest clean pixel on its row, mirrored. Shelves and spines are horizontal or vertical, so this keeps them aligned. Then feather the edge about 2 px.
    - **Tone and shade:** shift the filled area's mean colour to the clean ring around it. Then darken the area and its surroundings about 45% with a soft vignette, so the light animated smoke stands out against shadow.
    - A gaussian inpaint left a grey-teal smudge that vision read as ghost smoke in every round.
  - **Blood:** paint a tapered path from the measured mouth corner, 3 → 2 → 1 px, ending in a drop. Recolouring a box gives a red rectangle.
  - **Nostrils:** lighten only the darkest few pixels. Never fill a patch, because it reads as a salmon or grey block.
  - **Small held props:** settle prop size when you pick the base still. Measure the prop on the 180-wide grid: below about 12 art px tall it will not read at phone size. Do not try to fix it later by compositing a hand-drawn sprite or a finer-resolution patch cut from the source. In context both read as pasted, with hard patch edges, a sharper or brighter look than the surrounding cells, and light that does not match, even when the sprite scored well on its own. If the prop is too small, pick a closer framing or another base and say so; do not patch it. The same holds for a light source the base already draws, such as a laptop rim or a lamp. Animate its own bright pixels instead (a mask of bright, light-coloured pixels inside its measured bbox), and do not draw a lid or glow over it. An overlay drawn from vision coordinates landed beside the object.
  - **Line overlays:** lines drawn on the grid along measured source geometry are the one overlay that reads. A straight neck with a lit edge and 3+3 tuner dots scored about 7/10. Closed shapes drawn the same way do not: a body ring and two pickup bars read as a porthole or a pair of eyes. Lines drawn on a small face do not read either: a 1-px mouth line and a white tear stroke on an upturned face read as pasted marks. Bring face features through from the source instead, with a sharpened LANCZOS downscale blended in softly, and add at most a soft cast-shadow band, such as under a sleeve that crosses the face.
  - **Dark hardware lost in the downscale:** recover it from the source pixels.
    - Select inside the measured bbox by local brightness and low saturation, not by hue. The "gold" hardware was dark grey in the source, and a hue mask found nothing.
    - Max-pool the selection to the grid.
    - Recolour it a muted, shadowed gold that brightens only on the lit side.
  - **Dot face (no mouth):** on the source, refill an ellipse around the measured mouth (about 15x10 source px) with a least-squares plane fit of the skin ring around it. Use ring 1.5-3.5 in ellipse units, keep only bright, non-hair-coloured pixels as skin, and feather the outer 0.5. Vision found no smudge afterwards. Then draw the eyes with the hand-placed eye recipe above. Only darkening the existing eye strokes left the eyes too faint at phone size.
  - **Hide anatomy behind a foreground object** (legs too thick, a bad hand): enlarge the object in the source instead of repainting the body.
    - Draw a polygon mask of the object from measured corners (bezel, stand neck, base plate).
    - Resize the whole source by the factor, then paste it back through the resized mask, offset so a fixed anchor (the stand's foot on the cabinet) stays put.
    - Rebuild the screen mask and refit the quad from the edited source, and check the hidden fraction numerically (object mask ∩ measured limb bbox). A TV scaled x1.38 hid the legs fully.
  - **Enlarging or moving a prop on a base that has no room for it** (a TV that would run off the frame edge): erase the old prop and its floor reflection, scale the prop's own pixels about its floor contact, then pad the canvas to 9:16. Do not fill the pads by mirroring or tiling the picture. Mirrored pads brought back a second TV reflection and half an ashtray, and row-interpolated fills left a ghost slab and a seam. Instead, blur and dim the pads (gaussian about 30 px, x0.3), resize the canvas into a 1024x1536 seed, and outpaint it with image_generate ("the blurred areas are empty space… keep everything inside the sharp picture"). One outpaint pass came back seamless, with the pose unchanged.
  - **Screen mask:** keep only the largest connected component of the #00FF00 mask. Green glow reflected on the stand counted as screen and showed up as a lit slot under the TV.
  - **Green placeholder spill:** the #00FF00 screen also tints the drawing around it (cheek, chair edge) green. Outside the screen mask dilated 3 px, pull the green excess back: where `G - max(R, B) > 6`, subtract 0.85 of that excess from G. Otherwise the room keeps a green cast that no screen colour explains.
  - **Unlabelled bright shapes:** before acting on a vision critique of one, identify the shape on a source crop. A "faceted flare where the body should be" was the scene's own laptop lid.
  - **Narrowing a door gap** (a tighter peek): extend the panel's inner stile by repeating its own edge columns (`BASE[:sill, gap_r-1-k] = BASE[:sill, gap_r+2+(k%3)]`), so the paper texture and frame carry over. Narrow only from the side with no named prop; widening a panel over the amp hid it.
- **Tiny humanoid sprites** (on-screen dancers, about 20x24):
  - Silhouette-only figures read as insects, imps or staff-holders.
  - Use colour instead: a hair cap with bangs, a skin face with eye pixels, skin hands separated from the head by 1 px, and hair tails attached to the head.
  - Vision-check the sprite on its own at 12x on a flat background, then again in place on a native crop. A good standalone score does not carry over to the composited frame.
  - Even a sprite that vision read correctly as the character looked unnatural to the user on a monitor. When a screen should show a video, map the real footage onto it instead (see Screen content below).

## 3. Ambient motion (everything else static)

- **Light shaft:**
  - Use a gaussian band about 17-20 cells wide, sweeping at constant speed.
  - Fade it along its length so it is strong on the face and weak on dark cloth.
  - Tint bright surfaces toward gold with a soft shoulder, and add only a small orange screen on dark pixels.
  - Additive or multiplicative warm light on near-black cloth reads as a tan strap. A wide beam reads as a global brightness pulse.
- **No ordered (Bayer) dither in light on a quantized painted base.** It reads as a screen-door or halftone grid over the image. Use a smooth per-art-pixel lift instead. Bayer stepping only suits flat, few-colour ramp art.
- **Screen light:**
  - Draw the screen as a real panel inside the measured monitor quad, with a bezel on every side, scanlines and visible content.
  - Recolour the room spill with the screen colour.
  - Change the colour every 2 beats and flash on each beat.
  - A flat colour fill reads as a sign or a light bar.
- **Screen content: real footage, perspective-mapped.**
  - Fetch the official video at 480p or lower. Cut the same window as the audio, and crop off the channel watermark. Decode it at the clip fps to about 392x215 RGB.
  - Screen quad: when generating the base still, ask for the screen as a flat pure #00FF00 placeholder inside a black bezel. Then build the mask exactly (G>150, R<120, B<130, G>R+60) and fit the top and bottom edges as lines over the per-column extents. On an existing base, measure the quad with vision.
  - Compute a homography from the quad to the unit square (DLT via SVD on the 4 point pairs). Supersample 3x3 points per screen cell, and average the video pixels.
  - Stylize lightly so figures survive: saturation x1.1, posterize to 16 steps, darker even rows for scanlines. Darken the bezel ring.
  - If the quad runs off the image edge, the visible cells cover only part of U. Keep the video's true aspect, and shift it so the centre of the visible U range lands on the centre of the video (`U2 = 0.5 + U - (U_vis0 + 1)/2`). Stretching the visible range across a fixed 60% of the video width squeezed the dancers into close-up faces.
  - Before rendering, check that the footage window matches the audio window. Match a few grey 64x36 thumbnails of the cut clip against the source video over a sweep of `-ss` values; the best offset should equal the song start.
  - Room light follows the picture ("불빛 움직이는 이팩트"):
    - **Tint:** mean frame colour with an EMA of about 0.55, saturated about x1.6, mixed 88/12 with a neutral cool. Light the zone near the screen (floor, sofa end) with the lower half of the picture, and the rest with the full frame.
    - **Shape:** `0.75*exp(-d_edge/38)` measured from the screen's lit edge, plus a weak `0.14*exp(-d/90)` lobe toward the character. Zero it above the sofa back so the wall and window stay dark, and blur it about 2 cells. A 120-cell radius with a face fill washed the whole room; 62 cells with no lobe made the light invisible.
    - **Strength:** `level = 0.55 + 0.45*tanh(2*max_channel/255)`, times `1 + 0.35*kick` on the fitted beat. Driving it by mean brightness flashed bright frames to white and switched blue frames off.
    - **Blend:** mostly additive, `img*(1+0.12k) + 0.8k*tint*(1-img/330)`. A strong multiplicative term turned the lit fibres of a shag rug into glitter specks.
    - Add a halo outside the screen at about 0.28. Let the console top in front reflect the picture colour, graded by distance from the screen rather than as a flat patch. Never draw spill on the screen cells.
    - **Check it numerically, not by vision alone.** Vision called the same light a global wash in one round and invisible in the next. Render frames with differently coloured pictures and print the mean delta from BASE per region. The shipped balance was about +30 on the console, +50-64 on the sofa front, +14-18 on the face, 0 on the wall, nearly equal across pictures.
  - Vision read this as the actual dance video, with no bleed outside the bezel.
- **Screen content: a generated broadcast** (a news report that never aired, such as a character's death):
  - Generate one 16:9 broadcast still with image_generate, putting every line of on-screen text in the prompt verbatim: a portrait with a "CONFIRMED DEAD" banner and life dates, a BREAKING lower third, a ticker, the channel logo. The generator spells short quoted caps correctly. A prompt without the death wording came back as a "WANTED / PERSON OF INTEREST" manhunt frame, so check the frame with vision before building.
  - Animate it as a broadcast: a camera cut list of rectangles on the still (full frame, portrait plus banner, the event, the anchor), each with a slow 6% push-in and a short white flash on the cut. Paste the lower third from the full frame onto the close shots, scroll the ticker strip, and pulse the red banner.
  - On the event shot, warm the grade and flicker it. Take the room-light colour from the top 60% of the picture, using its brighter 40% of pixels. The mean of the whole frame was dominated by the blue lower-third graphics, and the fire shot then lit the room blue.
  - At grid resolution the text does not read, but the portrait, the red banner and the fire do. Say once in the report that the broadcast is a generated scene.
- **Choppy old TV** ("티비에서 버벅이는 느낌"): sample the broadcast at 12 fps and hold it in random freezes of 0.25-0.75 s every 1.3-3.2 s. Then add short analog glitches every 0.9-2.4 s: a row-band tear shifted 25-80 px with a bright edge line, a vertical roll with a pure-black blanking bar, and blocky high-contrast snow. Dim the room light during each glitch. A gray-wash snow read as a fade, and a 6-px tear did not show at all.
- **Slow headlight pass** ("불빛 차 지나가면서 아주 천천히 쉐이딩"): use one `car(t)` envelope (sin² over 10 s) to drive every part of the pass. Recolour the blind slats toward amber (255,172,70) and slide that band across the window. Throw soft slatted patches on the side wall, moving away from the window, and on the open floor, moving the other way, plus a thin rim on her hair. The patches need a gaussian edge falloff and a skewed parallelogram shape. Blend them additively with amber and keep them off the TV, the table, her legs and the chair base. Normal alpha over the near-black room read as a grey veil, and a straight-edged patch read as a pasted box.
- **Off-edge screen with no homography:** when the quad's left edge runs off the image, fit the top and bottom edges as lines per column, map x linearly to U across the visible part, and set the hidden fraction from the estimated full screen width. That mapping is enough for a TV seen nearly head-on.
- **One screen as the only light** (a laptop or phone in a dark room):
  - **Spill:**
    - Put the emitter on the screen-facing side of the lid, and aim the spill at the character: `exp(-d/34..48)` times a smooth side weight (a sigmoid over x).
    - Never gate a light map with a boolean threshold. A hard step left a see-through vertical strip that read as a pasted panel.
    - Clip the spill (about 0.45) so the nearest surface, usually the lap, does not blow out and outshine the face.
  - **Make one source read:** darken the base away from the light (about 30% at the far upper side). Then lift the floor a notch (`x*0.92 + 9`) so shadows do not crush to mud on a phone.
  - **Face:** light it with a small uplight offset toward the screen side. A gaussian centred on the face read as a blue mask over the mouth.
  - **Side light from a screen beside her:**
    - Put the face lobe on the near cheek, offset about 16 cells toward the screen with sigma about 15.
    - Multiply the whole spill by a falloff across x: `clip(1.15 - (x - x0)/70, 0.25, 1)`.
    - Darken the far cheek and shoulder in BASE by about 40%.
    - Check per-region means in code: near cheek about 130, far cheek about 60, far shoulder about 30, back wall under 10.
    - A centred face lobe lit both cheeks and the far shoulder evenly, and that read as an overlay rather than a side source.
  - **Separate forms:** darken the hair one step outside a face box. Dim near-white details (guitar binding, inlays) so they do not compete with the face.
  - **Floor spill out of a door gap:** fan the wedge toward the side the screen faces, with its brightest edge on that side. A wedge fanning away from the screen's facing was flagged as wrong-direction light.
  - **Saturated character colours under a cool single source:** grade them, do not repaint them. Mix the hair/jacket pixels about 45% toward a luminance-matched cool tint and drop them about 18%, keeping the hue family. At full saturation the character read as a sticker over the blue scene.
  - **Video playing on a hidden screen:** change the level at random cuts every 1.2-3.4 s with a 0.12 s blend, add a small flicker, and tint with a few cool hues. When the song has an official MV, drive the light from the MV instead, so it changes with the song's own cuts. Decode the same window as the audio at 32x18 at the clip fps, take the per-frame mean colour (EMA 0.5), saturate it x1.4, and mix it about 70/30 with the scene's key colour. Set the level to `0.70 + 0.30*tanh(2.2*max/255)` so the face never drops into mud on dark shots. Zero the spill over the window and city, or the whole frame reads as an exposure pump. Check per-region deltas from BASE in code.
  - **Dust lives only inside the light:**
    - Draw motes only where the spill is above about 0.3, and never inside a face ellipse.
    - Make some 2 px wide.
    - Keep them within a faint wedge from the screen to the face.
- **Light that rides the painted form** (a high-quality anime base that already draws its own light and shadow):
  - Build the light map from the base itself: `form = clip((gauss(lum, 1.6) - 24)/110, 0, 1)^1.2` times a soft reach from the light's side, `exp(-x/190)`, then blur it about 1.2 cells. Only planes the drawing already lights (near cheek, hand, forearm, hair gloss) take the screen colour, and the shadows keep their value. A distance-only spill tinted the shadows and the background too, and vision read it as a global colour grade.
  - Recolour at constant luminance: `lit = lum * tint / Y(tint)`, blended `img*(1-0.95k) + lit*0.95k`. Scale the pink so its luma equals the blue's. Additive spill made the blue phase brighter than the pink, and that read as an exposure pump.
  - The blur on `form` keeps the light edge soft. Without it the face light ended in a hard vertical slab down the nose.
- **Blue <-> pink colour swing** ("핑크도 나오고 색깔 전환 자연스럽게"):
  - Drive the pink weight with 0.25 of the MV's warm/cold measure, `(R-B)/(max+30)` smoothed about 0.2 s, plus 0.75 of a scripted swing. Use about an 8 s period, shaped by `0.5 + 0.5*tanh(2.2*sin)/tanh(2.2)`, so it dwells about 3 s at each colour and glides about 1 s between them. An MV-only weight stayed on one hue for many seconds, and pink barely showed.
  - Use about BLUE (60,140,255) and PINK (255,90,185), luma-matched. Check the result on the decoded MP4: per second, the face R-B should swing from about -75 to +5 while face luma stays within about 3.
  - Let the hair gloss (bright bluish pixels in the hair) take the light colour, and run a slow shimmer down the strands.
- **Breathing:** shift everything below the shoulders up by up to 1 art px with a soft vertical weight ramp, on a 4-5 s cosine. A hard body mask left a tear line.
- **Passing-car headlights** (a night room; the sweep doubles as a sync point with the car sound):
  - Slide a filled, slanted window quad with one thin dark mullion across the room over about 3.6 s, with a gaussian amplitude envelope.
  - Lean it about 0.3 px per row on vertical surfaces. Past the floor line, bend it the other way (about 1.8 px per row) so it reads as projected.
  - Use a pale warm colour (about 255,232,200) at about 0.22 strength. Give it the same pass times as the cars in the audio script.
  - A thin gaussian band read as a stripe artifact, while a bounded quad with a mullion read as headlights.
  - Keep the patch off the recessed space where the character sits by multiplying it by (1 - mask). Dilate the mask about 2 cells before blurring it, or the feathered edge leaks onto her.
  - Verify with the render function itself: the max of patch × mask over the whole pass. Colour stats on decoded frames also move with the screen flicker.
  - Use it only where a window can throw the patch onto visible surfaces: a window behind the camera, or a low floor. Through an upper-floor city window behind the subject the light would land on the ceiling. A patch drawn on the sofa there read as volumetric beams in the air from the wrong source, so let the screen carry the moving light instead.
- **Wind:**
  - Dust or ash streaks go behind the figure. Allow only a few bright flecks in front, and never over the face.
  - Hair-edge flutter runs at a 15 fps cadence, driven by a gust curve.
  - Use the same gust curve for the wind audio.
- **Smoke:**
  - Use one source; two cigarettes split the eye.
  - Draw a continuous plume, not loose specks: either strands (below) or the dense particle advection further down. Stamp soft discs along each strand path on a 3x sub-grid with max-compositing, blur about 1.2 sub-cells, average down to the grid, then scale the alpha to a cap near 0.8. Scattered 1-3 px particles vanished at phone size or read as noise blobs.
  - Structure: a faint, broken thread off the tip (about 6 art px, strength alternating every few steps, a small sway), then two open S-wisps that lean the same way, widen (`r = 0.55 + 0.07*s`) and fade fast (`exp(-s/15)`), with puffs travelling up the strand.
  - A long, bright, straight thread read as a second cigarette or an incense stick. Nested curls read as a coil, and a strand that turns back reads as a knot.
  - Over a near-black sofa the plume stays faint on a phone; route it across a mid-tone surface when the composition allows.
  - **Frame-to-frame change:** strands driven by periodic sines of t give nearly the same plume seconds apart, and vision flags that as a pasted sprite. Particle advection gave a plume that was different in every sampled frame.
    - Recycle about 1400 particles emitted at a steady rate. They rise 7.5-10.5 grid px/s and live 5-8 s.
    - Push them sideways with four incommensurate travelling sine waves, scaled up with height above the tip, plus a slow room draft and a per-particle phase.
    - Integrate each particle from its birth in about 14 steps. Spread it by `0.06 + 2.4*h^1.4*sqrt(age)`.
    - Weight the alpha low for the first few cells, so the tip is a faint thread.
    - Splat into radius buckets on a 3x sub-grid with one blur per bucket. Map `alpha = 1 - exp(-44*acc)`, capped near 0.72.
    - Check the correlation of alpha maps 7 s apart (aim under about 0.8) and between adjacent frames (about 0.99, for smooth motion).
    - A fixed haze blob above the plume read as a sprite puff.
  - Start the ribbon at the ember and route it away from the hand and arm. Mask the limb (bright pixels in a box above the ember) so the smoke passes behind it. Straight strands crossing the fingers read as claws or extra fingers.
- **Verify on the decoded MP4:**
  - Project the brightness excess onto the beam axis per frame. The peak should move steadily.
  - Check that screen-region brightness peaks sit within one frame of the source kicks.

## 4. Song

- **Source:** the rights holder's official upload, fetched with the latest yt-dlp in a fresh venv on a current Python.
- **A loose song name** ("미쿠 cctv"): search yt-dlp `ytsearch10:` and lrclib, and pick the official track that best fits the description. DECO*27 「モニタリング」 (watching, "I know") was the pick for "CCTV". Say in the report which track you used, and invite a correction in one clause. Do not stop to ask before building.
- **Timing:**
  - Get lyric times from lrclib, retrying with a short sleep on a 503.
  - Then find the real onset on the file: vocal band 300-3400 Hz RMS at 50-100 ms hops, and kicks from a low-pass under 120 Hz at 5 ms hops.
- **Hook offset.** "하이라이트 X N초전부터" means the clip starts N s before the hook line's vocal onset.
  - Ao no Sumika TV OP: 今でも starts at 47.50 s, so the clip starts at 37.5 s.
  - Caramelldansen official HD upload: 49.42-69.42 s covers "Dansa med oss … Caramelldansen" plus "Oo-oo-oa-oa".
  - Kessoku Band "ギターと孤独と蒼い惑星" official Topic upload: the こんなに chorus starts at 44.66 s. A 34.66-54.66 s window runs from 息も出来ない into the chorus at clip 10 s. For 30 s, use 34.66-64.66 s with a 2.6 s fade-out at the end.
  - DECO*27 「モニタリング feat. 初音ミク」 official upload (kbNdx0yqbZE): "Mwah!" at 40.47 s, chorus 「お願い きみが欲しいの」 at 41.43 s, about 131.5 BPM. A 29.9-54.9 s window (25 s) runs from 「グスングスン凹んでさ」 to "Mwah!" at clip 10.6 s and the chorus at 11.5 s. Some lyric lines are sexually suggestive, so choose windows by melody and do not quote those lines in the report.
  - Rosa Walton & Hallie Coggins "I Really Want to Stay at Your House" (the Cyberpunk: Edgerunners song; Lakeshore Records upload gzbLODUb1sA, 246.7 s, matching the 247 s lrclib entry): the chorus "'Cause I really wanna stay at your house" starts at 75.45 s and runs at about 131 BPM. A 65.45-90.45 s window (25 s) starts on "So, what do you wanna do" and puts the chorus at clip 10 s. For a dark night-room scene, a distant siren is a good slow bed event: a 720 Hz tone swinging ±160 Hz at 0.55 Hz, LP 1.1 kHz, about -47 dB, passing over 8 s.
- **Extending a cut** ("30초로 늘려"): keep the hook at the same clip time, add the extra length after it, and end on a phrase with a fade.
- **Tempo for beat sync.** Least-squares fit `t = k*period + offset` on the kick onsets, with outlier rejection. A median-interval estimate (166.67 instead of the true 164.40 BPM) drifted to chance alignment within 20 s.
- **Effects colour the song; it stays clearly audible.** Measure music RMS against the bed and keep the song about 11 dB over ambience.
  - **Behind a door / under water:** low-pass about 2.4 kHz at 12 dB/oct, a light 230-420 Hz box resonance (0.3 mix), light wow (about 0.9 ms delay depth) and a short dark tail. A 520-780 Hz low-pass buried the song.
  - **Out of a monitor speaker:** high-pass 260 Hz, low-pass 6.5 kHz, a 0.9-2.8 kHz bump, tanh saturation, early reflections plus a 0.45 s tail, a slight left bias toward the monitor, and a fan/hum bed at about -46 dB.
  - **From a device inside a closet, heard across a dark room** ("멀리서 들리는 느낌", "다른 방에서 틀어놓은"):
    - **Chain, in order:**
      1. Device speaker: HP 150 Hz, LP 8 kHz, stereo side 0.2.
      2. Closet IR: a few reflections at 2-11 ms plus a 35 ms dark tail.
      3. Wall or gap: two cascaded 2nd-order low-passes at fc and 2·fc, plus 0.3 of a 110-380 Hz band for the thump, then HP 70 Hz.
      4. Room: 8 ms travel, a 20-30 ms pre-delay, then 18-22 stereo early reflections at 12-85 ms.
      5. Late tail with band-dependent decay: lows longest, highs shortest.
      6. Far dark hall tail: about 3 s, LP 1.5 kHz.
      7. Air: LP 6 kHz.
    - **Working balance:**
      - Wall fc 3.6 kHz, opening to 4.8 kHz over 0.7 s at the hook.
      - Gains: direct 3.0, early 0.35, late 0.22, hall 0.14.
      - Measured: T30 about 2.6 s, C50 about +2.7 dB, L/R correlation about 0.16.
    - **Bounds:** a 0.28 s tail with no early reflections read as not spatial enough. Wall fc 1.5 kHz with C50 about -0.5 dB and a 3 s tail read as inaudible.
    - **Pre-roll:** process 3 s of pre-roll and trim it off, so the reverb is already built at clip 0.
    - **Bed:**
      - Laptop fan through the same chain, at about -40 dB.
      - Outside traffic plus passing cars behind a 1.3 kHz window low-pass, at about -34 dB.
      - No clock ticks and no string plucks: periodic short transients smear under the reverb into a recurring "챱챱".
  - **From PC speakers on her desk, heard across the room** ("공간음향 이펙트"; no wall in between):
    - Use the same chain with the closet and wall stages replaced by a desk IR (reflections at 1.6, 3.1 and 4.8 ms) and LP 7 kHz.
    - Add 10 ms of travel, and bias the speakers slightly toward the desk side (L x1.08, R x0.94).
    - Room: rt 2.0/1.5/0.6 s with a 22 ms pre-delay, plus a far tail of 2.8 s at LP 1.6 kHz.
    - Gains direct 2.2, early 0.40, late 0.30, far 0.14-0.18. Measured: C50 +2.7 dB, T30 2.3 s, L/R correlation 0.57, syllable match 0.74.
    - Direct 3.0 measured C50 +6.7 dB and correlation 0.77, which is too dry for a request for space.
    - Bed: PC fan through the chain at about -42 dB, and city rumble behind a 1.2 kHz low-pass at about -41 dB.
  - **Measure, since you cannot listen:**
    - **C50 and T30:** send an impulse through the exact chain. C50 is the energy before 50 ms against the energy after; T30 comes from the EDC slope between -5 and -35 dB. Aim for C50 of about +2 dB or more.
    - **Lyric syllable match:** take the 300-3400 Hz envelope of the mix and of the dry song over the window, band-pass both to 2-12 Hz (syllable rate), and correlate them. Adding space should not lower the score.
    - **Transient scan:** per 20 ms window, count where the bed's 2-8 kHz level is above the song's. The count should be zero.
    - **Report:** give the numbers, and say once that the tuning was done by measurement.
  - **Wind bed:** brown rumble plus hiss plus a narrow whistle that follows the gust curve.
- **Finish:** alimiter, then two-pass linear loudnorm to -14 LUFS / TP -1.5. Parse the pass-1 JSON in code on every run (`stderr[stderr.rindex('{'):stderr.rindex('}')+1]`), so measured values never go stale after a remix. Confirm with ebur128 on the mux, and confirm the vocal band jumps at the promised clip time.

## 5. Delivery checks

- ffprobe the stream and frame counts, run a full `-f null` decode, and run ebur128.
- Vision-read a decoded frame strip (2-2.5 s apart) and one phone-size frame (a "3-second read").
- **Discord copy under 10 MB:**
  - Encode at CRF 18 first and check the size. Two-pass is needed only above about 9.5 MB. A dark, mostly static 30 s scene came out at about 3.5 MB; 15 s at CRF 17 came out at about 3.8 MB. A 25 s scene with an animated broadcast on a TV, cut flashes and scanlines, came out at about 9.4 MB at CRF 18, which is near the cap.
  - Busy screen footage at 20 s came out at about 12 MB at CRF 18. Two-pass at about 3.4 Mb/s gives about 8.8 MB; for longer cuts, scale that bitrate by 20/duration.
- **Report:** what the picture shows, what moves, the song segment and where the hook lands, and the limits. Limits include a scene not yet animated (the still was generated from the manga composition) and the platform mute risk.

## 6. Proposing the next scene

- When asked for candidates "in a similar mood", send the list as soon as it is asked for, before starting any build.
- Group 5-6 options by mood, for example a decadent night room versus a meme song leaking from a device. For each, give the scene, what moves (light, smoke, rain, screen), and the song with how it reaches the room (radio, PC speaker, earphones, behind a door).
- Number the options continuously across groups (1-6), so a bare reply such as "2 ㄱㄱ" picks one without ambiguity. A pick by number is the go-ahead: start building in that turn, with no confirmation question.
- Recommend one. Say which problem from the last cut it avoids, such as one large prop instead of small ones, or light from a screen instead of tiny figures on it.
