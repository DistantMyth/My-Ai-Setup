# Cool Workflows

A curated collection of practical, repeatable AI workflows for development, creative engineering, and interactive web experiences.

---

## 1. Scroll-Linked Canvas Animation (Apple-Style Product Scrub)

Interactive product sequences (such as products rotating, exploding, or assembling as the user scrolls) are commonly built as a sequential folder of image frames rendered to a sticky HTML5 `<canvas>` and scrubbed via scroll offset.

- **Origin / Reference:** [Instagram Demo Reel](https://www.instagram.com/reel/DdyqkyqvuUM/)
- **Prompt Source:** [Pathwise Academy Prompts](https://pathwiseacademy.co/prompts)

```
[Start Frame & End Frame (Google Flow)]
                │
                ▼
[Frames-to-Video Motion Generation (Locked Camera, Linear Speed)]
                │
                ▼
[Extract Stills @ 30fps to ZIP (Ezgif Video-to-JPG)]
                │
                ▼
[Feed Frames + Canvas Build Prompt into AI Coding Tool (OMP / Claude / Cursor)]
```

### End-to-End Steps

1. **Generate Consistent Start and End Frames**
   - Use [Google Flow](https://labs.google/fx/tools/flow).
   - Generate two frames: front-facing (start) and 180° rotated rear (end).
   - Ensure identical camera distance, studio lighting, background color, and scale so the model only interpolates rotation without geometry distortion.

2. **Generate Motion Video (Frames to Video)**
   - Upload the start frame into the first slot and the end frame into the last slot in an image-to-video model.
   - Prompt for smooth, continuous rotation around a fixed vertical axis.
   - **Crucial:** Keep the camera locked (no panning, zooming, or handheld shake) and speed constant with **no easing**. Easing causes scroll scrubbing to feel sticky or laggy when scrubbed manually.

3. **Split Video into Image Stills**
   - Upload the generated MP4 to [Ezgif Video to JPG](https://ezgif.com/video-to-jpg).
   - Set output rate to 30 fps and download as a numbered ZIP archive (a 5-second clip yields ~150 frames).
   - *Optimization tip:* If loading for mobile, downscale resolution or lower frame rate to 20–24 fps to manage payload size. (PNG export is available if needed, but note it does not remove backgrounds).

4. **Generate the Canvas Scrubber with AI**
   - Unzip the numbered frames into your project asset folder.
   - Paste the Vanilla JS Canvas Build Prompt into OMP, Claude Code, Cursor, or your preferred agent.

---

### Prompts

#### Prompt A: Start & End Frames (Google Flow)

```text
START FRAME

Product photography of [YOUR PRODUCT], centred on a seamless matte
[COLOUR] background. Front-facing, straight-on camera angle, eye level.
Soft studio lighting from the upper left, gentle falloff, subtle contact
shadow beneath. Sharp focus, high detail, no text, no props.


END FRAME — use the same reference product, background and lighting

Product photography of [YOUR PRODUCT], centred on a seamless matte
[COLOUR] background. Rotated 180 degrees to show the rear, same eye-level
camera height and same distance. Soft studio lighting from the upper left,
gentle falloff, subtle contact shadow beneath. Sharp focus, high detail,
no text, no props.

Use the same supplied product reference for both frames. Preserve its visible shape, colour and branding. If the rear is not shown, request a rear reference or label the rear details as conceptual rather than inventing them as fact.
```

#### Prompt B: Motion Clip Generation (Frames-to-Video)

```text
Use an image-to-video tool that accepts start and end frames. Attach the two approved frames and select a supported duration in the tool.

The product rotates smoothly and continuously from front to back on a
fixed vertical axis. Camera is locked off — no pan, no zoom, no handheld
movement. Constant rotation speed, no easing at either end. Lighting and
background stay completely still.
```

#### Prompt C: Canvas Scrubber Implementation (Paste into AI Coding Tool)

```text
Build a single-page scroll-linked image sequence using vanilla JavaScript and no libraries. Frames are in ___, numbered ___. Confirm the filename pattern and count before coding.

- Put a sticky full-screen canvas inside a scroll container about four viewport heights tall.
- Load and decode the first usable frame promptly, then load the rest in bounded batches. Show progress and handle missing frames without a blank canvas or an infinite loading state.
- Map progress through the container to a clamped frame index. Use requestAnimationFrame and redraw only when the frame or size changes.
- Preserve image proportions with cover-style cropping; account for device pixel ratio without allocating an unnecessarily large canvas.
- Redraw on resize. With reduced motion enabled, show a static frame and readable content instead of requiring the animation.
- Add editable text overlays at 30% and 70% progress, with good contrast and a usable reading order.

Report checks at a narrow and desktop width, including missing frames and reduced motion. Clearly distinguish tests run from suggested checks.
```

---

### Context & Technical Caveats

- **Model & Credit Limits:** Check the active model, first-and-last-frame support, and credit costs before generating in Flow (see [Google Flow Documentation](https://support.google.com/flow/answer/16352836)).
- **Progressive Loading:** Preloading all 150 frames before rendering causes long initial white-screens. The prompt specifies progressive batch decoding so frame 1 renders immediately while subsequent frames stream in.
- **Accessibility:** `prefers-reduced-motion` must be respected by defaulting to a key static hero frame and readable overlay text rather than forcing scroll-scrubbing.
