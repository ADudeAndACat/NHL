# NHL Data Analysis & Visualization System - Design Document

## Project Overview
A comprehensive data analysis and visualization platform for NHL statistics using the official NHL API. The system will provide tools for data collection, processing, analysis, and interactive visualizations.

---

## 1. Architecture Overview

### High-Level Architecture
```
┌─────────────────┐
│   NHL API       │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Data Collection │ ← Scheduled fetchers, API clients
│     Layer       │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Data Storage    │ ← SQLite/PostgreSQL + Cache (Redis)
│     Layer       │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Processing &    │ ← Statistical analysis, aggregations
│ Analysis Layer  │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Visualization   │ ← Web dashboard, charts, reports
│     Layer       │
└─────────────────┘
```

---

## 2. Proposed Project Structure

```
NHL/
├── src/
│   ├── api/                    # NHL API interaction
│   │   ├── __init__.py
│   │   ├── client.py          # Base API client
│   │   ├── endpoints.py       # API endpoint definitions
│   │   ├── models.py          # Pydantic models for API responses
│   │   └── cache.py           # Response caching logic
│   │
│   ├── collectors/            # Data collection modules
│   │   ├── __init__.py
│   │   ├── games.py           # Game data collector
│   │   ├── players.py         # Player stats collector
│   │   ├── teams.py           # Team stats collector
│   │   ├── standings.py       # Standings collector
│   │   └── schedule.py        # Schedule collector
│   │
│   ├── database/              # Database layer
│   │   ├── __init__.py
│   │   ├── models.py          # SQLAlchemy/Django ORM models
│   │   ├── connection.py      # DB connection management
│   │   ├── migrations/        # Database migrations
│   │   └── queries.py         # Common query patterns
│   │
│   ├── analytics/             # Analysis engines
│   │   ├── __init__.py
│   │   ├── player_stats.py    # Player performance analytics
│   │   ├── team_stats.py      # Team performance analytics
│   │   ├── predictions.py     # Predictive models (ML)
│   │   ├── trends.py          # Trend analysis
│   │   └── comparisons.py     # Comparative analysis
│   │
│   ├── visualizations/        # Visualization components
│   │   ├── __init__.py
│   │   ├── charts.py          # Chart generation (Plotly/Matplotlib)
│   │   ├── dashboards.py      # Dashboard layouts
│   │   ├── heatmaps.py        # Heat map visualizations
│   │   └── reports.py         # Report generation
│   │
│   ├── web/                   # Web interface (optional)
│   │   ├── __init__.py
│   │   ├── app.py             # Flask/FastAPI/Django app
│   │   ├── routes.py          # API routes
│   │   ├── templates/         # HTML templates
│   │   └── static/            # CSS, JS, images
│   │
│   └── utils/                 # Utility functions
│       ├── __init__.py
│       ├── logging.py         # Logging configuration
│       ├── config.py          # Configuration management
│       └── helpers.py         # Helper functions
│
├── tests/                     # Test suite
│   ├── unit/
│   ├── integration/
│   └── fixtures/
│
├── notebooks/                 # Jupyter notebooks for exploration
│   ├── exploratory/
│   └── analysis/
│
├── data/                      # Local data storage
│   ├── raw/                   # Raw API responses
│   ├── processed/             # Processed datasets
│   └── exports/               # Exported reports/visualizations
│
├── scripts/                   # Utility scripts
│   ├── setup_db.py
│   ├── fetch_historical.py
│   └── generate_reports.py
│
├── docs/                      # Documentation
│   ├── api_reference.md
│   └── user_guide.md
│
├── config/                    # Configuration files
│   ├── development.yaml
│   ├── production.yaml
│   └── logging.yaml
│
├── requirements.txt           # Python dependencies
├── pyproject.toml            # Project metadata (Poetry/setuptools)
├── .env.example              # Environment variables template
├── .gitignore
├── README.md
└── DESIGN.md                 # This file
```

---

## 3. Core Features & Modules

### 3.1 Data Collection
**Purpose**: Fetch and store NHL data from the API

**Key Features**:
- **Real-time game data**: Live scores, play-by-play
- **Historical data**: Past seasons, games, statistics
- **Player profiles**: Career stats, biographical info
- **Team information**: Rosters, schedules, standings
- **Schedule tracking**: Upcoming games, season calendar

**Technologies**:
- `requests` or `httpx` for API calls
- `pydantic` for data validation
- Rate limiting and retry logic
- Caching layer (Redis or simple file cache)

### 3.2 Data Storage
**Purpose**: Persist and organize NHL data

**Database Schema (Proposed)**:
```
Teams
- team_id (PK)
- name, abbreviation, division, conference
- venue, established_year

Players
- player_id (PK)
- name, position, birth_date, nationality
- current_team_id (FK)
- jersey_number, shoots/catches

Games
- game_id (PK)
- season, game_type, date_time
- home_team_id (FK), away_team_id (FK)
- home_score, away_score
- status, venue

PlayerGameStats
- id (PK)
- game_id (FK), player_id (FK)
- goals, assists, points, plus_minus
- shots, hits, blocks, time_on_ice

TeamGameStats
- id (PK)
- game_id (FK), team_id (FK)
- shots, hits, faceoff_percentage
- power_play_goals, penalty_minutes

Standings
- id (PK)
- team_id (FK), season, date
- wins, losses, ot_losses, points
- goals_for, goals_against
```

