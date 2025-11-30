# neural-audio-synthesis-experiments
Empirical study comparing RVC and Diff-SVC architectures, focusing on data engineering challenges, audio preprocessing techniques, and ethical considerations in generative AI voice cloning.

## Project Abstract
A longitudinal study of voice synthesis technologies conducted between 2022 and 2024. This project compares **Diff-SVC** (Diffusion-based) and **RVC** (Retrieval-based) architectures.

The objective was to analyze the "Black Box" nature of Generative AI by stress-testing models with diverse data sources—from raw studio recordings to synthetic vocaloid renders and proprietary file extractions. The study focuses on how **Data Quality**, **Dynamic Range**, and **Preprocessing** impact inference fidelity.

## Ethical & Privacy Notice
**Status: Technical Report & Demo.**
To strictly adhere to **GDPR** and **Copyright** principles regarding biometric data:
*   **Training Data:** The raw source recordings remain **private**.
*   **Model Weights:** The trained models (`.pth`, `.ckpt`) are **not published**.
*   **Audio Demo:** Only the "Studio Experiment" (Case A) is included below, published with the **explicit consent** of the voice provider.

---

## Technical Environment

*   **Infrastructure:** Google Colab (Cloud Computing).
*   **Hardware:** Dynamic GPU Allocation (NVIDIA Tesla T4 / P100 instances).
*   **Software Stack:** 
    *   **Python 3** (Runtime)
    *   **Applio / RVC WebUI** (Retrieval-based framework)
    *   **Diff-SVC** (Diffusion Probabilistic Models)
    *   **UVR5** (Ultimate Vocal Remover for source separation)
    *   **Audacity** (Signal processing: Noise Gating, Center Extraction, Manual Trimming)

---

## Experiment Log & Findings

### Case Study A: The "Clean Data" Bias & Generalization Failure (Studio Model)
*   **Architecture:** RVC
*   **Source:** Raw studio session stems provided by a volunteer (Consented).
*   **Preprocessing:** None required (High Signal-to-Noise Ratio).
*   **Observation:** The model converged quickly with "Uncanny" realism in the voice's natural register.
*   **Limitation (Generalization Failure):** The source singer had a low vocal register. When attempting to infer notes 6 semitones higher, the model failed, introducing distortion and artifacts. This proves that the latent space cannot invent frequencies absent from the training data.
*   **🎧 Audio Demonstration:**
    > **Version 1 (Native Register):** [Click to Listen: Normal Sample](samples/demo_low_register.mp3)
    > **Version 2 (Stressed +6 Semitones):** [Click to Listen: Failure Sample](samples/demo_high_fail.mp3)

### Case Study B: Synthetic Data & Feature Leakage (Miku V2 / Yamaha VSQX)
*   **Architecture:** RVC
*   **Source:** Synthetic renders generated from Yamaha VSQX files.
*   **Preprocessing:** Manual de-reverberation (removing room acoustics) to ensure a "dry" signal.
*   **Challenge:** The source data consisted of fast-paced singing.
*   **Result:** While the timbre was accurate, the model suffered from **Feature Leakage**. It "learned" the rapid temporal characteristics (stuttering) rather than just the tone, resulting in unstable inference on slower songs.

### Case Study C: Destructive Preprocessing & Manual Editing (Rosé / K-Pop)
*   **Architecture:** Diff-SVC (Early generation).
*   **Source:** Extracted vocals using UVR.
*   **Engineering:** Extensive manual cleaning in **Audacity**.
    *   **Noise Gating:** Applied to remove background static.
    *   **Center Channel Extraction:** Used to isolate vocals from instrumental bleed.
*   **Result:** Successful cloning, but with distinct "Metallic" artifacts.
*   **Conclusion:** Aggressive audio cleaning is destructive; it removes necessary frequency information, causing the diffusion model to hallucinate metallic noise to fill the gaps.

### Case Study D: Legacy Formats & Granular Data (SeeU / DDB Extraction)
*   **Architecture:** Diff-SVC
*   **Source:** Raw audio samples extracted directly from proprietary `.ddb` (Voicebank) archives.
*   **Legal/Technical Note:** This experiment involved navigating the legally complex ("gray") area of proprietary file extraction. It was conducted strictly as a technical stress-test and was not distributed.
*   **Technical Constraint:** The extracted samples were extremely short (milliseconds in duration), representing isolated phonemes rather than continuous phrasing.
*   **Result:** Functional but low fidelity.
*   **Conclusion:** The model struggled to stitch together millisecond-long fragments into a cohesive voice. This proved that **Data Continuity** is just as important as audio quality; the model needs context to learn transitions between sounds.

---

## Final Conclusion
This project demonstrated that in Applied AI, **Data Engineering is the bottleneck**.
While newer architectures (RVC) provide superior inference speed compared to older ones (Diff-SVC), they cannot compensate for poor dataset quality. The most effective optimization was not hyperparameter tuning, but ensuring the input audio was clean, dry, and contextually rich.
