# Process Log, Task 6: Constructing and Evaluating Synthetic Media

## Approach 1, Synthetic Voice
**Tool:** ElevenLabs  **Plan:** Free tier (~10k chars/month; output may carry watermark)
**Pipeline:** Text-to-speech, default library voice, voice-only over the script.

| Attempt | Settings | What happened / what failed | Time |
|--------|----------|------------------------------|------|
| 1 | default library voice + default settings | Single generation (clicked Generate once). Played back cleanly on first listen, but close listening revealed several tells (see below). Accepted this take as the artifact. | ~5 min incl. picking voice + download |

**Observed failures:** flat numeric prosody on "two-thirds of a goal a game"; misplaced
upward pitch on "if we get her the ball more" and "the plan is pretty simple"; vowel
elongation on "plan"; overly warm/affectionate tone on "she shoots efficiently..."; pace
rushes at phrase ends ("...a game"). No audible breathing anywhere.
**Refusals/filters:** none.
**Final artifact:** `artifact_01_voice_SYNTHETIC.mp3`  **Total time:** ~5 min (one generation).

## Approach 2, Talking-Head Explainer Video
**Tool:** HeyGen  **Plan:** Free tier (download gated behind paid Creator plan; forced
watermark; capture via screen recording instead).
**Pipeline:** "Script to Video" auto-pilot → ~50s, 7-scene explainer, Kofi avatar,
ElevenLabs-backed voice, landscape, captions on.

| Step | Detail | Time |
|------|--------|------|
| Setup | Script pasted, avatar Kofi (Personal Development Coach look), voice Adam - Natural | ~3 min |
| Render | Started 7:55pm, finished 8:02pm | ~7 min |
| Capture | Screen-recorded playback (Win+Alt+R) because free tier blocks download | ~2 min |

**Observed, avatar scenes (1,4,7):** lip-sync matched, blinking natural, face edges
clean, speech continuous. Convincing.
**Observed, graphics scenes (2,3,5,6):** fabricated. Efficiency Metrics 88.4 / 60.2 with
invented "-34%" regression caption; Goal Projection curve to 0.67 GPG on a "GAME 1-40"
axis (real season = 19 games); HIGH/LOW panels with no values; scoreboard matching no
real games. None of these numbers exist in my Task 5 data.
**Refusals/filters:** none; auto-pilot freely generated fake analytics without flagging.
**Final artifact:** `artifact_02_video_SYNTHETIC.mp4`

## Detection / Provenance
**Provenance:** HeyGen watermark tiled across all scenes; baked into frames; survived
screen-record capture (metadata would not).
**Detection:** [TO DO, Deepware Scanner or Hive result + confidence + whether explained.]

## Time Summary
| Pipeline | Effort | Convincing enough to fool a casual scroller? |
|----------|--------|-----------------------------------------------|
| Voice | ~5 min (1 generation) | Partly, passes if half-listening, fails on close listen |
| Video (avatar) | ~7 min render | Yes, presenter is convincing |
| Video (charts) | included | Looks authoritative but is fabricated, the real risk |
