# Review an Atlas motion revision

Use this loop when feedback changes an approved scene's acting, mood or sound. Keep the accepted base identity and current request in the spec; the [skill](../SKILL.md) is the authority for the visual rules.

1. **Define the change.** Record the feedback, affected timestamps and what the viewer must perceive. Name the accepted source ID/hash, preserved character cues, text policy, frame rate and exact music-file window. This is ready when each requested change has an observable acceptance condition.
2. **Make a short proof.** Render the affected passage with the final pixel grid, cadence and audio treatment. Include the motion peak and both transitions. Decode a phone-width view, neighbouring-frame burst and representative stills from that MP4. This is ready when the proof covers each changed beat rather than only its most attractive still.
3. **Review in a separate pass.** Give another reviewer the request, spec and exact proof file/hash. Score the visual axes below from 1-10; 7 is the default pass level. Record audio measurements and whether listening occurred. Each low score needs a timestamp, visible defect and concrete next change.
4. **Fix the weakest affected beat.** Correct the identified issue and render that passage plus its transitions again. Reuse evidence only for unchanged regions or streams whose hashes match. Continue until the requested conditions pass. If a condition cannot be met with this base, report the remaining condition, reason and evidence instead of marking it passed.
5. **Verify the final export.** After the last revision, inspect the exact requested exports, complete decode, cadence, sound and delivery hashes. For a dashboard, check real play, later seeking, end looping and mute restoration. Record the final verdict and file hashes; only then register or commit a claimed finished result.

| Axis | What the reviewer must establish |
| --- | --- |
| Acting visibility | Head, breathing, hair or expression changes read in phone-width playback. Unique frame hashes alone are insufficient. |
| Character identity | Face, silhouette, outfit and identifying colours still match the accepted base. |
| Edge continuity | Neck, hat, hands, braids and revealed background stay connected without doubled edges, holes or warping. Smoke starts at the moving ember. |
| Gloom and light | The chosen cold or warm mood persists; the dominant source and any requested secondary light reach their intended planes while the room stays dark and readable. |
| Audio clarity and distance | The requested passage begins at the intended moment. Dry/wet balance, C50/T30, vocal-envelope correlation, transient scan and loudness support audible distant music. Listening, when available, evaluates the perceived space; measurements alone are labelled as such. |

Keep a compact record: `feedback | timestamp | acceptance condition | revision | evidence | verdict`. Store source URLs, offsets, hashes and measurements; keep song files and copied lyrics out of this repository. Start the next review by opening the short encoded proof at phone width.
