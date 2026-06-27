
## Quick recap
- Your server.js connected to PostgreSQL using the secret info in `.env`
- It asked the database a simple question ("what time is it?")
- The database answered back, proving the connection works

You now have all 3 pieces alive: **browser ↔ server ↔ database**.

## What's next: designing your actual data

Now we need to decide what *information* we're going to store. Two main things, in plain terms:

- **Businesses** — who is searching (name, email, what kind of influencer they want)
- **Influencers** — the YouTube channels we'll pull in (name, niche, subscriber count, etc.)

This is where your SQL knowledge actually kicks in — we'll design **tables** (like spreadsheets) for these.

Let's connect your server to the database, step by step.

## Step 1: Create a database in pgAdmin
1. Open **pgAdmin**, expand **Servers** → your PostgreSQL server
2. Right-click **Databases** → **Create** → **Database...**
3. Name it: `influencer_finder`
4. Click **Save**

## Step 2: Create a `.env` file (your secret info storage)
In VS Code, create a new file named exactly `.env` (no extension) in your project root.

Paste this inside, replacing `YOUR_PASSWORD` with the password you set during PostgreSQL install:

```
PORT=3000
DATABASE_URL=postgresql://postgres:YOUR_PASSWORD@localhost:5432/influencer_finder
```

**What this line means (plain words):**
- `postgres` → the default username PostgreSQL creates
- `YOUR_PASSWORD` → your database password
- `localhost:5432` → "my computer, door 5432" (PostgreSQL's default port)
- `influencer_finder` → the database name we just created

## Step 3: Update `server.js` to connect to the database

Replace your current `server.js` content with this:

```javascript
require('dotenv').config();
const express = require('express');
const { Pool } = require('pg');

const app = express();
const PORT = process.env.PORT || 3000;

// This sets up the connection to your database
const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
});

app.get('/', (req, res) => {
  res.send('Hello! Your server is working.');
});

// New: a test route to check the database connection
app.get('/test-db', async (req, res) => {
  try {
    const result = await pool.query('SELECT NOW()');
    res.send(`Database connected! Time: ${result.rows[0].now}`);
  } catch (err) {
    res.status(500).send('Database connection failed: ' + err.message);
  }
});

app.listen(PORT, () => {
  console.log(`Server running at http://localhost:${PORT}`);
});
```

**What's new here (plain words):**
- `require('dotenv').config()` → loads your `.env` secrets so the code can use them
- `new Pool(...)` → opens a "connection line" to PostgreSQL, reusable for many queries
- `/test-db` → a new page just for testing — it asks the database "what time is it?" (a simple way to confirm the connection works)

## Step 4: Run it
Stop your server if it's running (click in the terminal, press `Ctrl + C`), then restart:
```
node server.js
```

Visit in your browser:
```
http://localhost:3000/test-db
```

You should see something like: `Database connected! Time: 2026-06-27...`
