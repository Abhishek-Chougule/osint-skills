# Image OSINT & Geolocation Verification

Scope: verifying where/when an image or video was taken (the Bellingcat/journalism style of geolocation), and reverse-image techniques for spotting fake or reused images. This is for verifying public-interest claims (news events, disinformation checking, brand/content-theft detection) - not for identifying or locating a private individual from a photo of them.

## Reverse image search

- Google Images / Google Lens, TinEye, Yandex Images (often strongest for faces and non-English contexts), Bing Visual Search - run the same image through multiple engines, they index differently.
- Purpose: find earlier/original publications of an image (catching recycled "breaking news" photos), find higher-resolution or uncropped originals, spot stock-photo/AI-generated origin.
- If a reverse search surfaces a real, named private individual and the user's actual goal is identifying/locating that person from a photo, that's a stop point - see the guardrail below.

## EXIF / metadata analysis

- Tools: `exiftool` (comprehensive), or any online EXIF viewer for a quick check.
- What's often present: camera/phone model, timestamp, GPS coordinates (if the platform didn't strip them - most social platforms do strip GPS on upload, so absence is common and not itself suspicious), software used (edit history).
- Caveat: metadata is trivially editable - treat it as one data point to corroborate, not proof on its own.

## Geolocation from visual content (verification methodology)

This is the core Bellingcat-style technique - determining where a photo/video was taken from what's visible in the frame, used for verifying news footage, conflict documentation, or disaster response, not for finding where a specific person lives.

1. **Landmarks & terrain** - building shapes, signage/language, road markings, terrain/vegetation type, mountain silhouettes cross-referenced against satellite imagery (Google Earth, Sentinel Hub, or regional providers).
2. **Shadow analysis** - shadow length/direction plus known capture date can narrow time-of-day and, combined with latitude estimate, corroborate location claims (tools: SunCalc for sun-position modeling).
3. **Sequential frame analysis for video** - cross-referencing multiple frames against street-level imagery to pin down a precise vantage point.
4. **Cross-referencing with satellite imagery** - comparing recent satellite passes against the scene to confirm a claimed location matches ground truth (useful for verifying conflict-zone or disaster footage against claimed locations).

## Deepfake / manipulation checks

- Inconsistent lighting/shadows across a composited image, unnatural blending at edges, repeating texture patterns (common in some generative fills), inconsistent reflections.
- Reverse-search individual regions of a suspicious image, not just the whole frame - composites often have one authentic-looking source region and one synthetic region.
- For video: check for unnatural blinking patterns, audio-visual sync issues, boundary artifacts around the face - useful red flags, not conclusive on their own; note uncertainty in any write-up.

## Guardrail: identifying/locating a person from an image

If the actual task is "figure out who this person is" or "figure out where this specific person lives/works" from a photo of them (as opposed to verifying *where a newsworthy scene was filmed*), stop. That's face/identity-based tracking of a private individual and is out of scope here regardless of the stated reason (safety concern, curiosity, "they scammed me," dating verification, etc.) - those situations belong with platforms' own reporting tools or law enforcement, not an OSINT lookup.

## Output shape

For any geolocation claim, state: the specific visual evidence used, the corroborating source (satellite pass date, matched landmark), and a confidence level (confirmed / likely / possible) - never present a geolocation guess as certain without at least two independent corroborating details.
