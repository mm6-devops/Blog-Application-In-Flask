# Flask Blog

A full-featured blog application built with Flask.

## Features

- User registration and authentication
- User login/logout with "Remember Me" functionality
- User profile management with profile picture upload
- Create, read, update, and delete blog posts
- Password reset via email
- Pagination for blog posts
- User-specific post pages
- Error handling (404, 403, 500)

## Installation

1. Create a virtual environment:
```bash
python -m venv venv
```

2. Activate the virtual environment:
- On Windows:
```bash
venv\Scripts\activate
```
- On macOS/Linux:
```bash
source venv/bin/activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

4. Set up environment variables (optional):
Create a `.env` file or set environment variables:
- `SECRET_KEY`: Flask secret key
- `DATABASE_URL`: Database connection string (defaults to SQLite)
- `EMAIL_USER`: Email username for password reset
- `EMAIL_PASS`: Email password for password reset

5. Run the application:
```bash
python run.py
```

The application will be available at `http://localhost:5000`

## Docker Installation

### Using Docker Compose (Recommended)

1. Make sure Docker and Docker Compose are installed on your system.

2. Create a `.env` file in the project root (if you haven't already):
```bash
SECRET_KEY=your-secret-key-here
DATABASE_URL=sqlite:///instance/site.db
EMAIL_USER=your-email@gmail.com
EMAIL_PASS=your-app-password
```

3. Build and run the container:
```bash
docker-compose up --build
```

4. The application will be available at `http://localhost:5000`

### Using Docker directly

1. Build the Docker image:
```bash
docker build -t flask-blog .
```

2. Run the container:
```bash
docker run -d -p 5000:5000 \
  -e SECRET_KEY=your-secret-key \
  -e DATABASE_URL=sqlite:///instance/site.db \
  -v $(pwd)/instance:/app/instance \
  -v $(pwd)/flaskblog/static/profile_pics:/app/flaskblog/static/profile_pics \
  --name flask-blog \
  flask-blog
```

3. The application will be available at `http://localhost:5000`

### Docker Commands

- Stop the container: `docker-compose down` or `docker stop flask-blog`
- View logs: `docker-compose logs -f` or `docker logs -f flask-blog`
- Rebuild after changes: `docker-compose up --build`
- Run in detached mode: `docker-compose up -d`

## Project Structure

```
flaskblog/
├── __init__.py          # Application factory
├── routes.py            # Route handlers
├── models.py            # Database models
├── forms.py             # WTForms form classes
├── config.py            # Configuration settings
├── utils.py             # Utility functions
├── static/              # Static files (CSS, images)
│   ├── main.css
│   └── profile_pics/
└── templates/           # HTML templates
    ├── layout.html
    ├── home.html
    ├── register.html
    ├── login.html
    ├── account.html
    ├── create_post.html
    ├── post.html
    ├── user_posts.html
    ├── reset_request.html
    ├── reset_token.html
    ├── about.html
    └── errors/
        ├── 404.html
        ├── 403.html
        └── 500.html
run.py                   # Application entry point
requirements.txt         # Python dependencies
```

## Database

The application uses SQLite by default. The database will be created automatically when you run the application for the first time.

To create the database tables:
```python
from flaskblog import create_app, db
app = create_app()
with app.app_context():
    db.create_all()
```

## Notes

- Make sure to add a default profile picture at `flaskblog/static/profile_pics/default.jpg`
- For email functionality to work, configure your email settings in `config.py` or via environment variables
- The application uses Flask-Login for session management
- Password hashing is done using Flask-Bcrypt
