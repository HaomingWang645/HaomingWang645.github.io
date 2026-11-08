---
layout: post
title: "WorldMind: Test-Time Scaling for Visual Spatial Reasoning through Latent World Simulation"
date: 2026-11-08 00:00:00 +08:00
image: /paper_images/worldmind.png
categories: research_recent
author: "Haoming Wang"
venue: "Preprint"
authors: "<strong>Haoming Wang</strong>, Yongjian Fu, Ju Ren, Wei Gao"
subtitle: "WorldMind latent world simulation with process-verifiable reinforcement learning"
paper: /pdfs/worldmind_arxiv.pdf
---

WorldMind scales visual spatial reasoning at test time by letting a VLM choose viewpoint transformations while a world model simulates them in latent space. An adapter returns spatial features directly to the VLM, avoiding repeated RGB rendering, and process-verifiable reinforcement learning rewards valid intermediate steps. Across three benchmarks, the method improves average performance by 16.0% over the base VLM and 8.3% over the strongest RGB-based simulation baseline.
