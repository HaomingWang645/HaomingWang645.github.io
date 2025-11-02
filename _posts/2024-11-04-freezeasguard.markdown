---
layout: post
title:  "Freezeasguard: Mitigating illegal adaptation of diffusion models via selective tensor freezing"
date:   2024-11-04 22:21:59 +00:00
image: /paper_images/freezeasguard.png
categories: research_arxiv
author: "Haoming Wang"

authors: "Kai Huang, <strong>Haoming Wang (co-author)</strong>, Wei Gao"
subtitle: "freezeasguard"
paper: /pdfs/freezeasguard.pdf

---

Text-to-image diffusion models can be fine-tuned for personalized domains, but this adaptability also enables misuse, such as forging public figures, replicating copyrighted artworks, or producing explicit content. Existing detection or unlearning methods fail to prevent illegal adaptations. We introduce FreezeAsGuard, a technique that irreversibly mitigates such misuse by selectively freezing tensors in pre-trained diffusion models that are critical to illegal adaptations, while preserving legal fine-tuning capabilities.

