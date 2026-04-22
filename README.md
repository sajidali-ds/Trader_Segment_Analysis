# Trader_Segment_Analysis
Analysis of crypto trader behavior and performance across Fear, Neutral, and Greed market conditions using the Fear &amp; Greed Index and on-chain trade data.

## Setup & How to Run

```bash
pip install pandas numpy matplotlib
jupyter notebook analysis.ipynb
# or: python analysis_script.py
```

Place both CSVs in the same directory as the notebook:
- `Share_fear_greed_index.csv`
- `Share_historical_data.csv`

---

## Methodology

**Data Preparation**
- Fear/Greed Index: 2,644 daily rows (2018–2025), no nulls/duplicates. Classifications collapsed to Fear / Neutral / Greed for clarity.
- Trader Data: 211,224 trades across 32 accounts and 246 coins (May 2023–May 2025). Timestamps parsed from IST format to date. No nulls or duplicates.
- Datasets aligned by date; 211,218 trades successfully matched to a sentiment label (>99.9%).
- Leverage proxy derived as `|Start Position| / Size USD`, clipped at 100×.
- Daily aggregates computed per (date × account): PnL, win rate, trade count, avg position size, leverage, long/short ratio.
- Trader segments: **Low/Mid/High leverage** (≤5×, 5–15×, >15×); **Frequent/Infrequent** (median split on daily trade count).

---

## Key Insights

### Insight 1 — Greed Days Produce 2× Better PnL with Far Lower Drawdown Risk
| Sentiment | Median Daily PnL | Mean Win Rate | 5th-Pct PnL (Drawdown Proxy) |
|-----------|-----------------|--------------|------------------------------|
| Fear      | $122.74         | 35.7%        | **-$3,485**                  |
| Neutral   | $167.55         | 35.5%        | -$885                        |
| Greed     | **$265.25**     | **36.3%**    | -$174                        |

Greed days yield ~2× higher median daily PnL than Fear days. More importantly, the downside tail (drawdown proxy) on Fear days is **20× worse** than on Greed days, suggesting that the real risk difference lies in catastrophic loss days, not in average performance.

### Insight 2 — Traders Trade More Aggressively (and Larger) on Fear Days
| Sentiment | Mean Trades/Day | Mean Position Size |
|-----------|----------------|-------------------|
| Fear      | 105.4          | $8,530            |
| Neutral   | 100.2          | $6,964            |
| Greed     | 76.9           | $5,955            |

On Fear days, traders execute **37% more trades** at **43% larger position sizes** — the classic panic-overtrading signature. Despite the increased activity, win rates barely differ across sentiments (~35–36%), meaning higher churn on Fear days creates more fee drag with no win-rate reward.

### Insight 3 — High-Leverage Traders Are Most Exposed on Fear Days
High-leverage traders (>15× avg) show the steepest sentiment-sensitive drawdowns. Their median PnL on Greed days is comparable to mid-leverage peers, but on Fear days they absorb disproportionately large losses — consistent with liquidation-risk spikes during volatile Fear periods.

---

## Strategy Recommendations

### Strategy 1: "Greed-Day Calm Down" Rule
*For all traders:*  
On Greed days, reduce trade frequency and position sizes. Fewer, well-planned trades during Greed periods outperform the over-churning pattern observed on Fear days. Target: 70–80 trades/day max, position sizes ≤$6,000.

### Strategy 2: "Fear-Day Leverage Gate" for High-Leverage Traders
*For traders with avg leverage >15×:*  
On Fear days, cap leverage at ≤5× OR move to flat/neutral positioning until sentiment recovers to Neutral. The tail-loss exposure on Fear days (-$3,485 at 5th pct) vs Greed days (-$174) represents a **20× risk amplifier** that leverage magnifies further.

---

## Files
| File | Description |
|------|-------------|
| `analysis.ipynb` | Full notebook with code + charts |
| `fig1_performance_by_sentiment.png` | PnL, win rate, drawdown by sentiment |
| `fig2_behavior_by_sentiment.png` | Trade count, size, L/S ratio by sentiment |
| `fig3_segments.png` | Leverage & frequency segment analysis |
| `fig4_timeseries.png` | Rolling PnL with sentiment backdrop |
