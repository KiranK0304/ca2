# Minimal Django Notes App (AWS EC2 + RDS Ready)

This is a minimal Django-based note-taking app designed for **quick AWS deployment (EC2 + RDS)** in exams and learning environments.

---

## 🚀 Overview

This project demonstrates:

* Django app running on EC2
* MySQL database on AWS RDS
* Basic deployment workflow

---

## ⚙️ 1. System Setup (Amazon Linux)

```bash
sudo dnf update -y
sudo dnf install -y python3 python3-pip gcc python3-devel mariadb105-devel pkgconfig mariadb105 git
```

---

## 📦 2. Python Environment Setup

### Option A — Using uv (Recommended)

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
uv sync
```

### Option B — Using venv

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

---

## ▶️ 3. Run Project

```bash
python manage.py migrate
python manage.py runserver 0.0.0.0:8000
```

---

## 🌐 Access the Application

Open in browser:

```
http://<EC2-PUBLIC-IP>:8000
```

---

## ⚡ Optional: Nginx Reverse Proxy

1. Install Nginx:

```bash
sudo dnf install nginx -y
```

2. Configure:

```nginx
server {
    listen 80;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

3. Restart:

```bash
sudo systemctl restart nginx
```

Access:

```
http://<EC2-PUBLIC-IP>
```

---

## 🎯 Notes

* Ensure RDS is already created and accessible

* Database connection must be correctly configured in `settings.py`

* Security group must allow MySQL (3306) from EC2

* This setup is optimized for **exam use and quick deployment**

* Not production-ready

---
