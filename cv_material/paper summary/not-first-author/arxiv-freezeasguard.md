---
title: FreezeAsGuard: Mitigating Illegal Adaptation of Diffusion Models via Selective Tensor Freezing
short_name: FreezeAsGuard
venue: arXiv / Preprint
year: 2024
status: preprint
authorship: 2nd author (co-author)
authors: Kai Huang, Haoming Wang, Wei Gao
links: arXiv:2405.17472v2 (27 Nov 2024); code: https://github.com/pittisl/FreezeAsGuard
---

## One-line pitch
A publisher-side defense that selectively freezes a learned subset of diffusion-model tensors so downstream users cannot fine-tune the model toward illegal classes (public figures, copyrighted art, NSFW content), while legal adaptation stays largely intact.

## Problem / motivation
Open-sourced text-to-image diffusion models (e.g., Stable Diffusion) are being fine-tuned for illegal purposes such as forging public figures' portraits, duplicating copyrighted artworks, and generating explicit content. Prior defenses (copyright watermarking, prompt/image content filtering, data poisoning, unlearning, weight reinitialization) only modify data or weights and are reversible: users can relearn the removed knowledge by fine-tuning on their own clean data. There is no existing mechanism that irreversibly restricts illegal adaptation without also killing legitimate fine-tuning.

## Approach
FreezeAsGuard has the model publisher ship a binary mask that tells users which tensors to freeze during fine-tuning; because freezing lowers trainable parameters and GPU cost, non-expert users are incentivized to follow the instruction. The mask is learned via a bilevel optimization: an upper-level mask-learning loop minimizes a linear scalarization that penalizes fine-tuning loss on illegal classes and rewards it on legal classes, with a sparsity constraint enforcing a target freezing ratio rho. A continuous sigmoid relaxation m(w)=sigma(w/T) of the mask makes it differentiable, and the model is represented as m * theta_pre + (1 - m) * theta_ft so gradients flow through both pretrained and fine-tuned tensors. The lower-level simulated-user loop only runs a few fine-tuning steps per mask update (rather than to convergence) and keeps only theta(m) and theta_d = theta_pre - theta_ft in memory to cut cost. Tensor-level (not layer- or component-level) freezing is used because freezing whole components like attention projectors, conv layers, or time embeddings globally damages quality on legal classes as well.

## Key results
- On public figures (FF25 self-collected dataset, 8,703 images of 25 figures, SD v1.5): FG-30% degrades illegal-class image quality by 40% vs full fine-tuning and 37% vs UCE/IMMA unlearning baselines, while legal-class impact stays under 5%; human recognizability score on illegal prompts drops to 3.6 (vs 6.7 for full FT) while legal stays at 6.0.
- On copyrighted artworks (Artwork dataset, 1,134 images, SD v2.1): FG-70% gives 47% more mitigation in illegal classes vs full fine-tuning and ~30% more vs unlearning schemes; human score on illegal art drops to 1.7 vs 5.9 for full FT.
- On explicit content (NSFW-caption illegal / Modern-Logo-v4 legal): FG-70% reduces NSFW generation by up to 38% vs unlearning baselines while retaining logo-domain adaptability.
- Compute: up to 48% GPU memory and 21% wall-clock time saved vs full fine-tuning (freezing reduces trainable parameters).
- Scalability: mitigation holds as illegal-class count scales to 10 figures (FF25) and 3 artists (Artwork); consistently beats UCE and IMMA.

## CV-ready bullets
**Short (<= 15 words):** Co-authored FreezeAsGuard (arXiv 2024), a tensor-freezing defense against illegal diffusion-model fine-tuning.
**Medium (<= 30 words):** Co-authored FreezeAsGuard (arXiv 2024), a bilevel-optimization method that learns which diffusion-model tensors to freeze so publishers can block illegal fine-tuning (public figures, copyrighted art, NSFW) with <5% impact on legal adaptation.
**Long (<= 50 words, for research statement):** Co-authored FreezeAsGuard (arXiv 2024, 2nd of 3), which formulates illegal-adaptation defense for Stable Diffusion as bilevel mask learning over tensor freezing, yielding 37-47% stronger mitigation than UCE/IMMA unlearning baselines on portrait-forgery, copyrighted-art, and NSFW tasks while preserving legal fine-tuning and cutting 48% GPU memory / 21% wall-clock time.

## Keywords / themes
diffusion models, responsible AI, model protection, fine-tuning defense, bilevel optimization, selective tensor freezing, Stable Diffusion, unlearning, copyright protection, NSFW mitigation, portrait forgery

## Notable details
- Irreversibility is the key pitch: unlike unlearning/reinitialization, frozen tensors cannot be "retrained back" by the user because they are locked during the user's own fine-tuning.
- Incentive-compatible deployment: because freezing reduces trainable parameters, the defense aligns with users' own compute-saving interests, so they voluntarily apply the publisher-supplied mask.
- Memory trick: keeps only theta(m) and theta_d = theta_pre - theta_ft (not three full copies), enabling bilevel optimization on ~1-3B-parameter diffusion models.
- Introduces two self-collected datasets: Famous-Figures-25 (FF25, 8,703 images of 25 public figures) and Artwork (1,134 images from 5 digital artists).
- Evaluation uses domain-specific metrics (DeepFace FN-L/FN/VGG for portraits, CSD for artworks, NudeNet for NSFW) plus a 16-volunteer human study, arguing FID/CLIP alone are insufficient for this setting.
- Optimal freezing ratio is domain-dependent: 30% for portraits (subtle facial differences), 70% for artworks and NSFW (coarser stylistic differences).
- Code released at github.com/pittisl/FreezeAsGuard (Pitt IS Lab, Wei Gao's group).
