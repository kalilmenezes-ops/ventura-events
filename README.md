<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ventura Events</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --sand: #F2EBD9;
    --sand-dark: #E8DCC8;
    --ink: #1C1A16;
    --ink-muted: #6B6455;
    --ocean: #2A6E8C;
    --ocean-light: #D4E9F0;
    --sun: #E8943A;
    --sun-light: #FDF3E7;
    --sage: #5A7A5E;
    --sage-light: #E8F0E8;
    --coral: #C9503A;
    --coral-light: #FAE8E5;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--sand);
    color: var(--ink);
    font-family: 'DM Sans', sans-serif;
    font-weight: 300;
    min-height: 100vh;
  }

  header {
    padding: 48px 40px 32px;
    border-bottom: 1.5px solid var(--ink);
    display: flex;
    align-items: flex-end;
    justify-content: space-between;
    gap: 24px;
    flex-wrap: wrap;
  }

  .header-left h1 {
    font-family: 'Playfair Display', serif;
    font-size: clamp(36px, 6vw, 64px);
    font-weight: 700;
    line-height: 0.95;
    letter-spacing: -1px;
  }

  .header-left h1 em {
    font-style: italic;
    font-weight: 400;
    color: var(--ocean);
  }

  .header-left p {
    margin-top: 10px;
    font-size: 14px;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--ink-muted);
  }

  .header-right {
    display: flex;
    flex-direction: column;
    align-items: flex-end;
    gap: 10px;
  }

  #month-label {
    font-family: 'Playfair Display', serif;
    font-style: italic;
    font-size: 18px;
    color: var(--ink-muted);
  }

  .nav-btns { display: flex; gap: 8px; }

  .nav-btns button {
    background: none;
    border: 1.5px solid var(--ink);
    border-radius: 50%;
    width: 36px; height: 36px;
    cursor: pointer;
    font-size: 16px;
    transition: background 0.15s, color 0.15s;
    display: flex; align-items: center; justify-content: center;
  }

  .nav-btns button:hover { background: var(--ink); color: var(--sand); }

  .filters {
    padding: 18px 40px;
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
    border-bottom: 1px solid var(--sand-dark);
  }

  .filter-btn {
    padding: 6px 16px;
    border-radius: 99px;
    border: 1.5px solid var(--ink);
    background: none;
    font-family: 'DM Sans', sans-serif;
    font-size: 13px;
    font-weight: 400;
    cursor: pointer;
    transition: all 0.15s;
    letter-spacing: 0.02em;
  }

  .filter-btn:hover { background: var(--sand-dark); }
  .filter-btn.active { background: var(--ink); color: var(--sand); }
  .filter-btn[data-cat="music"].active      { background: var(--ocean);  border-color: var(--ocean); }
  .filter-btn[data-cat="outdoors"].active   { background: var(--sage);   border-color: var(--sage); }
  .filter-btn[data-cat="food"].active       { background: var(--sun);    border-color: var(--sun); }
  .filter-btn[data-cat="arts"].active       { background: var(--coral);  border-color: var(--coral); }
  .filter-btn[data-cat="community"].active  { background: #7A5C8A;       border-color: #7A5C8A; }

  .layout {
    display: grid;
    grid-template-columns: 280px 1fr;
    min-height: calc(100vh - 200px);
  }

  .sidebar {
    border-right: 1.5px solid var(--ink);
    padding: 28px 24px;
  }

  .mini-cal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 16px;
  }

  .mini-cal-header span {
    font-family: 'Playfair Display', serif;
    font-size: 15px;
  }

  .mini-grid {
    width: 100%;
    border-collapse: collapse;
    font-size: 13px;
  }

  .mini-grid th {
    text-align: center;
    padding: 4px 0 8px;
    color: var(--ink-muted);
    font-weight: 400;
    font-size: 11px;
    letter-spacing: 0.08em;
    text-transform: uppercase;
  }

  .mini-grid td {
    text-align: center;
    padding: 5px 2px;
    cursor: pointer;
    border-radius: 6px;
    transition: background 0.1s;
  }

  .mini-grid td:hover { background: var(--sand-dark); }

  .mini-grid td.today span {
    background: var(--ink);
    color: var(--sand);
    border-radius: 50%;
    width: 26px; height: 26px;
    display: inline-flex;
    align-items: center; justify-content: center;
  }

  .mini-grid td.has-event::after {
    content: '';
    display: block;
    width: 4px; height: 4px;
    background: var(--ocean);
    border-radius: 50%;
    margin: 2px auto 0;
  }

  .mini-grid td.other-month { color: #ccc; }

  .sidebar-tip {
    margin-top: 28px;
    padding: 16px;
    background: var(--sun-light);
    border-left: 3px solid var(--sun);
    border-radius: 0 8px 8px 0;
    font-size: 13px;
    line-height: 1.6;
    color: var(--ink-muted);
  }

  .sidebar-tip strong { color: var(--ink); font-weight: 500; }

  .event-list-area { padding: 28px 36px; }

  .loading {
    text-align: center;
    padding: 64px 24px;
    color: var(--ink-muted);
    font-family: 'Playfair Display', serif;
    font-style: italic;
    font-size: 18px;
  }

  .date-group { margin-bottom: 36px; }

  .date-heading {
    display: flex;
    align-items: baseline;
    gap: 12px;
    margin-bottom: 14px;
    padding-bottom: 8px;
    border-bottom: 1px solid var(--sand-dark);
  }

  .date-heading .day-num {
    font-family: 'Playfair Display', serif;
    font-size: 42px;
    line-height: 1;
    font-weight: 700;
    min-width: 52px;
  }

  .date-heading .day-info { display: flex; flex-direction: column; }
  .date-heading .day-name { font-size: 14px; font-weight: 500; text-transform: uppercase; letter-spacing: 0.1em; }
  .date-heading .month-yr { font-size: 13px; color: var(--ink-muted); }

  .events-row { display: flex; flex-direction: column; gap: 10px; }

  .event-card {
    display: flex;
    gap: 16px;
    padding: 16px 18px;
    background: white;
    border-radius: 12px;
    border: 1px solid var(--sand-dark);
    transition: transform 0.15s, box-shadow 0.15s;
    cursor: pointer;
    text-decoration: none;
    color: inherit;
  }

  .event-card:hover {
    transform: translateY(-2px);
    box-shadow: 0 6px 24px rgba(28,26,22,0.1);
  }

  .event-time-col {
    min-width: 68px;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    padding-top: 2px;
  }

  .event-time { font-size: 13px; font-weight: 500; }
  .event-duration { font-size: 11px; color: var(--ink-muted); margin-top: 2px; }

  .cat-dot {
    width: 8px; height: 8px;
    border-radius: 50%;
    margin-top: 8px;
  }

  .event-body { flex: 1; }

  .event-name {
    font-family: 'Playfair Display', serif;
    font-size: 17px;
    font-weight: 700;
    line-height: 1.25;
    margin-bottom: 4px;
  }

  .event-venue { font-size: 13px; color: var(--ink-muted); margin-bottom: 6px; }
  .event-desc { font-size: 13px; line-height: 1.6; color: #4a4438; }
  .event-tags { margin-top: 8px; display: flex; gap: 6px; flex-wrap: wrap; }

  .tag {
    padding: 3px 10px;
    border-radius: 99px;
    font-size: 11px;
    font-weight: 500;
    letter-spacing: 0.04em;
    text-transform: uppercase;
  }

  .tag-music     { background: var(--ocean-light);  color: var(--ocean); }
  .tag-outdoors  { background: var(--sage-light);   color: var(--sage); }
  .tag-food      { background: var(--sun-light);    color: #A0600E; }
  .tag-arts      { background: var(--coral-light);  color: var(--coral); }
  .tag-community { background: #F0EBF8;             color: #7A5C8A; }
  .tag-free      { background: #EDF8ED;             color: #2E7D32; }
  .tag-concert   { background: var(--ocean-light);  color: var(--ocean); }

  .cat-dot.music     { background: var(--ocean); }
  .cat-dot.outdoors  { background: var(--sage); }
  .cat-dot.food      { background: var(--sun); }
  .cat-dot.arts      { background: var(--coral); }
  .cat-dot.community { background: #7A5C8A; }
  .cat-dot.concert   { background: var(--ocean); }

  .empty-state {
    text-align: center;
    padding: 64px 24px;
    color: var(--ink-muted);
  }

  .empty-state .wave { font-size: 48px; margin-bottom: 16px; }
  .empty-state h3 { font-family: 'Playfair Display', serif; font-size: 22px; margin-bottom: 8px; color: var(--ink); }
  .empty-state p { font-size: 14px; line-height: 1.6; }

  footer {
    padding: 24px 40px;
    border-top: 1.5px solid var(--ink);
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 12px;
    color: var(--ink-muted);
    flex-wrap: wrap;
    gap: 8px;
  }

  footer a { color: var(--ocean); text-decoration: none; }
  footer a:hover { text-decoration: underline; }

  @media (max-width: 680px) {
    header { padding: 28px 20px 20px; }
    .filters { padding: 14px 20px; }
    .layout { grid-template-columns: 1fr; }
    .sidebar { border-right: none; border-bottom: 1.5px solid var(--ink); padding: 20px; }
    .event-list-area { padding: 20px; }
    footer { padding: 20px; }
  }
</style>
</head>
<body>

<header>
  <div class="header-left">
    <h1>Ventura<br><em>Events</em></h1>
    <p>805 · things to do · always free</p>
  </div>
  <div class="header-right">
    <span id="month-label"></span>
    <div class="nav-btns">
      <button id="prev-month" title="Previous month">‹</button>
      <button id="next-month" title="Next month">›</button>
    </div>
  </div>
</header>

<div class="filters">
  <button class="filter-btn active" data-cat="all">All events</button>
  <button class="filter-btn" data-cat="music">Music</button>
  <button class="filter-btn" data-cat="outdoors">Outdoors</button>
  <button class="filter-btn" data-cat="food">Food & Drink</button>
  <button class="filter-btn" data-cat="arts">Arts</button>
  <button class="filter-btn" data-cat="community">Community</button>
</div>

<div class="layout">
  <aside class="sidebar">
    <div class="mini-cal-header">
      <span id="mini-month-label"></span>
    </div>
    <table class="mini-grid">
      <thead>
        <tr>
          <th>Su</th><th>Mo</th><th>Tu</th>
          <th>We</th><th>Th</th><th>Fr</th><th>Sa</th>
        </tr>
      </thead>
      <tbody id="mini-cal-body"></tbody>
    </table>

    <div class="sidebar-tip">
      <strong>Adding events</strong><br>
      Add events in your Google Calendar. Any event with "concert" in the title or description gets tagged automatically.
    </div>
  </aside>

  <main class="event-list-area" id="event-list">
    <div class="loading">Loading events…</div>
  </main>
</div>

<footer>
  <span>Ventura Events · made with ♥ for the 805</span>
  <span>
    <a href="https://www.cityofventura.ca.gov/calendar" target="_blank">City of Ventura</a> ·
    <a href="https://www.visitventuraca.com" target="_blank">Visit Ventura</a>
  </span>
</footer>

<script>
// ============================================================
//  GOOGLE CALENDAR CONFIG
// ============================================================
const CALENDAR_ID = "38bebb1f740039248dbd383da4fad1cf350923ab82731b30b5cb769bea779503@group.calendar.google.com";
const PROXY = "https://corsproxy.io/?url=" + encodeURIComponent(
  "https://calendar.google.com/calendar/ical/" + CALENDAR_ID + "/public/basic.ics"
);

// ============================================================
//  AUTO-TAGGING RULES
//  Add more keywords here anytime to improve categorization.
// ============================================================
const TAGGING_RULES = [
  { keywords: ["concert", "live music", "band", "jazz", "symphony", "quartet", "orchestra", "dj", "show"], category: "music" },
  { keywords: ["hike", "kayak", "surf", "beach", "trail", "paddle", "bike", "outdoor", "birding", "cleanup", "nature", "walk"], category: "outdoors" },
  { keywords: ["taco", "food", "brew", "beer", "wine", "tasting", "dinner", "restaurant", "market", "farmers", "taste"], category: "food" },
  { keywords: ["art", "gallery", "exhibit", "painting", "mural", "sculpture", "plein air", "art walk", "craft"], category: "arts" },
  { keywords: ["community", "volunteer", "festival", "fair", "parade", "family", "free", "benefit"], category: "community" },
];

function autoTag(name, description) {
  const text = (name + " " + description).toLowerCase();
  const categories = new Set();
  TAGGING_RULES.forEach(rule => {
    if (rule.keywords.some(kw => text.includes(kw))) {
      categories.add(rule.category);
    }
  });
  if (text.includes("concert")) categories.add("concert");
  return categories.size > 0 ? [...categories] : ["community"];
}

// ============================================================
//  ICS PARSER
// ============================================================
function parseICS(text) {
  const parsed = [];
  const blocks = text.split("BEGIN:VEVENT").slice(1);

  blocks.forEach(block => {
    const get = (key) => {
      const match = block.match(new RegExp(key + "[A-Z;=a-z]*:([^\\r\\n]+)"));
      return match ? match[1].replace(/\\n/g, " ").replace(/\\,/g, ",").trim() : "";
    };

    const dtstart = get("DTSTART");
    const dtend   = get("DTEND");
    if (!dtstart) return;

    const dateStr = dtstart.slice(0,4) + "-" + dtstart.slice(4,6) + "-" + dtstart.slice(6,8);

    let time = "All day";
    let duration = "";
    if (dtstart.includes("T")) {
      const h = parseInt(dtstart.slice(9,11));
      const m = dtstart.slice(11,13);
      time = (h > 12 ? h - 12 : h || 12) + ":" + m + (h >= 12 ? " PM" : " AM");

      if (dtend && dtend.includes("T")) {
        const h2 = parseInt(dtend.slice(9,11));
        const m2 = dtend.slice(11,13);
        const diff = (h2 * 60 + parseInt(m2)) - (h * 60 + parseInt(m));
        if (diff > 0) {
          const hrs = Math.floor(diff / 60);
          const mins = diff % 60;
          duration = hrs > 0 ? hrs + (mins > 0 ? "h " + mins + "m" : " hr") : mins + "m";
        }
      }
    }

    const name = get("SUMMARY") || "Untitled";
    const desc = get("DESCRIPTION") || "";
    const venue = get("LOCATION") || "Ventura, CA";
    const url = get("URL") || "";
    const isFree = (name + " " + desc).toLowerCase().includes("free");
    const categories = autoTag(name, desc);

    parsed.push({ date: dateStr, time, duration, name, venue, description: desc, categories, free: isFree, url });
  });

  return parsed.sort((a, b) => a.date.localeCompare(b.date));
}

// ============================================================
//  STATE
// ============================================================
const MONTHS = ["January","February","March","April","May","June",
                "July","August","September","October","November","December"];
const DAYS = ["Sun","Mon","Tue","Wed","Thu","Fri","Sat"];

let events = [];
let viewYear  = new Date().getFullYear();
let viewMonth = new Date().getMonth();
let activeFilter = "all";

// ============================================================
//  RENDER
// ============================================================
function getEventDatesForMonth(yr, mo) {
  return new Set(
    events
      .filter(e => {
        const d = new Date(e.date + "T12:00:00");
        return d.getFullYear() === yr && d.getMonth() === mo;
      })
      .map(e => new Date(e.date + "T12:00:00").getDate())
  );
}

function renderMiniCal() {
  document.getElementById("mini-month-label").textContent = MONTHS[viewMonth] + " " + viewYear;
  const eventDates = getEventDatesForMonth(viewYear, viewMonth);
  const first = new Date(viewYear, viewMonth, 1).getDay();
  const daysInMonth = new Date(viewYear, viewMonth + 1, 0).getDate();
  const today = new Date();
  let html = "";
  let day = 1;
  let started = false;

  for (let row = 0; row < 6; row++) {
    html += "<tr>";
    for (let col = 0; col < 7; col++) {
      if (!started && col < first) {
        html += "<td class='other-month'></td>";
      } else if (day > daysInMonth) {
        html += "<td></td>";
      } else {
        started = true;
        const isToday = today.getFullYear() === viewYear && today.getMonth() === viewMonth && today.getDate() === day;
        const hasEvent = eventDates.has(day);
        const cls = [isToday ? "today" : "", hasEvent ? "has-event" : ""].filter(Boolean).join(" ");
        const d = day;
        html += `<td class="${cls}" onclick="jumpToDate(${viewYear},${viewMonth},${d})">`;
        html += isToday ? `<span>${day}</span>` : day;
        html += "</td>";
        day++;
      }
    }
    html += "</tr>";
    if (day > daysInMonth) break;
  }
  document.getElementById("mini-cal-body").innerHTML = html;
}

function renderEvents() {
  document.getElementById("month-label").textContent = MONTHS[viewMonth] + " " + viewYear;

  const filtered = events.filter(e => {
    const d = new Date(e.date + "T12:00:00");
    const matchMonth = d.getFullYear() === viewYear && d.getMonth() === viewMonth;
    const matchCat   = activeFilter === "all" || e.categories.includes(activeFilter);
    return matchMonth && matchCat;
  });

  if (filtered.length === 0) {
    document.getElementById("event-list").innerHTML = `
      <div class="empty-state">
        <div class="wave">🌊</div>
        <h3>No events this month</h3>
        <p>Try a different filter or another month.<br>Add events in Google Calendar and they'll appear here automatically.</p>
      </div>`;
    return;
  }

  const groups = {};
  filtered.forEach(e => {
    if (!groups[e.date]) groups[e.date] = [];
    groups[e.date].push(e);
  });

  const html = Object.keys(groups).sort().map(dateStr => {
    const d = new Date(dateStr + "T12:00:00");
    const cards = groups[dateStr].map(e => {
      const catTags = e.categories.map(c => `<span class="tag tag-${c}">${c}</span>`).join("");
      const freeBadge = e.free ? `<span class="tag tag-free">Free</span>` : "";
      const primaryCat = e.categories[0];
      const href = e.url ? `href="${e.url}" target="_blank"` : `href="#"`;
      return `
        <a class="event-card" ${href}>
          <div class="event-time-col">
            <span class="event-time">${e.time}</span>
            <span class="event-duration">${e.duration}</span>
            <div class="cat-dot ${primaryCat}"></div>
          </div>
          <div class="event-body">
            <div class="event-name">${e.name}</div>
            <div class="event-venue">📍 ${e.venue}</div>
            <div class="event-desc">${e.description}</div>
            <div class="event-tags">${catTags}${freeBadge}</div>
          </div>
        </a>`;
    }).join("");

    return `
      <div class="date-group" id="date-${dateStr}">
        <div class="date-heading">
          <span class="day-num">${d.getDate()}</span>
          <div class="day-info">
            <span class="day-name">${DAYS[d.getDay()]}</span>
            <span class="month-yr">${MONTHS[d.getMonth()]} ${d.getFullYear()}</span>
          </div>
        </div>
        <div class="events-row">${cards}</div>
      </div>`;
  }).join("");

  document.getElementById("event-list").innerHTML = html;
}

function jumpToDate(yr, mo, day) {
  viewYear = yr; viewMonth = mo;
  renderMiniCal(); renderEvents();
  const el = document.getElementById(`date-${yr}-${String(mo+1).padStart(2,"0")}-${String(day).padStart(2,"0")}`);
  if (el) el.scrollIntoView({ behavior: "smooth", block: "start" });
}

// ============================================================
//  LOAD FROM GOOGLE CALENDAR
// ============================================================
async function loadCalendar() {
  try {
    const res = await fetch(PROXY);
    if (!res.ok) throw new Error("Bad response");
    const text = await res.text();
    events = parseICS(text);

    // Jump to first month with events if current month is empty
    const currentHasEvents = events.some(e => {
      const d = new Date(e.date + "T12:00:00");
      return d.getFullYear() === viewYear && d.getMonth() === viewMonth;
    });
    if (!currentHasEvents && events.length > 0) {
      const first = new Date(events[0].date + "T12:00:00");
      viewYear  = first.getFullYear();
      viewMonth = first.getMonth();
    }

    renderMiniCal();
    renderEvents();
  } catch(err) {
    console.error(err);
    document.getElementById("event-list").innerHTML = `
      <div class="empty-state">
        <div class="wave">🌊</div>
        <h3>Couldn't load events</h3>
        <p>Make sure your Google Calendar is set to <strong>public</strong>.<br>
        In Google Calendar: click the three dots next to your calendar →
        Settings and sharing → Access permissions → Make available to public.</p>
      </div>`;
  }
}

// ============================================================
//  INIT
// ============================================================
document.querySelectorAll(".filter-btn").forEach(btn => {
  btn.addEventListener("click", () => {
    document.querySelectorAll(".filter-btn").forEach(b => b.classList.remove("active"));
    btn.classList.add("active");
    activeFilter = btn.dataset.cat;
    renderEvents();
  });
});

document.getElementById("prev-month").addEventListener("click", () => {
  viewMonth--;
  if (viewMonth < 0) { viewMonth = 11; viewYear--; }
  renderMiniCal(); renderEvents();
});

document.getElementById("next-month").addEventListener("click", () => {
  viewMonth++;
  if (viewMonth > 11) { viewMonth = 0; viewYear++; }
  renderMiniCal(); renderEvents();
});

loadCalendar();
</script>
</body>
</html>
