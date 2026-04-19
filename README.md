# Minimal Django Notes App (AWS EC2 + RDS Ready)

This is a minimal Django-based note-taking app designed for **quick AWS deployment (EC2 + RDS)** in exams and learning environments.

---

## 🚀 Overview

This project demonstrates:

* Django app running on EC2
* MySQL database on AWS RDS
* Basic full-stack deployment flow

---

## ⚙️ 1. System Setup (Amazon Linux)

Install required system dependencies:

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

## 🧠 ⚠️ IMPORTANT: Database Must Be Created First

Before running migrations, your RDS database must exist.

---

## 🗄️ 3. Setup RDS Database

Connect to your RDS instance from EC2:

```bash
mysql -h <RDS-ENDPOINT> -u admin -p
```

Then create the database:

```sql
CREATE DATABASE testdb;
```

---

## 🔧 4. Configure Django Database

Update `config/settings.py`:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'testdb',
        'USER': 'admin',
        'PASSWORD': 'your_password',
        'HOST': '<RDS-ENDPOINT>',
        'PORT': '3306',
    }
}
```

---

## 🔐 5. Security Group Configuration (Critical)

Ensure your RDS allows access from EC2:

| Type  | Port | Source             |
| ----- | ---- | ------------------ |
| MySQL | 3306 | EC2 Security Group |

If this is not configured, the app **will not connect to the database**.

---

## 🧪 6. Test Database Connection (Optional)

```bash
mysql -h <RDS-ENDPOINT> -u admin -p
```

If this fails, fix networking before proceeding.

---

## 🔄 7. Run Migrations

### Using uv:

```bash
uv run manage.py migrate
```

### Using venv:

```bash
python manage.py migrate
```

---

## ▶️ 8. Run Development Server

```bash
python manage.py runserver 0.0.0.0:8000
```

---

## 🌐 9. Access the Application

Open in browser:

```
http://<EC2-PUBLIC-IP>:8000
```

---

## ⚡ Optional: Nginx Reverse Proxy (Advanced)

To serve via port 80 instead of 8000:

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

Now access:

```
http://<EC2-PUBLIC-IP>
```

---

## 🧩 Troubleshooting

### ❌ MySQL connection fails

* Check RDS security group
* Verify endpoint, username, password

### ❌ `mysqlclient` install fails

```bash
sudo dnf install mariadb105-devel python3-devel -y
```

Or use fallback:

```bash
pip install PyMySQL
```

Add to settings:

```python
import pymysql
pymysql.install_as_MySQLdb()
```

---

## 🧠 Key Deployment Flow

```
EC2 → Install dependencies
     → Setup Python environment
     → Setup RDS database
     → Configure settings.py
     → Run migrations
     → Start server
     → (Optional) Configure Nginx
```

---

## 🎯 Notes

* This setup is optimized for **exam environments and quick deployment**
* Not production-grade (use Gunicorn, HTTPS, etc. in real systems)

---
