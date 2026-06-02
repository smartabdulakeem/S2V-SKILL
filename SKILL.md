---
name: s2v-video-renderer
description: >-
  Generates and renders edited videos on the user's machine from plain text narration scripts. 
  Triggered when the user pastes a script ending with `/S2V` or requests S2V rendering.
---

# S2V Video Renderer

## Overview
This skill allows the agent to convert raw text scripts into fully rendered videos directly on the user's computer. It handles storyboard planning, chunking, and executes the local S2V rendering command (`cli.py`).

## Dependencies
None.

## Workflow

When the user requests video rendering (e.g. by pasting a script ending in `/S2V` or explicitly asking to render a script):

### 1. Request Preview Details
Stop and ask the user to clarify:
- **Aspect Ratio:** (e.g., `16:9` Widescreen, `9:16` Vertical, `1:1` Square, `4:3` Fullscreen)
- **Visual Style:** (e.g., *Cinematic Photograph*, *Historical Oil Painting*, *Ink Sketch & Charcoal*, or any custom style description)
- **Voiceover:** (e.g., `local:piper` for offline Piper, or `edge:en-US-GuyNeural` / other Edge voices)

### 2. Generate the Storyboard JSON
Once the details are provided, split the script into logical visual scenes (15-45 words each). For each scene, define:
- `segment_id`: 1, 2, 3...
- `type`: "hook" (first scene), "conclusion" (last scene), or "body" (others)
- `narration`: Verbatim text segment
- `b_roll_keyword`: A descriptive 3-5 word visual prompt for image generation
- `ken_burns`: A motion effect ("zoom_in", "zoom_out", "pan_left", "pan_right", "none")
- `text_overlay`: Optional subtitle text overlay on screen (or null)

Construct the S2V schema dictionary:
```json
{
  "project": {
    "title": "[Video Title]",
    "output_filename": "[safe_title_name].mp4",
    "voice": "[Selected Voice, e.g. local:piper]",
    "voice_rate": "+0%",
    "voice_pitch": "+0Hz",
    "background_music": null,
    "visual_style": "[Visual Style]",
    "aspect_ratio": "[Aspect Ratio]"
  },
  "segments": [ ... ]
}
```

### 3. Save Script to Disk
Save the constructed JSON script into the S2V cache folder at:
`C:\Users\HomePC\Documents\GitHub\S2V\cache\planned_script.json`

### 4. Invoke CLI Render
Run the local rendering command using the python executable:
```powershell
C:\Users\HomePC\AppData\Local\Programs\Python\Python312\python.exe C:\Users\HomePC\Documents\GitHub\S2V\cli.py C:\Users\HomePC\Documents\GitHub\S2V\cache\planned_script.json
```
Let the command run in the background. Stream progress updates back to the user, and notify them when the render completes.

## Common Mistakes
- **Vercel Rendering:** Do not attempt to render videos when running on Vercel. Rendering must always be triggered locally.
- **Verbatim Narration:** Ensure every single word of the original script is included in the narration segments without changes.
