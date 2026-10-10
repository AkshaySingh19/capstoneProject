=== CONTEXT NOTES — Deep Research 2026-10-06 ===
Topic: Lip-Sync Deepfakes Using Mouth-Closure Phoneme Analysis — Novelty Verification & Dataset Identification
Domain: AI/ML, Deepfake Forensics
Top Priority: Novelty checking (do NOT assume idea is novel)
Secondary: Dataset identification

=== PROPOSED IDEA (from user's abstract / PDF status file) ===
- Retrain automatic mouth open/closed classifier on diverse multi-speaker dataset.
- Evaluate detection accuracy against modern (2024-2026) deepfake generation methods.
- Core detection cue: M, B, P phonemes require full lip closure; fake videos often fail to reproduce this (phoneme-to-viseme mismatch).
- Original method was single-speaker (Barack Obama).

=== CLARIFYING QUESTIONS (asked before start) ===
1. Confirm proposed idea matches abstract — ASSUMED YES based on user message.
2. Novelty threshold: any paper covering both multi-speaker retraining + modern generator evaluation = non-novel; partial overlap = gap remains.
3. Source scope: arXiv + peer-reviewed (IEEE/ACM/Google Scholar) included.
4. Context folder: /d/capstone/research_context/deep_research_2026-10-06/
5. Priority order: Novelty > Dataset.

=== RESEARCH LOG (step-by-step) ===

Step 1 (2026-10-06): Read existing context files.
- /d/capstone/context/deep_research_2026-10-05/01_readme_and_pdf_status.md (PDF extraction failed, image-based)
- /d/capstone/context/deep_research_2026-10-05/02_phase1_scope.md (read if present)
- /d/capstone/context/deep_research_2026-10-05/03_search_attempts.md
- /d/capstone/Capstone_Paper_Draft.docx (exists, needs extraction)

Step 2 (2026-10-06): Launch parallel web searches (5 tracks as per task).
Track 1: Original phoneme-viseme / mouth-closure detection papers (Obama single-speaker method, citations, follow-ups).
Track 2: Multi-speaker lip-sync / viseme classification studies (retrain across diverse speakers).
Track 3: Modern deepfake generation 2024-2026 (Wav2Lip variants, diffusion-based Diff2Lip, talking-head SadTalker/EAMM, commercial) — specifically M/B/P closure reproduction.
Track 4: Multi-speaker deepfake datasets (FaceForensics++, DFDC, DeepfakeTIMIT, LipSync datasets, Celeb-DF) — annotation types.
Track 5: Novelty gap studies — any paper combining multi-speaker retraining + modern generator evaluation for phoneme-based detection.

=== SEARCH STATUS ===
Will record each search URL, result count, and key claims.
=== END HEADER ===
=== TRACK 1 NOTES — Original phoneme-viseme / mouth-closure detection ===

=== MANUAL RESEARCH CONTINUED (post-rate-limit, 2026-10-06 01:30) ===
Source preference: Open access only (per user clarification).
Approach: Manual fetch via r.jina proxy + arXiv direct + Google Scholar metadata.
Rate limits prevent 50-paper automated sweep. Proceeding with targeted manual collection (~20 papers) and explicit documentation of gaps.

PAPER 3: PIA (Datta, Ranga, Sun, Lyu 2025) — arXiv:2510.14241
- Claims: phoneme-temporal + identity-dynamic framework for deepfake detection.
- Relevance: Mentions phoneme-viseme alignment but does NOT retrain a mouth-closure classifier on multi-speaker data; does NOT evaluate against Wav2Lip/Diff2Lip/SadTalker by name.
- Adversarial verification needed: Check if PIA paper's evaluation section mentions speaker diversity or specific generator versions. (NOT YET VERIFIED — need abstract/full-text access.)

PAPER 4: Original Agarwal/Farid 2020 (title: "Detecting deep-fake videos from phoneme-viseme mismatches")
- From earlier Google Scholar extraction: single-speaker (Obama), M/B/P closure cue.
- Need to verify whether any follow-up by same authors generalized to multi-speaker.
- Status: PARTIALLY DOCUMENTED (need direct paper fetch).

PAPER 5: Bohacek/Farid 2024 ("Lost in translation: Lip-sync deepfake detection from audio-video mismatch")
- Mentioned in Google Scholar extraction but URL fetch failed.
- Need to confirm: does it retrain on multi-speaker? Does it test modern generators?
- Status: PENDING MANUAL FETCH.

ACTION: Continue manual fetches; write structured notes to /d/capstone/research_context/deep_research_2026-10-06/search_angle_*.md; begin adversarial verification log once 3 sources per claim are identified.
