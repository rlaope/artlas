# Roxy, after midnight

Roxy Migurdia sits in a cold room with a cigarette and a faint candle. A narrow band of moonlight enters through the right curtain gap, reaches her face, braids and lap, then fades as though clouds cover the moon. This is a fan-made situation, with the accepted blue braids, witch hat and magician robe preserved.

The requested format is 24 seconds, 1080×1920 at 60 fps, with no on-screen titles, captions, lyrics or decorative labels. The same clean full-frame master serves both exports; the video and music files are not stored in this repository.

## What the feedback changed

- **Identity and mood:** lock the accepted source by ID and hash, retain Roxy's canonical silhouette, and keep the grade cold and dark rather than amber and cosy.
- **Visible acting:** make the head and hat incline together around the neck, deepen shoulder breathing and add delayed braid-tip follow-through. Use a matched closed-eye plate for two slow blinks and a clean room plate for newly revealed edges. Smoke starts at the moving cigarette tip.
- **Moonlight:** preserve that acting and add only the source-shaped light pass. Light arrives over 3–9 seconds and fades through cloud-like occlusion from 13.5–22 seconds. The candle and unlit room corners do not follow the moonlight's exposure.
- **Sound:** align the requested passage to the actual source, then put the music behind a karaoke-room door. Keep breath and cloth effects close to the listener, outside the distant music's reverberation path.

## Proofs and measurements

An independent review of the first encoded acting proof rated visibility 8/10, identity 9/10, edge continuity 8.5/10, gloom 9/10 and blink naturalness 8/10. The decoded head/face change was 21.21× the initial version, with braids 2.75×/3.44× and chest 3.15×. These comparisons support the phone-size visual review; they are not a substitute for watching the movement.

The rendered moonlight stills, before video encoding, showed a peak affecting about 9.51% of the logical canvas. At 9.5 seconds the near face gained 20.30 luma and the lap/hand 8.29, while the far wall, dark corner and candle gained zero. At 23 seconds all measured moonlight lifts were zero. Unreached pixels keep the base palette; extra cold ramps are used only where the beam reaches. With moonlight disabled, five sampled frames matched the previously accepted acting exactly.

## Music timing and space

The soundtrack is Mitski's [My Love Mine All Mine, official video](https://www.youtube.com/watch?v=vx4kLgnFexo). The requested approximately 1:49 vocal cue was aligned against the [official lyric video](https://www.youtube.com/watch?v=CwGbMYLjIpQ), whose audio matched this source at a 31.649-second offset (correlation 0.9314). A [timed phrase estimate](https://lrclib.net/api/get/36326470) put the cue at 110.569 seconds in this file; the cut starts at 110.349 seconds to retain its opening. The source window is 110.349–134.349 seconds. No lyrics are reproduced here.

The path uses a 150 Hz speaker high-pass, 6.5 kHz speaker low-pass, wall low-passes at 2.1/4.2 kHz, restrained box resonance and wow, early reflections and dark hall tails. Wet/direct RMS was calibrated to 0.78 on this song. The mix measured vocal-envelope correlation 0.8148, IR T30 2.307 seconds and music 24 dB above room ambience. Subjective listening was not observed; these are measurement results.

The reusable procedure is in [the feedback-loop reference](../skills/atlas-pixel-mood-reel/references/feedback-loop.md). Keep full-resolution source/export hashes, decoded proofs and the final independent verdict with a production project, and preserve the approved master bytes when copying to a library.

## Final export verification

The independent review of the exact v3 exports passed: the full arrival over 3–9 seconds, gradual withdrawal and completely unlit ending at 22.5–23.983 seconds retain the cold mood, accepted character and text-free frame. All 1,440 decoded frames are unique; full decode and the requested 1080×1920, 60 fps, 24-second format passed. The final mux copies the reviewed silent video's packets exactly, and the decoded audio matches the measured Mitski mix at correlation 0.9999966. Sampled scene regeneration at 0, 9.5 and 23.983 seconds passed with access to previous project folders denied; no denied open was attempted.

Both full-frame exports are 10,162,552 bytes and share SHA256 `6ebeeed6a275a88a08a54e724183c1057f8c3d190e90238c8561ce90c60e0523`. Audio is AAC, 48 kHz stereo; the delivery meter reads −14.0 LUFS integrated and −2.83 dBTP. The gallery image is a decoded 9.5-second frame of that exact final master, resized only for the still gallery. Library copies must retain the approved master bytes.
