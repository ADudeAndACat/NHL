# NHL API Client (src/api/)

## Overview
This module provides the interface for communicating with the NHL API. It handles authentication, request formatting, response parsing, and caching.

## Components

### client.py
Base HTTP client for NHL API communication.

**Key Features:**
- Async/sync HTTP requests
- Automatic retry logic with exponential backoff
- Rate limiting to respect API constraints
- Error handling and logging
- Response validation

**Example:**
```python
from src.api.client import NHLApiClient

client = NHLApiClient()
response = await client.get("/teams")
```

### endpoints.py
NHL API endpoint definitions and convenience wrappers.

**Endpoints:**
- Teams: `/teams`, `/team/{abbr}/roster`
- Players: `/player/{id}/landing`, `/player/{id}/game-log/{season}/{type}`
- Games: `/schedule/{date}`, `/gamecenter/{id}/play-by-play`
- Standings: `/standings/{date}`, `/standings-season`
- Stats: `/skater-stats-leaders/{season}/{type}`

**Example:**
```python
from src.api.endpoints import get_team_roster

roster = await get_team_roster("TOR")
```

### models.py
Pydantic models for API response validation.

**Models:**
- `Team`, `TeamRoster`
- `Player`, `PlayerStats`
- `Game`, `GameBoxScore`
- `Standings`

**Example:**
```python
from src.api.models import Team

team_data = await client.get("/teams/TOR")
team = Team(**team_data)
```

### cache.py
Response caching to reduce API calls.

**Features:**
- Time-based cache expiration
- In-memory and Redis support
- Cache invalidation strategies

**Example:**
```python
from src.api.cache import cached_request

@cached_request(ttl=300)  # Cache for 5 minutes
async def get_standings():
    return await client.get("/standings")
```

## NHL API Information

### Base URL
```
https://api-web.nhle.com/v1/
```

### Key Endpoints
```
Teams:
  GET /teams
  GET /team/{team_abbr}/roster

Players:
  GET /player/{player_id}/landing
  GET /player/{player_id}/game-log/{season}/{game_type}

Games:
  GET /schedule/{date}
  GET /gamecenter/{game_id}/play-by-play
  GET /gamecenter/{game_id}/boxscore

Standings:
  GET /standings/{date}
  GET /standings-season

Stats:
  GET /skater-stats-leaders/{season}/{game_type}
  GET /goalie-stats-leaders/{season}/{game_type}
```

## Usage Examples

### Fetch Team Data
```python
from src.api.client import NHLApiClient

async def main():
    client = NHLApiClient()
    
    # Get all teams
    teams = await client.get_teams()
    
    # Get specific team roster
    roster = await client.get_team_roster("TOR")
    
    for player in roster:
        print(f"{player.name} - {player.position}")
```

### Fetch Game Schedule
```python
from datetime import date
from src.api.endpoints import get_schedule

# Get today's games
today = date.today()
games = await get_schedule(today)

for game in games:
    print(f"{game.away_team} @ {game.home_team}")
```

### Player Statistics
```python
from src.api.endpoints import get_player_stats

player_id = 8478402  # Auston Matthews
season = "20232024"
stats = await get_player_stats(player_id, season)

print(f"Goals: {stats.goals}")
print(f"Assists: {stats.assists}")
```

## Best Practices

### Error Handling
Always wrap API calls in try-except blocks:
```python
from src.api.client import NHLApiClient, APIError

try:
    data = await client.get("/teams")
except APIError as e:
    logger.error(f"API request failed: {e}")
    # Handle error appropriately
```

### Rate Limiting
Respect API rate limits to avoid being blocked:
```python
import asyncio

# Batch requests with delays
for team_id in team_ids:
    data = await client.get(f"/team/{team_id}")
    await asyncio.sleep(0.1)  # Small delay between requests
```

### Caching
Use caching for frequently accessed, slowly changing data:
```python
# Cache standings for 5 minutes
standings = await cached_request("/standings", ttl=300)

# Don't cache live game data
live_score = await client.get(f"/gamecenter/{game_id}/boxscore")
```

## Testing

### Unit Tests
Test API client functionality with mocked responses:
```python
from unittest.mock import AsyncMock, patch

@patch('src.api.client.httpx.AsyncClient.get')
async def test_get_teams(mock_get):
    mock_get.return_value = AsyncMock(status_code=200, json=...)
    client = NHLApiClient()
    teams = await client.get_teams()
    assert len(teams) > 0
```

### Integration Tests
Test against real API (sparingly):
```python
@pytest.mark.integration
async def test_real_api():
    client = NHLApiClient()
    teams = await client.get_teams()
    assert "TOR" in [t.abbreviation for t in teams]
```

## Resources
- [NHL API Community Docs](https://gitlab.com/dword4/nhlapi)
- [Sport API Specifications](https://github.com/erunion/sport-api-specifications/tree/master/nhl)
- [httpx Documentation](https://www.python-httpx.org/)
- [Pydantic Documentation](https://docs.pydantic.dev/)

## Related Modules
- `src/collectors/` - Uses this module to collect data
- `src/database/` - Stores data retrieved by this module
- `tests/unit/test_api_client.py` - API client tests
