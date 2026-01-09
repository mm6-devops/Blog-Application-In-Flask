# Docker Guide for Flask Blog

This guide explains how to run the Flask Blog application using Docker.

## Prerequisites

- Docker installed on your system
- Docker Compose (optional, but recommended)

## Quick Start

### Option 1: Using Docker Compose (Easiest)

1. **Create a `.env` file** (if you haven't already):
```bash
SECRET_KEY=your-secret-key-change-this
DATABASE_URL=sqlite:///instance/site.db
EMAIL_USER=your-email@gmail.com
EMAIL_PASS=your-app-password
```

2. **Build and run**:
```bash
docker-compose up --build
```

3. **Access the application** at `http://localhost:5000`

### Option 2: Using Docker directly

1. **Build the image**:
```bash
docker build -t flask-blog .
```

2. **Run the container**:
```bash
docker run -d -p 5000:5000 \
  --env-file .env \
  -v $(pwd)/instance:/app/instance \
  -v $(pwd)/flaskblog/static/profile_pics:/app/flaskblog/static/profile_pics \
  --name flask-blog \
  flask-blog
```

## Docker Compose Commands

- **Start services**: `docker-compose up`
- **Start in background**: `docker-compose up -d`
- **Stop services**: `docker-compose down`
- **View logs**: `docker-compose logs -f`
- **Rebuild**: `docker-compose up --build`
- **Restart**: `docker-compose restart`

## Docker Commands

- **Build image**: `docker build -t flask-blog .`
- **Run container**: `docker run -d -p 5000:5000 --name flask-blog flask-blog`
- **Stop container**: `docker stop flask-blog`
- **Start container**: `docker start flask-blog`
- **View logs**: `docker logs -f flask-blog`
- **Remove container**: `docker rm flask-blog`
- **Remove image**: `docker rmi flask-blog`

## Environment Variables

You can set environment variables in several ways:

1. **Using `.env` file** (recommended):
   - Create a `.env` file in the project root
   - Docker Compose will automatically load it

2. **Using Docker Compose**:
   - Edit the `environment` section in `docker-compose.yml`

3. **Using Docker run**:
   ```bash
   docker run -e SECRET_KEY=your-key -e DATABASE_URL=... flask-blog
   ```

## Volumes

The Docker setup uses volumes to persist data:

- `./instance:/app/instance` - Database files
- `./flaskblog/static/profile_pics:/app/flaskblog/static/profile_pics` - User profile pictures

This ensures your data persists even if you remove the container.

## Production Considerations

For production deployment:

1. **Use a production WSGI server** (e.g., Gunicorn):
   ```dockerfile
   RUN pip install gunicorn
   CMD ["gunicorn", "--bind", "0.0.0.0:5000", "run:app"]
   ```

2. **Use a proper database** (PostgreSQL, MySQL) instead of SQLite

3. **Set `FLASK_ENV=production`** to disable debug mode

4. **Use secrets management** for sensitive data

5. **Add health checks** to your Dockerfile

6. **Use multi-stage builds** to reduce image size

## Troubleshooting

### Container won't start
- Check logs: `docker logs flask-blog`
- Verify port 5000 is not in use
- Check environment variables

### Database not persisting
- Ensure volumes are properly mounted
- Check file permissions

### Can't access the application
- Verify the port mapping: `-p 5000:5000`
- Check if the container is running: `docker ps`
- Try accessing `http://localhost:5000` or `http://127.0.0.1:5000`
