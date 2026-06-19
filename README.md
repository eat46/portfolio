# portfolio
Frontend case studies from my work on fintech and stock analysis tools.

# Frontend Case Studies

A collection of technical case studies from my work building financial analysis tools.

## Financial Statement Visualization with Interactive Treemap

Financial Statement Visualization with Interactive Treemap
Built a financial ratio treemap using Highcharts to help investors instantly identify a company's performance trends — turning dense financial data into an intuitive visual overview.
The core design challenge was the color logic: rather than using absolute values, each indicator's color reflects the delta between the earliest and latest data points in the selected time range — with red indicating growth and green indicating decline, following Taiwan market conventions. Color opacity scales with the magnitude of change, allowing users to spot outliers at a glance without reading individual numbers.
The system supports unlimited cross-category and cross-stock comparisons, with indicators selectable across custom-defined groups. A patented drag-and-drop interaction allows users to pull any indicator directly onto a comparison chart for on-the-fly multi-metric analysis.
