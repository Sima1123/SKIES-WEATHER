# SKIES-WEATHER
AI-Powered Wrather
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SKIES — Weather</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Sans:ital,wght@0,300;0,400;0,500;1,300&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --sky: #0a0e1a;
    --mid: #111827;
    --card: rgba(255,255,255,0.04);
    --border: rgba(255,255,255,0.08);
    --accent: #38bdf8;
    --warm: #fbbf24;
    --text: #e2e8f0;
    --muted: #64748b;
    --rain: #60a5fa;
    --snow: #bfdbfe;
    --sun: #fcd34d;
    --cloud: #94a3b8;
  }

  html, body {
    height: 100%;
    background: var(--sky);
    color: var(--text);
    font-family: 'DM Sans', sans-serif;
    font-weight: 300;
    overflow-x: hidden;
  }

  /* Animated star background */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      radial-gradient(1px 1px at 10% 15%, rgba(255,255,255,0.5) 0%, transparent 100%),
      radial-gradient(1px 1px at 30% 40%, rgba(255,255,255,0.3) 0%, transparent 100%),
      radial-gradient(1px 1px at 55% 20%, rgba(255,255,255,0.4) 0%, transparent 100%),
      radial-gradient(1px 1px at 80% 60%, rgba(255,255,255,0.2) 0%, transparent 100%),
      radial-gradient(1px 1px at 95% 10%, rgba(255,255,255,0.5) 0%, transparent 100%),
      radial-gradient(1px 1px at 70% 80%, rgba(255,255,255,0.3) 0%, transparent 100%),
      radial-gradient(1px 1px at 20% 75%, rgba(255,255,255,0.25) 0%, transparent 100%),
      radial-gradient(1px 1px at 45% 90%, rgba(255,255,255,0.35) 0%, transparent 100%),
      radial-gradient(1.5px 1.5px at 62% 50%, rgba(255,255,255,0.2) 0%, transparent 100%);
    pointer-events: none;
    z-index: 0;
  }

  .app {
    position: relative;
    z-index: 1;
    min-height: 100vh;
    max-width: 480px;
    margin: 0 auto;
    padding: 0 20px 40px;
    display: flex;
    flex-direction: column;
  }

  header {
    padding: 48px 0 32px;
    display: flex;
    align-items: baseline;
    gap: 12px;
  }

  .logo {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 42px;
    letter-spacing: 4px;
    color: #fff;
    line-height: 1;
  }

  .logo-dot {
    width: 8px;
    height: 8px;
    background: var(--accent);
    border-radius: 50%;
    display: inline-block;
    margin-bottom: 4px;
    margin-left: 2px;
    animation: pulse 2s ease-in-out infinite;
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.5; transform: scale(0.8); }
  }

  /* Search */
  .search-wrap {
    position: relative;
    margin-bottom: 32px;
  }

  .search-input {
    width: 100%;
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 16px 52px 16px 20px;
    font-family: 'DM Sans', sans-serif;
    font-size: 15px;
    font-weight: 400;
    color: var(--text);
    outline: none;
    transition: border-color 0.2s, background 0.2s;
    -webkit-appearance: none;
  }

  .search-input::placeholder { color: var(--muted); }

  .search-input:focus {
    border-color: var(--accent);
    background: rgba(56,189,248,0.05);
  }

  .search-btn {
    position: absolute;
    right: 12px;
    top: 50%;
    transform: translateY(-50%);
    background: var(--accent);
    border: none;
    border-radius: 8px;
    width: 34px;
    height: 34px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: background 0.2s, transform 0.1s;
  }
  .search-btn:hover { background: #7dd3fc; }
  .search-btn:active { transform: translateY(-50%) scale(0.95); }
  .search-btn svg { width: 16px; height: 16px; fill: #0a0e1a; }

  /* Main card */
  .main-card {
    background: linear-gradient(135deg, rgba(56,189,248,0.12), rgba(251,191,36,0.05));
    border: 1px solid var(--border);
    border-radius: 24px;
    padding: 32px 28px 28px;
    margin-bottom: 16px;
    position: relative;
    overflow: hidden;
    animation: fadeUp 0.5s ease both;
  }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(16px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  .main-card::before {
    content: '';
    position: absolute;
    top: -60px;
    right: -60px;
    width: 200px;
    height: 200px;
    border-radius: 50%;
    background: radial-gradient(circle, rgba(56,189,248,0.15) 0%, transparent 70%);
    pointer-events: none;
  }

  .location-row {
    display: flex;
    align-items: center;
    gap: 6px;
    margin-bottom: 8px;
  }

  .location-name {
    font-size: 14px;
    font-weight: 500;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 2px;
  }

  .location-icon { width: 14px; height: 14px; stroke: var(--muted); fill: none; }

  .temp-row {
    display: flex;
    align-items: flex-start;
    gap: 20px;
    margin: 12px 0 20px;
  }

  .temp-main {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 96px;
    line-height: 0.9;
    color: #fff;
    letter-spacing: -2px;
  }

  .temp-unit {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 36px;
    color: var(--muted);
    margin-top: 8px;
  }

  .weather-icon-big {
    font-size: 64px;
    margin-left: auto;
    filter: drop-shadow(0 0 20px rgba(251,191,36,0.3));
    animation: float 4s ease-in-out infinite;
  }

  @keyframes float {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-8px); }
  }

  .condition-text {
    font-size: 18px;
    font-weight: 300;
    color: #cbd5e1;
    margin-bottom: 24px;
    font-style: italic;
  }

  .meta-row {
    display: flex;
    gap: 24px;
    padding-top: 20px;
    border-top: 1px solid var(--border);
  }

  .meta-item {
    display: flex;
    flex-direction: column;
    gap: 2px;
  }

  .meta-label {
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 1.5px;
    color: var(--muted);
  }

  .meta-val {
    font-size: 16px;
    font-weight: 500;
    color: var(--text);
  }

  /* Sub-cards row */
  .sub-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
    margin-bottom: 16px;
  }

  .sub-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 18px;
    padding: 20px 18px;
    animation: fadeUp 0.5s ease both;
  }

  .sub-card:nth-child(1) { animation-delay: 0.05s; }
  .sub-card:nth-child(2) { animation-delay: 0.1s; }

  .sub-title {
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 2px;
    color: var(--muted);
    margin-bottom: 10px;
  }

  .sub-value {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 36px;
    color: #fff;
    letter-spacing: 1px;
    line-height: 1;
  }

  .sub-desc {
    font-size: 12px;
    color: var(--muted);
    margin-top: 4px;
  }

  /* UV bar */
  .uv-bar-wrap {
    margin-top: 8px;
    height: 4px;
    background: rgba(255,255,255,0.1);
    border-radius: 4px;
    overflow: hidden;
  }
  .uv-bar-fill {
    height: 100%;
    border-radius: 4px;
    background: linear-gradient(90deg, #22c55e, #eab308, #ef4444);
    transition: width 0.8s ease;
  }

  /* Forecast */
  .forecast-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 18px;
    padding: 20px;
    animation: fadeUp 0.5s 0.15s ease both;
  }

  .section-label {
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 2px;
    color: var(--muted);
    margin-bottom: 16px;
  }

  .forecast-days {
    display: flex;
    flex-direction: column;
    gap: 12px;
  }

  .forecast-day {
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .day-name {
    width: 44px;
    font-size: 13px;
    color: var(--muted);
  }

  .day-icon { font-size: 20px; width: 28px; text-align: center; }

  .day-bar-wrap {
    flex: 1;
    height: 4px;
    background: rgba(255,255,255,0.07);
    border-radius: 4px;
    overflow: hidden;
  }

  .day-bar-fill {
    height: 100%;
    border-radius: 4px;
    background: linear-gradient(90deg, var(--rain), var(--sun));
  }

  .day-temps {
    display: flex;
    gap: 8px;
    font-size: 13px;
    min-width: 60px;
    justify-content: flex-end;
  }

  .day-hi { color: #fff; font-weight: 500; }
  .day-lo { color: var(--muted); }

  /* Loading / Error */
  .status-msg {
    text-align: center;
    padding: 48px 20px;
    color: var(--muted);
    font-size: 15px;
    font-style: italic;
  }

  .spinner {
    width: 28px;
    height: 28px;
    border: 2px solid var(--border);
    border-top-color: var(--accent);
    border-radius: 50%;
    animation: spin 0.7s linear infinite;
    margin: 0 auto 16px;
  }

  @keyframes spin { to { transform: rotate(360deg); } }

  .error-msg {
    background: rgba(239,68,68,0.1);
    border: 1px solid rgba(239,68,68,0.25);
    border-radius: 12px;
    padding: 16px 20px;
    color: #fca5a5;
    font-size: 14px;
    margin-bottom: 16px;
    display: none;
  }

  .toggle-units {
    display: flex;
    justify-content: flex-end;
    margin-bottom: 20px;
  }

  .unit-btn {
    background: none;
    border: 1px solid var(--border);
    border-radius: 8px;
    color: var(--muted);
    font-family: 'DM Sans', sans-serif;
    font-size: 13px;
    padding: 6px 14px;
    cursor: pointer;
    transition: all 0.2s;
  }
  .unit-btn:hover { border-color: var(--accent); color: var(--accent); }
  .unit-btn.active { background: var(--accent); border-color: var(--accent); color: #0a0e1a; font-weight: 500; }

  .weather-output { display: none; }
</style>
</head>
<body>
<div class="app">
  <header>
    <div class="logo">SKIES<span class="logo-dot"></span></div>
  </header>

  <div class="search-wrap">
    <input class="search-input" id="cityInput" type="text" placeholder="Search city, zip code…" autocomplete="off" />
    <button class="search-btn" id="searchBtn" title="Search">
      <svg viewBox="0 0 24 24"><path d="M21 21l-4.35-4.35M17 11A6 6 0 1 1 5 11a6 6 0 0 1 12 0z" stroke="#0a0e1a" stroke-width="2.5" stroke-linecap="round" fill="none"/></svg>
    </button>
  </div>

  <div class="toggle-units" id="unitToggle" style="display:none">
    <button class="unit-btn active" id="btnC" onclick="switchUnit('C')">°C</button>
    <button class="unit-btn" id="btnF" onclick="switchUnit('F')" style="margin-left:6px">°F</button>
  </div>

  <div class="error-msg" id="errorMsg"></div>

  <div id="loadingEl" style="display:none">
    <div class="status-msg"><div class="spinner"></div>Asking the clouds…</div>
  </div>

  <div class="weather-output" id="weatherOutput">
    <div class="main-card" id="mainCard"></div>
    <div class="sub-row" id="subRow"></div>
    <div class="forecast-card" id="forecastCard"></div>
  </div>
</div>

<script>
const CLAUDE_API = "https://api.anthropic.com/v1/messages";
let currentUnit = 'C';
let lastData = null;

const $ = id => document.getElementById(id);

async function fetchWeather(city) {
  $('loadingEl').style.display = 'block';
  $('weatherOutput').style.display = 'none';
  $('errorMsg').style.display = 'none';
  $('unitToggle').style.display = 'none';

  const prompt = `You are a weather data API. Return ONLY a JSON object (no markdown, no explanation) for the city: "${city}".

Use realistic, plausible weather data for that location and current season (April in Northern Hemisphere, October in Southern Hemisphere). Make it feel authentic.

JSON schema (strictly follow this):
{
  "city": "City Name",
  "country": "Country Code",
  "temp_c": number,
  "temp_f": number,
  "feels_like_c": number,
  "feels_like_f": number,
  "condition": "Short condition text",
  "emoji": "single weather emoji",
  "humidity": number,
  "wind_kph": number,
  "wind_mph": number,
  "uv_index": number,
  "visibility_km": number,
  "pressure_mb": number,
  "forecast": [
    { "day": "Mon", "hi_c": number, "lo_c": number, "hi_f": number, "lo_f": number, "emoji": "emoji", "pop": number },
    { "day": "Tue", "hi_c": number, "lo_c": number, "hi_f": number, "lo_f": number, "emoji": "emoji", "pop": number },
    { "day": "Wed", "hi_c": number, "lo_c": number, "hi_f": number, "lo_f": number, "emoji": "emoji", "pop": number },
    { "day": "Thu", "hi_c": number, "lo_c": number, "hi_f": number, "lo_f": number, "emoji": "emoji", "pop": number },
    { "day": "Fri", "hi_c": number, "lo_c": number, "hi_f": number, "lo_f": number, "emoji": "emoji", "pop": number }
  ]
}

Only return JSON. No other text.`;

  try {
    const res = await fetch(CLAUDE_API, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        model: 'claude-sonnet-4-20250514',
        max_tokens: 1000,
        messages: [{ role: 'user', content: prompt }]
      })
    });

    const raw = await res.json();
    const text = raw.content.map(b => b.text || '').join('');
    const clean = text.replace(/```json|```/g, '').trim();
    const data = JSON.parse(clean);

    lastData = data;
    renderWeather(data, currentUnit);
    $('unitToggle').style.display = 'flex';
  } catch (e) {
    showError("Couldn't find weather for that location. Try another city.");
  } finally {
    $('loadingEl').style.display = 'none';
  }
}

