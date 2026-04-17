# AlgoQuest

AlgoQuest is a full-stack Django platform for algorithm learning, competitive real-time battles, gamification, and user performance analytics.

[![Live Demo](https://img.shields.io/badge/Live%20Demo-Open%20App-2ea44f?style=for-the-badge)](https://algoquest-web.onrender.com)
![Django](https://img.shields.io/badge/Django-5.x-0C4B33?style=for-the-badge&logo=django)
![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=for-the-badge&logo=python&logoColor=ffdd54)
![DRF](https://img.shields.io/badge/DRF-API-red?style=for-the-badge)
![Channels](https://img.shields.io/badge/Channels-WebSocket-1f6feb?style=for-the-badge)

## Live Demo
- Render deployment: [https://algoquest-web.onrender.com](https://algoquest-web.onrender.com)

## UI Screenshots
Screenshots captured from the live deployment.

### Home
![AlgoQuest Home UI](docs/screenshots/home.png)

### Login
![AlgoQuest Login UI](docs/screenshots/login.png)

### Signup
![AlgoQuest Signup UI](docs/screenshots/signup.png)

### Challenges
![AlgoQuest Challenges UI](docs/screenshots/challenges.png)

### Leaderboard
![AlgoQuest Leaderboard UI](docs/screenshots/leaderboard.png)

## User Manual (Step-by-Step)
This section is for regular users (not admin).

1. Open the app: [https://algoquest-web.onrender.com](https://algoquest-web.onrender.com)
2. Create your account:
   - Go to `/accounts/signup/`
   - Enter username, email, and password
   - Submit signup form
3. Login to your account:
   - Go to `/accounts/login/`
   - Enter username/password
4. Visit your dashboard:
   - Open `/users/dashboard/` to view activity and progress
5. Start solving challenges:
   - Open `/challenges/`
   - Select a challenge and submit your solution
6. Join real-time battles:
   - Open `/battle/`
   - Join or start a battle room and compete live
7. Track ranking:
   - Open `/leaderboard/`
   - Check your position on global/weekly boards
8. Manage your profile:
   - Open `/users/profile/`
   - Update profile details and review your performance

## Stack
- Backend: Django 5
- APIs: Django REST framework
- Real-time: Django Channels (WebSockets)
- Database: SQLite for local development, PostgreSQL recommended for production
- Frontend: Django Templates + Bootstrap 5 + vanilla JavaScript

## Apps
- `users`: auth flows, profile, dashboard, user API
- `challenges`: challenge catalog, challenge attempts, algorithm views/API
- `battle`: matchmaking + live battle pages + WebSocket consumer/API
- `leaderboard`: global/weekly ranking, rewards, leaderboard API
- `analytics`: performance tracking and challenge recommendations

## Project Structure
```text
AlgoQuest/
  AlgoQuest/
    settings.py
    urls.py
    asgi.py
  users/
  challenges/
  battle/
  leaderboard/
  analytics/
  templates/
  static/
  manage.py
  requirements.txt
```

## Setup
1. Create and activate a virtual environment.
2. Install dependencies:
```bash
pip install -r requirements.txt
```
3. Run migrations:
```bash
python manage.py makemigrations
python manage.py migrate
```
4. Create a superuser:
```bash
python manage.py createsuperuser
```
5. Seed sample data (users, BFS/DFS/A* challenges, leaderboard):
```bash
python manage.py seed_data
```

## Database Configuration
Local development can continue using SQLite by leaving `DJANGO_USE_POSTGRES=False`.

For PostgreSQL, use either:

```env
DJANGO_USE_POSTGRES=True
POSTGRES_DB=algoquest_db
POSTGRES_USER=algoquest_user
POSTGRES_PASSWORD=replace-with-db-password
POSTGRES_HOST=127.0.0.1
POSTGRES_PORT=5432
POSTGRES_SSLMODE=require
```

or a single connection string:

```env
DATABASE_URL=postgresql://algoquest_user:replace-with-db-password@127.0.0.1:5432/algoquest_db
```

For production, PostgreSQL is the recommended database. On Render, set `DATABASE_URL` from your Render PostgreSQL connection string.

## Render Deployment
This repository includes both `build.sh` and `render.yaml` for Render deployment.

The easiest path is using a Render Blueprint:

1. Push this repository to GitHub.
2. In Render, create a new Blueprint and point it to this repository.
3. Render will create:
   - a PostgreSQL database named `algoquest-db`
   - a Python web service named `algoquest-web`
4. The Blueprint will:
   - install dependencies
   - run `collectstatic`
   - run `migrate`
   - start the app with Daphne/ASGI

If you deploy manually in the Render dashboard instead, use:

```bash
Build Command: bash build.sh
Start Command: daphne -b 0.0.0.0 -p $PORT AlgoQuest.asgi:application
```

Recommended environment variables for a manual Render deploy:

```env
DJANGO_DEBUG=False
DJANGO_SECRET_KEY=replace-with-a-new-secret
DATABASE_URL=postgresql://...
```

Render automatically provides `RENDER` and `RENDER_EXTERNAL_HOSTNAME`, and this project uses those values to derive safe production defaults for `DEBUG`, `ALLOWED_HOSTS`, and `CSRF_TRUSTED_ORIGINS` when you do not set them manually.

Notes:
- Static files are served by WhiteNoise in production.
- User-uploaded media should be moved to object storage before production use.
- The default SQLite database is only for local development.

## Run Servers
### HTTP development server
```bash
python manage.py runserver
```

### WebSocket-capable ASGI server
Use either command:
```bash
python manage.py runserver
```
or
```bash
daphne -p 8000 AlgoQuest.asgi:application
```

## Default Seed Users
- `alice` / `AlgoQuest123!`
- `bob` / `AlgoQuest123!`
- `charlie` / `AlgoQuest123!`

## Main Pages
- `/` Home
- `/users/dashboard/` Dashboard
- `/challenges/` Challenges list
- `/battle/` Battle lobby
- `/leaderboard/` Leaderboard
- `/users/profile/` Profile & settings

## API Endpoints
- `/api/challenges/`
- `/api/leaderboard/`
- `/api/battle/`
- `/api/users/`
- `/api/analytics/`

## Battle WebSocket Endpoint
- `ws://<host>/ws/battle/<room_code>/`

## Notes
- If `channels` is not installed in your environment, HTTP features still run, but WebSocket battle mode requires installing `channels` (and optionally `daphne`).
