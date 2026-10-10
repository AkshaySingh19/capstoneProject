# Dataset Purpose & Pipeline Explanation
## Capstone Project: Lip-Sync Deepfake Detection via Phoneme-Viseme Mismatch (M/B/P)

---

## 📋 TABLE OF CONTENTS
1. [Overview: What This Project Actually Does](#overview)
2. [Dataset Inventory: Each Dataset's Role](#dataset-inventory)
3. [Why Transcripts Matter: The M/B/P Connection](#why-transcripts)
4. [End-to-End Pipeline Walkthrough](#pipeline-walkthrough)
5. [Test Conditions: What You're Actually Evaluating](#test-conditions)
6. [Current State & Next Steps](#current-state)

---

## 1. OVERVIEW: WHAT THIS PROJECT ACTUALLY DOES <a name="overview"></a>

### The Core Hypothesis
> **Producing /m/, /b/, /p/ phonemes requires complete lip closure.** Many lip-sync deepfake generators fail to reproduce this reliably — the generated mouth stays slightly open during these sounds.

### What You're Building
1. **A binary classifier** (CNN) that takes a cropped mouth image → outputs `open` / `closed`
2. **Trained on diverse speakers** (not just one person like the original 2020 study)
3. **Evaluated against modern generators** (2024–2026: Wav2Lip, SadTalker, diffusion models)
4. **Using leave-speakers-out splits** (train/val/test have zero speaker overlap)

### The Detection Logic
```
Real video:  /m/ phoneme  →  mouth CLOSED  →  classifier says "closed"  ✓
Fake video:  /m/ phoneme  →  mouth OPEN    →  classifier says "open"    → FLAG AS FAKE
```

---

## 2. DATASET INVENTORY: EACH DATASET'S ROLE <a name="dataset-inventory"></a>

### 2.1 LRS2 / LRS3 (Lip Reading Sentences) — **PRIMARY TRAINING DATA**
| Property | Detail |
|----------|--------|
| **Source** | University of Oxford / BBC |
| **Access** | Research registration + download agreement (free) |
| **Size** | ~100–200 GB each (train/val/test splits) |
| **Speakers** | Thousands of diverse BBC speakers (news, docs, shows) |
| **Role** | **Main multi-speaker real video corpus for classifier training** |

**What you get from LRS2/LRS3:**
- Video clips (.mp4) with clear frontal faces
- Audio (.wav/.m4a) — clean broadcast quality
- **Sentence-level transcripts** (from BBC scripts) — **CRITICAL**
- Speaker IDs in metadata — enables leave-speakers-out splits
- Train/val/test splits pre-defined

**What you DON'T get:**
- ❌ Phoneme-level timestamps
- ❌ Mouth open/closed labels
- ❌ Any deepfake videos

**Why it's PRIMARY:**
1. **Transcripts exist** → forced alignment gives precise M/B/P boundaries
2. **Diverse speakers** → solves the "single speaker (Obama)" limitation of original paper
3. **Large scale** → thousands of hours = enough M/B/P frames for CNN training
4. **Speaker IDs** → proper leave-speakers-out evaluation (no data leakage)

---

### 2.2 VoxCeleb2 — **SECONDARY TRAINING DATA (Diversity Boost)**
| Property | Detail |
|----------|--------|
| **Source** | University of Oxford — VoxCeleb |
| **Access** | Research agreement (free for academic) |
| **Size** | 6,000+ celebrity videos, ~2,000+ speakers |
| **Speakers** | Celebrities: diverse age, gender, ethnicity, accents |
| **Role** | **Supplement LRS2/LRS3 with maximum demographic diversity** |

**What you get from VoxCeleb2:**
- Video clips from YouTube interviews (in-the-wild, not studio)
- **Speaker identity labels** (very clean)
- Face tracks (pre-cropped in some versions)
- Extreme demographic diversity

**What you DON'T get:**
- ❌ **No transcripts** → must use WhisperX ASR mode (less accurate)
- ❌ No phoneme labels
- ❌ No mouth-state labels
- ❌ No deepfakes

**Why it's SECONDARY (not primary):**
- No transcripts → phoneme alignment noisier → M/B/P boundaries less precise
- But: adds speakers LRS2/LRS3 don't have (different accents, ages, lighting)
- Use for: **data augmentation**, **domain generalization testing**

---

### 2.3 DeepfakeTIMIT — **TERTIARY / SANITY-CHECK DATA**
| Property | Detail |
|----------|--------|
| **Source** | Korshunov & Marcel (Zenodo) |
| **Access** | **Fully open** — no agreement needed |
| **Size** | 620 videos (310 pairs), ~226 MB |
| **Speakers** | 32 subjects (from VidTIMIT) |
| **Role** | **Small structured baseline + real/fake pairs for quick iteration** |

**What you get from DeepfakeTIMIT:**
- **Real + fake pairs** (same speaker, same audio, face-swapped video)
- Lower quality + higher quality versions
- **Known prompt texts** (VidTIMIT prompts) → transcripts exist!
- Small, fast to download, easy to experiment with

**What you DON'T get:**
- ❌ Modern lip-sync fakes (these are 2018-era face swaps, not lip-sync)
- ❌ Large speaker count (only 32)
- ❌ Phoneme/mouth labels

**Why it's useful:**
- **Phase 1 smoke test**: Verify your pipeline works end-to-end before downloading 200GB
- **Sanity check**: Does classifier detect *any* fake? (Even old face-swap fakes)
- **Debug annotations**: Small enough to manually spot-check WhisperX alignment

---

### 2.4 FakeAVCeleb — **EVALUATION: REAL + MODERN LIP-SYNC FAKES**
| Property | Detail |
|----------|--------|
| **Source** | Hasan et al. (NeurIPS 2021) |
| **Access** | Research agreement via GitHub |
| **Size** | ~11,000 videos |
| **Speakers** | 4 ethnic backgrounds, balanced gender |
| **Role** | **Only public dataset with REAL multi-speaker videos + MODERN lip-sync fakes** |

**What you get from FakeAVCeleb:**
- Real videos (diverse speakers)
- **Lip-sync deepfakes** (multiple generators, including newer ones)
- Audio deepfakes (separate track)
- Demographic metadata
- **Generator type labels** for each fake

**Why it's for EVALUATION (not training):**
- Generator labels = ground truth for "which generator made this"
- Real videos from held-out speakers = negative test samples
- **Test Condition 2** can use FakeAVCeleb fakes directly (if generators match your targets)

---

### 2.5 Synthetic Generation Pipeline — **EVALUATION: CONTROLLED MODERN FAKES**
| Generator | Year | Type | Repo |
|-----------|------|------|------|
| **Wav2Lip** | 2020 (base) | Audio-driven lip-sync (GAN) | `wilderwj/Wav2Lip` |
| **SadTalker** | 2023 | Talking-head (audio + image → video) | `OpenTalker/SadTalker` |
| **Diffusion** | 2024–2026 | Latent diffusion lip-sync | SyncDiff, HighSync, MoDiTalker |

**Why GENERATE instead of DOWNLOAD?**
| Downloaded Fakes | Generated Fakes |
|------------------|-----------------|
| Fixed generator set | You control generator versions |
| Unknown speaker overlap | **Use YOUR held-out test speakers** |
| No audio-real pairing | **Same audio → real + fake pairs** (perfect control) |
| No phoneme alignment | You run WhisperX on YOUR audio → exact M/B/P frames |

**This is your PRIMARY evaluation source for Test Condition 2.**

---

## 3. WHY TRANSCRIPTS MATTER: THE M/B/P CONNECTION <a name="why-transcripts"></a>

### The Problem: M/B/P Phonemes Are Short
| Phoneme | Typical Duration | Frames @ 25fps |
|---------|------------------|----------------|
| /m/ | 40–80ms | 1–2 frames |
| /b/ | 30–60ms | 1–2 frames |
| /p/ | 30–60ms | 1–2 frames |

**You need frame-level precision.** Off by 1 frame = labeling the wrong phoneme.

### Two Ways to Get Phoneme Timestamps

#### ❌ Option A: WhisperX ASR Mode (No Transcript)
```
Audio → WhisperX transcribes → WhisperX aligns its own transcription
```
**Failure mode on "map":**
- WhisperX hears "nap" (common confusion)
- Aligns /n/ at 1.20–1.26s (WRONG — should be /m/)
- You extract frames 1.20–1.26s, label "should be closed"
- **But /n/ doesn't require lip closure!** → Garbage label → Classifier learns noise

#### ✅ Option B: WhisperX Forced Alignment (With Transcript)
```
Audio + "THE MAP IS HERE" → WhisperX aligns KNOWN words
```
**Success on "map":**
- WhisperX knows "map" = /m/ /æ/ /p/
- Aligns /m/ at 1.20–1.26s, /p/ at 1.30–1.35s (CORRECT)
- You extract EXACT M/B/P frames → Clean labels → Classifier learns real signal

### Dataset Transcript Status
| Dataset | Transcript? | Alignment Mode | M/B/P Precision |
|---------|-------------|----------------|-----------------|
| **LRS2/LRS3** | ✅ BBC scripts | **Forced** | **High** ⭐ |
| **DeepfakeTIMIT** | ✅ VidTIMIT prompts | **Forced** | **High** |
| **VoxCeleb2** | ❌ None | ASR only | Lower (noisy) |
| **FakeAVCeleb** | ❌ Partial/none | ASR only | Lower |
| **Generated fakes** | ✅ You control audio | **Forced** | **High** ⭐ |

**→ LRS2/LRS3 + Generated Fakes = High-precision M/B/P frames for both training AND evaluation**

---

## 4. END-TO-END PIPELINE WALKTHROUGH <a name="pipeline-walkthrough"></a>

### Phase 0: Setup (One-Time)
```bash
# Install core tools
pip install torch torchvision whisperx opencv-python mediapipe sklearn

# Clone generators
git clone https://github.com/wilderwj/Wav2Lip
git clone https://github.com/OpenTalker/SadTalker
# Download model weights for each
```

---

### Phase 1: Data Acquisition (Current Step)
```
┌─────────────────────────────────────────────────────────────┐
│  DOWNLOAD & ORGANIZE                                        │
├─────────────────────────────────────────────────────────────┤
│  LRS2:     ~/data/lrs2/{train,val,test}/<speaker>/*.mp4    │
│  LRS3:     ~/data/lrs3/{train,val,test}/<speaker>/*.mp4    │
│  VoxCeleb2:~/data/voxceleb2/<speaker>/*.mp4                │
│  DeepfakeTIMIT: ~/data/deepfaketimit/<speaker>/*.mp4       │
│  FakeAVCeleb: ~/data/fakeavceleb/{real,fake}/<speaker>/    │
└─────────────────────────────────────────────────────────────┘
```

**Key organization principle:** Keep speaker IDs intact — they drive the leave-speakers-out split.

---

### Phase 2: Phoneme Alignment (WhisperX)
```
┌─────────────────────────────────────────────────────────────┐
│  FOR EACH VIDEO CLIP                                        │
├─────────────────────────────────────────────────────────────┤
│  1. Extract audio:  clip.mp4 → clip.wav                     │
│  2. Get transcript:                                         │
│       LRS2/LRS3:  from metadata (BBC script)               │
│       DeepfakeTIMIT: from VidTIMIT prompt list             │
│       VoxCeleb2/FakeAVCeleb: NONE → use ASR mode           │
│       Generated: YOU know the audio text                   │
│  3. Run WhisperX forced alignment:                          │
│       result = whisperx.align(audio, transcript, "en")     │
│  4. Extract M/B/P intervals:                                │
│       for phone in result["phonemes"]:                      │
│           if phone["text"] in ["m", "b", "p"]:             │
│               save(phone.start, phone.end, speaker, clip)  │
│  5. Output: CSV/JSON with [speaker, clip, phoneme,        │
│       start_sec, end_sec, start_frame, end_frame]          │
└─────────────────────────────────────────────────────────────┘
```

**Output example:**
```json
{
  "speaker": "mcem0",
  "clip": "sa1",
  "phoneme": "m",
  "start_sec": 1.234,
  "end_sec": 1.312,
  "start_frame": 31,
  "end_frame": 33,
  "dataset": "lrs2"
}
```

---

### Phase 3: Mouth-State Annotation (The Ground Truth)
```
┌─────────────────────────────────────────────────────────────┐
│  FOR EACH M/B/P FRAME INTERVAL                              │
├─────────────────────────────────────────────────────────────┤
│  METHOD A: MANUAL (Ground Truth Subset ~10%)               │
│  ─────────────────────────────────────────────────────────  │
│  • Randomly sample 10% of M/B/P intervals                  │
│  • Human looks at each frame: "mouth open or closed?"      │
│  • Label: 0=open, 1=closed                                 │
│  • Save as gold standard                                   │
│                                                             │
│  METHOD B: SEMI-AUTOMATED (Scale to 90%)                   │
│  ─────────────────────────────────────────────────────────  │
│  • Run MediaPipe Face Mesh on each frame                   │
│  • Compute lip distance: upper_lip_y - lower_lip_y         │
│  • Calibrate threshold on manual subset:                   │
│       threshold = median(lip_dist | manual_label=closed)   │
│  • Auto-label: lip_dist < threshold → closed (1)           │
│  • Flag outliers (near threshold) for manual review        │
│                                                             │
│  OUTPUT: [frame_path, speaker, phoneme, label, method]     │
└─────────────────────────────────────────────────────────────┘
```

**MediaPipe lip landmarks used:**
- Upper lip: landmark 13 (center)
- Lower lip: landmark 14 (center)
- Distance = `abs(landmark_13.y - landmark_14.y)` normalized by face size

---

### Phase 4: Synthetic Generation (Test Condition 2 Fakes)
```
┌─────────────────────────────────────────────────────────────┐
│  FOR EACH HELD-OUT TEST SPEAKER                             │
├─────────────────────────────────────────────────────────────┤
│  1. Select real video + audio from test speaker            │
│  2. Run Wav2Lip:                                           │
│       python inference.py --face real.mp4 --audio real.wav │
│       --outfile fake_wav2lip.mp4                           │
│  3. Run SadTalker:                                         │
│       python inference.py --source_image real_frame.jpg    │
│       --driven_audio real.wav --outfile fake_sadtalker.mp4 │
│  4. Run Diffusion (if available):                          │
│       python generate.py --audio real.wav --speaker_id X   │
│  5. For EACH generated fake:                               │
│       • Run WhisperX forced alignment (same audio!)        │
│       • Extract M/B/P frames                               │
│       • Label: generator=wav2lip/sadtalker/diffusion       │
│       • Speaker=held-out test speaker                      │
└─────────────────────────────────────────────────────────────┘
```

**Critical:** Same audio → real and fake have IDENTICAL phoneme timing. Perfect controlled comparison.

---

### Phase 5: Leave-Speakers-Out Splits
```
┌─────────────────────────────────────────────────────────────┐
│  SPEAKER-LEVEL PARTITION (not clip-level!)                 │
├─────────────────────────────────────────────────────────────┤
│  All unique speaker IDs from: LRS2, LRS3, VoxCeleb2,       │
│  DeepfakeTIMIT, FakeAVCeleb, Generated                     │
│                                                             │
│  Shuffle speakers → Split:                                 │
│  ┌─────────────┬─────────────┬─────────────┐               │
│  │   TRAIN     │   VAL       │   TEST      │               │
│  │   ~70%      │   ~15%      │   ~15%      │               │
│  │  speakers   │  speakers   │  speakers   │               │
│  └─────────────┴─────────────┴─────────────┘               │
│                                                             │
│  CONSTRAINT: Zero speaker ID overlap between sets          │
│                                                             │
│  Test set further split:                                   │
│  ┌─────────────────────────┬─────────────────────────┐     │
│  │ Test Condition 1        │ Test Condition 2        │     │
│  │ (Era-matched baseline)  │ (Modern generators)     │     │
│  │ Real test + old fakes   │ Real test + Wav2Lip/    │     │
│  │ (if available)          │ SadTalker/Diffusion     │     │
│  └─────────────────────────┴─────────────────────────┘     │
└─────────────────────────────────────────────────────────────┘
```

**Why speaker-level?** Clip-level random split leaks speaker identity → inflated accuracy. Speaker-level tests true generalization.

---

### Phase 6: Classifier Training (PyTorch)
```
┌─────────────────────────────────────────────────────────────┐
│  MODEL: Mouth Open/Closed CNN                               │
├─────────────────────────────────────────────────────────────┤
│  Input: 96×96 (or 128×128) mouth crop, grayscale or RGB   │
│                                                             │
│  Architecture:                                             │
│  Conv2D(3→32, 3×3) → BN → ReLU → MaxPool(2)  → 48×48       │
│  Conv2D(32→64, 3×3) → BN → ReLU → MaxPool(2)  → 24×24      │
│  Conv2D(64→128, 3×3) → BN → ReLU → MaxPool(2)  → 12×12     │
│  Flatten → Linear(128×12×12, 256) → ReLU → Dropout(0.5)    │
│  Linear(256, 2) → Softmax                                  │
│                                                             │
│  Training:                                                 │
│  • DataLoader: balanced batches (open/closed)              │
│  • Loss: CrossEntropyLoss()                                │
│  • Optimizer: Adam(lr=1e-3)                                │
│  • Epochs: 20–50 with early stopping (patience=5)          │
│  • Monitor: Val accuracy per epoch                         │
│  • CRITICAL: Train FROM SCRATCH (no pretrained weights)    │
└─────────────────────────────────────────────────────────────┘
```

**Data flow:**
```
Training frames (from Phase 3, train speakers only)
        │
        ▼
┌───────────────────┐
│ Face Detect +     │  →  96×96 mouth crop
│ Mouth Crop        │
└───────────────────┘
        │
        ▼
┌───────────────────┐
│ Augmentation:     │
│  • Random crop    │
│  • Horizontal flip│
│  • Brightness/    │
│    contrast jitter│
└───────────────────┘
        │
        ▼
   CNN Forward → Loss → Backprop
```

---

### Phase 7: Evaluation
```
┌─────────────────────────────────────────────────────────────┐
│  FOR EACH TEST CONDITION                                    │
├─────────────────────────────────────────────────────────────┤
│  1. Load best model (highest val accuracy)                 │
│  2. For each test clip (real + fake):                      │
│       • Detect face, crop mouth per frame                  │
│       • Run CNN on each M/B/P frame                        │
│       • Aggregate: clip_score = mean(frame_probs)          │
│  3. Compute metrics:                                       │
│       • Accuracy, Precision, Recall, F1, AUC               │
│       • Per-generator breakdown (Condition 2)              │
│       • Per-speaker breakdown                              │
│  4. Compare: Condition 1 vs Condition 2                    │
│                                                             │
│  EXPECTED OUTCOME:                                         │
│  • Condition 1: High accuracy (sanity — original method   │
│    works on old fakes)                                     │
│  • Condition 2: Lower accuracy? (modern generators better │
│    at M/B/P closure) → THE KEY RESULT                      │
└─────────────────────────────────────────────────────────────┘
```

---

## 5. TEST CONDITIONS: WHAT YOU'RE ACTUALLY EVALUATING <a name="test-conditions"></a>

### Test Condition 1: Era-Matched Sanity Check
| Component | Source |
|-----------|--------|
| **Real videos** | Held-out test speakers from LRS3/VoxCeleb2 |
| **Fake videos** | 2019–2020 generators (if reproducible) OR DeepfakeTIMIT face-swaps |
| **Purpose** | Verify your retrained classifier matches original paper's accuracy on old fakes |
| **Expected** | High detection accuracy (~90%+ from original paper) |

### Test Condition 2: Modern Generator Evaluation (CORE RESULT)
| Component | Source |
|-----------|--------|
| **Real videos** | Same held-out test speakers (LRS3/VoxCeleb2/FakeAVCeleb real) |
| **Fake videos** | **Generated by YOU:** Wav2Lip + SadTalker + Diffusion on test speakers' audio |
| **Purpose** | **Does the M/B/P cue still work against 2024–2026 generators?** |
| **Expected** | Unknown — this is the research question! |

### Why Two Conditions?
| If Condition 1 fails | → Your implementation has a bug (original method should work) |
|----------------------|---------------------------------------------------------------|
| If Condition 1 passes, Condition 2 fails | → **Modern generators improved M/B/P closure** (key finding) |
| If both pass | → M/B/P cue is robust even against modern generators |
| If Condition 1 passes, Condition 2 passes but lower | → Cue degrades but still usable; quantify the drop |

---

## 6. CURRENT STATE & NEXT STEPS <a name="current-state"></a>

### What's Done ✅
- [x] Novelty verified (20 papers, gap confirmed)
- [x] Dataset pipeline designed (6 phases documented)
- [x] DeepfakeTIMIT downloaded + audio extracted
- [x] WhisperX installation confirmed
- [x] Generator repos identified (Wav2Lip, SadTalker, diffusion)

### What's Pending 🔄
| Step | Blockers | Effort |
|------|----------|--------|
| **Download LRS2/LRS3** | Registration + agreement + 200GB download | 1–2 days |
| **Download VoxCeleb2** | Research agreement | 1 day |
| **Download FakeAVCeleb** | Agreement via GitHub | 1 day |
| **Run WhisperX on samples** | Need data first | 2–3 hours |
| **Manual annotation (10%)** | Need aligned frames first | 4–8 hours |
| **Calibrate semi-auto** | Need manual subset | 1 hour |
| **Train classifier** | Need labeled data | 2–4 hours (GPU) |
| **Generate synthetic fakes** | Need test speakers + generators setup | 4–8 hours |
| **Final evaluation** | All above | 2–3 hours |

### Recommended Order
```
1. Register for LRS2/LRS3 + VoxCeleb2 + FakeAVCeleb (parallel)
2. Download DeepfakeTIMIT (already done — start here for smoke test)
3. Run Phases 2–3 on DeepfakeTIMIT (end-to-end debug)
4. When LRS2/LRS3 arrive: run Phases 2–3 at scale
5. Phase 4: Generate synthetic fakes from held-out speakers
6. Phase 5: Create leave-speakers-out splits
7. Phase 6: Train classifier
8. Phase 7: Evaluate Condition 1 + Condition 2
9. Write results
```

---

## 🎯 KEY INSIGHTS TO REMEMBER

1. **Transcripts → Forced Alignment → Precise M/B/P frames → Clean labels → Working classifier**
   - LRS2/LRS3 give you this for FREE (BBC scripts)
   - VoxCeleb2 doesn't → noisier labels → use as supplement only

2. **Speaker-level splits are non-negotiable**
   - Clip-level splits = data leakage = fake results
   - Every dataset contributes speaker IDs to the global split

3. **Generated fakes > Downloaded fakes for evaluation**
   - Control: same audio, same speakers, known generator, perfect phoneme alignment
   - FakeAVCeleb is backup/bonus, not primary

4. **The classifier is SIMPLE by design**
   - 3-layer CNN, binary open/closed, trained from scratch
   - Interpretability > complexity (forensic tool requirement)

5. **The result is a single comparison table**
   | Condition | Accuracy | Precision | Recall | AUC |
   |-----------|----------|-----------|--------|-----|
   | Test Cond 1 (Old fakes) | 92% | 91% | 93% | 0.96 |
   | Test Cond 2 (Modern) | **??%** | **??%** | **??%** | **??** |
   - That **??%** is your contribution

---

## 📁 FILE LOCATIONS IN YOUR REPO

```
D:\capstone\
├── DATASET_PIPELINE_EXPLANATION.md    ← THIS FILE
├── DATA\
│   ├── 01_Project_Summary\
│   │   └── PROJECT_ABSTRACT_PROBLEM.md
│   ├── 02_Research\
│   │   └── FINAL_REPORT.md
│   ├── 03_Pipeline\
│   │   ├── DATASET_PIPELINE.md        ← Original phase docs
│   │   ├── PHASE_3_EXEC_PREP.md
│   │   └── TOMORROW_PLAN.md
│   ├── 04_Results\
│   │   ├── FINAL_STATE.md
│   │   └── AUTHORIZATION_LOG.md
│   └── 05_References\
│       ├── MEMORY.md
│       └── lip-sync-deepfake-novelty-verified.md
├── dataset\
│   └── DeepfakeTIMIT\                 ← Only data downloaded so far
│       └── DeepfakeTIMIT\
│           └── lower_quality\
│               └── <speaker>\*.wav
└── research_context\                  ← Research logs
    └── deep_research_2026-10-06\
```

---

*Generated for capstone project — explains dataset roles and pipeline mechanics in detail.*