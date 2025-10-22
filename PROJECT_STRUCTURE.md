# NHL Analytics Project Structure

## Overview
This document provides a complete overview of the project structure created for the NHL Data Analysis & Visualization System.

## Directory Tree
```
NHL/
├── .venv/                      # Python virtual environment (not tracked in git)
├── config/                     # Configuration files
│   └── README.md              # Configuration documentation
├── data/                       # Local data storage
│   ├── raw/                   # Raw API responses
│   │   └── .gitkeep
│   ├── processed/             # Processed datasets
│   │   └── .gitkeep
│   ├── exports/               # Exported reports/visualizations
│   │   └── .gitkeep
│   └── README.md              # Data directory documentation
├── docs/                       # Project documentation
│   └── README.md              # Documentation index
├── logs/                       # Application logs
│   └── .gitkeep
├── notebooks/                  # Jupyter notebooks
│   ├── exploratory/           # Data exploration notebooks
│   ├── analysis/              # Analysis notebooks
│   └── README.md              # Notebooks documentation
├── scripts/                    # Utility scripts
│   └── README.md              # Scripts documentation
├── src/                        # Main source code
│   ├── __init__.py
│   ├── api/                   # NHL API client
│   │   ├── __init__.py
│   │   └── README.md
│   ├── collectors/            # Data collection modules
│   │   ├── __init__.py
│   │   └── README.md
│   ├── database/              # Database layer
│   │   ├── __init__.py
│   │   ├── migrations/        # Database migrations
│   │   └── README.md
│   ├── analytics/             # Analysis engines
│   │   ├── __init__.py
│   │   └── README.md
│   ├── visualizations/        # Visualization components
│   │   ├── __init__.py
│   │   └── README.md
│   ├── web/                   # Web interface
│   │   ├── __init__.py
│   │   ├── templates/         # HTML templates
│   │   ├── static/            # CSS, JS, images
│   │   └── README.md
│   ├── utils/                 # Utility functions
│   │   ├── __init__.py
│   │   └── README.md
│   └── README.md              # Source code overview
├── tests/                      # Test suite
│   ├── __init__.py
│   ├── unit/                  # Unit tests
│   ├── integration/           # Integration tests
│   ├── fixtures/              # Test fixtures
│   └── README.md              # Testing documentation
├── workbench/                  # 🔧 Prototyping & experimentation area
│   ├── experiments/           # Quick experiments
│   ├── prototypes/            # Feature prototypes
│   ├── sandbox/               # Free-form experimentation
│   └── README.md              # Workbench documentation
├── .env.example                # Environment variables template
├── .gitignore                  # Git ignore rules
├── DESIGN.md                   # Comprehensive design document
├── PROJECT_STRUCTURE.md        # This file
├── README.md                   # Project overview and quick start
└── requirements.txt            # Python dependencies
```

## Key Components

