# Comfy-UI-Nodes
AI Vibed Coded Nodes for Comfy UI

<strong><h2>Absynth Film Look Creator v1.0 - A ComfyUI Node</h2></strong>
<strong>Crafting a cinematic aesthetic for your digital art!</strong>

The Absynth Film Look Creator is a custom ComfyUI & fully AI Vibe Coded Node, to elevate your digital renders with professional-grade cinematic qualities. This tool provides a comprehensive suite of controls for film emulation and atmospheric enhancement, allowing you to move beyond simple filters and achieve a truly distinctive visual style. Inspired by the rich, imperfect character of analog film and classic cinematography, it offers precise control over every aspect of your image's look and feel.

<div class="image-container">
  <img src="https://raw.githubusercontent.com/Absynth-Vibe-Coding/Comfy-UI-Nodes/main/absynth-film-look-creator-1.0/absynth-film-look-creator-node-comfy-ui_3.png"
       alt="Absynth Film Look Creator Node 3">
</div>

<strong>Key Features:</strong>

<ul>
  <li><strong>Color & Tonal Grading:</strong> Precisely adjust the brightness, contrast, saturation, temperature, and tint to perform professional color correction and set the overall mood of your scene.</li><br>
  <li><strong>Film Emulation & LUTs:</strong> Emulate the unique characteristics of analog film with controls for grain intensity and a customizable vignette. This node allows you to import and apply your own custom LUT files (.cube format) to instantly transform the color and tone of your renders. The LUT intensity slider provides precise control over the strength of the applied LUT.</li><br>
  <li><strong>Lens & Atmospheric Effects:</strong> Introduce subtle imperfections and artistic flares with chromatic aberration and halation intensity. Fine-tune the visual atmosphere using clarity and bloom to control sharpness and light diffusion.</li><br>
  <li><strong>Highlight & Shadow Control:</strong> Gain granular control over the image's tonal range by defining shadow tint color and highlight tint color. Adjust their influence with dedicated intensity sliders for precise stylistic control.</li>
</ul>

<div class="image-container">
  <img src="https://raw.githubusercontent.com/Absynth-Vibe-Coding/Comfy-UI-Nodes/main/absynth-film-look-creator-1.0/absynth-film-look-creator-node-comfy-ui_4.png"
       alt="Absynth Film Look Creator Node 4">
</div>

<p>Important Note: This is the initial public release (v1.0). Currently, presets are applied and saved upon node execution ("Run"). I am actively working on improving the user interface and functionality in future updates to provide a more streamlined workflow.</p>

<br><br>

<strong>How To Install</strong>
<ol>
  <li>Download the Zip File</li>
  <li>Unpack it into \ComfyUI\custom_nodes\ <br>
  </li>
  <li>In \ComfyUI\custom_nodes\absynth\luts\ you can put as many LUTS as you like :-) <br>
  </li>
  <li>Start Comfy UI</li>
  <li>Double Click the left mouse button in Comfy in a workflow and type in Absynth <br>
  <img width="779" height="126" alt="image" src="https://github.com/user-attachments/assets/aa512985-30fa-4e0c-868b-0901130c8746" /><br>
  </li>
  <li>The Absynth Film Look Creator node is ready for getting connected with image upload, image preview, image save... <br>
    <img width="429" height="904" alt="image" src="https://github.com/user-attachments/assets/89552300-ec7c-46a4-a689-96e4d6f24e3c" /><br>
  </li>
</ol>
<br>
<strong>UPDATE:</strong> There is a workaround for editing all presets: just chain 2 of the nodes together. <br>
<img src="https://raw.githubusercontent.com/Absynth-Vibe-Coding/Comfy-UI-Nodes/main/absynth-film-look-creator-1.0/absynth-film-look-creator-node-comfy-ui_5.png">

<p>In the first node set the preset to custom and in the second choose a preset, then you can make changes in the first node - run and that will change the preset.</p>

<br><br>

<p>I welcome contributions, feedback, and bug reports. Your input is valuable in helping me refine this tool and expand its capabilities for the creative community.</p>

