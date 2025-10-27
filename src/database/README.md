# Database Layer (src/database/)

## Overview
This module handles all database operations including ORM models, queries, connections, and migrations. It uses SQLAlchemy as the ORM and supports both PostgreSQL (production) and SQLite (development).

## Components

### models.py
SQLAlchemy ORM models defining the database schema.

**Key Models:**
- `Team` - NHL teams
- `Player` - Player information
- `Game` - Game records
- `PlayerGameStats` - Player statistics per game
- `TeamGameStats` - Team statistics per game
- `Standings` - League standings

**Example:**
```python
from src.database.models import Player, Team

# Query example
player = session.query(Player).filter_by(player_id=8478402).first()
print(f"{player.name} plays for {player.team.name}")
```

### connection.py
Database connection management and session handling.

**Features:**
- Connection pooling
- Session lifecycle management
- Connection string configuration
- Transaction management

**Example:**
```python
from src.database.connection import get_session

async with get_session() as session:
    # Perform database operations
    results = await session.execute(query)
```

### queries.py
Common query patterns and database operations.

**Functions:**
- `get_player_by_id(player_id: int)`
- `get_games_by_date(date: str)`
- `get_team_roster(team_abbr: str)`
- `get_season_stats(player_id: int, season: str)`

**Example:**
```python
from src.database.queries import get_player_stats

stats = await get_player_stats(player_id=8478402, season="20232024")
```

### migrations/
Database migration scripts using Alembic.

**Structure:**
```
migrations/
├── versions/          # Migration version files
├── env.py            # Migration environment
├── script.py.mako    # Migration template
└── alembic.ini       # Alembic configuration
```

## Database Schema

### Core Tables

