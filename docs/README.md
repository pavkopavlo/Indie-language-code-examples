**Pine Script to Indie Language Conversion Cheat Sheet**
This guide helps Pine Script users migrate to TakeProfit's Indie language. It covers language structures, built-in indicators, plotting, and context handling. Examples are taken from official sources and the Indie code examples GitHub repo.

 

## 1\. Script Declaration & Context

This section outlines how to declare scripts and set their context in both Pine Script and Indie, highlighting the structural differences and similarities.

| **Feature** | **Pine Script** | **Indie** | **Notes** |
| ------- | ----------- | ----- | ----- |
| **Version Declaration** | `//@version=5` | `# indie:lang_version = 5` | Pine Script uses a special comment to specify the version, while Indie employs a directive comment. |
| **Indicator Declaration** | `indicator("My Indicator", overlay=true)` | `@indicator("My Indicator", overlay_main_pane=True)class Main(MainContext):` | Pine Script uses the `indicator` function for declaration. Indie utilizes the `@indicator` decorator above a class that inherits from `MainContext`. The `overlay` parameter in Pine corresponds to `overlay_main_pane` in Indie. |
| **Strategy Declaration** | `strategy("My Strategy", overlay=true)` | *Not directly supported* | Indie focuses on indicator development and does not have a direct equivalent to Pine Script's `strategy` function. |
| **Accessing OHLC Data** | `open`, `high`, `low`, `close`, `volume` | `self.open`, `self.high`, `self.low`, `self.close`, `self.volume` | In Pine Script, OHLCV data is accessed directly. Indie requires prefixing with `self.` to access these attributes within the class. |
| **Historical Referencing** | `close[1]` | `self.close[1]` | Both languages use a similar indexing method to reference historical data, with Indie requiring the `self.` prefix. |
| **Time and Bar Index** | `time`, `bar_index` | `self.time`, `self.bar_index` | Accessing time and bar index information follows the same pattern, with Indie using the `self.` prefix. |

**Detailed Explanation:**

1. **Version Declaration:**
    * *Pine Script:* Uses `//@version=5` at the beginning of the script to specify the version.
    * *Indie:* Uses `# indie:lang_version = 5` as a directive comment to indicate the language version.
2. **Indicator Declaration:**
    * *Pine Script:* Declares an indicator using the `indicator` function, specifying parameters like the name and whether it overlays the main chart.
    * *Indie:* Utilizes the `@indicator` decorator above a class that inherits from `MainContext`. This decorator specifies the indicator's name and overlay properties.
3. **Strategy Declaration:**
    * *Pine Script:* Supports strategy development using the `strategy` function.
    * *Indie:* Does not have a direct equivalent for strategies, focusing primarily on indicator development.
4. **Accessing OHLC Data:**
    * *Pine Script:* Accesses OHLCV data directly using variables like `open`, `high`, `low`, `close`, and `volume`.
    * *Indie:* Requires accessing these attributes with the `self.` prefix within the class context.
5. **Historical Referencing:**
    * Both languages allow referencing historical data using indexing, e.g., `close[1]` in Pine Script and `self.close[1]` in Indie.
6. **Time and Bar Index:**
    * Accessing time and bar index information is done using `time` and `bar_index` in Pine Script, and `self.time` and `self.bar_index` in Indie.

***

Continuing with the comprehensive comparison between Pine Script and Indie language, here is **Section 2: Data Types & Variables**.

***

## 2\. Data Types & Variables

| **Concept** | **Pine Script** | **Indie** | **Notes** |
| ------- | ----------- | ----- | ----- |
| **Variable Declaration** | `var float myVar = navar int count = 0` | `my_var = MutSeriesF.new(math.nan)count = MutSeriesI.new(0)` | Pine Script uses `var` for persistent variables. Indie uses `MutSeriesF` or `MutSeriesI` for mutable series of floats or integers, respectively. |
| **Data Types** | `int`, `float`, `bool`, `string`, `color`, `line`, `label`, `array`, `map` | `int`, `float`, `bool`, `str`, `color`, `SeriesF`, `MutSeriesF`, `Series[int]`, etc. | Pine Script has a variety of built-in types. Indie aligns more with Python's type system and includes specific series types. |
| **Type Inference** | Implicit based on assigned value | Explicit when necessary; often inferred | Pine Script infers types but allows explicit declarations. Indie relies on Python-like inference but may require explicit typing in certain contexts. |
| **Series vs. Non-Series** | All variables are series by default | Distinction between series (`SeriesF`, `MutSeriesF`) and non-series (`float`, `int`) | Pine Script treats all variables as series. Indie requires explicit handling of series data using specific classes. |
| **NaN Handling** | `na`, `nz(x, 0)` | `math.nan`, `math.isnan(x)`, fallback using `or` | Pine Script uses `na` and `nz` for NaN handling. Indie utilizes Python's `math.nan` and related functions. |
| **Persistent Variables** | `var float x = 0` | `x = MutSeriesF.new(0.0)` | Pine Script uses `var` for persistence. Indie uses mutable series classes like `MutSeriesF`. |
| **Undefined Variables** | Defaults to `na` if not initialized | Raises an error if accessed before assignment | Pine Script allows undefined variables to default to `na`. Indie requires explicit initialization. |

**Detailed Explanation:**

1. **Variable Declaration:**
    * *Pine Script:* Uses the `var` keyword to declare persistent variables that retain their values across bars. For example:

        ```
        var float myVar = na
        var int count = 0
        
        ```
    * *Indie:* Utilizes classes like `MutSeriesF` for mutable series of floats. Variables are assigned as follows:

        ```
        my_var = MutSeriesF.new(math.nan)
        count = MutSeriesI.new(0)
        
        ```
2. **Data Types:**
    * *Pine Script:* Supports several data types including `int`, `float`, `bool`, `string`, `color`, `line`, `label`, `array`, and `map`. citeturn0search0
    * *Indie:* Aligns more closely with Python's type system, including types like `int`, `float`, `bool`, `str`, and specific series types such as `SeriesF` and `MutSeriesF`.
