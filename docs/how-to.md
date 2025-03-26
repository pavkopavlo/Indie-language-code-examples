### How can I create a filled area between two plot lines with custom transparency in Indie?

I'm working with **Indie** and want to create a filled area between two moving averages. I know that `@plot.fill` can be used for this, but I also need to control the transparency of the fill color.
How can I achieve this?

### Solution:

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

### Explanation:

* The `@plot.line` decorators define two plot lines (`fast_ma` and `slow_ma`).
* The `@plot.fill` decorator references these lines to create a filled area between them.
* Transparency is controlled using `color.PURPLE(0.3)`, where `0.3` represents **30% opacity**.
* The `calc` method returns the moving averages and a `plot.Fill()` object.

### Customization:

* Adjust transparency by modifying the alpha value (e.g., `color.PURPLE(0.5)` for 50% opacity).
* Change colors as needed, using predefined colors or custom RGBA values.