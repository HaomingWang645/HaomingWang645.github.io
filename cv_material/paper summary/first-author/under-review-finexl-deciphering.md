---
title: "Deciphering Personalization: Towards Fine-Grained Explainability in Natural Language for Personalized Image Generation Models"
short_name: FineXL
venue: Under Review
year: 2025
status: under review
authorship: first author
authors: Haoming Wang, Wei Gao
links: arXiv:2511.01932 (cs.LG, 2 Nov 2025)
---

## One-line pitch
FineXL delivers fine-grained, natural-language explanations of how a personalized image generation model differs from its base model, decomposing the distributional divergence into orthogonal low-level concepts with quantitative per-aspect personalization scores.

## Problem / motivation
Over 80,000 personalized text-to-image models exist on HuggingFace alone, and users must choose among them without knowing how each was personalized. Existing natural-language explanation methods (VisDiff, GSCLIP, Diff Caption, Chg2Cap) are coarse-grained: when a model is personalized along multiple aspects (e.g., both vividity and abstractionism) or at varying levels, these methods produce vague summaries like "a modern artistic style" that cannot distinguish aspects or quantify their intensity. Visual-feature approaches are computationally identifiable but not human-interpretable.

## Approach
FineXL is training-free and works in three stages. (1) Quantify distributional divergence: a CLIP image encoder converts the difference between base and personalized model outputs over n sampled prompts into a high-level vector V_div in a shared text-image representation space. (2) Discover low-level concepts: a VLM (GPT-4o) is prompted on pairs of base/personalized images to emit single-word adjectives or short phrases, whose text-encoder embeddings are computed via a divergence formulation (Enc_text[t_i union C] minus Enc_text[t_i]). (3) Enforce orthogonality and decompose: concepts with high projection onto already-retained vectors are pruned (redundancy filter), then V_div is linearly decomposed as sum w_i * f(C_i) based on the linear representation hypothesis, with residual kept below threshold e_decomp (empirically 0.2). The coefficients w_i serve as per-aspect personalization scores. Alignment, linearity, and orthogonality of the embedding space are validated using Stable Diffusion 3.5 as an approximate G_ideal.

## Key results
- Single-aspect varying-level personalization: FineXL reduces explanation MAE by 56% vs baselines — synthetic 0.7 vs 1.6 (VisDiff), Style Transfer 1.6 vs 2.5, WikiArt 1.4 vs 2.2.
- Multi-aspect personalization (2 aspects, 3 levels each): selection accuracy 94.4% / 94.4% / 88.8% on synthetic / Style Transfer / WikiArt, vs best baseline 77.8% / 88.9% / 83.3% — at least 50% error reduction.
- Scales with granularity: at 10 personalization levels, FineXL MAE = 0.7 vs VisDiff 1.6 and Naive GPT-4o 2.2 (synthetic dataset).
- Generalizes across model families: similar gains on ControlGAN (0.9/1.4/1.4 MAE) and Anole-7B autoregressive model (1.5/0.7/1.8 MAE) without retraining.
- Subject-driven personalization (facial "mustache" via NoiseCLR editing): FineXL MAE 0.3 vs VisDiff 0.9 vs GSCLIP 0.8; coefficients track editing scales (0.04 -> 0.34 -> 0.83 -> 0.87 -> 0.90 for scales 1/3/5/7/9).
- Ablations: CLIP gives best overall encoder performance (linearity 0.79, orthogonality 0.03, alignment 0.72) over ALIGN / OpenCLIP / EVA-CLIP; GPT-4o outperforms Llama-3.2-11B-Vision and Qwen2-VL-7B as the concept-discovery VLM (93.3% vs 66.7% vs 80.0% correctness at 15 alternatives); robust to prompt template phrasing.
- Evaluated on three datasets: synthetic (400 images, 15 styles), Style Transfer (2,500 images, 50 styles), WikiArt (81,444 paintings, 129 artists).

## CV-ready bullets
**Short (≤ 15 words):** Proposed FineXL, training-free fine-grained natural-language explanations for personalized image generation models.
**Medium (≤ 30 words):** Developed FineXL, a training-free method that decomposes the embedding-space divergence between base and personalized diffusion/GAN/autoregressive models into orthogonal natural-language concepts with quantitative per-aspect scores, cutting explanation error 56%.
**Long (≤ 50 words, for research statement):** First-authored FineXL (under review), which leverages the linear representation hypothesis to decompose CLIP-space divergence between base and personalized image generators into orthogonal VLM-discovered concepts with quantitative scores; training-free, generalizes across diffusion/GAN/autoregressive models, reducing single-aspect explanation MAE 56% and multi-aspect selection error at least 50% over VisDiff, GSCLIP, and GPT-4o baselines.

## Keywords / themes
diffusion models, personalization, explainable AI, natural language explanation, image generation, CLIP, linear representation hypothesis, concept decomposition, vision-language models, GPT-4o, GANs, autoregressive image models, distributional divergence, training-free interpretability

## Notable details
- Motivated by the real problem of model selection across 80,000+ HuggingFace personalized T2I models.
- Novel technical insight: represent each low-level concept not as its raw text embedding but as a text-space divergence Enc_text[t union C] - Enc_text[t], mirroring the image-space divergence construction — this is what enables alignment between the two spaces.
- Theoretical grounding in the linear representation hypothesis (Park et al. 2023, Jiang et al. 2024) with empirical verification of alignment, linearity, and orthogonality as a dedicated experiment.
- Fully training-free and model-agnostic: validated on Stable Diffusion v2.1, ControlGAN, and Anole-7B, plus subject-driven (face) personalization via NoiseCLR editing.
- Introduces a new benchmark methodology for fine-grained explanation: simulates model selection with mixed-proportion training data coordinates and measures ranking/selection error — useful contribution beyond the method itself.
- Identifies subtle differences between foundation model versions (SD v1.4 vs v2.1 via SD v1.1 base): FineXL quantifies v2.1's greater color vibrancy (0.34 vs 0.18).
- Sample efficiency: 25–50 prompts sufficient for style-transfer data; 100–200 for WikiArt.
- Pitch angles: (1) practical tool for the HuggingFace personalization ecosystem; (2) fine-grained interpretability as a bridge between mechanistic interpretability and visual concept discovery; (3) compositional/linear structure exploited for explainability — a rare training-free concrete application of the linear representation hypothesis to generative models.
