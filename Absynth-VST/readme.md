<h1>🎹 VST Plugins in ComfyUI</h1>
<strong>Turn your VST plugins into ComfyUI nodes and let AI write your bangers. Yes, really.</strong>

<img width="2121" height="900" alt="image" src="https://github.com/user-attachments/assets/92d90571-36d7-492a-a992-129ef7003257" />





## 📖 Table of Contents

- [What The Heck Is This?](#what-the-heck-is-this)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [The Nodes](#the-nodes)
- [LLM MIDI Generator - Still experimental - bigger LLM more chance for better result ](#llm-midi-generator)
- Absynth Mixer: Mix leads, bass, pads, drums..together and adjust volume
- Drums...but how?
- VST Editor <- now you can see and edit your VST Plugin in Comfy!
- VST List <- it scans your VST3 Folder and gives you a dropdown with all your VST3 Plugins
- ABSYNTH Preset Maker <- YES!! you read this right, the LLM can generate your Presets incl. Effects, currently only Serum 2 (more to come)
- Added Midi Analyzer: Yes! It scans your Midi and detects chords and scales and passes it to the LLM Midi Looper
- LLM Midi Looper: It creates Leads, Basslines, Pads, Arps, Chord Riffs...that fits to your chords :-)
- LLM Cache <- Local Caching for your Prompts, tells the LLM what it did before and will always do different results
- Absynth Audio Preview: Listen, Loop and Save Audio with nice Waveform
- LLM Status Display: shows LLM Success or Fail
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
5. Local Random Midi Generator <- can give you actually pretty good stuff which is usable in your daw.<br>
Local is basicly Claude 4.5 it wrote the whole thing :-)

Watch the Video here and subscribe to my YouTube Channel: https://www.youtube.com/watch?v=UgYhJi4wWcE <br>

Think of it as your DAW, but inside ComfyUI, powered by LLMs that may or may not have better music taste than you.

---

## Installation

### Step 1: Download the ZIP and put the Absynth-vst folder in your Custom Nodes folder like its 1999

Enter the Matrix...i mean the Absynth-vst folder

### Step 2: 

- open CMD
- Install Dependencies

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


### Step 3: Restart ComfyUI

Turn it off and on again. Works every time. 🔌

---

## Quick Start

### The "I Just Want To Make Music" Speedrun:

1. The ComfyUI Workflow is in the folder Workflow
2. In the VST Player type in your vst3 path like: C:\Program Files\Common Files\VST3\Serum2.vst3
3. Select a Preset from the list, you can add presets in format .vstpreset <- only these will work
<img width="508" height="814" alt="image" src="https://github.com/user-attachments/assets/cd688fd0-a96f-4715-bc24-6e2de5398d17" />




4. if you want you can change cutoff...using the parameters midi cc
5. In the LLM Node **Set provider to:** `local` <- randomly generates pretty good stuff or use LLM like in ollama, GPT... <- still experimental but try for example local deepseek huihui_ai/deepseek-r1-abliterated:14b thsis one got some really good seeds, quite musical and gpt-oss:20b
The Coder LLMs follow your prompt better, especially if you prompt for something like "chord progreesion with changing chords that are arpeggiated 16th notes"
6. **Set temperature to:** `1.2` 
7. Type in Prompt, see examples
8. You can also use a MIDI from the list which is in the folder MIDI, add as many as you like
<img width="606" height="824" alt="image" src="https://github.com/user-attachments/assets/68e23c68-571b-43de-9cff-70f85f2af2d0" />



9. **Click Queue Prompt**
11. **Receive 🎵 and the MIDI



---

## The Nodes

### 1. Absynth-VST Player 🎹
**What it does:** Loads your expensive VST plugins and makes them work in ComfyUI.

<img width="372" height="795" alt="image" src="https://github.com/user-attachments/assets/adf4843f-df3b-4389-be5c-95ba71279b57" />



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
- ✅ **DIVA** - Yes it works!
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

