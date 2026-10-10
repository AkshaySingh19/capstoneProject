=== DATASET PIPELINE — PHASE 1 EXECUTION ===
Started: 2026-10-06 (after cross-check complete)
Reference: DATASET_PIPELINE.md (phases defined); FINAL_REPORT.md Section 3 (dataset recommendations)
Phase: 1 — Data Acquisition (LRS2/LRS3, VoxCeleb2, DeepfakeTIMIT)
Phase: 2 — Phoneme Alignment (WhisperX setup)
Phase: 3 — Mouth-State Annotation (manual + semi-automated)
Phase: 4 — Synthetic Generation (Wav2Lip, SadTalker, diffusion)
Phase: 5 — Dataset Splits (leave-speakers-out)
Phase: 6 — Implementation (PyTorch + open-source tools)

===============================================================================
PHASE 1A: LRS2 / LRS3 ACQUISITION
===============================================================================
Source: University of Oxford / BBC — Lip Reading Sentences
URL: https://www.robots.ox.ac.uk/~vgg/data/lip_reading/lrs2.html (LRS2); https://www.robots.ox.ac.uk/~vgg/data/lip_reading/lrs3.html (LRS3)
Access: Public for research (requires registration/download agreement)
Content: Thousands of sentences from diverse BBC speakers; audio + video + transcript
Steps:
1. Visit LRS2 / LRS3 download pages; register and submit download agreement.
2. Download train/val/test splits (typically tar.gz, ~100-200GB each).
3. Extract audio (.wav or .m4a) and video frames (.mp4) for each clip.
4. Note speaker IDs (available in metadata) for leave-speakers-out split.
Alternative: Check Hugging Face datasets (hf.co/datasets) for 'lrs2' or 'lrs3' mirrors.
Note: No pre-existing mouth-state labels; will require custom annotation (Phase 3).

===============================================================================
PHASE 1B: VOXCELEB2 ACQUISITION
===============================================================================
Source: University of Oxford — VoxCeleb (VoxCeleb2 updated version)
URL: https://www.robots.ox.ac.uk/~vgg/data/voxceleb/ (official site)
Access: Public with research agreement (free for academic/research use)
Content: 6,000+ celebrity videos; diverse demographics; speaker identity labels; no transcripts by default
Steps:
1. Visit VoxCeleb site; download VoxCeleb2 dataset (videos + speaker IDs).
2. Filter for videos with clear frontal face and visible mouth region.
3. Note speaker identity for split; aim for diverse gender/age/ethnicity representation.
Note: Requires custom phoneme extraction (WhisperX ASR for audio without transcript) and custom mouth-state labeling.

===============================================================================
PHASE 1C: DEEPFAKETIMIT ACQUISITION (UPDATED — FOUND)
===============================================================================
Source: DeepfakeTIMIT (Korshunov & Marcel) — Zenodo open dataset
URL (CORRECTED — verified 2026-10-06): https://zenodo.org/records/4068245
Access: Open (public Zenodo record — no agreement required)
Content: 620 swapped-face videos (lower + higher quality) with untouched audio; from VidTIMIT pairs; archive ~226.6 MB (DeepfakeTIMIT.tar.gz + MD5SUM.txt)
Previous incorrect URL (GitHub): https://github.com/albahnasawi/DeepfakeTIMIT — NOT FOUND (repo not present / moved)
Steps:
1. Download from Zenodo: https://zenodo.org/records/4068245 → DeepfakeTIMIT.tar.gz (226.6 MB)
2. Verify with MD5SUM.txt (included in download).
3. Extract archive; note 620 videos (lower + higher quality pairs).
4. Note: Videos are face-swapped (fake) + real audio; useful for structured small-scale baseline and synthetic comparison.
5. Add custom phoneme and mouth-state annotations (Phase 2/3 — WhisperX alignment + manual labeling for M/B/P intervals).

===============================================================================
PHASE 1D: FAKEAVCELEB ACQUISITION (MODERN LIP-SYNC FAKES)
===============================================================================
Source: FakeAVCeleb dataset (NeurIPS 2021, arXiv:2108.05080)
URL: https://github.com/hasanirtaza/FakeAVCeleb (official repo)
Access: Public with agreement (free for research)
Content: ~11,000 videos; 4 ethnic backgrounds, balanced gender; real + lip-sync fakes + audio fakes; includes modern lip-sync generators
Steps:
1. Download from GitHub repo or request access via official site.
2. Use real videos + lip-sync fake videos for evaluation (Test Condition 2).
3. Note: Includes modern lip-sync generators; useful for modern generator evaluation.
Note: FakeAVCeleb provides BOTH real multi-speaker videos AND modern lip-sync fakes — ideal for Test Condition 2.

