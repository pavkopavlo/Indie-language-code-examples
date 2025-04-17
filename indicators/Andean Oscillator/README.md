Andean Oscillator — Indie Port

This is a direct port of the original Andean Oscillator, conceptualized by © alexgrover, and implemented for the Indie platform with strict adherence to the core logic.

The Andean Oscillator is designed to detect directional volatility using an online algorithm for exponential envelope tracking. It estimates bullish and bearish price dynamics separately and derives a volatility-based signal line for trend evaluation.

Core Components:

Exponential Envelopes: Tracks smoothed maxima and minima between open and close prices.

Volatility Estimation: Uses squared price smoothing to approximate standard deviation for upward and downward movements.

Bullish & Bearish Channels: Calculates separate metrics to reflect buying vs. selling pressure.

Signal Line: Applies EMA smoothing to the dominant component (bull or bear) for signal generation.

Mathematical Integrity: Fully replicates the logic presented in the original PineScript version, now adapted to Indie v5 standards.

This version is ideal for those seeking a clean, research-grade implementation of the Andean Oscillator without visual enhancements or stylistic bias. Suitable for further experimentation, study, or integration into larger composite indicators.