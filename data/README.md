# Data Directory (data/)

## Overview
Local data storage for raw API responses, processed datasets, and exported reports.

**Note:** Data files are not tracked in git (.gitignore).

## Structure
```
data/
├── raw/             # Raw API responses
├── processed/       # Processed and cleaned datasets
└── exports/         # Generated reports and visualizations
```

## Directories

### raw/
Original, unmodified API responses stored for reference and replay.

**Format:** JSON files named by data type and timestamp
```
raw/
├── teams_20240115_120000.json
├── games_20240115.json
├── player_8478402_20240115.json
└── standings_20240115.json
```

**Usage:**
- Audit trail
- Replay data collection
- Debugging
- Testing

### processed/
Cleaned and transformed data ready for analysis.

**Format:** CSV, Parquet, or JSON files
```
processed/
├── player_stats_20232024.csv
├── team_stats_20232024.csv
├── game_results_20232024.parquet
└── standings_history.csv
```

**Usage:**
- Analytics input
- Machine learning training
- Report generation
- Data distribution

### exports/
Generated reports, visualizations, and exports.

**Format:** Various (PDF, PNG, CSV, Excel)
```
exports/
├── season_report_20232024.pdf
├── player_rankings_20240115.csv
├── shot_chart_8478402.png
└── team_dashboard_TOR.html
```

**Usage:**
- Sharing analysis results
- Presentations
- Archiving reports
- External distribution

## Data Management

### Storage Guidelines
- Use descriptive file names with dates
- Compress large files
- Clean old files periodically
- Document data schema

### File Naming Convention
```
{data_type}_{identifier}_{timestamp}.{extension}

Examples:
- games_20240115_120000.json
- player_stats_8478402_20232024.csv
- team_report_TOR_20240115.pdf
```

### Data Retention
- Raw data: Keep for audit (30-90 days)
- Processed data: Keep current season + last 2 seasons
- Exports: Archive important reports, delete temporary files

## Data Pipeline

```
NHL API
   ↓
[data/raw/]          ← Store raw responses
   ↓
Processing
   ↓
[data/processed/]    ← Store cleaned data
   ↓
Analytics
   ↓
[data/exports/]      ← Store reports/visualizations
```

## Usage Examples

### Save Raw Data
```python
import json
from datetime import datetime

def save_raw_data(data, data_type):
    timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
    filename = f"data/raw/{data_type}_{timestamp}.json"
    with open(filename, 'w') as f:
        json.dump(data, f, indent=2)
```

### Load Processed Data
```python
import pandas as pd

df = pd.read_csv("data/processed/player_stats_20232024.csv")
```

### Export Report
```python
from src.visualizations.reports import generate_report

report = generate_report(data)
report.save("data/exports/season_report_20232024.pdf")
```

## Best Practices

### Organization
- Keep related data together
- Use subdirectories for different seasons
- Maintain consistent naming

### Documentation
- Include README in data subdirectories
- Document data schema
- Note data sources and collection dates

### Security
- Never commit sensitive data
- Ensure .gitignore excludes data/
- Be cautious with data exports

### Performance
- Use efficient formats (Parquet for large datasets)
- Compress historical data
- Index frequently queried fields

## Cleanup Script

```python
# scripts/cleanup_data.py
import os
from datetime import datetime, timedelta

def cleanup_old_files(directory, days=90):
    """Remove files older than specified days."""
    cutoff = datetime.now() - timedelta(days=days)
    
    for filename in os.listdir(directory):
        filepath = os.path.join(directory, filename)
        if os.path.getmtime(filepath) < cutoff.timestamp():
            os.remove(filepath)
            print(f"Removed: {filename}")
```

## Related Documentation
- [src/collectors/](../src/collectors/) - Data collection
- [src/analytics/](../src/analytics/) - Data processing