**Technologies**:
- SQLAlchemy (ORM) or Django ORM
- PostgreSQL (production) or SQLite (development)
- Alembic for migrations

### 3.3 Analytics Engine
**Purpose**: Process and analyze NHL data

**Analysis Types**:
1. **Player Analytics**:
   - Career progression tracking
   - Performance trends (hot/cold streaks)
   - Advanced metrics (Corsi, Fenwick, PDO)
   - Position-specific analysis
   - Comparison tools (player vs player)

2. **Team Analytics**:
   - Win/loss patterns
   - Home vs away performance
   - Division/conference standings
   - Offensive/defensive efficiency
   - Special teams analysis

3. **Predictive Models** (Optional/Advanced):
   - Game outcome predictions
   - Player performance forecasting
   - Playoff probability calculations
   - Draft pick analysis

4. **Historical Analysis**:
   - Season-over-season comparisons
   - Franchise records and milestones
   - Era comparisons

**Technologies**:
- `pandas` for data manipulation
- `numpy` for numerical computations
- `scikit-learn` for ML models (optional)
- `statsmodels` for statistical analysis

### 3.4 Visualization Layer
**Purpose**: Create interactive and static visualizations

**Visualization Types**:
1. **Charts & Graphs**:
   - Line charts (trends over time)
   - Bar charts (comparisons)
   - Scatter plots (correlations)
   - Box plots (distribution analysis)

2. **Interactive Dashboards**:
   - Team performance dashboard
   - Player comparison tool
   - League-wide statistics
   - Live game tracker

3. **Specialized Visualizations**:
   - Shot charts (heat maps)
   - Ice rink visualizations
   - Network graphs (passing patterns)
   - Geographic maps (player origins)

**Technologies**:
- **Plotly**: Interactive web-based charts
- **Matplotlib/Seaborn**: Static publication-quality charts
- **Dash** or **Streamlit**: Dashboard framework
- **D3.js**: Advanced custom visualizations (if web-based)

### 3.5 Web Interface (Optional)
**Purpose**: User-friendly access to analytics

**Features**:
- Browse teams, players, games
- Custom query builder
- Export data (CSV, JSON, Excel)
- Scheduled reports
- API endpoints for external access

**Technologies**:
- **FastAPI**: Modern, fast API framework
- **Flask**: Lightweight alternative
- **Django**: Full-featured (if you want admin panel)
- **React/Vue.js**: Frontend (if SPA)
- **Bootstrap/Tailwind**: Styling

---

## 4. Technology Stack Recommendations

### Core Stack
```yaml
Language: Python 3.11+

API & Data Collection:
  - httpx: Async HTTP client
  - pydantic: Data validation
  - tenacity: Retry logic

Database:
  - SQLAlchemy: ORM
  - PostgreSQL: Production database
  - Redis: Caching layer

Analytics:
  - pandas: Data manipulation
  - numpy: Numerical computing
  - scipy: Scientific computing
  - scikit-learn: Machine learning (optional)

Visualization:
  - plotly: Interactive charts
  - matplotlib: Static charts
  - seaborn: Statistical visualizations
  - streamlit: Quick dashboards

Web Framework (choose one):
  - FastAPI: Modern API
  - Flask: Lightweight
  - Django: Full-featured

Testing:
  - pytest: Testing framework
  - pytest-cov: Coverage
  - faker: Test data generation

Development Tools:
  - black: Code formatting
  - ruff: Linting
  - mypy: Type checking
  - pre-commit: Git hooks
```

---

## 5. Development Phases

### Phase 1: Foundation (Week 1-2)
- [ ] Set up project structure
- [ ] Configure development environment
- [ ] Implement NHL API client
- [ ] Create basic data models
- [ ] Set up database schema
- [ ] Write initial tests

### Phase 2: Data Collection (Week 3-4)
- [ ] Implement data collectors for:
  - [ ] Teams
  - [ ] Players
  - [ ] Games
  - [ ] Standings
- [ ] Add caching layer
- [ ] Create data validation
- [ ] Build historical data fetcher

### Phase 3: Analytics (Week 5-6)
- [ ] Implement basic statistics calculations
- [ ] Create player analytics module
- [ ] Create team analytics module
- [ ] Add trend analysis
- [ ] Build comparison tools

### Phase 4: Visualization (Week 7-8)
- [ ] Create chart generation functions
- [ ] Build interactive dashboards
- [ ] Implement specialized visualizations
- [ ] Add export functionality

### Phase 5: Web Interface (Week 9-10)
- [ ] Set up web framework
- [ ] Create API endpoints
- [ ] Build frontend interface
- [ ] Add user authentication (optional)
- [ ] Deploy to production

