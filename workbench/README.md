# Workbench (workbench/)

## 🔧 Your Prototyping Playground!

This is a **self-contained area for rapid prototyping and experimentation**. No rules, no tests required, just experiment and learn!

## Structure
```
workbench/
├── experiments/     # Quick tests and proof-of-concepts
├── prototypes/      # Feature prototypes before integration
└── sandbox/         # Free-form experimentation and learning
```

## Purpose

### experiments/
Quick tests and proof-of-concepts:
- Test NHL API endpoints
- Try out new libraries
- Validate approaches
- Quick data checks

**Example:**
```python
# experiments/test_nhl_api.py
import requests

response = requests.get("https://api-web.nhle.com/v1/teams")
print(response.json())
```

### prototypes/
Feature prototypes before moving to production:
- New analytics algorithms
- Visualization experiments
- Database query optimizations
- UI components

**Example:**
```python
# prototypes/shot_chart_prototype.py
import matplotlib.pyplot as plt
# Prototype shot chart visualization
```

### sandbox/
Free-form experimentation:
- Learning new techniques
- Testing ideas
- Playing with data
- Tutorial code

## Guidelines

### What Goes Here
✅ Quick experiments
✅ Feature prototypes
✅ Learning code
✅ Throwaway scripts
✅ API exploration
✅ Data exploration

### What Doesn't Go Here
❌ Production code (use `src/`)
❌ Reusable functions (use `src/utils/`)
❌ Formal tests (use `tests/`)
❌ Notebooks (use `notebooks/`)

## Workflow

### 1. Experiment
Start with a quick script:
```bash
# Create experiment
touch workbench/experiments/test_standings_api.py

# Run it
python workbench/experiments/test_standings_api.py
```

### 2. Prototype
If the experiment works, create a prototype:
```bash
# Create prototype
mkdir workbench/prototypes/standings_collector
# Develop more structured code
```

### 3. Production
When ready, move to production:
```bash
# Clean up code
# Add tests
# Move to src/
mv workbench/prototypes/standings_collector/collector.py src/collectors/standings.py
```

## Best Practices

### Organization
- Use descriptive file names
- Add comments to explain your thinking
- Date files if tracking progress (e.g., `2024_01_15_api_test.py`)

### Documentation
- Add a comment at the top explaining what you're testing
- Note any interesting findings
- Link to relevant documentation

### Cleanup
- Archive successful prototypes when moved to production
- Delete failed experiments periodically
- Keep the workbench tidy

## Example Files

### Quick API Test
```python
# experiments/quick_api_test.py
"""
Testing the new NHL API endpoint for player stats.
Goal: Verify data structure and response format.
"""
import requests
import json

url = "https://api-web.nhle.com/v1/player/8478402/landing"
response = requests.get(url)
data = response.json()

# Print structure
print(json.dumps(data, indent=2))

# Findings:
# - Response includes career stats
# - Need to handle missing fields
# - Response time: ~200ms
```

### Feature Prototype
```python
# prototypes/player_streak_detector.py
"""
Prototype for detecting hot/cold streaks in player performance.
Algorithm: Moving average with threshold detection.
"""
import pandas as pd

def detect_streaks(games, threshold=0.5, window=5):
    """
    Detect performance streaks.
    threshold: Points per game threshold
    window: Games to consider for moving average
    """
    df = pd.DataFrame(games)
    df['ma'] = df['points'].rolling(window).mean()
    df['hot'] = df['ma'] > threshold
    return df

# Test with sample data
sample_games = [
    {'game': 1, 'points': 2},
    {'game': 2, 'points': 1},
    # ...
]

result = detect_streaks(sample_games)
print(result)
```

## Tips

### Debugging
- Add print statements liberally
- Use simple test data
- Test one thing at a time

### Learning
- Try new libraries here first
- Experiment with different approaches
- Don't worry about perfect code

### Sharing
- If you find something useful, share it
- Move successful prototypes to production
- Document lessons learned

## No Judgement Zone
This is your space to:
- Make mistakes
- Try wild ideas
- Break things
- Learn by doing

**Remember:** All great features start as messy experiments!

## Next Steps

1. Start experimenting! Create your first test:
```bash
touch workbench/experiments/my_first_test.py
```

2. When you have something working, move it to prototypes

3. When it's polished, move it to production in `src/`

Happy experimenting! 🚀
