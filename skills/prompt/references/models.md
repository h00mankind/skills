# Prompt Routing and Model Reference

Read this file for provider syntax, model choice, and API parameters for image and video prompts. It has no audio providers. For audio, use the **Audio** section of SKILL.md.

## Modality Routing Architecture

Every prompt request routes through two classification steps:

```
User Request
  │
  ├─► image ─┬─► create (new image) ─► Route Model (Default: ChatGPT 2.5 / GPT Image 2.5)
  │          │
  │          └─► edit (existing img) ─► Route Model & Edit Strategy (Delta prompting, role tags)
  │
  ├─► video ─┬─► text-to-video ──────► Route Model (Front-loaded camera motion + physics)
  │          │
  │          └─► image-to-video ─────► Route Model (Keyframe role tags + differential motion)
  │
  └─► audio ─┬─► speech (TTS) ───────► Voice intent, speed, emotion, pronunciation cues
             │
             └─► music / SFX ────────► Mood, genre, instrumentation, tempo, duration
```

## Image Provider Routing

### 1. OpenAI ChatGPT 2.5 / GPT Image 2.5 (Default)

Use it for new images and image edits, unless the user asks for another provider.

**Model Selection:**
- `gpt-image-2.5-flare`: Default for speed, prototyping, high-volume, and standard tasks.
- `gpt-image-2.5-sunburst`: High-fidelity model for demanding precision, complex typography, and intricate multi-step edits.

**API Parameters:**
- `model`: `"gpt-image-2.5-flare"` or `"gpt-image-2.5-sunburst"`
- `quality`: `"auto"` (default), `"low"`, `"medium"`, `"high"`, `"xhigh"`, `"max"`
- `size`: Standard formats (`1024x1024`, `1536x1024`, `1024x1536`, `2048x2048`, `2048x1152`, `3840x2160`, `2160x3840`) or custom `WIDTHxHEIGHT` (multiples of 16, edge <= 3840, aspect ratio <= 3:1)
- `background`: `"auto"`, `"opaque"`, `"transparent"`
- `output_format`: `"png"`, `"webp"`, `"jpeg"`

**Prompt Structure for New Creations:**
- **Natural descriptive sentences:** Avoid tag-soup. Describe visible details, materials, lighting, and framing.
- **Text rendering:** Put required text in double quotes (e.g. `tagline "Yours to Create"`). Specify typography and placement.
- **Logos / Cutouts:** Set `background="transparent"`, `output_format="png"`. Specify clean alpha edges and zero solid backdrop.

**Prompt Structure for Edits:**
- **Delta prompting:** State what changes and what stays identical: `Change only X. Preserve Y and Z exactly.`
- **Single-change iterations:** Make one adjustment per edit turn to prevent drift.
- **Reference roles:** Designate inputs by number (`Image 1` for identity/scene, `Image 2` for clothing/accessory).
- **Subject preservation:** Restate likeness, facial geometry, pose, and lighting constraints.

### 2. Midjourney (v8.2 Only)

Use it only when the user names Midjourney. Use only the v8.2 release.

**Prompt Syntax:**
- Conceptual keyword clusters and descriptive phrases separated by commas.
- Avoid obsolete buzzwords ("photorealistic", "8k"). Use lens types (`35mm lens, f/1.8`) and physical lighting.

**Parameters:**
- `--v 8.2`: Current version engine flag (always specify `--v 8.2`)
- `--ar <width:height>`: Aspect ratio (`--ar 16:9`, `--ar 9:16`, `--ar 1:1`)
- `--s <0-1000>`: Stylize value (default 100; use 50 for prompt fidelity, 750 for artistic interpretation)
- `--style raw`: Reduces Midjourney aesthetic bias for realistic documentary style
- `--sref <url>`: Style reference with optional weight `--sw <0-1000>`
- `--cref <url>`: Character reference with weight `--cw <0-100>` (`--cw 0` face only, `--cw 100` face + clothes)
- `::`: Multi-prompt weighting (e.g. `concept one::2 concept two::1`)
- `--no <elements>`: Negative prompt parameter

**Edits:**
- Vary Region (inpainting) with Remix mode: select the area and rewrite the prompt for only the modified item.

### 3. Google Nano Banana (Nano Banana Pro / Nano Banana 2)

Use for Google visual generation and editing tasks.

**Model Selection:**
- `Nano Banana Pro`: High-fidelity model for complex visual synthesis, scientific diagrams, and detailed textures.
- `Nano Banana 2`: High-throughput model for fast iterative visual generation and multimodal workflows.

**Prompt Syntax:**
- 5-part structure: Core subject, artistic medium, spatial context/environment, directional lighting, camera perspective.
- Put exact text in double quotation marks.
- Write clear connected prose. Do not write comma-separated tag clusters.
- Say what must be visible. Do not list what to exclude.

**Parameters:**
- `aspectRatio`: `"1:1"`, `"3:4"`, `"4:3"`, `"9:16"`, `"16:9"`
- `personGeneration`: `"ALLOW_ADULT"`, `"DONT_ALLOW"`, `"ALLOW_ALL"`
- `safety_filter_level`: `"BLOCK_ONLY_HIGH"`, `"BLOCK_SOME"`

**Edits:**
- Native multimodal instruction: provide reference images with explicit guidance on modifications.
- Declare the exact region or subject to replace while maintaining background lighting and perspective.

### 4. xAI Grok Imagine (Aurora)

Use when generating in Grok or targeting xAI image endpoints.

**Prompt Syntax:**
- Director's brief format: `[Subject] + [Setting/Context] + [Style/Medium] + [Lighting/Mood] + [Compositional Details]`.
- Front-load the subject within the first 30-80 words.
- You can write exclusions in plain language in the prose.

**Edits:**
- Make one change per turn. State the exact change, and tell the model to keep the surrounding context.

---

## Video Provider Routing

### 1. Front-Loaded Attention Rule
All leading video diffusion models (Runway, Luma, Sora, Kling, Seedance) prioritize the opening 20 words. Always place camera movement and core subject motion at the start of the prompt.

### 2. Provider Directives

**OpenAI Sora:**
- Structure: `[Subject & Physical Action] + [Environment & Depth] + [Cinematography: Lens, Framing, Camera Motion] + [Lighting & Atmosphere]`.
- Ground movements in real physics (inertia, gravity, fluid dynamics).
- For static shots: `locked-off stationary tripod shot, static camera, zero movement`.

**Runway Gen-3 (Alpha & Turbo):**
- Structure: `[Camera Movement]: [Subject & Physical Action]. [Environment]. [Lighting & Lens Aesthetic].`
- Camera prefixes: `dolly in`, `tracking shot`, `crane up`, `orbit arc`, `static locked shot`.
- Multi-axis motion brush: X (horizontal), Y (vertical), Z (proximity).

**Luma Dream Machine (Ray-2):**
- Structure: `[Camera Motion], [Framing & Shot Scale], [Subject Identity & Action], [Environment & Lighting], [Atmospheric Finish].`
- Front-load camera directives. Use start and end keyframe images for interpolation.

**Kling AI:**
- Structure: `[Subject & Traits] + [Physical Action] + [Location & Lighting] + [Camera Path & Framing] + [Style].`
- Use 6-axis camera parameters when available. Use Static Brush to freeze backgrounds and prevent warping.

**ByteDance Seedance 2.5:**
- Structure: `[Multimodal Role Tags (@Image1 / @Video1)] + [Timestamped Actions (0-5s, 5-10s)] + [Spatial Staging] + [Camera Motion Directives] + [Audio Constraints].`
- Assign explicit roles to all input assets.
