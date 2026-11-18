# Backend REST API Guide

**Author:**   
**Project type:** Backend REST API practice project

This project provides a simple introduction to building a backend REST API. It covers concepts discussed in freeCodeCamp's **Intro to Backend Web Development** tutorial and is written as a separate implementation with Node.js, Express, MongoDB, Mongoose, bcrypt and JSON Web Tokens.

## Key Features

- Register new users while securely hashing passwords with bcrypt
- Authenticate users through JWT-based login
- Retrieve the signed-in user's details and return a logout confirmation
- Perform create, read, update and delete (CRUD) operations on posts
- Restrict post editing and deletion to the user who created the post
- Connect to MongoDB using values stored in environment variables
- Organize the application into config, model, controller, middleware and route folders

## Prerequisites

Make sure the following tools and accounts are available:

1. **Node.js LTS** — https://nodejs.org/
2. **MongoDB Atlas account** — https://www.mongodb.com/atlas/database (a locally installed MongoDB server also works)
3. **Postman** — https://www.postman.com/downloads/ to send and inspect API requests
4. **Git** — https://git-scm.com/downloads
5. **GitHub account** — https://github.com/

## Installation and Setup

1. Download the project files and extract the archive.
2. Open the extracted `backend-api` directory in VS Code.
3. Launch the integrated terminal while inside that directory.
4. Install the required packages:

   ```bash
   npm install
   ```

5. Duplicate `.env.example` and rename the new file to `.env`.
6. In `.env`, replace the example MongoDB URI with your Atlas connection string. Also provide a long, randomly generated value for `JWT_SECRET`.
7. Run the application in development mode:

   ```bash
   npm run dev
   ```

8. Open http://localhost:4000 in your browser. A JSON response should appear to indicate that the API has started successfully.

> Keep `.env` out of GitHub because it stores database login details and the JWT secret. The `.gitignore` file is configured to exclude it.

## Configuring MongoDB Atlas

1. Sign up for MongoDB Atlas and create a free cluster.
2. Add a database user with its own username and strong password. These credentials are different from the ones used to sign in to the Atlas website.
3. Open **Network Access** and allow your current IP address. For projects containing real data, do not open access to every address with `0.0.0.0/0`.
4. Choose **Connect → Drivers → Node.js**, then copy the connection URI.
5. Put the URI in `.env`, replacing the example username, password and cluster values. If the password contains special characters, URL-encode them.
6. Include a database name in the URI, for example `app_database`.

## Available API Routes

Base URL: `http://localhost:4000/api`

| Method | Endpoint | Auth required | Description |
|---|---|---:|---|
| POST | `/users/register` | No | Register a new user |
| POST | `/users/login` | No | Sign in and obtain a JWT |
| POST | `/users/logout` | Yes | Send logout confirmation; remove the token on the client |
| GET | `/users/me` | Yes | Retrieve the current user's profile |
| GET | `/posts` | No | Retrieve the complete list of posts |
| GET | `/posts/:id` | No | Retrieve a particular post |
| POST | `/posts` | Yes | Publish a new post |
| PATCH | `/posts/:id` | Yes | Edit a post you created |
| DELETE | `/posts/:id` | Yes | Remove a post you created |

### Sample Request Data

User registration (`POST /api/users/register`):

```json
{
  "name": "",
  "email": "user@example.com",
  "password": "change-this-password"
}
```

User login (`POST /api/users/login`):

```json
{
  "email": "user@example.com",
  "password": "change-this-password"
}
```

Creating a post (`POST /api/posts`):

```json
{
  "title": "My first backend post",
  "content": "I created this post using my own REST API."
}
```

To access routes that require authentication, copy the JWT returned after registration or login and add the following header in Postman:

`Authorization: Bearer YOUR_TOKEN_HERE`

## Testing with Postman

Load `postman/backend-api.postman_collection.json` into Postman. Register a user or log in first, then place the returned JWT in the collection's `token` variable.

## Folder and File Layout

```text
backend-api/
├── backend/
│   └── src/
│       ├── config/
│       │   ├── constants.js
│       │   └── database.js
│       ├── controllers/
│       │   ├── post.controller.js
│       │   └── user.controller.js
│       ├── middleware/
│       │   └── auth.middleware.js
│       ├── models/
│       │   ├── post.model.js
│       │   └── user.model.js
│       ├── routes/
│       │   ├── post.route.js
│       │   └── user.route.js
│       ├── app.js
│       └── index.js
├── postman/
│   └── backend-api.postman_collection.json
├── .env.example
├── .gitignore
├── package.json
└── README.md
```

## Uploading the Project to GitHub

Create a new repository from your GitHub account. You can use `backend-api` as the repository name. No existing GitHub username is built into this project. Once the empty repository has been created, run these commands from the project directory:

```bash
git init
git add .
git status
git commit -m "Initial backend project"
git branch -M main
git remote add origin https://github.com/YOUR-GITHUB-USERNAME/backend-api.git
git push -u origin main
```

Change `REPLACE-WITH-YOUR-USERNAME` to your own GitHub username. This is the only step that depends on your account. Before creating the commit, review the output of `git status` and confirm that `.env` is not included.

## Important Learning Point

This codebase is intended for learning and should not be treated as a production-ready authentication system. Its logout route does not invalidate a JWT on the server. The token stays usable until it expires unless the client removes it or a server-side revocation mechanism is implemented.

Before using an authentication system in a real application, consider adding request rate limits, thorough input checks, HTTPS, secure token storage, automated tests, logging and a suitable token-revocation approach.
