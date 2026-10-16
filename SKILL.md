---
name: s2v-video-renderer
description: >-
  A master 3-phase video production engine for historical documentary videos. Handles visual pre-planning,
  asset feasibility & vault reusability audits, 3-tier budget modeling, clean dual copy-blocks, intelligent
  clip re-sequencing, quality-first video compression, dynamic contextual Ken Burns camera motion, and automated
  video rendering via SmartStudio (default) or Concat. Triggered when the user pastes a script ending with `/S2V`
  or requests S2V rendering.
---

# S2V Video Production Skill (3-Phase Master Framework)

## Overview
This skill governs the end-to-end production of historical documentary videos from approved scripts on the local PC. It enforces a strict **3-Phase Workflow** designed for high production quality, AI credit conservation, intelligent asset reuse, and seamless automated rendering.

---

## Core Production Rules & Architecture

### 1. Visual Composition Backbone
- **Image-First Foundation:** Production is primarily high-detail still images animated with dynamic **Ken Burns camera motion** (zoom, pan, tilt, compound drift).
- **Targeted Video Elevation:** 10-second video clips are embedded strictly at high-impact strategic moments (battle climaxes, turning points, atmospheric hooks). Video is never used aimlessly.

### 2. Dual Shared Asset Vaults
To eliminate redundant generation and conserve AI credits, the system maintains two permanent asset repositories:
- `assets/video_vault/`: Reusable 10-second video clips (e.g., flickering oil lamp, desert wind over dunes, galloping cavalry, banners snapping in wind, tactical maps).
- `assets/image_vault/`: Reusable high-resolution still images (e.g., ancient oasis settlements, parchment scrolls, acacia trees, council tents, camel caravans).

### 3. Intelligent Re-sequencing & Re-cutting
- When reusing existing video clips across different episodes, **never repeat an identical 30-second sequence**.
- Re-cut clips, alter their sequencing, pair them with fresh assets, and adjust pacing so returning viewers always experience a fresh, engaging narrative.

### 4. Aspect Ratio Policy (16:9 Landscape Default)
- **Default Production:** Strictly **16:9 Widescreen (1080p Landscape)**.
- **Vertical Video Rule:** 9:16 Portrait / Shorts are **never** produced by cropping landscape assets. Dedicated vertical videos are generated **only separately upon explicit user demand** with dedicated portrait-composed prompts.

### 5. Production Engine Priority (SmartStudio Default)
- **Primary Workhorse — SmartStudio MCP (Default):** The automated, headless, script-to-video assembler for this studio. Synthesizes local Kokoro TTS audio, aligns visuals, applies Ken Burns motion, and exports finished MP4s without manual intervention.
- **Studio Editor — Concat MCP (127.0.0.1:7420):** Interactive NLE timeline editor used **only when the user explicitly commands "Use Concat"** for hands-on timeline trimming, audio detachment, and multi-track editing.
- *Background Sync:* Even when SmartStudio is running, timeline metadata (`concat.json` and `recents.json`) should be maintained in the background as a creator convenience.

### 6. Decoupled 2-Track Master Audio Timeline Architecture
- **Continuous Master Audio Track (The Foundational Ruler):**
  - Once the narration script is locked and approved, all narration segments are synthesized via local TTS and **immediately stitched together** into a single, contiguous master audio file (`master_narration.wav` / `master_narration.mp3`).
  - This single continuous audio track acts as the immutable timeline ruler. It eliminates segment boundary gaps, micro-silences, and desync.
- **Video-Only Visual Layering (Track T1 / Intermediate Clips):**
  - All visual assets (images with Ken Burns motion, 10s video clips) are rendered strictly **video-only** (`-an` in FFmpeg, or audio detached/muted in Concat).
  - **Zero Audio Conflict:** B-roll video clips NEVER carry their native audio into the mix. This completely prevents FFmpeg or NLE software from overriding or muting the spoken narration track.
