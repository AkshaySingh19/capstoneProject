=== TRACK 1 — ORIGINAL PHONEME-VISEME / MOUTH-CLOSURE DETECTION ===

Reference method (well-cited, Obama single-speaker):
- Title: "Detecting Deepfake Videos by Analyzing Mouth Movements" / "Lip Forensics"
- Common citation: Agarwal, S., et al. "Detecting Deepfake Videos Using Lip Movement Analysis." (approx 2019-2020)
- Core claim: Real speech requires full lip closure for M, B, P phonemes; many synthesized videos fail this mapping.
- Validation: Single speaker (Barack Obama) videos; automatic mouth open/closed classifier trained/validated on same speaker.
- Relevance to novelty gap: The method's generalization to diverse speakers is untested; modern generator evaluation is missing.

Follow-up / citation network (approximate years 2020-2023):
- Studies extending lip-movement analysis to multiple speakers have used different classifiers (CNN, LSTM) but rarely the exact M/B/P phoneme-closure cue retrained across diverse speakers.
- Deepfake detection surveys (e.g., 2023-2024 surveys in IEEE Access / ACM Computing Surveys) mention lip-region forensics but do not report multi-speaker retraining of the exact phoneme-based closure classifier.

Adversarial verification status (3 independent checks needed):
- Check 1 (training knowledge): Confirmed — original method single-speaker.
- Check 2 (citation search): Partial — follow-ups discuss multi-speaker datasets but do not explicitly retrain M/B/P closure classifier.
- Check 3 (modern generator evaluation): Not confirmed in literature — gap likely real.
Agreement: 2/3 partial/confirmed → gap remains unverified for modern evaluation.
