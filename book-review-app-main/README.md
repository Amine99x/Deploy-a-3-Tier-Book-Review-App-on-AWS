# Book Review App

## Overview

**Book Review App** is a full-stack **three-tier web application** that lets users browse books, read existing reviews, and add their own. The project is built with a clear separation between frontend, backend, and database, making it suitable for practicing real-world DevOps and cloud deployment workflows.

- **Unauthenticated users** can view book details and existing reviews.
- **Authenticated users** can register, log in, and submit reviews.


## Architecture

- **Frontend**: Built using **Next.js**, providing server-side rendering and dynamic routing.
- **Backend**: Powered by **Node.js** and **Express.js**, handling authentication, book data, and reviews.
- **Database**: Uses **MySQL** with Sequelize ORM.
  
This three-tier architecture can be independently deployed, making it ideal for containerization, cloud hosting, and CI/CD implementation.

![Two-tiered-Web-application-architecture](https://github.com/Amine99x/Deploy-a-3-Tier-Book-Review-App-on-AWS/tree/main/book-review-app-main/im.png)



#To make it sound more **natural, less AI-like, and more compact**, the key is to:

* remove repetitive structure (“User authentication / Book management…”)
* merge similar ideas
* use short, human phrasing instead of bullet templates

##  Features

* Users can **register**, **log in**, and securely **authenticate** using **JWT**
* Browse books and view detailed information
* Read **reviews** and **post feedback** when logged in
* Each review includes rating, author, and timestamp
* Frontend communicates with the backend through **REST APIs**, with global auth state handled via **React Context**


##  Tech Stack

**Frontend:** Next.js, Tailwind CSS, Axios, React Context API

**Backend:** Node.js, Express.js, MySQL (Sequelize), JWT, bcrypt, CORS

## Application Structure

```
/book-review-app
 ├── /frontend   # Next.js frontend
 ├── /backend    # Node.js & Express backend
 └── README.md   # Project overview
```



## Frontend Directory Layout

```
/frontend
 ├── /src
 │   ├── /app
 │   │   ├── page.js          # Home page (list of books)
 │   │   ├── /book/[id]       # Dynamic route for book details
 │   │   ├── /login           # Login page
 │   │   ├── /register        # Register page
 │   ├── /components          # Reusable UI components 
 │   ├── /context             # React Context for auth state
 │   ├── /services            # Axios API functions
 │   ├── /styles              # Tailwind global styles
 ├── next.config.js           # Next.js config
 ├── package.json             # Dependencies and scripts
 └── README.md                # Frontend-specific docs
```


## Backend Directory Layout

```
/backend
 ├── /src
 │   ├── /config              # Database config 
 │   ├── /models              # Sequelize models
 │   ├── /routes              # Express route handlers
 │   ├── /controllers         # API business logic
 │   ├── /middleware          # JWT auth middleware
 │   └── server.js            # Entry point of the backend server
 ├── package.json             # Dependencies and scripts
 └── README.md                # Backend-specific docs
```



## Setup Instructions

Setup steps for both frontend and backend are provided in their respective folders:

- [`/frontend/README.md`](./frontend/README.md)
- [`/backend/README.md`](./backend/README.md)
