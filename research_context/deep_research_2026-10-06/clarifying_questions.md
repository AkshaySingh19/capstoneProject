# Clarifying Questions — Before Launching 50+ Paper Search

Topic: Lip-Sync Deepfakes Using Mouth-Closure Phoneme Analysis — Novelty Verification & Dataset Identification

Based on the previous attempt (2026-10-05) that stalled here and the user's abstract/research proposal, these questions must be answered before searching:

Q1 — NOVELTY THRESHOLD (critical): Should I treat the idea as NON-NOVEL only if the EXACT combination exists (multi-speaker retraining of mouth-closure classifier + evaluation against 2024-2026 modern generators + M/B/P phoneme cue)? Or does ANY partial overlap (e.g., multi-speaker classifier alone, modern generator evaluation alone) reduce novelty?

Q2 — SOURCE SCOPE (affects 50-paper count): Should I include ONLY peer-reviewed (IEEE/ACM/arXiv published/CVPR/ICCV/ICML/NeurIPS), or also preprints, patents, GitHub repos (Wav2Lip, Diff2Lip, SadTalker), commercial docs, and technical blogs? Confirm which categories count.

Q3 — PROPOSED IDEA CONFIRMATION: Confirm the proposed method = retrain the automatic mouth open/closed classifier (original Obama single-speaker method) on diverse multi-speaker dataset, then test detection accuracy (phoneme-to-viseme mismatch for M/B/P requiring full lip closure) against modern (2024-2026) deepfake generators. Any deviation from this description?

Q4 — DATASET / GENERATOR EXCLUSIONS: Confirm dataset targets include multi-speaker video with mouth-state/phoneme labels (LRS2/LRS3, VoxCeleb, FaceForensics++, DFDC, DeepfakeTIMIT, LRW, Celeb-DF) and synthetic generation pipelines (Wav2Lip, Diff2Lip, SadTalker, EAMM, modern diffusion talking-head). Any exclusions or additions?

Please confirm answers. Search will not launch until clarification is received per instruction: "if you have any questions ask before starting."
