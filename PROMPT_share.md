Make me a lyric video for a song, end to end. Do every step yourself. Only stop if a file is missing.

INPUTS
- SONG: [path to your song file, e.g. ~/Music/song.m4a]
- PART: [start, e.g. 2:39] to [end, e.g. 3:52]
- MOOD: [one or two lines on the feeling and look, e.g. "dark and sad, a rainy city at 3 a.m., blue-black, heavy grain"]
- FORMAT: [1080x1920 vertical for posting, or 1920x1080 landscape to play on a laptop and film]
- FOOTAGE: [a folder of free night clips from pexels.com, or "none" to draw every scene in code]

1. SETUP. Make a Python venv with demucs and a Whisper (mlx-whisper on Apple Silicon, faster-whisper otherwise) plus librosa. Make a Remotion project. Check ffmpeg.

2. AUDIO. If PART is "auto" and LYRICS is a file: do one fast pass on the full mix (small Whisper model, 16 kHz, no vocal split) just to find where the LYRICS lines are sung (fuzzy match, in order), and set PART from 1 s before the first line to 2 s after the last. Print the PART you found. If some lines are not in the song, stop and list them by row number. Then cut PART from SONG with ffmpeg. Split out the vocals with demucs (htdemucs, two stems).

3. TIMING. If LYRICS is a file, skip open transcription: force-align the known words to the vocal stem in one pass (CTC forced alignment, e.g. torchaudio MMS_FA). Otherwise transcribe the vocal stem with word timestamps. Snap each word to the nearest vocal onset (librosa) so it lands within 40 ms of the sung word. Group words into the sung lines. Save lines.json (text, start, end per word) and print it. If a word looks misheard, fix it from context and flag it. If LYRICS points to a file, use its words as the source of truth and take only the timings from the transcription.
   LYRICS: [optional: a .txt file with the lyrics, one sung line per row, or "none" to transcribe them]

4. MEANING. Read each line for what it feels like, not just what it says. Write one sentence of its subtext, then design its scene as a visual metaphor for that feeling (the line should be understood with the text hidden). Pick the HERO word that carries the feeling and one motion (slow push, drift, flicker, hold). Plan the whole PART as an arc that follows the song (quiet, building, breaking, aftermath) and drive motion intensity, light and grain from the music's loudness (RMS of the full mix), so the climax visibly breaks open and the end goes still. Save it all in meaning.json.

5. LOOK. Build the palette, grain and scenes from MOOD. Two typefaces only: small tracked caps for the line, one huge italic serif for the HERO word. If FOOTAGE is a folder of video clips, build the scenes from them: look at a few frames of every clip, give each line the clip that best matches its feeling, cut to 1920x1080 proxies of just the needed seconds with ffmpeg first (never load the full 4K files in Remotion), add a slow push or drift, and grade them all to the MOOD (crushed blacks, low saturation, cool shadows, grain) so they feel like one film. Where a clip has a steady surface (fogged glass, a window, a sign, a phone held still), put the line on that surface with matching perspective and softness; otherwise use the caps line and hero word over the footage. Crop or blur away any visible brand names. If FOOTAGE is "none", draw every scene in code (shaders, canvas, SVG, particles). No API keys either way.
   Structure:
   - Opening: a music-app style lyrics card (song title, artist, current line bright, next line dim) floating over the line's words repeated huge in the background.
   - Then: one full-screen scene per line, caps line plus the serif HERO word, crossfading on the beat.
   - Every word appears on its own timestamp with a soft spring. Nothing appears before it is sung.

6. RENDER. [1080x1920 vertical for posting, or 1920x1080 landscape to play on a laptop and film], 30 fps, H.264 CRF 20, yuv420p (aim under 60 MB per file), Remotion concurrency = CPU cores minus 2. Export final_silent.mp4 (no audio: add the song in Instagram from its music library, starting at PART) and preview_with_audio.mp4 to check sync.

7. CHECK. Render stills only (one every 2 s) for the contact sheet, never the full video, while fixing; re-render the full video once at the end. Print how long each step took.
   Contact sheet: look at it, fix overlaps, cut-off text and weak contrast, and confirm each word shows within 0.1 s of its time. Re-render. At most 3 rounds.

8. SHOWCASE (only if FORMAT is vertical). Also export showcase_silent.mp4: the same video playing on a laptop screen in a dark room lit by the screen's colour, a tile on top reading "<song title> × Claude" in plain type (no album art, no band logos) and a small "Single prompt" caption at the bottom.

When done, print the paths and a one-line summary of the look you chose.