3. **Type Inference:**
    * *Pine Script:* Infers types based on the assigned value but allows for explicit type declarations.
    * *Indie:* Generally infers types in a Pythonic manner but may require explicit typing in certain contexts for clarity and correctness.
4. **Series vs. Non-Series:**
    * *Pine Script:* Treats all variables as series by default, meaning they can hold a value for each bar in the dataset.
    * *Indie:* Distinguishes between series and non-series variables. Series data must be handled using specific classes like `SeriesF` or `MutSeriesF`.
5. **NaN Handling:**
    * *Pine Script:* Uses `na` to represent 'not available' or NaN values and provides functions like `nz(x, 0)` to handle them.
    * *Indie:* Utilizes Python's `math.nan` and related functions for NaN representation and checking, such as `math.isnan(x)`. Fallback values can be assigned using the `or` operator.
6. **Persistent Variables:**
    * *Pine Script:* Uses the `var` keyword to declare variables that persist their values across bars.
    * *Indie:* Achieves persistence through mutable series classes. For example:

        ```
        x = MutSeriesF.new(0.0)
        
        ```
7. **Undefined Variables:**
    * *Pine Script:* Allows variables to be used without explicit initialization, defaulting their value to `na`.
    * *Indie:* Requires variables to be explicitly initialized before use; accessing an uninitialized variable will raise an error.

***

Continuing with the detailed comparison between Pine Script and Indie language, here is **Section 3: Inputs (Parameters)**.

***

## 3\. Inputs \(Parameters\)

| **Feature** | **Pine Script** | **Indie** | **Notes** |
| ------- | ----------- | ----- | ----- |
| **Input Declaration** | `length = input(14, title="Length")` | `@param.int("length", default=14)def Main(self, length):` | Pine Script uses the `input()` function to create user inputs, while Indie employs decorators like `@param.int` to define parameters, which are then passed to the `Main` function. |
| **Input Types** | `input.int`, `input.float`, `input.bool`, `input.string`, `input.color`, `input.source`, etc. | `@param.int`, `@param.float`, `@param.bool`, `@param.string`, `@param.color`, `@param.source`, etc. | Both languages support similar input types, but Indie uses decorators for parameter definitions. |
| **Default Values** | `input(14, title="Length")` (default is 14) | `@param.int("length", default=14)` | Default values are specified directly in both languages; Indie uses the `default` keyword in the decorator. |
| **Options for Inputs** | `input.string(title="MA Type", options=["SMA", "EMA"])` | `@param.string("ma_type", options=["SMA", "EMA"])` | Both languages allow specifying a list of options for string inputs to create dropdown menus. |
| **Grouping Inputs** | `input(14, title="Length", group="Settings")` | *Not directly supported* | Pine Script allows grouping inputs under a common heading; Indie does not have a direct equivalent. |
| **Inline Inputs** | `input(14, title="Length", inline="MA Settings")` | *Not directly supported* | Pine Script supports inline grouping of inputs; Indie does not have a direct equivalent. |
| **Tooltips** | `input(14, title="Length", tooltip="Number of periods for calculation")` | *Not directly supported* | Pine Script allows adding tooltips to inputs; Indie does not have a direct equivalent. |

**Detailed Explanation:**

1. **Input Declaration:**
    * *Pine Script:* Uses the `input()` function to declare user inputs. For example:

        ```
        length = input(14, title="Length")
        
        ```

        This creates an input field labeled "Length" with a default value of 14.
    * *Indie:* Utilizes parameter decorators to define inputs, which are then passed as arguments to the `Main` function:

        ```
        @param.int("length", default=14)
        def Main(self, length):
            pass
        
        ```

        Here, `@param.int` defines an integer parameter named "length" with a default value of 14.
2. **Input Types:**
    * *Pine Script:* Provides various input functions such as `input.int`, `input.float`, `input.bool`, `input.string`, `input.color`, and `input.source` to handle different data types.
    * *Indie:* Offers corresponding decorators like `@param.int`, `@param.float`, `@param.bool`, `@param.string`, `@param.color`, and `@param.source` for parameter definitions.
3. **Default Values:**
    * *Pine Script:* Specifies default values within the `input()` function call.
    * *Indie:* Sets default values using the `default` keyword within the parameter decorator.
4. **Options for Inputs:**
    * *Pine Script:* Allows creating dropdown menus by specifying the `options` parameter:

        ```
        ma_type = input.string(title="MA Type", options=["SMA", "EMA"])
        
        ```
    * *Indie:* Achieves similar functionality using the `options` parameter in decorators:

        ```
        @param.string("ma_type", options=["SMA", "EMA"])
        
        ```
5. **Grouping Inputs:**
    * *Pine Script:* Supports grouping inputs under a common heading using the `group` parameter:

        ```
        length = input(14, title="Length", group="Settings")
        
        ```
    * *Indie:* Does not have a direct equivalent for grouping inputs.
6. **Inline Inputs:**
    * *Pine Script:* Allows placing multiple inputs on the same line using the `inline` parameter:

        ```
        length = input(14, title="Length", inline="MA Settings")
        
        ```
    * *Indie:* Does not support inline grouping of inputs.
7. **Tooltips:**
    * *Pine Script:* Provides the `tooltip` parameter to add explanatory text to inputs:

        ```
        length = input(14, title="Length", tooltip="Number of periods for calculation")
        
        ```
    * *Indie:* Does not offer a direct way to add tooltips to inputs.

***

## 4\. Series & State Handling

