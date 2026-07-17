---
layout: post
title: "Watching the System Think"
date: 2026-07-18 02:30:00+1200
description: a look inside the layered, tiered architecture behind the platform — and what it looks like watching it run live
tags: nlp deep-learning osint rag
categories: systems
related_posts: true
published: false
---

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/hasti_corpus_hero.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

I wrote about [the platform](/blog/2026/osint-platform-introduction/) a couple weeks back — the short version, the pitch. This one's the long version.

Here's the honest pitch, the one I actually give people: I've basically stopped consuming geopolitical analysis on YouTube. Instead I've got an array of journalists, authors, and commentators my system crawls for directly — a corpus of their actual knowledge, transcribed, translated, analyzed, turned into knowledge graphs. Saved me an enormous amount of time. For a news junkie, this isn't news-wire aggregation, it's geopolitical analysis compounding on intellectual minds — YouTube to Substack to synthesized analysis, a layered, tiered living-minds architecture with a 10-stage query pipeline and model triage underneath it. That's the whole thing, really. Everything below is just how.

## It's not one model. It's a hierarchy of them.

The easiest way to get this wrong is to imagine one big model that "knows about" 38 analysts. That's not it. Every analyst is their own corpus, their own retrieval space, their own voice — but they're not all treated equally, and that's on purpose.

**Tiers** decide who anchors the panel. T1 minds are the ones I've assessed as substantively dense enough to carry real analytical weight — right now that's a dozen names, the people whose corpora are deep and whose positions matter. T2 is calibration-phase: still useful, still queryable, but not yet trusted to anchor a synthesis on their own — maybe the corpus is thinner, maybe the voice is too conversational to extract clean signal from yet. T0 is a different category entirely — reference-layer, technical consultants rather than intel voices at all.

**Layers** decide how grounded a T1 voice actually is before it's allowed to extrapolate. L1 is corpus sufficiency — do we actually have enough of this person's own words to ground a take at all. L2 is a prior — a structured sense of their intellectual formation, what shaped how they think, distinct from what they've said recently. L3 is the one I actually care most about: an epistemic floor — a documented answer to "how does this person actually reason," not just "what have they said." Right now, most T1 minds don't have an L3 floor built yet. The ones that do get to anchor panel synthesis with real confidence. The ones that don't are T1-provisional — present, but the platform knows the difference, and so do I.

That's the part that took the longest to get right, and it's also the part I think actually matters: a system with 38 voices is not automatically more trustworthy than one. It's only more trustworthy if you can say, honestly, which voices you actually trust yet, and why.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/hasti_layer_tier_architecture.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    The layer/tier model laid out. Five layers per mind — corpus, worldview prior, network, unpublished, evolution — each with an honest build status, not a claimed one. Most T1 minds are still corpus-plus-prior; the graph and evolution layers are deliberately marked not-built rather than faked.
</div>

## What happens when you ask it something

Here's the part people usually assume is simpler than it is. A query doesn't just get embedded and matched. It goes through ten stages:

1. **Plan** — figure out which minds are actually relevant to this question, not just which minds exist.
2. **Retrieve** — hybrid search per mind, dense embeddings for meaning plus lexical scoring for the specific terms that anchor it, so a query doesn't drift semantically away from what someone actually said.
3. **Compress** — if the retrieved passages are too much to reason over cleanly, summarize before synthesis, not after.
4. **Synthesize** — a per-mind voiced take, tagged with exactly how grounded it is: corpus-grounded, thin-but-real, or extrapolated from prior when the corpus doesn't cover it — and it says so.
5. **Tag provenance** — that grounding label gets attached at the point of synthesis, not bolted on after, so the take and its receipt are never separable.
6. **Requery** — if a mind's corpus came back thin, try again before giving up on them.
7. **Check the graph** — where does this mind actually stand relative to the others on record, not just what did retrieval happen to surface this time.
8. **Convene the panel** — this is the synthesis step: where do the voices converge, where's the real tension, who's the outlier worth flagging.
9. **Manifest the provenance** — the panel-level receipt: every take carries how grounded it was, specifically, all the way up to the synthesized brief.
10. **Log it** — claims get registered so they can be checked against how things actually unfold later. This is the part that turns "confident-sounding system" into "system that can be shown to be right or wrong."

That's the actual shape of it. Not one call to one model — a pipeline where every stage exists because skipping it produced a real failure mode at some point.

## Watching it, instead of just querying it

All of that runs autonomously too — not just when I ask it something, but continuously, watching sources, deciding what's actually novel, flagging what a desk should look at. For a long time that only lived in a database, queryable but not watchable. Then I built a way to actually watch it happen.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/kg_live_triage_wire.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    The live geo-operations map, mid-cycle.
</div>

There are three things actually happening in that one screenshot, and they're worth separating out, because together they're the whole point.

**The map itself is a rotating, evolving picture of relevance, not a fixed diagram.** The window at the top (6h, 24h, 72h) controls how far back it's looking, and within that window, analysts on the left rail glow and fade based on how relevant they are *right now* — a mind that was hot yesterday can drop off the rail entirely today if nothing new is touching their beat, and someone quiet for a week can suddenly light up because a story just landed in their lane. Same on the map itself: countries and entities light up as events actually touch them — Oman here, freshly lit by ten events, with the tooltip showing exactly which headline put it there. Nothing on this map is permanent. It's a snapshot of where the narrative battlefield currently is, and it reshapes itself every time new signal comes in.

**The right-hand wire is the wire, not a summary of it.** Every item there is a real story that's been through the triage pipeline — vetted, scored, attributed to a desk — landing in that list the moment it clears the bar, tagged with how many edges it lit up in the knowledge graph on the way in. Watching the map "glow" and watching this list update are the same event seen from two angles: one is the geography, one is the actual text.

**The bottom half is the intel shop's own floor, narrated.** This is the newest piece — a plain-language feed of the triage decisions themselves, in something close to real time: a sweep comes in and surfaces some number of fresh items, a desk crosses its significance threshold and a memo gets cut, and when a story is significant enough, a full case-officer report gets filed, not just a memo. It's deliberately not a raw log. The goal was to make the autonomous system's own reasoning legible to someone who isn't going to go read the database — sitting in on the floor instead of auditing the schema.

## The other button: dispatches

The map has a second mode. Where the command line narrates the shop's internal triage, the dispatches panel is the finished product — the actual synthesized reports the platform produces off the back of everything above.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/kg_live_dispatches_view.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    The dispatches panel open on a Fulcrum report, with the live map and wire still running alongside it.
</div>

These are select, deep intelligence products — synthesized across the panel of voices on a specific evolving geopolitical situation, not a running commentary on every story that crosses the wire. Where the command line tells you a memo got cut, this is what the memo actually says once it's been through the full synthesis pipeline: convergence, tension, the outlier take, and a provenance manifest tracing every claim back to a specific voice. The wire is the raw material. This is what the shop actually produces from it.

Look — I built the system to have a deep analytical engine to analyze with, on a belief that AI can help me discover new intelligence as I look across the chess board.
