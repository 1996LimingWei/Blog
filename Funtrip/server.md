# Server

server/src/server.ts is the main entry point for the backend server. 

We import some js packages to our project:
```
import express from 'express';
import * as dotenv from 'dotenv';
dotenv.config();
import cors from 'cors';
```
---

### 1. **Express**
- **What:** A library for building web servers and APIs in Node.js.
- **Why:** Makes it easy to handle HTTP requests (like GET, POST) and set up routes (URLs).
- **How do we know to import it?**  
  - You need Express if you want to create a web server or API in Node.js.
  - It’s listed as a dependency in your `package.json` (under `dependencies`).
  - You install it with:  
    ```
    npm install express
    ```

---

### 2. **dotenv**
- **What:** A library to load environment variables from a `.env` file into `process.env`.
- **Why:** Keeps sensitive info (like API keys, secrets, or config) out of your code.
- **How do we know to import it?**  
  - If your project uses a `.env` file for configuration, you need this.
  - It’s also listed in `package.json`.
  - Install with:  
    ```
    npm install dotenv
    ```

---

### 3. **cors**
- **What:** Middleware for Express to enable Cross-Origin Resource Sharing.
- **Why:** Allows your frontend (which may run on a different port or domain) to make requests to your backend.
- **How do we know to import it?**  
  - If your frontend and backend are on different origins (e.g., localhost:3000 and localhost:3001), you need CORS.
  - Install with:  
    ```
    npm install cors
    ```

---

### 4. **http**
- **What:** A built-in Node.js module for creating HTTP servers.
- **Why:** Sometimes you need more control over the server (e.g., for WebSockets).
- **How do we know to import it?**  
  - If you want to use WebSockets (like with `socket.io`), you often need to create a raw HTTP server.
  - No need to install—comes with Node.js.

---

## What is a "Route"?

A **route** in a web server is a rule that tells the server how to respond to a specific URL (path) and HTTP method (GET, POST, etc.).

**Example:**  
If you have this code:
```js
app.get('/hello', (req, res) => {
  res.send('Hello, world!');
});
```
- This is a **route**.
- It means: “When someone visits `/hello` with a GET request, send back ‘Hello, world!’”

Routes can handle different HTTP methods:
- `GET` (fetch data)
- `POST` (send data)
- `PUT` (update data)
- `DELETE` (remove data)

---

## What Does "Mount All Routes" Mean?

**Mounting** routes means attaching a group of routes (usually defined in another file) to a specific path in your main server.

### Example

Suppose you have a file called `spotify.ts` that looks like this:
```js
import express from 'express';
const router = express.Router();

router.post('/login', ...);
router.post('/refresh', ...);

export default router;
```
- Here, `router` is a mini Express app that contains several routes, like `/login` and `/refresh`.

In your main server file:
```js
import spotifyRoutes from './routes/spotify';
app.use('/api/spotify', spotifyRoutes);
```
- This **mounts** all the routes from `spotifyRoutes` under `/api/spotify`.

**What does this mean?**
- If you have a route `/login` in `spotifyRoutes`, it becomes accessible at `/api/spotify/login`.
- If you have `/refresh`, it becomes `/api/spotify/refresh`.

**Why do this?**
- **Organization:** Keeps your code clean by grouping related routes together.
- **Scalability:** Makes it easy to add more features without cluttering your main server file.

---

## Visual Example

| Route in `spotifyRoutes` | Full URL after mounting         |
|--------------------------|---------------------------------|
| `/login`                 | `/api/spotify/login`            |
| `/refresh`               | `/api/spotify/refresh`          |

---
