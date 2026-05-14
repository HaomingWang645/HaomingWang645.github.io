---
layout: page
title: "Why VLMs still can't reason about 3D space — and what we can do at inference time"
permalink: /notes/vlm-spatial-reasoning/
date: 2026-05-13
read_time: "~6 min read"
tags: "spatial reasoning, VLMs, inference-time scaling"
summary: "State-of-the-art VLMs still fall apart on basic 3D spatial questions, and just adding more training data isn't fixing it. The interesting frontier is inference-time."
---

<article class="blog-post standalone">
  <header class="blog-post-header">
    <div class="blog-post-meta">
      <span>{{ page.date | date: "%B %-d, %Y" }}</span>
      <span>&middot;</span>
      <span>{{ page.read_time }}</span>
      <span>&middot;</span>
      <span>{{ page.tags }}</span>
    </div>
    <p class="blog-post-back"><a href="{{ site.baseurl }}/notes/">&larr; all posts</a></p>
  </header>

  <div class="blog-post-body" markdown="1">

**TL;DR.** State-of-the-art VLMs still fall apart on basic 3D spatial questions — and just adding more training data isn't fixing it. Bolting a 3D encoder onto the input layer doesn't help much either. The interesting frontier is *inference-time*: how do we get the model to actually **think** in space, not just describe it?

---

#### The problem is more annoying than it sounds

Spatial reasoning is the unflashy backbone of every embodied application I care about — pick-and-place manipulators, autonomous drones, wearable AR, anything that has to know "is the cup to the left of the laptop, and how far?".

<figure class="blog-figure">
  <a href="{{ site.baseurl }}/blog_images/vlm-spatial/slide-02.jpg" target="_blank"><img src="{{ site.baseurl }}/blog_images/vlm-spatial/slide-02.jpg" alt="The basic spatial reasoning capabilities (measurement, relative relation, mental rotation, perspective taking, multi-step) and the applications they unlock (robot manipulators, autonomous drones, wearable AR)." loading="lazy"></a>
  <figcaption>The basic spatial reasoning skills humans use without thinking — and the apps that wait on them.</figcaption>
</figure>

The benchmarks tell a humbling story. On basic visual cognition tasks, GPT-5 and InternVL3 sit at human-comparable levels (78–79%). On spatial reasoning tasks — the same models drop to **25–36%**, while humans are still at 95%. The gap isn't closing the way it has for language or general perception.

<figure class="blog-figure">
  <a href="{{ site.baseurl }}/blog_images/vlm-spatial/slide-03.jpg" target="_blank"><img src="{{ site.baseurl }}/blog_images/vlm-spatial/slide-03.jpg" alt="Bar charts showing human vs. GPT5 vs. InternVL3 on basic cognition (79/78/51%) versus spatial reasoning (95/36/25%)." loading="lazy"></a>
  <figcaption>Basic cognition is close to solved. Spatial reasoning is not.</figcaption>
</figure>

#### Throwing more data at it has hit a wall

The first instinct is the obvious one: keep scaling. Train on more spatial data. But the curves are flattening — measurement-style tasks plateau after about 2M samples, and multi-step spatial reasoning barely moves.

<figure class="blog-figure">
  <a href="{{ site.baseurl }}/blog_images/vlm-spatial/slide-04.jpg" target="_blank"><img src="{{ site.baseurl }}/blog_images/vlm-spatial/slide-04.jpg" alt="Two performance-vs-data-volume curves. Left: measurement task plateaus after ~2M samples. Right: multi-step reasoning curve almost flat." loading="lazy"></a>
  <figcaption>More data isn't the bottleneck. The model isn't building a mental scene; it's pattern-matching tokens.</figcaption>
</figure>

Why? Because the bottleneck isn't *exposure*. It's that the model has no mechanism for actually maintaining a mental 3D scene. You can show it a million top-down floor plans; at inference time it's still doing 2D pattern matching.

#### "Just add a 3D encoder" — also a trap

The next instinct: bolt a 3D point-cloud (or depth) encoder onto the input layer, fine-tune, done.

<figure class="blog-figure">
  <a href="{{ site.baseurl }}/blog_images/vlm-spatial/slide-06.jpg" target="_blank"><img src="{{ site.baseurl }}/blog_images/vlm-spatial/slide-06.jpg" alt="Standard pipeline: 3D data encoder, LLM backbone, fine-tuning. Bar chart shows VLMs at 40–60%, humans at 89%, fine-tuned variants barely better." loading="lazy"></a>
  <figcaption>The standard recipe: add a 3D encoder, fine-tune, ship. The bar chart is the punchline.</figcaption>
</figure>

We tried this. The improvement is small and the failure mode is funny:

- Pull out the **language hints** (anything that mentions object names by position) and accuracy collapses. The model was reading the text, not the geometry.
- Stuff in the **raw 3D data** and you flood the context with low-level point coordinates that drown out anything semantically useful. Hallucination rate goes up.

<figure class="blog-figure">
  <a href="{{ site.baseurl }}/blog_images/vlm-spatial/slide-07.jpg" target="_blank"><img src="{{ site.baseurl }}/blog_images/vlm-spatial/slide-07.jpg" alt="Two failure modes visualized: removing linguistic cues makes performance plummet (language shortcut); raw 3D data floods context with distracting geometric details." loading="lazy"></a>
  <figcaption>Either the model is cheating with language, or it's drowning in points. Neither is reasoning.</figcaption>
</figure>

So the model was either cheating with language shortcuts or being overwhelmed by raw geometry. Neither of those is *reasoning*.

#### Inference-time scaling is where it gets interesting