<img width="1733" height="778" alt="image" src="https://github.com/user-attachments/assets/bed9d8aa-0d06-448a-9e9f-5933d2ce51b8" />

Leave Empty and hit run to show all parameters of a VST

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

<img width="334" height="220" alt="image" src="https://github.com/user-attachments/assets/a9a2eef9-cc69-44b0-a896-0ab12363a14b" />


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
<img width="466" height="379" alt="image" src="https://github.com/user-attachments/assets/c9992412-7382-442d-bfe9-64fd8978458d" />


**What it does:** Shows you what the VST Player just did.

**Input:**
- `vst_info` - Connect from VST Player's plugin_info output

**Output:**
- `info_text` - Formatted text showing plugin name, preset, and status

**Use case:** Debugging, or just wanting to feel informed.

---

## 5. Absynth-VST  MIDI Generator
<img width="502" height="493" alt="image" src="https://github.com/user-attachments/assets/5886167b-214c-4cb0-a46e-97133806fb8a" />


**What it does:** Uses AI to generate MIDI. Or just generates good MIDI locally. Either way, you get bangers.

**Inputs:**
- `prompt` - Create trance arpeggio in D minor with chord changes.
- `api_key` - Your API key (if using OpenAI/Anthropic)
- `_provider` - `local`, `ollama`, `openai`, or `anthropic`
- `model` - Model name (if using )
- `temperature` - **SET TO 1.2...try **
- `seed` - `-1` for random, or set number for reproducibility
- `bpm` - Beats per minute (128 for techno, 90 for chill)
- `duration` - Length in seconds (10 is a good start)
- `output_filename` - Base name for the file

**Output:**
- `midi_path` - Path to the generated MIDI file (auto-saved in `/midi` folder)

---

##  MIDI Generator   

### Temperature Guide 🌡️

**The Secret Sauce:** Temperature controls creativity (for LLM providers only)

- **0.0-0.3** - Boring predictable patterns (for elevator music)
- **0.4-0.7** - Balanced and reliable (safe choice)
- **0.8-1.0** - Getting creative (nice variety)
- **1.2** - ✨ **pretty good also for the local setting** ✨
- **1.3-1.8** - Experimental chaos (sometimes ok, sometimes broken)
- **1.9-2.0** - Full artistic insanity (code might explode, music might transcend)

### Prompt Examples

**Sequences:**
```
"Melodic trance sequencer pattern in E minor with chord changes and bassline." <- sounds amazing, try this one!
"Create trance arpeggio in D minor with chord changes."
"Generate uplifting trance arpeggio in C major"
"Trance arpeggiated progression in A minor with chord changes"
"Create trance sequence in F# minor"
"Generate complex techno sequencer pattern in C minor"
"Generate Trance sequencer pattern in C minor
```

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

