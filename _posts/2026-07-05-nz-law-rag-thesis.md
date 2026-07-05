---
layout: post
title: "RAG for New Zealand Social Law: What My Thesis Found"
date: 2026-07-05 10:00:00+1200
description: driving through my Masters thesis — citation-aware retrieval over the Social Security Act 2018
tags: rag legal-ai evaluation
categories: research
related_posts: false
---

My Masters thesis asked a deceptively simple question: can a language model answer benefit-eligibility questions under New Zealand's Social Security Act 2018 without misleading anyone? The Act resists this. Its sections are long and interdependent, "expressed through definitions, exceptions, and cross-references that distribute legal meaning across multiple clauses" — and it exists in no AI-ready form. New Zealand has no legal NLP datasets, no supervised training resources, no benchmarks. So the research question became: how can retrieval design and document preprocessing improve grounding, citation fidelity, and hallucination reduction when applying LLMs to New Zealand social law?

I built the corpus first — structuring the raw legislation into hierarchical metadata (Part / Subpart / Section / Subsection) with human-readable citation identifiers — then ran five controlled experiments, all on local open-source models (LLaMA-3.2-3B-Instruct, all-MiniLM-L6-v2 embeddings), varying one design decision at a time.

The most instructive result was a failure. In Experiment 4, the pipeline had section reconstruction and chain-of-thought prompting, but retrieval was dense-only with raw L2 distance. Asked about Jobseeker Support on health grounds, the retriever surfaced Section 411 — rights of appeal — while Section 27, the provision that actually governs eligibility, never entered the context. The model then answered anyway, fluently and with citations:

> "Yes, you are eligible for Jobseeker Support if you cannot work due to health reasons… Citations: SSA 2018, Section 141(1)"
>
> — <cite>Experiment 4 output: zero retrieval precision, perfect legal register</cite>

That log is why "the model sounds confident" means nothing in high-stakes domains.

Experiment 5 changed only the representational layer. ChuLo Lite v2 — the section-aware semantic chunking method I developed — creates boundaries that isolate definitions, exceptions, and entitlement clauses in coherent spans. Retrieval became hybrid: cosine-normalised dense similarity plus a custom BM25. The signals proved complementary — dense similarity captured semantic alignment with the question, while lexical relevance reinforced the statutory terminology that anchors legal meaning: "medical certificate", "capacity for work". Section 27 came back, was reconstructed in full into the prompt, and the model's answer grounded itself in the actual legal mechanism. Retrieval precision moved from 0% to 33%, recall from 0% to 50%. Same generator; different representations.

The conclusion I stand behind, quoting the thesis directly: chunking, metadata hierarchy, hybrid retrieval and structured prompting "are not ancillary details but core determinants of legal fidelity… effective legal AI does not depend solely on larger models but on disciplined engineering of the representations that feed them."

I'm precise about the claim: I didn't ablate model scale, and a bigger model fed the wrong section would likely just fail more eloquently. The point is narrower and more actionable — in data-scarce, high-stakes domains, trustworthiness is won or lost in the retrieval layer, which is entirely within the engineer's control today.

The full pipeline, all five experiments and the output logs are open source under MIT: [github.com/msid876-commits/nz-law-rag-thesis](https://github.com/msid876-commits/nz-law-rag-thesis). Supervised by Dr. Qian Liu at the University of Auckland; First Class Honours, 2026.
