# NHL Data Analysis & Visualization System

A comprehensive data analysis and visualization platform for NHL statistics using the official NHL API.

## Project Status
🚧 **Planning & Initial Setup Phase** - See [DESIGN.md](DESIGN.md) for detailed design documentation.

## Overview
This system provides tools for:
- **Data Collection**: Fetch NHL data from official API
- **Data Storage**: Persist data in structured database
- **Analytics**: Statistical analysis and advanced metrics
- **Visualizations**: Interactive charts and dashboards
- **Web Interface**: User-friendly access to analytics

## Quick Start

### Prerequisites
- Python 3.11 or higher
- PostgreSQL (for production) or SQLite (for development)
- Git

### Installation
```bash
# Clone the repository
git clone <repository-url>
cd NHL

# Create virtual environment
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Set up environment variables
cp .env.example .env
# Edit .env with your configuration

# Initialize database
python scripts/setup_db.py

# Run the application
python src/web/app.py
```

## Project Structure
```
NHL/
├── src/              # Main source code
│   ├── api/          # NHL API client
│   ├── collectors/   # Data collection modules
│   ├── database/     # Database models and queries
│   ├── analytics/    # Analysis engines
│   ├── visualizations/ # Chart generation
│   ├── web/          # Web interface
│   └── utils/        # Utility functions
├── tests/            # Test suite
├── notebooks/        # Jupyter notebooks for exploration
├── workbench/        # Prototyping and experimentation area
├── data/             # Local data storage
├── scripts/          # Utility scripts
├── config/           # Configuration files
└── docs/             # Documentation
```

See individual README.md files in each directory for detailed information.

## Features (Planned)

### Data Collection
- Real-time game data and live scores
- Historical statistics (multiple seasons)
- Player profiles and career stats
- Team information and rosters
- League standings and schedules

### Analytics
- Player performance analysis
- Team statistics and trends
- Advanced metrics (Corsi, Fenwick, PDO)
- Predictive modeling
- Comparative analysis tools

### Visualizations
- Interactive dashboards
- Performance trend charts
- Shot charts and heat maps
- Statistical comparisons
- Custom reports

## Development

### Setting Up Development Environment
```bash
# Install development dependencies
pip install -r requirements-dev.txt

# Install pre-commit hooks
pre-commit install

# Run tests
pytest

# Run with coverage
pytest --cov=src
```

### Running Tests
```bash
# All tests
pytest

# Specific test file
pytest tests/unit/test_api_client.py

# With verbose output
pytest -v
```

### Code Quality
```bash
# Format code
black src/ tests/

# Lint code
ruff check src/ tests/

# Type checking
mypy src/
```

## Workbench Area
The `workbench/` directory is a dedicated space for prototyping and experimentation:
- **experiments/** - Quick tests and proof-of-concepts
- **prototypes/** - Feature prototypes before integration
- **sandbox/** - Free-form experimentation

See [workbench/README.md](workbench/README.md) for details.

## Documentation
- [DESIGN.md](DESIGN.md) - Comprehensive design document
- [docs/api_reference.md](docs/api_reference.md) - API documentation (TBD)
- [docs/user_guide.md](docs/user_guide.md) - User guide (TBD)
- [docs/developer_guide.md](docs/developer_guide.md) - Developer guide (TBD)

## NHL API Resources
- New API Base: https://api-web.nhle.com/v1/
- Community Documentation: https://gitlab.com/dword4/nhlapi
- API Specifications: https://github.com/erunion/sport-api-specifications/tree/master/nhl

## Technology Stack
- **Language**: Python 3.11+
- **API Client**: httpx, pydantic
- **Database**: SQLAlchemy, PostgreSQL/SQLite
- **Analytics**: pandas, numpy, scikit-learn
- **Visualization**: Plotly, Matplotlib, Seaborn
- **Web Framework**: FastAPI (planned)
- **Testing**: pytest

## Contributing
(Guidelines to be added)

## License
(To be determined)

## Acknowledgments
- NHL for providing the API
- Community API documentation maintainers

---

**Last Updated**: 2025-10-22  
**Version**: 0.1.0  
**Status**: Initial Setup
