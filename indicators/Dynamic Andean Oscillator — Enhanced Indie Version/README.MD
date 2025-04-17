# **Andean Oscillator — Indie Version**

This is a refined version of the *Andean Oscillator* by © alexgrover, designed specifically for the Indie platform.

> **The Andean Oscillator uses an online algorithm to analyze price trends through exponential envelope smoothing. This version adds structural and visual improvements for better clarity and usability.**

### **Features:**

* **Exponential Smoothing Envelopes:** Captures trend dynamics by combining high-res envelope tracking of price and squared price.
* **Bullish/Bearish Variability Analysis:** Calculates standard deviation-like metrics for upward and downward price components.
* **Dual Histogram:** Market condition is shown via color-coded histogram bars — green for bullish, red for bearish — as a symmetrical "waveform".
* **Compact:** Histogram amplitude is scaled and centered to reduce visual distortion and preserve chart readability on lower-volatility assets.
* **Clear Signal Line:** EMA-based signal on max volatility metric to aid in crossover strategies.
* **Indie v5 compatible:** Fully compliant with latest Indie scripting, uses MutSeriesF, plot.columns and parameterized config.

Good for momentum and trend following strategies, this version of the Andean Oscillator has a cleaner, more usable visual structure while keeping the math of the original.