- **Concat NLE Timeline Project Standards:**
  - **Track T1 (Video Lane):** Houses all visual media clips sequenced back-to-back at their exact segment start timestamps.
  - **Track T2 (Audio Lane):** Houses the single continuous `master_narration.wav` track running from 00:00 to the end.
  - **Standard Concat Operations & Controls:**
    - `Split Clip` (`S` / Razor button): Slice visual clips without affecting the continuous audio track.
    - `Detach Audio`: Separate any native video sound into a separate track and delete it.
    - `Magnetic Timeline` (`M` toggle): Ensure visual clips stay snapped without unwanted black frames.
    - `Transform & Fit` (Inspector): Set aspect ratio to 16:9 (1920x1080) and scale to canvas.
    - `Export Video` (`Ctrl+E`): Render timeline to 1080p MP4.
- **Master Multiplexing (Final Render):**
  - The stitched continuous video-only stream and the continuous master audio stream are combined in a single copy-mux pass:
    `ffmpeg -i full_video_track.mp4 -i master_narration.wav -c:v copy -c:a aac -b:a 192k -shortest final_master.mp4`
  - Guarantees 100% voiceover presence across every second of the video with zero glitches.

### 7. Multi-Shot Montage & 8–10s Cut Pacing Rule (Anti-Loop Guarantee)
- **Zero Excessive Loops:** Visual video clips must NEVER repeat / loop consecutively more than once. Repetitive looping causes immediate viewer drop-off.
- **Multi-Shot Sequence Assembly:** For any sequence exceeding 8–10 seconds (such as the cold open, tactical breakdowns, or dramatic pauses), assemble a multi-shot montage of complementary b-roll clips (e.g., terrain panning shot $\to$ cavalry close-up $\to$ banner snapping $\to$ map line movement) rather than looping a single 4–5s clip 4+ times.

### 8. Quality-First Pre-Upload Video Compression Pipeline
- **Mandatory Pre-Upload Stage:** Every rendered video must pass through an automated compression optimization pass before uploading to YouTube.
- **Visual Quality is Paramount (No Arbitrary 300 MB Cap):**
  - Compression must never degrade crisp 1080p visual sharpness or audio fidelity.
  - There is **no arbitrary file size cap** (an epic 25–30 minute documentary will properly exceed 300 MB or 400 MB).
- **Purpose of Compression:**
  - Raw unconstrained NLE/FFmpeg renders produce 15–30 Mbps export bitrates that cause network drops and exceed MCP 3-minute request timeouts.
  - Normalizes the video to a broadcast-grade H.264 profile (`-preset ultrafast -b:v 2500k -maxrate 3000k -bufsize 6000k`) with bit-exact audio pass-through (`-c:a copy`).
  - Standard command:
    `ffmpeg -i final_master.mp4 -c:v libx264 -preset ultrafast -b:v 2500k -maxrate 3000k -bufsize 6000k -c:a copy compressed_upload.mp4`

### 9. Dynamic Contextual Camera Paths & Ken Burns Protocol
- **Zero Mechanical Zooms:** Eliminate uniform, linear center-zooming across consecutive still scenes. Camera movement must reflect the narrative emotion and spatial geometry of each shot.
- **Master Camera Movement Presets:**
  - `zoom_in` / `dramatic_push_in`: For pivotal emotional climaxes, warrior silhouettes, high tension, and dramatic revelations. Uses smooth ease-in acceleration.
  - `zoom_out` / `slow_reveal`: For vast horizons, battle aftermaths, contemplative epilogues, and wide scenic context. Uses gentle ease-out deceleration.
  - `pan_left` / `pan_right`: For horizontal movement (cavalry advance, desert caravans, traveling along ancient trade routes, military lines).
  - `tilt_up` (Bottom-to-Top): For architectural majesty (minarets, fortress walls, palace gates, mountain ridges, standing commanders looking to the sky).
  - `tilt_down` (Top-to-Bottom): For plunging canyon descents, cavalry descending ridges, birds-eye downward focus.
  - `diagonal_drift`: For ambient exposition (marketplaces, encampments, council tents). Combines a subtle 1.05x zoom with slight lateral drift.
- **Storyboard Camera Contract:** Every still image in the Phase 1 & 2 storyboard is tagged with its explicit camera directive (`motion: "tilt_up"`, `motion: "pan_right"`, etc.). SmartStudio / FFmpeg translates these into exact mathematical `zoompan` coordinates with smooth easing curves.

---

## The 3-Phase Production Workflow

