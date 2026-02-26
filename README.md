# Cross-Exchange Cryptocurrency Arbitrage Detection System

## Overview
Real-time arbitrage detection system that monitors Bitcoin prices across three major cryptocurrency exchanges (Binance, Coinbase, Kraken) using WebSocket connections. The system identifies profitable trading opportunities by detecting price discrepancies between exchanges after accounting for transaction fees.

## Key Results
- **Data Collection:** 24-hour continuous monitoring (516,138 price checks)
- **Profitable Opportunities:** 86,000 opportunities detected (16.7% success rate)
- **Average Profit:** $0.58 per $1,000 trade
- **Total Potential:** $49,959.75 in aggregate opportunities
- **Dominant Pair:** Coinbase → Binance (99.98% of profitable opportunities)

## Technical Implementation

### Data Collection
- **Real-time WebSocket connections** to 3 exchanges simultaneously
- **Asynchronous programming** (asyncio) for concurrent stream handling
- **Continuous logging** to CSV (every second, 6 exchange pairs checked)
- **24-hour dataset:** 516,138 timestamped price comparisons

### Fee Structure & Profitability Calculation
Realistic transaction cost modeling:
- **Binance:** 0.01% maker fee
- **Coinbase:** 0.00% maker fee  
- **Kraken:** 0.16% maker fee
- **Trade size:** $1,000 per opportunity
- **Net profit:** Gross spread minus all fees

### Architecture
**Asynchronous Components:**
1. **Binance WebSocket Stream:** Monitors BTC/USDT order book
2. **Coinbase WebSocket Stream:** Monitors BTC-USD ticker
3. **Kraken WebSocket Stream:** Monitors XBT/USD ticker
4. **Arbitrage Detector:** Checks all 6 pairs every second, logs opportunities

**Data Pipeline:**
```
WebSocket Streams → Price Dictionary → Arbitrage Detection → Fee Calculation → CSV Logging
```

### Analysis & Visualization
- **Statistical analysis:** Profit distribution, exchange pair breakdown, hourly patterns
- **4-chart visualization:** Distribution histogram, time series, pair comparison, cumulative profit
- **ML feature engineering (optional):** 12 features including temporal patterns, rolling metrics, price levels

## Technical Stack
- **Language:** Python 3.x
- **Async Framework:** asyncio
- **WebSocket Library:** websockets
- **Data Processing:** pandas, numpy
- **Visualization:** matplotlib, seaborn
- **Machine Learning (optional):** scikit-learn (Gradient Boosting Classifier)

## Project Structure
```
Project-2-Arbitrage-Detection/
├── notebook/
│   └── Arbitrage_Detection.ipynb    # Complete implementation (15 cells)
└── results/
    ├── arbitrage_analysis.png       # 4-chart visualization
    └── arbitrage_summary.csv        # Summary statistics
```

## Key Findings

### Profitability Insights
- **Best opportunity:** $1.58 profit
- **Median profit:** $0.61 (very consistent)
- **Standard deviation:** $0.15 (tight distribution)
- **Opportunities per hour:** ~3,583

### Exchange Pair Analysis
| Pair | Count | Avg Profit | Total Profit |
|------|-------|------------|--------------|
| Coinbase → Binance | 85,979 | $0.58 | $49,954.40 |
| Kraken → Binance | 12 | $0.22 | $2.59 |
| Binance → Coinbase | 5 | $0.51 | $2.57 |
| Coinbase → Kraken | 4 | $0.05 | $0.19 |

**Key Insight:** Coinbase's 0% maker fee creates systematic advantage when buying there and selling on Binance.

### Temporal Patterns
- **Most profitable hours:** 2-4 AM UTC ($0.68-$0.77 average)
- **Consistent activity:** ~60 opportunities per 10 minutes throughout 24 hours
- **No significant day-of-week pattern** (limited to 1-day sample)

## How to Run
1. **Open notebook:** `notebook/Arbitrage_Detection.ipynb`
2. **Install dependencies:**
```bash
   pip install websockets pandas numpy matplotlib seaborn scikit-learn
```
3. **Run cells sequentially:**
   - **Cells 1-3:** Setup, fee calculator, exchange streams
   - **Cell 4-7:** Arbitrage detection, monitoring functions
   - **Cell 8:** 5-minute test run (verify system works)
   - **Cell 9:** 24-hour data collection (requires overnight run)
   - **Cells 10-12:** Analysis, visualization, summary
   - **Cells 13-15 (optional):** ML feature engineering, classifier training, final report

## Limitations & Real-World Considerations

### Execution Challenges
- **Latency:** Real trading requires <100ms execution (detection is only first step)
- **Slippage:** Actual execution prices may differ from observed prices
- **Order book depth:** Large trades move prices, reducing profitability
- **Capital requirements:** Need funds on multiple exchanges simultaneously

### Risk Factors
- **Inventory risk:** Holding BTC exposes to price volatility
- **Exchange risk:** Withdrawal delays, downtime, or insolvency
- **Regulatory risk:** Cross-border trading restrictions
- **Competition:** HFT firms dominate this space with microsecond latency

### Why Small Profits?
- Average $0.58 per $1,000 trade = **0.058% return**
- After fees, spreads are tiny in liquid BTC markets
- High-frequency traders capture larger spreads before they persist
- This demonstrates **detection capability**, not production trading strategy

## Future Enhancements
- **Futures hedging:** Eliminate inventory risk using derivatives
- **Order execution:** Implement actual trading via exchange APIs
- **Multi-asset support:** Expand beyond Bitcoin (ETH, SOL, etc.)
- **Real-time dashboard:** Web interface for live monitoring
- **Alert system:** Notifications for high-profit opportunities (>$1.00)
- **More exchanges:** Add Bitfinex, Gemini, OKX for more pairs

## Machine Learning Extension (Optional)
**Gradient Boosting Classifier** trained to predict opportunity profitability:
- **Features:** Spread size, hour, day, price level, rolling counts, exchange pair
- **Purpose:** Filter opportunities before execution
- **Benefit:** Reduce false positives, focus on highest-quality signals

## Learnings & Insights
1. **Arbitrage exists but is small:** Markets are efficient; only tiny spreads remain
2. **Fees dominate:** Coinbase's 0% fee creates systematic advantage
3. **Detection ≠ Execution:** Observing opportunities much easier than capturing them
4. **Production complexity:** Real trading requires infrastructure far beyond this POC
5. **Educational value:** Excellent demonstration of async programming, real-time data, and financial modeling

## References
- [Binance WebSocket Documentation](https://binance-docs.github.io/apidocs/spot/en/)
- [Coinbase WebSocket Documentation](https://docs.cloud.coinbase.com/exchange/docs/websocket-overview)
- [Kraken WebSocket Documentation](https://docs.kraken.com/websockets/)

## Author
Anagh Das  
[LinkedIn](linkedin.com/in/dasanagh) | [Email](ddas948@gmail.com)

---

*I Created this project to demonstrating real-time systems, asynchronous programming, and quantitative finance applications.*
