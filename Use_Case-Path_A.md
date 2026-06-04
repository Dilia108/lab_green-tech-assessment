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
| **Model / Size** | A large, powerful AI model is called for every single image upload — even for clearly harmless product photos | Route simple images to a smaller, cheaper model first and only escalate uncertain ones to the large model. Add a confidence threshold so that if the small model is very sure an image is clean, the large model is skipped entirely. Apply simple rule-based checks before any AI call (e.g. reject blurry or wrongly-sized images using basic code). Consider a smaller model fine-tuned on ClearCart's own violation history, which could be more accurate at a fraction of the energy cost |
| **Call Pattern** | One API call per image, processed immediately on upload, with no batching and no reuse of previous results | Batch low-priority uploads and cache decisions for near-duplicate images. Use image fingerprinting (hashing) to detect duplicate uploads before any AI is involved — same image gets the same decision with no new API call. Schedule non-urgent batches during off-peak hours when the energy grid is cleaner (e.g. overnight). Make async processing the default and reserve instant moderation for high-priority or premium sellers only |
| **Infra / Region / Retention** | API calls are routed to a US data center (Virginia), even though ClearCart and its sellers are based in Europe. Moderation data may be kept indefinitely | Switch to a European data center and set a 90-day retention policy for moderation metadata. Go further by choosing a low-carbon region specifically, such as Sweden (AWS eu-north-1), which runs largely on hydropower. If batching is adopted, use carbon-aware scheduling to route jobs to whichever region has the cleanest energy at that moment. Apply a retention policy to the images themselves, not just metadata. Trim API response payloads to only the pass/flag decision, cutting unnecessary data transfer |

---