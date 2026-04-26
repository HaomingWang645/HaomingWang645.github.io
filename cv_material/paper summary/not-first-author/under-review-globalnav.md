---
title: "GlobalNav: Daily Object Navigation in VLM-based Autonomous Mobile Systems with Aligned Local and Global Views"
short_name: GlobalNav
venue: Under Review (not on arXiv yet)
year: 2026
status: under review
authorship: 3rd author
authors: anonymous submission (double-blind; Haoming Wang listed as 3rd author per CV metadata)
links: none public yet
---

## One-line pitch
GlobalNav is an infrastructure-assisted VLM navigation system that semantically aligns elevated CCTV-style global views with a robot's egocentric local view, lifting daily-object navigation success rates from under 30% to roughly 90% without pre-built maps.

## Problem / motivation
VLM-based mobile robots rely almost entirely on egocentric perception and commonsense priors, so they collapse into exhaustive blind searches when daily objects (glasses, backpack, mug) are placed in unpredictable spots - existing Object-Goal and Vision-Language Navigation methods achieve under 30% success on such targets. Indoor spaces are already densely equipped with infrastructure cameras (home surveillance, office CCTV) that offer a continuous global view, but bridging the severe perspective gap between the elevated wide-angle infrastructure view and the robot's low-angle egocentric view is a non-trivial systems challenge that naive geometric projection or image matching cannot solve.

## Approach
GlobalNav has three components running across an edge server (infrastructure) and the robot. (1) A Discretized Spatial Topology module builds a shared topological graph on each side: Grounding-SAM detects landmarks, a VLM generates fine-grained descriptions, and pairwise spatial relations are quantized into 8 directional bins (front, front-left, ... front-right) using monocular depth (DepthPro) on the infrastructure side and RGB-D on the robot side. (2) A Cross-View Perspective Alignment module uses SAM-3D / 3DGS to render V multi-angle views of each infrastructure landmark, computes visual + cross-modal semantic similarity with FG-CLIP plus a cosine-based directional edge similarity, and jointly solves for landmark correspondences and the single discrete 8-bin rotational offset via spectral graph matching over an affinity matrix M(k). (3) A Hierarchical Navigation Strategy uses VLM (GPT-5.2) Chain-of-Thought reasoning over the global view to plan a landmark chain; Path Update reroutes through an alternative landmark when the next is blocked, and when no alternative exists the system falls back to Voronoi-graph waypoint selection with structured "<direction> of <landmark> at <distance>" node descriptions, plus a Target Confirmation stage near the terminal landmark. A two-level fault-tolerant recovery invalidates wrong matches and replans.

## Key results
- Habitat simulator (20 scenes, 80 infrastructure cameras, 1500 episodes): SR 90.91% vs IEVE 53.82%, UniGoal 37.09%, FreeReg 17.9%; SPL 51.37% vs 33.37 / 31.58 / 15.1; DTG 0.78 m vs 4.31 / 4.25 / 10.45; Steps 150.36 vs 293.94 / 329.67 / 438.37 - roughly 40% higher SR and ~50% fewer steps than best baseline.
- Real-world (Agile LIMO + RealSense D435i + RoboSense Airy LiDAR robot; Basler 1200x1920 infrastructure cam on Jetson Orin; 10 scenes across home/office/restaurant, 300 episodes): ~85% SR and SPL ~0.5 across backpack/laptop/mug/glasses, beating baselines by >40% SR and >50% SPL.
- Robustness: maintains ~90% SR across low/medium/high scene complexity, landmark density, and object size, and still ~80% SR at >6 m start-to-target distance where baselines drop below 40%.
- System overhead: ~50% lower search time and ~40% lower VLM token usage than UniGoal (GlobalNav uses ~60% of UniGoal's tokens because it queries the VLM once up front and only on discrepancy).
- Ablations: cross-view alignment reaches >90% at 1-bin-offset accuracy (~15% over best baseline); stage-wise ablation shows +30% SR from Path Planning, +10% from Plan Update + Waypoint Selection, +15% from Target Confirmation, reaching 90.91% SR from a 32% blind-search baseline.

## CV-ready bullets
**Short (<= 15 words):** Third author on GlobalNav, an infrastructure-assisted VLM navigation system achieving 90% daily-object success.
**Medium (<= 30 words):** Third author on GlobalNav (under review, 2026): a VLM-based navigation system aligning CCTV global views with egocentric robot views via discretized spatial topology, boosting daily-object success ~40% over SOTA.
**Long (<= 50 words, for research statement):** Co-authored GlobalNav (third author, under review 2026), an infrastructure-assisted VLM navigation system that aligns elevated CCTV global views with robot egocentric views through an 8-bin discretized spatial topology, spectral-graph cross-view matching, and hierarchical landmark-plus-Voronoi-waypoint planning, achieving 90.91% SR (vs 53.82% best baseline) on 1500 Habitat episodes and ~85% SR on a real LIMO robot.

## Keywords / themes
VLM, object goal navigation, embodied AI, autonomous mobile systems, robotics, infrastructure-assisted perception, cross-view alignment, CCTV / surveillance-assisted robots, edge computing, Habitat simulator, Voronoi planning, spectral graph matching, Gaussian Splatting, hierarchical navigation, daily object retrieval.

## Notable details
- Positions itself as the first VLM-based navigation system to exploit infrastructure-camera global views for daily-object retrieval; eliminates need for pre-built maps or commonsense priors.
- Clever engineering choice: discretizing spatial relations into 8 bins sidesteps monocular depth error and reduces perspective alignment to solving for one of 8 discrete rotations - tractable via spectral graph matching.
- Uses a stack of modern VLM/3D tooling: GPT-5.2 for reasoning, Grounding-SAM for detection, DepthPro for monocular depth, SAM-3D + 3D Gaussian Splatting for multi-view landmark rendering, FG-CLIP for image/text embeddings.
- New dataset/testbed contribution: 20 Habitat scenes with 80 placed virtual infrastructure cameras, 1500 episodes, 5 daily objects from PIN dataset, plus a 10-scene / 30-infrastructure-camera / 300-episode real-world benchmark with Polycam-reconstructed ground-truth paths - fills a gap since existing navigation datasets lack infrastructure viewpoints.
- Systems-paper framing (edge server + Wi-Fi text messaging between nodes, Jetson Orin deployment) makes it a strong fit for venues like MobiSys/MobiCom/SenSys or ASPLOS/SOSP-adjacent systems tracks; also relevant to robotics venues (ICRA/IROS) and embodied-AI tracks (CVPR/NeurIPS).
- Honest framing of limitations (single-camera assumption, indoor-only) opens natural follow-up directions (multi-camera handoff, outdoor delivery robots with roadside cameras).
