---
name: s2v-video-renderer
description: >-
  A master 3-phase video production engine for historical documentary videos. Handles visual pre-planning,
  asset feasibility & vault reusability audits, 3-tier budget modeling, clean dual copy-blocks, intelligent
  clip re-sequencing, and automated video rendering via SmartStudio (default) or Concat. Triggered when the user
  pastes a script ending with `/S2V` or requests S2V rendering.
---

# S2V Video Production Skill (3-Phase Master Framework)

## Overview
This skill governs the end-to-end production of historical documentary videos from approved scripts on the local PC. It enforces a strict **3-Phase Workflow** designed for high production quality, AI credit conservation, intelligent asset reuse, and seamless automated rendering.

---

## Core Production Rules & Architecture

### 1. Visual Composition Backbone
- **Image-First Foundation:** Production is primarily high-detail still images animated with dynamic **Ken Burns camera motion** (zoom, pan, tilt).
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

### 5. Dual Render Engine Routing
- **Default Engine — SmartStudio MCP:** Automated, headless, script-to-video assembler. Synthesizes local Kokoro TTS audio, aligns visuals, applies Ken Burns motion, and exports finished MP4s without manual intervention.
- **Studio Engine — Concat MCP (127.0.0.1:7420):** Live NLE timeline editor used **only when the user explicitly commands "Use Concat"** for hands-on timeline trimming, audio detachment, and multi-track editing.

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
       │  • Reusability Analysis (Evergreen vs. Episode-Specific)
       │  • 3 Budget Options (High, Medium, Low Tiers)
       │  • Antigravity Strategic Recommendation
       │
       ▼ [User Selects Budget Tier: High, Medium, or Low]
PHASE 2: Targeted Clean Copy-Blocks & Folder Specification
       │  • Block A: Pure Text-to-Video Prompts (Only for newly needed clips)
       │  • Block B: Pure Text-to-Image Prompts (Clean, zero text pollution)
       │  • Universal Islamic historical aesthetic (dynamic era, no hardcoded century)
       │  • Anonymous dignified depiction (no character face generation)
       │  • Explicit deposit folder path provided
       │
       ▼ [User drops generated files into working folder & replies: "Complete, continue"]
PHASE 3: Assembly, Alignment & Final Render
       • Verifies all expected media files in project directory
       • Re-cuts and aligns video clips to designated video zones
       • Maps images to narrative scenes with Ken Burns motion
       • Synthesizes Kokoro TTS voiceover and mixes background music
       • Executes render via SmartStudio (default) or Concat (on demand)
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

3. **Strategic Video Zone Proposal:**
   - Identify 2 to 4 strategic locations where video motion creates maximum retention:
     * *Opening Hook* (0:00–0:45)
     * *Mid-Episode Battle/Strategic Climax*
     * *Episode Epilogue / Turning Point*
   - Calculate 10-second Google Flow clip math (30s zone = 3 clips, 60s zone = 6 clips).

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

1. **Prompt Cleanliness Rule:**
   - Prompts must contain **zero text pollution**.
   - No numbers, no IDs (`V01`, `IMG01`), no labels, and no target filenames in the prompt text.
   - One prompt per paragraph so the user can batch-paste directly into Google Flow without AI confusion.

2. **BLOCK A — Text-to-Video Prompts:**
   - Provided **only** for new clips required under the chosen tier (none if Option 3).
   - Designed for 10-second duration with fluid cinematic motion (pans, tracking, cavalry runs).

3. **BLOCK B — Text-to-Image Prompts:**
   - Provided for all newly required still scenes (excluding vault-reused images).
   - Rich physical textures: lighting, authentic materials, stone, parchment, cloth, weather.

4. **Master Historical Aesthetic Directive:**
   - Covers sacred and classical Islamic history across all eras (from the creation of Adam, ancient Prophets, classical Arabia, Caliphates, Golden Age, through classical civilizations).
   - Never artificially limit or hardcode prompts to the 7th century unless the script specifies it.
   - Enforce anonymous dignified depictions: silhouettes, armor details, cavalry, banners, weapons, maps, authentic architecture. **Zero individual character face generations.**

5. **Working Folder Deposit Path:**
   - Explicitly instruct the user where to deposit downloaded files:
     `C:\Users\HomePC\Documents\GitHub\SmartStudio\projects\[Episode_Folder]\`

> **GATE:** User generates media, deposits files into folder, and replies: **"Complete, continue"**.

---

### PHASE 3: Assembly, Alignment & Final Render

Triggered when the user says **"Complete, continue"**:

1. **Vault & Folder Verification:**
   - Verify that all newly downloaded assets and linked vault clips exist in the working directory.

2. **SmartStudio Execution (Default):**
   - Call the `smartstudio` MCP tool `create_storyboard` with the mapped segment dictionary.
   - Call `smartstudio` MCP tool `align_and_place_assets` to match video clips and images to their respective segment slots.
   - Call `smartstudio` MCP tool `render_video` to synthesize Kokoro TTS audio and render the master 16:9 1080p MP4.

3. **Concat Execution (Only If Explicitly Demanded):**
   - Verify Concat API connection on `127.0.0.1:7420`.
   - Call `concat_project_create` and import all media assets via `concat_media_import`.
   - Sequence video and audio clips on timeline tracks via `concat_add_clip`.
   - Call `concat_export_video` to export master timeline.

4. **Vault Auto-Cataloging:**
   - Automatically copy all newly generated evergreen clips into `assets/video_vault/` and `assets/image_vault/` so future episodes immediately benefit from them.

---

## Error Handling & Recovery
- **Missing Vault Clip:** Fall back to a Ken Burns still image from `image_vault/`.
- **Render Timeout / Missing Asset:** SmartStudio orchestrator logs errors in `logs/` and supports resume-capable execution without re-rendering completed segments.
- **Concat Offline:** If Concat API on port 7420 is unreachable when requested, alert user with exact port troubleshooting steps before falling back to SmartStudio.
