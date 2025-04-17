# This work is licensed under the Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)
# https://creativecommons.org/licenses/by-nc-sa/4.0/
# Inspired by the original concept by © alexgrover
# Indie version rewritten and enhanced by Pavel Medd
# Education purposes

# indie:lang_version = 5
from indie import indicator, param, plot, color, MutSeriesF
from indie.algorithms import Ema
from math import sqrt

# Indicator declaration and parameters
@indicator('Dynamic Andean Oscillator')
@param.int('length', default=50)  # Base length for smoothing
@param.int('sig_length', default=9, title='Signal Length')  # EMA period for signal line

# Plotting configuration
@plot.line('bull_line', title='Bullish Component', color=color.GREEN, line_width=2)
@plot.line('bear_line', title='Bearish Component', color=color.RED, line_width=2)
@plot.line(title='Signal', color=color.YELLOW, line_width=2)
@plot.columns('bull_hist', title='Bull Histogram', color=color.rgba(204, 255, 0, 0.3), base_value=0.0)
@plot.columns('bear_hist', title='Bear Histogram', color=color.rgba(246, 45, 174, 0.3), base_value=0.0)
def Main(self, length, sig_length):
    # Core smoothing coefficient for exponential update
    alpha = 2 / (length + 1)

    # Mutable time series to retain recursive envelope values
    up1 = MutSeriesF.new(self.close[0])  # Exponential smoothing of max(close, open)
    up2 = MutSeriesF.new(self.close[0] * self.close[0])  # Squared smoothing (for variance-like calc)
    dn1 = MutSeriesF.new(self.close[0])
    dn2 = MutSeriesF.new(self.close[0] * self.close[0])

    # Previous values (time index -1)
    prev_up1 = up1[1]
    prev_up2 = up2[1]
    prev_dn1 = dn1[1]
    prev_dn2 = dn2[1]

    # Recursive exponential envelope update (upper band)
    up1[0] = max(self.close[0], self.open[0], prev_up1 - (prev_up1 - self.close[0]) * alpha)
    up2[0] = max(self.close[0]**2, self.open[0]**2, prev_up2 - (prev_up2 - self.close[0]**2) * alpha)

    # Recursive exponential envelope update (lower band)
    dn1[0] = min(self.close[0], self.open[0], prev_dn1 + (self.close[0] - prev_dn1) * alpha)
    dn2[0] = min(self.close[0]**2, self.open[0]**2, prev_dn2 + (self.close[0]**2 - prev_dn2) * alpha)

    # Deviation from the envelope mean (bull/bear volatility proxy)
    bull_var = dn2[0] - dn1[0] * dn1[0]
    bear_var = up2[0] - up1[0] * up1[0]

    # Square root of variance, safely clamped to avoid math domain errors
    bull = sqrt(bull_var) if bull_var > 0 else 0.0
    bear = sqrt(bear_var) if bear_var > 0 else 0.0

    # Signal line: EMA of the max component (trend highlight)
    signal_input = MutSeriesF.new(max(bull, bear))
    signal = Ema.new(signal_input, sig_length)[0]

    # Amplitude: difference in bull/bear strengths
    amp = abs(bull - bear)

    # Colored histogram: green if bull > bear, red otherwise
    hist_green = amp if bull > bear else 0.0
    hist_red = amp if bear > bull else 0.0

    return bull, bear, signal, hist_green, hist_red
