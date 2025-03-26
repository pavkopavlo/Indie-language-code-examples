# Indie FAQ / solutions

# How can I create a filled area between two plot lines with custom transparency in Indie?

I'm working with **Indie** and want to create a filled area between two moving averages. I know that `@plot.fill` can be used for this, but I also need to control the transparency of the fill color.
How can I achieve this?

## Solution:

You can use the `@plot.fill` decorator along with `plot.Fill()` to create a shaded area between two plot lines. The transparency is set using the alpha value in the color.
Here’s a complete example:

```python
#indie:lang_version = 5
from indie import indicator, plot, color, MainContext
from indie.algorithms import Sma

@indicator('Area Between Moving Averages', overlay_main_pane=True)
@plot.line('fast_ma', color=color.BLUE, title='Fast MA')
@plot.line('slow_ma', color=color.RED, title='Slow MA')
@plot.fill('fast_ma', 'slow_ma', color=color.PURPLE(0.3), title='MA Area')
class Main(MainContext):
    def calc(self):
        # Calculate two moving averages
        fast_ma = Sma.new(self.close, 20)[0]
        slow_ma = Sma.new(self.close, 50)[0]
        
        # Return values for plotting, including fill object
        return fast_ma, slow_ma, plot.Fill()
```

## Explanation:

* The `@plot.line` decorators define two plot lines (`fast_ma` and `slow_ma`).
* The `@plot.fill` decorator references these lines to create a filled area between them.
* Transparency is controlled using `color.PURPLE(0.3)`, where `0.3` represents **30% opacity**.
* The `calc` method returns the moving averages and a `plot.Fill()` object.

## Customization:

* Adjust transparency by modifying the alpha value (e.g., `color.PURPLE(0.5)` for 50% opacity).
* Change colors as needed, using predefined colors or custom RGBA values.

Here’s your second question formatted in Stack Overflow style:

***

# How can I implement multiple fill areas with different transparencies in Indie?

I need to create **multiple fill areas** between different plot lines in Indie, where each fill area has a **custom transparency** based on market conditions. How can I achieve this?

## Solution:

You can use the `@plot.fill` decorator for multiple fill areas and control transparency dynamically by modifying the alpha channel of the fill color.
Here's a complete example:

```python
# indie:lang_version = 5
from indie import indicator, plot, color
from indie.algorithms import Sma

@indicator("Multiple Fill Areas", overlay_main_pane=True)
@plot.line('sma_fast', title="Fast MA", color=color.BLUE)
@plot.line('price', title="Price", color=color.WHITE)
@plot.line('sma_slow', title="Slow MA", color=color.RED)
@plot.fill('price', 'sma_fast', title="Upper Fill")
@plot.fill('price', 'sma_slow', title="Lower Fill")
def Main(self):
    # Calculate indicators
    sma_fast = Sma.new(self.close, 20)[0]
    sma_slow = Sma.new(self.close, 50)[0]
    price = self.close[0]
    
    # Determine transparency based on conditions (example: trend direction)
    upper_trend_strength = max(0, min(1, (price - sma_fast) / price * 10))
    lower_trend_strength = max(0, min(1, (sma_slow - price) / price * 10))
    
    # Create different transparency colors for the fill areas
    upper_fill_color = color.GREEN(0.2 + 0.3 * upper_trend_strength)  # 20%-50% opacity
    lower_fill_color = color.RED(0.2 + 0.3 * lower_trend_strength)    # 20%-50% opacity
    
    # Return the values and fill objects with custom colors
    return (
        sma_fast,
        price,
        sma_slow,
        plot.Fill(color=upper_fill_color),
        plot.Fill(color=lower_fill_color)
    )
```

***

## Key Components:

1. **Plot Lines:**
    * Three lines:
        * `sma_fast` (Fast MA, **blue**)
        * `price` (**white**)
        * `sma_slow` (Slow MA, **red**)
2. **Fill Areas:**
    * `@plot.fill('price', 'sma_fast')` → **Upper Fill**
    * `@plot.fill('price', 'sma_slow')` → **Lower Fill**
3. **Dynamic Transparency Calculation:**
    * `upper_trend_strength` and `lower_trend_strength` are used to scale transparency.
    * `color.GREEN(0.2 + 0.3 * upper_trend_strength)` results in transparency between **20% and 50%**.
    * `color.RED(0.2 + 0.3 * lower_trend_strength)` does the same for the lower area.