** Example:**
```
Create trance arpeggio in D minor with chord changes..
Create trance sequence in F# minor
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

<h2>Absynth Mixer</h2>

<img width="492" height="798" alt="image" src="https://github.com/user-attachments/assets/8d7f060a-66a5-4a43-8a81-5669dca9a4e4" />

- 4 Audio Inputs (can be any)
- 4 individual Audio Outputs
- Mixer Output
- Track Volume
- Track Panning left/right
- Set Name of Track


<h2>...but how?</h2>

<img width="775" height="765" alt="image" src="https://github.com/user-attachments/assets/20f6059a-af65-4a63-b83d-6be36bc4fed4" />

1. Use Sample Robot 
2. Use the Table in the Workflow to put your Samples on the Keyboard where they should be
3. Export Drumkit in Sample Robot as .sfz
4. Import Drumkit in Serum2 into Wavetable as .sfz
5. Save Serum 2 Preset in Cubase... as .vstpreset
6. Put the .vstpreset in the absynth-vst node folder Presets like the synth presets.
7. In the node select Serum 2 and select your Drumkit Preset
8. Let the Drums roll and enjoy!

Prompts for Drums
Watch my Video: https://www.youtube.com/watch?v=1aKlRxVkuxY + See Prompt Examples in the Workflows you can create simple drum loops or even drum sequences!

<h2>Absynth Audio Preview</h2>
A node that shows how great your Audio Waveform looks ;-)

<img width="1106" height="699" alt="image" src="https://github.com/user-attachments/assets/814542af-f38c-4f60-a92c-ef8f2391e987"/>

-Play Audio <br>
-Select Audiopart and loop it<br>
-Save Audio<br>

<h2>Absynth LLM Status Display</h2>
A node that shows you if LLM MIDI generation was successful or if it failed, and it looks great ;-)

<img width="1117" height="668" alt="image" src="https://github.com/user-attachments/assets/49389d84-9c1c-4ef0-82a5-088883369023"/>



<H2>VST EDITOR <- See and control your VST in ComfyUI</H2>

<img width="1900" height="806" alt="image" src="https://github.com/user-attachments/assets/bc02bb23-f431-4a03-86ba-7135c308aa33" />
 1. Set capture parameters to true in the VST Editor<br>
 2. Select Plugin<br>
 3. Run the workflow which is included in the folder<br>
 4. Your VST Plugin popsup in front of you!<br>
 5. Select your Preset<br>
 6. Twist some knobs<br>
 7. Close the VST Popup<br>
 8. Your Workflow continues and passed the captured data incl. preset to the player<br>

<br>
IMPORTANT: Not all Plugins will work perfectly with this, there is a LOT of testing involved and would need additional coding to make all of them work as expected, which is not possible, there are millions of plugins available.
<br>
What works very well:
- SERUM 2 incl. Presetmaker<br>
- VIPER<br>
- DIVA<br>
- DUNE<br>
<br>
Dont use Spire right now, it will crash comfy, others just test and expirement!
<br>

<h2>VST LIST</h2>
- it scans your VST3 folder and gives you a dropdown to select your plugin
<img width="756" height="836" alt="image" src="https://github.com/user-attachments/assets/e1cfdbad-0485-4737-8469-069293c0c174" />


<h2>ABSYNTH PRESET MAKER</h2>
Oh yes!! The LLM can now make Serum 2 Presets for you incl. effects like reverb and delay...
<img width="1999" height="729" alt="image" src="https://github.com/user-attachments/assets/6dd3e14d-03c8-492c-8ab5-094c16b29024" />

1. Select Plugin from Dropdown List <- right now only Serum 2 will work (more to come :-)<br>
2. Select your LLM, deepseek v3.1:671B Cloud is incredible and can do it (its even free for a long time)<br>
3. Prompt like: make an epic trance pluck with reverb<br>
3. Run the Workflow<br>
4. It creates you a .vstpreset that works in Serum 2!<br>
5. it saves the preset automaticly in the vst3 folder of Serum 2 and also in the folder of my node<br>
<br>
I use Cubase for producing Music, so this is how it works now, i am not planning to do any further DAW optimizations.<br>
VST3 is a huge standard, so it should work with any DAW basifly that supports .vstpreset format.<br>

Right now you can Generate:<br> 
- Leads, plucks, pads, bass <br>
- Sequences using LFO <br>
- Arpeggiator including 8th, 16th, chords, directions like up, down, convergence... <br>
- Add effects like reverb, delay, compressor, chorus...to your Serum 2 Preset! <br>

<h2>Absynth MIDI Analyzer</h2>
<img width="567" height="423" alt="image" src="https://github.com/user-attachments/assets/4e44aa45-6b81-486d-ae31-44a0884cd609" /><br>
Yes! It really analyzs your Midi-files and detects the right chords and scales, also timing, bars, tempo<br>
1. Click browse Midi File<br>
2. Select your Midi<br>
3. The node analyzes the File when you click Run<br>
<img width="530" height="564" alt="image" src="https://github.com/user-attachments/assets/c57a72c6-cbe1-4554-bd37-ae18bd85d826" />
<br>

<br><br>
<h2>Absynth LLM Midi Loop Generator</h2>
  <img width="583" height="711" alt="image" src="https://github.com/user-attachments/assets/e79e9c5d-5f60-433b-87b3-47cc3299504d" /><br>
Yes! it gets the analyzed Midi information from the analyzer and generates the right leads, bass, pads, arps and riffs to your chords and scale!

1. Pick LLM or Local, both work
2. For LLM also set Temperature -> see included comfy workflow for documentation
3. Set Note Density -> sets the amound of notes basicly
4. Prompt create a trance chord riff...(prompt examples see workflow)
5. Yes! It generates Midi that rally fits to your chords it has detected :-)
  
<h2>LLM Local Caching</h2>

We now have cache which prevents repetetive MIDI generation!<br>
The cache tells the LLM what it did before and so it should always generate new MIDI.<br>

Threshold = 0.7 (Recommended - High Diversity)<br>
  - Rejects anything >70% similar<br>
  - Forces very different melodies<br>
  - Good for creative variety<br>
<br>
  Threshold = 0.85 (Moderate)<br>
  - Allows more similarity<br>
  - Still prevents duplicates<br>
  - Good for variations on a theme<br>
<br>
  Threshold = 0.5 (Strict)<br>
  - Rejects anything >50% similar<br>
  - VERY different melodies every time<br>
  - Might retry a lot<br>



## Tips and Tricks<br>

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

## Troubleshooting

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
  - Viper ✅
  - Diva ✅
  - Vital ✅
  - Surge XT ✅
  - Dexed ✅

---

### "No MIDI files in dropdown!"

**Problem:** MIDI dropdown only shows "select MIDI file or let it locally and randomly generate Midi for you"

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

## FAQ


### Q: Why temperature 1.2 specifically?

A: Tt creates a good balance of creativity and coherence. Not too predictable, not too chaotic. The result will also depend on your presets of course, is it an arp...and hey its still only AI ;-)

### Q: Do I need an LLM to use this?

A: You can use this also without LLMs just select a Midi File or switch to local generation

### Q: What if I want silence?

A: Then why are you here? 😄

### Q: Can I load multiple VSTs at once?

A: One VST per player node, but you can use multiple player nodes in one workflow!

### Q: My VST has 10,000 parameters. Do I really need to search them all?

A: Use the Parameter Lister with search filter! Type "cutoff" to find only cutoff-related params.

### Q: Can this replace my DAW?

A: Hello?? Of course not!

### Q: Why is it called Absynth-VST?
Because i use this Nickname since 25 years of music producing

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


## 🐛 Found a Bug?

Cool! Open an issue on GitHub with:
1. What you tried to do
2. What actually happened
3. Any error messages
4. Your VST (so we can test)
5. A funny joke (optional but appreciated)



<strong>Bugfixes and improvements in version v1.1.1</strong>

-Fixed bug when the created midi was messed up and played too fast in comfy<br>
-LLM Midi Generator: is now a dropdown and your LLMs from Ollama are automaticly detected<br>
-VST Player: added BPM Override if you want to change the tempo in the player<br>
-The Midi Files are now saved with key and bpm info in the name<br>


<strong>Improvements in version v1.1</strong>

Sequences: Support for generating musical sequences based on harmonic context. Dynamic sequencer system with intelligent rhythm and harmony processing Best suited for Trance, EDM, House, Progressive House/Trance, Techno
Circle of Fifths (Harmonic Theory) implemented
Related keys are now derived using the Circle of Fifths.
Progressions with rhythmic patterns: Extended MIDI chord progressions with rhythm patterns.
Voice Leading: Minimal voice movement between chords
Common tones are sustained
Automatic inversions for smooth transitions
Extended Harmonies:
Proper chord extensions and advanced voicings are now supported.
Key Detection: The system now recognizes musical keys from the prompt more accurately.



## 🌟 Final Words

Go forth and make bangers. The AI gods have blessed you with generative MIDI.

Now stop reading documentation and start making music! 🎵🎹🎸

---

*"The best DAW is the one you actually use. Even if it's inside an image generation tool."* - Ancient Music Producer Proverb
