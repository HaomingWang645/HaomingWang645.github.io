---
layout: post
title:  "Uncovering and Shaping the Latent Representation of 3D Scene Topology in Vision-Language Models"
date:   2026-09-25 00:00:00 +08:00
image: /paper_images/vlm-latent-shaping.png
categories: research_pub
author: "Haoming Wang"
venue_badge: "NeurIPS 2026 · Spotlight"
venue_badge_class: "venue-neurips"
news_badge: "NeurIPS 2026 Spotlight (1.32%)"

venue: "Conference on Neural Information Processing Systems (NeurIPS) 2026 <strong>(Spotlight: 1.32%)</strong>"
authors: "<strong>Haoming Wang</strong>, Wei Gao"
subtitle: "vlm_latent_shaping"
paper: /pdfs/arxiv26_vlm_latent_shaping.pdf
code: https://github.com/pittisl/vlm-latent-shaping

---

We show that current VLMs hold a latent topological map of 3D scenes, but it is overshadowed by non-geometric semantics like color and shape. Isolating this subspace via cross-scene linear feature extraction reveals its correspondence to the Laplacian eigenmaps of the scene's 3D Gaussian-kernel graph, motivating a Dirichlet-energy-based latent regularizer that beats standard SFT by up to 12.1% on real-world spatial benchmarks.
