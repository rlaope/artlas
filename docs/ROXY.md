# Roxy, after midnight

Roxy Migurdia sits in a cold room with a cigarette and a faint candle. Narrow moonlight enters through the right curtain gap, reaches her face, braids and lap, then fades as clouds pass. The accepted blue braids, witch hat, modest magician robe and left-hand cigarette composition remain intact.

The current v4 delivery is one text-free, full-frame MP4: **24 seconds, 2160×3840 at 60 fps**. Additional exports require an explicit request; library copies preserve the approved master bytes. Video and music files are not stored in this repository.

## Repairing the rejected image and blink

The requester rejected v3's eye closure and picture quality. A source/logical/encoded comparison located most detail loss before encoding: reducing the 941×1672 drawing to 216×384 and limiting its colours merged iris, lash and hair detail. The old 0.9-second eyelid wipe left conflicting contours and switched abruptly to a separate shut-eye result near full closure. Its closed-eye plate also contained cyan arcs.

V4 samples the approved drawing directly onto a 540×960 grid, preserving the source's pixel clusters and local dark shade ramps without a global 160-colour cap. It performs one nearest-neighbour foreground warp per frame, then enlarges the grid ×4. The logical canvas contains 6.25 times as many cells as v3. The detail gain is established by matching source, pre-codec and decoded face crops, including a 360-pixel phone view.

New built-in imagegen edits supply aligned half-closed and closed-eye plates. Only the measured eye interiors change; the eye corners and overlying bangs stay anchored. Curved lash correspondence carries the open, half and closed drawings through one continuous motion. Each blink lasts 0.30 seconds, beginning at 6.88 and 17.88 seconds: 110 ms closing, 30 ms shut and 160 ms reopening. The single dark lash replaces the rejected glowing inner arc. A raw continuity probe across closure amounts 0.994–0.995 measured a maximum RGB change of 0.043, replacing the old branch's 68.57 jump; actual neighbouring encoded frames were also reviewed.

Gentle head, shoulder and braid motion remains visible, with smoke emitted from the moving cigarette. Moonlight arrives over 3–9 seconds and disappears over 13.5–22 seconds while the room corners and candle retain their darkness.

## Independent final review

A fresh reviewer inspected both encoded blink sequences, native detail crops, phone-width playback, head motion and the full moonlight timeline. Scores were blink naturalness 8/10, detail 8.5/10, pixel readability 8/10, identity 9/10, edges 8.5/10 and gloom 9/10. The exact final master passed full decode and actual 0–24-second browser playback. It contains 1,440 frames with presentation steps of 1/60 second, within 1.1 microseconds.

The single final master is 14,326,526 bytes with SHA256 `9c0f5c5ecf2e78eb982c50deabc8a7a701b42234782da2e58faecf766c886df9`. Both video and audio were copied during muxing. AAC is 48 kHz stereo, measuring −14.0 LUFS integrated and −2.83 dBTP. The gallery is the decoded 9.5-second frame, reduced to 1080×1920 only for the still gallery; library copies must retain this master’s bytes.

## Music timing and space

The soundtrack is Mitski's [My Love Mine All Mine, official video](https://www.youtube.com/watch?v=vx4kLgnFexo). The requested approximately 1:49 passage was aligned against the [official lyric video](https://www.youtube.com/watch?v=CwGbMYLjIpQ). Correlation found a 31.649-second source offset; a [timed phrase estimate](https://lrclib.net/api/get/36326470) placed the cue at 110.569 seconds, so the 24-second cut starts at 110.349 seconds. No lyrics are reproduced here.

The music retains the reviewed karaoke-room-through-door treatment: speaker and wall filtering, restrained box resonance and wow, early reflections and dark corridor tails. Close breath and cloth effects stay separate. V4 copies the reviewed AAC packets exactly; their SHA256 remains `a98ed3344e8985d9cd77fa14eb67967a880d823e3548a81a7fb7c35fb4ae77ed`. Subjective audio listening was not claimed.

The reusable procedure is in [the feedback-loop reference](../skills/atlas-pixel-mood-reel/references/feedback-loop.md). Keep actual decoded proofs and the exact final verdict with a production project, and compare against the requester's rejected output before accepting a repair.
