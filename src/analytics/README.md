# Analytics Engine (src/analytics/)

## Overview
Statistical analysis and advanced metrics for NHL data. Processes stored data to generate insights, trends, and predictions.

## Components

### player_stats.py
- Career statistics aggregation
- Performance trends and streaks
- Advanced metrics (Corsi, Fenwick, PDO)
- Player comparisons

### team_stats.py
- Win/loss analysis
- Home vs away performance
- Special teams analysis
- Head-to-head records

### predictions.py
- Game outcome predictions
- Player performance forecasting
- Playoff probability calculations

### trends.py
- Hot/cold streak detection
- Performance over time
- Seasonal patterns

### comparisons.py
- Player vs player comparisons
- Team vs team comparisons
- Statistical rankings

## Usage Examples

```python
from src.analytics.player_stats import PlayerAnalytics
from src.analytics.team_stats import TeamAnalytics

# Player analysis
analytics = PlayerAnalytics()
trends = analytics.analyze_player_trends(8478402, "20232024")

# Team analysis
team_analytics = TeamAnalytics()
efficiency = team_analytics.calculate_team_efficiency("TOR", "20232024")
```

## Technologies
- pandas - Data manipulation
- numpy - Numerical computing
- scikit-learn - Machine learning
- statsmodels - Statistical analysis

## Related Documentation
- [DESIGN.md](../../DESIGN.md) - See Section 3.3 for Analytics design
