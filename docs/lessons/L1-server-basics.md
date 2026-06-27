# Lesson Recap: What Just Happened

## The big picture: how a website "shows up" on your screen
# Lesson Recap: What Just Happened

## The big picture: how a website "shows up" on your screen

1. You type `http://localhost:3000` in your browser
2. Your browser sends a **request** ("hey, give me something to show")
3. Your **server** (the Node.js program we wrote) receives that request
4. The server runs some code to decide what to send back
5. The server sends a **response** (in our case, just text)
6. Your browser displays that response on the screen

That's it — a website is really just two programs talking to each other: a **browser** (asks for things) and a **server** (answers).

## Breaking down `server.js` line by line

```javascript
const express = require('express');
```
- We're loading a tool called Express
- Express handles the boring/repetitive parts of being a server, so we don't write that from scratch

```javascript
const app = express();
```
- This creates "our server" using that tool — think of `app` as the server itself

```javascript
const PORT = 3000;
```
- Just a number we picked — the "door" our server listens at

```javascript
app.get('/', (req, res) => {
  res.send('Hello! Your server is working.');
});
```
- This says: **"when someone visits the homepage (`/`), run this code"**
- `req` = the incoming request (what the visitor is asking for)
- `res` = the response (what we send back)
- `res.send(...)` = actually sends text back to the browser

```javascript
app.listen(PORT, () => { ... });
```
- This **starts** the server and tells it to keep running, waiting for visitors, on port 3000

## Key vocabulary learned today

| Word | Simple meaning |
|---|---|
| Server | A program that waits for requests and sends back answers |
| Request | What the browser asks for |
| Response | What the server sends back |
| Port | A specific "door" number the server listens on |
| localhost | "My own computer" (not the internet) |
| Package | A reusable piece of code someone else wrote (like Express) |
| `package.json` | A list of which packages your project uses |

You now understand the full round trip: **browser asks → server answers → browser shows it**. Everything we build next (database, AI) just adds more steps *inside* that same loop.
