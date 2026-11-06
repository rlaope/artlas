# Human form through the pixel pipeline

Use for scenes with people, before approving their base, grid or motion; repeat the affected checks when feedback reports smearing, damaged anatomy or unnatural acting. This applies to visible form, not a demand for a detailed or exposed face. Native-game scenes retain their original head/body proportions, missing facial features, palette and intentional clusters; do not impose anime anatomy or a finer face patch.

## Record what must remain visible

On the accepted source, mark the intended head direction, exposed face/ear/neck planes, hair overlaps, limbs, cuffs and support or grip contacts. Record deliberately hidden features and their actual occluder: bangs, sleeve, shadow or camera angle. Mark only those unseen features not applicable. The visible head-to-neck-to-torso connection and whole silhouette still need review where present in the chosen shot. Follow the current canonical design and intentional occlusion, crop or injury contract: a natively noseless sprite, covered eye, out-of-frame limb or explicitly requested missing limb is not automatically malformed. Review the visible attachments and boundaries rather than imposing universal feature or limb counts. Do not uncover the face, invent eyes/nose/mouth or change the chosen camera/gaze to make a failed check easier.

A supported pose needs a plausible chain: for example, forehead rests on one continuous sleeve/forearm, elbow meets the desk, and the other wrist connects to the hand gripping gathered fabric. Inspect the whole actor before a finger or face crop. Extra cuffs, a thick palm, a disconnected jaw or a limb that only seems connected under a dark grade are source issues even if their pixels remain unchanged later.

## Locate the first loss before choosing a repair

Keep one aligned comparison at the accepted framing: source crop, unquantized logical-grid reduction, palette result, posed/composited frame and decoded MP4. Inspect native cells and phone-width context; close crops diagnose, but do not establish readability in the scene. Keep lighting off and on comparisons where illumination could conceal the cause. Inspect a nearest-neighbour native enlargement separately from the actual player at its normal display size; display interpolation can soften a sound native frame. Attribute player blur only after comparing the same decoded frame through both views, not as an excuse for form already lost on the native grid.

| Stage | Required observation | HOLD condition and next investigation |
| --- | --- | --- |
| Source | Accepted pose, identity, proportions, visible features and contacts are intelligible before processing. | Duplicated sleeves, malformed hands or ambiguous head/neck attachment already exist. Resolve the source within the authorized edit scope; a renderer cannot prove a defective source correct. |
| Logical grid, before palette | Visible ear/cheek/jaw/neck, hair separation and hand/contact shapes survive the actual crop and sampling. | Structural planes merge during reduction. Compare grid, sampling and framing choices against the requested dot read; export resolution cannot restore them. |
| Palette and cleanup | Required local value/colour differences survive without inventing detail. | Skin/hair/cloth merge, or a small feature disappears after quantization, denoising or cleanup. Isolate that stage; do not mask the loss with sharpening or a finer-resolution face overlay. |
| Pose and joint assembly | Every complete pose retains the approved design and plausible connections, including neutral and return states. | A donor changes neck length, collar, ear, hair overlap or limb topology. Stable use of that donor across all frames fixes switching, not its design error. Compare the integrated pose to the accepted source before approving temporal continuity. |
| Lighting and compositing | Off, arrival, peak and withdrawal retain the intended visible form; shadows follow surfaces and occluders. | A face becomes a featureless patch, a contour vanishes, or light/backing paints a duplicate form. Compare source-fitted masks and material ranks, not only region means. |
| Encoded phone motion | The complete action, face planes and contacts remain coherent through extremes and immediate neighbours. | Smear, rubber-like motion, joint-design pops, detached clothing strokes or ambiguous human form remain. Record time and visible symptom; a sharp still, low encode error or exact frame hash cannot override it. |

Record **observation** separately from **cause**. “The neck/collar planes are already merged before encoding” is evidence; “the codec did it” needs a matching pre-encode/decoded difference. For example, a consistent neutral joint atlas can already alter the collar/neck design in the first light-off frame. If the encode reproduces it, neither passing light nor the codec originated that persistent design change; inspect the donor integration first. An intentionally covered frontal face is not evidence of missing frontal eyes. If a visible structure remains ambiguous, keep that gate on HOLD instead of inventing a feature or declaring occlusion a blanket exemption.

## Judge body action, not just transport

Plan the small physical action and its supports. In a slumped supported pose, upper back, shoulder, chest and neck must respond as one believable body; a gripping wrist, fingers and gathered cloth stay attached. Hair and loose fabric may lag, but cannot provide the only movement when body acting was requested. Preserve deliberate stillness of an unresponsive subject.

Choose a motion method suitable for that action; no method name guarantees anatomy. Rigid parts can look hinged, authored cels can switch designs, and a connected mesh can make flesh or clothing ripple. Do not impose a universal warp ban or solve a failed acting gate by freezing the requested body and moving only ambience.

For transformed or deformed parts, compare matched source/pose landmarks, silhouette and material planes at extrema and returns. Where useful, inverse-align a posed crop using its recorded mapping to expose stretched widths, unstable facial planes or donor drift. Also inspect the forward outline, newly revealed area and support contacts: a correct inverse landmark inside the body does not prove the outer silhouette moved correctly. Expected physical bending or foreshortening is not a requirement for identical pixels; judge it against the intended pose. Never let inverse error, fixed-contact counts or hash equality approve geometry on their own.

Use a short encoded proof covering each changed pose family, neutral crossing, extreme and return. Review light-off acting to separate body motion from passing light, then the combined scene at phone width. Check immediate neighbours around joint/donor switches and mask boundaries. The body should read as acting without a moving-region annotation; a repeating surface wave or independently sliding coat panel fails even when every layer is connected mathematically.

## Decision and scope

Keep one compact record per affected condition: `visible requirement / intentional occlusion / first failing stage / timestamp or pose / observed defect / supported cause or uncertainty / next scoped change / evidence / verdict`.

A new user report reopens that condition as **HOLD**, superseding the relevant earlier aesthetic pass. Reuse unchanged technical evidence only for the claim it establishes. The independent reviewer must examine current source-to-grid form and actual encoded motion, not merely the author's assertions. Passing the short proof permits the full candidate; check the exact saved final through late poses and returns before delivery.

For diagnostic-only or skill-only requests, follow the [feedback loop scope guard](feedback-loop.md): preserve production hashes, isolate any necessary controls, and update evidence/procedures only. A committed skill improvement does not mean the reported video has been repaired or passed again.
