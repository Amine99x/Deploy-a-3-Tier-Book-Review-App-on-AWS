
# Book Review App - Frontend

Hi !

This is the **frontend** of my Book Review App, built with **Next.js 14** and **Tailwind CSS**. It’s clean, responsive, and feels really nice to use. You can browse books, read reviews, register, login, and submit your own reviews once you're logged in.

I took an existing project and spent time improving the UI/UX, fixing some bugs, making the code cleaner, and adding better responsiveness. As a DevOps Engineer, I focused more on making both frontend and backend work well together, especially around deployment and environment setup. I'm pretty happy with how smooth it feels now!

---

## Prerequisites

- **Node.js** (v18 or higher)
- Backend should be running on `http://localhost:3001` (check the backend README if you haven't set it up yet)

---

## Quick Start

```sh
git clone https://github.com/YOUR-USERNAME/book-review-app.git
cd book-review-app/frontend
```

```sh
npm install
```

Create the environment file:

```sh
cp .env.example .env.local
```

Update `.env.local` with your backend URL:

```env
NEXT_PUBLIC_API_URL=http://localhost:3001
```

Then start the app:

```sh
npm run dev
```

Open your browser and go to **http://localhost:3000**. You should see the homepage with books loaded.

---

## What I Improved

- Better UI design and mobile responsiveness
- Smoother login/register flow with good feedback
- Improved state management with React Context
- Cleaner component structure
- Better error handling and loading states
- Made it easier to deploy


## Folder Structure

```sh
/frontend
 ├── src/
 │   ├── app/
 │   │   ├── page.js                 # Homepage - Book list
 │   │   ├── book/[id]/page.js       # Book details + reviews
 │   │   ├── login/page.js
 │   │   └── register/page.js
 │   ├── components/                 # Navbar, BookCard, ReviewForm...
 │   ├── context/                    # UserContext for auth
 │   ├── services/                   # API calls with Axios
 │   └── styles/
 ├── .env.local
 ├── next.config.js
 └── package.json
```


## How to Test It

- **Homepage**: Browse all books
- **Register/Login**: Create account and sign in
- **Book Details**: Click any book to see info and reviews
- **Submit Review**: Only visible when logged in — it updates instantly

The app stores your JWT in localStorage and shows your name in the navbar after login. It feels pretty natural.


## Deployment

I recommend deploying on **Vercel** — it’s super fast for Next.js apps.

```sh
npm install -g vercel
vercel
```

Don’t forget to add your `NEXT_PUBLIC_API_URL` in Vercel’s environment variables (pointing to your deployed backend).



## Troubleshooting

- **API not working?** → Make sure the backend is running and `NEXT_PUBLIC_API_URL` is correct.
- **Login issues?** → Check browser console and localStorage (F12 → Application).
- **CORS errors?** → Go back to backend and check allowed origins.

--
## Final Thoughts

This was a fun project for me. Even though I didn’t write the original code, I put real effort into making the frontend look good and work reliably with the backend. It taught me a lot about full-stack integration from a DevOps perspective.

Feel free to fork it, improve it, or use it as a base for your own ideas. Any feedback or suggestions are very welcome!

Happy reading and coding! 