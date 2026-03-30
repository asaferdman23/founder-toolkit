# Cloud & Infrastructure Cost Optimization

## Supabase Storage

**Problem:** Images served directly from Supabase — no CDN, high egress costs.

**Current buckets:** `concept-images`, `floor-plan-venues`, supplier catalogs, contracts

**Optimizations:**
1. **Add CDN** — Cloudflare R2 ($0.015/GB, free egress) or CloudFront in front of Supabase Storage
2. **Compress images** — DALL-E generates 1024x1024 PNGs; convert to WebP (60-80% smaller)
3. **Set cache headers** — `Cache-Control: public, max-age=86400` on concept images (they don't change)
4. **Lazy-load thumbnails** — Generate 256px thumbnails for gallery views, full-size on click

**Estimated savings:** 40-60% on storage egress at scale

## Railway Hosting

**Current:** Single dyno for API + WebSocket server

**Optimizations:**
1. **Right-size containers** — Monitor actual RAM/CPU usage via Railway metrics; don't over-provision
2. **Sleep on inactivity** — Use Railway's sleep feature for staging environments
3. **Separate WebSocket** — If WS traffic grows, split to dedicated service to scale independently

## Redis

**Current:** Used ONLY for BullMQ job queues (not caching)

**Optimizations:**
1. **Use same Redis for caching** — Already paying for Redis; add data caching layer (supplier lists, event summaries)
2. **Set maxmemory policy** — `allkeys-lru` to auto-evict stale cache entries
3. **Monitor memory** — BullMQ completed jobs accumulate; set `removeOnComplete: { count: 100 }` on queues

## Supabase Database

**Current plan:** Pro ($25/month base)

**Optimizations:**
1. **Connection pooling** — Use Supabase's built-in PgBouncer (already available)
2. **Index hot queries** — Add indexes on `event_id`, `organization_id`, `supplier_id` columns used in WHERE clauses
3. **Archive old data** — Move completed events older than 6 months to archive table
4. **Monitor query performance** — Enable Supabase Query Performance dashboard
