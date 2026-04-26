---
title: "Never Start from Scratch: Expediting On-Device LLM Personalization via Explainable Model Selection"
short_name: XPerT
venue: MobiSys 2025
year: 2025
status: published
authorship: first author
authors: Haoming Wang, Boyuan Yang, Xiangyu Yin, Wei Gao
acceptance_rate: 18.0%
links: https://doi.org/10.1145/3711875.3729132
---

## One-line pitch
XPerT expedites on-device LLM personalization by picking the closest already-personalized LLM (pLLM) from a cloud cache, using explainable, orthogonal-sub-vector decompositions of pLLM output distribution shifts so the device never has to download or evaluate full models.

## Problem / motivation
On-device LLM personalization is needed for privacy but is bottlenecked by scarce mobile compute (hundreds of ms per LoRA step on a Pixel 9 Pro for Qwen2-0.5B) and scarce user data (users generate only ~50-100 texts/day, far from the thousands of samples needed to avoid overfitting). Starting fine-tuning from a pre-trained base LLM wastes both. The paper argues that once many users have uploaded their personalized LLMs (pLLMs) to a trusted server, a new user should fine-tune a well-matched cached pLLM instead of starting from scratch; the open question is how to pick the right pLLM without downloading all of them or leaking personal data, which is exactly what "explainable model selection" solves.

## Approach
XPerT (eXplainable Personalized Tuning) keeps pLLM explanation computations on the server and only ships lightweight explanations to the device. For each cached pLLM the server probes both the base LLM and pLLM with the same 50 prompt samples, feeds the response pairs to a summarization LLM (Llama-3.1-8B-Instruct) with a style-difference instruction, and extracts the pre-output-layer hidden state as an embedding vector V capturing the distribution drift. V is decomposed into a linear combination of orthogonal sub-vectors v_i (one per candidate style word from the summarization LLM vocabulary), exploiting the linear-representation property of LLMs so each sub-vector cleanly maps to one human-readable language style and its coefficient z_i quantifies that style's contribution. All pLLMs share a common set of sub-vectors built sequentially (Algorithm 1), so the device only downloads the sub-vector basis plus each pLLM's coordinates, maps its local personal data into the same latent space, and picks the nearest pLLM; when no single pLLM matches, task-arithmetic-based merging of multiple pLLMs is used on-device.

## Key results
- 83% reduction in wall-clock on-device fine-tuning time when the selected pLLM matches the user's personalization need (up to 80% fewer training steps to converge).
- 51% improvement in data efficiency (up to 51% fewer training samples to reach the same testing loss).
- 96% (specifically 96.4%) model-selection accuracy across Llama-3.2-1B, Qwen2-0.5B, and SmolLM-360M on OnePlus 12R, Pixel 9 Pro, and Pixel 7.
- 96.5% lower communication cost and up to 71.4% lower computation cost for on-device model selection vs. Exhaustive Search, Bayesian Optimization, and HyperBand (e.g., Llama-3.2-1B on OnePlus 12R: t_comm drops from 1.26h to 2.7min, t_comp from 48.3min to 13.8min, power from 13.1kJ to 3.7kJ); selection cost stays constant as the number of cached pLLMs scales, while baselines grow linearly.
- BLEU/ROUGE on downstream text generation is matched or slightly improved vs. fine-tuning the base LLM from scratch even at just 30% similarity; generalizes to image generation (Stable Diffusion v2.1 fine-tuning with 9 artistic styles) at >77.8% pLLM selection accuracy; beats vanilla in-context learning on MedDialog, DISC-Law-SFT, Educhat-sft, and FinanceAlpaca.

## CV-ready bullets
**Short (<= 15 words):** First-authored XPerT (MobiSys 2025), cutting on-device LLM personalization cost 83% via explainable pLLM selection.
**Medium (<= 30 words):** Led XPerT (MobiSys 2025, 18% accept), a cloud-to-mobile pipeline that selects the best personalized LLM via orthogonal sub-vector explanations, cutting on-device fine-tuning time 83% and data needs 51% at 96% selection accuracy.
**Long (<= 50 words, for research statement):** First author of XPerT (ACM MobiSys 2025, 18% acceptance), an on-device LLM personalization system that replaces from-scratch fine-tuning with explainable selection of cached personalized LLMs; by decomposing summarization-LLM embeddings into orthogonal language-style sub-vectors, XPerT achieves 96% selection accuracy, 83% compute savings, 51% data-efficiency gain, and 96.5% lower communication than prior methods across Pixel and OnePlus smartphones.

## Keywords / themes
on-device LLM, LLM personalization, explainability, model selection, mobile systems, parameter-efficient fine-tuning, LoRA, linear representation hypothesis, task arithmetic / model merging, privacy-preserving ML, summarization LLM, cross-modality (text + diffusion image generation)

## Notable details
- Flagship ACM MobiSys venue (23rd edition, Anaheim, June 2025); 18.0% acceptance rate.
- Real smartphone deployment: Llama-3.2-1B on OnePlus 12R, Qwen2-0.5B on Google Pixel 9 Pro, SmolLM-360M on Pixel 7; measured wall-clock, energy (kJ), and power.
- First work to enable LLM-model selection for LLM personalization at the user's device with full explainability without acquiring the full models themselves (per authors).
- Cross-modality generalization: same framework applied to Stable Diffusion v2.1 U-Net fine-tuning over WikiArt, DiffusionDB, Midjourney-Detailed, TinySketch, using Qwen2-VL-7B-Instruct as summarizer and SD v3.5 for synthetic probes.
- Builds on the linear-representation-in-LLMs literature (Mikolov, Elhage, Nanda, Park et al.) in a systems-flavored application.
- Funded by NSF (IIS-2205360, CCF-2217003, CCF-2215042) and NIH (R01HL170368); all authors from University of Pittsburgh (advisor: Wei Gao).
- Strong headline numbers (83% / 51% / 96% / 96.5%) and a scalability curve that stays flat while baselines grow linearly - easy to quote in talks, SOPs, and intro cover letters.
