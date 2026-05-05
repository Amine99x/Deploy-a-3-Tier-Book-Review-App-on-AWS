
# Book Review App - Backend

Hi ! 

This is the backend for my **Book Review App**. I took an existing project and enhanced it quite a bit to make it more production-ready. As a **DevOps Engineer**, my focus wasn't on writing the core source code from scratch, but rather on improving the architecture, adding better deployment options, security hardening, Docker support, and making the whole setup smoother to run and maintain.

I added several improvements that make this project easier to deploy, more secure, and better organized. I'm really happy with how it turned out!


## Prerequisites

Before jumping in, make sure you have these ready:

- **Node.js** (v18 or higher)  
  [Download here](https://nodejs.org/)

- **MySQL** (or MariaDB)  
  I personally use MySQL, but MariaDB works great too.

- **Postman** (highly recommended for testing the APIs)

Quick checks:
```sh
node -v
mysql --version
```


## Step 1: Getting the Project

```sh
git clone https://github.com/Amine99x/book-review-app.git
cd book-review-app/backend
```


---

## Step 2: Install Dependencies

```sh
npm install
```

This will pull in everything: Express, Sequelize, bcrypt for secure passwords, JWT for auth, dotenv, cors, etc. I tried to keep the dependencies clean but solid.


## Step 3: Setup Environment Variables

Create a `.env` file in the `backend` folder:

```sh
touch .env
```

Then add this (update with your own values):

```env
# Database
DB_HOST=localhost
DB_USER=root
DB_PASS=yourpassword
DB_NAME=book_review_db
DB_DIALECT=mysql

# Security
JWT_SECRET=super_long_and_random_secret_key_here
```

## Step 4: Database Setup

Start MySQL and create the database:

```sql
CREATE DATABASE book_review_db;
```

I made the app use Sequelize to auto-create the tables on first run, which saved me a ton of manual work. It also seeds some sample books, users, and reviews so you can start testing right away.

For CORS (if your frontend is on another port):

```env
ALLOWED_ORIGINS=http://localhost:3000,https://your-frontend-domain.com
```

## Step 5: Run the Server

```sh
node src/server.js
```



## Testing the APIs

Here are the main endpoints I implemented:

### Register a new user
```sh
curl -X POST http://localhost:3001/api/users/register \
  -H "Content-Type: application/json" \
  -d '{"name": "Alex Rivera", "email": "alex@example.com", "password": "strongpass123"}'
```

### Login
```sh
curl -X POST http://localhost:3001/api/users/login \
  -H "Content-Type: application/json" \
  -d '{"email": "alex@example.com", "password": "strongpass123"}'
```

You'll get a JWT token back — keep it safe, you'll need it for protected routes.

### Get all books
```sh
curl -X GET http://localhost:3001/api/books
```

### Add a review (protected)
```sh
curl -X POST http://localhost:3001/api/reviews \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -H "Content-Type: application/json" \
  -d '{"bookId": 1, "comment": "Absolutely loved this book! Changed how I think about coding.", "rating": 5}'
```


## Project Structure

```
/backend
 ├── src/
 │   ├── config/          # DB connection
 │   ├── models/          # User, Book, Review models
 │   ├── controllers/     # Main logic
 │   ├── routes/          # API routes
 │   ├── middleware/      # JWT auth
 │   └── server.js
 ├── package.json
 └── .env (don't commit this!)
```

I tried to keep it clean and organized. Separation of concerns helped a lot when I started adding more features.

---

## Deployment

I tested it with PM2 for production and also Docker. Both work well.

**With PM2:**
```sh
npm install -g pm2
pm2 start src/server.js --name "book-review-backend"
```

**With Docker:**
I included a basic Dockerfile. Just build and run.

For cloud deployment, I recommend putting MySQL on RDS or Azure and setting proper environment variables.

## Challenges I faced (and learned from)

- Getting JWT + protected routes right took some debugging
- CORS gave me headaches at first (classic)
- Balancing clean code with performance

But honestly, seeing the whole thing work end-to-end made it worth it.