```
[APPROVED NARRATION SCRIPT]
       │
       ▼
PHASE 1: Visual Pre-Plan, Vault Audit & 3-Tier Budget Model
       │  • Runtime calculation (~145 wpm with Kokoro TTS)
       │  • Vault Audit (Scans video_vault & image_vault on disk)
       │  • Strategic Video Zone mapping (Hook, Climax, Epilogue)
       │  • Scene Camera Mapping (Directional pans, tilts, dramatic push-ins)
       │  • Reusability Analysis (Evergreen vs. Episode-Specific)
       │  • 3 Budget Options (High, Medium, Low Tiers)
       │  • Antigravity Strategic Recommendation
       │
       ▼ [User Selects Budget Tier: High, Medium, or Low]
PHASE 2: Targeted Clean Copy-Blocks & Folder Specification
       │  • Block A: Pure Text-to-Video Prompts (Only for newly needed clips)
       │  • Block B: Pure Text-to-Image Prompts (Clean, zero text pollution)
       │  • Storyboard Motion Contract (Explicit camera motion directives per scene)
       │  • Universal Islamic historical aesthetic (dynamic era, no hardcoded century)
       │  • Anonymous dignified depiction (no character face generation)
       │  • Explicit deposit folder path provided
       │
       ▼ [User drops generated files into working folder & replies: "Complete, continue"]
PHASE 3: Assembly, Alignment, Compression & Render
       • Verifies all expected media files in project directory
       • Synthesizes and pre-stitches continuous master audio track
       • Re-cuts and aligns video clips to designated zones using multi-shot montages
       • Applies contextual Ken Burns motion (directional pans, architectural tilts, smooth zooms)
       • Executes render via SmartStudio (default) or Concat (on demand)
       • Executes Quality-First Pre-Upload Compression pass
       • Automatically catalogs newly generated clips into shared vaults
```

---

### PHASE 1: Visual Pre-Plan & Feasibility Analysis

Immediately upon script approval, generate the Phase 1 blueprint:

1. **Episode Dimensions:**
   - Word count and estimated runtime based on natural Kokoro speech pacing (~145 words/minute).

2. **Vault Feasibility Audit:**
   - Scan `assets/video_vault/` and `assets/image_vault/` on the PC.
   - List existing clips that fit this episode's scenes without generating new media.

3. **Strategic Video Zone & Camera Motion Proposal:**
   - Identify 2 to 4 strategic locations where video motion creates maximum retention:
     * *Opening Hook* (0:00–0:45)
     * *Mid-Episode Battle/Strategic Climax*
     * *Episode Epilogue / Turning Point*
   - Calculate 10-second Google Flow clip math (30s zone = 3 clips, 60s zone = 6 clips).
   - Outline the primary camera choreography for still scenes (e.g. Map scenes = `pan_right`, Fortresses = `tilt_up`, Climaxes = `dramatic_push_in`).

4. **Reusability & Strategic Value Analysis:**
   - For every proposed video clip, specify:
     * **Evergreen/Reusable:** Can be repurposed across multiple future episodes.
     * **Episode-Specific:** Unique to this story.
     * **Strategic Justification:** Why video is required instead of Ken Burns still imagery.

5. **The 3 Budget Options:**
   - **Option 1 — High Video Tier (Epic Mode):** 15–20 clips (~2.5–3.0 mins of video). For season premieres and major historical battles.
   - **Option 2 — Medium Video Tier (Balanced Mode — Recommended):** 6–8 clips (~1.0–1.5 mins of video). Uses 3–4 vault clips + 3–4 new reusable clips. Balance of speed and credits.
   - **Option 3 — Low Video Tier (Economy / Rapid Mode):** 2–4 clips (uses **100% existing vault clips**, 0 new video generations). For fast 24h turnaround or credit exhaustion.

6. **Strategic Recommendation:**
   - State Antigravity's prioritized recommendation for this episode.

> **GATE:** Await user selection of Tier 1, 2, or 3 before proceeding to Phase 2.

---

### PHASE 2: Targeted Clean Copy-Blocks

Once the user approves a budget tier, deliver Phase 2:

