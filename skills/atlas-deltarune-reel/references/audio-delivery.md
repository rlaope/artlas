# DELTARUNE reel sound, review and delivery

Use this reference for a requested song or cover, speed/space treatment, separate effects and the final movie. These recipes are configurable; no specific song, sadness treatment, karaoke wall or playback rate belongs to every DELTARUNE scene.

## Source window and optional slower playback

Record source title, creator, URL, downloaded file hash, sample rate, selected source start and the intended clip cue. Distinguish the original game recording, an official upload and an unofficial cover. Align a requested timestamp or phrase on the actual file; a music-video intro or cover arrangement may differ. Do not move a requested opening cue to a generic hook ten seconds later. Keep downloaded music and copied lyrics out of the skill repository.

When slower playback is requested, choose and record the rate and pitch policy. `atempo=rate` changes tempo while retaining pitch; changing the interpreted sample rate changes both speed and pitch. See FFmpeg's [atempo](https://www.ffmpeg.org/ffmpeg-filters.html#atempo) and [asetrate](https://www.ffmpeg.org/ffmpeg-filters.html#asetrate) documentation. Choose the policy from the request rather than silently assuming the two are interchangeable. A rate such as 0.94 is a possible slight slowdown, not a required value or a claim that a current mix passed QA.

For output duration `D`, rate `r` consumes roughly `D*r` source seconds, plus processing pre-roll/tail. A source cue at `s` reaches output time `(s-source_start)/r`. Recheck music/SFX alignment and phrase boundaries after a rate change; stretching an old combined mix also stretches contact cues and ambience that may need to stay independent.

These executable examples create fresh PCM files from a project-local stereo source. They use the same source start, 0.94 rate and 24-second output to demonstrate the two pitch policies; adapt the paths and numbers to the scene:

```bash
ffmpeg -hide_banner -i music-source.wav -af "atrim=start=17.4,asetpts=PTS-STARTPTS,atempo=0.94,atrim=duration=24,asetpts=PTS-STARTPTS" -ar 48000 -ac 2 -c:a pcm_s24le music-slower-pitch-kept.wav
ffmpeg -hide_banner -i music-source.wav -af "atrim=start=17.4,asetpts=PTS-STARTPTS,aresample=48000,asetrate=45120,aresample=48000,atrim=duration=24,asetpts=PTS-STARTPTS" -ar 48000 -ac 2 -c:a pcm_s24le music-slower-pitch-lowered.wav
```

Ensure the source has enough samples; these commands do not invent a missing ending. Retain dry PCM and process room pre-roll before trimming when a reverb must already exist at clip 0. Use short easing that preserves the first intended attack, and choose a musical ending instead of applying an arbitrary long fade over the requested cue.

## Independent stems and restrained effects

Keep dry music, treated music, water/fountain, wind/air and any one-shot contacts as separate stems before the premix. This makes a new “slightly slower” or “sadder effects” request concrete without altering accepted visuals or stretching every sound together. Changes to an unrequested visual scene are outside an audio-only revision.

For a quiet fountain scene, seeded filtered noise can supply a low water/resonance bed and a separate higher wind/air layer. Shape them with slow irregular envelopes rather than repeated clicks. Low-pass a room tail and use sparse early reflections to place the music; calibrate wet/direct RMS on the actual chosen passage, not only on broadband impulse energy. A narrow resonance can swamp piano or vocals even when an IR's clarity measure looks acceptable.

Add contact, cloth or metal cues only where the current action and timestamp support them. A nonresponsive body does not need a heartbeat, gasp or healing shimmer. Sadness may come from space, restrained dynamics and sparse ambience; do not insert sobs, tears or periodic ticking merely because a prior scene used grief. Preserve the requested tune above the bed and assess each effect separately before the combined mix.

Measure stem RMS balance, silence/clipping, final integrated loudness and true peak. Atlas's starting finish is −14 LUFS integrated and no higher than −1.5 dBTP; adjust only for a requested output target. Two-pass `loudnorm` should use fresh pass-1 measurements on each revision. Measure the **encoded mux** as well as the PCM, since AAC can change peaks and has priming/skip-sample behaviour.

