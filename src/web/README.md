# Web Interface (src/web/)

## Overview
Web application providing user-friendly access to NHL analytics.

## Structure
```
web/
├── app.py           # Main application entry point
├── routes.py        # API routes and endpoints
├── templates/       # HTML templates
└── static/          # CSS, JavaScript, images
```

## Features
- Browse teams, players, games
- Custom query builder
- Export data (CSV, JSON, Excel)
- Interactive dashboards
- API endpoints for external access

## Usage

### Start Application
```bash
python src/web/app.py
```

### API Endpoints
```
GET  /api/teams
GET  /api/teams/{abbreviation}
GET  /api/players/{player_id}
GET  /api/games?date={date}
GET  /api/standings
```

## Technologies
- FastAPI - Web framework
- Jinja2 - Templating
- Bootstrap/Tailwind - Styling

## Related Documentation
- [DESIGN.md](../../DESIGN.md) - See Section 3.5 for Web Interface design
