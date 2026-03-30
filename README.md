# AM4 RouteMine

Python CLI that uses the [`am4`](https://github.com/abc8747/am4) package to extract aircraft, airports, and route economics into SQLite, with CSV/Excel export and a **FastAPI** web dashboard (Jinja2, HTMX, Tailwind via CDN).

See **PRD/am4-routemine-PRD.md** and **PRD/am4-routemine-SETUP-GUIDE.md** for the main specification and WSL setup. Dashboard behavior is described in **PRD/am4-routemine-DASHBOARD-SPEC.md**. **My Fleet** (saved slots and route links) is specified in **PRD/am4-routemine-FLEET-SPEC.md**.

## Quick start (WSL Ubuntu)

```bash
cd am4-routemine
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
python main.py extract --hubs KHI,DXB --mode easy --ci 200
python main.py query --hub KHI --top 20
python main.py dashboard
```

Then open **http://127.0.0.1:8000** (default port). Use `--port` and `--host` to change binding.

The dashboard reads the database path from the environment variable **`AM4_ROUTEMINE_DB`** (set automatically when you run `python main.py dashboard --db path/to.db`).

Always call `from am4.utils.db import init; init()` before am4 lookups; `extract` does this internally.

## Commands

- `python main.py extract --hubs IATA1,IATA2` or `--all-hubs`
- `python main.py export --format csv --output ./exports/`
- `python main.py query --hub KHI --top 20`
- `python main.py dashboard [--db am4_data.db] [--port 8000] [--host 0.0.0.0]`

## Dashboard highlights

| Area | Path |
|------|------|
| Overview | `/` |
| Hub Explorer | `/hub-explorer` |
| Aircraft | `/aircraft` |
| Route Analyzer | `/route-analyzer` |
| Fleet Planner | `/fleet-planner` |
| **My Fleet** (saved aircraft + route assignments) | `/my-fleet` |
| Contributions | `/contributions` |
| Heatmap | `/heatmap` |

HTMX fragments and JSON live under **`/api/*`** (see dashboard PRD). Re-run **`extract`** (or apply schema) so older databases gain **`fleet_aircraft`** and **`fleet_route_assignment`** tables if you use My Fleet.