```bash
ffmpeg -hide_banner -i premix.wav -af loudnorm=I=-14:TP=-1.5:LRA=9:print_format=json -f null -
ffmpeg -hide_banner -i final.mp4 -filter_complex ebur128=peak=true -f null -
```

The first command measures; it is not a finished two-pass normalization. Build pass 2 from its returned `input_i`, `input_tp`, `input_lra`, `input_thresh` and `target_offset`, following [loudnorm's measured-input requirements](https://www.ffmpeg.org/ffmpeg-filters.html#loudnorm). Check actual listening when available; otherwise mark it **not observed** and report only measured findings. Never infer that a “sad” mix was subjectively heard from its settings.

## Review the timed encoded result

Make a short encoded proof covering changed actions, pauses, every pose transition and the loop seam, with the final grid, cadence and sound. Give a separate reviewer the latest request, source/spec identifiers and exact proof/hash. Review native frames, integer enlargements and actual phone-width playback; collect immediate decoded neighbours around each affected transition. A pre-encode still does not prove the codec, timing or composite passed.

| Gate | Evidence required |
| --- | --- |
| Story and identity | Whole canonical actor, head attachment, gaze toward the actual target and literal hand/response beats read on the phone. |
| Pixel and temporal stability | Intentional native holds remain crisp; fine edges do not crawl or pop. Compare source, raw native render and decoded neighbours. Container FPS and unique full-frame hashes are supplementary. |
| Grounding and props | Floor light/contact/cast shadows match the actors; torn SOUL cuts and other semantic details survive actual placement, sampling and occlusion. Two components alone cannot pass a tear. |
| Stillness and text | Requested static actors/props stay still through compositing. Only currently permitted wording appears, at natural scale and occlusion. |
| Sound | Requested source cue, rate/pitch choice, effect timing, duration, loudness and peaks pass. Listening status is recorded separately from measurement. |

Record a timestamp, observed defect, evidence and next action for a failed gate. Correct the affected beat within the user's scope, rerender that window plus its transitions and have it reviewed again. Reuse prior evidence only for unchanged hashed sources/regions/streams. If a condition remains unresolved, report it; do not turn a local asset PASS into final-scene approval. A shared head crop or grounded feet may be called pixel-identical only after the appropriate region comparison.

For diagnostic-only or skill-only work, put comparisons outside production, redirect all QA/output paths **before** constructing the renderer, and hash current sources, media and library copies before/after. Update only authorized evidence/procedures. Do not regenerate production, sync its vendored skill, replace an export or claim the scene repaired in that mode.

## Verify and deliver one master

Use the requested resolution/FPS/duration and one integer native enlargement; a bigger container cannot recover a collapsed prop or face. Verify the exact final MP4 with:

```bash
ffprobe -v error -show_streams -show_format -count_frames -of json final.mp4
ffmpeg -v error -i final.mp4 -f null -
ffmpeg -hide_banner -i final.mp4 -filter_complex ebur128=peak=true -f null -
```

Record the file SHA256, actual decoded frame count, dimensions, FPS, duration, codec/colour metadata and audio results. Check timed decoded transitions and the end/loop seam. Play the exact final file and, when registering a dashboard, verify play, seeking, end/loop and download behaviour. A source AAC hash is not automatically the mux's decoded audio identity: inspect packet copy/skip samples or compare decoded aligned samples when claiming unchanged sound.

Deliver **one** latest completed Atlas MP4, not a duplicated normal/shorts pair. Under the user's local delivery convention, place it directly in `~/Downloads/shorts/` with a work/version filename. Provide one absolute-path media preview and download link. Add no unrequested title, margin or alternate encoding. Dashboard playback/download/archive copies must match the approved master's SHA256; quality or capacity limits do not authorize a reduced copy. Extra formats require an explicit request.

Keep a self-contained latest production project with its raw/generated/native sources, pinned source manifest, deterministic processing code/settings, audio source/stems and final review receipts sufficient for regeneration. Within authorized cleanup scope, only after latest-file storage, library registration, full verification and actual playback succeed, move this work's obsolete delivery copies, archive versions and old work folders to the trash. Record exact retired paths; retain small audit/hash records. Do not touch another work or a separate map project. If reproduction or playback remains unverified, report that state rather than declaring cleanup complete. Apply any available current local-delivery instructions when present, with the Atlas single-MP4 rule taking precedence over a generic dual-export default.
