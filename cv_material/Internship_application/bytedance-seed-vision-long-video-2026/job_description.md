---
company: ByteDance
team: Seed Vision
role: Student Researcher (Long-Range Video Generation), 2026 Start (PhD)
location: San Jose, CA
type: Campus Intern (PhD)
employment_code: A215422
url: https://jobs.bytedance.com/en/position/7533027329703282951/detail
canonical_url: https://joinbytedance.com/search/7533027329703282951
date_captured: 2026-05-08
---

# ByteDance Seed Vision — Student Researcher, Long-Range Video Generation

## Headline
Develop scalable systems for **extended video generation**: architectures that maintain consistent motion, identity, and layout across long temporal spans; hierarchical/recurrent latent structures; chunked/autoregressive generation; long-video evaluation protocols.

## Responsibilities (verbatim)
- Create architectures that maintain "consistent motion, identity, and layout" across longer temporal spans.
- Explore hierarchical or recurrent latent structures for extended generation.
- Tackle **temporal drift, motion collapse, and detail preservation** challenges.
- Investigate **autoregressive or chunked generation** approaches balancing quality and memory efficiency.
- Establish **evaluation protocols** for assessing long-video quality metrics.

## Minimum Qualifications
- Pursuing a PhD in Computer Vision, Machine Learning, or related field.
- Research experience in **generative modeling for video or temporal sequences**.
- **First-author** publications at top-tier venues (CVPR, ICCV, ECCV, NeurIPS, ICLR, ICML).
- Proficiency with deep learning frameworks and large-scale video datasets.

## Preferred Qualifications
- Experience with **diffusion or transformer-based video models**.
- Familiarity with long-form video datasets.
- Knowledge of perceptual metrics and video evaluation methods.

## Compensation / Logistics
- $60/hour. Health/life insurance, wellness, 10 paid holidays, paid sick time.
- Location: San Jose, in-person.

## Why I'm a fit (mapping to my work)

| JD requirement | Best matching evidence in my work |
|---|---|
| First-author top-tier publication | **CVPR'26 Oral — InfiniBench** (first author) |
| Video / temporal sequence experience | InfiniBench renders task-aware egocentric video benchmarks via Blender Cycles; MosaicThinker reasons over RGB egocentric video with iterative key-frame selection; ReMindView analyzes multi-view spatial cognition across video frames |
| Long-video evaluation protocols | **InfiniBench is literally a video-benchmark synthesis + evaluation methodology** -- diagnoses VLMs across measurement / perspective-taking / spatiotemporal task families with parameterized compositional, relational, and observational complexity; this is the closest possible direct match to "establish evaluation protocols for long-video quality" |
| Hierarchical / recurrent latent structures | **VLM-Latent-Shaping** (Dirichlet-energy regularizer over Laplacian eigenmaps of 3D scene graph) -- explicit latent-structure manipulation; **HEVEN** Spatial Semantic Tree (rooms / areas / pillars) -- hierarchical latent decomposition over spatial content |
| Chunked / autoregressive generation | MosaicThinker iterative key-frame selection is a sequential-decision policy over a temporal observation budget -- sister problem to chunked generation; FineXL evaluates personalized **autoregressive image models** (Anole-7B) alongside diffusion |
| Temporal drift / detail preservation / consistency across frames | MosaicThinker's MST-based cross-frame alignment explicitly fixes drift in multi-view fusion; ReMindView's joint reasoning-path + entropy analysis quantifies coherence across frames |
| Diffusion or transformer-based video models | **FreezeAsGuard** (bilevel mask learning over Stable Diffusion tensors, co-author) and **FineXL** (training-free fine-grained explanation of personalized SD v2.1, SD 3.5, ControlGAN, Anole-7B; first author) -- both deep diffusion-model experience |
| Perceptual metrics / video quality | InfiniBench evaluation reports CLIP alignment, fidelity, realism, OB / CN, plus per-task accuracy; FineXL builds an explanation-MAE benchmark; FreezeAsGuard uses CSD / NudeNet / DeepFace metrics + 16-volunteer human study |
| Large-scale video datasets | Renders 1500+ Habitat video episodes (GlobalNav) and Infinigen + Blender video corpora (InfiniBench, ReMindView); evaluated on VSI-Bench, STI-Bench, Metro-Spatial-QA |
| Deep learning frameworks | PyTorch, CUDA, Hugging Face, ONNX, TensorRT |

## Honest gap and how I frame it
I have not yet trained a generative *video* diffusion model end-to-end. My direct generative-model experience is on **image** diffusion (FineXL, FreezeAsGuard) and **video as the rendered output of a controllable pipeline** (InfiniBench Blender). To frame this honestly without leading with the gap, the CV emphasizes:

1. The video work I *do* have (InfiniBench video benchmark + evaluation, MosaicThinker cross-frame video reasoning, ReMindView multi-view analysis).
2. Diffusion-model interpretability and training mechanics (FineXL linear-decomposition explanation; FreezeAsGuard bilevel tensor freezing -- a concrete recipe for differentiable manipulation of diffusion model weights).
3. Latent-representation shaping (VLM-Latent-Shaping) as direct evidence I can think about "hierarchical / recurrent latent structures."
4. Evaluation expertise (InfiniBench's parameterized complexity dial-up is a transferable evaluation framework idea).

## Tailoring decisions for the CV
- **Reorder** to lead with InfiniBench's video + evaluation contribution (not its spatial-reasoning angle), then diffusion work (FineXL, FreezeAsGuard) as a paired block, then VLM-Latent-Shaping for latent-structure work, then MosaicThinker / ReMindView for cross-frame video reasoning.
- **Drop or compress**: federated-learning publications, embodied-navigation papers (HEVEN / GlobalNav can be one line each since the focus is video generation, not navigation), TA section, ZJU RA bullet.
- **Foreground keywords** the JD names: video / generation / temporal / diffusion / latent / autoregressive / evaluation / perceptual metrics / Stable Diffusion.
- **Add** a "Generative Models & Video Tooling" skills block (Stable Diffusion v1.5/v2.1/3.5, Anole-7B, ControlGAN, Blender Cycles, Infinigen, Habitat video pipelines, FG-CLIP / CLIP, perceptual metrics).