===============================================================================
PHASE 1D: SYNTHETIC GENERATION SETUP (Pre-prepare for Phase 4)
===============================================================================
4A — Wav2Lip (Open-source)
Repo: https://github.com/Rudrabha/Wav2Lip (or https://github.com/wilderwj/Wav2Lip)
Setup: Clone repo; install dependencies (PyTorch, OpenCV, etc.); download pre-trained model weights (if available); test on sample video.
Usage: Input = audio (.wav) + face video; output = lip-sync video.

4B — SadTalker (Open-source)
Repo: https://github.com/OpenTalker/SadTalker
Setup: Clone repo; install dependencies; download model weights; test on sample image/video + audio.
Usage: Input = audio + single face/video; output = talking-head video.

4C — Diffusion-based / Modern Repositories
Search terms: "diffusion talking face github", "audio-driven diffusion face github", "lip-sync diffusion model open source"
Examples from literature: SyncDiff (arXiv:2501.11927 reference / WACV 2025), HighSync (arXiv:2606.22772 / 2026 repository may not be public yet)
Note: Availability variable; use Wav2Lip + SadTalker as primary synthetic generation; add diffusion repos if found.

===============================================================================
PHASE 2: PHONEME ALIGNMENT (WHISPERX)
===============================================================================
Tool: WhisperX (https://github.com/m-bain/whisperX)
Purpose: Forced phoneme alignment (frame-level start/end for M, B, P phonemes)
Setup steps:
1. Install WhisperX: pip install whisperx
2. Download Whisper model (base/medium/large — medium recommended for accuracy).
3. Prepare audio files (.wav) from video clips.
4. If transcript available (LRS2/LRS3): use forced alignment mode.
5. If no transcript (VoxCeleb2): use ASR + alignment mode.
6. Output: Phoneme intervals with time codes; filter for /m/, /b/, /p/.

===============================================================================
PHASE 3: MOUTH-STATE ANNOTATION (CUSTOM)
===============================================================================
Purpose: Label each M/B/P frame as open (fake mismatch indicator) or closed (real speech expected).
Method A — Manual (ground truth subset):
- Randomly select ~10% of real M/B/P frames; manually label open/closed by visual inspection.
- Create ground-truth reference set for threshold calibration.

Method B — Semi-automated (scale):
- Use face/mouth landmark detection (MediaPipe / dlib / OpenCV) to compute lip-opening metric (distance between upper/lower lip landmarks).
- Calibrate threshold on manual subset; apply to remaining frames automatically.
- Review outliers (frames with ambiguous opening) manually.

===============================================================================
PHASE 4: SYNTHETIC GENERATION (EXPANDED — Wav2Lip / SadTalker / Diffusion)
===============================================================================
Purpose: Generate synthetic fake videos using modern/open generators for Test Condition 2 evaluation.

4A — Wav2Lip (Open-source, audio-driven lip-sync)
Repo: https://github.com/wilderwj/Wav2Lip (or https://github.com/Rudrabha/Wav2Lip)
Command pattern:
  python inference.py --checkpoint_path checkpoints/wav2lip_gan.pth \
    --face "input_video.mp4" --audio "audio.wav" --outfile "output_wav2lip.mp4"
Notes: Requires face detection + crop; pre-trained model available; good for M/B/P phoneme frame generation.

4B — SadTalker (Open-source, talking-head synthesis)
Repo: https://github.com/OpenTalker/SadTalker
Setup: Clone repo; install requirements; download model weights.
Usage: Input = audio (.wav) + source face image/video; output = animated face video.
Notes: Produces full-face animation; useful for generating diverse speaker synthetic samples.

4C — Diffusion / Modern (Variable availability)
Search terms for repos: "diffusion talking face github", "lip-sync diffusion open source"
Examples from literature (repos may not be public yet):
  - SyncDiff (arXiv:2501.11927 / WACV 2025) — diffusion with temporal visual prior
  - HighSync (arXiv:2606.22772 / 2026) — latent diffusion for high-quality lip sync
  - MoDiTalker (arXiv:2505.22581 / 2025) — motion-disentangled diffusion
Note: If repos not available, rely on Wav2Lip + SadTalker as primary synthetic sources.

4D — Synthetic Labeling
For each generated video:
  - Label generator type (Wav2Lip / SadTalker / diffusion / commercial)
  - Note speaker identity (held-out from training set)
  - Confirm M/B/P phoneme intervals via WhisperX alignment (same as real videos)

===============================================================================
PHASE 5: DATASET SPLITS (LEAVE-SPEAKERS-OUT) — EXPANDED
===============================================================================
Purpose: Ensure no speaker identity overlap between train/val/test; evaluate cross-speaker generalization.
Split protocol:
1. Identify all unique speaker IDs from dataset metadata (LRS2/LRS3 transcripts; VoxCeleb2 speaker labels; DeepfakeTIMIT subject IDs).
2. Randomly partition speakers (not individual clips) into 3 disjoint sets:
   - Training (Set A): ~70% of speakers (diverse demographics: gender, age, ethnicity).
   - Validation (Set B): ~15% of speakers (held out; used only for hyperparameter tuning / threshold calibration).
   - Test (Set C): ~15% of speakers (held out; used ONLY for final evaluation — never for training or validation).
3. For each split:
   - Include ALL clips (real + synthetic) from speakers in that split.
   - Ensure no speaker ID appears in more than one split.
4. Sub-split Test (Set C) for two conditions:
   - Test Condition 1 (Era-matched / sanity check): Real clips from Set C + available 2019-2020 synthetic baseline (if reproducible; skip if unavailable).
   - Test Condition 2 (Modern — core): Real clips from Set C + synthetic fakes generated with Wav2Lip + SadTalker (+ diffusion if available) from Set C audio/video.
5. File organization (recommended):
   /data/train/<speaker_id>/  (real clips + annotations)
   /data/val/<speaker_id>/
   /data/test/real/<speaker_id>/
   /data/test/synthetic/<generator_name>/<speaker_id>/
6. Verification step:
   - After split creation, run a script to confirm zero speaker ID overlap across train/val/test directories.
   - Log overlap check result (pass/fail) in split log file.

Note: This is a leave-speakers-out cross-validation design (not clip-level random split) — critical for testing multi-speaker generalization per the project abstract.

===============================================================================
PHASE 6: IMPLEMENTATION (PYTORCH) — EXPANDED
===============================================================================
Classifier architecture (retrained from scratch, NOT fine-tuned):
- Input: Cropped mouth region image (e.g., 96x96 or 128x128 pixels, grayscale or RGB, from face detection crop).
- CNN Backbone (example structure, same family as original):
  Conv2D(3/1, 32, kernel=3, padding=1) → BatchNorm → ReLU → MaxPool(2)
  Conv2D(32, 64, kernel=3, padding=1) → BatchNorm → ReLU → MaxPool(2)
  Conv2D(64, 128, kernel=3, padding=1) → BatchNorm → ReLU → MaxPool(2)
  Flatten → Linear(128 * feature_size, 256) → ReLU → Dropout(0.5)
  Linear(256, 2) → Softmax (binary: open / closed)
Note: Exact layer dimensions depend on input crop size; adjust Linear input accordingly.

Training loop (PyTorch):
- DataLoader: Load annotated M/B/P frames (manual subset + semi-automated labels).
- Loss: torch.nn.CrossEntropyLoss() (binary classification with 2 classes: open=0, closed=1 or vice versa — consistent mapping required).
- Optimizer: Adam with learning_rate=1e-3 (or SGD with momentum; tune on validation set).
- Epochs: Train for fixed epochs (e.g., 20-50); monitor validation accuracy per epoch.
- Early stopping: If validation accuracy does not improve for N consecutive epochs (e.g., 5), stop and restore best weights.
- Key constraint: Retrain from scratch — do NOT initialize from pre-trained single-speaker model weights.

Evaluation script outline:
1. Load trained model weights (best validation epoch).
2. Load test dataset (Test Condition 1 + Test Condition 2 separately).
3. For each M/B/P frame in test clips: run model prediction (open/closed).
4. Compute metrics per condition:
   - Accuracy = (TP + TN) / total
   - Precision = TP / (TP + FP)  [for "correctly closed" class, or as defined]
   - Recall = TP / (TP + FN)
   - AUC = sklearn.metrics.roc_auc_score(y_true, y_pred_prob)
5. Report results table comparing:
   - Original study figures (if available) vs. this retrained model on Condition 1 (sanity check)
   - Retrained model performance on Condition 2 (modern generators) — core result.
6. Log all predictions, ground-truth labels, and metric outputs to file for reproducibility.

Tools / dependencies:
- PyTorch (CNN implementation)
- torchvision (data transforms, loading)
- OpenCV / MediaPipe (mouth crop from video)
- sklearn (metrics: accuracy_score, precision_score, recall_score, roc_auc_score)
- WhisperX (phoneme alignment — Phase 2)

Note: Implementation execution requires dataset download (Phase 1 authorization) + Phase 3 annotations complete.

===============================================================================
EXPECTED NEXT ACTIONS AFTER PHASE 1 SETUP
===============================================================================
After dataset acquisition and WhisperX installation:
1. Run phoneme alignment on sample clips; verify M/B/P intervals.
2. Build manual ground-truth subset; calibrate semi-automated labeling.
3. Retrain classifier; evaluate on validation speakers.
4. Generate synthetic evaluation videos; run final evaluation.
5. Report results comparing Test Condition 1 vs. Test Condition 2.

===============================================================================
NOTES / LIMITATIONS (from FINAL_REPORT.md)
===============================================================================
- Forced phonetic alignment may have residual error; manual spot-check required.
- Synthetic generation uses open-source pipelines; commercial/proprietary samples optional if user provides access.
- Real-time video-call deepfakes and compressed social media video are future extensions.
- Custom dataset construction required — confirms practical significance but adds steps to project timeline.
- All sources open-access (no IEEE, no paywalled content relied on for claims).