| **Concept** | **Pine Script** | **Indie** | **Notes** |
| ------- | ----------- | ----- | ----- |
| **Series Data Handling** | All variables are series by default; e.g., `close` refers to a series of closing prices. | Distinguishes between single values (`float`, `int`) and series (`SeriesF`, `MutSeriesF`). | Pine Script treats all variables as series, whereas Indie requires explicit declaration for series data. |
| **Historical Referencing** | `close[1]` accesses the previous bar's closing price. | `self.close[1]` accesses the previous bar's closing price. | Both languages use zero-based indexing for historical data, with Indie requiring the `self.` prefix. |
| **Persistent Variables** | `var float myVar = na` initializes `myVar` only once, retaining its value across bars. | `my_var = MutSeriesF.new(math.nan)` creates a mutable series that maintains state across bars. | Pine Script uses the `var` keyword for persistence; Indie employs mutable series classes like `MutSeriesF`. |
| **State Initialization** | Variables can be initialized conditionally, e.g., `if barstate.isfirst: myVar := 0`. | State is typically initialized in the `__init__` method of the class. | Pine Script uses `barstate` variables for initialization checks; Indie uses class constructors. |
| **Updating State** | `myVar := myVar + 1` updates the variable's value on each bar. | `self.my_var[0] = self.my_var[0] + 1` updates the current value of the series. | Indie requires specifying the index when updating series data. |
| **Bar State Detection** | Utilizes `barstate` variables like `barstate.isfirst`, `barstate.islast`. | No direct equivalent; state management is handled through class methods and attributes. | Pine Script provides built-in variables for bar state detection; Indie relies on object-oriented approaches. |

**Detailed Explanation:**

1. **Series Data Handling:**
    * *Pine Script:* All variables are inherently series. For example, `close` represents the series of closing prices for each bar.
    * *Indie:* Differentiates between single values and series. Series data must be explicitly defined using classes like `SeriesF` for immutable series or `MutSeriesF` for mutable series.
2. **Historical Referencing:**
    * *Pine Script:* Accesses historical data using square brackets, e.g., `close[1]` for the previous bar's close.
    * *Indie:* Uses a similar approach but requires the `self.` prefix, e.g., `self.close[1]`.
3. **Persistent Variables:**
    * *Pine Script:* Uses the `var` keyword to declare variables that retain their value across bars.
    * *Indie:* Achieves persistence through mutable series classes. For example:

        ```
        self.my_var = MutSeriesF.new(math.nan)
        
        ```
4. **State Initialization:**
    * *Pine Script:* Can initialize variables conditionally using `barstate` variables. For example:

        ```
        if barstate.isfirst
            myVar := 0
        
        ```
    * *Indie:* Typically initializes state in the `__init__` method of the class:

        ```
        def __init__(self):
            self.my_var = MutSeriesF.new(0.0)
        
        ```
5. **Updating State:**
    * *Pine Script:* Updates variable values directly, e.g., `myVar := myVar + 1`.
    * *Indie:* Requires specifying the index when updating series data:

        ```
        self.my_var[0] = self.my_var[0] + 1
        
        ```
6. **Bar State Detection:**
    * *Pine Script:* Provides built-in `barstate` variables to detect the state of the current bar, such as `barstate.isfirst` for the first bar and `barstate.islast` for the last bar.
    * *Indie:* Does not have direct equivalents but manages state through class methods and attributes.

***

 Continuing with the detailed comparison between Pine Script and Indie language, here is **Section 5: Built-in Functions & Indicators**.

***

## 5\. Built\-in Functions & Indicators

| **Function/Indicator** | **Pine Script** | **Indie** | **Notes** |
| ------------------ | ----------- | ----- | ----- |
| **Simple Moving Average (SMA)** | `ta.sma(source, length)` | `Sma.new(source, length)` | Both languages provide built-in functions for SMA calculation, with Indie utilizing object-oriented syntax. |
| **Exponential Moving Average (EMA)** | `ta.ema(source, length)` | `Ema.new(source, length)` | Similar functionality with different syntax; Indie uses class instantiation. |
| **Relative Strength Index (RSI)** | `ta.rsi(source, length)` | `Rsi.new(source, length)` | Both languages offer built-in RSI functions; Indie follows an object-oriented approach. |
| **Moving Average Convergence Divergence (MACD)** | `ta.macd(source, short_length, long_length, signal_length)` | `Macd.new(source, short_length, long_length, signal_length)` | Indie encapsulates MACD parameters within a class constructor. |
| **Bollinger Bands (BB)** | `ta.bb(source, length, mult)` | `Bb.new(source, length, mult)` | Both provide BB functions; Indie uses a class-based approach. |
| **Average True Range (ATR)** | `ta.atr(length)` | `Atr.new(length)` | ATR calculation is built-in for both, with Indie using class instantiation. |
| **True Range (TR)** | `ta.tr()` | `Tr.new()` | True Range is available in both, following the respective language's syntax. |
| **Crossover Detection** | `ta.crossover(series1, series2)` | `series1[1] < series2[1] and series1[0] > series2[0]` | Indie does not have a built-in crossover function; it is implemented manually. |
| **Crossunder Detection** | `ta.crossunder(series1, series2)` | `series1[1] > series2[1] and series1[0] < series2[0]` | Similar to crossover; manual implementation is required in Indie. |
| **Standard Deviation** | `ta.stdev(source, length)` | `Stdev.new(source, length)` | Standard deviation functions are built-in, with Indie using a class-based approach. |
| **Variance** | `ta.variance(source, length)` | `Variance.new(source, length)` | Variance calculation is available in both, following respective syntax. |

**Detailed Explanation:**

1. **Simple Moving Average (SMA):**
    * *Pine Script:* Uses the `ta.sma()` function to calculate the simple moving average.

        ```
        sma_value = ta.sma(close, 14)
        
        ```
    * *Indie:* Utilizes the `Sma` class with the `new` method.

        ```
        sma = Sma.new(self.close, 14)
        
        ```
2. **Exponential Moving Average (EMA):**
    * *Pine Script:* Employs `ta.ema()` for exponential moving average calculation.

        ```
        ema_value = ta.ema(close, 14)
        
        ```
    * *Indie:* Uses the `Ema` class.

        ```
        ema = Ema.new(self.close, 14)
        
        ```
