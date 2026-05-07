---
layout: post
title:  "Uncovering and Shaping the Latent Representation of 3D Scene Topology in Vision-Language Models"
date:   2026-05-07 00:00:00 +00:00
categories: research_recent
author: "Haoming Wang"

authors: "<strong>Haoming Wang</strong>, Wei Gao"
subtitle: "vlm_latent_shaping"
paper: /pdfs/arxiv26_vlm_latent_shaping.pdf
code: https://github.com/pittisl/vlm-latent-shaping

---

We show that current Vision-Language Models possess a latent topological map of 3D scenes, but it is heavily overshadowed by non-geometric visual semantics like color and shape. By isolating this spatial subspace via cross-scene linear feature extraction, we recover a clean subspace that causally controls the model's spatial outputs, and prove its correspondence to the Laplacian eigenmaps of the scene's 3D Gaussian-kernel graph. Motivated by this geometric identification, we introduce a Dirichlet-energy-based latent regularizer that, with a single 500-step supervised fine-tuning run on simple synthetic data, outperforms standard SFT and competitive baselines by up to 12.1% on real-world spatial benchmarks involving scene topology understanding.
