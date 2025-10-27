# Configuration (config/)

## Overview
Environment-specific configuration files for development, testing, and production environments.

## Configuration Files

### development.yaml
Development environment settings.

```yaml
database:
  url: sqlite:///./nhl_data.db
  echo: true

api:
  base_url: https://api-web.nhle.com/v1/
  timeout: 30
  rate_limit: 100

logging:
  level: DEBUG
  format: detailed
```

### production.yaml
Production environment settings.

```yaml
database:
  url: ${DATABASE_URL}  # From environment variable
  pool_size: 10
  echo: false

api:
  base_url: https://api-web.nhle.com/v1/
  timeout: 10
  rate_limit: 50

logging:
  level: INFO
  format: json
```

### logging.yaml
Logging configuration.

```yaml
version: 1
formatters:
  simple:
    format: '%(levelname)s - %(message)s'
  detailed:
    format: '%(asctime)s - %(name)s - %(levelname)s - %(message)s'

handlers:
  console:
    class: logging.StreamHandler
    formatter: detailed
    level: DEBUG
  
  file:
    class: logging.handlers.RotatingFileHandler
    filename: logs/app.log
    maxBytes: 10485760  # 10MB
    backupCount: 5
    formatter: detailed
    level: INFO

root:
  level: DEBUG
  handlers: [console, file]
```

## Loading Configuration

### Python Code
```python
from src.utils.config import load_config

# Load environment-specific config
config = load_config("development")

# Access values
db_url = config['database']['url']
log_level = config['logging']['level']
```

### Environment Variables
Configuration can be overridden with environment variables:

```bash
# .env file
DATABASE_URL=postgresql://user:pass@localhost/nhl_db
API_TIMEOUT=30
LOG_LEVEL=DEBUG
```

## Configuration Priority

1. Environment variables (highest priority)
2. Environment-specific YAML file
3. Default values (lowest priority)

## Best Practices

### Security
- Never commit sensitive data
- Use environment variables for secrets
- Use .env.example as template

### Organization
- Group related settings
- Use clear naming
- Document required vs optional settings

### Validation
- Validate configuration on startup
- Provide clear error messages
- Use type hints for config values

## Example Configuration Class

```python
# src/utils/config.py
from dataclasses import dataclass
from typing import Optional
import yaml
import os

@dataclass
class DatabaseConfig:
    url: str
    pool_size: int = 5
    echo: bool = False

@dataclass
class APIConfig:
    base_url: str
    timeout: int = 30
    rate_limit: int = 100

@dataclass
class Config:
    database: DatabaseConfig
    api: APIConfig
    
    @classmethod
    def load(cls, env: str = "development"):
        with open(f"config/{env}.yaml") as f:
            data = yaml.safe_load(f)
        
        # Override with environment variables
        if db_url := os.getenv("DATABASE_URL"):
            data['database']['url'] = db_url
        
        return cls(
            database=DatabaseConfig(**data['database']),
            api=APIConfig(**data['api'])
        )
```

## Related Documentation
- [.env.example](../.env.example) - Environment variables template
- [src/utils/config.py](../src/utils/config.py) - Configuration loader