3. **Relative Strength Index (RSI):**
    * *Pine Script:* Calculates RSI with `ta.rsi()`.

        ```
        rsi_value = ta.rsi(close, 14)
        
        ```
    * *Indie:* Uses the `Rsi` class.

        ```
        rsi = Rsi.new(self.close, 14)
        
        ```
4. **Moving Average Convergence Divergence (MACD):**
    * *Pine Script:* Provides `ta.macd()` for MACD calculations.

        ```
        [macdLine, signalLine, histLine] = ta.macd(close, 12, 26, 9)
        
        ```
    * *Indie:* Implements MACD through the `Macd` class.

        ```
        macd = Macd.new(self.close, 12, 26, 9)
        
        ```
5. **Bollinger Bands (BB):**
    * *Pine Script:* Uses `ta.bb()` to compute Bollinger Bands.

        ```
        [bbMiddle, bbUpper, bbLower] = ta.bb(close, 20, 2)
        
        ```
    * *Indie:* Utilizes the `Bb` class.

        ```
        bb = Bb.new(self.close, 20, 2)
        
        ```
6. **Average True Range (ATR):**
    * *Pine Script:* Calculates ATR using `ta.atr()`.

        ```
        atr_value = ta.atr(14)
        
        ```
    * *Indie:* Uses the `Atr` class.

        ```
        atr = Atr.new(14)
        
        ```
7. **True Range (TR):**
    * *Pine Script:* Computes True Range with `ta.tr()`.

        ```
        tr_value = ta.tr()
        
        ```
    * *Indie:* Implements True Range via the `Tr` class.

        ```
        tr = Tr.new()
        
        ```
8. **Crossover and Crossunder Detection:**
    * *Pine Script:* Provides `ta.crossover()` and `ta.crossunder()` functions.

        ```
        is_crossover = ta.crossover(series1, series2)
        is_crossunder = ta.crossunder(series1, series2)
        
        ```
    * *Indie:* Requires manual implementation.

        ```
        is_crossover = series1[1] < series2[1] and series1[0] > series2[0]
        is_crossunder = series1[1] > series2[1] and series1[0] < series2[0]
        
        ```
9. **Standard Deviation:**
    * *Pine Script:* Uses `ta.stdev()` for standard deviation calculations.

        ```
        stdev_value = ta.stdev(close, 14)
        
        ```
    * *Indie:* Implements via the `Stdev` class.

        ```
        stdev = Stdev.new(self.close, 14)
        
        ```
10. **Variance:**
    * *Pine Script:* Calculates variance using `ta.variance()`.

        ```
        variance_value = ta.variance(close, 14)
        
        ```
    * *Indie:* Utilizes the `Variance` class.

        ```
        variance = Variance.new(self.close, 14)
        
        ```

***
 
Continuing with the detailed comparison between Pine Script and Indie language, here is **Section 6: Plotting & Visualization**.

---

## 6. Plotting & Visualization

This section explores how both Pine Script and Indie handle the visualization of data through plotting functions, including their capabilities, syntax, and customization options.

| **Feature**             | **Pine Script**                                                                 | **Indie**                                                                                 | **Notes**                                                                                                                                                                                                                   |
|-------------------------|---------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Basic Plotting**      | `plot(series, title="My Plot")`                                                 | `@plot("My Plot")`<br>`def plot_my_plot(self):`<br>`    return series`                     | Pine Script uses the `plot()` function for basic plotting, while Indie employs the `@plot` decorator above a function that returns the series to plot. |
| **Plot Styles**         | `plot(series, style=plot.style_line)`                                           | `@plot("My Plot", style="line")`                                                          | Both languages support various plot styles; Indie specifies the style as a string parameter in the `@plot` decorator. |
| **Color Customization** | `plot(series, color=color.red)`                                                 | `@plot("My Plot", color="red")`                                                           | Color customization is straightforward in both languages, with Indie using color names as strings. |
| **Line Width**          | `plot(series, linewidth=2)`                                                     | `@plot("My Plot", width=2)`                                                               | Line width can be adjusted in both languages; Indie uses the `width` parameter. |
| **Conditional Coloring** | `plot(series, color=series > 0 ? color.green : color.red)`                      | `@plot("My Plot", color=when(series > 0, "green", "red"))`                                | Both languages support conditional coloring; Indie uses the `when` function for this purpose. |
| **Shapes and Arrows**   | `plotshape(series, style=shape.triangleup)`                                     | `@plot("My Shape", style="triangle_up")`                                                  | Pine Script uses `plotshape()` for shapes; Indie integrates shape styles within the `@plot` decorator. |
| **Filling Areas**       | `fill(plot1, plot2, color=color.blue, transp=90)`                               | `@fill("My Fill", plot1, plot2, color="blue", transp=90)`                                 | Area filling between plots is supported in both languages, with Indie using the `@fill` decorator. |
| **Bar Colors**          | `barcolor(color=color.red)`                                                     | `@bar_color("red")`                                                                       | Bar coloring is achieved using `barcolor()` in Pine Script and `@bar_color` in Indie. |
| **Background Coloring** | `bgcolor(color=color.gray)`                                                     | `@background_color("gray")`                                                               | Background coloring is handled by `bgcolor()` in Pine Script and `@background_color` in Indie. |
| **Labels and Text**     | `label.new(x=bar_index, y=high, text="Label")`                                  | `@label("My Label", x=bar_index, y=high, text="Label")`                                   | Label creation is similar, with Indie using the `@label` decorator. |

**Detailed Explanation:**

1. **Basic Plotting:**
   - *Pine Script:* Uses the `plot()` function to visualize data series.
     ```pinescript
     plot(series, title="My Plot")
     ```
   - *Indie:* Utilizes the `@plot` decorator above a function that returns the series to be plotted.
     ```python
     @plot("My Plot")
     def plot_my_plot(self):
         return series
     ```

