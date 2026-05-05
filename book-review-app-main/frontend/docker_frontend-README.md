# Dockerizing the Next.js Frontend (Without Nginx)

Hey! 

I spent some time figuring out how to properly Dockerize the **Next.js frontend** without using Nginx. Here's the cleanest and simplest way I found that actually works well.

This guide uses a **multi-stage build** so the final image is lighter, and I made sure the `.env` variables are handled correctly for both local and production.



## 1. Prepare the Environment Variable

In the `frontend` folder, create (or update) a `.env` file:

```env
NEXT_PUBLIC_API_URL=http://YOUR_BACKEND_IP:3001
```

**Important:**
- For local development → use `http://localhost:3001`
- For production / server → put your backend server's actual IP or domain.



## 2. Create the Dockerfile

Create a file named `Dockerfile` in the `frontend` folder:

```dockerfile
# Build stage
FROM node:18 AS builder
WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

# Pass environment variable during build
ARG NEXT_PUBLIC_API_URL
ENV NEXT_PUBLIC_API_URL=${NEXT_PUBLIC_API_URL}

RUN npm run build

# Production stage
FROM node:18 AS runner
WORKDIR /app

COPY --from=builder /app/.next ./.next
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/public ./public
COPY --from=builder /app/package.json ./package.json

ENV NODE_ENV=production
ENV PORT=3000

EXPOSE 3000

CMD ["npm", "run", "start"]
```



## 3. Build the Image

Run this command inside the `frontend` folder:

```sh
docker build --build-arg NEXT_PUBLIC_API_URL=http://YOUR_BACKEND_IP:3001 -t book-review-frontend .
```



## 4. Run the Container

```sh
docker run -d \
  --name frontend-container \
  -p 3000:3000 \
  -e NEXT_PUBLIC_API_URL=http://YOUR_BACKEND_IP:3001 \
  book-review-frontend
```



## 5. Check if it's Working

```sh
docker ps
```

You should see the container running. Then open your browser:

- Local: `http://localhost:3000`
- Remote server: `http://YOUR_SERVER_IP:3000`



## 6. Useful Commands

**Stop the container:**
```sh
docker stop frontend-container
```

**Remove it:**
```sh
docker rm frontend-container
```

**Rebuild after making changes:**
```sh
docker build --build-arg NEXT_PUBLIC_API_URL=http://YOUR_BACKEND_IP:3001 -t book-review-frontend .
docker run -d -p 3000:3000 --name frontend-container -e NEXT_PUBLIC_API_URL=http://YOUR_BACKEND_IP:3001 book-review-frontend
```



## 7. Update .gitignore

Make sure you add these lines so you don’t push unnecessary stuff:

```gitignore
node_modules
.next
.env
.env.local
.env.production
```



## Final Thoughts

This setup is way cleaner than using Nginx for a simple Next.js app. I like it because it's straightforward and the image stays relatively small. It took me a few tries to get the environment variables right during build time, but now it works smoothly.

If you're deploying on a VPS **(AWS)**, this should make your life much easier.

Let me know if you run into any issues — happy to help!

