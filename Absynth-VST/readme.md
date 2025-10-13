<h1>🎹 Absynth-VST Documentation</h1>
<strong>Turn your VST plugins into ComfyUI nodes and let AI write your bangers. Yes, really.</strong>

## 📖 Table of Contents

- [What The Heck Is This?](#what-the-heck-is-this)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [The Nodes](#the-nodes)
- [LLM MIDI Generator - The Star of the Show](#llm-midi-generator---the-star-of-the-show)
- [Tips and Tricks](#tips-and-tricks)
- [Troubleshooting](#troubleshooting)
- [FAQ](#faq)

---

## What The Heck Is This?

Absynth-VST is a ComfyUI custom node that lets you:

1. **Load VST plugins** (Serum, Vital, Surge, etc.)
2. **Generate MIDI with AI** (or without, we don't judge)
3. **Render audio** directly in your ComfyUI workflow
4. **Control parameters** like a DJ on too much coffee

Think of it as your DAW, but inside ComfyUI, powered by LLMs that may or may not have better music taste than you.

---

## Installation

### Step 1: Clone This Bad Boy

```bash
cd ComfyUI/custom_nodes
git clone https://github.com/yourusername/absynth-vst
cd absynth-vst
```

### Step 2: Install Dependencies

```bash
pip install -r requirements.txt
```

**What gets installed:**
- `numpy` - Math stuff (boring but necessary)
- `soundfile` - For audio I/O (cool)
- `dawdreamer` - The VST magic (🔥 LEGENDARY 🔥)
- `mido` - MIDI file handling (reliable friend)
- `MIDIUtil` - MIDI generation (your new best friend)
- `requests` - For talking to Ollama (optional)

**Optional LLM Support:**
```bash
# For OpenAI
pip install openai

# For Anthropic Claude
pip install anthropic
```

### Step 3: Restart ComfyUI

Turn it off and on again. Works every time. 🔌

---

## Quick Start

### The "I Just Want To Make Music" Speedrun:

1. **Add LLM MIDI Generator node**
2. **Type:** `"Create a dark techno bassline in D minor"`
3. **Set provider to:** `local`
4. **Set temperature to:** `1.2` (trust us on this one 😉)
5. **Click Queue Prompt**
6. **Receive banger** 🎵

**Optional:** Connect to VST Player node if you actually want to hear it.

---

## 🎛️ The Nodes

### 1. Absynth-VST Player 🎹

**What it does:** Loads your expensive VST plugins and makes them work in ComfyUI.

**Inputs:**
- `vst_path` - Where your VST lives (find it or it finds you)
- `preset` - Dropdown of available presets (put .vstpreset files in `/presets` folder)
- `midi_file` - Dropdown of MIDI files (put .mid files in `/midi` folder)
- `sample_rate` - 44100 is fine, 192000 if you have golden ears
- `buffer_size` - 512 is the sweet spot
- `reverb_tail` - Extra seconds after MIDI ends (for that sweet reverb decay)
- `parameter_1-6` - Control knobs (-1.0 to disable, 0.0-1.0 to control)
- `parameter_X_index` - Manual parameter index (use Parameter Lister to find)
- `parameter_X_name` - Search term (e.g., "cutoff", "reverb")

**Outputs:**
- `file_path` - Where the WAV got saved
- `audio` - The actual audio (ComfyUI audio format)
- `plugin_info` - Metadata about what just happened

**Pro Tips:**
- Put presets in `custom_nodes/absynth-vst/presets/`
- Put MIDI files in `custom_nodes/absynth-vst/midi/`
- Use Parameter Lister to find exact parameter indices

**Known Issues (VST Drama):**
Some VSTs are blacklisted because they crash harder than a dubstep drop:
- ❌ **Spire** - Crashes ComfyUI (blacklisted)
- ❌ **Omnisphere** - Too omnipotent (incompatible)
- ❌ **Kontakt** - Contact denied (incompatible)
- ❌ **Massive X** - Massively broken (incompatible)

**Tested & Working VSTs:**
- ✅ **Serum 2** - Confirmed working! (See parameter list below)
- ✅ **Viper** - Tested and works perfectly
- ✅ **Vital** - Free and excellent
- ✅ **Surge XT** - Open source legend
- ✅ **Dexed** - FM synthesis goodness

---

### 🎛️ Serum 2 Parameter Guide

Here are the parameter indices for Serum 2 (discovered through testing):

**Basic Controls:**
- **Volume:** Parameter 44
- **Cutoff:** Parameter 8
- **Pitch Bend:** Parameter 7
- **Semitone:** Parameter 1

**Effects:**
- **Distortion:** Parameter 9
- **Panning:** Parameter 22

**Sound Design (Make it weird):**
- **Detune/Crush:** Parameters 23, 24, 28, 29, 79, 114
- **Phase:** Parameter 116

**Voices & Polyphony (Make it FAT!):**
- Parameters: 25, 46, 51, 53, 55, 76, 78
- **Parameter 80 at 0.70** = Trance mania! 🎹

**Pro Tip:** Use Parameter Lister node to discover parameters for other VSTs!

---

### 📁 Folder Structure

```
custom_nodes/
└── absynth-vst/
    ├── presets/
    │   ├── Serum/
    │   │   ├── Bass.vstpreset
    │   │   └── Lead.vstpreset
    │   └── Vital/
    │       └── Pluck.vstpreset
    └── midi/
        └── your_midi_files.mid
```

**Important:** Your presets must be in `.vstpreset` format. If you have `.fxb` presets, save them as `.vstpreset` in Cubase (or your DAW) first, then copy them to the presets folder.

---

### 2. Absynth-VST Parameter Lister 🔍

**What it does:** Lists every single parameter in your VST. All 847 of them. Have fun.

**Inputs:**
- `vst_path` - Your VST plugin path
- `search_filter` - Type "cutoff" to find cutoff knobs (genius, right?)
- `sample_rate` - 44100 is still fine

**Output:**
- `parameter_list` - A novel-length list of every parameter

**When to use:**
- When parameter auto-search fails
- When you need exact parameter indices
- When you want to feel overwhelmed by options

**Example Output:**
```
[  0] Master Volume                       = 0.800 (80%)
[  1] Filter Cutoff                       = 0.500 (1000 Hz)
[  2] Filter Resonance                    = 0.300 (30%)
[847] That One Knob Nobody Uses           = 0.000 (Off)
```

---

### 3. Absynth-VST MIDI Creator ✏️

**What it does:** Creates simple MIDI files when you're feeling old school.

**Inputs:**
- `notes` - MIDI note numbers: `60,64,67` (C, E, G - a C major chord!)
- `velocities` - How hard to hit: `100,100,100`
- `durations` - How long in beats: `1.0,1.0,1.0`
- `output_path` - Where to save it

**Example:**
```
notes: 60,62,64,65,67
velocities: 80,85,90,95,100
durations: 0.5,0.5,0.5,0.5,2.0
```
Creates a rising C major scale that ends with a long note. Beautiful. 😢

---

### 4. Absynth-VST Info Display 📊

**What it does:** Shows you what the VST Player just did.

**Input:**
- `vst_info` - Connect from VST Player's plugin_info output

**Output:**
- `info_text` - Formatted text showing plugin name, preset, and status

**Use case:** Debugging, or just wanting to feel informed.

---

### 5. Absynth-VST LLM MIDI Generator ⭐ (THE STAR OF THE SHOW)

**What it does:** Uses AI to generate MIDI. Or just generates good MIDI locally. Either way, you get bangers.

**Inputs:**
- `prompt` - Describe your musical vision (or "make bleep bloop")
- `api_key` - Your API key (if using OpenAI/Anthropic)
- `llm_provider` - `local`, `ollama`, `openai`, or `anthropic`
- `model` - Model name (if using LLM)
- `temperature` - **SET TO 1.2 FOR MAGIC** 🔥
- `seed` - `-1` for random, or set number for reproducibility
- `bpm` - Beats per minute (128 for techno, 90 for chill)
- `duration` - Length in seconds (10 is a good start)
- `output_filename` - Base name for the file

**Output:**
- `midi_path` - Path to the generated MIDI file (auto-saved in `/midi` folder)

---

## 🎵 LLM MIDI Generator: The Star of the Show

### Temperature Guide 🌡️

**The Secret Sauce:** Temperature controls creativity (for LLM providers only)

- **0.0-0.3** - Boring predictable patterns (for elevator music)
- **0.4-0.7** - Balanced and reliable (safe choice)
- **0.8-1.0** - Getting creative (nice variety)
- **1.2** - ✨ **THE SWEET SPOT** ✨ (discovered by our users, chef's kiss)
- **1.3-1.8** - Experimental chaos (sometimes brilliant, sometimes broken)
- **1.9-2.0** - Full artistic insanity (code might explode, music might transcend)

### Prompt Examples

**Chord Progressions:**
```
"Create a D major chord progression"
"Make C minor chords"
"Generate a sad chord progression in E minor"
```

**Riffs:**
```
"Create a G riff"
"Make a dark techno riff in D minor"
"Generate an aggressive metal riff in Drop D"
```

**Pads:**
```
"Create an F minor pad"
"Make a dreamy ambient pad in A major"
"Generate an atmospheric cinematic pad in C minor"
```

**Bass Lines:**
```
"Create a dark techno bassline in D minor"
"Make a groovy funk bass in E minor"
"Generate a deep dubstep bass in F# minor"
```

**Melodies:**
```
"Create a C minor melody"
"Make an uplifting trance melody in G major"
"Generate a sad piano melody in B minor"
```

**Get Creative:**
```
"Create a mysterious underwater melody in E minor"
"Make a glitchy cyberpunk bass in F# minor"
"Generate a nostalgic 80s synth lead in A minor"
"Create a chaotic breakcore pattern in D minor"
```

**Pro Example (Try This!):**
```
"You are a professional music producer. Make an epic pop trance melody with chord progression in E minor."
```
This detailed prompt works especially well with LLM providers at temperature 1.2!

### Supported Keys

**Major:** C, D, E, F, G, A, B (and their variations)
**Minor:** C, D, E, F, G, A, B (and their variations)

Just include the key in your prompt: `"in D minor"`, `"in G major"`, etc.

### Music Types (Auto-Detected)

The generator listens for keywords:
- **"chord"/"progression"** → Generates full chord progressions (I-IV-V-I style)
- **"riff"/"pattern"/"groove"** → Creates tight, rhythmic repeating patterns
- **"pad"/"ambient"/"atmosphere"** → Long sustained chords with 7ths
- **"bass"** → Low-end focused patterns
- **Default** → Melodic phrases with proper phrasing

### LLM Providers

#### Local (Recommended for Most Users)
```
Provider: local
Temperature: Ignored (use seed for variation)
Cost: FREE 💰
Speed: INSTANT ⚡
Quality: Surprisingly good!
```

**Why use local:**
- Works offline
- No API keys needed
- No rate limits
- Generates in milliseconds
- Actually makes pretty decent music

#### Ollama (For Local LLM Nerds)
```
Provider: ollama
Model: llama2, codellama, mistral
Temperature: 0.7-1.2
Cost: FREE (but requires local Ollama installation)
Speed: 5-30 seconds
Quality: Good to excellent
```

**Setup Ollama:**
1. Install from https://ollama.com
2. Run: `ollama pull llama2`
3. Keep Ollama running in background
4. Set provider to "ollama"

#### OpenAI (For The Big Brain)
```
Provider: openai
Model: gpt-4, gpt-3.5-turbo
Temperature: 0.6-1.2
Cost: $$$
Speed: 3-10 seconds
Quality: Excellent
```

**Setup:**
1. Get API key from platform.openai.com
2. `pip install openai`
3. Paste API key in node
4. Prepare wallet

#### Anthropic (For Claude Fans)
```
Provider: anthropic
Model: claude-3-sonnet-20240229
Temperature: 0.7-1.2
Cost: $$
Speed: 3-10 seconds
Quality: Excellent
```

**Setup:**
1. Get API key from console.anthropic.com
2. `pip install anthropic`
3. Paste API key in node
4. Enjoy Claude's musical creativity

---

## 💡 Tips & Tricks

### Golden Workflows

**1. The "AI Composer" Stack:**
```
LLM MIDI Generator → VST Player → Audio Output
```
Describe music, get music. It's that easy.

**2. The "Layered Banger" Stack:**
```
LLM Generator (bass) ─┐
                       ├→ Multiple VST Players → Mix → Export
LLM Generator (lead) ─┘
```
Generate multiple parts, layer them up.

**3. The "Parameter Tweaker" Stack:**
```
LLM Generator → VST Player (with params) → Audio Output
```
Generate MIDI, but control the VST parameters for unique sounds.

### Pro Tips

1. **Save Your Seeds** 🌱
   - Found an amazing generation at temp 1.2? Write down the seed!
   - Same seed = same music (with same prompt)

2. **Experiment With Keys** 🎹
   - Minor = sad/dark vibes
   - Major = happy/uplifting vibes
   - Try all 12 keys for different flavors

3. **BPM Matters** ⏱️
   - 60-80: Ballads, slow jams
   - 90-110: Hip-hop, R&B
   - 120-130: House, pop
   - 140-150: Techno, trance
   - 160-180: Drum & bass, hardcore

4. **Duration Sweet Spots** ⏰
   - 4-8 seconds: Short loops
   - 10-16 seconds: Full phrases
   - 20-30 seconds: Mini compositions

5. **Combine Generators** 🎼
   - Generate bass in D minor
   - Generate chords in D minor
   - Generate melody in D minor
   - Render all three with same VST
   - Instant arrangement!

---

## 🔧 Troubleshooting

### "VST not found!"

**Problem:** Can't find your VST plugin.

**Solution:**
- **Windows:** `C:\Program Files\Common Files\VST3\YourPlugin.vst3`
- **Mac:** `/Library/Audio/Plug-Ins/VST3/YourPlugin.vst3`
- **Linux:** `~/.vst3/YourPlugin.vst3`

Use the actual path on YOUR system, not the default.

---

### "VST crashes ComfyUI!"

**Problem:** Some VSTs crash harder than your first mixtape.

**Solution:**
- Check the blacklist (Spire, Omnisphere, Kontakt, Massive X)
- Try these confirmed working VSTs:
  - Serum ✅
  - Vital ✅
  - Surge XT ✅
  - Dexed ✅

---

### "No MIDI files in dropdown!"

**Problem:** MIDI dropdown only shows "select MIDI file"

**Solution:**
1. Put .mid files in `custom_nodes/absynth-vst/midi/`
2. Restart ComfyUI
3. Files should appear in dropdown

Or just use the LLM Generator and connect it directly!

---

### "Parameter search finds nothing!"

**Problem:** Auto-search can't find "cutoff"

**Solution:**
1. Use **Parameter Lister** node
2. Search for your parameter
3. Note the **index number** (e.g., `[42]`)
4. Set `parameter_X_index` to `42`
5. Set `parameter_X` value to desired amount

Manual mode never fails!

---

### "Ollama not responding!"

**Problem:** Getting connection errors with Ollama provider

**Solution:**
1. Is Ollama actually running? Check terminal/task manager
2. Did you pull a model? `ollama pull llama2`
3. Is it on localhost:11434? (default port)
4. Try pinging: `curl http://localhost:11434/api/tags`

Still broken? Just use `local` provider. 🤷

---

### "Generated MIDI sounds weird!"

**Problem:** Music doesn't sound right

**Solution:**
- Try different temperature (1.2 is the magic number!)
- Change the seed (some seeds are cursed)
- Be more specific in prompt: "dark techno bassline" vs "make music"
- Try different keys: some keys just hit different
- Use local provider if LLM is drunk

---

### "DawDreamer installation failed!"

**Problem:** Can't install dawdreamer

**macOS Solution:**
```bash
brew install portaudio
pip install dawdreamer
```

**Linux Solution:**
```bash
sudo apt-get install libsndfile1 portaudio19-dev
pip install dawdreamer
```

**Windows Solution:**
Should work out of the box. If not, you might need Visual C++ Build Tools.

---

## ❓ FAQ

### Q: Can I use this commercially?

A: Check your VST licenses and LLM provider terms. The code itself is free to use.

### Q: Why temperature 1.2 specifically?

A: One of our users discovered it creates the perfect balance of creativity and coherence. Not too predictable, not too chaotic. Just right. 🐻

### Q: Do I need an LLM to use this?

A: Nope! Set provider to `local` and it generates good music instantly without any AI magic.

### Q: Can I generate loops?

A: Yes! Just set duration to 8 or 16 seconds and most generations will loop nicely.

### Q: What if I want silence?

A: Then why are you here? 😄 But seriously, set velocity to 0 or duration to 0.01.

### Q: Can I load multiple VSTs at once?

A: One VST per player node, but you can use multiple player nodes in one workflow!

### Q: My VST has 10,000 parameters. Do I really need to search them all?

A: Use the Parameter Lister with search filter! Type "cutoff" to find only cutoff-related params.

### Q: Can this replace my DAW?

A: For quick ideas and experimentation? Yes! For full production? Probably not (yet). But it's getting there!

### Q: Why is it called Absynth-VST?

A: Because we like absinth... I mean, it's for synthesizing abstract sounds. Yeah, that's it. 🍸

---

## 🎓 Advanced Usage

### Custom Scales

Want to generate in exotic scales? Edit the `_create_fallback_midi` method and add your scale:

```python
elif 'dorian' in prompt_lower:
    scale = [0, 2, 3, 5, 7, 9, 10]  # Dorian mode
elif 'phrygian' in prompt_lower:
    scale = [0, 1, 3, 5, 7, 8, 10]  # Phrygian mode
```

### Custom Chord Progressions

Edit `_generate_chord_progression` to add your own progressions:

```python
progressions = [
    [0, 3, 4, 0],      # I-IV-V-I
    [0, 5, 3, 4],      # I-vi-IV-V (your new progression)
]
```

### Batch Generation

Use ComfyUI's batch mode to generate 100 variations:
1. Set seed to -1 (random)
2. Enable batch mode
3. Queue 100 prompts
4. Sort through your new music library
5. Keep the bangers

---

## 🙏 Credits

- **DawDreamer** - For making VST hosting possible in Python
- **MIDIUtil** - For reliable MIDI file generation
- **ComfyUI** - For being the best node-based UI
- **That one user who discovered temperature 1.2** - You're a legend
- **You** - For reading this whole thing. Here's a cookie: 🍪

---

## 📝 License

Check the LICENSE file. It's probably MIT or something equally permissive.

---

## 🐛 Found a Bug?

Cool! Open an issue on GitHub with:
1. What you tried to do
2. What actually happened
3. Any error messages
4. Your VST (so we can test)
5. A funny joke (optional but appreciated)

---

## 🌟 Final Words

Go forth and make bangers. The AI gods have blessed you with generative MIDI.

Remember: Temperature 1.2 is the way. 🔥

Now stop reading documentation and start making music! 🎵🎹🎸

---

*"The best DAW is the one you actually use. Even if it's inside an image generation tool."* - Ancient Music Producer Proverb
