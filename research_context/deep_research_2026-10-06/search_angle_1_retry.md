=== SEARCH ANGLE 1 RETRY — 5 KEY PAPERS ===
Search date: 2026-10-06
Status: COMPLETED (manual / bash-based retrieval after rate limits)
RATE LIMIT NOTICE: WebSearch returned repeated 403 / authorization errors for all 5 queries. WebFetch partially worked (arXiv, r.jina.ai) but CVF PDF and Google Scholar links returned 403. Manual bash curl via r.jina.ai was used as fallback.

--- PAPER 1 ---
Title: Detecting deep-fake videos from phoneme-viseme mismatches (also cited as "Detecting Deepfake Videos Using Lip Movement Analysis" / "Lip Forensics")
Authors: S. Agarwal, H. Farid, O. Fried, M. Agrawala
Year: 2020
URL: https://ieeexplore.ieee.org/abstract/document/9151013/
Multi-speaker retraining: NO (original method validated on single speaker — Barack Obama videos)
Modern generator evaluation: NO (no evaluation against Wav2Lip / Diff2Lip / SadTalker / diffusion generators mentioned)
Notes: Core claim — real speech requires full lip closure for M, B, P phonemes; synthesized videos show mismatch. Single-speaker classifier.

--- PAPER 2 ---
Title: Lost in translation: Lip-sync deepfake detection from audio-video mismatch
Authors: M. Bohacek, H. Farid
Year: 2024
Venue: IEEE/CVF CVPR 2024
URL: https://openaccess.thecvf.com/content/CVPR2024/html/Bohacek_Lost_in_Translation_Lip-Sync_Deepfake_CVPR_2024_paper.html (CVF open-access page; PDF link blocked by 403 at fetch time)
Multi-speaker retraining: NO (not mentioned in visible abstract / metadata)
Modern generator evaluation: PARTIAL / IMPLIED (mentions evaluation on "controlled dataset" plus "in-the-wild fakes"; no explicit modern generator names listed)
Notes: Detects fakes by independently translating audio (automated lip-reading) and comparing audio-video mismatches. Rate-limit blocked full PDF retrieval.

--- PAPER 3 ---
Title: PIA: Deepfake Detection Using Phoneme-Temporal and Identity-Dynamic Analysis
Authors: Soumyya Kanti Datta, Tanvi Ranga, Chengzhe Sun, Siwei Lyu
Year: 2025
arXiv: 2510.14241
URL: https://arxiv.org/abs/2510.14241
Multi-speaker retraining: NO (explicitly: "No mention of multi-speaker retraining appears")
Modern generator evaluation: YES (abstract explicitly names GANs, diffusion models, neural rendering techniques; proposes detection against outputs from these modern generators)
Notes: Multimodal framework using phoneme sequences, lip geometry, facial identity embeddings. Code linked (GitHub). Self-supervised pre-training for downstream fine-tuning.

--- PAPER 4 ---
Title: NPVForensics: Jointing Non-critical Phonemes and Visemes for Deepfake Detection
Authors: Yu Chen, Yang Yu, Rongrong Ni, Yao Zhao, Haoliang Li
Year: 2023
arXiv: 2306.06885
URL: https://arxiv.org/abs/2306.06885
Multi-speaker retraining: NO (no multi-speaker retraining mentioned; model fine-tuned on downstream datasets but no multi-speaker closure classifier retraining)
Modern generator evaluation: NO (tests on "existing benchmarks" — unspecified which; no explicit modern generator evaluation such as Wav2Lip/Diff2Lip/SadTalker)
Notes: Proposes LFA-ST block, motion-consistency loss, phoneme-viseme awareness module, self-supervised pre-training. Addresses critical phoneme-viseme calibration challenges from advanced deepfakes but does not explicitly retrain a mouth-closure classifier across diverse speakers.

--- PAPER 5 ---
Title: Towards Highly Generalized Lip-Sync Deepfake Detection via Detailed Audio-Visual Inconsistency Analysis
Authors: Y. He, S. Wang
Year: 2025
Venue: Chinese Conference on Pattern Recognition and Computer Vision (PRCV), 2025 — Springer
URL: https://link.springer.com/chapter/10.1007/978-981-95-5740-0_36
Multi-speaker retraining: NO (model "trained only on real data with detail-aware temporal augmentations"; no multi-speaker retraining of phoneme-based classifier mentioned)
Modern generator evaluation: PARTIAL / IMPLIED (evaluated on FakeAVCeleb and DF-40 benchmarks against "unseen, highly realistic lip-sync manipulations"; modern generator names not explicitly listed but benchmark coverage implies some modern evaluation)
Notes: Joint analysis of word-level semantic consistency and phoneme-level temporal alignment. Generalization-focused; no explicit multi-speaker retraining of M/B/P closure classifier.

--- SUMMARY CHECKLIST ---
Paper 1 (Agarwal 2020):  Multi-speaker retraining = NO | Modern generator eval = NO
Paper 2 (Bohacek 2024): Multi-speaker retraining = NO | Modern generator eval = PARTIAL
Paper 3 (Datta 2025):  Multi-speaker retraining = NO | Modern generator eval = YES
Paper 4 (Chen 2023):  Multi-speaker retraining = NO | Modern generator eval = NO
Paper 5 (He/Wang 2025): Multi-speaker retraining = NO | Modern generator eval = PARTIAL

Rate limit status: WebSearch blocked (403/auth). WebFetch blocked for CVF and Google Scholar. Bash curl via r.jina.ai used successfully for arXiv. Full PDF for Bohacek (CVF) could not be retrieved due to 403.
