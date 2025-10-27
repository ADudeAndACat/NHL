# Scripts (scripts/)

## Overview
Utility scripts for database setup, data collection, maintenance, and operations.

## Scripts

### setup_db.py
Initialize the database with schema and initial data.

```bash
python scripts/setup_db.py
```

**Features:**
- Create database tables
- Run initial migrations
- Load seed data (teams, etc.)
- Verify database connection

### fetch_historical.py
Fetch historical NHL data for past seasons.

```bash
python scripts/fetch_historical.py --season 20232024
python scripts/fetch_historical.py --start 20202021 --end 20232024
```

**Options:**
- `--season` - Single season to fetch
- `--start` - Start season for range
- `--end` - End season for range
- `--data-type` - games, players, stats, all

### generate_reports.py
Generate analytical reports and exports.

```bash
python scripts/generate_reports.py --type season-summary
python scripts/generate_reports.py --type player-rankings --season 20232024
```

**Report Types:**
- season-summary
- player-rankings
- team-stats
- custom

### update_data.py
Update existing data with latest information.

```bash
python scripts/update_data.py
```

**Updates:**
- Current season stats
- Player rosters
- Standings
- Recent games

### backup_database.py
Create database backup.

```bash
python scripts/backup_database.py --output backup.sql
```

### validate_data.py
Validate data integrity and consistency.

```bash
python scripts/validate_data.py
```

**Checks:**
- Missing required fields
- Data consistency
- Referential integrity
- Duplicate records

## Creating New Scripts

### Script Template
```python
#!/usr/bin/env python3
"""
Script description.

Usage:
    python scripts/my_script.py --option value
"""
import argparse
import logging
from src.utils.logging import setup_logging

logger = logging.getLogger(__name__)

def main():
    """Main script logic."""
    parser = argparse.ArgumentParser(description="Script description")
    parser.add_argument("--option", help="Option description")
    args = parser.parse_args()
    
    # Script logic here
    logger.info("Script started")
    # ...
    logger.info("Script completed")

if __name__ == "__main__":
    setup_logging()
    main()
```

## Best Practices

### Error Handling
- Handle errors gracefully
- Log errors with context
- Provide clear error messages
- Exit with appropriate codes

### Logging
- Log start and completion
- Log progress for long operations
- Use appropriate log levels
- Include timestamps

### Configuration
- Use command-line arguments
- Support environment variables
- Provide sensible defaults
- Validate inputs

### Idempotency
- Scripts should be safe to run multiple times
- Check existing data before inserting
- Use upsert operations where appropriate

## Scheduling

### Using Cron (Linux/Mac)
```bash
# Daily data update at 6 AM
0 6 * * * /path/to/.venv/bin/python /path/to/scripts/update_data.py

# Weekly backup on Sunday at midnight
0 0 * * 0 /path/to/.venv/bin/python /path/to/scripts/backup_database.py
```

### Using Task Scheduler (Windows)
Create scheduled task to run scripts at specified times.

### Using Python APScheduler
```python
from apscheduler.schedulers.blocking import BlockingScheduler

scheduler = BlockingScheduler()

@scheduler.scheduled_job('cron', hour=6)
def daily_update():
    # Run update script
    pass

scheduler.start()
```

## Related Documentation
- [DESIGN.md](../DESIGN.md) - System design
- [src/collectors/](../src/collectors/) - Data collection modules
