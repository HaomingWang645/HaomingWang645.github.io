---
company: ByteDance
team: Seed
role: Student Researcher (Multimodal Interaction & World Model — RL Focused), 2026 Start (PhD)
location: San Jose, CA
type: Campus Intern (PhD)
employment_code: A45303
url: https://jobs.bytedance.com/en/position/7529661012351420690/detail
canonical_url: https://joinbytedance.com/search/7529661012351420690
date_captured: 2026-05-08
---

# ByteDance Seed — Student Researcher (Multimodal Interaction & World Model, RL Focused)

## Headline
Designing and implementing **reinforcement-learning training systems for large-scale multimodal foundation models** within the ByteDance Seed team. The role sits at the intersection of multimodal foundation models, RL post-training, and world models for instruction-conditioned generation.

## Responsibilities (verbatim-ish)
- Design and implement RL training systems for large-scale multimodal foundation models.
- Develop frameworks integrating **video, audio, and language**, with a focus on **visual reasoning**.
- Explore RL approaches for both **understanding and generation**.
- Collaborate on model evaluation for **world modeling and instruction-conditioned generation**.

## Minimum Qualifications
- Currently pursuing a PhD in Software Development, Computer Science, or related technical field.
- Publications in premier venues such as **CVPR, ECCV, ICCV, NeurIPS, ICLR, ICML**.
- Strong research background in **reinforcement learning, multimodal learning, video understanding, or vision-language modeling**.
- U.S. work authorization (no sponsorship provided).

## Preferred Qualifications
- RL experience in **multimodal or interactive environments**.
- Knowledge of **video generation or diffusion models**.
- Large-scale model training experience.
- Strong programming with **ML pipeline development** skills.

## Compensation / Logistics
- $60/hour. Day-one health & life insurance, wellness, 10 paid holidays, paid sick time. Possible housing allowance for non-remote interns.
- Location: San Jose (in-person team).

## Why I'm a fit (mapping to my work)

| JD requirement | Best matching evidence in my work |
|---|---|
| Premier-venue publications | **CVPR'26 Oral — InfiniBench** (visual spatial reasoning benchmark, first author) |
| Multimodal learning / VLM | InfiniBench, MosaicThinker, VLM-Latent-Shaping, ReMindView, Spatial-Reasoning Survey |
| Visual reasoning | InfiniBench, MosaicThinker, ReMindView (cognitive-science framing of multi-view spatial reasoning) |
| RL / interactive multimodal environments | MosaicThinker iterative key-frame selection (sequential decision under observation budget); GlobalNav / HEVEN VLM-driven embodied navigation with Tree-of-Thought planning, hierarchical landmark/Voronoi waypoint selection, speculative execution |
| World model / instruction-conditioned generation | InfiniBench (LLM-agent + procedural 3D world generator with constraint satisfaction); VLM-Latent-Shaping (Dirichlet-energy regularizer that aligns the VLM's latent topology with the underlying 3D scene graph — directly relevant to world-model representation learning) |
| Video understanding | InfiniBench renders task-aware egocentric videos and probes VLMs across measurement / perspective-taking / spatiotemporal task families; MosaicThinker reasons over RGB egocentric video |
| Diffusion models | **FineXL** (training-free fine-grained explanations of personalized diffusion models, first author) and **FreezeAsGuard** (bilevel-optimization tensor-freezing defense for Stable Diffusion, co-author) |
| Large-scale model training | FedDC / AAAI'25 staleness — distributed FL training systems with device heterogeneity; MosaicThinker — end-to-end edge deployment on Jetson, AR Glass, smartphone |
| ML pipeline / programming | Open-sourced full pipelines: github.com/pittisl/{infinibench, vlm-latent-shaping, ReMindView-Bench, FreezeAsGuard} |

## Tailoring decisions for the CV
- **Reorder**: lead with InfiniBench (CVPR'26 Oral; the only Min-Qualifying premier-venue pub), then MosaicThinker, VLM-Latent-Shaping, ReMindView, GlobalNav/HEVEN as a connected "VLM + spatial reasoning + embodied interaction" arc.
- **De-emphasize** federated-learning systems work (MobiCom'25 FedDC, AAAI'25 staleness, MobiSys'25 Xpert) — keep them visible but compressed; they fit "large-scale model training" only loosely and dilute the narrative.
- **Foreground keywords** the JD names: RL / interactive environment / multimodal foundation model / world model / video / visual reasoning / diffusion. Add a "Research Themes" line at the top mirroring the JD vocabulary.
- **Add interactive-decision angle** to MosaicThinker (key-frame selection as sequential information-acquisition policy) and GlobalNav/HEVEN (Tree-of-Thought / hierarchical planner with utility-based decision making) — this is the closest evidence I have to RL-style interaction.
- **Highlight diffusion** by promoting FineXL into the published-or-equivalent visibility tier and keeping FreezeAsGuard.
- **Drop / shorten** Teaching Assistant section (only 1-line summary) and the Zhejiang University RA bullet — they don't earn space here.
