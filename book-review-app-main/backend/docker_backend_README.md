# **Backend - Book Review API**
This backend is built with **Node.js, Express, and MySQL** and handles authentication, books, and reviews. It can run locally or inside Docker.


## **1. Update `.env` File**
Before starting the server, I configured the environment variables inside a `.env` file:

```sh
touch .env
vim .env
```

```env
DB_HOST=localhost
DB_NAME=book_review_db
DB_USER=root
DB_PASS=my-secret-pw
DB_DIALECT=mysql

PORT=3001

JWT_SECRET=mysecretkey

ALLOWED_ORIGINS=http://4.213.140.155:3000,http://localhost:3000
```

## **2. Dockerizing the Backend API**
Inside the backend folder, I added a Dockerfile to containerize the API:
### **Step 1: Create `Dockerfile` for Backend**
```sh
cd ~/book-review-app/backend
touch Dockerfile
vim Dockerfile
```

```Dockerfile
FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3001

ENV NODE_ENV=production

CMD ["node", "src/server.js"]
```

---

### **Step 2: Create `.dockerignore`**
To optimize the build process, create `.dockerignore`:
```sh
touch .dockerignore
nano .dockerignore
```

```
node_modules
npm-debug.log
.env
```

---

### **Step 3: Build Docker Image**
```sh
docker build -t book-review-backend .
```

Verify the image was created:
```sh
docker images
```


---

### **Step 4: Run Backend Container**
Now, I will run the backend container and **connect it to MySQL**:
```sh
docker run -d --name backend-container -p 3001:3001 --env-file .env --network host book-review-backend
```

### **Explanation**
- `-d`: Runs the container in **detached mode**.
- `--name backend-container`: Names the container.
- `-p 3001:3001`: Maps port **3001** inside Docker to **3001** on the host.
- `--env-file .env`: Passes environment variables.

---

### **Step 5: Verify Backend is Running**
Check the running containers:
```sh
docker ps
```


Check logs:
```sh
docker logs backend-container
```

If the backend is running, it should print:
```
Database connected successfully!
Server running on port 3001
```

---

## **6. Test Backend API using `curl`**

### **Health Check**
```sh
curl -X GET http://localhost:3001/
```
Expected output:
```
Book Review API is running...
```

### **Fetch Books**
```sh
curl -X GET http://localhost:3001/api/books
```

### **Register a User**
```sh
curl -X POST http://localhost:3001/api/users/register \
     -H "Content-Type: application/json" \
     -d '{"name": "Test User", "email": "test@example.com", "password": "test123"}'
```

### **Login User**
```sh
curl -X POST http://localhost:3001/api/users/login \
     -H "Content-Type: application/json" \
     -d '{"email": "test@example.com", "password": "test123"}'
```

### **Add a Book**
```sh
curl -X POST http://localhost:3001/api/books \
     -H "Content-Type: application/json" \
     -d '{"title": "New Book", "author": "John Doe", "rating": 4.5}'
```

---

## **7. Restart & Stop Containers**
To restart the backend:
```sh
docker restart backend-container
```

To stop the container:
```sh
docker stop backend-container
```

To remove the container:
```sh
docker rm backend-container
```
