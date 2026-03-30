# Founder Toolkit — Developer Guide

## What This Is

A public Claude plugin containing 8 founder-focused skills for business strategy, financial modeling, and go-to-market execution.

The plugin follows the Claude plugin specification and is installed via:
```bash
/plugin marketplace add asaferdman23/founder-toolkit
```

## Architecture

```
founder-toolkit/
├── .claude-plugin/
│   ├── plugin.json         ← Install-facing metadata
│   └── marketplace.json    ← Registry-facing metadata
├── commands/               ← User entry points (8 commands)
│   ├── cofounder.md       → Router to sub-skills
│   ├── cofounder-roi.md   → Unit economics
│   ├── cofounder-market.md → Market analysis
│   ├── cofounder-gtm.md   → Go-to-market
│   ├── cofounder-pitch.md → Pitch preparation
│   ├── investor.md        → Fundraising
│   ├── cost-reducer.md    → Cost optimization
│   └── social-marketing.md → Social campaigns
├── skills/                ← Skill implementations (8 skills)
│   ├── cofounder/
│   ├── cofounder-roi-calculator/
│   ├── cofounder-market-intel/
│   ├── cofounder-gtm-playbook/
│   ├── cofounder-pitch-prep/
│   ├── investor-agent/
│   ├── cost-reducer/
│   └── social-marketing/
├── index.html             ← GitHub Pages landing page
├── CNAME                  ← Custom domain
├── LICENSE                ← MIT
├── README.md              ← User guide
└── CLAUDE.md              ← This file

## Design Patterns

### 1. Command Pattern
Each command is a thin wrapper that delegates to a skill:
```markdown
---
description: [What the skill does]
argument-hint: [Example arguments]
allowed-tools: [Read, Write, Glob, Grep, WebSearch, WebFetch, Agent]
---

Invoke the `skill-name` skill to handle this request.

The user said: $ARGUMENTS
```

**Key principle:** Commands are entry points only. Skills contain all logic.

### 2. Routing in the Cofounder Skill
The main `cofounder` skill uses a decision table to route questions:
- ROI/pricing questions → `cofounder-roi-calculator`
- Market/segment questions → `cofounder-market-intel`
- GTM/channel questions → `cofounder-gtm-playbook`
- Pitch/meeting questions → `cofounder-pitch-prep`

If ambiguous, it asks one clarifying question before routing.

### 3. Memory Pattern
Skills use file-based memory in `~/.claude/projects/*/memory/` to persist:
- Founder's hourly rate / opportunity cost
- Business metrics (CAC, LTV, pricing, runway)
- Customer/market data
- Past decision outcomes

This enables skills to improve with use and avoid repeated data entry.

### 4. Structured Output
All financial/analytical outputs are tables with:
- **Assumptions** stated explicitly at the top
- **Key metrics** in bold
- **Interpretation** in plain English
- **Next actions** (what to do Monday morning)

## Skill Details

### cofounder
**File:** `skills/cofounder/SKILL.md`
**Role:** Business strategy router and generalist co-founder

Routes based on this logic:
- Specific numbers (CAC, LTV, price impact) → roi-calculator
- Segment/market strategy → market-intel
- GTM/channel/networking → gtm-playbook
- Meeting/pitch prep → pitch-prep
- Everything else → Answer directly as generalist

### cofounder-roi-calculator
**File:** `skills/cofounder-roi-calculator/SKILL.md`
**Role:** Financial modeling and unit economics

Handles:
- CAC and LTV calculations
- Break-even and runway analysis
- Pricing sensitivity ("what if we charge X?")
- Client/segment ROI models
- Cash flow projections

### cofounder-market-intel
**File:** `skills/cofounder-market-intel/SKILL.md`
**Role:** Market analysis and competitive intelligence

Handles:
- Customer segment analysis and scoring
- TAM sizing and opportunity identification
- Competitive landscape and positioning
- Pricing strategy
- Go-to-market channel prioritization

### cofounder-gtm-playbook
**File:** `skills/cofounder-gtm-playbook/SKILL.md`
**Role:** Go-to-market and growth strategy

Handles:
- Networking strategy and conference selection
- LinkedIn strategy and content themes
- Content marketing and awareness
- Partnerships and strategic alliances
- Cold outreach and inbound marketing

### cofounder-pitch-prep
**File:** `skills/cofounder-pitch-prep/SKILL.md`
**Role:** Meeting and pitch preparation

Handles:
- Client research and decision-maker profiling
- ROI value propositions
- Objection handling
- Pricing/contract proposals
- Full meeting scripts with talking points
- Follow-up strategy

### investor-agent
**File:** `skills/investor-agent/SKILL.md`
**Role:** Fundraising strategy and materials

Handles:
- Pitch deck structure and content
- Financial model templates
- Fundraising strategy
- Valuation and term sheet analysis
- Investor positioning

### cost-reducer
**File:** `skills/cost-reducer/SKILL.md`
**Role:** Cost optimization and spending review

Handles:
- Infrastructure cost analysis
- API spend optimization
- Vendor negotiation
- Cost-benefit for new services

### social-marketing
**File:** `skills/social-marketing/SKILL.md`
**Role:** Social media and content marketing

Handles:
- Content calendars
- Post writing and copywriting
- Campaign planning
- Growth strategy
- Analytics and performance tracking

## Installation & Testing

### Local Installation
```bash
# Build plugin structure locally
mkdir -p ~/test-founder-toolkit/.claude-plugin
cp -r ./* ~/test-founder-toolkit/

# Install in Claude Code
/plugin marketplace add ~/test-founder-toolkit
```

### Testing
1. Try `/cofounder what should our pricing be?` (should route to roi-calculator)
2. Try `/cofounder-roi` directly (should work independently)
3. Verify all 8 commands are available
4. Test memory persistence (save data, check it's available in next conversation)

## Publishing to GitHub

```bash
# Create repo
gh repo create founder-toolkit --public --source=. --remote=origin --push

# Enable GitHub Pages
gh repo edit --enable-wiki=false
# Then in GitHub: Settings > Pages > Source: main branch / root folder
```

Add custom domain:
- Create CNAME file with `cofounder.cc`
- Update DNS: `cofounder.cc` CNAME `asaferdman23.github.io`

## Updating & Versioning

### Minor Updates (1.0.1)
- Bug fixes in skills
- Command tweaks
- No breaking changes

```bash
git tag v1.0.1
git push --tags
```

### Major Updates (1.1.0)
- New skills
- New commands
- Breaking changes

Update version in:
- `.claude-plugin/plugin.json` → `version`
- `.claude-plugin/marketplace.json` → `version` (in both places)

## Contributing

Contributions welcome! Fork and PR with:
- Clear description of change
- Updated SKILL.md if skill logic changed
- Test scenarios if new routing added

## License

MIT © 2026 Asaf Erdman

## Contact

- GitHub: [@asaferdman23](https://github.com/asaferdman23)
- Twitter/X: @asaferdman
- Email: [your email]
