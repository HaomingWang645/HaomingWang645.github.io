---
title: "InfiniBench: Infinite Benchmarking for Visual Spatial Reasoning with Customizable Scene Complexity"
short_name: InfiniBench
venue: CVPR 2026 (Oral)
year: 2026
status: published
authorship: first author
authors: Haoming Wang, Qiyao Xue, Wei Gao
acceptance_rate: CVPR 2026 oral (top-tier selective distinction; exact oral rate not stated in paper)
links: arXiv 2511.18200; code https://github.com/pittisl/infinibench; data https://huggingface.co/datasets/Haoming645/infinibench
---

## One-line pitch
InfiniBench is a fully automated, LLM-agent-driven 3D benchmark generator that synthesizes a theoretically infinite family of photo-realistic video scenes with user-parameterized control over compositional, relational, and observational complexity, enabling fine-grained diagnosis of VLM spatial-reasoning failure modes.

## Problem / motivation
Existing visual-spatial-reasoning benchmarks for VLMs are either photorealistic but unscalable (real-world captures), or synthetic but coarse — they conflate multiple axes of scene complexity (object count, occupancy, occlusion, viewpoint) and cannot isolate specific failure modes. Prior LLM-based scene generators produce physically implausible layouts (overlapping / out-of-boundary / mis-oriented objects) once complexity grows, while procedural frameworks (Infinigen, ProcTHOR) lose prompt fidelity at high density. This gap prevents principled analysis of where and why VLMs break down in 3D spatial reasoning.

## Approach
InfiniBench introduces a three-stage pipeline that combines LLM planning with procedural execution. (1) An LLM-agentic framework (Gemini-2.5-Pro) translates natural-language scene descriptions into machine-readable procedural constraints and iteratively refines them via a CoT feedback loop driven by optimizer error reports (BEV collision maps + unmet-constraint summaries). (2) A new cluster-based layout optimizer replaces traditional hierarchical optimization by grouping related objects (e.g., a table + its surrounding chairs) into movable clusters, enabling cluster-level action spaces and collective-bounding-box collision checks that make previously intractable dense/cluttered scenes feasible. (3) A task-aware, frontier-based camera trajectory optimizer defines unvisited target objects as frontiers, samples viewpoints scored on validity / in-FOV / occlusion, and links them with Dijkstra-planned collision-free paths so that every task-relevant object is clearly captured in the rendered video (Blender Cycles). The system accepts user-parameterized control over compositional, relational, and observational complexity.

## Key results
- Outperforms both LLM-based generators (I-Design, Holodeck, LayoutGPT) and procedural frameworks (Infinigen, Luminous) on prompt fidelity, CLIP alignment, realism, and physical plausibility — and is the only method that avoids the fidelity-vs-plausibility tradeoff at high complexity.
- In high-object-count settings: InfiniBench achieves Fidelity 0.98 (vs Infinigen 0.64, Luminous 0.42) with 0.1 out-of-boundary objects and 0.0 collisions, while LayoutGPT's collisions rise 5.6x (from 2.4 to 13.5) as object count grows.
- In high-occupancy settings: Fidelity 0.91 with 0.0 OB / 0.1 CN; baselines either drop fidelity to 0.43-0.49 (procedural) or incur 5.6-9.6 collisions (LLM-based).
- Ablation: constraint-refinement alone lifts Fidelity 0.64 to 0.71; cluster-optimization alone lifts it to 0.68; combined full system reaches 0.92 — a synergistic jump. Constraint refinement converges within 5 iterations.
- VLM diagnostic study across Gemini-2.5-Pro, GPT-5, LLaVA-Video-7B, InternVL3.5-8B, Cambrian-S-7B on measurement / perspective-taking / spatiotemporal tasks shows sharp degradation with compositional complexity (5 to 50 objects), moderate drops from irrelevant-object distractors (e.g., Gemini-2.5-Pro spatiotemporal 87.9 to 56.2 from low to high), and a large bird's-eye-vs-egocentric gap for perspective-taking and spatiotemporal tasks (e.g., Gemini-2.5-Pro perspective-taking 77.4 BEV vs 67.2 ego; GPT-5 spatiotemporal 49.0 BEV vs 31.3 ego), while measurement-task accuracy is viewpoint-invariant — revealing failure modes that aggregate benchmarks cannot surface.

## CV-ready bullets
**Short (<= 15 words):** First-authored CVPR 2026 Oral paper on infinite 3D benchmark generation for VLM spatial reasoning.
**Medium (<= 30 words):** First-authored CVPR 2026 Oral (InfiniBench): an LLM-agentic, cluster-optimization 3D benchmark generator that produces infinitely many photo-realistic scenes with parameterized complexity for fine-grained VLM spatial-reasoning diagnosis.
**Long (<= 50 words, for research statement):** First-authored CVPR 2026 Oral paper "InfiniBench": a fully automated benchmark generator combining an LLM-agentic constraint-refinement loop, a cluster-based layout optimizer, and frontier-based camera-trajectory planning to synthesize theoretically unlimited photo-realistic 3D scenes with user-controllable compositional, relational, and observational complexity, enabling principled diagnosis of VLM spatial-reasoning failure modes across five frontier models.

## Keywords / themes
visual spatial reasoning, VLM evaluation, 3D scene generation, LLM agent, procedural generation, benchmark synthesis, cluster-based layout optimization, camera trajectory planning, frontier-based exploration, Infinigen, embodied AI, video understanding

## Notable details
ORAL at CVPR 2026 — top-tier selective recognition. First author at University of Pittsburgh (advisor: Wei Gao). Open-sourced code (github.com/pittisl/infinibench) and sample benchmarks on HuggingFace (Haoming645/infinibench); arXiv 2511.18200. Novel methodological contributions: (1) CoT-driven iterative constraint refinement using BEV error reports as feedback — a concrete recipe for wrapping LLMs around optimizers; (2) "movable cluster" abstraction that expands action space and enables dense-scene procedural generation previously intractable; (3) redefinition of frontier-based exploration where frontiers are unvisited task-relevant objects instead of occupancy-grid cells. Diagnostic study covers two closed-source (Gemini-2.5-Pro, GPT-5) and three open-source (LLaVA-Video-7B, InternVL3.5-8B, Cambrian-S-7B) VLMs across three task families (measurement, perspective-taking, spatiotemporal). Positions the work as infrastructure for future spatially-grounded VLM training/prompt-tuning, not just evaluation — giving it long-term citation upside.
