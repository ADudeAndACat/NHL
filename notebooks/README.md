# Jupyter Notebooks (notebooks/)

## Overview
Interactive notebooks for data exploration, analysis, and experimentation.

## Structure
```
notebooks/
├── exploratory/     # Initial data exploration
└── analysis/        # In-depth analysis and reports
```

## Notebook Types

### Exploratory (exploratory/)
Quick data exploration and initial analysis:
- `01_api_exploration.ipynb` - Explore NHL API endpoints
- `02_data_structure.ipynb` - Understand data structures
- `03_initial_stats.ipynb` - Basic statistical overview

### Analysis (analysis/)
Detailed analysis and reports:
- `player_performance_analysis.ipynb` - Player statistics deep dive
- `team_trends.ipynb` - Team performance trends
- `advanced_metrics.ipynb` - Corsi, Fenwick, PDO calculations
- `season_report.ipynb` - End-of-season analysis

## Getting Started

### Install Jupyter
```bash
pip install jupyter jupyterlab
```

### Start Jupyter Lab
```bash
jupyter lab
```

### Start Jupyter Notebook
```bash
jupyter notebook
```

## Best Practices

### Notebook Organization
- One topic per notebook
- Clear markdown documentation
- Numbered prefixes for sequence
- Regular cells for imports

### Code Quality
- Import necessary libraries at the top
- Use meaningful variable names
- Add comments for complex logic
- Keep cells focused and small

### Documentation
- Use markdown cells liberally
- Explain analysis steps
- Document assumptions
- Include visualizations

## Example Notebook Structure

```python
# Cell 1: Title and Overview
"""
# Player Performance Analysis
## Overview
This notebook analyzes player performance trends for the 2023-24 season.
"""

# Cell 2: Imports
import pandas as pd
import numpy as np
import plotly.express as px
from src.database.queries import get_player_stats

# Cell 3: Load Data
player_id = 8478402  # Auston Matthews
season = "20232024"
stats = await get_player_stats(player_id, season)
df = pd.DataFrame(stats)

# Cell 4: Analysis
df['points'] = df['goals'] + df['assists']
df['game_number'] = range(1, len(df) + 1)

# Cell 5: Visualization
fig = px.line(df, x='game_number', y='points', title='Points per Game')
fig.show()

# Cell 6: Insights
"""
## Key Findings
- Player showed consistent performance
- Notable scoring streak in games 15-20
"""
```

## Converting to Production Code

When a notebook analysis is ready for production:
1. Clean up the code
2. Remove exploratory dead-ends
3. Extract functions to appropriate `src/` modules
4. Add tests in `tests/`
5. Document in `docs/`

## Sharing Notebooks

### Export to HTML
```bash
jupyter nbconvert --to html notebook.ipynb
```

### Export to PDF
```bash
jupyter nbconvert --to pdf notebook.ipynb
```

### Export to Python Script
```bash
jupyter nbconvert --to script notebook.ipynb
```

## Tips

### Restart Kernel
If you encounter issues, restart the kernel:
- `Kernel > Restart Kernel`

### Clear Output
Before committing notebooks:
- `Edit > Clear All Outputs`

### Auto-reload Modules
```python
%load_ext autoreload
%autoreload 2
```

## Resources
- [Jupyter Documentation](https://jupyter.org/documentation)
- [JupyterLab Documentation](https://jupyterlab.readthedocs.io/)
- [Pandas Documentation](https://pandas.pydata.org/)
