---
title: MarketPulse API
description: A Dockerized Financial Microservice in Python
authors:
  - name: Pierre Graef
    to: ""
    avatar:
      src: https://i.pravatar.cc/128?u=2
image:
  src: /img/article_marketpulse/marketpulse.jpg
date: 2025-10-31T01:00:00.000Z
---

MarketPulse API is a small financial microservice I built using Flask, yfinance, and Docker.
It exposes live market indicators, historical data, and two simple dashboard interfaces.
The goal was to design a clean, modular backend project that showcases:

- API development with Python
- Separation of concerns (routes, services, config)
- Good engineering practices (logging, environments, timestamps)
- Dockerized development & production workflows
- Lightweight front-end dashboards

A structure similar to internal tools used in banks, fintech, and corporate analytics teams

This article explains the project’s architecture, the design choices, the key parts of the code, and ends with a short “how to run” guide.

## Architecture & folder structure

```bash
marketpulse_api/
│
├── app/
│   ├── __init__.py             # Flask app factory + logging
│   ├── routes.py               # JSON + HTML endpoints
│   ├── config.py               # Configuration (env, version…)
│   ├── services/
│   │   └── market_service.py   # Market data logic
│   └── templates/              # HTML pages (home, dashboard, history)
│       ├── home.html 
│       ├── dashboard.html   
│       ├── history.html
|       └── navigation_home.html
├── run.py
├── Dockerfile
├── docker-compose.yml
├── docker-compose.prod.yml
└── requirements.txt

```

## Backend : Flask + Services layer

All business logic is in services/market\_service.py .

- Example : fetching the latest indcators

```Python
import yfinance as yf

def get_index(symbol: str) -> Optional[float]:
    """Return the latest closing price of a financial index or asset."""
    try:
        ticker = yf.Ticker(symbol)
        history = ticker.history(period="1d")
        return round(float(history["Close"].iloc[-1]), 2)
    except Exception:
        return None
```

If we use the symbol "^FCHI", the function get\_index("^FCHI") returns the latest CAC40 value (french capitalization index).

## The API layer (routes.py)

*routes.py* defines all API endpoints and HTML pages.

#### Live data (JSON)

```Python
@market_bp.route("/latest", methods=["GET"])
def latest_data():
    data = get_market_data()
    return jsonify({
        "timestamp": datetime.utcnow().isoformat() + "Z",
        "data": data
    })
```

#### Dashboard (HTML)

```Python
@market_bp.route("/dashboard", methods=["GET"])
def dashboard_view():
    data = get_market_data()
    return render_template("dashboard.html", data=data)
```

## UI

MarketPulse API provides two small UI pages :

- /marketpulse/dashboard : Shows the latest EUR/USD, CAC40 and BTC/EUR indicators
- /marketpulse/history/view : Interatcive chart with selectable symbol & period

Both pages are accessible from /marketpulse.

## Config & Environments

All configuration are centralized in *config.py*. Docker ovverides these values depending on the environment.

#### Development

```yaml
environment:
  - APP_ENV=development
  - DEBUG=True
```

#### Production

```yaml
environment:
  - APP_ENV=production
  - DEBUG=False
```

This allows the service to run identically in local and deployment scenarios.

## Docker Setup (dev & prod)

I built two Docker Compose files :

- Development (with auto-refresh)

```bash
docker compose up --build
```

- Production (clean image, no volumes)

```bash
docker compose -f docker-compose.prod.yml up --build
```

## Now let's run it !

After building the project, Docker creates a lightweight image that contains the entire service: the Flask app, the dependencies, and the configuration.This image is reproducible and can be run on any machine, which is exactly what you expect from a small microservice.

![marketpulse-docker-images-run.png](/img/article_marketpulse/marketpulse-docker-images-run.png)

This image will be used to start containers in both development and production environments.

Once the image is ready, the next step is to create a container based on it.
The container represents the running instance of the service — the actual microservice executing the code inside an isolated environment.

![marketpulse-docker-images-run-2.png](/img/article_marketpulse/marketpulse-docker-images-run-2.png)

This makes the app portable and consistent: the code behaves the same way regardless of the machine or OS.

When the container starts, Flask boots inside Docker, and the service becomes available at `http://localhost:5000/marketpulse`.The logs visible in Docker Desktop confirm that the API is running, the routes are loaded, and the logging system is active.

![marketpulse-docker-images-run-3.png](/img/article_marketpulse/marketpulse-docker-images-run-3.png)

Seeing the logs update in real time also helps verify that each API call is correctly handled by the service.

The home page provides a simple navigation menu for the API. Instead of typing each route manually, I added buttons that link directly to the dashboard, the latest data, and the history viewer.

![marketpulse-docker-images-run-4.png](/img/article_marketpulse/marketpulse-docker-images-run-4.png)

The history viewer allows users to select a market symbol and a period (7 days, 1 month, 3 months…) and instantly see the corresponding price evolution.

![marketpulse-docker-images-run-5.png](/img/article_marketpulse/marketpulse-docker-images-run-5.png)

This interface makes the service more interactive and shows how the API can power simple visual dashboards alongside programmatic access.

## How to run the project without Docker

```bash
pip install -r requirements.txt
python run.py
```


## Project Links

- GitHub repository: <https://github.com/PierreGR7/MarketPulse-API>