<strong>If you enjoy my Absynth Film Look Creator subscribe to my YouTube channel:</strong> <br>
<a href="https://www.youtube.com/@Electric-Dreamz">https://www.youtube.com/@Electric-Dreamz</a> <br>
Thx! ;-)




=========================================================================



<h2>Absynth LipSync Correction 🎵➡️🎬</h2>

<img width="2079" height="887" alt="image" src="https://github.com/user-attachments/assets/5206a388-3ad5-4528-a822-d7b0f7f5f96c" />


*Because nobody wants their AI rapper looking like they're dubbing a foreign film*

Ever generated an amazing music video with Wan 2.2 S2V, only to discover your AI performer looks like they learned lip-syncing from a badly dubbed kung fu movie? Yeah, we've all been there. This ComfyUI node fixes that awkward "audio is doing the 100m dash while video is taking a leisurely stroll" problem.

## What Does This Thing Actually Do?

Think of it as a really smart audio-video matchmaker. It analyzes your audio energy (when the beat drops, when vocals hit) and compares it to visual changes in your video (when mouths move, when heads bob). Then it nudges everything into perfect harmony.

**The Before Times:**
- Generate video with Wan 2.2 S2V
- Watch in horror as lips move completely out of sync
- Spend 30 minutes manually padding audio in DaVinci Resolve
- Cry a little
- Try again

**The After Times:**
- Generate video with Wan 2.2 S2V
- Connect this node
- Watch it automatically fix the sync
- Make coffee while it works
- Smile

## Installation (The "Please Don't Break My ComfyUI" Guide)

