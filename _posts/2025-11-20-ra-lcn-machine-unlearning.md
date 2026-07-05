---
layout: post
title: "RA-LCN: Teaching Generative Models to Forget, Retain-Aware"
date: 2025-11-20 10:00:00+1300
description: a retraining-free machine unlearning framework, co-developed with Austin Fu
tags: machine-unlearning ai-safety generative-models
categories: research
related_posts: false
---

The right to be forgotten, privacy regulation, and copyright protection all demand something generative models were never designed to do: remove a concept after training. With Austin Fu, I co-developed Retain-Aware Latent Concept Nulling (RA-LCN) — a machine unlearning framework that needs no retraining at all. Manuscript in preparation; here's the shape of it.

Unlearning in classifiers is relatively tame. Generative models are harder because diffusion architectures, GANs and VAEs "encode concepts in highly entangled latent representations." A concept isn't stored anywhere you can delete it — it's smeared across latent space, tangled with everything else. My contribution began with naming the two failure modes that plague existing approaches. **Boundary ambiguity:** forget and retain concepts share semantic subspaces — dragons and heroes share narrative structure; there's no clean cut. **Erasure creep:** aggressive removal degrades retained capabilities that depended on shared directions — erase "dragon" and your mountains and castles quietly suffer. Both stem from treating unlearning as parameter surgery. Our reframe: treat the latent space "as a continuous statistical manifold rather than a flat vector space," and make unlearning a problem of geometry.

The mechanics, in six steps: encode the forget set and retain set with the frozen encoder; compute the forget covariance and eigendecompose it —

$$ C_f = \frac{1}{N_f}\sum_i (z_{f_i} - \mu_f)(z_{f_i} - \mu_f)^{T} \;\longrightarrow\; \text{top-}k \text{ eigenvectors } U_f $$

— whose top eigenvectors span the forget subspace, the principal directions of the concept to be removed. Then compute the retain second-moment matrix \\( M = \frac{1}{N_r}\sum_i z_{r_i} z_{r_i}^{T} \\), which maps where retained knowledge lives. The projector fuses both:

$$ P = I - U_f\,(U_f^{T} M\, U_f)^{-1} U_f^{T} M, \qquad z' = Pz $$

The Mahalanobis-style middle term is where retain-awareness lives: each forget direction is rescaled by its overlap with retain structure — directions the retained data doesn't need are removed outright; shared directions are suppressed gently. That term is the direct answer to erasure creep. At inference, unlearning is a single matrix multiply with every weight frozen: encode, project, decode. Model-agnostic, no gradients, no retraining.

We validated across three modalities. GPT-2 (text-to-text): forgetting "dragon" dropped concept alignment from 0.30 to 0.06 with fluency intact. Stable Diffusion v1.5 (text-to-image): forget-set FID rose from 0 to 391 — "photo of a dragon" now renders abstract terrain — while retain-set FID actually improved, 453 to 76. Image-to-image: the strongest forget-domain divergence among all baselines compared (FID-F 288).

We're equally direct about limits: RA-LCN "may not guarantee permanent forgetting" — indirect prompts may re-elicit a suppressed concept — and the current projection is one-shot, linear, global. Future work runs toward layer-wise and nonlinear extensions, and toward the evaluation I care most about: adversarial re-activation probes, because a forgetting claim you can't attack is a claim you can't trust.

Why it matters: removing a capability from a deployed model without retraining it is safety infrastructure — for privacy takedowns, copyright compliance, and dangerous-capability removal alike. Austin led the experimental programme across the three model families; I contributed the conceptual framework, its literature grounding, and the retain-aware projection design. Code: [github.com/AustinAllen/RA-LCN-on-Machine-Unlearning](https://github.com/AustinAllen/RA-LCN-on-Machine-Unlearning).
