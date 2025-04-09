### **Enhanced Andean Oscillator 2025 — Smoothed Volatility Envelope Momentum**

This **professional-grade momentum oscillator,** based on adaptive envelope tracking and volatility projection, is re-engineered from the original Andean Oscillator (by Alpaca Team), with advanced enhancements for 2025 markets.

***

### 🔍 **What It Does**

This indicator dynamically analyzes price momentum using two sets of **adaptive envelopes** and **volatility-weighted deviation calculations**. It detects bullish and bearish pressure by projecting variance through a nonlinear smoothing model. Entry and exit signals are filtered using:

* ✅ **Double EMA signal smoothing**
* ✅ **Dead zone filtering** (neutral range avoidance)
* ✅ **Slope confirmation** (momentum acceleration check)
* ✅ **Higher timeframe trend alignment** (multi-timeframe validation)

***

### **Trading Signals**

| Signal Type | Trigger Condition |
| ----------- | ----------------- |
| **Long Entry** | Bull > Bear + dead zone AND slope rising AND HTF Bull > Bear |
| **Short Entry** | Bear > Bull + dead zone AND slope falling AND HTF Bear > Bull |
| **Exit Long** | Bull < Signal |
| **Exit Short** | Bear < Signal |

***

### ⚙️ **Parameters**

| Name | Description |
| ---- | ----------- |
| Base Length | Base envelope smoothing period |
| Signal Smoothing | EMA smoothing length (applied twice) |
| ATR Length | Used for adaptive alpha calculation |
| Volume Lookback | Used to normalize volatility strength |
| Slope Lookback | Number of bars for slope confirmation |
| Dead Zone Threshold | Minimum deviation between bull/bear required to signal |
| Higher Timeframe | Confirms trade signals with higher timeframe momentum |

***

###  **Visuals**

* 🟢 **Green Line**: Bullish strength
* 🔴 **Red Line**: Bearish strength
* 🟡 **Yellow Line**: Smoothed signal
* 🟩 **Long Entry** marker
* 🟥 **Short Entry** marker
* ➕ Exit markers for both sides (Red - exit short, green - exit long)