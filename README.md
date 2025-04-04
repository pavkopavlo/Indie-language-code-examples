# Indie Language Code Examples

# Unofficial Indie language community page

### What is TakeProfit?

**TakeProfit** is a browser-based research and trading platform for retail traders and independent analysts. It has real-time charting, a flexible workspace and custom analytics tooling. TakeProfit supports a wide range of technical and fundamental analysis use cases, with a modular UI and community oriented research sharing. Key features:

* Widget based workspace for organizing charts, watchlists, notes and tools.
* WebGL powered chart engine with multiple chart types, overlays and drawing tools.
* Integrated screener with 80+ fundamental and technical parameters.
* Real-time data (via integrated broker feeds).

### What is Indie?

**Indie** is TakeProfit’s native scripting language for creating custom indicators and analytics tools. It’s a Python based DSL for time-series data in trading. Key features:

* Declarative decorators for defining indicators (`@indicator`, `@plot`, `@param`, etc.).
* Metadata and execution logic separation for better readability and modularity.
* `@ctx_func` for multi-instrument calculations (equivalent to `request.security` in Pine Script).
* Native cloud runtime – no local compilation or deployment required.
* Script sharing, versioning and monetization through the TakeProfit Marketplace.

Indie is designed to balance accessibility (for users familiar with Python) with flexibility and power for advanced custom analytics.

## Essentials

[Indie language official docs](https://takeprofit.com/docs/indie/What-is-Indie)

## Indicators

[Education indicators](https://github.com/pavkopavlo/Indie-language-code-examples/tree/master/indicators)

[Platform Built-ins](https://github.com/pavkopavlo/Indie-language-code-examples/blob/master/indicators/_Built-ins/_Built-ins.indie5)

[Community Indicators](https://takeprofit.com/indicators) 

## FAQ / Issues

[Indie FAQ / solutions](https://github.com/pavkopavlo/Indie-language-code-examples/blob/master/docs/indie-FAQ.md)

[Indie StackOverflow Issues/Solutions](https://stackoverflow.com/questions/tagged/indie)

## Articles

[Step-by-Step Guide: Rewriting Indicators from Pine Script to Indie](https://www.reddit.com/r/IndieLang/comments/1j4xss4/stepbystep_guide_rewriting_indicators_from_pine/)


![TakeProfit.com Platform](image.png)