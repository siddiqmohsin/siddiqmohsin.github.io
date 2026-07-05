---
layout: post
title: "An Open-Source Intelligence Platform, Built From First Principles in NLP"
date: 2026-07-05 14:00:00+1200
description: introducing a multi-perspective analysis platform — evaluation, constraints, and controllability as an operating discipline
tags: nlp deep-learning osint rag
categories: systems
related_posts: false
---

For the past several months I've been designing and building — solo, end-to-end — a research platform for multi-perspective analysis of open-source geopolitical commentary. It began as a question left over from my thesis and my unlearning work: if segmentation, retrieval, and constraints decide whether one AI system can be trusted, what does it take to run dozens of them together, autonomously, and still trust the output?

The core idea is the panel. Instead of one model with one voice, the platform maintains 38 author-specific corpora — around 66,000 semantically chunked passages of published analysis, each corpus embedding a single analyst's body of work. A query doesn't retrieve from a pile; it convenes a panel. Each perspective answers from its own corpus through hybrid retrieval — dense embeddings for semantic alignment, lexical scoring for the terminology that anchors meaning — a design lifted directly from my thesis findings. A ten-stage synthesis pipeline then does what a good intelligence desk does: it surfaces where perspectives converge, where they tension, and which outliers deserve attention, with provenance manifests tracing every claim to its source passage.

The deep-learning and NLP stack runs entirely on local open-source models: sentence-transformer embeddings, transformer-based generation, NER-driven entity resolution feeding a triple store of several thousand confidence-weighted stance relations between analysts, actors, and ideologies. But the part I consider the actual research contribution is the control surface. Provenance-gated retrieval means synthetic content can never ground an output — the system cannot cite itself. Autonomous monitoring is threshold-bounded: the platform watches sources and flags events on its own, but its authority to act is explicitly capped. And a claim-level evidence register resolves analytical claims against how events actually unfold, compounding into per-analyst calibration scorecards — the panel is not just quoted, it is graded.

Evaluation isn't a phase here; it's the operating discipline. Every design decision descends from the same three words that organise all my research — evaluation, constraints, controllability — because an autonomous analysis system that cannot show its evidence, bound its own behaviour, and be scored against reality is not an analysis system. It's an opinion generator with good typography.

I'll write more about the individual mechanisms — the stance graph, the calibration scorecards, the bounded-autonomy design — in coming posts, along with architecture diagrams and sample outputs once they're cleared for publication. For now, the platform runs live, unsupervised, every day, on hardware I own. That was the point: trustworthiness you can operate, not just argue for.
