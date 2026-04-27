# Ads-sync Setup Guide

This repository contains two Python apps:

- FastAPI backend server (root app via `main.py`)
- Media player app (`ads-player/`)

## 1) Prerequisites

- Python 3.10+ installed
- PostgreSQL running locally (or reachable remotely)
- VLC media player installed (required by `python-vlc`)

## 2) Create and activate virtual environment

From repository root:

```bash
python -m venv .venv
```

### Windows (Git Bash)

```bash
source .venv/Scripts/activate
```

### Windows (PowerShell)

```powershell
.venv\Scripts\Activate.ps1
```

### macOS/Linux

```bash
source .venv/bin/activate
```

## 3) Install dependencies

```bash
python -m pip install --upgrade pip setuptools wheel
pip install -r requirements.txt
```
### This command ensures your Python package manager and its core installation tools are up to date. Here is the breakdown: 

- python: Runs the Python interpreter.
- -m pip: Tells Python to run the pip module as a script. This is often safer than typing pip directly because it ensures you are using the specific version of pip tied to that Python installation.
- install: The command to download and set up new packages.
- --upgrade: Tells pip to update the specified packages to the latest available versions if they are already installed.
- pip setuptools wheel: These are the three target packages:
- pip: The package manager itself.
- setuptools: Used to download, build, and install Python packages.
- wheel: A format that allows for faster installations by using pre-compiled binaries instead of building from source every time. 

## 4) Create `.env` in repo root

Create a `.env` file in the project root with:

```env
DATABASE_URL=postgresql+psycopg2://postgres:root@localhost:5432/adsync
CLOUDINARY_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret
```

Adjust values for your environment.

## 5) Apply database migrations

From project root:

```bash
alembic upgrade head
```

## 6) Start FastAPI server

From project root:

```bash
uvicorn main:app --host 127.0.0.1 --port 8000 --reload
```

Useful endpoints:

- API root: `http://127.0.0.1:8000/`
- Swagger docs: `http://127.0.0.1:8000/docs`

## 7) Start media player app

Open a second terminal, activate the same venv, then:

```bash
cd ads-player
python main.py
```

The player is configured to connect to backend on `127.0.0.1:8000`, so keep the FastAPI server running first.

## 8) Typical startup flow (daily use)

Terminal 1 (server):

```bash
source .venv/Scripts/activate
uvicorn main:app --host 127.0.0.1 --port 8000 --reload
```

Terminal 2 (player):

```bash
source .venv/Scripts/activate
cd ads-player
python main.py
```

