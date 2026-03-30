# Services & FinOps

## Service Tier Recommendations

| Service | Current Tier | Recommended | When to Upgrade |
|---------|-------------|-------------|-----------------|
| Supabase | Pro ($25/mo) | Stay Pro | >5GB storage or >50K rows |
| Clerk | Free/Pro | Free until 10K MAU | >500 MAU |
| Railway | Starter | Starter | >2 services or need HA |
| Redis | Railway add-on | Keep | >256MB memory usage |
| Stripe | Standard | Standard | Volume discounts at $80K+/mo |
| OpenAI | Pay-as-you-go | Pay-as-you-go | Batch API for bulk ops |
| Twilio | Pay-as-you-go | Pay-as-you-go | >10K msg/mo consider bundles |

## Cost Monitoring Checklist

### Weekly
- [ ] Check OpenAI usage dashboard (platform.openai.com/usage)
- [ ] Review Twilio message logs for unexpected spikes
- [ ] Monitor Railway resource metrics

### Monthly
- [ ] Supabase storage & egress report
- [ ] Stripe fee total vs revenue
- [ ] Clerk MAU count
- [ ] Total infrastructure spend vs budget

## Billing Alerts to Set Up

1. **OpenAI** — Set hard limit at $100/month initially (platform.openai.com/account/limits)
2. **Supabase** — Enable spend cap in project settings
3. **Railway** — Set usage limit in team settings
4. **Twilio** — Set account spend trigger at $50/month

## Cost-Per-Feature Estimates

| Feature | Primary Cost Driver | Est. Cost/Use |
|---------|-------------------|---------------|
| AI Planning Chat | GPT-4o tokens | $0.02-0.10/conversation |
| Concept Image Gen | DALL-E 3 | $0.08/image ($0.40/event) |
| Supplier Matching | Embeddings | $0.001/query |
| WhatsApp Onboarding | Twilio | $0.05-0.15/supplier |
| RFQ Follow-ups | Twilio | $0.01-0.05/message |
| Payment Processing | Stripe | 2.2% + $0.20/payment |
| Weekly Digest | Twilio | $0.01-0.05/user |

## Free Tier Maximization

- **Firebase FCM** — 1M push notifications/month FREE (currently using, stay)
- **Supabase Auth** — Not using (Clerk instead). If Clerk costs grow, evaluate switching
- **Vercel** — Frontend hosting free tier covers most needs
- **GitHub Actions** — Free for public repos; 2K min/month for private

## When Adding New Paid Services

**Decision framework:**
1. Is there a free alternative that works for <1K users?
2. What's the cost at 10x current usage?
3. Can we start on free tier and auto-upgrade?
4. Does it replace an existing cost? (net savings?)
5. Add to monthly monitoring checklist immediately
