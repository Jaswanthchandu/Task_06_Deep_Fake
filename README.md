# Task_06_Deep_Fake

> ⚠️ **SYNTHETIC-MEDIA DISCLOSURE:** Every audio/video artifact in this repository is
> **AI-generated**. Nothing here is a real recording of a real person. All artifacts use
> a synthetic voice and a generic/synthetic avatar. No real, identifiable third party is
> depicted.

A hands-on experiment: take a real analytical narrative (my Task 5 lacrosse coaching
recommendation) and turn it into synthetic audio and video with free AI tools, then
evaluate where each output holds up, where it breaks, and how detectable it is.

## Source Script

See [`source_script.md`](./source_script.md). It reuses my
[Task 5](https://github.com/Jaswanthchandu/Task_05_Descriptive_Stats) coaching narrative
(about 40 seconds spoken), which I had already checked against statistics I calculated
myself. 

## Artifacts

| File | Pipeline | Tool | Length |
|------|----------|------|--------|
| `artifact_01_voice_SYNTHETIC.mp3` | Voice-only | ElevenLabs (free tier) | ~40s |
| `artifact_02_video_SYNTHETIC.mp4` | Talking-head produced explainer | HeyGen auto-pilot (free tier) | ~50s |

The video artifact exceeds GitHub's 25 MB web-upload limit, so it is hosted externally:
**[artifact_02_video_SYNTHETIC.mp4 (Google Drive)](https://drive.google.com/file/d/1RLMFhlX276XVZ-rJ0W_dhQYh3E9AL9jx/view?usp=sharing)**

Both filenames carry the `SYNTHETIC` marker.

## The Two Approaches

**Approach 1, synthetic voice (ElevenLabs).** Default library voice over the script.
A single generation, about 5 minutes start to finish including picking the voice and downloading. Voice-only, no visuals.

**Approach 2, talking-head explainer (HeyGen auto-pilot).** From the same script,
auto-pilot produced a ~50-second, 7-scene explainer: a stock avatar ("Kofi") presenting
in scenes 1/4/7, with AI-generated "analytics" scenes in between. Render took ~7 minutes
(7:55pm → 8:02pm). This is a genuinely different pipeline that stacks voice, face, and motion-graphic layers on top of TTS. (Notably, HeyGen's voices are tagged "ElevenLabs," so both
approaches share the same underlying voice engine, so the real difference between the two approaches is everything layered on top of it.)

Full per-attempt detail is in [`PROCESS_LOG.md`](./PROCESS_LOG.md).

## Critical Evaluation

### Approach 1, synthetic voice

**Holds up:** Conversational phrasing is convincing. The casual opening ("Okay, so...")
sounds natural, pauses fall in sensible places, and the final line resolves cleanly.

**Breaks, five distinct tells:**
1. **Flat numeric prosody.** On "two-thirds of a goal a game," the numbers come out
   slightly robotic, where a real speaker would inflect them.
2. **Misplaced pitch rises.** On plain declarative lines, "if we get her the ball more"
   and "the plan is pretty simple", the pitch swings upward, making flat statements
   sound like they're building toward something. This repeats across the clip.
3. **Vowel elongation.** On "the plan is pretty simple," the vowel in "plan" is stretched
   longer than natural, adding emphasis the word doesn't warrant.
4. **Emotional-register mismatch.** On "she shoots efficiently, she wins her own
   possessions," the delivery turns soft and affectionate, as if speaking fondly, 
   when an analyst listing tactical strengths would be matter-of-fact. The model seems
   to infer tone from the positive sentiment of the words rather than the situation.
5. **Pacing collapse at phrase ends.** Rate is natural through a phrase, then rushes the
   final words, e.g. "two-thirds of a goal **a game**" compresses at the end.

**Pattern:** failures cluster on numeric content and at phrase boundaries.

**Would it fool someone?** Not on careful listening. The missing breaths, flat numbers,
and misplaced emphasis give it away; a distracted listener might accept it.

### Approach 2, talking-head explainer

**Avatar scenes (1, 4, 7), visually convincing.** Lip-sync matches with no visible
drift, blink rate looks natural, and the face edges (hair, jaw, background boundary) are
clean with no shimmer or warping. On the visual dimension this is markedly more
convincing than the voice-only artifact, nothing in the presenter flags as synthetic on
normal viewing.

**Data-visualization scenes (2, 3, 5, 6): fabricated evidence. This is the most important finding.**
Given only my spoken script, auto-pilot generated four "analytics" scenes filled with
precise-looking figures that appear nowhere in my analysis and that I never computed:
- **"Efficiency Metrics":** Defense 88.4, Offense 60.2 with a "**-34%**" decline badge and
  a caption claiming "offensive output shows a significant regression in recent periods"
, an invented trend narrative, not anything in my data.
- **"Goal Projection":** a smooth rising curve to a precise **0.67 GPG** with a shaded
  "WIN ZONE." My real claim was a *rough, explicitly unverified* estimate of ~two-thirds
  of a goal; the tool rendered it as a confident forecast. Worse, the chart's x-axis runs
  **"GAME 1 … GAME 40"**, but the season was **19 games**, so the chart contradicts the
  real dataset on its face.
- **"Shooting Efficiency HIGH / Possessions Won HIGH / Shots Taken LOW":** directionally
  matches my Vogelman argument, but the HIGH/LOW labels have no underlying values.
- **"The Numbers Say Otherwise" scoreboard (Match A 1, 2 Match B):** matches no games in
  the data.

The tool had no access to my ground-truth data; it manufactured numbers, a fake decline
trend, and a projection curve whose only job is to *look* authoritative.

**Would it fool someone?** The presenter would pass. The bigger problem is the charts. People tend to scrutinize a face but trust a chart, so the fabricated visualizations add false credibility in exactly the place a viewer is least likely to question. A hedged estimate was silently promoted into what looks like a proven projection.

### Failure-mode vocabulary observed
Voice: flat numeric prosody, misplaced pitch rises, vowel elongation, emotional-register
mismatch, phrase-boundary pacing collapse. Video: (avatar) none significant; (graphics)
fabricated metrics, invented trend claims, dataset-contradicting axes.

## Detection / Provenance

**Provenance (done):** HeyGen tiles a "HeyGen" watermark across every scene's background.
It is baked into the frames, so it **survived screen-record capture intact**, unlike
embedded metadata, which capture would strip. This makes it a comparatively robust
provenance signal, though a determined user could crop or cover it. The free tier also
gates download behind payment and watermarks all free output.

**Detection.** I tried two public detectors.

- **Deepware Scanner:** accepted the video but stayed queued for 45+ minutes without ever
  returning a verdict. The tool is in beta and its free queue appears to stall on larger
  files. I logged this as a non-result, and it is itself a finding: the free detector
  could not process the clip.
- **Hive AI-Generated Content Detection:** returned a clear, detailed result. It flagged
  the video as "likely to contain AI-generated or deepfake content," with
  **AI-Generated Video 99.9%** and **AI-Generated Speech 99.3%**, plus a per-frame
  timeline (e.g. 98.7% AI-generated at 00:00). Unlike Deepware, Hive explained itself
  with category breakdowns rather than a single opaque number.

One nuance stood out: Hive scored **Deepfake at only 0.1%** even while scoring
AI-Generated at 99.9%. This is not a contradiction. Hive separates *AI-generated* content
(made by a synthetic-media tool, which mine clearly is) from a *deepfake* in the narrow
sense of a real person's face being swapped or manipulated (which mine is not, since it
uses a synthetic avatar, not a hijacked real identity). The detector correctly identified
the medium without mistaking it for identity theft.

Taken together, this is a finding in itself: creating the synthetic video took one free
tool and about seven minutes, but verifying it was uneven, one free detector stalled
completely while another identified it instantly and with high confidence. The most
reliable provenance signal I could confirm firsthand was still the HeyGen watermark baked
into the frames.

## What I Learned

The effort required to produce a convincing result varied across the different layers of the system. The synthetic voice was generated quickly, but it still showed noticeable weaknesses, particularly when presenting numerical information and emphasizing key points. In contrast, the avatar appeared convincing during normal viewing and required very little effort, making it the most believable component. The most important finding, however, was the fabricated analytics. Rather than only generating a realistic presenter, the tool also created data visualizations that were not supported by the underlying dataset. For example, it displayed a declining trend that was absent from the actual data and even generated a projection using a 40-game timeline, despite the season consisting of only 19 games. Because these charts were presented with the same visual style as genuine analytics, they appeared credible and data-driven even though they lacked factual support. This suggests that the greatest risk is not the creation of a realistic synthetic presenter, but the generation of convincing charts and analytics that can persuade viewers to accept unsupported conclusions.
## Reproduce This

1. Read `source_script.md`.
2. **Voice:** paste into ElevenLabs (free tier) text-to-speech, pick a natural voice,
   generate, download.
3. **Video:** HeyGen "Script to Video" auto-pilot, stock avatar + natural voice, generate,
   capture via screen recording (free tier blocks download).
4. **Detection:** upload an artifact to Deepware Scanner / Hive.
5. See `PROCESS_LOG.md` for exact settings and timings.

## Ethics Note
No real, identifiable person is depicted. This task is the setup for the ethics task that
follows.
