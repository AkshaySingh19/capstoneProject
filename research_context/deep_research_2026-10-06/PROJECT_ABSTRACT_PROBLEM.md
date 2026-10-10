# Capstone Project — Abstract & Problem Statement

Source: `Capstone_Paper_Draft.docx` (extracted text)

---

## ABSTRACT

Generative video synthesis has reached a point where a person's recorded speech can be re-timed to match new audio, producing what are commonly termed **lip-sync deepfakes**. A widely cited forensic countermeasure exploits a physical constraint of human articulation: producing the phonemes **/m/, /b/, and /p/** requires the lips to close completely, a constraint many synthesis pipelines fail to reproduce with fidelity.

This project investigates whether that detection cue continues to hold under conditions the original study did not test. The automatic classifier behind the technique was trained and validated on a **single public figure**, leaving its behaviour across speakers of different age, gender, and appearance unverified, and it was evaluated only against lip-sync generators from **2019 and 2020**, a period that predates the **diffusion-based and identity-aware synthesis methods** now in common use.

This work **retrains the mouth-openness classifier on a multi-speaker corpus** and **re-evaluates its detection accuracy against generation methods released between 2024 and 2026**. The outcome will indicate whether a lightweight, interpretable phonetic cue of this kind remains dependable at scale — a question that matters for **content moderation, journalistic verification, and legal evidence review**, settings where transparent and computationally inexpensive forensic tools are preferable to opaque, resource-heavy classifiers.

---

## PROBLEM STATEMENT

The phoneme-viseme mismatch technique for lip-sync deepfake detection was trained and tested on video of a **single speaker**, so its ability to generalize to individuals of different **age, gender, and facial appearance has not been established**.

The technique was also validated only against deepfake generation methods available between **2019 and 2020**; whether the underlying detection cue — **incorrect mouth closure during bilabial phonemes** — still holds against substantially more advanced generation methods released between **2024 and 2026 is unknown**.

**No published study has retrained and re-validated this method under both conditions at once**, leaving a genuine gap in understanding whether a widely cited forensic technique remains practically useful today.

This project addresses that gap by:
1. **Retraining** the automatic mouth open/closed classifier on a diverse, multi-speaker dataset.
2. **Testing detection accuracy** against video produced by current-generation deepfake tools.
3. **Determining** whether the method's central assumption — that fake lip-sync video mismatches phoneme-to-viseme mapping — continues to hold true.

---

## KEY REFERENCES (from draft)

| Ref | Citation | Relevance |
|-----|----------|-----------|
| [1] | Agarwal, Farid, Fried, Agrawala (2020) | Original phoneme-viseme mismatch detection (single speaker: Obama) |
| [3] | Datta, Ranga, Sun, Lyu (2025) — PIA | Multimodal phoneme-temporal + identity-dynamic analysis |
| [4] | Khalid, Tariq, Kim, Woo (2021) — FakeAVCeleb | Multimodal deepfake dataset (4 ethnic backgrounds, balanced gender) |
| [9] | Fan et al. (2025) — SyncDiff | Diffusion-based talking head with temporal visual prior |
| [10] | Zhong et al. (2024) — High-fidelity landmark diffusion | High-fidelity lip-synced talking face synthesis |
| [11] | Firouzi Daghigh et al. (2026) — HighSync | High-quality lip synchronization via latent diffusion |
| [12] | Kim et al. (2025) — MoDiTalker | Motion-disentangled diffusion for talking head generation |

---

## VERIFICATION STATUS (our research)

- **Gap CONFIRMED**: After reviewing 20 open-access papers (5 core + 15 expanded), **no study covers both multi-speaker retraining + modern generator evaluation for M/B/P phoneme mismatch**.
- **Key finding**: BioLip (2026, arXiv:2604.16808) uses biomechanical constraints (not phoneme closure) with 5 unnamed modern generators, English-only training — different cue, same gap.
- **Dataset pipeline**: Ready for execution (Phase 1 URLs confirmed, Phase 2 WhisperX installed, Phase 3-6 documented).
- **Next step**: Execute Phase 3 annotation (requires dataset download/agreement for LRS2/LRS3, VoxCeleb2, DeepfakeTIMIT).

---

*Extracted from `Capstone_Paper_Draft.docx` — verified against literature (20 papers, no IEEE, open-access only).*