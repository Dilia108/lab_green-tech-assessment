# Use Case - Path A
---

## 🛍️ Use Case: "ClearCart" — AI-Powered Product Image Moderation for an E-commerce Marketplace

**Who:** ClearCart is a mid-sized European online marketplace (think a regional alternative to eBay) with ~50,000 active sellers uploading product listings daily.

**What workflow:** Every time a seller uploads a product image, an AI vision model checks it automatically for policy violations — prohibited items, explicit content, counterfeit brand logos, and low-quality/blurry images. Images that pass go live immediately; flagged ones enter a human review queue.

**Which model/API:** They call a third-party vision model API (e.g. a GPT-4o-mini vision or similar) synchronously on every upload, one API call per image, with no caching and no batching. The model runs in a US-East region (AWS us-east-1 in Virginia) even though most sellers and the company HQ are in Germany.

---

## Your "One Unit of Value" sentence (Slide 2):

> *"One unit of value for this system is: one product image moderated to a policy decision (pass or flag)."*

---

## Starter Lever Table (for your speaker notes):

| Lever | Current assumption (honest guess) | Better alternative to explore |
|---|---|---|
| Model / size | Large multimodal model called for every image, including clearly benign ones | Route simple/low-risk images to a smaller, cheaper classifier first; only escalate ambiguous ones to the large model |
| Call pattern (batch, cache, sync) | Synchronous call per image, no batching, no caching of repeated/identical images | Batch low-priority uploads; cache embeddings or decisions for near-duplicate images (same seller, same SKU) |
| Infra / region / retention | API calls routed to US-East; image data may be stored indefinitely | Switch to EU region (lower grid carbon intensity); set retention policy to purge moderation metadata after 90 days |

---