2. **Plot Styles:**
   - *Pine Script:* Specifies the plot style using the `style` parameter.
     ```pinescript
     plot(series, style=plot.style_line)
     ```
   - *Indie:* Defines the style within the `@plot` decorator.
     ```python
     @plot("My Plot", style="line")
     ```

3. **Color Customization:**
   - *Pine Script:* Sets the color using the `color` parameter.
     ```pinescript
     plot(series, color=color.red)
     ```
   - *Indie:* Assigns color directly in the `@plot` decorator.
     ```python
     @plot("My Plot", color="red")
     ```

4. **Line Width:**
   - *Pine Script:* Adjusts line width with the `linewidth` parameter.
     ```pinescript
     plot(series, linewidth=2)
     ```
   - *Indie:* Uses the `width` parameter in the `@plot` decorator.
     ```python
     @plot("My Plot", width=2)
     ```

5. **Conditional Coloring:**
   - *Pine Script:* Implements conditional coloring using the ternary operator.
     ```pinescript
     plot(series, color=series > 0 ? color.green : color.red)
     ```
   - *Indie:* Achieves this with the `when` function.
     ```python
     @plot("My Plot", color=when(series > 0, "green", "red"))
     ```

6. **Shapes and Arrows:**
   - *Pine Script:* Uses `plotshape()` to add shapes like triangles or arrows.
     ```pinescript
     plotshape(series, style=shape.triangleup)
     ```
   - *Indie:* Integrates shape styles within the `@plot` decorator.
     ```python
     @plot("My Shape", style="triangle_up")
     ```

7. **Filling Areas:**
   - *Pine Script:* Fills areas between two plots using the `fill()` function.
     ```pinescript
     fill(plot1, plot2, color=color.blue, transp=90)
     ```
   - *Indie:* Uses the `@fill` decorator for similar functionality.
     ```python
     @fill("My Fill", plot1, plot2, color="blue", transp=90)
     ```

8. **Bar Colors:**
   - *Pine Script:* Changes bar colors with `barcolor()`.
     ```pinescript
     barcolor(color=color.red)
     ```
   - *Indie:* Applies bar coloring using the `@bar_color` decorator.
     ```python
     @bar_color("red")
     ```

9. **Background Coloring:**
   - *Pine Script:* Sets background color with `bgcolor()`.
     ```pinescript
     bgcolor(color=color.gray)
     ```
   - *Indie:* Uses the `@background_color` decorator.
     ```python
     @background_color("gray")
     ```

10. **Labels and Text:**
    - *Pine Script:* Creates labels using `label.new()`.
      ```pinescript
      label.new(x=bar_index, y=high, text="Label")
      ```
    - *Indie:* Implements labels with the `@label` decorator.
      ```python
      @label("My Label", x=bar_index, y=high, text="Label")
      ```

---

 ## 7\. Control Flow & Logic

This section examines the control flow structures and logical operations in both Pine Script and Indie, focusing on their syntax, capabilities, and key differences.

| **Concept** | **Pine Script** | **Indie** | **Notes** |
| ------- | ----------- | ----- | ----- |
| **If Statement** | `if condition`<br>`    statement` | `if condition:`<br>`    statement` | Both languages use similar `if` statements; Indie requires a colon and indentation. |
| **Else / Else If** | `if condition`<br>`    statement`<br>`else if condition2`<br>`    statement2`<br>`else`<br>`    statement3` | `if condition:`<br>`    statement`<br>`elif condition2:`<br>`    statement2`<br>`else:`<br>`    statement3` | Indie uses `elif` instead of `else if`, similar to Python syntax. |
| **Ternary Operator** | `value = condition ? result1 : result2` | `value = result1 if condition else result2` | Both languages support ternary operations; Indie follows Python's syntax. |
| **For Loop** | `for i = start to end`<br>`    statement` | `for i in range(start, end + 1):`<br>`    statement` | Pine Script uses `to` for range; Indie uses `range()` function. |
| **While Loop** | *Not supported* | `while condition:`<br>`    statement` | Pine Script does not support `while` loops; Indie supports them similar to Python. |
| **Break Statement** | `break` (within `for` loop) | `break` (within loops) | Both languages support `break` to exit loops prematurely. |
| **Continue Statement** | *Not supported* | `continue` (within loops) | Pine Script lacks `continue`; Indie supports it to skip to the next iteration. |
| **Switch Statement** | `switch expression`<br>`    expression1 => statement1`<br>`    expression2 => statement2`<br>`    => defaultStatement` | *Not directly supported* | Pine Script supports `switch`; Indie requires multiple `if-elif-else` statements. |
| **Logical Operators** | `and`, `or`, `not` | `and`, `or`, `not` | Both languages use similar logical operators. |
| **Comparison Operators** | `<`, `<=`, `>`, `>=`, `==`, `!=` | `<`, `<=`, `>`, `>=`, `==`, `!=` | Standard comparison operators are the same in both languages. |

**Detailed Explanation:**

1. **If Statement:**
    * *Pine Script:* Uses the `if` keyword followed by the condition and an indented block.

        ```
        if close > open
            bgcolor(color.green)
        
        ```
    * *Indie:* Similar syntax but requires a colon after the condition.

        ```
        if self.close > self.open:
            self.bgcolor("green")
        
        ```
2. **Else / Else If:**
    * *Pine Script:* Uses `else if` for additional conditions.

        ```
        if close > open
            bgcolor(color.green)
        else if close < open
            bgcolor(color.red)
        else
            bgcolor(color.gray)
        
        ```
    * *Indie:* Uses `elif`, following Python's convention.

        ```
        if self.close > self.open:
            self.bgcolor("green")
        elif self.close < self.open:
            self.bgcolor("red")
        else:
            self.bgcolor("gray")
        
        ```
