---
title: When Device Delays Meet Data Heterogeneity in Federated AIoT Applications
short_name: FedDC
venue: MobiCom 2025
year: 2025
status: published
authorship: first author
authors: Haoming Wang, Wei Gao
acceptance_rate: 17.1%
links: https://doi.org/10.1145/3680207.3723481
---

## One-line pitch
FedDC is the first federated learning technique that tackles the correlated coupling of device delays and data heterogeneity in AIoT by using server-side gradient inversion to compensate delayed model updates, improving accuracy by up to 34% without changing on-device training or leaking privacy.

## Problem / motivation
In real federated AIoT applications (human activity recognition, smart health, hazard rescue, industrial sensing), slow devices are systematically the ones holding unique data classes (e.g., users doing outdoor activities under poor connectivity), so device delays and data heterogeneity are tightly correlated rather than independent. Prior FL frameworks (synchronous discard, asynchronous down-weighting, first-order Taylor compensation) treat delay as independent of data and therefore either drop unique knowledge from the global model or blow up compensation error when delays grow large/unbounded. No prior work had addressed this correlated regime, which is exactly where practical AIoT operates.

## Approach
FedDC ("Delay Compensator in FL") keeps the IoT device's local FL procedure completely unchanged and pushes all extra work to the server. When the server receives a delayed update w_i^{t-tau} computed against an outdated global model, it runs gradient inversion against the cached outdated global model to recover an intermediate surrogate dataset D_est that approximates the slow device's local data distribution (using L1-norm as the inversion distance metric, shown empirically most stable vs. L2/cosine). The server then "replays" local training of the current global model w_global^t on D_est to approximate what the delayed update would have been without delay, and aggregates that compensated update. An adaptive rule decides when to stop compensating and switch back to vanilla FL as training matures. To make gradient inversion affordable, 95% gradient sparsification and selective computation cut server cost by ~20x, and server-side gradient inversion is parallelized with the next epoch's on-device local training so compensation adds zero wall-clock delay. Because only distributional knowledge (not individual samples) is recovered, local data privacy is preserved.

## Key results
- Up to 34% accuracy improvement on data classes hit by high device delays; on ExtraSensory specifically, FedDC beats the best prior baseline (W-Pred) by 35.5% under high delays, and beats baselines by 11.7% on PAMAP2.
- Evaluated on 3 real-world AIoT datasets (PAMAP2 HAR with MLP, ExtraSensory with 1D-CNN, MDI disaster images with ResNet18/2D-CNN) against 5 baselines (Unweighted, Weighted, Asyn-Tiers, 1st-Order, W-Pred) deployed on 13 heterogeneous IoT devices including 6 smartphone models (Pixel XL/2/4/5/7, LG G5), Raspberry Pi 4B, and NVIDIA Jetson Nano.
- Under a variant global data distribution (harder, realistic AIoT setting), FedDC reaches 53.5% / 34.7% / 69.3% accuracy on PAMAP2 / ExtraSensory / MDI vs. 38.9% / 20.3% / 65.1% for unweighted FedAvg — an even larger gap than the fixed-distribution setting.
- Server-side cost reduced from 21.63 epochs (MLP, full gradient inversion) to 0.91 epochs with selective computation + 95% sparsification, fitting within a single global epoch and adding zero end-to-end training delay; 95% sparsification causes less than 2% FL performance drop.
- Scales from 50 to 300 IoT devices (30x growth) with consistent performance; zero extra compute or communication cost at IoT devices; privacy preserved (server cannot reconstruct original samples or labels).

## CV-ready bullets
**Short (<= 15 words):** FedDC (MobiCom '25): server-side gradient inversion compensates delayed FL updates, +34% accuracy.
**Medium (<= 30 words):** First-authored MobiCom '25 paper FedDC addressing correlated device delays and data heterogeneity in federated AIoT; server-side gradient inversion compensates delayed updates, improving accuracy up to 34% across 13 heterogeneous IoT devices.
**Long (<= 50 words, for research statement):** At MobiCom 2025 (17.1% acceptance), I introduced FedDC, the first federated learning technique for the correlated regime where slow AIoT devices hold the unique data classes. FedDC uses server-side gradient inversion to recover data distributions and compensate delayed updates, raising accuracy up to 34% on three real-world AIoT benchmarks with zero on-device overhead and preserved privacy.

## Keywords / themes
federated learning, AIoT, mobile systems, device heterogeneity, data heterogeneity, stragglers, asynchronous training, gradient inversion, delay compensation, human activity recognition, privacy-preserving ML, on-device training, semi-asynchronous FL

## Notable details
- Flagship ACM venue: MobiCom 2025 (31st edition), Hong Kong, Nov 4-8 2025; 17.1% acceptance rate — the top-tier mobile computing conference alongside MobiSys.
- First author; advisor Wei Gao; University of Pittsburgh.
- First work to identify and tackle the correlation between device delays and data heterogeneity in FL — a novel problem formulation, not just a new method.
- Real hardware deployment: 6 smartphone models (Pixel XL/2/4/5/7, LG G5) + Raspberry Pi 4B + NVIDIA Jetson Nano, implemented on top of the Flower FL framework with an RTX A5000 server over WiFi.
- Three desirable properties pitch well together: (i) no extra on-device compute/communication, (ii) no auxiliary dataset and works at any FL stage, (iii) privacy-preserving (cannot recover samples or labels).
- Funded by NSF IIS-2205360, CCF-2217003, CCF-2215042 and NIH R01HL170368 — signals a well-resourced, high-impact research line.
- Natural companion to the AAAI 2025 paper on intertwined data/device heterogeneity in FL under unlimited staleness — shows a sustained research agenda on federated systems.
