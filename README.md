> This project is optimized for quick AWS EC2 + RDS deployment for exams and learning purposes.

# Minimal Django Notes App

This is a clean, minimalistic Note-taking application built with Django, using HTML/CSS templates for the frontend, and designed to easily connect to an external AWS RDS MySQL database.

## Prerequisites

To use the ultra-fast **uv** Python package manager (highly recommended), install it first:
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```
*(If you use `uv`, you can skip creating a venv and using `pip` entirely! Just use `uv sync` to install dependencies, and `uv run manage.py migrate` instead).*

## Step-by-Step Setup Guide

### Ubuntu Setup
```bash
sudo apt update
sudo apt install -y python3-pip python3-dev libmysqlclient-dev pkg-config
pip install -r requirements.txt
```

---

### Amazon Linux Setup
```bash
sudo dnf update -y
sudo dnf install -y python3 pip gcc python3-devel mariadb105-devel pkgconfig mariadb105
pip install -r requirements.txt
```

---

### Run Project
```bash
python manage.py migrate
python manage.py runserver 0.0.0.0:8000
```

---

### RDS Setup Notes
* **Create database manually**: Connect to your RDS instance from your EC2 and run:
  ```sql
  CREATE DATABASE testdb;
  ```
* **Update `settings.py`** with your specific connection details:
  * RDS endpoint (`HOST`)
  * Username and Password
* Ensure your RDS **Security Group** allows inbound traffic from your EC2 instance (on port 3306).
