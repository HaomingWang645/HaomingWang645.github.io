---
title: MosaicThinker: On-Device Visual Spatial Reasoning for Embodied AI via Iterative Construction of Space Representation
short_name: MosaicThinker
venue: Under Review
year: 2026
status: under review
authorship: first author (co-first with Qiyao Xue)
authors: Haoming Wang, Qiyao Xue, Weichen Liu, Wei Gao
links: arXiv:2602.07082
---

## One-line pitch
A training-free, inference-time technique that boosts small on-device VLMs on cross-frame visual spatial reasoning by iteratively fusing fragmented multi-view observations into a unified sparse semantic map used as a visual prompt.

## Problem / motivation
Embodied AI on low-cost devices (robots, AR glasses, drones) increasingly needs 3D spatial reasoning (manipulation, actuation planning, egocentric-allocentric mapping) that off-the-shelf small VLMs perform very poorly, because their 2D training data and sequential token representations cannot align fragmented, occluded views across frames. Cloud-based large VLMs are impractical on-device, and training-based 3D-aware solutions need expensive 3D data, hardware (depth sensors, LiDAR), and generalize poorly across scenes. There is no prior training-free method that enables complex cross-frame spatial reasoning on small on-device VLMs from RGB video alone.

## Approach
MosaicThinker iteratively constructs a sparse global semantic map (task-relevant objects + camera poses on a top-down grid) from RGB-only egocentric video, and hands it to the VLM as a carefully crafted visual prompt (BEV symbolic grid + textual bounding-box coordinates). Key components: (1) Preprocessing grounds the query into target + cue objects via VLM. (2) Per-frame extraction uses off-the-shelf segmentation + monocular depth (e.g., DepthPro, GroundingDINO / YOLO-World). (3) Cross-frame alignment uses MatchAnything on RGB pixels (restricted to object bounding boxes) instead of ICP, with a topology-aware Maximum Spanning Tree over CLIP-similarity to compute global transformations through the most-similar path (avoids drift), plus an occlusion/partial-visibility refinement that re-segments frames using the predicted FoV and pixel correspondences. (4) Iterative key-frame selection progressively refines a sampling distribution using a Gaussian kernel over temporal locality to pick ~20-25 informative frames (latency budget roughly 1s per reasoning). The entire pipeline is training-free and deployed on NVidia Jetson Orion, Meta AR Glass, and OnePlus 12R smartphone.

## Key results
- On Jetson with InternVL3-8B, average accuracy rises from 42.0 (direct input) / 45.4 (APC) to 50.8 on VSI-Bench and from 40.7 / 45.4 to 51.2 on Metro-Spatial-QA; near-matches the ground-truth semantic map oracle (52.9).
- On Jetson with Qwen3-VL-32B-4bit, VSI-Bench average jumps from 56.4 to 67.1 (vs 62.5 for APC), with object counting 67.4 -> 77.2 and absolute distance 47.7 -> 53.2.
- On OnePlus 12R smartphone with Qwen-2.5-VL-3B, VSI-Bench accuracy improves from 30.2 to 33.4 while strongest baseline APC runs out of memory; MosaicThinker uses only 11.2 GB memory (comparable to lighter baselines).
- Up to ~40% accuracy improvement on difficult cross-frame reasoning tasks, up to 11% average gain on Metro-Spatial-QA with larger VLMs; on Jetson InternVL3-8B reduces memory by >26% (50.3 -> 36.9 GB) and is faster than APC (509s vs 536s).
- Built a new benchmark Metro-Spatial-QA (40 egocentric videos across 5 indoor scene types: supermarkets, libraries, museums, restaurants, classrooms; 160 QA pairs with occlusions and dynamic lighting). Key-frame selection reaches F1 0.70 with 20 iterations at 26.1% sampled frames (vs 0.31 uniform baseline).

## CV-ready bullets
**Short (<=15 words):** First training-free method enabling cross-frame spatial reasoning on small on-device VLMs.

**Medium (<=30 words):** Built MosaicThinker, a training-free inference-time pipeline that fuses fragmented multi-view RGB frames into a unified semantic map, boosting small on-device VLMs' spatial reasoning accuracy by up to 40%.

**Long (<=50 words, for research statement):** Led MosaicThinker, a training-free inference-time system that iteratively builds a sparse global semantic map from RGB video via MST-based cross-frame alignment and Gaussian-kernel key-frame selection, boosting 3B-32B on-device VLMs' cross-frame spatial reasoning by up to 40% on VSI-Bench, STI-Bench, and the new Metro-Spatial-QA benchmark across Jetson Orion, AR glasses, and smartphones.

## Keywords / themes
on-device AI, embodied AI, visual spatial reasoning, VLM, cross-frame reasoning, inference-time computing, training-free, semantic map, BEV, key-frame selection, 3D scene representation, visual prompting, edge deployment, Jetson Orion, AR glasses, robotic manipulation

## Notable details
- First training-free method (to authors' knowledge) for complex cross-frame spatial reasoning on small on-device VLMs - a clean gap in the literature.
- Uses sparse semantic map (not dense BEV or scene-graph) specifically because small VLMs cannot interpret dense 3D pixel representations; ablation confirms semantic-map-plus-text beats dense-point-cloud BEV and pure text description by ~13 points on InternVL3-8B.
- Topology-aware MST alignment is a clean technical contribution: replaces drift-prone sequential ICP chaining by routing every frame to a global anchor root through its highest-CLIP-similarity path; isolates outlier frames at MST leaves.
- MST alignment implemented via Kruskal's algorithm (O(N^2 log N)); uses cheap PSNR/SSIM (95.5% / 97.3% selection accuracy vs FID) for frame-pair similarity to avoid neural-metric cost.
- Occlusion refinement stage uses estimated camera pose + cross-frame pixel correspondences to re-prompt the segmentation model - a nice example of closing the loop between geometry and perception.
- Released new Metro-Spatial-QA benchmark covering 5 non-residential indoor scenes (supermarkets, libraries, museums, restaurants, classrooms) filling a clear gap in existing benchmarks (which focus on homes/offices).
- Deployed and profiled end-to-end on three real devices: Jetson Orion (PyTorch + TensorRT), OnePlus 12R smartphone (ONNX Runtime + NNAPI), Meta AR Glass - demonstrates genuine edge-systems chops, not just a simulator demo.
- Robust to scene complexity: accuracy drops gracefully as object count/spatial scale grows; latency stays flat (~506-519s) across complexity levels.
- Co-first author with Qiyao Xue. Advisor: Wei Gao (Pitt ECE). Connects naturally with the group's InfiniBench (CVPR 2026 oral) work on spatial-reasoning benchmarking.