4. **Return Values:**
    * **Three plots:** `sma_fast`, `price`, `sma_slow`
    * **Two fill objects** with dynamically assigned colors.

***

## Customization Ideas:

* Adjust transparency by modifying `0.2 + 0.3 * trend_strength` range.
* Change colors to suit your strategy.
* Apply different logic for transparency, such as volatility-based adjustments.

This setup creates a **dynamic shading effect** that visually represents price trends with varying transparency.
Hope this helps! 🚀 Let me know if you have any questions.

#What's the best way to implement background coloring that changes based on the trend direction in Indie?

```python
# indie:lang_version = 5
from indie import indicator, plot, color, MutSeriesF
from indie.algorithms import Sma

@indicator('Trend Background Color', overlay_main_pane=True)
@plot.line('price', color=color.WHITE, title='Price')
@plot.line('ma', color=color.YELLOW, title='Moving Average')
@plot.fill('price', 'ma', title='Trend Background')
def Main(self):
    # Calculate trend direction
    ma = Sma.new(self.close, 20)
    trend_up = self.close[0] > ma[0]
    
    # Set background color based on trend
    fill_color = color.GREEN(0.1) if trend_up else color.RED(0.1)
    
    # Return values and fill with dynamic color
    return self.close[0], ma[0], plot.Fill(color=fill_color)
```

This indicator:

1. Shows the price and a 20-period moving average
2. Colors the background green when price is above the MA (uptrend)
3. Colors the background red when price is below the MA (downtrend)
4. Uses transparency (0.2) to make the colors subtle

![image.png]()



# How can I create a custom plotted RSI with colored zones in Indie?

This example demonstrates how to create an **Advanced RSI Indicator** with **colored zones**, **divergence detection**, and **trend-based coloring** in Indie. The RSI is plotted with optional smoothing and overlays a volatility band using standard deviation.

## Features:

* **Overbought & Oversold Zones**: Customizable levels with shading.
* **Neutral Zone**: Middle range visualization for balanced conditions.
* **Divergence Detection**: Highlights potential trend reversals.
* **Trend-Based Coloring**: RSI line changes color depending on market conditions.
* **Optional Smoothing**: Simple Moving Average (SMA) smoothing for RSI.

## Code Implementation:

```python
# indie:lang_version = 5
from indie import (
    indicator, param, source, plot, color,
    level, band, MutSeriesF
)
from indie.algorithms import Rsi, Ma, StdDev

@indicator('Advanced RSI with Divergence and Trend')
@param.int('length', default=14, min=1, title='RSI Length')
@param.source('src', default=source.CLOSE, title='Source')
@param.int('overbought', default=70, min=50, max=100, title='Overbought Level')
@param.int('oversold', default=30, min=0, max=50, title='Oversold Level')
@param.int('neutral_high', default=60, min=40, max=70, title='Neutral High Level')
@param.int('neutral_low', default=40, min=30, max=60, title='Neutral Low Level')
@param.int('smoothing_length', default=0, min=0, max=50, title='Smoothing Length (0 = No Smoothing)')
@param.bool('show_divergence', default=False, title='Show Divergence Signals')
@param.bool('show_trend_color', default=True, title='Color Based on Trend')
@level(50, line_color=color.GRAY(0.5), title='Middle Line')
@band(70, 100, line_color=color.RED, fill_color=color.RED(0.1), title='Overbought Zone')
@band(0, 30, line_color=color.GREEN, fill_color=color.GREEN(0.1), title='Oversold Zone')
@band(40, 60, line_color=color.GRAY, fill_color=color.GRAY(0.1), title='Neutral Zone')
@plot.line(color=color.BLUE, line_width=2, title='RSI')
def Main(self, length, src, overbought, oversold,
          neutral_high, neutral_low, smoothing_length,
          show_divergence, show_trend_color):
    # Calculate RSI
    rsi = Rsi.new(src, length)
    rsi_value = rsi[0]
    
    # Optional Smoothing
    if smoothing_length > 0:
        rsi_value = Ma.new(rsi, smoothing_length, 'SMA')[0]
    
    # Divergence Detection (Optional)
    divergence_signal = color.GRAY
    if show_divergence:
        # Bullish Divergence: Price makes lower low, RSI makes higher low
        if (self.close[0] < self.close[1] and
            rsi_value > rsi[1] and
            rsi_value <= oversold):
            divergence_signal = color.GREEN
        
        # Bearish Divergence: Price makes higher high, RSI makes lower high
        elif (self.close[0] > self.close[1] and
              rsi_value < rsi[1] and
              rsi_value >= overbought):
            divergence_signal = color.RED
    
    # Color Determination
    rsi_color = color.BLUE  # Default
    
    if show_trend_color:
        if rsi_value >= overbought:
            rsi_color = color.RED  # Overbought zone
        elif rsi_value <= oversold:
            rsi_color = color.GREEN  # Oversold zone
        elif rsi_value >= neutral_low and rsi_value <= neutral_high:
            rsi_color = color.GRAY  # Neutral zone
    
    # Optional Volatility Band
    volatility_band = StdDev.new(rsi, length)[0]
    
    # Return visualization elements
    return plot.Line(rsi_value, color=rsi_color), plot.Line(rsi_value + volatility_band, color=color.PURPLE(0.5)), plot.Line(rsi_value - volatility_band, color=color.PURPLE(0.5))
```