1. **Prompt Cleanliness & Zero Pollution Rule:**
   - **Zero Text Pollution:** No numbers, no IDs (`V01`, `IMG01`), no labels (`Prompt:`, `Scene 1:`), no target filenames, and no metadata inside the prompt text.
   - **No Aspect Ratio in Prompts:** NEVER include aspect ratio flags or text (e.g., `16:9`, `9:16`, `--ar 16:9`) in any prompt. Aspect ratio is configured directly in the generation tool UI or video rendering engine. Adding aspect ratio in the prompt text is strictly forbidden.
   - **Single-Click Copy Block Formatting:** Prompts must ALWAYS be presented inside unified code blocks (one code block for Text-to-Video prompts, and one code block for Text-to-Image prompts). Each prompt is separated by a blank line so the user can copy all prompts with one click of the copy button.

2. **BLOCK A — Text-to-Video Prompts:**
   - Output in a single unified code block. One prompt per paragraph separated by a blank line.
   - No aspect ratios, no numbers, no IDs, no labels.
   - Designed for 10-second duration with fluid cinematic motion (pans, tracking, cavalry runs).

3. **BLOCK B — Text-to-Image Prompts:**
   - Output in a single unified code block. One prompt per paragraph separated by a blank line.
   - No aspect ratios, no numbers, no IDs, no labels.
   - Rich physical textures: lighting, authentic materials, stone, parchment, cloth, weather.

4. **Storyboard Motion Metadata:**
   - Accompanying the copy blocks, maintain the segment motion mapping dictionary linking each segment index to its camera directive (`motion: "tilt_up"`, `motion: "pan_right"`, etc.) for Phase 3 render handoff.

5. **Master Historical Aesthetic Directive:**
   - Covers sacred and classical Islamic history across all eras (from the creation of Adam, ancient Prophets, classical Arabia, Caliphates, Golden Age, through classical civilizations).
   - Never artificially limit or hardcode prompts to the 7th century unless the script specifies it.
   - Enforce anonymous dignified depictions: silhouettes, armor details, cavalry, banners, weapons, maps, authentic architecture. **Zero individual character face generations.**

6. **Working Folder Deposit Path:**
   - Explicitly instruct the user where to deposit downloaded files:
     `C:\Users\HomePC\Documents\GitHub\SmartStudio\projects\[Episode_Folder]\`

> **GATE:** User generates media, deposits files into folder, and replies: **"Complete, continue"**.

---

### PHASE 3: Assembly, Alignment, Compression & Render

Triggered when the user says **"Complete, continue"**:

1. **Vault & Folder Verification:**
   - Verify that all newly downloaded assets and linked vault clips exist in the working directory.

2. **Continuous Audio Foundation:**
   - Synthesize and pre-stitch the continuous master audio track (`master_narration.wav`) first.

3. **SmartStudio Execution (Default):**
   - Call the `smartstudio` MCP tool `create_storyboard` with the mapped segment dictionary including camera motion directives.
   - Call `smartstudio` MCP tool `align_and_place_assets` to match video clips and images to their respective segment slots, ensuring multi-shot montages for long sequences.
   - Call `smartstudio` MCP tool `render_video` to execute dynamic Ken Burns motion and render the master 16:9 1080p MP4.

4. **Concat Execution (Only If Explicitly Demanded):**
   - Verify Concat API connection on `127.0.0.1:7420`.
   - Call `concat_project_create` and import all media assets via `concat_media_import`.
   - Sequence video and audio clips on timeline tracks via `concat_add_clip`.
   - Call `concat_export_video` to export master timeline.

5. **Quality-First Compression Pass:**
   - Normalize the exported MP4 to broadcast-grade H.264 profile using FFmpeg:
     `ffmpeg -i final_master.mp4 -c:v libx264 -preset ultrafast -b:v 2500k -maxrate 3000k -bufsize 6000k -c:a copy compressed_upload.mp4`
   - Verify compressed file integrity and visual sharpness.

6. **Vault Auto-Cataloging:**
   - Automatically copy all newly generated evergreen clips into `assets/video_vault/` and `assets/image_vault/` so future episodes immediately benefit from them.

---

## Error Handling & Recovery
- **Missing Vault Clip:** Fall back to a Ken Burns still image from `image_vault/`.
- **Render Timeout / Missing Asset:** SmartStudio orchestrator logs errors in `logs/` and supports resume-capable execution without re-rendering completed segments.
- **Concat Offline:** If Concat API on port 7420 is unreachable when requested, alert user with exact port troubleshooting steps before falling back to SmartStudio.
- **Excessive Video Looping:** Detect if a single video asset is scheduled for $>10$s of runtime and split with an alternating still or secondary b-roll asset.
