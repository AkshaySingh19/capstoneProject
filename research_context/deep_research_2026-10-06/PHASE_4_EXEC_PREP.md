=== PHASE 4 EXECUTION PREPARATION (Ready to run) ===
Prerequisites: Python 3.8+, PyTorch, OpenCV, Git

STEP 1: Clone Wav2Lip
  git clone https://github.com/wilderwj/Wav2Lip.git
  cd Wav2Lip
  # Download pre-trained checkpoint (wav2lip_gan.pth) to checkpoints/ folder
  # Check repo README for exact download link

STEP 2: Install dependencies
  pip install -r requirements.txt
  # Typically: torch, torchvision, opencv-python, librosa, etc.

STEP 3: Clone SadTalker
  git clone https://github.com/OpenTalker/SadTalker.git
  cd SadTalker
  pip install -r requirements.txt
  # Download model weights per repo instructions

STEP 4: Test generation (requires sample audio + face video from dataset — dataset must be present first)
  # After dataset acquisition (Phase 1 authorization), run inference commands from DATASET_PIPELINE.md Phase 4A/4B.

STEP 5: Synthetic labeling (per Phase 4D)
  # Label each output with: generator_type, speaker_id, file_path, M/B/P intervals (from WhisperX)

NOTE: Actual synthetic video generation requires dataset present (Phase 1 download complete + Phase 3 annotations). This file prepares repo setup only.
Status: Prep documented; execution blocked by dataset presence + authorization.
