# Docker Compose (Backend + Frontend + MySQL)

Now that both the backend and frontend are running with Docker, I combined everything into a single `docker-compose.yml` so I can start the whole app with one command instead of running each service manually.

## Step 1: Create `docker-compose.yml`

In the root of the project (same level as `backend/` and `frontend/`), create the file:

```bash
cd ~/book-review-app
touch docker-compose.yml
```

###  docker-compose.yml

```yaml
version: '2.1'

services:
  mysql:
    image: mysql:8.0
    container_name: mysql_container
    restart: always
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
      interval: 10s
      timeout: 5s
      retries: 5
    environment:
      MYSQL_ROOT_PASSWORD: my-secret-pw
      MYSQL_DATABASE: book_review_db
      MYSQL_USER: pravin
      MYSQL_PASSWORD: Demo12@Test23
    ports:
      - "3306:3306"
    volumes:
      - mysql_data:/var/lib/mysql

  backend:
    build: ./backend
    container_name: backend_container
    restart: always
    depends_on:
      mysql:
        condition: service_healthy
    environment:
      PORT: 3001
      DB_HOST: mysql
      DB_NAME: book_review_db
      DB_USER: pravin
      DB_PASS: Demo12@Test23
      DB_DIALECT: mysql
      JWT_SECRET: mysecretkey
      ALLOWED_ORIGINS: http://<FRONTEND PUBLIC IP>:3000
    ports:
      - "3001:3001"

  frontend:
    build: ./frontend
    container_name: frontend_container
    restart: always
    depends_on:
      - backend
    environment:
      NEXT_PUBLIC_API_URL: http://<BACKEND PUBLIC IP>:3001
    ports:
      - "3000:3000"

volumes:
  mysql_data:
```

This setup runs : MySQL database , Backend API , Frontend app
  all connected together

## Step 2: Update `.env` files

Before running everything, I made sure the environment variables are aligned.

### Backend `.env`

```env
DB_HOST=mysql
DB_NAME=book_review_db
DB_USER=appuser
DB_PASS=apppassword
DB_DIALECT=mysql
PORT=3001
JWT_SECRET=mysecretkey
ALLOWED_ORIGINS=http://localhost:3000,http://your-public-ip:3000
```

### Frontend `.env`

```env
NEXT_PUBLIC_API_URL=http://localhost:3001
```

## Step 3: Start the Application

```bash
docker-compose up -d --build
```

### Check running containers:

```bash
docker ps
```

---

## Step 4: Verify Everything

### Check logs

```bash
docker logs backend-container
docker logs frontend-container
docker logs mysql-container
```

### Test backend

```bash
curl http://localhost:3001/
```

### Expected:

```
Book Review API is running...
```

### Access the app

* Frontend → [http://localhost:3000](http://localhost:3000)
* Backend → [http://localhost:3001/api/books](http://localhost:3001/api/books)

---

## Step 5 : Stop / Restart

### Stop everything:

```bash
docker-compose down
```

### Restart:

```bash
docker-compose up -d
```

### Restart only backend:

```bash
docker-compose restart backend
```