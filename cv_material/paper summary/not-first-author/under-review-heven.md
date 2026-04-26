---
title: HEVEN: Small Object Search and Navigation on VLM-Based Autonomous Mobile Systems
short_name: HEVEN
venue: Under Review (not on arXiv yet)
year: 2026
status: under review
authorship: 3rd author
authors: (anonymized submission; Haoming Wang listed as 3rd author)
links: none public yet
---

## One-line pitch
HEVEN is the first VLM-based autonomous mobile system that locates and navigates to small, movable everyday objects by organizing scene memory as a hierarchical spatial semantic tree, enabling coarse-to-fine VLM reasoning that is pipelined with physical motion.

## Problem / motivation
Service robots are increasingly deployed in homes, offices, and healthcare settings, where a high-demand task is helping users find small daily objects (keys, medications, wallets) that are numerous, frequently relocated, and easily occluded. Existing VLM-based navigation approaches either run blind search without memory or use flat unstructured scene memory, both of which collapse when the target is small: success rates drop by up to 55% (non-memory) and 70% (flat memory) going from a sofa to a watch, and VLM reasoning cost balloons as the candidate object list grows.

## Approach
HEVEN's core insight is that humans do not memorize coordinates; they remember objects via spatial containment and support (e.g., "cup is on the dining table, which is in the kitchen"). HEVEN encodes this as a Spatial Semantic Tree (SST) with three levels: rooms, areas, and pillars, where a pillar is a novel abstraction that anchors a group of small objects to the stable supporting furniture (table, shelf) as a single minimum navigable unit, making navigation robust to small-object movement. A Tree-of-Thought (ToT) reasoning engine operates in two phases: logical breadth-first pruning queries the cloud VLM level-by-level to score and prune rooms, then areas, then pillars (top-k retained, k=2), keeping each prompt compact; physical depth-first traversal then visits remaining candidates using a utility U(v) = s_v / (1 + lambda * d_geo) that jointly balances semantic relevance and geodesic travel cost, with backtracking up the tree when utilities fall below threshold. An asynchronous speculative execution module decouples a deliberative planner from a high-frequency (>20 Hz) reactive controller, so the robot begins moving toward the top-scoring room as soon as room-level pruning finishes, and refines to area then pillar as later VLM rounds complete, masking cloud latency behind motion. The SST is built bottom-up from RGB-D exploration using Grounding SAM for open-vocabulary detection, with gradual-decay updates and event-triggered local reconstruction for dynamic scenes.

## Key results
- **Real-world dataset (10 scenes, 600 episodes, Unitree Go2 + Jetson Orin + D435i, GPT-4o backend):** HEVEN exceeds 70% success rate across large/medium/small targets with <110 s search time, while baselines (UniNavid, UniGoal, 3DMem) degrade sharply on small objects.
- **Synthetic multi-room benchmark (50 scenes, 1500 episodes, Infinigen + Isaac Sim):** 73.86% SR and 0.35 SPL, vs. DOVSG 24.20% / 0.09 - roughly 3x success and 4x path efficiency over SOTA. D2G 3.60 m (lowest), CE 16.16% (lowest). For small targets specifically: >60% SR (baselines <15%).
- **Resource efficiency:** SST memory footprint 15.8 MB vs. 3DMem 137.2 MB (8.7x reduction); per-episode VLM inference 7.22k tokens and 16.4 s, 2.4x lower than UniGoal and 4.4x lower than 3DMem; environment update 0.58 min vs. 3DMem's 24.28 min.
- **Robustness:** Under high object count and low spatial entropy (cluttered/concentrated scenes), HEVEN holds ~70% SR with SPL ~0.4-0.45, while baselines fall below 25% SR and 0.15 SPL.
- **Ablations:** Adding the pillar level alone lifts SR from 32% to 55%; full 3-level SST reaches ~75%. The joint cost-confidence utility (vs. distance-only, score-only, or no-backtrack variants) yields the best SPL (0.35) and fewest steps (~310).

## CV-ready bullets
**Short (<= 15 words):** Co-authored (3rd author) HEVEN, a VLM-based small-object search and navigation system.
**Medium (<= 30 words):** Contributed (3rd author) to HEVEN, a hierarchical-memory VLM navigation system that finds small movable objects with 74% success rate and 4x path efficiency over SOTA baselines.
**Long (<= 50 words, for research statement):** Co-authored (3rd author, under review) HEVEN, the first VLM-based autonomous mobile system for small-object search and navigation; it introduces a Spatial Semantic Tree with a pillar abstraction, tree-of-thought reasoning, and asynchronous speculative execution, achieving 73.86% success, 8.7x smaller memory, and 4.4x lower VLM inference overhead vs. SOTA.

## Keywords / themes
VLM, small object search, object-goal navigation, embodied AI, autonomous mobile systems, service robots, hierarchical scene memory, spatial semantic tree, tree-of-thought reasoning, speculative execution, cloud-edge pipelining, open-vocabulary 3D scene graphs, Unitree Go2, Isaac Sim, Infinigen.

## Notable details
- "First" VLM-based autonomous mobile system specifically targeting small-object search/navigation - a clean novelty framing.
- Introduces two design primitives with clear names: the **pillar abstraction** (grouping small items under their stationary supporter) and the **Spatial Semantic Tree (SST)** (rooms / areas / pillars + hierarchy edges + reachability edges).
- Pipelines cloud VLM reasoning with physical control via a deliberative-planner / reactive-controller split (>20 Hz) - a systems-flavored contribution that hides cloud latency, suggesting a systems/MobiSys/SenSys-style venue.
- Introduces two new datasets to fill a real benchmark gap (existing ObjectNav benchmarks only use large static furniture): 10 real-world scenes (600 episodes) and 100 procedurally-generated synthetic scenes (2500 episodes).
- Strong quantitative story: 40%+ absolute SR improvement, 4x SPL, 8.7x memory reduction, 4.4x inference reduction, 22% less end-to-end search time - easy to cite.
- Ablations cleanly justify each design choice (SST depth L0 to L3; utility function variants Dis / Sc / Sib vs. full).
- Reference [54] is "MosaicThinker" by Haoming Wang, Qiyao Xue, Weichen Liu, Wei Gao (arXiv 2602.07082, 2026) - suggests Wei Gao's group at Pitt; HEVEN likely shares that lab.
- Deployed on real hardware (Unitree Go2 quadruped + Jetson AGX Orin + RealSense D435i, GPT-4o via API) - strong real-system credibility.
