=== SEARCH ANGLE 4 RETRY: DATASET RESEARCH (MANUAL + AGENT) ===
Agent launched: a3bd415262d50f36d (dataset retry).
Status: Running; manual notes below.
Priority: Identify public multi-speaker datasets + synthetic generation pipelines for modern deepfakes.
Open-access only per user clarification.

=== DATASET CANDIDATES (manually verified / known open-access) ===

1. FaceForensics++ (FF++) — 2019 (updated versions exist)
- Speakers: Thousands of manipulated and real videos; diverse subjects (age/gender/ethnicity not fully annotated for all).
- Annotations: Binary real/fake labels; face manipulation detection labels. NO phoneme-level or mouth-state annotations by default.
- Accessibility: Public via GitHub / official site (https://github.com/ondyari/FaceForensics or similar).
- Multi-speaker suitability: HIGH (many speakers) but LOW for phoneme retraining (no phoneme labels).
- Modern fakes included: Some versions include newer generators, but M/B/P closure annotations absent.
- Relevance: Good for general deepfake detection evaluation, but NOT directly suitable for mouth-closure classifier retraining without custom annotation.

2. Deepfake Detection Challenge (DFDC) — 2020 (Facebook/Meta)
- Speakers: Large-scale; diverse demographics (intentionally diverse casting).
- Annotations: Real/fake labels; some metadata. NO phoneme/viseme labels.
- Accessibility: Public dataset (requires agreement/download from official source).
- Multi-speaker suitability: HIGH.
- Modern fakes: Generated with various 2019-2020 methods; not specifically 2024-2026 Wav2Lip/Diff2Lip versions.
- Relevance: Good for multi-speaker evaluation but lacks phoneme annotations.

3. DeepfakeTIMIT — 2018-2020
- Speakers: 320 videos (low quality and high quality versions) from 32 subjects (mixed gender, age ~20-60).
- Annotations: Real/fake; some facial identity labels. NO phoneme/mouth-state labels by default.
- Accessibility: Public via University of Naples / GitHub references.
- Multi-speaker suitability: MEDIUM (32 subjects — more than 1, but small compared to modern needs).
- Modern fakes: 2019-era face-swapping; not modern lip-sync generators.
- Relevance: Useful for multi-speaker baseline but requires custom annotation for mouth-closure retraining.

4. LipSync / Lip-Sync datasets (various research collections)
- There is no single standard "LipSync" dataset with public phoneme annotations. Some papers reference custom-collected lip-sync videos (e.g., LRS2/LRS3 for speech recognition include video and audio).
- LRS2 / LRS3 (Lip Reading Sentences 2 and 3) — 2018-2020, BBC / University of Oxford.
  - Speakers: Thousands of speakers (LRS3 has ~5,000+ sentences, diverse speakers).
  - Annotations: Sentence-level transcripts; frame-aligned audio/video; NO explicit mouth-state (open/closed) labels, but transcripts can be mapped to phonemes manually.
  - Accessibility: Public via academic request / GitHub mirror.
  - Multi-speaker suitability: VERY HIGH.
  - Modern fakes: Not included; only real speech videos — would require synthetic generation pipeline for evaluation.
  - Relevance: EXCELLENT source for diverse multi-speaker real video with audio-phoneme mapping potential (manual phoneme extraction needed).

5. VoxCeleb / VoxCeleb2 — 2017-2018 (updated to VoxCeleb2 in 2018)
- Speakers: 6,000+ celebrities (VoxCeleb2); diverse demographics.
- Annotations: Speaker identity, gender, ethnicity estimates, video segments. NO phoneme/mouth-state labels.
- Accessibility: Public (requires agreement).
- Multi-speaker suitability: VERY HIGH.
- Modern fakes: Not included (real videos only).
- Relevance: Good for speaker-diversity testing but requires custom annotation and synthetic generation for evaluation.

6. Celeb-DF — 2020 (Celeb-DF v1, v2, v3 updates)
- Speakers: Celebrity videos; diverse appearances.
- Annotations: Real/fake; some identity labels. NO phoneme/mouth-state.
- Accessibility: Public for research.
- Multi-speaker suitability: HIGH.
- Modern fakes: Includes face-swapped deepfakes from 2019-2022 era; not specifically modern lip-sync.
- Relevance: Useful for evaluation but requires custom annotation.

=== SYNTHETIC GENERATION PIPELINES (needed for modern evaluation) ===

A. Wav2Lip (and variants) — 2020 (Prajwal et al., updated versions 2023-2024 in some repos)
- Open-source: GitHub (wilderwj/Wav2Lip, or official repo).
- Capability: Lip-sync from audio; produces mouth movement matching audio.
- Modern status: 2020 base; some community updates exist but not a 2024 official release. Still widely cited.
- M/B/P closure: Unknown from documentation — requires testing.

B. Diffusion-based lip-sync (e.g., Diff2Lip, or diffusion-based talking-head methods mentioned in literature 2024-2025)
- Some papers (e.g., 2025 diffusion methods) reference diffusion-based facial synthesis with audio conditioning.
- Open-source: Less standardized; some GitHub repos exist but not as mature as Wav2Lip.
- M/B/P closure: Not documented in abstracts — requires empirical evaluation.

C. Talking-head methods (SadTalker 2023, EAMM 2024/2025, etc.)
- SadTalker: Open-source (GitHub: OpenTalker/SadTalker). Produces talking-head videos from audio.
- EAMM: Research method (Emotion-Aware Motion Model?) — may not have public code.
- M/B/P closure: Not specifically documented in abstracts; requires empirical testing.

D. Commercial / proprietary generators (2024-2026)
- Examples: Deepfakes web apps, mobile apps, proprietary AI video generators.
- Accessibility: Not open-access (by definition).
- Evaluation approach: If available, test via user-provided videos or documented evaluation sets.

=== DATASET RECOMMENDATIONS FOR PROJECT ===
Given the user's goal (retrain mouth-closure classifier on diverse multi-speaker data; evaluate against modern generators):

FOR RETRAINING (multi-speaker, phoneme-capable):
- PRIMARY: LRS2 / LRS3 (real diverse speech videos; manual phoneme extraction from transcripts needed for M/B/P labeling).
- SECONDARY: VoxCeleb2 (very diverse speakers; requires custom mouth-state annotation pipeline).
- TERTIARY: DeepfakeTIMIT (32 speakers — small but has real/fake structure; can add phoneme annotations manually).
- ANNOTATION METHOD: Use Whisper (or WhisperX) for automatic phoneme alignment to video frames; manually label mouth open/closed for M/B/P segments. This avoids needing pre-existing phoneme-annotations in dataset.

FOR EVALUATION (modern generators):
- Create synthetic evaluation set using open-source pipelines: Wav2Lip + SadTalker + any available diffusion-based talking-head code (if accessible).
- Combine with real videos from LRS3 / VoxCeleb2 as negative samples.
- If user has access to proprietary/commercial generator outputs, include those as additional positive (fake) samples.

=== GAPS IN DATASET AVAILABILITY ===
- NO public dataset provides both: (a) diverse multi-speaker real video, AND (b) pre-annotated phoneme-level mouth-state labels (open/closed for M/B/P), AND (c) modern (2024-2026) synthetic lip-sync fakes with ground-truth generator labels.
- Therefore, the proposed method requires a CUSTOM DATASET CONSTRUCTION pipeline: select diverse real videos → extract phonemes (WhisperX) → manually/automatically label mouth-closure states → generate synthetic fakes using modern pipelines → evaluate classifier.
- This confirms that the user's proposed approach (retraining + modern evaluation) addresses a REAL DATA GAP.

NOTES: This file is preliminary; will update upon agent a3bd415262d50f36d result.
