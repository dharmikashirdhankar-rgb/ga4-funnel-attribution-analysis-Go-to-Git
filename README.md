# GA4 Marketing Funnel & Attribution Analysis

**Dataset:** Google Analytics 4 Demo Account — Google Merchandise Store (public e-commerce property)
**Tools used:** Google Analytics 4 (Explorations, Advertising → Attribution), Looker Studio
**Live dashboard:** [Insert your shared Looker Studio link here]

![Full Dashboard](screenshots/dashboard-full.png)

---

## The Question

Which marketing channels are actually driving conversions — and which ones are being undervalued by standard last-click reporting?

Most teams default to last-click attribution because it's simple, but it only rewards whichever channel happened to be clicked right before a purchase, ignoring everything that led up to it. This project set out to test whether that default view was hiding real performance from certain channels.

---

## Method

I used the public GA4 demo account (Google Merchandise Store) to build a purchase funnel and segment it by channel using GA4 Explorations. I then compared Last Click vs Data-Driven attribution models in GA4's Advertising → Attribution report to see where they diverged, and pulled channel-level session, conversion, and conversion-rate data into a Looker Studio dashboard to visualize the findings.

---

## Key Findings

### 1. Steep drop-off happens early in the funnel, not at checkout

| Stage | Users | % of previous stage |
|---|---|---|
| Session Start | 90,135 | — |
| View Item | 19,324 | 21.4% |
| Add to Cart | 4,457 | 23.1% |
| Begin Checkout | 2,250 | 50.5% |
| Purchase | 1,137 | 50.5% |

The single biggest drop-off is between **Session Start and View Item** — nearly 4 in 5 users never even view a product. Once someone reaches checkout, they convert to purchase roughly half the time, which is actually a fairly healthy completion rate. This suggests the bigger opportunity isn't fixing checkout friction — it's improving product discoverability and landing page relevance so more sessions turn into product views in the first place.

### 2. Referral traffic is undervalued by last-click attribution

| Channel | Last Click Revenue | Data-Driven Revenue | Change |
|---|---|---|---|
| Referral | $8,416.14 | $10,483.86 | **+24.6%** |
| Paid Search | $6,077.35 | $6,861.29 | +12.9% |
| Direct | $113,128.37 | $113,128.37 | 0% (no upstream channel) |
| Organic Search | $71,394.19 | $68,634.80 | −3.9% |

When comparing GA4's Data-Driven attribution model (which distributes credit across a user's full path) against Last Click, **referral traffic is undervalued by nearly 25%**. This means referral is playing a meaningful assisting role in conversions — bringing users back or nudging them along their path — that a last-click-only view completely misses.

### 3. Why the attribution models disagree: Referral shows up early and mid-journey nearly twice as often as average

GA4's Key Event Attribution Paths report breaks every touchpoint across all conversion paths into three buckets — Early, Mid, and Late (i.e. the final touch before conversion) — and shows exactly where each channel shows up:

| Stage | All Channels (avg. share) | Referral (share of its own touchpoints) |
|---|---|---|
| Early | 5.68% | 14.0% (692.39 of 4,943.32 credits) |
| Mid | 4.00% | 3.9% (190.38 of 4,943.32 credits) |
| Late | 90.32% | 82.1% (4,060.55 of 4,943.32 credits) |

Combining Early + Mid: **17.9% of Referral's touchpoints occur before the final click, compared to a 9.68% average across all channels** — nearly double. This is the direct mechanical explanation for the attribution divergence in Finding 2: Last Click attribution only ever counts a channel's final touchpoint, so it structurally misses Referral's above-average role in initiating or assisting a conversion earlier in the journey. Data-Driven attribution counts all of it, which is exactly why it credits Referral 24.6% more revenue than Last Click does.

### 4. Referral also converts well on its own merits

| Channel | Sessions | Purchases | Conversion Rate |
|---|---|---|---|
| Organic Video | 57 | 3 | 5.26%* |
| Organic Shopping | 469 | 13 | 2.77% |
| **Referral** | **1,734** | **46** | **2.65%** |
| Organic Social | 890 | 11 | 1.24% |
| Organic Search | 26,438 | 294 | 1.11% |
| Cross-network | 1,016 | 11 | 1.08% |
| Direct | 62,733 | 600 | 0.96% |
| Unassigned | 6,069 | 39 | 0.64% |
| Paid Search | 10,387 | 42 | 0.40% |

*Organic Video's 5.26% rate is based on a very small sample (57 sessions) and shouldn't be read as a reliable signal.

Excluding low-volume outliers, **Referral is the strongest-converting channel at meaningful scale** — nearly 2.8x the conversion rate of Direct traffic, despite Direct driving far more raw revenue overall. This corroborates both prior findings: Referral isn't just under-credited in attribution reporting, it's genuinely a high-quality channel on a per-session basis too.

By contrast, **Direct and Paid Search drive high session volume but convert relatively poorly per session** (0.96% and 0.40%), suggesting volume and channel quality aren't the same thing.

---

## Recommendation

If I were advising a marketing team on this data, I'd suggest:

1. **Don't evaluate referral partnerships using last-click numbers alone.** Its true contribution is roughly 25% higher than last-click reporting shows — because it plays an early/mid-journey role nearly twice as often as the average channel — and it independently converts at a materially higher rate than Direct or Paid Search. Referral is a strong candidate for further investment (e.g., expanding partner/affiliate relationships) that a last-click-only dashboard would likely miss.
2. **Focus funnel optimization efforts upstream, not at checkout.** The largest single drop-off happens between landing (Session Start) and product view — the checkout-to-purchase step is already reasonably efficient. Improving product recommendations, search relevance, or landing page targeting would likely have more impact than checkout UX changes.
3. **Re-evaluate Paid Search spend efficiency.** It drives meaningful volume (10,387 sessions) but the lowest conversion rate of any channel with real scale (0.40%) — worth investigating whether targeting or landing page match is off.

---

## Notes on the data

This analysis uses GA4's public demo e-commerce dataset, not live business data, so findings are illustrative of the *method* rather than actionable for a real company. The same approach (funnel exploration → channel segmentation → attribution model comparison) applies directly to any GA4 property.

---

## Repo Structure

```
ga4-funnel-attribution-analysis/
├── README.md
└── screenshots/
    └── dashboard-full.png
```
