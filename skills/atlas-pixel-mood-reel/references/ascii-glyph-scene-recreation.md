# Recreating an ASCII-glyph scene with a swapped pixel sprite

Use when the user points at a reference video (e.g. an X post) and asks for "the same style, but with our character flying".

## Measure the reference first
- Fetch media without the browser: `curl https://api.fxtwitter.com/<user>/status/<id>` gives every MP4 variant; download the 1080p one.
- Measure, don't eyeball: glyph cell size from 2x nearest crops (the observed reference used 6x10 px cells at 1080p, 320x108 grid), ring radii from a radial occupancy scan, terrain scroll speed by best-shift of consecutive frames (the reference moved about 1-3 px/frame, the ring stayed static, the sprite only bobbed).
- Compare your still against the reference with the same numeric instrument (row-band mean luminance, cell coverage >90, radial band range). Vision critiques of downscaled side-by-sides repeatedly reported a "tent-shaped" ring that the radial scan showed matched within ~5%; trust the scan.

## Glyph rendering pitfalls
- Build an atlas once (PIL text into 6x10 tiles) and compose frames with numpy (`atlas[gidx] * bright`); about 0.08 s per 1080p frame.
- Menlo Bold (`Menlo.ttc` index 1) has no U+2500 `─`: it renders a tofu box. Draw that tile by hand or use Regular. Check every glyph tile as ASCII art before rendering.
- Glyph strokes are thin; multiply atlas alpha ~1.35 to match the reference's perceived brightness.
- Terrain that reads as the reference: filled massif (density decaying ~13 rows below crest with a floor), heavy glyphs (`■ ◆ # X o`) only near crests, foreground restricted to slope-directed `/ \` plus `. '`. Outline-only crests read as teeth; uniform `= o x` foreground reads as confetti.
- Halo: chained `/` (left) and `\` (right) on every 3rd cell diagonal, ~100 px wide, fading by density not brightness.

## Seamless loop
- Make every parallax period divide the loop: with F frames and layer period P px, offset = f*P//F. Example used: F=960 (32 s), periods 1920/2880/3840 px -> 2/3/4 px per frame. Bob/twinkle periods must divide F too. Assert `frame(0) == frame(F)` before rendering.

## Character sprite from an avatar
- `image_generate` with `image_url=<avatar>` and a prompt for a coarse-grid sprite ("about 44x34 art pixels, big flat blocks, flat colors, solid #FF00FF background") gave a usable base. Passing `reference_image_urls` together with `image_url` failed with 400 "'image' is required" on the OpenAI route; use `image_url` alone.
- Snap it: key magenta, map every pixel to a fixed named palette, take the per-cell dominant palette index (boost small features: eye, buckle, lining), prune isolated pixels, add a cool rim on dark edge pixels so black hair/coat separate from a near-black background.
- Black-haired characters on a #1c1c1c scene need a lighter navy coat and a slate rim; otherwise the body dissolves.

## Delivery
- Master: x264 crf 18 `-tune animation`. Discord share: two-pass ~2.3 Mb/s, 1080p 32 s lands about 9 MB, under the 10 MB bot cap. Verify decoded frames at 1:1 for glyph smear and check the loop seam diff equals a normal consecutive-frame diff.
