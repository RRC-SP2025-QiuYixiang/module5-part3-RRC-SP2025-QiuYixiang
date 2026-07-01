# Pixel River Financial — Module 5 Part 3

Student portfolio and banking microservices deployed with Docker Compose and NGINX.

**Author:** Yixiang Qiu  
**GitHub:** [RRC-SP2025-QiuYixiang](https://github.com/RRC-SP2025-QiuYixiang)

## Project Overview

This project connects three microservices:

- **studentportfolio** — static HTML portfolio UI
- **backend** — Python/Flask banking API (register, login, deposit, withdraw)
- **transactions** — Node.js/Express service for monthly transaction reports

All services are fronted by **NGINX** on port 80 and share a **MongoDB** database.

## Architecture

```
Browser
   |
   v
NGINX (:80)
   |-- /              --> studentportfolio (static UI)
   |-- /api/*         --> backend (Flask, :5000)
   |-- /api/transactions --> transactions (Node.js, :3000)
                              |
                              v
                           MongoDB (:27017)
```

### Services

| Service | Technology | Port | Role |
|---------|------------|------|------|
| nginx | NGINX Alpine | 80 | Reverse proxy / entry point |
| studentportfolio | HTML + NGINX | 80 (internal) | Portfolio UI |
| backend | Python + Flask | 5000 (internal) | Banking API |
| transactions | Node.js + Express | 3000 (internal) | Transaction reports |
| mongo | MongoDB 6 | 27017 (internal) | Database |

### NGINX Routing

- `/api/transactions` → transactions service
- `/api/` → backend service
- `/` → studentportfolio

## Technologies Used

- **Frontend:** HTML, CSS, Bootstrap, NGINX
- **Backend:** Python, Flask, Flask-PyMongo
- **Transactions:** Node.js, Express, MongoDB driver
- **Database:** MongoDB
- **Proxy:** NGINX
- **Containerization:** Docker, Docker Compose
- **CI/CD:** GitHub Actions

## Run Locally

```bash
cd Module5_Part3
docker compose up --build -d
```

Open `http://localhost/` for the portfolio.  
Banking app: `http://localhost/api/login`  
Transactions API: `http://localhost/api/transactions`

## Stop and Clean Up

```bash
docker compose down -v
```

## Tests

Backend tests run in CI via pytest with mongomock (no live MongoDB required).

```bash
cd backend
pip install -r requirements.txt
pytest -q
```
