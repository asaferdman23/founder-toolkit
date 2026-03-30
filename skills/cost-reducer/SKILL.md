---
name: cost-reducer
description: Use when reviewing infrastructure costs, optimizing API spend, reducing cloud bills, or before adding new paid service integrations. Triggers on high OpenAI/Twilio/Supabase usage, DALL-E generation, embedding indexing, or storage egress concerns.
---

# ProduceGerX Cost Reducer

## Overview
Systematic cost analysis and optimization for ProduceGerX's paid services. OpenAI (GPT-4o + DALL-E 3 + embeddings), Twilio, Supabase, Stripe, Clerk, Redis, and Railway.

## When to Use
- Adding or modifying OpenAI/DALL-E/embedding calls
- Adding Twilio messaging flows
- Reviewing monthly cloud spend
- Before scaling to more users
- Optimizing database queries or storage

## Quick Reference: Monthly Cost Estimate

| Service | Low (MVP) | Mid (100 users) | High (1K+) |
|---------|-----------|-----------------|-------------|
| OpenAI (GPT + DALL-E + embeddings) | $200 | $500 | $2,000+ |
| Twilio (WhatsApp + SMS) | $50 | $200 | $500+ |
| Supabase (DB + storage) | $25 | $100 | $300+ |
| Stripe (2.2% + $0.20/tx) | $0 | $100 | $500+ |
| Clerk (auth) | $25 | $50 | $100+ |
| Redis + Railway | $17 | $35 | $100+ |
| **TOTAL** | **$317** | **$985** | **$3,500+** |

**Biggest levers:** OpenAI (DALL-E images + embeddings) and Supabase storage egress.

## Cost Analysis Workflow

1. Identify which paid service the change touches
2. Read the relevant sub-file for specific optimizations
3. Check if caching/batching can reduce API calls
4. Estimate cost impact before implementing

## Sub-Files
- **cloud-and-infra.md** — Railway, Redis, Supabase infrastructure optimization
- **code-level-savings.md** — OpenAI, DALL-E, embedding, and Twilio call optimization
- **services-and-finops.md** — Service tier selection, billing alerts, cost monitoring