function renderWeather(d, unit) {
  const isC = unit === 'C';
  const temp = isC ? d.temp_c : d.temp_f;
  const feels = isC ? d.feels_like_c : d.feels_like_f;
  const wind = isC ? d.wind_kph + ' km/h' : d.wind_mph + ' mph';
  const unitSym = isC ? '°C' : '°F';

  // Main card
  $('mainCard').innerHTML = `
    <div class="location-row">
      <svg class="location-icon" viewBox="0 0 24 24" stroke-width="1.5"><path d="M12 2C8.13 2 5 5.13 5 9c0 5.25 7 13 7 13s7-7.75 7-13c0-3.87-3.13-7-7-7zm0 9.5c-1.38 0-2.5-1.12-2.5-2.5S10.62 6.5 12 6.5s2.5 1.12 2.5 2.5-1.12 2.5-2.5 2.5z" fill="currentColor"/></svg>
      <span class="location-name">${d.city}, ${d.country}</span>
    </div>
    <div class="temp-row">
      <div>
        <span class="temp-main">${Math.round(temp)}</span><span class="temp-unit">${unitSym}</span>
      </div>
      <div class="weather-icon-big">${d.emoji}</div>
    </div>
    <div class="condition-text">${d.condition}</div>
    <div class="meta-row">
      <div class="meta-item"><span class="meta-label">Feels like</span><span class="meta-val">${Math.round(feels)}${unitSym}</span></div>
      <div class="meta-item"><span class="meta-label">Humidity</span><span class="meta-val">${d.humidity}%</span></div>
      <div class="meta-item"><span class="meta-label">Wind</span><span class="meta-val">${wind}</span></div>
    </div>
  `;

  // Sub cards
  const uvPct = Math.min(100, (d.uv_index / 11) * 100);
  $('subRow').innerHTML = `
    <div class="sub-card">
      <div class="sub-title">UV Index</div>
      <div class="sub-value">${d.uv_index}</div>
      <div class="uv-bar-wrap"><div class="uv-bar-fill" style="width:${uvPct}%"></div></div>
      <div class="sub-desc">${d.uv_index <= 2 ? 'Low' : d.uv_index <= 5 ? 'Moderate' : d.uv_index <= 7 ? 'High' : 'Very High'}</div>
    </div>
    <div class="sub-card">
      <div class="sub-title">Visibility</div>
      <div class="sub-value">${isC ? d.visibility_km : Math.round(d.visibility_km * 0.621)}<span style="font-size:18px;color:var(--muted)"> ${isC ? 'km' : 'mi'}</span></div>
      <div class="sub-desc">Pressure ${d.pressure_mb} mb</div>
    </div>
  `;

  // Forecast
  const days = d.forecast.map(f => {
    const hi = isC ? f.hi_c : f.hi_f;
    const lo = isC ? f.lo_c : f.lo_f;
    const barW = Math.max(10, f.pop);
    return `
      <div class="forecast-day">
        <span class="day-name">${f.day}</span>
        <span class="day-icon">${f.emoji}</span>
        <div class="day-bar-wrap"><div class="day-bar-fill" style="width:${barW}%"></div></div>
        <div class="day-temps"><span class="day-hi">${Math.round(hi)}°</span><span class="day-lo">${Math.round(lo)}°</span></div>
      </div>`;
  }).join('');

  $('forecastCard').innerHTML = `
    <div class="section-label">5-Day Forecast</div>
    <div class="forecast-days">${days}</div>
  `;

  $('weatherOutput').style.display = 'block';
}

function switchUnit(unit) {
  currentUnit = unit;
  $('btnC').classList.toggle('active', unit === 'C');
  $('btnF').classList.toggle('active', unit === 'F');
  if (lastData) renderWeather(lastData, unit);
}

function showError(msg) {
  $('errorMsg').textContent = msg;
  $('errorMsg').style.display = 'block';
}

function doSearch() {
  const val = $('cityInput').value.trim();
  if (!val) return;
  fetchWeather(val);
}

$('searchBtn').addEventListener('click', doSearch);
$('cityInput').addEventListener('keydown', e => { if (e.key === 'Enter') doSearch(); });

// Default load
fetchWeather('New York');
$('cityInput').value = 'New York';
</script>
</body>
</html>

