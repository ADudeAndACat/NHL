# Tests (tests/)

## Overview
Comprehensive test suite for the NHL Analytics project.

## Structure
```
tests/
├── unit/            # Unit tests for individual components
├── integration/     # Integration tests for module interactions
└── fixtures/        # Test data and mock responses
```

## Running Tests

### All Tests
```bash
pytest
```

### Specific Test File
```bash
pytest tests/unit/test_api_client.py
```

### With Coverage
```bash
pytest --cov=src --cov-report=html
```

### With Verbose Output
```bash
pytest -v
```

## Writing Tests

### Unit Test Example
```python
import pytest
from src.api.client import NHLApiClient

def test_api_client_initialization():
    client = NHLApiClient()
    assert client.base_url == "https://api-web.nhle.com/v1/"

@pytest.mark.asyncio
async def test_get_teams():
    client = NHLApiClient()
    teams = await client.get_teams()
    assert len(teams) > 0
```

### Integration Test Example
```python
@pytest.mark.integration
async def test_full_data_flow():
    # Collect data
    collector = GameCollector()
    games = await collector.fetch_games_by_date("2024-01-15")
    
    # Store in database
    for game in games:
        await collector.store(game)
    
    # Query from database
    stored = await get_games_by_date("2024-01-15")
    assert len(stored) == len(games)
```

### Using Fixtures
```python
# conftest.py
@pytest.fixture
def mock_api_response():
    return {
        "teams": [
            {"id": 10, "name": "Toronto Maple Leafs", "abbreviation": "TOR"}
        ]
    }

# test_file.py
def test_with_fixture(mock_api_response):
    assert "teams" in mock_api_response
```

## Test Organization

### Unit Tests (tests/unit/)
Test individual functions and classes in isolation:
- `test_api_client.py` - API client tests
- `test_collectors.py` - Data collector tests
- `test_models.py` - Database model tests
- `test_analytics.py` - Analytics function tests

### Integration Tests (tests/integration/)
Test interactions between components:
- `test_data_flow.py` - End-to-end data collection and storage
- `test_api_database.py` - API to database integration
- `test_analytics_pipeline.py` - Full analytics workflow

### Fixtures (tests/fixtures/)
Mock data and responses:
- `api_responses.json` - Sample API responses
- `test_database.py` - Test database setup
- `mock_data.py` - Mock NHL data

## Best Practices

### Test Coverage
- Aim for >80% code coverage
- Test both success and failure cases
- Test edge cases and boundary conditions

### Mocking
- Mock external API calls
- Use fixtures for test data
- Isolate tests from external dependencies

### Test Naming
- Use descriptive test names
- Follow pattern: `test_<function>_<scenario>_<expected_result>`
- Example: `test_get_player_with_invalid_id_raises_error`

### Markers
```python
# Skip test
@pytest.mark.skip(reason="Not implemented yet")

# Expected to fail
@pytest.mark.xfail

# Integration test
@pytest.mark.integration

# Slow test
@pytest.mark.slow
```

## Configuration

### pytest.ini
```ini
[pytest]
testpaths = tests
python_files = test_*.py
python_classes = Test*
python_functions = test_*
markers =
    integration: Integration tests
    slow: Slow running tests
    unit: Unit tests
```

### Coverage Configuration
```ini
[coverage:run]
source = src
omit = 
    */tests/*
    */migrations/*
    */__init__.py

[coverage:report]
exclude_lines =
    pragma: no cover
    def __repr__
    raise AssertionError
    raise NotImplementedError
```

## Continuous Integration

### GitHub Actions Example
```yaml
name: Tests
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: 3.11
      - name: Install dependencies
        run: pip install -r requirements.txt
      - name: Run tests
        run: pytest --cov=src
```

## Resources
- [pytest Documentation](https://docs.pytest.org/)
- [pytest-cov](https://pytest-cov.readthedocs.io/)
- [unittest.mock](https://docs.python.org/3/library/unittest.mock.html)
