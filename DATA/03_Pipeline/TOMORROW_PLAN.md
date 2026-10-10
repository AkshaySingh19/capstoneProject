=== TOMORROW WORK PLAN (Next session start point) ===

STEP 1: READ STATE FILES (5 minutes)
- Read DATASET_PIPELINE.md (all 6 phases)
- Read FINAL_STATE.md (full session status)
- Read AUTHORIZATION_LOG.md (A + B confirmed)

STEP 2: DATASET ACQUISITION (A — user action required)
If user submits agreements / downloads data:
- LRS2/LRS3: https://www.robots.ox.ac.uk/~vgg/data/lip_reading/ (agreement + download)
- VoxCeleb2: https://www.robots.ox.ac.uk/~vgg/data/voxceleb/ (agreement + download)
- DeepfakeTIMIT: https://github.com/albahnasawi/DeepfakeTIMIT (open — download immediately)
- FakeAVCeleb: https://github.com/hasanirtaza/FakeAVCeleb (agreement + download)
Note: DeepfakeTIMIT can be downloaded immediately (no agreement). Others require agreement submission.
After download: extract audio (.wav) and video (.mp4) into /data/train/, /data/val/, /data/test/real/

STEP 3: REPO CLONE (B — manual/network-independent)
If user runs locally or network available:
- git clone https://github.com/wilderwj/Wav2Lip.git
- git clone https://github.com/OpenTalker/SadTalker.git
- Install dependencies (pip install -r requirements.txt for each)
- Download model weights per repo README

STEP 4: PHASE 3 ANNOTATION (requires dataset present)
Once dataset downloaded:
- Run WhisperX alignment on audio files (force-alignment for LRS2/LRS3; ASR + alignment for VoxCeleb2)
- Filter phoneme intervals for /m/, /b/, /p/
- Build manual ground-truth subset (~10% of M/B/P frames) — label open/closed
- Calibrate semi-automated threshold using MediaPipe/dlib lip distance metric
- Apply to remaining frames; review outliers manually
- Save annotations to /data/annotations/<speaker_id>/

STEP 5: PHASE 5 SPLITS (after annotations complete)
- Identify speaker IDs from metadata
- Create leave-speakers-out split: 70% train / 15% val / 15% test
- Ensure zero overlap; verify with script; log result
- Organize files: /data/train/<id>/, /data/val/<id>/, /data/test/real/<id>/, /data/test/synthetic/<generator>/<id>/

STEP 6: PHASE 6 IMPLEMENTATION (after splits + annotations)
- Implement CNN (DATASET_PIPELINE.md Phase 6 architecture)
- Train from scratch (not fine-tuned) on multi-speaker annotations
- Monitor validation accuracy; early stopping; save best weights
- Generate synthetic fakes (Phase 4) for Test Condition 2
- Evaluate on Test Condition 1 (sanity check) + Test Condition 2 (modern) — compute Accuracy, Precision, Recall, AUC
- Report results comparing retrained model vs original study figures

STEP 7: SAVE RESULTS
- Log all predictions, metrics, split verification results
- Update FINAL_REPORT.md with empirical results (currently has verified gap + dataset recommendations; empirical evaluation closes the gap)
- Update memory file with results

BLOCKERS (must resolve before execution advances):
1. Dataset download agreements submitted (A)
2. Dataset extracted and organized (Phase 1 complete)
3. Repo clone completed or repo access available (B)
4. Phase 3 annotations built (requires 1 + 3)

READY STATE CONFIRMED: All documentation complete; authorization A + B confirmed; TOMORROW_PLAN.md saved.
