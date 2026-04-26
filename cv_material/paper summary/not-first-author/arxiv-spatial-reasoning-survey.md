---
title: Spatial Reasoning in Multimodal Large Language Models: A Survey of Tasks, Benchmarks and Methods
short_name: Spatial Reasoning MLLM Survey
venue: arXiv / Under Review
year: 2026
status: under review / arXiv preprint
authorship: 3rd author (of 6)
authors: Weichen Liu, Qiyao Xue, Haoming Wang, Xiangyu Yin, Boyuan Yang, Wei Gao
links: arXiv:2511.15722 (cs.AI, 14 Nov 2025)
---

## One-line pitch
A cognitive-function taxonomy of spatial reasoning in MLLMs that unifies tasks, benchmarks, and training- and inference-based improvement methods across text, vision-language, and 3D/embodied settings.

## Scope of survey
Surveys spatial reasoning capabilities of LLMs, VLMs, and broader MLLMs, covering tasks, benchmarks, evaluation metrics, and methods for improvement. Scope spans text-only, image/video, and 3D/embodied settings, with literature drawn primarily from 2020-2025 work on spatial intelligence, 3D grounding, embodied navigation, and VLM spatial reasoning.

## Contributions
Introduces a novel cognitive-perspective taxonomy that departs from prior modality-centric or task-role-centric surveys (e.g., Ma et al. 2024, Zha et al. 2025) by organizing spatial tasks along three cognitive dimensions (intrinsic/extrinsic frame of reference, qualitative/quantitative spatial information, static/dynamic transformation) and four levels of reasoning complexity (direct perception, single-step inference, multi-step chaining, advanced synthetic problems). Maps existing benchmarks across text-only, vision-language, and 3D/embodied settings onto this taxonomy and reviews evaluation metrics (traditional NLP/CV metrics plus LLM-as-judge approaches). Categorizes improvement methods into training-based (spatial-aware modules, synthetic fine-tuning data, RL-based reasoning training) and inference-based (CoT/visualization-of-thought prompting, explicit spatial representation) paradigms, analyzing complementary mechanisms. Identifies open challenges in dataset deficiencies, incomplete spatial understanding, and architectural/training paradigm issues, with corresponding future directions.

## Key numbers
- 5 cognitive task categories x 4 reasoning complexity levels in the proposed taxonomy
- Benchmarks mapped across 3 settings (text-only, image/video, 3D/embodied) with 20+ named benchmarks catalogued (e.g., SPARTQA, StepGame, SpatialEval, VSI-bench, MindCube, ScanRefer, SQA3D, EmbSpatial-Bench, ViewSpatial-Bench, OmniSpatial, Q-Spatial Bench, 3DSRBench)
- 30+ improvement methods categorized across training- and inference-based paradigms (e.g., LLaVA-3D, SpatialVLM, SAT, Embodied-R, RoboRefer, SpatialCoT, VoT, MVoT, SG-Nav)
- 34-page manuscript with extensive reference list spanning cognitive science foundations and 2020-2025 MLLM literature

## CV-ready bullets
**Short (<=15 words):** Co-authored survey on spatial reasoning in MLLMs proposing cognitive-function taxonomy (arXiv 2025).
**Medium (<=30 words):** Co-authored arXiv survey (3rd of 6) on spatial reasoning in multimodal LLMs, introducing a cognitive-function taxonomy across 5 task categories and 4 reasoning complexity levels over text, vision-language, and embodied settings.
**Long (<=50 words, for research statement):** Co-authored a comprehensive survey (3rd of 6 authors) of spatial reasoning in multimodal LLMs, introducing a cognitive-perspective taxonomy that departs from prior modality-centric frameworks, mapping 20+ benchmarks across text, vision-language, and 3D/embodied settings, and categorizing training-based and inference-based improvement methods to identify gaps toward human-like spatial intelligence.

## Keywords / themes
spatial reasoning, multimodal large language models, MLLM, VLM, survey, taxonomy, cognitive science, 3D grounding, embodied AI, vision-language, benchmarks, chain-of-thought, reinforcement learning, mental models, frames of reference

## Notable details
Positioned as a cognitively-grounded alternative to prior modality-driven 3D-LLM surveys (Ma et al. 2024b; Zha et al. 2025), making it the first spatial-reasoning survey to organize the field by cognitive function and reasoning complexity rather than input modality or task role. Draws explicit bridges to cognitive science (Johnson-Laird mental models, Tversky, place/grid cells) and the symbol grounding problem (Harnad 1990), giving it cross-disciplinary appeal. Covers both the latest 2025 MLLM work (MindCube, OmniSpatial, VSI-bench, RoboRefer, MetaSpatial, SpatialCoT) and classic benchmarks, making it pitch-worthy as a comprehensive and timely reference for researchers entering the spatial intelligence / embodied AI space.
