# Portfolio

Frontend case studies from my work on fintech and stock analysis tools.


## Financial Statement Visualization with Interactive Treemap

Built a financial ratio treemap using Highcharts to help investors instantly identify a company's performance trends — turning dense financial data into an intuitive visual overview.

The core design challenge was the color logic: rather than using absolute values, each indicator's color reflects the delta between the earliest and latest data points in the selected time range — with red indicating growth and green indicating decline, following Taiwan market conventions. Color opacity scales with the magnitude of change, allowing users to spot outliers at a glance without reading individual numbers.

The system supports unlimited cross-category and cross-stock comparisons, with indicators selectable across custom-defined groups. A patented drag-and-drop interaction allows users to pull any indicator directly onto a comparison chart for on-the-fly multi-metric analysis.

## Multi-Dimensional Stock Screening Radar Chart

Built a radar chart using D3.js to help users evaluate a stock's strength across multiple proprietary indicators — such as foreign/investment trust buying momentum, institutional flow trends, and revenue growth — at a glance.

D3 was chosen over Highcharts for this feature because it offered more flexibility in both shape rendering and interaction handling, which was necessary given the custom scoring logic involved. One of the trickier implementation details was label alignment: since indicator names vary significantly in length, positioning them cleanly around the circular axis without overlap or visual imbalance required careful adjustment.

The chart works alongside a separate hard-filter system — users first narrow down stocks using binary screening criteria (e.g., volume thresholds, technical signals), then the radar chart visualizes how the filtered stock performs across ten weighted dimensions, with raw business metrics converted into normalized 0-100 scores by the backend.
