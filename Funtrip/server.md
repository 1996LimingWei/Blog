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

---

# What is `axios`?

- **`axios`** is a **client-side** library (used in frontend React code).
- It is used to **send HTTP requests** (like GET, POST, etc.) from your browser (frontend) to a server (backend).
- Example:
  ```js
  axios.post('/download-song', { song: 'song name' });
  ```
  This sends a POST request to the backend.

---

## What is `app` (in Express)?

- **`app`** is our **server-side** Express application (used in our backend Node.js code).
- **`app.post`** defines a **route handler**: it tells the server what to do when it receives a POST request at a certain URL.
- Example:
  ```js
  app.post('/download-song', (req, res) => {
    // handle the request here
  });
  ```
  This listens for POST requests at `/download-song`.

---

- **`axios.post`** (frontend) **sends** a POST request to the server.
- **`app.post`** (backend) **receives** that POST request and handles it.

**They are two sides of the same conversation:**
- `axios.post` = "Hey server, here’s some data, please do something!"
- `app.post` = "Okay, I got your request, here’s my response!"

---

| Action                        | HTTP Method | Why?                                 |
|-------------------------------|-------------|--------------------------------------|
| Get info (host, hostId, etc.) | GET         | Just reading data                    |
| Download/search song          | POST        | Sending data, triggering an action   |
| Send auth code to Spotify     | POST        | Sending data, triggering an action   |

---

# What is `socket`?

| On the Client (Frontend)         | On the Server (Backend)         |
|----------------------------------|---------------------------------|
| `socket.emit('event', data)`     | `socket.on('event', callback)`  |
| Sends an event to the server     | Listens for the event and runs code |

- socket.on is used to **listen for events** sent from the client (frontend) to the server (backend).
- socket.emit is used to **send an event** (with optional data) from the client to the server, or from the server to the client.

---

## How does it work?

- **Socket.IO** enables real-time, two-way communication between the frontend and backend using WebSockets (or fallback methods).
- On the **server**, you use `socket.on('eventName', callback)` to listen for a specific event sent by the client.
- When the client emits that event, the server runs the callback function.

---

## Example in Code

```js
socket.on('joinRoom', ({roomId, username, action}) => {
    // This code runs when the client emits a 'joinRoom' event
    // with the data {roomId, username, action}
});
```

- **'joinRoom'** is the name of the event.
- When a user tries to create or join a room from the frontend, the frontend emits a `'joinRoom'` event with the necessary data.
- The second argument is a **callback function** that receives the data sent from the client.
- The backend listens for this event using `socket.on('joinRoom', ...)`.
- When the event is received, the backend runs the code inside the callback to handle room creation or joining.

---

## How does the client send the event?

On the frontend Home.tsx, we see:
<img width="618" height="317" alt="截屏2025-07-28 16 58 01" src="https://github.com/user-attachments/assets/73624717-bcc0-41d8-a292-7751f0bd28df" />

- This sends the `'joinRoom'` event and data to the server.

---

# What is `socket.join(roomId)`?

- It **adds the current socket (user)** to the room with the given `roomId`.

**Example:**
```js
socket.join('myRoom123');
```
- Now, this socket is part of the room called `'myRoom123'`.

---

## What is a "room" in Socket.IO?

- A **room** is a way to group sockets (users) together.
- You can send messages/events to **all sockets in a specific room**.
- Rooms are identified by a string (room ID).
- 
---


## Example in Your Project

When a user joins a room:
```js
socket.join(roomId);
```
- The user is now part of that room.
- You can now send updates (like song changes, chat messages, etc.) to everyone in that room using:
  ```js
  io.to(roomId).emit('someEvent', data);
  ```

---

