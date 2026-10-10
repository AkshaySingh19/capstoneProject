=== TRACK 2 — MULTI-SPEAKER LIP-SYNC / VISEME CLASSIFICATION ===

Studies that retrain classifiers across diverse speakers:
1. LRW (Lip Reading in the Wild) dataset studies — multi-speaker word-level lip-reading (not specifically M/B/P closure). Common references: Chung et al. / "Lip Reading in the Wild" (2016, 2017).
2. LRS2 / LRS3 (BBC / Oxford) — multi-speaker sentence-level lip-reading datasets (2018-2019). Not specifically annotated for phoneme-level mouth open/closed state.
3. VoxCeleb / VoxCeleb2 — speaker identification with video; not annotated for phoneme-level mouth state.
4. DeepfakeTIMIT — multi-speaker deepfake dataset, but annotations are typically real/fake labels, not mouth-state per phoneme.
5. Celeb-DF — celebrity deepfake videos; no phoneme-level mouth-state labels.
6. LipSync datasets (e.g., LRS-LipSync, LRW-Sync) — focus on audio-visual synchronization quality, not explicit M/B/P closure classification.

Key observation: There are multi-speaker video datasets and multi-speaker classification studies, but the specific combination — retraining an automatic mouth open/closed classifier (trained for M/B/P phoneme closure) on diverse multi-speaker data — is not well-documented in the cited literature through 2024. This supports the novelty claim for the retraining component.

Dataset annotations available:
- LRW: word-level (no phoneme-level closure labels).
- LRS2/LRS3: sentence-level (no explicit mouth-state per phoneme).
- DeepfakeTIMIT / DFDC / Celeb-DF: binary real/fake (sometimes face-level); no phoneme-level annotations.
- VoxCeleb2: speaker identity + face track; no phoneme annotations.

Implication: Retraining would require either manual annotation of mouth open/closed states for M/B/P frames on a multi-speaker dataset, or synthetic generation with known audio alignment to derive labels automatically.
