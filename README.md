# Minimal Django Notes App

This is a clean, minimalistic Note-taking application built with Django, using `uv` for modern and fast package management. The application uses a custom user model configuration with HTML/CSS templates for the frontend, and is designed to connect to an external RDS MySQL database.

## Prerequisites
- **Git**
- **uv** (Install via `curl -LsSf https://astral.sh/uv/install.sh | sh`)
- **MySQL Libraries** required by `mysqlclient`. For Ubuntu/Debian, install the C-headers via:
  ```bash
  sudo apt-get update
  sudo apt-get install default-libmysqlclient-dev pkg-config
  ```

## Deployment / Setup Instructions

### 1. Clone the repository
```bash
git clone <repository_url>
cd noteproject
```

### 2. Set up Environment Variables
Create a `.env` file in the root directory containing your RDS or MySQL database credentials. By default it uses these generic names:
```ini
DB_NAME=testdb
DB_USER=admin
DB_PASSWORD=your_password
DB_HOST=your-rds-endpoint
DB_PORT=3306
```

### 3. Run the Database Migrations
Run the following command. The magic of `uv` is that you don't need to manually create a virtual environment or run `pip install`. `uv run` will automatically install Python, build an isolated `.venv`, install the exact packages traced in `uv.lock`, and execute the command!
```bash
uv run manage.py migrate
```

### 4. Start the Web Server
Run the development server on your EC2 instance. Binding to `0.0.0.0` makes the application publicly accessible via your EC2's Public IP address (ensure your AWS Security Groups allow inbound traffic on port 8000).
```bash
uv run manage.py runserver 0.0.0.0:8000
```
