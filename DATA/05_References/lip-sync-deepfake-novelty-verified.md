---
name: lip-sync-deepfake-novelty-verified
description: Deep research on phoneme-viseme mismatch detection for lip-sync deepfakes confirmed novelty gap remains open after 20 open-access papers.
metadata:
  node_type: memory
  type: project
  originSessionId: b778ffe6-9971-42ee-9d0a-b9baa883e189
  modified: 2026-10-05T21:36:21.086Z
---

NOVELTY VERIFIED: The proposed idea (retrain mouth-closure classifier on diverse multi-speaker data + evaluate against modern 2024-2026 generators for M/B/P phoneme mismatch) remains unaddressed in open-access literature.

PAPERS REVIEWED (20 total; no IEEE; open-access only): 5 full-detail (Agarwal 2020, Bohacek 2024, PIA 2025/arXiv:2510.14241, NPVForensics 2023/arXiv:2306.06885, He/Wang 2025) + 15 abstract/metadata (BioLip/arXiv:2604.16808, LIPINC-V2/arXiv:2504.01470, AV-Lip-Sync+/arXiv:2311.02733, FakeAVCeleb/arXiv:2108.05080, LIPINC/arXiv:2401.10113, Lips Are Lying/arXiv:2401.15668, ArEnAV/arXiv:2505.22581, DeepfakeTIMIT, plus others).

KEY NEW FINDING: BioLip (2026) uses biomechanical constraint modeling (not phoneme M/B/P closure classifier) with 5 unnamed modern generators and English-only training — confirms modern evaluation exists with DIFFERENT forensic cue, but gap remains.

DATASET RECOMMENDATIONS: LRS2/LRS3 (primary multi-speaker real video + WhisperX phoneme extraction), VoxCeleb2 (diversity), DeepfakeTIMIT (small structured baseline); synthetic evaluation via Wav2Lip, SadTalker, diffusion repos.

PIPELINE: DATASET_PIPELINE.md (6 phases: Acquisition, WhisperX Alignment, Annotation, Synthetic Generation, Splits, Implementation — execution documented but not executed; user selected documentation-only).

NO FABRICATION: All claims tied to fetched arXiv/Google Scholar sources; rate limits (429/403) and open-access constraints documented in FINAL_REPORT.md Appendix.

**Why:** User requested deep-research verification for capstone project; needed confirmed novelty before proceeding with dataset construction.
**How to apply:** Proceed with dataset pipeline (Phase 1: LRS2/LRS3 download, VoxCeleb2 agreement, DeepfakeTIMIT access) when ready; retrain classifier on multi-speaker M/B/P annotations; generate synthetic fakes with Wav2Lip/SadTalker; evaluate against modern generators to close the verified gap.

Related: [[memory-index]]
