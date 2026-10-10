=== PHASE 3 EXECUTION PREPARATION (Annotation) ===
Prerequisites: Dataset downloaded and organized (Phase 1 complete); WhisperX installed (Phase 2 complete — bcfs1azcf exit 0).

STEP 1: Organize dataset files
- Create directories: /data/train/<speaker_id>/, /data/val/<speaker_id>/, /data/test/real/<speaker_id>/
- Place real video clips (.mp4) + audio (.wav extracted) per speaker.
- Note: DeepfakeTIMIT videos (fake) should be labeled separately for evaluation (synthetic/real split).

STEP 2: WhisperX phoneme alignment (Phase 2 execution)
- Install/check WhisperX: pip show whisperx (installed per background task)
- For each audio file: whisperx <audio_file> --language en --align_model base/medium --align_task phoneme --model medium
- Filter outputs for phonemes: /m/, /b/, /p/ (bilabial — require full lip closure)
- Output: JSON/TSV with frame-level intervals for M/B/P phonemes.

STEP 3: Manual ground-truth labeling (10% subset)
- Randomly select ~10% of M/B/P frames from real videos.
- Visual inspection: label each frame as OPEN (lips not fully closed — potential mismatch indicator) or CLOSED (full lip closure — expected for real speech).
- Save labels: /data/annotations/manual/<speaker_id>_labels.csv

STEP 4: Semi-automated labeling (scale)
- Use MediaPipe Face Mesh or dlib facial landmarks to compute lip-opening metric (distance between upper/lower lip landmarks).
- Calibrate threshold on manual subset (find distance threshold separating OPEN vs CLOSED).
- Apply threshold to remaining M/B/P frames automatically.
- Review outliers (frames with ambiguous opening) manually; adjust labels as needed.
- Save automated + corrected labels: /data/annotations/auto/<speaker_id>_labels.csv

STEP 5: Verification
- Confirm annotation coverage: each M/B/P interval in dataset should have corresponding label.
- Check for missing labels or misaligned intervals (compare WhisperX output timestamps with video frames).
- Log any annotation errors or outliers in /data/annotations/quality_log.txt

BLOCKERS for Phase 3 execution:
- Dataset must be downloaded and organized (Phase 1 complete — URLs verified, DeepfakeTIMIT open).
- User must submit/download dataset files (manual action for agreement-required sources: LRS2/LRS3, VoxCeleb2, FakeAVCeleb).
- DeepfakeTIMIT can be downloaded immediately (https://zenodo.org/records/4068245) — no agreement needed.
- Once dataset present, Phase 3 can execute (annotation pipeline ready).