### Phase 6: Advanced Features (Week 11+)
- [ ] Machine learning models
- [ ] Real-time data streaming
- [ ] Advanced visualizations
- [ ] Mobile app (optional)

---

## 6. NHL API Endpoints Reference

### Key Endpoints to Use
```
Base URL: https://api-web.nhle.com/v1/

Teams:
  - /teams
  - /team/{team_abbr}/roster

Players:
  - /player/{player_id}/landing
  - /player/{player_id}/game-log/{season}/{game_type}

Games:
  - /schedule/{date}
  - /gamecenter/{game_id}/play-by-play
  - /gamecenter/{game_id}/boxscore

Standings:
  - /standings/{date}
  - /standings-season

Stats:
  - /skater-stats-leaders/{season}/{game_type}
  - /goalie-stats-leaders/{season}/{game_type}
```

**Note**: The NHL API has changed over time. The new API (api-web.nhle.com) is different from the old statsapi.web.nhl.com. Research current endpoints.

---

## 7. Data Analysis Ideas

### Beginner Level
1. **Team Win/Loss Records**: Track and visualize team performance
2. **Top Scorers**: Identify leading goal scorers and point leaders
3. **Standings Tracker**: Monitor division and conference standings
4. **Player Comparison**: Compare two players' statistics side-by-side

### Intermediate Level
5. **Performance Trends**: Identify hot/cold streaks for players/teams
6. **Home vs Away Analysis**: Compare team performance by venue
7. **Shot Charts**: Visualize where goals are scored on the ice
8. **Power Play Efficiency**: Analyze special teams performance
9. **Goalie Statistics**: Save percentage, GAA, shutouts
10. **Head-to-Head Records**: Team matchup history

### Advanced Level
11. **Predictive Modeling**: Predict game outcomes using ML
12. **Advanced Metrics**: Calculate Corsi, Fenwick, PDO, xG
13. **Line Combination Analysis**: Evaluate player chemistry
14. **Draft Analysis**: Analyze draft pick success rates
15. **Injury Impact**: Correlate injuries with team performance
16. **Salary Cap Analysis**: Optimize roster construction
17. **Playoff Probability**: Calculate playoff chances
18. **Trade Impact Analysis**: Evaluate trade effectiveness

---

## 8. Best Practices

### Code Quality
- Use type hints throughout
- Write comprehensive docstrings
- Maintain >80% test coverage
- Follow PEP 8 style guide
- Use meaningful variable names

### Data Management
- Implement proper error handling
- Validate all API responses
- Cache frequently accessed data
- Regular database backups
- Handle rate limits gracefully

### Performance
- Use async operations where possible
- Implement pagination for large datasets
- Optimize database queries
- Lazy load visualizations
- Profile and optimize bottlenecks

### Security
- Never commit API keys
- Use environment variables
- Sanitize user inputs
- Implement rate limiting
- Regular dependency updates

---

## 9. Future Enhancements

### Potential Features
- **Mobile App**: iOS/Android companion app
- **Notifications**: Alerts for game starts, scores, milestones
- **Social Features**: Share visualizations, compete with friends
- **Fantasy Hockey Integration**: Draft analysis, lineup optimization
- **Video Analysis**: Integrate game highlights
- **Multi-Sport**: Expand to other leagues (NBA, MLB, NFL)
- **AI Assistant**: Natural language queries ("Who scored the most goals in 2023?")
- **Custom Alerts**: Notify when specific conditions are met

### Monetization (Optional)
- Premium features subscription
- API access for developers
- Custom reports for teams/media
- Advertising (free tier)

---

## 10. Getting Started Checklist

- [ ] Review this design document
- [ ] Research current NHL API documentation
- [ ] Set up Python virtual environment
- [ ] Install core dependencies
- [ ] Create initial project structure
- [ ] Set up version control (Git)
- [ ] Write a simple API client test
- [ ] Fetch and display basic team data
- [ ] Design initial database schema
- [ ] Create first visualization

---

## Resources

### NHL API Documentation
- Official NHL API (research current endpoints)
- Community documentation: https://gitlab.com/dword4/nhlapi
- Alternative: https://github.com/erunion/sport-api-specifications/tree/master/nhl

### Python Libraries
- Pandas: https://pandas.pydata.org/
- Plotly: https://plotly.com/python/
- FastAPI: https://fastapi.tiangolo.com/
- SQLAlchemy: https://www.sqlalchemy.org/
- Streamlit: https://streamlit.io/

### Inspiration
- Natural Stat Trick: https://www.naturalstattrick.com/
- Hockey Reference: https://www.hockey-reference.com/
- Evolving Hockey: https://evolving-hockey.com/

---

## Notes

- Start small and iterate
- Focus on one feature at a time
- Test with current season data first
- Consider data storage costs for historical data
- The NHL API is free but may have rate limits
- Some advanced stats may require additional calculation

---

**Last Updated**: 2025-10-22  
**Version**: 1.0  
**Status**: Planning Phase