3. **Ternary Operator:**
    * *Pine Script:* Uses the `? :` syntax.

        ```
        bgcolor(close > open ? color.green : color.red)
        
        ```
    * *Indie:* Follows Python's `if-else` expression.

        ```
        self.bgcolor("green" if self.close > self.open else "red")
        
        ```
4. **For Loop:**
    * *Pine Script:* Iterates over a range using `to`.

        ```
        for i = 0 to 9
            sum := sum + close[i]
        
        ```
    * *Indie:* Uses `range()` function.

        ```
        for i in range(10):
            sum += self.close[i]
        
        ```
5. **While Loop:**
    * *Pine Script:* Does not support `while` loops.
    * *Indie:* Supports `while` loops similar to Python.

        ```
        while condition:
            # statements
        
        ```
6. **Break Statement:**
    * *Pine Script:* Allows `break` within `for` loops to exit prematurely.
    * *Indie:* Supports `break` in all loop types.
7. **Continue Statement:**
    * *Pine Script:* Does not support `continue`.
    * *Indie:* Supports `continue` to skip the rest of the code in the current loop iteration.
8. **Switch Statement:**
    * *Pine Script:* Supports `switch` statements.

        ```
        switch condition
            1 => statement1
            2 => statement2
            => defaultStatement
        
        ```
    * *Indie:* Does not have a `switch` statement; uses multiple `if-elif-else` instead.
9. **Logical Operators:**
    * Both languages use `and`, `or`, and `not` for logical operations.
10. **Comparison Operators:**
    * Both languages share standard comparison operators: `<`, `<=`, `>`, `>=`, `==`, `!=`.

***

 ## 8\. Functions & Methods

### 🔸 Overview Table

| **Concept** | **Pine Script** | **Indie** | **Notes** |
| ------- | ----------- | ----- | ----- |
| Function definition | `f() => expr` | `def f(...): return ...` | Pine uses single-line arrow functions; Indie supports full Python syntax. |
| Multiline function | Not supported | ✅ Yes | Indie allows multiline functions with indentation. |
| Function scope | Global only (cannot nest inside `if`, `for`, etc.) | Python rules — local or module-level | Indie allows nested/local helpers, standard Python rules apply. |
| Arguments (default) | `f(x = 14)` | `def f(x=14):` | Both support default arguments. |
| Named return values | No | ✅ Yes | Indie can return named results via `return` dicts or tuples. |
| Anonymous/lambda funcs | ❌ Not available | ❌ Not available | Not available |
| Calling built-in funcs | `ta.sma(close, 14)` | `Sma.new(self.close, 14)` | Indie uses class-based construction for indicator functions. |
| Method calling on objects | Not used | Yes, e.g., `algo.update()` | Indie uses OOP patterns for stateful indicators or helper classes. |

***

### 🔹 Examples

#### 1\. Simple One\-Line Function

**Pine Script:**

```
f(x) => x * x
plot(f(close))
```

**Indie:**

```
def f(x):
    return x * x

return f(self.close[0])
```

***

#### 2\. Function With Default Parameter

**Pine Script:**

```
double(x = 2) => x * 2
```

**Indie:**

```
def double(x=2):
    return x * 2
```

***

#### 3\. Multiline Function \(Only Indie\)

**Indie:**

```
def average_highs(source, length):
    total = 0.0
    for i in range(length):
        total += source[i]
    return total / length
```

🔹 *Equivalent logic in Pine would require inline loops or `ta.sma()`.*

***

#### 4\. Return Multiple Values

**Pine Script:**

```
f(x, y) =>
    x + y, x - y

[a, b] = f(5, 3)
```

**Indie:**

```
def f(x, y):
    return x + y, x - y

a, b = f(5, 3)
```

***

#### 5\. Calling Indicator Functions

**Pine Script:**

```
ema = ta.ema(close, 20)
```

**Indie:**

```
ema = Ema.new(self.close, 20)
```

***

#### 6\. Calling Object Methods \(Indie only\)

```
rsi = Rsi.new(self.close, 14)
value = rsi[0]  # access latest value
```

#### 

## 9\. Multi\-Timeframe & Security Access

This section compares how Pine Script and Indie handle access to other timeframes and instruments (multi-timeframe or MTF logic), focusing on functions like `request.security()` in Pine and `@sec_context`/`calc_on()` in Indie.

***

| **Concept** | **Pine Script** | **Indie** | **Notes** |
| ------- | ----------- | ----- | ----- |
| **Accessing another timeframe** | `request.security(syminfo.tickerid, "D", close)` | `@sec_context` + `calc_on()` | Indie uses a decorator/context system instead of a single function call. |
| **Accessing another symbol** | `request.security("AAPL", "D", close)` | `calc_on(Sec, time_frame="D", symbol="AAPL")` | Symbol access is done via argument inside `calc_on()`. |
| **Bar reference in MTF** | `... [1]`, `... [0]` | MTF result is returned as Series — you can index `[0]` or `[1]` as usual | Identical indexing approach. |
| **Custom logic per timeframe** | Body inside `request.security()` lambda | Define a `@sec_context` function | MTF logic is modularized into its own context-decorated function. |
| **Multi-value return** | `request.security(..., expression=[a, b])` | `def Sec(self): return a, b` → `a, b = self.calc_on(Sec, time_frame="D")` | Multiple outputs are supported in both, but Indie uses tuple unpacking. |
| **Intraday to daily sync** | `request.security(syminfo.tickerid, "D", close)[1]` | `daily = self.calc_on(Sec, time_frame="D"); prev_close = daily[1]` | Equivalent logic — just requires naming the result. |

***

### 🔹 Code Examples

#### 1\. Daily Close on Intraday Chart

**Pine Script:**

```
daily_close = request.security(syminfo.tickerid, "D", close)
```

**Indie:**

```
@sec_context
def Sec(self):
    return self.close[0]

daily_close = self.calc_on(Sec, time_frame="D")
```

***

#### 2\. Daily Previous Close

**Pine Script:**

```
prev_daily_close = request.security(syminfo.tickerid, "D", close)[1]
```

