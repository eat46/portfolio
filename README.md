# Portfolio

Frontend case studies from my work on fintech and stock analysis tools.

## Migrating a Legacy React App from CRA to Vite (React 16 → React 19)

Led the migration of a data-heavy financial dashboard from Create React App (React 16.8) to Vite (React 19) — a project touching the build system, routing, and a decade of accumulated dependencies.

One of the trickiest issues was React Router v6/v7's removal of the `withRouter` HOC, which many legacy class components relied on to access `history`, `location`, and `match` as props. I built a custom polyfill to preserve the old API surface, but an early version caused an infinite re-render loop — the app would crash with "Too many calls to Location or History APIs." The root cause was that the polyfilled `history` object was a new reference on every render, and several components used it as a `useEffect` dependency. Wrapping it in `useMemo` resolved the instability.

Another core challenge was Vite's reliance on native ES Modules, which broke CommonJS-style `require()` calls used throughout the Highcharts integration. These needed to be rewritten module-by-module into ES6 imports.

Perhaps the most important decision, though, was knowing when *not* to upgrade. Both D3 and Highcharts had newer major versions available, but bumping them introduced breaking changes — D3 v7's removal of the global `d3.event` broke several custom chart components, and major Highcharts version jumps risked visual regressions across dozens of chart types already in production. Rather than chasing the latest versions, I evaluated the actual risk-to-benefit ratio and kept these libraries pinned, prioritizing stability for a production financial tool over technical novelty.

## Performance Optimization for Multi-Chart Comparison Dashboard

Built a peer comparison dashboard displaying 10+ charts simultaneously — covering revenue forecasts, profitability trends, valuation multiples, and capital expenditure — each comparing a selected stock against multiple industry peers across different time ranges.

Rendering this many data-heavy charts at once created a real performance bottleneck. To solve this, I implemented lazy rendering using the Intersection Observer API: charts only fetch data and initialize once they scroll into the viewport, significantly reducing initial load time and unnecessary computation for charts the user might never scroll to.

For several charts, I also built a toggle between an absolute comparison view (each peer plotted individually) and a relative view that aggregates all other peers into a single "industry total" series, contrasted against the selected stock. This aggregation logic — summing and reshaping peer data into a two-series comparison — is computed entirely on the frontend, allowing users to instantly reframe the analysis from "how do we compare to each peer" to "how do we compare to the market as a whole."

## Multi-Dimensional Stock Screening Radar Chart

Built a radar chart using D3.js to help users evaluate a stock's strength across multiple proprietary indicators — such as foreign/investment trust buying momentum, institutional flow trends, and revenue growth — at a glance.

D3 was chosen over Highcharts for this feature because it offered more flexibility in both shape rendering and interaction handling, which was necessary given the custom scoring logic involved. One of the trickier implementation details was label alignment: since indicator names vary significantly in length, positioning them cleanly around the circular axis without overlap or visual imbalance required careful adjustment.

The chart works alongside a separate hard-filter system — users first narrow down stocks using binary screening criteria (e.g., volume thresholds, technical signals), then the radar chart visualizes how the filtered stock performs across ten weighted dimensions, with raw business metrics converted into normalized 0-100 scores by the backend.

## Financial Statement Visualization with Interactive Treemap

Built a financial ratio treemap using Highcharts to help investors instantly identify a company's performance trends — turning dense financial data into an intuitive visual overview.

The core design challenge was the color logic: rather than using absolute values, each indicator's color reflects the delta between the earliest and latest data points in the selected time range — with red indicating growth and green indicating decline, following Taiwan market conventions. Color opacity scales with the magnitude of change, allowing users to spot outliers at a glance without reading individual numbers.

The system supports unlimited cross-category and cross-stock comparisons, with indicators selectable across custom-defined groups. A patented drag-and-drop interaction allows users to pull any indicator directly onto a comparison chart for on-the-fly multi-metric analysis.
