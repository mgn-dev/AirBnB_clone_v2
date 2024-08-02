# HBnB — Phase 2: Web Framework

**AirBnB Clone · Phase 2 of 4**

Phase 2 introduces **Flask** as the delivery layer for HBnB. Retains the Python domain model and JSON-backed console from Phase 1, then covers HTTP routes, URL parameters, and **Jinja2 templates** turn backend data into rendered HTML pages.

| Phase | Project | Focus |
|-------|------------|-------|
| 1 | [AirBnB_clone](https://github.com/mgn-dev/AirBnB_clone) | Python OOP, console, JSON storage, static HTML/CSS |
| **2 — current phase** | `AirBnB_clone_v2` | Flask routes and Jinja2 templates |
| 3 | [AirBnB_clone_v3](https://github.com/mgn-dev/AirBnB_clone_v3) | REST API and CRUD endpoints |
| 4 | [AirBnB_clone_v4](https://github.com/mgn-dev/AirBnB_clone_v4) | MySQL, SQLAlchemy, Swagger, dynamic JS, auth |

---

## Skills covered


- Create a **Flask application** with modular route handlers
- Map URL paths and **query parameters** to Python functions
- Render dynamic HTML with **Jinja2 templates** and pass context variables
- Serve conditional content (odd/even checks, typed route segments)
- Connect Flask views to the existing **HBnB storage layer**
- Deploy static assets to a remote server with **Fabric** and **Puppet** scripts
- Prepare a **MySQL development database** for later phases

---

## Project Structure

```
AirBnB_clone_v2/
├── console.py                  # Phase 1 command interpreter (extended)
├── models/                     # Domain model + FileStorage (from Phase 1)
├── web_flask/
│   ├── 0-hello_route.py        # "Hello HBNB!" root route
│   ├── 1-hbnb_route.py         # /hbnb route
│   ├── 2-c_route.py            # /c/<text> with underscore-to-space
│   ├── 3-python_route.py       # /python/<text> with default value
│   ├── 4-number_route.py       # /number/<int:n> type validation
│   ├── 5-number_template.py    # Jinja2 template rendering
│   ├── 6-number_odd_or_even.py # Conditional template logic
│   ├── 7-states_list.py        # List states from storage in HTML
│   ├── 8-cities_by_states.py   # Nested state/city listing
│   ├── 9-states.py             # Full state detail pages
│   └── templates/              # Jinja2 HTML templates
├── web_static/                 # Static HTML/CSS assets + deployment targets
├── setup_mysql_dev.sql         # MySQL dev database bootstrap
├── setup_mysql_test.sql        # MySQL test database bootstrap
└── tests/
```

---

## Flask Progression (`web_flask/`)

Each numbered script is a self-contained learning milestone. Run any module directly:

```bash
python3 web_flask/0-hello_route.py
# Visit http://0.0.0.0:5000/
```

| Task | Module | Route(s) | Concept |
|------|--------|----------|---------|
| 0 | `0-hello_route.py` | `/` | Minimal Flask app, `strict_slashes=False` |
| 1 | `1-hbnb_route.py` | `/`, `/hbnb` | Multiple routes, string responses |
| 2 | `2-c_route.py` | `/c/<text>` | Path variables, `_` → space |
| 3 | `3-python_route.py` | `/python/`, `/python/<text>` | Default parameter values |
| 4 | `4-number_route.py` | `/number/<int:n>` | Typed converters, 404 on invalid input |
| 5 | `5-number_template.py` | `/number_template/<int:n>` | `render_template()` with context |
| 6 | `6-number_odd_or_even.py` | `/number_odd_or_even/<int:n>` | Template conditionals |
| 7 | `7-states_list.py` | `/states_list` | Read from `storage`, sort, render list |
| 8 | `8-cities_by_states.py` | `/cities_by_states` | Nested relationships in templates |
| 9 | `9-states.py` | `/states/<id>` | Detail view for a single state |

### Template Example

`5-number_template.py` passes an integer to Jinja2:

```python
@app.route('/number_template/<int:n>', strict_slashes=False)
def number_template(n):
    return render_template('5-number.html', n=n)
```

The template receives `n` and renders it inside HTML markup.

---

## Connecting Flask to the Domain Model

Starting at task 7, routes import the shared storage singleton:

```python
from models import storage

states = sorted(storage.all("State").values(), key=lambda x: x.name)
return render_template('7-states_list.html', states=states)
```

This bridges the **console-driven backend** from Phase 1 with **server-rendered pages**—the same pattern used in production MVC applications.

---

## Static Asset Deployment

Root-level scripts automate packaging and deploying `web_static/` to remote servers:

| Script | Purpose |
|--------|---------|
| `0-setup_web_static.sh` | Prepare the remote Nginx document root |
| `1-pack_web_static.py` | Archive static files for transfer |
| `2-do_deploy_web_static.py` | Upload and extract the archive via Fabric |
| `3-deploy_web_static.py` | Full deploy pipeline (pack + upload + cleanup) |
| `100-clean_web_static.py` | Remove old releases |
| `101-setup_web_static.pp` | Puppet manifest for server configuration |

These teach **DevOps fundamentals**: idempotent server setup, release packaging, and zero-downtime static deploys.

---

## MySQL Preparation

SQL setup scripts create isolated databases for development and testing:

```bash
cat setup_mysql_dev.sql | mysql -u root -p
```

Environment variables used in later phases:

| Variable | Example |
|----------|---------|
| `HBNB_MYSQL_USER` | `hbnb_dev` |
| `HBNB_MYSQL_PWD` | `hbnb_dev_pwd` |
| `HBNB_MYSQL_HOST` | `localhost` |
| `HBNB_MYSQL_DB` | `hbnb_dev_db` |

---

## Console (Still Available)

The Phase 1 interpreter remains fully functional for seeding data before hitting Flask routes:

```bash
./console.py
(hbnb) create State
(hbnb) update State <id> name "California"
(hbnb) quit
```

Populate states and cities in the console, then browse them at `/states_list`.

---

## Environment

- **Python** 3.x
- **Flask** — web microframework
- **Fabric** — remote deployment (tasks 2–3)
- **Nginx** — static file server on the target host

---

## What Came Before · What Comes Next

- **[Phase 1](https://github.com/mgn-dev/AirBnB_clone)** — Introduces the models, JSON persistence, console, and static CSS/HTML prototype.
- **[Phase 3](https://github.com/mgn-dev/AirBnB_clone_v3)** — The curriculum exposes the same data over a **JSON REST API** with full CRUD, replacing page-by-page rendering with machine-readable endpoints.

---

## License

Public domain.
