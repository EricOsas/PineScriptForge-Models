# PineScriptForge Models

## ES Cumulative Delta Divergence

File: `strategies/cumulative_delta_divergence_es.pine`

Pine Script v6 strategy for ES futures. Also suitable for NQ and CL when usable delta data is available.

### Entry logic

Bearish:
- Confirmed price swing makes a higher high.
- Cumulative delta at the same pivot makes a lower high.
- Buy aggressor volume declines versus the previous high pivot.
- Buy aggressor ratio declines versus the previous high pivot.
- Enter short only after the pivot is confirmed.

Bullish:
- Confirmed price swing makes a lower low.
- Cumulative delta at the same pivot makes a higher low.
- Sell aggressor volume declines versus the previous low pivot.
- Sell aggressor ratio declines versus the previous low pivot.
- Enter long only after the pivot is confirmed.

### Exit logic

- Short target: most recent confirmed swing low that occurred before the divergence pivot.
- Long target: most recent confirmed swing high that occurred before the divergence pivot.
- Short stop: above the bearish divergence pivot plus configurable tick buffer.
- Long stop: below the bullish divergence pivot minus configurable tick buffer.

### Order-flow data

The strategy imports TradingView's official `TradingView/ta/14` library and uses:

- `requestVolumeDelta()` for cumulative delta.
- `requestUpAndDownVolume()` for directional aggressor volume.
- `1T` lower-timeframe data by default.

TradingView's tick-based implementation can use bid/ask comparisons when available and fallback classification otherwise. Historical tick coverage may be shorter than minute-based coverage, so the lower timeframe is configurable.

### Recommended charts

- 5 minute
- 15 minute
- 1 hour

The strategy can optionally block entries on other chart timeframes.

### Backtest assumptions

Defaults follow the referenced PineScriptForge strategy page:

- Initial capital: $25,000
- Position size: 1 contract
- Commission: $2.25 per contract per side, equivalent to $4.50 round turn
- Slippage: 1 tick per side
- Pyramiding: disabled

### Notes

- Pivots are confirmed using left/right swing bars, so divergence signals do not use future-unconfirmed pivots.
- CVD resets daily by default.
- By default, the two pivots used for divergence must belong to the same CVD reset period.
- If both bullish and bearish signals occur on the same confirmation bar, no trade is taken.
- Aggressor confirmation is configurable as volume only, ratio only, or both.