### 📁 Source Code (`src/`)
The main application code organized into functional modules:
- **api/** - NHL API interaction layer
- **collectors/** - Data fetching and collection
- **database/** - ORM models and database operations
- **analytics/** - Statistical analysis and metrics
- **visualizations/** - Chart and dashboard generation
- **web/** - Web interface and API endpoints
- **utils/** - Shared utilities and helpers

### 🧪 Tests (`tests/`)
Comprehensive test suite:
- **unit/** - Unit tests for individual components
- **integration/** - Integration tests for module interactions
- **fixtures/** - Test data and mock responses

### 📓 Notebooks (`notebooks/`)
Jupyter notebooks for interactive analysis:
- **exploratory/** - Initial data exploration
- **analysis/** - In-depth analysis and reports

### 🔧 Workbench (`workbench/`)
**Your prototyping playground!** This is a self-contained area for:
- **experiments/** - Quick tests and proof-of-concepts
- **prototypes/** - Feature prototypes before integration
- **sandbox/** - Free-form experimentation and learning

The workbench is designed for rapid iteration without affecting the main codebase. No rules, no tests required, just experiment and learn!

### 📊 Data (`data/`)
Local data storage (not tracked in git):
- **raw/** - Original API responses
- **processed/** - Cleaned and transformed data
- **exports/** - Generated reports and visualizations

### 🛠️ Scripts (`scripts/`)
Utility scripts for operations:
- Database setup and migrations
- Data collection and updates
- Report generation
- Maintenance tasks

### ⚙️ Configuration (`config/`)
Environment-specific configuration files:
- Development settings
- Production settings
- Logging configuration

### 📚 Documentation (`docs/`)
Project documentation:
- API reference
- User guides
- Developer guides
- Data dictionary

## File Purposes

### Root Level Files
- **DESIGN.md** - Comprehensive design document with architecture, features, and development phases
- **README.md** - Project overview, quick start, and basic documentation
- **PROJECT_STRUCTURE.md** - This file, explaining the project structure
- **requirements.txt** - Python package dependencies
- **.env.example** - Template for environment variables
- **.gitignore** - Files and directories to exclude from git

## README Files
Each major directory contains a README.md explaining:
- Purpose of the directory
- What files should go there
- Usage examples
- Best practices
- Related documentation

## Getting Started

### 1. Read the Documentation
- Start with [README.md](README.md) for project overview
- Review [DESIGN.md](DESIGN.md) for detailed design
- Check individual README files for specific modules

### 2. Set Up Environment
```bash
# Create virtual environment
python -m venv .venv
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Configure environment
cp .env.example .env
# Edit .env with your settings
```

### 3. Start Prototyping
The `workbench/` directory is ready for you to start experimenting:
```bash
# Create your first experiment
touch workbench/experiments/test_nhl_api.py

# Start exploring!
python workbench/experiments/test_nhl_api.py
```

### 4. Explore with Notebooks
```bash
# Start Jupyter
jupyter lab

# Create a new notebook in notebooks/exploratory/
```

## Development Workflow

### Typical Development Flow
1. **Prototype** in `workbench/` - Quick experiments and proof-of-concepts
2. **Explore** in `notebooks/` - Interactive data analysis
3. **Implement** in `src/` - Production-ready code
4. **Test** in `tests/` - Comprehensive test coverage
5. **Document** in `docs/` - Keep documentation updated

### From Prototype to Production
1. Experiment in workbench
2. Validate approach
3. Clean up code
4. Add tests
5. Move to appropriate `src/` module
6. Update documentation

## Best Practices

### Code Organization
- Keep modules focused and single-purpose
- Use clear, descriptive names
- Follow Python conventions (PEP 8)
- Add type hints and docstrings

### Data Management
- Raw data goes in `data/raw/`
- Processed data goes in `data/processed/`
- Don't commit data files to git
- Use .gitkeep to track empty directories

### Testing
- Write tests as you develop
- Aim for >80% code coverage
- Use fixtures for test data
- Mock external API calls

### Documentation
- Update README files when structure changes
- Document complex logic
- Keep examples up-to-date
- Write clear commit messages

## Navigation Tips

### Finding Things
- **API code?** → `src/api/`
- **Database models?** → `src/database/models.py`
- **Analytics?** → `src/analytics/`
- **Tests?** → `tests/unit/` or `tests/integration/`
- **Quick experiment?** → `workbench/experiments/`
- **Data exploration?** → `notebooks/exploratory/`
- **Configuration?** → `config/` or `.env`

### README Locations
Every major directory has a README.md:
```bash
# View all READMEs
find . -name "README.md" -not -path "./.venv/*"
```

## Next Steps

### Immediate Actions
1. ✅ Review DESIGN.md for detailed architecture
2. ✅ Set up virtual environment and install dependencies
3. ✅ Configure .env file
4. ✅ Start experimenting in workbench/
5. ✅ Create your first API client prototype

### Development Phases
See DESIGN.md Section 5 for detailed development phases:
- Phase 1: Foundation
- Phase 2: Data Collection
- Phase 3: Analytics
- Phase 4: Visualization
- Phase 5: Web Interface
- Phase 6: Advanced Features

## Questions?

### Where should I...
- **Test an API endpoint?** → `workbench/experiments/`
- **Explore data?** → `notebooks/exploratory/`
- **Write production code?** → `src/`
- **Add tests?** → `tests/`
- **Store data?** → `data/`
- **Add a script?** → `scripts/`
- **Update docs?** → `docs/`

### Need more info?
- Check the README.md in the relevant directory
- Review DESIGN.md for architecture details
- Look at code examples in workbench/

---

**Structure Created**: 2025-10-22  
**Version**: 1.0  
**Status**: Ready for Development 🚀
