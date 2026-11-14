---
name: prompt
description: Write or improve prompts for image, video, and audio generation and editing. Use for an image prompt, an image edit, a video prompt, image-to-video, a voice or TTS prompt, a music or SFX prompt, or a prompt for ChatGPT 2.5, Midjourney, Nano Banana, Grok Imagine, Sora, Runway, Luma, Kling, or Seedance.
---

# Prompt

Find the output medium and the edit intent first. Then write the prompt for the target model. Route image generation and image edits to ChatGPT 2.5, unless the user names another model.

This skill covers media only. A prompt for an agent or a text model goes to the `/ambitious` command.

## Routing

1. **Find the medium.**
   - `image`: go to **New image** or **Image edit**.
   - `video`: go to **Video**.
   - `audio`: go to **Audio**.
2. **Find the target model.** Use the model the user names. Otherwise, use the default for the medium. Only image has a default. For video or audio with no model named, write a model-neutral prompt with target `general`, and tell the user that the prompt names no model. For image and video provider syntax, model choice, and API parameters, read `references/models.md`. Read only the section for the target provider. For audio, the **Audio** section of this file is the reference.

## New image

- Default model: ChatGPT 2.5. Use `gpt-image-2.5-flare` for speed and volume. Use `gpt-image-2.5-sunburst` for precision and high quality.
- Describe the subject, the action, the environment, the lighting, and the framing, in that order.
- Write natural sentences, not a list of tags.
- Put required text in double quotes.
- Set the dimensions, or `background="transparent"`, when the use needs it.

## Image edit

- Name the exact change, and name what stays the same: `Change only X. Preserve Y and Z.`
- Make one edit per turn.
- Give each input image a role: `Image 1` for identity, `Image 2` for clothing.

## Other image models

- **Midjourney:** use `--v 8.2` only. Write comma-separated descriptors. Use `--ar`, `--s`, `--cref`, and `--sref` as needed.
- **Google:** use Nano Banana Pro or Nano Banana 2. Write a five-part layered description. Set `aspectRatio`. Quote exact text.
- **Grok Imagine:** write a director's brief: `[Subject] + [Setting] + [Style] + [Lighting]`.

## Video

- Put the camera movement in the first 20 words: dolly, tracking, static tripod.
- Put the physical action before the style.
- Give each reference a role: `@Image1` as the start frame, `@Image2` as the end frame.

## Audio

- State the intent and the mood first.
- For speech (TTS), give the voice, the speed, the emotion, and pronunciation cues.
- For music or SFX, give the genre, the instruments, the tempo, and the length.

## Output mode

- **Quick (default):** return the finished prompt in a copyable `text` block. Put the model and parameter notes below it. Do not make files.
- **Structured:** use it only when the user asks to save the prompt. Make `docs/prompts/NNNN-<mode>-<slug>/prompt.md` in the user's project. Find the highest four-digit prefix in `docs/prompts/`. Use the next unused number. Never reuse a number. If the user's original media files must stay, save them in `docs/prompts/NNNN-<mode>-<slug>/assets/` in the project.

Use this format for a structured prompt:

````markdown
---
title: <short title>
mode: <image|video|audio>
target: <chatgpt-2.5|midjourney|nano-banana|grok|sora|runway|luma|kling|seedance|general>
intent: <create|edit|tts|music|sfx>
created: <YYYY-MM-DD>
---

# Prompt

```text
<finished prompt>
```

## Settings and usage

- Model: <target model>
- Parameters: <size, quality, aspect ratio, or provider flags>
- Workflow: <generation steps or edit reference roles>
````

## Guardrails

- Keep the change and the preserved regions apart in each image edit prompt.
- Add Midjourney flags (`--ar`, `--v`) only to Midjourney prompts. Never add them to ChatGPT 2.5 or Nano Banana prompts.
- Start each video prompt with the camera motion and the physical action. Put the style after them.
- For a transparent image asset, ask for a transparent background and PNG or WebP output.
- Do not make HTML artifacts here. Use the `present` skill for those.

## Example

The user asks to edit an image and change the subject's clothing:

```text
Edit the image to dress the woman using the provided clothing images.
Do not change her face, facial features, skin tone, body shape, pose, or identity in any way.
Preserve her exact likeness, expression, hairstyle, and proportions.
Replace only the clothing with the beige jacket and white top from the reference images, fitting the garments naturally to her pose.
Match the original lighting, shadows, and color temperature.
Do not change the background, camera angle, or framing.
```

## Done

- The prompt follows the conventions of the target model.
- An edit prompt keeps the change apart from what stays the same.
- The prompt is in a copyable block, or saved at the structured path.

## Sources

- **h00man:prompt** by h00man23, https://github.com/h00mankind/skills, MIT. Takes the routing, the model defaults, the output modes, the guardrails, the example, and `references/models.md`, narrowed to image, video, and audio.
