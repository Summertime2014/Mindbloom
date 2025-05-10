index.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>MindBloom</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f1f4f8;
      color: #333;
      margin: 0;
      padding: 20px;
    }
    h1 { text-align: center; }
    .section { margin: 20px 0; }
    label, textarea, select, button { display: block; width: 100%; margin: 10px 0; }
    textarea { height: 100px; }
    .entry { background: #fff; padding: 10px; margin: 10px 0; border-radius: 6px; }
    .mood-emoji { font-size: 24px; }
    button { padding: 10px; background: #0066cc; color: white; border: none; border-radius: 5px; cursor: pointer; }
    button:hover { background: #004999; }
  </style>
</head>
<body>
  <h1>MindBloom</h1>

  <div class="section">
    <h2>How are you feeling?</h2>
    <select id="moodSelect">
      <option value="😀">Happy</option>
      <option value="😐">Okay</option>
      <option value="😢">Sad</option>
      <option value="😠">Angry</option>
      <option value="😰">Anxious</option>
    </select>
    <button onclick="saveMood()">Save Mood</button>
  </div>

  <div class="section">
    <h2>Journal</h2>
    <textarea id="journalText" placeholder="Write your thoughts..."></textarea>
    <button onclick="saveJournal()">Save Entry</button>
    <div id="journalEntries"></div>
  </div>

  <div class="section">
    <h2>Quick Calm</h2>
    <p>Try this simple breathing pattern:</p>
    <p>Inhale 4s - Hold 4s - Exhale 4s - Hold 4s</p>
    <button onclick="startBreathing()">Start Breathing Timer</button>
    <p id="breathTimer"></p>
  </div>

  <script>
    const journalKey = 'mindbloom_journal';
    const moodKey = 'mindbloom_mood';

    function saveMood() {
      const mood = document.getElementById('moodSelect').value;
      const moodData = { mood, date: new Date().toLocaleString() };
      localStorage.setItem(moodKey, JSON.stringify(moodData));
      alert("Mood saved: " + mood);
    }

    function saveJournal() {
      const text = document.getElementById('journalText').value.trim();
      if (!text) return alert("Please write something.");
      const entries = JSON.parse(localStorage.getItem(journalKey)) || [];
      entries.unshift({ text, date: new Date().toLocaleString() });
      localStorage.setItem(journalKey, JSON.stringify(entries));
      document.getElementById('journalText').value = '';
      loadJournal();
    }

    function loadJournal() {
      const entries = JSON.parse(localStorage.getItem(journalKey)) || [];
      const container = document.getElementById('journalEntries');
      container.innerHTML = '';
      entries.forEach(e => {
        container.innerHTML += `<div class="entry"><strong>${e.date}</strong><p>${e.text}</p></div>`;
      });
    }

    function startBreathing() {
      const times = ["Inhale...", "Hold...", "Exhale...", "Hold..."];
      let i = 0;
      const timer = document.getElementById('breathTimer');
      timer.textContent = times[i];
      const interval = setInterval(() => {
        i = (i + 1) % times.length;
        timer.textContent = times[i];
      }, 4000);
      setTimeout(() => clearInterval(interval), 16000);
    }

    loadJournal();
  </script>
</body>
</html>