**Indie:**

```
daily = self.calc_on(Sec, time_frame="D")
prev_daily_close = daily[1]
```

***

#### 3\. Multi\-Value Return \(e\.g\.\, high & low\)

**Pine Script:**

```
[hh, ll] = request.security(syminfo.tickerid, "D", [high, low])
```

**Indie:**

```
@sec_context
def Sec(self):
    return self.high[0], self.low[0]

hh, ll = self.calc_on(Sec, time_frame="D")
```

***

#### 4\. Different Symbol \+ Timeframe

**Pine Script:**

```
spy_daily_close = request.security("SPY", "D", close)
```

**Indie:**

```
spy_daily_close = self.calc_on(Sec, time_frame="D", symbol="SPY")
```

***

#### 5\. MTF Condition \(e\.g\. daily trend confirmation\)

**Pine Script:**

```
daily_ema = request.security(syminfo.tickerid, "D", ta.ema(close, 20))
is_trend_up = close > daily_ema
```

**Indie:**

```
@sec_context
def Sec(self):
    return Ema.new(self.close, 20)[0]

daily_ema = self.calc_on(Sec, time_frame="D")
is_trend_up = self.close[0] > daily_ema[0]
```

###


***

## 10\. Alerts\, Labels & Tables


### 🔸 Overview Table

| **Concept** | **Pine Script** | **Indie** | **Notes** |
| ------- | ----------- | ----- | ----- |
| **Alert condition** | `alertcondition(cond, title, message)` | No built-in alerts — emulate via `plot.Marker()` or color signals | Indie is visualization-only; alerts must be defined visually (e.g. marker, color, label). |
| **Create label** | `label.new(x=bar_index, y=high, text="Signal")` | `@plot.marker(...)` with `marker_position`, `text`, `style` | Indie supports basic labels through marker plotting with position, color, and text. |
| **Label style** | `label.style_label_up`, `label.style_label_down` | `marker_style.LABEL` (only one label style) | Indie only supports a single label style (`LABEL`), unlike Pine’s many shapes. |
| **Label text** | `text="BUY"` | `@plot.marker(text="BUY")` | Indie allows `text` in markers, displayed at the marker location. |
| **Positioning** | `location.abovebar`, `location.belowbar`, `price` | `marker_position.ABOVE`, `BELOW`, `PRICE` | Position mapping is direct. |
| **Coloring marker** | `color=color.red` | `color="red"` or `color=color.RED` | Indie uses string or constant color codes. |
| **Conditional marker** | `plotshape(cond, style=shape.triangleup)` | `return plot.Marker(...) if cond else plot.Marker(nan)` | Indie uses `return` from function decorated with `@plot`. |
| **Table display** | `table.new(...)` | No native table object | Indie currently Not supported (simulate with markers) |

***

### 🔹 Code Examples

#### 1\. Conditional Alert Marker

**Pine Script:**

```
alertcondition(close > open, title="Bullish", message="Green candle!")
plotshape(close > open, style=shape.triangleup, color=color.green)
```

**Indie:**

```
@plot("Bullish Marker", style=marker_style.LABEL, color="green", marker_position=marker_position.ABOVE)
def bullish_marker(self):
    return plot.Marker(text="BUY") if self.close[0] > self.open[0] else plot.Marker(nan)
```

***

#### 2\. Label Marker with Text

**Pine Script:**

```
label.new(bar_index, high, text="Breakout", style=label.style_label_up, color=color.orange)
```

**Indie:**

```
@plot("Breakout Label", style=marker_style.LABEL, marker_position=marker_position.ABOVE)
def breakout_label(self):
    return plot.Marker(text="Breakout", color="orange") if breakout_cond else plot.Marker(nan)
```

***

#### 3\. Simulating Table Columns

**Pine Script:**

```
table = table.new(position.top_right, 2, 2)
table.cell(0, 0, "RSI")
table.cell(0, 1, str.tostring(rsi))
```

**Indie (Simulated via marker):**

```
@plot("Text Col 1", style=marker_style.LABEL, marker_position=marker_position.PRICE)
def text_col_1(self):
    return plot.Marker(text=f"RSI: {round(rsi_val)}", color="gray") if self.bar_index % 20 == 0 else plot.Marker(nan)
```

📌 *Note:* This isn't a real table — Indie doesn't support `table.new()` or similar layout grids yet. Instead, space-separated labels or columns of text/markers are used.


## 11\. Color System & Styling

| **Feature** | **Pine Script** | **Indie** | **Notes** |
| ------- | ----------- | ----- | ----- |
| **Predefined colors** | `color.red`, `color.blue`, `color.green`, etc. | `color.RED`, `color.BLUE`, `color.GREEN` | Indie uses `color.` constants in ALL CAPS. |
| **Custom RGB color** | `color.rgb(r, g, b)` | `color.rgba(r, g, b, a)` | Pine transparency = 0–100; Indie uses `a = 0.0–1.0` (alpha). |
| **Transparency (opacity)** | `color.new(color.red, 90)` | `color.rgba(255, 0, 0, 0.1)` | Pine uses transparency %; Indie uses alpha (1.0 = fully opaque). |
| **Hex color** | `#FF0000` | Not supported directly — must use `rgba()` | Indie requires RGBA declaration, no hex. |
| **Dynamic color** | `plot(x, color = x > 0 ? color.green : color.red)` | `return plot.Line(x, color = "green" if x > 0 else "red")` | Both support conditional colors; Indie uses Python-style ternary. |
| **Gradient (2 colors)** | `plot(x, color = x > y ? color.green : color.red)` | same as above | No dedicated `color.gradient()` — gradients simulated manually. |
| **Color variables** | `myColor = color.rgb(0,255,0)` | `my_color = color.rgba(0, 255, 0, 1.0)` | Assign as variable for re-use. |
| **Inline color** | `plot(x, color=color.red)` | `@plot(..., color='red')` | Set directly in decorator or inside return. |
| **Plot transparency** | `plot(x, transp=80)` | `color.rgba(..., 0.2)` | Indie has no `transp=` param — handled via RGBA. |

