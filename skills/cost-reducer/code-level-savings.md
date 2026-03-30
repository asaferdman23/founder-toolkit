# Code-Level Cost Savings

## OpenAI GPT-4o — AI Planning Chat

**File:** `backend/src/services/ai.service.ts`
**Cost:** ~$15/1M input tokens, $60/1M output tokens

**Optimizations:**
1. **Use gpt-4o-mini for simple tasks** — Planning scope extraction, category matching don't need full GPT-4o
   ```typescript
   // For simple classification tasks
   model: 'gpt-4o-mini'  // 10x cheaper than gpt-4o
   ```
2. **Reduce max_tokens** — Current: 1200. Many responses are <500 tokens; set per-task limits
3. **Cache repeated queries** — Same event scope questions get similar answers; Redis cache with 1hr TTL
4. **Trim conversation history** — Only send last 5 messages as context, not full history

**Estimated savings:** 50-70% on GPT costs by using mini + caching

## DALL-E 3 — Concept Images

**File:** `backend/src/services/concept-image.service.ts`
**Cost:** $0.08/image (1024x1024 standard)

**Optimizations:**
1. **Limit generations per event** — Currently generates 5 areas (entrance, stage, seating, bar, decor). Allow 2 free, charge for more
2. **Use 512x512 for previews** — Half the cost, upscale only on export
3. **Cache by prompt hash** — Similar events produce similar concepts; deduplicate
4. **Rate limit per org** — Max 10 images/day per organization
5. **Queue generation** — Use BullMQ instead of on-demand; batch during off-peak

**Estimated savings:** 60-80% by limiting + caching

## Embeddings — RAG System

**File:** `backend/src/services/embedding.service.ts`
**Cost:** $0.02/1M tokens (text-embedding-3-small)

**Optimizations:**
1. **Only re-embed on change** — Track content hash; skip if supplier data unchanged
   ```typescript
   const contentHash = crypto.createHash('md5').update(JSON.stringify(data)).digest('hex');
   if (existing?.content_hash === contentHash) return; // Skip
   ```
2. **Batch API calls** — Current: 1 call per supplier. OpenAI supports batching up to 2048 inputs
3. **Consolidate tables** — 3 separate tables (supplier, event, negotiation). Single table with `type` column reduces indexing overhead

**Estimated savings:** 80-90% by deduplication + batching

## Twilio — WhatsApp/SMS

**File:** `backend/src/services/messaging/index.ts`
**Cost:** ~$0.01-0.05 per WhatsApp message

**Optimizations:**
1. **Consolidate messages** — Don't send 3 messages when 1 with sections works
2. **Use templates wisely** — Pre-approved templates are cheaper than freeform
3. **Debounce notifications** — Multiple supplier updates within 5min = 1 message
4. **Mock mode for dev** — Already has `MESSAGING_MODE=mock` (good! keep using it)
5. **Track message costs** — Log `message.price` from Twilio callback for visibility

## General Patterns

**Before adding ANY new API call, ask:**
1. Can this be cached? (Redis, 1hr TTL default)
2. Can this be batched? (Queue + process in bulk)
3. Can a cheaper model do this? (gpt-4o-mini vs gpt-4o)
4. Can this be rate-limited per org/user?
5. Can this run async? (BullMQ worker instead of request handler)
