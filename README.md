# LAB | Defend your stack's carbon story

**Author:** Dilia Navarro

## Green Tech Assessment — ClearCart

A Green Software Foundation (GSF) stakeholder assessment for **ClearCart**, a mid-sized European e-commerce marketplace running an AI-powered product image moderation pipeline.

**Audience:** CTO / Head of Engineering  
**Framework:** [Green Software Foundation — SCI & Patterns Catalog](https://patterns.greensoftware.foundation/)

---

## Project Files

| File | Description |
|---|---|
| `Use_Case-Path_A.md` | Use case brief: ClearCart's moderation workflow, the "one unit of value" definition, and the starter lever table |
| `ClearCart_GreenTech_Assessment.pptx` | Full stakeholder presentation (14 slides) |

---

## Use Case Summary

**ClearCart** has ~50,000 active sellers uploading product images daily. Every image upload triggers an automatic AI vision model check for policy violations (prohibited items, explicit content, counterfeit logos, low quality). Images that pass go live immediately; flagged ones enter a human review queue.

**One unit of value (R):** one product image moderated to a policy decision (pass or flag).

**Current inefficiencies identified:**

- Large multimodal model (GPT-4o-class) called for every image, including trivially safe ones
- Synchronous, one-call-per-image — no batching, no caching
- API routed to US-East (Virginia) despite sellers and HQ being in Germany
- No deduplication — identical re-uploaded images trigger new API calls each time

---

## Presentation Structure

| Slide | Content |
|---|---|
| 1 | Title |
| 2 | The Hook — scale and cost of the problem |
| 3 | One Unit of Value (R) and SCI formula |
| 4 | **"Why should we believe this?"** — evidence tiers and commitment to verify |
| 5 | Assessment — where the compute concentrates (4 problem areas + GSF pillars) |
| 6 | Honest Assumptions & Confidence Levels |
| 7 | GSF Pillars Map (Carbon, Energy, Hardware, Measurement) |
| 8 | Solution 1 — Model Cascade (Tiered Routing) |
| 9 | Solution 2 — Perceptual Hashing & Decision Caching |
| 10 | Solution 3 — EU Region Routing + Off-Peak Batching |
| 11 | Solution 4 — Instrument Before You Optimise |
| 12 | Measurement Plan: 2-Week Sprint |
| 13 | Caveats & What We Will Not Claim |
| 14 | Before / After Hypothesis Table |

---

## GSF Pattern Mapping

| Solution | GSF Category | Pattern |
|---|---|---|
| Model Cascade (tiered routing) | **Architecture** | Optimize the size of AI/ML models + Queue non-urgent processing requests |
| Perceptual Hashing & Decision Caching | **Development** | Cache static data |
| EU Region Routing | **Architecture** | Choose the region closest to users |
| Off-Peak Batching | **Operations** | Time-shift workloads / Scale infrastructure with user load |
| Instrumentation & SCI Baseline | **Operations** | Measurement baseline |

---

## Key Recommendations

1. **Model Cascade** — route trivially safe images to a lightweight classifier first; only escalate ambiguous ones to the large model. Estimated cost reduction: ~75% per image.
2. **Perceptual Hash Deduplication** — compute a hash on upload and skip the API call entirely if the image was seen before. Estimated call reduction: 15–20% in month 1.
3. **EU Region Switch** — move API routing from US-East (~400 gCO₂/kWh) to EU-West (~200 gCO₂/kWh) for ~50% carbon reduction per call. Low-carbon target: AWS eu-north-1 (Sweden).
4. **Off-Peak Batching** — defer non-urgent uploads to a 2–6 am batch window; group into 50-image batches for better hardware utilisation and potential 30%+ pricing discount.

> All recommendations are falsifiable. No change goes live until Week 1 measurement data confirms or revises the assumption behind it.

---

## Caveats

- No "carbon neutral" claim is made. All carbon figures are estimates using API cost as an energy proxy.
- Duplicate image rate (15–20%) is an industry benchmark, unconfirmed for ClearCart.
- Model cascade requires precision validation on ClearCart's own image corpus before rollout.
- EU region switch needs legal sign-off on GDPR / data residency before deployment.

---

## References

- [Green Software Foundation Patterns Catalog](https://patterns.greensoftware.foundation/)
- [GSF Software Carbon Intensity (SCI) Specification](https://sci.greensoftware.foundation/)
