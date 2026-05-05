#  Running the Book Review App with Docker

In this guide, I’ll walk through how I run this project using Docker.
The idea is to containerize both the frontend and backend, then run everything together.

What I covered here:

* Creating Dockerfiles for both services
* Running each container manually
* Using Docker Compose to simplify everything
* Testing that the app actually works


##  Step 1: Dockerfiles (Backend & Frontend)

###  Backend Dockerfile

Inside the `backend` folder, I created this Dockerfile:

```dockerfile
FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3001

CMD ["node", "src/server.js"]
```

Nothing fancy here, just a simple setup that installs dependencies, copies the code, and runs the server.


###  Frontend Dockerfile

Inside the `frontend` folder:

```dockerfile
FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

RUN npm run build

EXPOSE 3000

CMD ["npm", "run", "start"]
```

For the frontend, I added a build step since it's using Next.js.


##  Step 2: Running Containers Manually

Before using Docker Compose, I tested each service separately.

###  Backend

```bash
cd backend
docker build -t book-review-backend .
docker run -d \
  --name backend_container \
  -p 3001:3001 \
  --env-file .env \
  book-review-backend
```

Check logs:

```bash
docker logs backend_container
```

Quick test:

```bash
curl http://localhost:3001/api/books
```

Expected:

```json
[]
```


###  Frontend

```bash
cd ../frontend
docker build -t book-review-frontend .
docker run -d \
  --name frontend_container \
  -p 3000:3000 \
  --env NEXT_PUBLIC_API_URL=http://localhost:3001/api \
  book-review-frontend
```

Check logs:

```bash
docker logs frontend_container
```

Open in browser:

```
http://localhost:3000
```


##  Step 3: Using Docker Compose 

Running containers one by one is fine for testing, but not practical.

So I created a `docker-compose.yml` in the root:

```yaml
version: "3.8"

services:
  backend:
    build: ./backend
    container_name: backend_container
    restart: always
    ports:
      - "3001:3001"
    environment:
      - DB_HOST=mysql
      - DB_USER=root
      - DB_PASS=my-secret-pw
      - DB_NAME=book_review_db
      - PORT=3001
      - JWT_SECRET=mysecretkey
      - ALLOWED_ORIGINS=http://localhost:3000
    depends_on:
      - mysql

  frontend:
    build: ./frontend
    container_name: frontend_container
    restart: always
    depends_on:
      - backend
    environment:
      - NEXT_PUBLIC_API_URL=http://backend:3001/api
    ports:
      - "3000:3000"

  mysql:
    image: mysql:8
    container_name: mysql_container
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=my-secret-pw
      - MYSQL_DATABASE=book_review_db
    ports:
      - "3306:3306"
    volumes:
      - mysql_data:/var/lib/mysql

volumes:
  mysql_data:
```

This sets up:

* Backend
* Frontend
* MySQL database
  all connected together


##  Step 4: Run Everything

```bash
docker-compose up --build
```

Check running containers:

```bash
docker ps
```


##  Step 5: Testing

###  Backend

```bash
curl http://localhost:3001/api/books
```

###  Frontend

Open:

```
http://localhost:3000
```

If everything is working, the frontend should load and call the backend correctly.


##  Step 6: Cleanup

Stop everything:

```bash
docker-compose down
```

Remove everything (including volumes):

```bash
docker-compose down -v
```
