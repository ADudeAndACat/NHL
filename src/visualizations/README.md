# Visualizations (src/visualizations/)

## Overview
Create interactive and static visualizations for NHL analytics.

## Components

### charts.py
- Line charts (trends over time)
- Bar charts (comparisons)
- Scatter plots (correlations)
- Box plots (distributions)

### dashboards.py
- Team performance dashboard
- Player comparison tool
- League-wide statistics
- Live game tracker

### heatmaps.py
- Shot charts
- Ice rink visualizations
- Performance heat maps

### reports.py
- Generate static reports
- Export visualizations
- PDF/PNG exports

## Usage Examples

```python
from src.visualizations.charts import create_player_trend_chart
from src.visualizations.dashboards import PlayerDashboard

# Create chart
chart = create_player_trend_chart(player_id=8478402)
chart.show()

# Create dashboard
dashboard = PlayerDashboard(player_id=8478402)
dashboard.render()
```

## Technologies
- Plotly - Interactive charts
- Matplotlib - Static charts
- Seaborn - Statistical visualizations
- Streamlit/Dash - Dashboard frameworks

## Related Documentation
- [DESIGN.md](../../DESIGN.md) - See Section 3.4 for Visualization design