## Key Highlights:

1. **Customizable RSI Levels**: Configure overbought, oversold, and neutral zones.
2. **Trend-Based Coloring**: RSI line dynamically changes color.
3. **Divergence Detection**: Highlights bullish/bearish divergences.
4. **Volatility Bands**: Uses standard deviation to visualize RSI volatility.
5. **Configurable Smoothing**: Option to smooth RSI using SMA.

This approach enhances traditional RSI analysis by providing **clearer visual cues** and **additional insights** for traders. 🚀

# How do I implement stacked histogram bars with different colors in Indie


1. **True Stacked Histogram**
    * Separate volumes for bullish and bearish periods
    * Uses two `plot.Histogram()` calls to create stacked bars
2. **Volume Calculation**
    * Separate volumes for up and down periods
    * Calculates volume sums for each direction
    * Uses `Sum.new()` to smooth volume over a specified length
3. **Dynamic Coloring**
    * Bullish volume colors change based on RSI levels
    * Bearish volume colors change based on RSI levels
    * Provides visual context of market strength

Color Logic Breakdown:

* High RSI (≥ 60): Bright green bullish volume
* Moderate RSI (50-60): Lighter green bullish volume
* Low RSI (≤ 40): Bright red bearish volume
* Moderate RSI (40-50): Lighter red bearish volume

```python
# indie:lang_version = 5
from indie import (
    indicator, param, source, plot, color, 
    MutSeriesF
)
from indie.algorithms import Rsi, Sum

@indicator('Stacked Volume Histogram')
@param.int('rsi_length', default=14, min=1, title='RSI Length')
@param.int('volume_length', default=14, min=1, title='Volume Length')
@param.source('src', default=source.CLOSE, title='Source')
@plot.histogram(title='Bullish Volume')
@plot.histogram(title='Bearish Volume')
def Main(self, rsi_length, volume_length, src):
    # Calculate RSI
    rsi = Rsi.new(src, rsi_length)[0]
    
    # Determine volume direction and RSI-based color
    bullish_volume = MutSeriesF.new(
        self.volume[0] if self.close[0] > self.close[1] else 0
    )
    bearish_volume = MutSeriesF.new(
        self.volume[0] if self.close[0] < self.close[1] else 0
    )
    
    # Calculate volume sums
    bullish_vol_sum = Sum.new(bullish_volume, volume_length)[0]
    bearish_vol_sum = Sum.new(bearish_volume, volume_length)[0]
    
    # Color selection based on RSI
    bullish_color = (
        color.GREEN if rsi >= 60 else 
        color.LIME if rsi >= 50 else 
        color.GREEN(0.5)
    )
    
    bearish_color = (
        color.RED if rsi <= 40 else 
        color.MAROON if rsi <= 50 else 
        color.RED(0.5)
    )
    
    # Return stacked histogram bars
    return (
        plot.Histogram(bullish_vol_sum, color=bullish_color), 
        plot.Histogram(bearish_vol_sum, color=bearish_color)
    )
```

![image.png]()
 



 