#### Teams
```sql
CREATE TABLE teams (
    team_id INTEGER PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    abbreviation VARCHAR(3) NOT NULL UNIQUE,
    division VARCHAR(50),
    conference VARCHAR(50),
    venue VARCHAR(100),
    established_year INTEGER,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### Players
```sql
CREATE TABLE players (
    player_id INTEGER PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    position VARCHAR(10),
    birth_date DATE,
    nationality VARCHAR(50),
    current_team_id INTEGER REFERENCES teams(team_id),
    jersey_number INTEGER,
    shoots VARCHAR(1),
    height_inches INTEGER,
    weight_pounds INTEGER,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### Games
```sql
CREATE TABLE games (
    game_id INTEGER PRIMARY KEY,
    season VARCHAR(8) NOT NULL,
    game_type VARCHAR(2),
    game_date DATE NOT NULL,
    start_time TIMESTAMP,
    home_team_id INTEGER REFERENCES teams(team_id),
    away_team_id INTEGER REFERENCES teams(team_id),
    home_score INTEGER,
    away_score INTEGER,
    status VARCHAR(20),
    venue VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### PlayerGameStats
```sql
CREATE TABLE player_game_stats (
    id SERIAL PRIMARY KEY,
    game_id INTEGER REFERENCES games(game_id),
    player_id INTEGER REFERENCES players(player_id),
    team_id INTEGER REFERENCES teams(team_id),
    goals INTEGER DEFAULT 0,
    assists INTEGER DEFAULT 0,
    points INTEGER DEFAULT 0,
    plus_minus INTEGER DEFAULT 0,
    shots INTEGER DEFAULT 0,
    hits INTEGER DEFAULT 0,
    blocks INTEGER DEFAULT 0,
    time_on_ice_seconds INTEGER,
    power_play_goals INTEGER DEFAULT 0,
    short_handed_goals INTEGER DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(game_id, player_id)
);
```

#### Standings
```sql
CREATE TABLE standings (
    id SERIAL PRIMARY KEY,
    team_id INTEGER REFERENCES teams(team_id),
    season VARCHAR(8) NOT NULL,
    standing_date DATE NOT NULL,
    wins INTEGER DEFAULT 0,
    losses INTEGER DEFAULT 0,
    ot_losses INTEGER DEFAULT 0,
    points INTEGER DEFAULT 0,
    goals_for INTEGER DEFAULT 0,
    goals_against INTEGER DEFAULT 0,
    division_rank INTEGER,
    conference_rank INTEGER,
    league_rank INTEGER,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(team_id, season, standing_date)
);
```

## Usage Examples

### Creating Models
```python
from src.database.models import Team, Player, Base
from src.database.connection import engine

# Create all tables
Base.metadata.create_all(engine)

# Add a team
team = Team(
    team_id=10,
    name="Toronto Maple Leafs",
    abbreviation="TOR",
    division="Atlantic",
    conference="Eastern"
)
session.add(team)
session.commit()
```

### Querying Data
```python
from src.database.connection import get_session
from src.database.models import Player, Team
from sqlalchemy import select

async with get_session() as session:
    # Simple query
    stmt = select(Player).where(Player.player_id == 8478402)
    result = await session.execute(stmt)
    player = result.scalar_one_or_none()
    
    # Join query
    stmt = select(Player, Team).join(Team).where(Team.abbreviation == "TOR")
    result = await session.execute(stmt)
    roster = result.all()
```

### Using Helper Queries
```python
from src.database.queries import (
    get_player_by_id,
    get_games_by_date,
    get_team_stats
)

# Get player
player = await get_player_by_id(8478402)

# Get games
games = await get_games_by_date("2024-01-15")

# Get team stats
stats = await get_team_stats("TOR", "20232024")
```

### Transaction Management
```python
from src.database.connection import get_session

async with get_session() as session:
    try:
        # Multiple operations
        player = Player(...)
        session.add(player)
        
        stats = PlayerGameStats(...)
        session.add(stats)
        
        # Commit all or nothing
        await session.commit()
    except Exception as e:
        await session.rollback()
        raise
```

## Migrations

### Creating a Migration
```bash
# Create a new migration
alembic revision --autogenerate -m "Add player stats table"

# Review the generated migration in migrations/versions/
# Edit if necessary

# Apply migration
alembic upgrade head
```

### Migration Commands
```bash
# Show current version
alembic current

# Show migration history
alembic history

# Upgrade to latest
alembic upgrade head

# Downgrade one version
alembic downgrade -1

# Downgrade to specific version
alembic downgrade <revision_id>
```

## Configuration

### Database Connection String
```python
# Development (SQLite)
DATABASE_URL = "sqlite:///./nhl_data.db"

# Production (PostgreSQL)
DATABASE_URL = "postgresql://user:password@localhost:5432/nhl_db"

# Configure in .env file
DATABASE_URL=postgresql://user:password@localhost:5432/nhl_db
```

### Connection Pool Settings
```python
from sqlalchemy import create_engine

engine = create_engine(
    DATABASE_URL,
    pool_size=10,
    max_overflow=20,
    pool_timeout=30,
    pool_recycle=3600
)
```

## Best Practices

### Model Design
- Use appropriate data types
- Add indexes for frequently queried fields
- Define relationships clearly
- Include timestamps (created_at, updated_at)

### Query Optimization
- Use indexes on foreign keys
- Avoid N+1 queries with joins/eager loading
- Use select only needed columns
- Implement pagination for large results

### Data Integrity
- Use foreign key constraints
- Add unique constraints where appropriate
- Validate data before insertion
- Handle duplicates gracefully

### Session Management
- Use context managers for sessions
- Commit after successful operations
- Rollback on errors
- Close sessions properly

## Testing

### Unit Tests
```python
import pytest
from sqlalchemy import create_engine
from src.database.models import Base, Player

@pytest.fixture
def test_db():
    engine = create_engine("sqlite:///:memory:")
    Base.metadata.create_all(engine)
    yield engine
    Base.metadata.drop_all(engine)

def test_create_player(test_db):
    player = Player(
        player_id=12345,
        name="Test Player",
        position="C"
    )
    # Test operations
```

### Integration Tests
```python
@pytest.mark.integration
async def test_full_query_workflow():
    from src.database.queries import get_player_by_id
    
    # Insert test data
    player = Player(...)
    await insert_player(player)
    
    # Query and verify
    result = await get_player_by_id(player.player_id)
    assert result.name == player.name
```

## Performance Tips

### Indexing
```python
from sqlalchemy import Index

# Add indexes in models
class PlayerGameStats(Base):
    __tablename__ = "player_game_stats"
    
    # Define index
    __table_args__ = (
        Index('idx_player_game', 'player_id', 'game_id'),
        Index('idx_game_date', 'game_id'),
    )
```

### Bulk Operations
```python
# Bulk insert
players = [Player(...), Player(...), ...]
session.bulk_save_objects(players)
session.commit()

# Bulk update
session.bulk_update_mappings(
    Player,
    [{"player_id": 1, "jersey_number": 34}, ...]
)
```

## Resources
- [SQLAlchemy Documentation](https://docs.sqlalchemy.org/)
- [Alembic Documentation](https://alembic.sqlalchemy.org/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)

## Related Modules
- `src/collectors/` - Stores collected data using this module
- `src/analytics/` - Queries data through this module
- `scripts/setup_db.py` - Database initialization script
