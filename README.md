mood-tracker/
├── backend/
│   ├── server.js
│   ├── package.json
├── frontend/
│   ├── index.html
│   ├── style.css
│   ├── script.js
├── db/
│   └── database.sql
└── README.md
const express = require('express');
const cors = require('cors');
const bodyParser = require('body-parser');

const app = express();
const PORT = 3000;

app.use(cors());
app.use(bodyParser.json());

let moods = []; // Temporary in-memory storage for moods

// Get all moods
app.get('/api/moods', (req, res) => {
    res.json(moods);
});

// Add a new mood
app.post('/api/moods', (req, res) => {
    const { mood, date, notes } = req.body;
    if (mood && date) {
        moods.push({ mood, date, notes });
        res.status(201).json({ message: 'Mood added successfully' });
    } else {
        res.status(400).json({ error: 'Mood and date are required' });
    }
});

app.listen(PORT, () => {
    console.log(`Server running on http://localhost:${PORT}`);
});
{
  "name": "mood-tracker-backend",
  "version": "1.0.0",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "cors": "^2.8.5",
    "body-parser": "^1.20.2"
  }
}

cd backend
npm install
npm start
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mood Tracker</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>Mood Tracker</h1>
        <form id="mood-form">
            <label for="mood">Mood:</label>
            <select id="mood" required>
                <option value="">Select...</option>
                <option value="Happy">Happy</option>
                <option value="Sad">Sad</option>
                <option value="Anxious">Anxious</option>
                <option value="Neutral">Neutral</option>
            </select>
            <label for="date">Date:</label>
            <input type="date" id="date" required>
            <label for="notes">Notes:</label>
            <textarea id="notes" placeholder="Optional"></textarea>
            <button type="submit">Log Mood</button>
        </form>
        <h2>Mood Log</h2>
        <div id="mood-log"></div>
    </div>
    <script src="script.js"></script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-color: #f4f4f4;
    color: #333;
}

.container {
    max-width: 600px;
    margin: 2rem auto;
    padding: 2rem;
    background: #fff;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

h1, h2 {
    text-align: center;
}

form {
    display: flex;
    flex-direction: column;
}

label {
    margin: 0.5rem 0 0.2rem;
}

input, select, textarea, button {
    margin-bottom: 1rem;
    padding: 0.8rem;
    border: 1px solid #ccc;
    border-radius: 4px;
}

button {
    background-color: #5cb85c;
    color: white;
    border: none;
    cursor: pointer;
}

button:hover {
    background-color: #4cae4c;
}

#mood-log {
    margin-top: 1rem;
}
const form = document.getElementById('mood-form');
const moodLog = document.getElementById('mood-log');

const API_URL = 'http://localhost:3000/api/moods';

// Load moods
async function loadMoods() {
    const response = await fetch(API_URL);
    const moods = await response.json();
    moodLog.innerHTML = moods
        .map((mood) => `
            <div>
                <strong>${mood.date}</strong>: ${mood.mood}
                ${mood.notes ? `<p>${mood.notes}</p>` : ''}
            </div>
        `)
        .join('');
}

// Submit mood
form.addEventListener('submit', async (e) => {
    e.preventDefault();
    const mood = document.getElementById('mood').value;
    const date = document.getElementById('date').value;
    const notes = document.getElementById('notes').value;

    await fetch(API_URL, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ mood, date, notes }),
    });

    form.reset();
    loadMoods();
});

// Initial load
loadMoods();
