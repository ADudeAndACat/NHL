# Data Collectors (src/collectors/)

## Overview
This module contains data collection components that fetch NHL data from the API and prepare it for storage in the database. Each collector focuses on a specific type of NHL data.

## Collectors

### games.py
Collects game data including schedules, scores, and play-by-play information.

**Features:**
- Fetch game schedules by date or date range
- Retrieve live game data and scores
- Collect play-by-play data
- Extract box score statistics

**Example:**
```python
from src.collectors.games import GameCollector

collector = GameCollector()
games = await collector.fetch_games_by_date("2024-01-15")
```

### players.py
Collects player information and statistics.

**Features:**
- Player profiles and biographical data
- Season and career statistics
- Game logs
- Advanced metrics

**Example:**
```python
from src.collectors.players import PlayerCollector

collector = PlayerCollector()
player = await collector.fetch_player_profile(8478402)
stats = await collector.fetch_season_stats(8478402, "20232024")
```

### teams.py
Collects team information and statistics.

**Features:**
- Team profiles and metadata
- Current rosters
- Season statistics
- Historical records

**Example:**
```python
from src.collectors.teams import TeamCollector

collector = TeamCollector()
roster = await collector.fetch_team_roster("TOR")
stats = await collector.fetch_team_stats("TOR", "20232024")
```

### standings.py
Collects league standings information.

**Features:**
- Division standings
- Conference standings
- League-wide standings
- Historical standings

**Example:**
```python
from src.collectors.standings import StandingsCollector

collector = StandingsCollector()
standings = await collector.fetch_current_standings()
```

### schedule.py
Collects schedule and calendar information.

**Features:**
- Season schedules
- Team-specific schedules
- Playoff schedules
- Game status updates

**Example:**
```python
from src.collectors.schedule import ScheduleCollector

collector = ScheduleCollector()
schedule = await collector.fetch_season_schedule("20232024")
```

## Architecture

### Base Collector Pattern
All collectors inherit from a base collector class:

```python
from abc import ABC, abstractmethod
from src.api.client import NHLApiClient

class BaseCollector(ABC):
    def __init__(self):
        self.client = NHLApiClient()
        
    @abstractmethod
    async def fetch(self, *args, **kwargs):
        """Fetch data from API."""
        pass
        
    @abstractmethod
    async def process(self, raw_data):
        """Process raw API data."""
        pass
        
    @abstractmethod
    async def store(self, processed_data):
        """Store data in database."""
        pass
```

### Collector Workflow
1. **Fetch** - Retrieve raw data from NHL API
2. **Validate** - Validate data using Pydantic models
3. **Transform** - Convert to internal data format
4. **Store** - Persist to database

## Usage Examples

### Collecting Game Data
```python
from src.collectors.games import GameCollector
from datetime import date

async def collect_todays_games():
    collector = GameCollector()
    today = date.today()
    
    # Fetch games
    games = await collector.fetch_games_by_date(today)
    
    # Process and store
    for game in games:
        processed = await collector.process(game)
        await collector.store(processed)
        print(f"Stored: {game.away_team} @ {game.home_team}")
```

### Collecting Player Statistics
```python
from src.collectors.players import PlayerCollector

async def collect_player_stats(player_id: int, season: str):
    collector = PlayerCollector()
    
    # Fetch player data
    profile = await collector.fetch_player_profile(player_id)
    stats = await collector.fetch_season_stats(player_id, season)
    
    # Store in database
    await collector.store_player(profile)
    await collector.store_stats(stats)
    
    return profile, stats
```

### Bulk Collection
```python
from src.collectors.teams import TeamCollector

async def collect_all_rosters():
    collector = TeamCollector()
    
    # Get all teams
    teams = await collector.fetch_all_teams()
    
    # Collect rosters for each team
    for team in teams:
        roster = await collector.fetch_team_roster(team.abbreviation)
        await collector.store_roster(roster)
        print(f"Collected roster for {team.name}")
```

## Scheduled Collection

### Using APScheduler
```python
from apscheduler.schedulers.asyncio import AsyncIOScheduler
from src.collectors.games import GameCollector
from datetime import date

scheduler = AsyncIOScheduler()

async def daily_game_collection():
    collector = GameCollector()
    games = await collector.fetch_games_by_date(date.today())
    for game in games:
        await collector.store(game)

# Schedule to run daily at 6 AM
scheduler.add_job(daily_game_collection, 'cron', hour=6)
scheduler.start()
```

## Error Handling

### Retry Logic
```python
from tenacity import retry, stop_after_attempt, wait_exponential

class GameCollector(BaseCollector):
    @retry(
        stop=stop_after_attempt(3),
        wait=wait_exponential(multiplier=1, min=4, max=10)
    )
    async def fetch_game(self, game_id: int):
        return await self.client.get(f"/gamecenter/{game_id}/boxscore")
```

### Error Logging
```python
import logging

logger = logging.getLogger(__name__)

async def collect_with_error_handling(collector, game_id):
    try:
        game = await collector.fetch_game(game_id)
        await collector.store(game)
    except APIError as e:
        logger.error(f"Failed to collect game {game_id}: {e}")
    except Exception as e:
        logger.exception(f"Unexpected error collecting game {game_id}")
```

## Best Practices

### Data Validation
- Always validate API responses with Pydantic models
- Check for required fields before storage
- Handle missing or null values gracefully

### Incremental Collection
- Track what's already collected
- Fetch only new or updated data
- Use timestamps for change detection

### Performance
- Batch requests when possible
- Use async operations for concurrent collection
- Implement rate limiting to avoid API throttling

### Data Freshness
- Different data types have different update frequencies:
  - Live games: Every 10-30 seconds
  - Standings: Daily
  - Player profiles: Weekly
  - Historical data: Once

## Testing

### Unit Tests
```python
from unittest.mock import AsyncMock, patch

@patch('src.collectors.games.NHLApiClient')
async def test_game_collector(mock_client):
    mock_client.return_value.get = AsyncMock(return_value={...})
    
    collector = GameCollector()
    games = await collector.fetch_games_by_date("2024-01-15")
    
    assert len(games) > 0
```

### Integration Tests
```python
@pytest.mark.integration
async def test_full_collection_workflow():
    collector = GameCollector()
    games = await collector.fetch_games_by_date("2024-01-15")
    
    for game in games:
        processed = await collector.process(game)
        await collector.store(processed)
    
    # Verify data in database
    from src.database.queries import get_games_by_date
    stored_games = await get_games_by_date("2024-01-15")
    assert len(stored_games) == len(games)
```

## Resources
- [src/api/](../api/) - API client used by collectors
- [src/database/](../database/) - Database storage layer
- [scripts/fetch_historical.py](../../scripts/) - Bulk collection script

## Related Documentation
- [DESIGN.md](../../DESIGN.md) - See Section 3.1 for Data Collection design
- API endpoint documentation in `src/api/README.md`