***

### ✅ Notes

* Indie always uses RGBA (Red, Green, Blue, Alpha) → `color.rgba(255, 0, 0, 0.75)`
* Alpha in Indie is **float between 0 and 1**
    (0 = fully transparent, 1 = opaque)

***

### 🔹 Code Examples

#### 1\. Transparent red line

**Pine Script:**

```
plot(close, color=color.new(color.red, 80))
```

**Indie:**

```
@plot("Red Line", color=color.rgba(255, 0, 0, 0.2))
def red_line(self):
    return self.close[0]
```

***

#### 2\. Dynamic color \(up/down\)

**Pine Script:**

```
plot(close, color=close > open ? color.green : color.red)
```

**Indie:**

```
@plot("Dynamic Color")
def dyn(self):
    return plot.Line(
        self.close[0],
        color="green" if self.close[0] > self.open[0] else "red"
    )
```


## 12\. Error Handling & NaN Detection

| **Feature** | **Pine Script** | **Indie** | **Notes** |
| ------- | ----------- | ----- | ----- |
| **Undefined value (NaN)** | `na` | `math.nan` | Both represent “not available” with language-specific constants. |
| **Check for NaN** | `na(x)` | `math.isnan(x)` | Indie uses Python’s `math.isnan()` function. |
| **Replace NaN with fallback** | `nz(x, 0)` | `x if not math.isnan(x) else 0` | Indie does not have `nz()`, you handle this manually. |
| **Raise error manually** | `runtime.error("Message")` | Not supported | Indie does not support runtime exceptions or explicit errors. |
| **Catch errors / try-except** | ❌ Not available | ❌ Not supported | Indie does not support Python `try`/`except` blocks. |
| **Divide by zero behavior** | Returns `na` (no crash) | Returns `math.nan` (safe, no crash) | Both languages are designed to fail silently — no hard crashes. |
| **Missing data fallback** | `x := na(x) ? 0 : x` | `x = 0 if math.isnan(x) else x` | Both require conditional assignment for fallbacks. |
| **Silent fail prevention** | `na(x)` guards before plot | `if not math.isnan(x): return plot.Line(x)` | Use guard condition before plotting or calculation. |
| **isfinite() / isinf()** | ❌ Not available | `math.isfinite(x)`, `math.isinf(x)` | Indie can use Python `math` module for additional safety checks. |

***

### 🔹 Code Examples

#### 1\. Basic NaN Check

**Pine Script:**

```
plot(na(close) ? 0 : close)
```

**Indie:**

```
@plot("Clean Close")
def clean_close(self):
    return 0 if math.isnan(self.close[0]) else self.close[0]
```

***

#### 2\. Safe divide \(avoid div by zero\)

**Pine Script:**

```
safe_div = close / (volume == 0 ? na : volume)
```

**Indie:**

```
vol = self.volume[0]
safe_div = self.close[0] / vol if vol != 0 else math.nan
```

***

#### 3\. Plot only if valid

**Pine Script:**

```
plot(na(x) ? na : x)
```

**Indie:**

```
@plot("Safe Plot")
def safe(self):
    return plot.Line(self.x[0]) if not math.isnan(self.x[0]) else plot.Line(math.nan)
```

***

<br>
## 13\. Built\-in Constants & System Variables

| **Concept** | **Pine Script** | **Indie** | **Notes** |
| ------- | ----------- | ----- | ----- |
| **Bar index** | `bar_index` | `self.bar_index` | Current bar number (0-based). Identical meaning. |
| **Timestamp (bar time)** | `time` | `self.time` | Epoch timestamp (ms in Pine, datetime in Indie). |
| **Time (human)** | `year`, `month`, `dayofmonth`, `hour` | `self.time.year`, `self.time.month`, etc. | Indie uses Python `datetime` methods on `self.time`. |
| **Session check** | `session.isfirst` | `self.bar_index == 0` | No built-in session flags in Indie — emulate manually. |
| **Symbol name** | `syminfo.ticker` | `self.symbol` | Active instrument name. |
| **Resolution / timeframe** | `syminfo.resolution` | `self.time_frame` | Chart resolution ("1D", "5", etc.). |
| **Instrument full ID** | `syminfo.tickerid` | Not available | Indie exposes `symbol`, not exchange info. |
| **Chart type / scale info** | `chart.*` (e.g. `chart.style_line`) | Not available | Indie currently lacks access to chart styling settings. |
| **Timezone info** | `timenow`, `timestamp()` | `datetime.now()` or `self.time` | Indie uses standard `datetime` objects. |

***

### 🔹 Code Examples

#### 1\. Detect First Bar

**Pine Script:**

```
if bar_index == 0
    label.new(bar_index, high, text="First bar")
```

**Indie:**

```
@plot("First Bar Label", style=marker_style.LABEL, marker_position=marker_position.ABOVE)
def first_bar(self):
    return plot.Marker(text="First") if self.bar_index == 0 else plot.Marker(math.nan)
```

***

#### 2\. Print Time Info

**Pine Script:**

```
plotchar(year, "Year")
```

**Indie:**

```
@plot("Year", style=marker_style.LABEL, marker_position=marker_position.ABOVE)
def year_marker(self):
    return plot.Marker(text=str(self.time.year)) if self.bar_index % 20 == 0 else plot.Marker(math.nan)
```

***

#### 3\. Symbol & Resolution

**Pine Script:**

```
label.new(bar_index, high, text=syminfo.ticker + " - " + syminfo.resolution)
```

**Indie:**

```
@plot("Symbol", style=marker_style.LABEL, marker_position=marker_position.ABOVE)
def symbol_label(self):
    return plot.Marker(text=f"{self.symbol} / {self.time_frame}")
```

***

 

 

 
