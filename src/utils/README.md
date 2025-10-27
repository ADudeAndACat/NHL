# Utilities (src/utils/)

## Overview
Shared utility functions and helpers used across the application.

## Components

### logging.py
Logging configuration and setup.

```python
from src.utils.logging import get_logger

logger = get_logger(__name__)
logger.info("Application started")
```

### config.py
Configuration management from environment variables and config files.

```python
from src.utils.config import get_config

config = get_config()
database_url = config.DATABASE_URL
```

### helpers.py
General helper functions.

```python
from src.utils.helpers import format_date, parse_season

date_str = format_date(date.today())
season = parse_season("20232024")
```

## Best Practices
- Keep utilities focused and reusable
- Avoid dependencies on other modules
- Write comprehensive docstrings
- Add unit tests for all utilities