There's a well-known trick from the text-reasoning world: at inference time, give the model more thinking budget — longer chain-of-thought, self-consistency, search. Performance scales with compute. The "aha moment" is real.

The annoying thing is that **thinking in text actively hurts spatial reasoning**. Verbalizing a 3D scene into tokens compresses away the geometry. So the question becomes: how do you scale inference-time compute *without* dumping everything into language?

<figure class="blog-figure">
  <a href="{{ site.baseurl }}/blog_images/vlm-spatial/slide-11.jpg" target="_blank"><img src="{{ site.baseurl }}/blog_images/vlm-spatial/slide-11.jpg" alt="Diagram of four inference-time scaling alternatives: (1) think with image, (2) think with 3D-aware tools as an agent, (3) think in semantically rich latent space." loading="lazy"></a>
  <figcaption>Four routes to scale inference-time compute for spatial reasoning, with their core trade-off (external tools vs. internal VLM reasoning).</figcaption>
</figure>

Four directions on the table:

1. **Think with images.** Crop, zoom, attend to task-relevant regions. Useful for *focusing* the model, but it doesn't actually extract spatial structure — it just tells the VLM where to look.
2. **Think with 3D-aware tools (agentic).** Treat 3D understanding as an external module. The model calls "verify the layout" or "measure this distance" as tool calls. This is the most interpretable path; it separates semantic reasoning from geometric computation. Open question: can it scale to messy downstream tasks like navigation in a cluttered apartment?
3. **Think in a semantically-rich latent space.** Skip the verbal bottleneck entirely — reason in continuous vision-language latents, with 3D structure injected as a latent token rather than as raw geometry or as text. Recent work uses VGGT-style 3D encoders + GRPO-based RL to nudge the latents in this direction. Current limitation: only *dense* 3D features get into the latent; fine-grained 2D supervision at intermediate steps is missing.
4. **(My favorite) Step-level spatial supervision.** When the camera moves between views, the model loses track of *what corresponds to what*. So inject a `[2D latent token]` per view, conditioned on the camera pose, and supervise reasoning at each view transition.

<figure class="blog-figure">
  <a href="{{ site.baseurl }}/blog_images/vlm-spatial/slide-15.jpg" target="_blank"><img src="{{ site.baseurl }}/blog_images/vlm-spatial/slide-15.jpg" alt="Proposed pipeline: extract step-level spatial cues from per-view inputs using a 3D VGGT encoder, plus camera-pose tokens, fed into the VLM as [3D latent token] and [2D latent token] interleaved with text tokens." loading="lazy"></a>
  <figcaption>The setup I'm exploring: anchor latent reasoning with per-view 2D cues and camera-pose tokens — chain-of-thought, but for geometry.</figcaption>
</figure>

The idea is to give the latent reasoning chain explicit anchor points the way chain-of-thought tokens anchor verbal reasoning.

#### Where I'm headed

Two threads I'm pulling on right now:

- An **agentic workflow** layered on top of a VLM-based mobile robot — using 3D tools and learned navigation skills to handle dynamic blocking and rerouting. The robot doesn't need a perfect spatial map; it needs to know *when* to query a tool.
- A **latent-supervision setup** where step-level 2D spatial cues are extracted from the input views and used as targets during multi-step reasoning. Early experiments suggest entropy and probe-loss are decent signals for whether the model is actually maintaining spatial coherence (vs. confabulating).

Both feel like they're attacking the right thing: not making the model see better, but making the model *reason* in a space where geometry has somewhere to live.

---

#### References & further reading

**Our own work this draws on**

- *InfiniBench: Infinite Benchmarking for Visual Spatial Reasoning with Customizable Scene Complexity* — Wang, Xue, Gao. **CVPR 2026 (Oral)**. [arXiv:2511.18200](https://arxiv.org/abs/2511.18200). The benchmark backing many of the numbers above.
- *Uncovering and Shaping the Latent Representation of 3D Scene Topology in Vision-Language Models* — Wang, Gao. <a href="{{ site.baseurl }}/pdfs/arxiv26_vlm_latent_shaping.pdf" target="_blank">[Paper]</a> / [Code](https://github.com/pittisl/vlm-latent-shaping). Direct evidence that VLMs carry a latent topological map, and the regularizer that makes it usable.
- *MosaicThinker: On-Device Visual Spatial Reasoning for Embodied AI via Iterative Construction of Space Representation* — Wang, Xue, Liu, Gao. [arXiv:2602.07082](https://arxiv.org/abs/2602.07082). Inference-time fusion of cross-frame cues for small on-device VLMs.
- *Reasoning Path and Latent State Analysis for Multi-view Visual Spatial Reasoning* — Xue, Liu, Wang, Wang, Wu, Gao. [arXiv:2512.02340](https://arxiv.org/abs/2512.02340). Cognitively-grounded analysis showing where in the reasoning chain VLMs actually break.

**Background and related work**

- VGGT — *Visual Geometry Grounded Transformer*, the 3D encoder cited as a feature source in the latent-reasoning pipeline.
- GRPO — *DeepSeekMath / DeepSeek-R1* (Shao et al.), the RL recipe powering recent latent-reasoning work.
- Inference-time scaling in the text domain — *OpenAI o1*, *DeepSeek-R1*. The "aha moment" reference.
- "Think with images" line of work — VLMs that crop/zoom on tool-call-like primitives (e.g., V*).

**The slides**

The deck this post is built from: <a href="{{ site.baseurl }}/others/scaling visual spatial reasoning at inference time.pdf" target="_blank">scaling visual spatial reasoning at inference time (PDF)</a>.

Always happy to chat about any of this — `haw200 [at] pitt.edu`.

  </div>
</article>