### What You Need
- ComfyUI (any flavor that doesn't crash when you look at it funny)
- Basic ability to copy files without accidentally deleting your entire system

### The Copy-Paste Dance

1. Just download the Zip File and extract it to your custom nodes folder, no dependencies

4. **Restart ComfyUI** (the classic "turn it off and on again" move)

5. **Look for "Absynth LipSync Correction"** in your node menu under video/audio

## How to Use It (Without Losing Your Sanity)

### The Basic Setup

Think of your workflow like a sandwich:
- Bread slice 1: Your video generation
- The good stuff: Absynth LipSync Correction 
- Bread slice 2: Video export

```
Your Awesome Video → [Absynth LipSync Correction] → Final Perfect Video
                            ↗
                    Your Audio ──┘
```

### The Magic Knobs and What They Do

| Knob | What It Does | When to Fiddle With It |
|------|--------------|------------------------|
| **sync_offset_frames** | Manual timing adjustment | When you want to override the AI and do it yourself |
| **audio_sensitivity** | How hard the node listens to your audio | Your vocals are whisper-quiet or screaming loud |
| **auto_detect_offset** | Let the AI figure it out | Almost always leave this ON |
| **smoothing_factor** | How much the AI second-guesses itself | When results are too jittery or too smooth |

### Reading the Tea Leaves (Understanding Outputs)

**synced_video**: Your video, but now with proper timing. This is what you actually want.

**debug_overlay**: A nerdy visualization showing what the AI is thinking:
- Red corner = "I hear audio energy here!"
- Green corner = "I see movement here!" 
- Blue corner = "I applied a timing fix!"
- White corner = "This is frame number X"

**sync_report**: A report card telling you how well everything worked. Look for correlation scores above 0.3 for good results.

## The Science-y Bit (For the Curious)

The node works like a detective investigating a crime scene:

1. **Audio Investigation**: "When does the audio get exciting?" (RMS energy analysis)
2. **Visual Investigation**: "When do things move in the video?" (frame difference detection)
3. **Pattern Matching**: "Do these energy spikes line up with movement?" (cross-correlation)
4. **The Fix**: "Let me nudge things X frames to make it perfect" (temporal offset)

It's basically doing what your brain does automatically when watching movies, but with math.

Settings: You have to ajdust settings per video, just try!
is the music faster than the video? then set sync offset to -5, -10...
is the music slower than the sync offset needs to be 5, 10...

<img width="1086" height="441" alt="image" src="https://github.com/user-attachments/assets/a747cda7-9577-489f-9139-019eb7d83d9c" />



## When Things Go Sideways (Troubleshooting)

### "The Sync is Still Terrible"
**Correlation score below 0.3?**
- Crank up `audio_sensitivity` to 2.0 or 3.0
- Turn down `smoothing_factor` to 0.1
- Check if your audio actually has vocals (instrumental tracks won't sync to lip movement)

### "It Fixed It Backwards"
**Audio rushing ahead or lagging behind?**
- The AI detected the right amount of offset but wrong direction
- Try the opposite sign: if it suggests +8, manually set it to -8
- This is like GPS telling you to turn left when you should turn right

### "It's Not Detecting My Audio"
**Radio silence from the node?**
- Check your workflow connections (that green line should be connected)
- Look at the console output for clues
- Make sure your audio isn't just 10 seconds of silence


### "The Debug Overlay Looks Like Christmas Lights"
**Red and green corners flashing randomly?**
- Your video might have too much motion or your audio might be too chaotic
- Try with cleaner source material
- Increase smoothing_factor to calm things down

<img width="372" height="455" alt="image" src="https://github.com/user-attachments/assets/1879c4eb-1e63-4e15-b064-3f926c0f6445" />


## Real Talk: What This Node Can and Can't Do

### What It's Great At:
- Fixing timing issues in AI-generated music videos
- Working with any audio that has clear energy patterns
- Running on potato computers (it's CPU-only)
- Being completely automatic once you set it up

### What It Won't Do:
- Detect individual facial features (it's not reading lips, just timing)
- Fix videos where there's no correlation between audio and visual movement
- Make your AI look like Ryan Gosling (that's a different node)
- Work miracles with completely silent audio or static video

## Integration with Wan 2.2 (The Whole Point)

This node specifically solves the "Wan 2.2 timing curse" where your carefully crafted audio ends up 4 seconds ahead of your lovingly generated video. Instead of:

1. Generate video
2. Export audio separately  
3. Open DaVinci Resolve
4. Manually add 4 seconds of silence
5. Re-export
6. Hope it's right
7. Repeat until insane

You now do:
1. Generate video
2. Connect this node
3. Coffee break
4. Done

## Pro Tips from the Trenches

- **Start with defaults**: The node usually gets it right on the first try
- **Check the correlation score**: Above 0.3 = good, below 0.2 = probably needs tweaking
- **Negative offsets for fast audio**: If your audio is rushing ahead, use negative values
- **Use debug overlay**: It's actually useful for understanding what's happening
- **Manual override when needed**: Sometimes you know better than the AI

<strong>Example in my S2V Workflow - works also great with rife for smooth motion</strong>
IMPORTANT: use 32 frames in the VHS Combine, since Wan 2.2 S2V does 16 frames - Multiplier 2 = 32 Frames
<img width="2090" height="794" alt="image" src="https://github.com/user-attachments/assets/faaaf3f2-15ca-4f5e-bd06-5976ebc60399" />


## Contributing (Join the Sync Revolution)

Found a bug? Have an idea? Want to make the documentation even more ridiculous? Pull requests welcome!

Just remember:
- Test with different types of audio (rap, singing, instrumental)
- Don't break existing functionality
- Keep the fun factor in any documentation updates

## Credits and Thanks

Created for the Absynth Finetune workflow because manually fixing sync in post-production is about as fun as debugging regex at 3 AM.

Special thanks to everyone who suffered through bad lip-sync before this existed.

## Support

- **GitHub Issues**: For when things break spectacularly
- **ComfyUI Discord**: For general "how do I even use this" questions  
- **YouTube Tutorials**: [Electric-Dreamz Channel](https://www.youtube.com/@Electric-Dreamz)

---

*Version 1.0 - "The One That Actually Works"*

*No AIs were harmed in the making of this node. Some developers lost sleep, but they're used to it.*
