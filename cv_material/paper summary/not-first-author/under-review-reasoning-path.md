---
title: Reasoning Path and Latent State Analysis for Multi-view Visual Spatial Reasoning: A Cognitive Science Perspective
short_name: ReMindView-Bench
venue: Under Review
year: 2026
status: under review
authorship: 2nd author / co-author (of 6)
authors: Qiyao Xue, Haoming Wang (co-author), Weichen Liu, Shiqi Wang, Yuyang Wu, Wei Gao
links: arXiv:2512.02340; https://huggingface.co/datasets/Xue0823/ReMindView-Bench; https://github.com/pittisl/ReMindView-Bench
---

## One-line pitch
A cognitively grounded benchmark (ReMindView-Bench, >50K VQAs) and paired reasoning-path plus latent-state analysis that pinpoints where and how VLMs fail at multi-view spatial reasoning relative to human cognition.

## Problem / motivation
Current VLMs struggle with multi-view visual spatial reasoning because they rely on 2D co-occurrence cues rather than coherent 3D cross-view representations, and existing benchmarks conflate single-view perception, temporal video confounds, or view coverage with true cross-view geometric reasoning. Multi-view spatial cognition (the ability to construct, align, and maintain spatial mental models across complementary viewpoints) is diagnostically opaque: there is no framework grounded in cognitive science that decomposes where in the reasoning process VLMs break down. This paper fills that gap with a cognitively structured benchmark and a joint explicit-path / implicit-latent diagnostic protocol.

## Approach
The authors build ReMindView-Bench by procedurally generating diverse indoor scenes (Infinigen + Blender, 5 room types, sparse/dense clutter), rendering 4 canonically orthogonal views under object-centric vs view-centric spatial patterns at 10 distance levels, and instantiating 22 query templates covering view-view / view-object / object-object relations, relative direction vs distance, self-perspective vs perspective-change, and cross-frame vs non-cross-frame reasoning. Guided by Johnson-Laird's mental-model theory, they instruct VLMs to follow a 4-phase reasoning template: (1) perceptual encoding of view content, (2) frame-level spatial-relation alignment, (3) query-specific inference, (4) decision. Explicit analysis uses an LLM-as-a-judge panel scored against ground-truth view graphs from Blender metadata, plus self-consistency prompting that feeds the reasoning path back to the VLM. Implicit analysis extracts per-phase token logits, trains a 4-layer MLP linear probe against final-answer logits (cross-entropy loss), and tracks per-phase entropy dynamics to quantify calibration and uncertainty propagation.

## Key results
- Evaluated 15 VLMs (GPT-4o, Gemini-2.5-Pro, Claude-4-Sonnet, InternVL3.5-2B/8B/38B, Qwen2.5-VL-3B/7B/32B, LLaVA-Video/OneVision, SpatialVLM, SpatialMLLM, SpaceQwen2.5-VL); best model Gemini-2.5-Pro reaches only 43.1% average accuracy vs 81.5% human level, with open-source models clustering near 30-38%.
- Object-centric patterns consistently beat view-centric, and perspective-change settings drop >4% versus self-perspective across models, exposing weak allocentric / cross-view reference-frame alignment.
- Cross-frame reasoning degrades sharply relative to non-cross-frame, and accuracy falls monotonically as scene object count grows from 0-5 to 20-25, with a shallow U-shape over object-viewpoint distance (close and far easier than mid-range).
- LLM-as-a-judge phase scores for Qwen2.5-VL family show strong in-frame perception (Phase 1: 73.2%/76.1%/81.9% for 3B/7B/32B on correct cases) but steep decay in later inference phases (Phase 3: 32.6%/36.3%/38.8%); self-consistency on correct vs incorrect answers: 64.8/32.5 (3B), 74.1/41.7 (7B), 83.6/54.2 (32B), confirming coherence-accuracy coupling.
- Linear probing shows cross-entropy loss rises monotonically across Stages 1-3 (and with model size), while entropy trajectories cleanly separate correct (low, tightly distributed) from incorrect (high, diffuse) reasoning paths - entropy acts as an effective proxy for spatial reasoning reliability.

## CV-ready bullets
**Short (less than 15 words):** Co-authored (2nd of 6, co-author) cognitively grounded multi-view spatial-reasoning benchmark and VLM latent-state analysis.
**Medium (less than 30 words):** Co-authored (2nd of 6, co-author, under review) ReMindView-Bench with >50K VQAs and a 4-phase cognitive analysis pipeline showing 15 VLMs plateau at 30-45% vs 81.5% human on multi-view spatial reasoning.
**Long (less than 50 words, for research statement):** Co-authored (2nd of 6, co-author, under review) ReMindView-Bench, a cognitively grounded multi-view spatial-reasoning benchmark (>50K VQAs across 5 room types, object-/view-centric patterns, 10 distance levels), and a paired explicit reasoning-path (LLM-as-judge, self-consistency) plus implicit latent-state (linear probing, entropy dynamics) analysis across 15 VLMs, diagnosing cross-view geometric misalignment as the dominant failure mode.

## Keywords / themes
visual spatial reasoning, multi-view, VLM interpretability, cognitive science, mental models, reasoning path analysis, latent state probing, linear probing, entropy dynamics, LLM-as-a-judge, self-consistency, perspective taking, cross-view alignment, benchmark construction, Infinigen, Blender, VQA

## Notable details
- First cognitively grounded multi-view spatial benchmark explicitly disentangling viewpoint spatial pattern (object-centric vs view-centric), perspective taking (self vs perspective-change), relation hierarchy (V-V / V-O / O-O), cross-frame vs non-cross-frame, and object-viewpoint distance.
- Pitch angle: bridges cognitive science (Johnson-Laird mental models, Biederman recognition-by-components, Kosslyn mental imagery, Loomis allocentric transformations) with mechanistic interpretability tools (linear probes, entropy probes).
- Includes a 660-sample ReMindView-Bench (small) subset used for direct human evaluation, providing the 81.5% human anchor.
- Releases benchmark on Hugging Face (Xue0823/ReMindView-Bench) and code on GitHub (pittisl/ReMindView-Bench); arXiv preprint 2512.02340 posted Dec 2 2025.
- Strong narrative hook for research-statement use: entropy and probe loss both separate correct vs incorrect reasoning trajectories, suggesting a concrete training signal (uncertainty calibration, representation-preservation) for cognitively grounded VLM training.
- Complements the survey paper (arxiv-spatial-reasoning-survey) already in the same CV folder - positions Haoming in the University of Pittsburgh spatial-reasoning group's benchmark + analysis line of work.
