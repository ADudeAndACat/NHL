# Source Code (src/)

## Overview
This directory contains the main application code for the NHL Data Analysis & Visualization System, organized into functional modules.

## Structure
```
src/
├── api/              # NHL API interaction layer
├── collectors/       # Data fetching and collection
├── database/         # ORM models and database operations
├── analytics/        # Statistical analysis and metrics
├── visualizations/   # Chart and dashboard generation
├── web/             # Web interface and API endpoints
└── utils/           # Shared utilities and helpers
```

## Modules

### api/
NHL API client implementation:
- `client.py` - Base HTTP client for API communication
- `endpoints.py` - API endpoint definitions and wrappers
- `models.py` - Pydantic models for API response validation
- `cache.py` - Response caching logic

### collectors/
Data collection modules:
- `games.py` - Game data and play-by-play collector
- `players.py` - Player statistics collector
- `teams.py` - Team information collector
- `standings.py` - League standings collector
- `schedule.py` - Schedule and calendar collector

### database/
Database layer:
- `models.py` - SQLAlchemy ORM models
- `connection.py` - Database connection management
- `queries.py` - Common query patterns
- `migrations/` - Database migration scripts

### analytics/
Analysis engines:
- `player_stats.py` - Player performance analytics
- `team_stats.py` - Team performance analytics
- `predictions.py` - Predictive models (ML)
- `trends.py` - Trend analysis
- `comparisons.py` - Comparative analysis tools

### visualizations/
Visualization components:
- `charts.py` - Chart generation (Plotly/Matplotlib)
- `dashboards.py` - Dashboard layouts
- `heatmaps.py` - Heat map visualizations
- `reports.py` - Report generation

### web/
Web interface:
- `app.py` - Main application entry point
- `routes.py` - API routes and endpoints
- `templates/` - HTML templates
- `static/` - CSS, JavaScript, images

### utils/
Shared utilities:
- `logging.py` - Logging configuration
- `config.py` - Configuration management
- `helpers.py` - Helper functions

## Development Guidelines

### Code Style
- Follow PEP 8 guidelines
- Use type hints for all functions
- Write comprehensive docstrings
- Keep functions focused and single-purpose

### Testing
- Write unit tests for all modules
- Mock external API calls
- Aim for >80% code coverage

### Documentation
- Update module docstrings
- Document complex algorithms
- Add usage examples

## Getting Started

### Adding a New Module
1. Create the module file in the appropriate directory
2. Add necessary imports
3. Implement functionality
4. Write tests in `tests/`
5. Update this README if needed

### Example Module Structure
```python
"""
Module description.
"""
from typing import Optional
import logging

logger = logging.getLogger(__name__)

class MyClass:
    """Class description."""
    
    def __init__(self):
        """Initialize."""
        pass
    
    def my_method(self, param: str) -> Optional[str]:
        """
        Method description.
        
        Args:
            param: Parameter description
            
        Returns:
            Return value description
        """
        pass
```

## Related Documentation
- [DESIGN.md](../DESIGN.md) - Comprehensive design document
- [PROJECT_STRUCTURE.md](../PROJECT_STRUCTURE.md) - Complete project structure
- Individual module READMEs for detailed information
