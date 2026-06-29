<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Daily Activity Transport — NDC Sidoarjo</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #0d0f14;
    --surface: #151820;
    --card: #1c2030;
    --card2: #222638;
    --border: rgba(255,255,255,0.07);
    --border2: rgba(255,255,255,0.12);
    --text: #e8eaf0;
    --text-sec: #8b90a0;
    --text-muted: #5a5f72;
    --accent-blue: #3987e5;
    --accent-teal: #199e70;
    --accent-amber: #eda100;
    --accent-red: #e34948;
    --accent-purple: #9085e9;
    --accent-coral: #eb6834;
    --radius: 10px;
    --radius-lg: 14px;
  }

  body {
    font-family: 'Inter', sans-serif;
    background: var(--bg);
    color: var(--text);
    font-size: 13px;
    line-height: 1.5;
    min-height: 100vh;
  }

  /* ── TOP BAR ── */
  .topbar {
    background: var(--surface);
    border-bottom: 1px solid var(--border);
    padding: 12px 24px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    position: sticky;
    top: 0;
    z-index: 100;
  }
  .topbar-left { display: flex; align-items: center; gap: 12px; }
  .logo-dot { width: 8px; height: 8px; border-radius: 50%; background: var(--accent-blue); box-shadow: 0 0 6px var(--accent-blue); }
  .topbar h1 { font-size: 14px; font-weight: 600; letter-spacing: 0.02em; }
  .topbar-right { display: flex; align-items: center; gap: 12px; }
  .date-badge {
    background: var(--card);
    border: 1px solid var(--border2);
    border-radius: 6px;
    padding: 5px 12px;
    font-size: 12px;
    color: var(--text-sec);
  }
  .duty-badge {
    background: rgba(57,135,229,0.15);
    border: 1px solid rgba(57,135,229,0.3);
    border-radius: 6px;
    padding: 5px 12px;
    font-size: 11px;
    font-weight: 600;
    color: var(--accent-blue);
    letter-spacing: 0.05em;
    text-transform: uppercase;
  }

  /* ── LAYOUT ── */
  .main { padding: 20px 24px; max-width: 1400px; margin: 0 auto; }

  /* ── SECTION LABEL ── */
  .section-label {
    font-size: 10px;
    font-weight: 600;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--text-muted);
    margin-bottom: 10px;
    margin-top: 20px;
  }

  /* ── KPI ROW ── */
  .kpi-row { display: grid; grid-template-columns: repeat(auto-fit, minmax(140px, 1fr)); gap: 10px; margin-bottom: 16px; }

  .kpi-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 14px 16px;
    position: relative;
    overflow: hidden;
  }
  .kpi-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    border-radius: var(--radius) var(--radius) 0 0;
  }
  .kpi-card.blue::before  { background: var(--accent-blue); }
  .kpi-card.teal::before  { background: var(--accent-teal); }
  .kpi-card.amber::before { background: var(--accent-amber); }
  .kpi-card.red::before   { background: var(--accent-red); }
  .kpi-card.purple::before { background: var(--accent-purple); }
  .kpi-card.coral::before { background: var(--accent-coral); }

  .kpi-label { font-size: 10px; color: var(--text-muted); letter-spacing: 0.05em; text-transform: uppercase; margin-bottom: 6px; }
  .kpi-value { font-size: 28px; font-weight: 700; line-height: 1; }
  .kpi-value.blue  { color: var(--accent-blue); }
  .kpi-value.teal  { color: var(--accent-teal); }
  .kpi-value.amber { color: var(--accent-amber); }
  .kpi-value.red   { color: var(--accent-red); }
  .kpi-value.purple { color: var(--accent-purple); }
  .kpi-value.coral { color: var(--accent-coral); }
  .kpi-sub { font-size: 11px; color: var(--text-sec); margin-top: 4px; }

  /* ── GRID LAYOUTS ── */
  .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
  .grid-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 12px; }
  .grid-65-35 { display: grid; grid-template-columns: 1.8fr 1fr; gap: 12px; }
  .grid-35-65 { display: grid; grid-template-columns: 1fr 1.8fr; gap: 12px; }

  @media(max-width: 900px) {
    .grid-2, .grid-3, .grid-65-35, .grid-35-65 { grid-template-columns: 1fr; }
  }

  /* ── PANEL CARD ── */
  .panel {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: var(--radius-lg);
    padding: 16px;
  }
  .panel-title {
    font-size: 11px;
    font-weight: 600;
    color: var(--text-sec);
    letter-spacing: 0.06em;
    text-transform: uppercase;
    margin-bottom: 14px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  .panel-badge {
    font-size: 10px;
    padding: 2px 8px;
    border-radius: 20px;
    font-weight: 600;
    letter-spacing: 0.04em;
  }
  .badge-blue   { background: rgba(57,135,229,0.15); color: var(--accent-blue); }
  .badge-teal   { background: rgba(25,158,112,0.15); color: var(--accent-teal); }
  .badge-amber  { background: rgba(237,161,0,0.15); color: var(--accent-amber); }
  .badge-red    { background: rgba(227,73,72,0.15); color: var(--accent-red); }

  /* ── DONUT WRAPPER ── */
  .donut-row {
    display: flex;
    gap: 20px;
    align-items: center;
  }
  .donut-wrap {
    position: relative;
    width: 110px;
    height: 110px;
    flex-shrink: 0;
  }
  .donut-center {
    position: absolute;
    inset: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    pointer-events: none;
  }
  .donut-num { font-size: 22px; font-weight: 700; line-height: 1; }
  .donut-lbl { font-size: 9px; color: var(--text-muted); margin-top: 2px; text-transform: uppercase; }
  .legend-list { list-style: none; display: flex; flex-direction: column; gap: 7px; }
  .legend-list li { display: flex; align-items: center; gap: 8px; font-size: 12px; color: var(--text-sec); }
  .legend-dot { width: 8px; height: 8px; border-radius: 2px; flex-shrink: 0; }
  .legend-val { margin-left: auto; font-weight: 600; color: var(--text); }

  /* ── TABLE ── */
  .data-table { width: 100%; border-collapse: collapse; }
  .data-table th {
    font-size: 10px;
    font-weight: 600;
    color: var(--text-muted);
    letter-spacing: 0.05em;
    text-transform: uppercase;
    padding: 6px 10px;
    text-align: left;
    border-bottom: 1px solid var(--border);
  }
  .data-table td {
    padding: 8px 10px;
    font-size: 12px;
    border-bottom: 1px solid var(--border);
    color: var(--text-sec);
  }
  .data-table td:first-child { color: var(--text); font-weight: 500; }
  .data-table tr:last-child td { border-bottom: none; }
  .data-table tr:hover td { background: rgba(255,255,255,0.02); }
  .num-cell { text-align: right; font-variant-numeric: tabular-nums; }
  .highlight-cell { color: var(--accent-blue) !important; font-weight: 600; }

  /* ── OLF PROGRESS BAR ── */
  .olf-row { display: flex; align-items: center; gap: 10px; margin-bottom: 10px; }
  .olf-label { font-size: 11px; color: var(--text-sec); width: 70px; flex-shrink: 0; }
  .olf-bar-wrap { flex: 1; height: 6px; background: var(--card2); border-radius: 10px; overflow: hidden; }
  .olf-bar { height: 100%; border-radius: 10px; transition: width 0.8s ease; }
  .olf-pct { font-size: 11px; font-weight: 600; width: 42px; text-align: right; flex-shrink: 0; }

  /* ── STATUS CHIPS ── */
  .chip-grid { display: flex; flex-wrap: wrap; gap: 8px; }
  .chip {
    display: flex;
    align-items: center;
    gap: 6px;
    background: var(--card2);
    border: 1px solid var(--border);
    border-radius: 6px;
    padding: 6px 10px;
    font-size: 11px;
  }
  .chip-dot { width: 6px; height: 6px; border-radius: 50%; }
  .chip-val { font-weight: 600; font-size: 13px; }

  /* ── ISSUE TABLE ── */
  .issue-row {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 10px 14px;
    margin-bottom: 8px;
    display: grid;
    grid-template-columns: 20px 1fr 1fr auto;
    gap: 12px;
    align-items: start;
  }
  .issue-num { font-weight: 700; color: var(--text-muted); font-size: 11px; }
  .issue-text { font-size: 12px; color: var(--text); line-height: 1.5; }
  .issue-action { font-size: 11px; color: var(--text-sec); line-height: 1.5; }
  .status-pill {
    font-size: 10px;
    font-weight: 600;
    padding: 2px 8px;
    border-radius: 20px;
    white-space: nowrap;
    letter-spacing: 0.04em;
  }
  .pill-process { background: rgba(237,161,0,0.15); color: var(--accent-amber); }
  .pill-done    { background: rgba(25,158,112,0.15); color: var(--accent-teal); }

  /* ── DELIVERY PROGRESS ── */
  .delivery-item { display: flex; align-items: center; gap: 10px; margin-bottom: 8px; }
  .delivery-name { font-size: 11px; color: var(--text-sec); width: 100px; flex-shrink: 0; }
  .delivery-bar-wrap { flex: 1; height: 8px; background: var(--card2); border-radius: 10px; overflow: hidden; }
  .delivery-bar { height: 100%; border-radius: 10px; }
  .delivery-pct { font-size: 11px; font-weight: 600; width: 48px; text-align: right; color: var(--text); }

  /* ── BIG METRIC ── */
  .big-metric { text-align: center; padding: 12px 0; }
  .big-metric-val { font-size: 42px; font-weight: 700; line-height: 1; }
  .big-metric-lbl { font-size: 11px; color: var(--text-muted); margin-top: 4px; text-transform: uppercase; letter-spacing: 0.06em; }

  /* ── SEPARATOR ── */
  .sep { height: 1px; background: var(--border); margin: 16px 0; }

  /* ── HUB STATUS ── */
  .hub-row { display: flex; align-items: center; gap: 10px; margin-bottom: 10px; }
  .hub-name { font-size: 11px; color: var(--text); width: 110px; flex-shrink: 0; }
  .hub-stats { display: flex; gap: 8px; flex: 1; }
  .hub-stat { background: var(--card2); border-radius: 6px; padding: 3px 8px; font-size: 11px; }
  .hub-stat span { color: var(--text-muted); font-size: 10px; }

  /* ── SCROLLBAR ── */
  ::-webkit-scrollbar { width: 4px; }
  ::-webkit-scrollbar-track { background: var(--bg); }
  ::-webkit-scrollbar-thumb { background: var(--card2); border-radius: 4px; }
</style>
</head>
<body>

<!-- TOP BAR -->
<div class="topbar">
  <div class="topbar-left">
    <div class="logo-dot"></div>
    <h1>Daily Activity Transport — NDC Sidoarjo</h1>
  </div>
  <div class="topbar-right">
    <div class="date-badge" id="live-date">28 Jun 2026</div>
    <div class="duty-badge">Duty Pagi · MUNIF</div>
  </div>
</div>

<div class="main">

  <!-- KPI ROW -->
  <p class="section-label">Ringkasan Trip Hari Ini</p>
  <div class="kpi-row">
    <div class="kpi-card blue">
      <div class="kpi-label">Total Trip Internal</div>
      <div class="kpi-value blue">199</div>
      <div class="kpi-sub">On Delivery: 1 &nbsp;·&nbsp; Balik DC: 84</div>
    </div>
    <div class="kpi-card teal">
      <div class="kpi-label">Total Trip External</div>
      <div class="kpi-value teal">125</div>
      <div class="kpi-sub">New: 45 &nbsp;·&nbsp; Close: 80</div>
    </div>
    <div class="kpi-card amber">
      <div class="kpi-label">Total LC</div>
      <div class="kpi-value amber">5,458</div>
      <div class="kpi-sub">DO: 1,156 &nbsp;·&nbsp; OLF: 71.99%</div>
    </div>
    <div class="kpi-card coral">
      <div class="kpi-label">Persentasi Kirim</div>
      <div class="kpi-value coral">99.87%</div>
      <div class="kpi-sub">Plan MPP: 16,162</div>
    </div>
    <div class="kpi-card purple">
      <div class="kpi-label">% Soken</div>
      <div class="kpi-value purple">20,804%</div>
      <div class="kpi-sub">Demand: 314</div>
    </div>
    <div class="kpi-card red">
      <div class="kpi-label">Armada Availability</div>
      <div class="kpi-value red">54 / 57</div>
      <div class="kpi-sub">Non-Available: 3 unit</div>
    </div>
  </div>

  <!-- ROW 1: DONUT INTERNAL + DONUT EXTERNAL + OLF -->
  <div class="grid-3" style="margin-bottom:12px;">

    <!-- Internal Armada Donut -->
    <div class="panel">
      <div class="panel-title">Armada Internal <span class="panel-badge badge-blue">199 Trip</span></div>
      <div class="donut-row">
        <div class="donut-wrap">
          <canvas id="chartInternal" role="img" aria-label="Status armada internal"></canvas>
          <div class="donut-center">
            <div class="donut-num" style="color:var(--accent-blue)">199</div>
            <div class="donut-lbl">Trip</div>
          </div>
        </div>
        <ul class="legend-list">
          <li><span class="legend-dot" style="background:#3987e5"></span>New <span class="legend-val">63</span></li>
          <li><span class="legend-dot" style="background:#9085e9"></span>Checkin <span class="legend-val">21</span></li>
          <li><span class="legend-dot" style="background:#5a5f72"></span>Open <span class="legend-val">9</span></li>
          <li><span class="legend-dot" style="background:#199e70"></span>Close <span class="legend-val">20</span></li>
          <li><span class="legend-dot" style="background:#eda100"></span>Checkout <span class="legend-val">86</span></li>
        </ul>
      </div>
    </div>

    <!-- External Armada Donut -->
    <div class="panel">
      <div class="panel-title">Armada External <span class="panel-badge badge-teal">125 Trip</span></div>
      <div class="donut-row">
        <div class="donut-wrap">
          <canvas id="chartExternal" role="img" aria-label="Status armada external"></canvas>
          <div class="donut-center">
            <div class="donut-num" style="color:var(--accent-teal)">125</div>
            <div class="donut-lbl">Trip</div>
          </div>
        </div>
        <ul class="legend-list">
          <li><span class="legend-dot" style="background:#3987e5"></span>New <span class="legend-val">45</span></li>
          <li><span class="legend-dot" style="background:#5a5f72"></span>Open <span class="legend-val">0</span></li>
          <li><span class="legend-dot" style="background:#199e70"></span>Close <span class="legend-val">12</span></li>
          <li><span class="legend-dot" style="background:#eda100"></span>Checkout <span class="legend-val">80</span></li>
        </ul>
      </div>
    </div>

    <!-- OLF Panel -->
    <div class="panel">
      <div class="panel-title">OLF / Plan / Aktual</div>
      <div style="margin-bottom:14px;">
        <div style="font-size:10px;color:var(--text-muted);margin-bottom:8px;letter-spacing:.05em;text-transform:uppercase;">HCI — Plan vs Aktual</div>
        <div class="olf-row">
          <div class="olf-label">Store LK</div>
          <div class="olf-bar-wrap"><div class="olf-bar" style="width:81%;background:var(--accent-blue)"></div></div>
          <div class="olf-pct" style="color:var(--accent-blue)">81.32%</div>
        </div>
        <div class="olf-row">
          <div class="olf-label">Store DK</div>
          <div class="olf-bar-wrap"><div class="olf-bar" style="width:78%;background:var(--accent-teal)"></div></div>
          <div class="olf-pct" style="color:var(--accent-teal)">78.00%</div>
        </div>
        <div class="olf-row">
          <div class="olf-label">Hub</div>
          <div class="olf-bar-wrap"><div class="olf-bar" style="width:79%;background:var(--accent-purple)"></div></div>
          <div class="olf-pct" style="color:var(--accent-purple)">79.38%</div>
        </div>
      </div>
      <div class="sep"></div>
      <div>
        <div style="font-size:10px;color:var(--text-muted);margin-bottom:8px;letter-spacing:.05em;text-transform:uppercase;">AHI — Plan vs Aktual</div>
        <div class="olf-row">
          <div class="olf-label">Store LK</div>
          <div class="olf-bar-wrap"><div class="olf-bar" style="width:72%;background:var(--accent-amber)"></div></div>
          <div class="olf-pct" style="color:var(--accent-amber)">72.34%</div>
        </div>
        <div class="olf-row">
          <div class="olf-label">Store DK</div>
          <div class="olf-bar-wrap"><div class="olf-bar" style="width:75%;background:var(--accent-coral)"></div></div>
          <div class="olf-pct" style="color:var(--accent-coral)">75.21%</div>
        </div>
        <div class="olf-row">
          <div class="olf-label">Hub</div>
          <div class="olf-bar-wrap"><div class="olf-bar" style="width:76%;background:var(--accent-red)"></div></div>
          <div class="olf-pct" style="color:var(--accent-red)">76.75%</div>
        </div>
      </div>
    </div>
  </div>

  <!-- ROW 2: TYPE TABLE + AVAILABILITY -->
  <div class="grid-65-35" style="margin-bottom:12px;">

    <!-- Type Table Internal -->
    <div class="panel">
      <div class="panel-title">Detail Type — Armada Internal</div>
      <div style="overflow-x:auto;">
        <table class="data-table">
          <thead>
            <tr>
              <th>Type</th>
              <th class="num-cell">LC</th>
              <th class="num-cell">DO</th>
              <th class="num-cell">OLF</th>
              <th class="num-cell">DO/Trip</th>
              <th class="num-cell">DP/Trip</th>
              <th class="num-cell">New</th>
              <th class="num-cell">Close</th>
              <th class="num-cell">CO</th>
              <th class="num-cell">Balik DC</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>Customer</td>
              <td class="num-cell">133</td>
              <td class="num-cell">1,494</td>
              <td class="num-cell highlight-cell">73.49%</td>
              <td class="num-cell">11.2</td>
              <td class="num-cell">7.0</td>
              <td class="num-cell">38</td>
              <td class="num-cell">15</td>
              <td class="num-cell">59</td>
              <td class="num-cell">58</td>
            </tr>
            <tr>
              <td>Store DK</td>
              <td class="num-cell">41</td>
              <td class="num-cell">2,508</td>
              <td class="num-cell highlight-cell">69.00%</td>
              <td class="num-cell">61.2</td>
              <td class="num-cell">4.2</td>
              <td class="num-cell">15</td>
              <td class="num-cell">3</td>
              <td class="num-cell">17</td>
              <td class="num-cell">16</td>
            </tr>
            <tr>
              <td>Hub</td>
              <td class="num-cell">12</td>
              <td class="num-cell">581</td>
              <td class="num-cell highlight-cell">76.75%</td>
              <td class="num-cell">48.4</td>
              <td class="num-cell">2.4</td>
              <td class="num-cell">5</td>
              <td class="num-cell">0</td>
              <td class="num-cell">5</td>
              <td class="num-cell">5</td>
            </tr>
            <tr>
              <td>Store LK</td>
              <td class="num-cell">10</td>
              <td class="num-cell">865</td>
              <td class="num-cell highlight-cell">78.56%</td>
              <td class="num-cell">86.5</td>
              <td class="num-cell">2.4</td>
              <td class="num-cell">5</td>
              <td class="num-cell">2</td>
              <td class="num-cell">2</td>
              <td class="num-cell">2</td>
            </tr>
            <tr>
              <td>Others</td>
              <td class="num-cell">3</td>
              <td class="num-cell">10</td>
              <td class="num-cell" style="color:var(--accent-red);font-weight:600;">8.17%</td>
              <td class="num-cell">3.3</td>
              <td class="num-cell">1.7</td>
              <td class="num-cell">0</td>
              <td class="num-cell">0</td>
              <td class="num-cell">3</td>
              <td class="num-cell">3</td>
            </tr>
            <tr style="background:rgba(57,135,229,0.05);">
              <td style="color:var(--text);font-weight:700;">Grand Total</td>
              <td class="num-cell" style="color:var(--text);font-weight:700;">199</td>
              <td class="num-cell" style="color:var(--text);font-weight:700;">5,458</td>
              <td class="num-cell" style="color:var(--accent-blue);font-weight:700;">71.99%</td>
              <td class="num-cell" style="color:var(--text);font-weight:700;">27.4</td>
              <td class="num-cell" style="color:var(--text);font-weight:700;">5.8</td>
              <td class="num-cell" style="color:var(--text);font-weight:700;">63</td>
              <td class="num-cell" style="color:var(--text);font-weight:700;">20</td>
              <td class="num-cell" style="color:var(--text);font-weight:700;">86</td>
              <td class="num-cell" style="color:var(--text);font-weight:700;">84</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- Availability Armada -->
    <div class="panel">
      <div class="panel-title">Availability Armada</div>
      <table class="data-table">
        <thead>
          <tr>
            <th>Type</th>
            <th class="num-cell">Asset</th>
            <th class="num-cell">Avl</th>
            <th class="num-cell">NonAvl</th>
            <th class="num-cell">Utl</th>
          </tr>
        </thead>
        <tbody>
          <tr><td>CDD</td><td class="num-cell">8</td><td class="num-cell" style="color:var(--accent-teal)">8</td><td class="num-cell">0</td><td class="num-cell">8</td></tr>
          <tr><td>CDE</td><td class="num-cell">34</td><td class="num-cell" style="color:var(--accent-teal)">33</td><td class="num-cell" style="color:var(--accent-red)">1</td><td class="num-cell">33</td></tr>
          <tr><td>CDE PU</td><td class="num-cell">2</td><td class="num-cell" style="color:var(--accent-amber)">1</td><td class="num-cell" style="color:var(--accent-red)">1</td><td class="num-cell">1</td></tr>
          <tr><td>MVB</td><td class="num-cell">5</td><td class="num-cell" style="color:var(--accent-teal)">4</td><td class="num-cell" style="color:var(--accent-red)">1</td><td class="num-cell">4</td></tr>
          <tr><td>PICKUP</td><td class="num-cell">4</td><td class="num-cell" style="color:var(--accent-teal)">4</td><td class="num-cell">0</td><td class="num-cell">4</td></tr>
          <tr><td>WB</td><td class="num-cell">4</td><td class="num-cell" style="color:var(--accent-teal)">4</td><td class="num-cell">0</td><td class="num-cell">4</td></tr>
          <tr style="background:rgba(57,135,229,0.05);">
            <td style="font-weight:700;color:var(--text)">Total</td>
            <td class="num-cell" style="font-weight:700;color:var(--text)">57</td>
            <td class="num-cell" style="font-weight:700;color:var(--accent-teal)">54</td>
            <td class="num-cell" style="font-weight:700;color:var(--accent-red)">3</td>
            <td class="num-cell" style="font-weight:700;color:var(--text)">54</td>
          </tr>
        </tbody>
      </table>
      <div class="sep"></div>
      <div style="font-size:10px;color:var(--text-muted);margin-bottom:8px;letter-spacing:.05em;text-transform:uppercase;">Plan H-1 Status</div>
      <div class="chip-grid">
        <div class="chip"><div class="chip-dot" style="background:var(--accent-red)"></div>Belum Plot <span class="chip-val" style="margin-left:4px;color:var(--accent-red)">63</span></div>
        <div class="chip"><div class="chip-dot" style="background:var(--accent-teal)"></div>Sudah Plot <span class="chip-val" style="margin-left:4px;color:var(--accent-teal)">136</span></div>
      </div>
    </div>
  </div>

  <!-- ROW 3: CHART TRIP + DELIVERY PROGRESS + BONGKARAN -->
  <div class="grid-3" style="margin-bottom:12px;">

    <!-- Trip by Type Chart -->
    <div class="panel">
      <div class="panel-title">Trip Per Type — Internal</div>
      <div style="position:relative;height:180px;">
        <canvas id="chartTripType" role="img" aria-label="Bar chart trip per type armada internal"></canvas>
      </div>
    </div>

    <!-- Delivery Progress -->
    <div class="panel">
      <div class="panel-title">Progress Delivery <span class="panel-badge badge-teal">99.87%</span></div>
      <div class="big-metric" style="padding:6px 0 12px;">
        <div class="big-metric-val" style="color:var(--accent-teal);font-size:36px;">99.87%</div>
        <div class="big-metric-lbl">Persentasi pengiriman</div>
      </div>
      <div class="sep"></div>
      <div style="font-size:10px;color:var(--text-muted);margin-bottom:10px;letter-spacing:.05em;text-transform:uppercase;">Status Plan Area LK</div>
      <div class="delivery-item">
        <div class="delivery-name">GRW</div>
        <div class="delivery-bar-wrap"><div class="delivery-bar" style="width:85%;background:var(--accent-blue)"></div></div>
        <div class="delivery-pct">4,456</div>
      </div>
      <div class="delivery-item">
        <div class="delivery-name">Customer</div>
        <div class="delivery-bar-wrap"><div class="delivery-bar" style="width:99%;background:var(--accent-teal)"></div></div>
        <div class="delivery-pct">735</div>
      </div>
      <div class="delivery-item">
        <div class="delivery-name">HUB-GRW</div>
        <div class="delivery-bar-wrap"><div class="delivery-bar" style="width:70%;background:var(--accent-amber)"></div></div>
        <div class="delivery-pct">146</div>
      </div>
      <div class="delivery-item">
        <div class="delivery-name">HUB-Cust</div>
        <div class="delivery-bar-wrap"><div class="delivery-bar" style="width:92%;background:var(--accent-purple)"></div></div>
        <div class="delivery-pct">194</div>
      </div>
      <div class="delivery-item">
        <div class="delivery-name">RT</div>
        <div class="delivery-bar-wrap"><div class="delivery-bar" style="width:77%;background:var(--accent-coral)"></div></div>
        <div class="delivery-pct">103</div>
      </div>
    </div>

    <!-- Status Bongkaran Hub -->
    <div class="panel">
      <div class="panel-title">Status Bongkaran Hub</div>
      <div class="kpi-row" style="grid-template-columns:1fr 1fr;gap:8px;margin-bottom:12px;">
        <div class="kpi-card teal" style="padding:10px 12px;">
          <div class="kpi-label">Bongkar</div>
          <div class="kpi-value teal" style="font-size:22px;">152</div>
        </div>
        <div class="kpi-card blue" style="padding:10px 12px;">
          <div class="kpi-label">Console</div>
          <div class="kpi-value blue" style="font-size:22px;">95</div>
        </div>
        <div class="kpi-card amber" style="padding:10px 12px;">
          <div class="kpi-label">CBM DK</div>
          <div class="kpi-value amber" style="font-size:18px;">1,230</div>
        </div>
        <div class="kpi-card purple" style="padding:10px 12px;">
          <div class="kpi-label">Langsir</div>
          <div class="kpi-value purple" style="font-size:22px;">393</div>
        </div>
      </div>
      <div class="sep"></div>
      <div style="font-size:10px;color:var(--text-muted);margin-bottom:10px;letter-spacing:.05em;text-transform:uppercase;">Hub Tujuan</div>
      <div class="hub-row"><div class="hub-name">Hub Malang</div><div class="hub-stats"><div class="hub-stat">LC Int: <strong>2</strong></div><div class="hub-stat">ETA: <strong>5:52</strong></div><div class="hub-stat">Ext: <strong>0</strong></div></div></div>
      <div class="hub-row"><div class="hub-name">Hub Kediri</div><div class="hub-stats"><div class="hub-stat">LC Int: <strong>1</strong></div><div class="hub-stat">ETA: <strong>4:54</strong></div><div class="hub-stat">Ext: <strong>0</strong></div></div></div>
      <div class="hub-row"><div class="hub-name">Hub Jember</div><div class="hub-stats"><div class="hub-stat">LC Int: <strong>0</strong></div><div class="hub-stat">ETA: <strong>—</strong></div><div class="hub-stat">Ext: <strong>0</strong></div></div></div>
      <div class="hub-row"><div class="hub-name">Hub Bali</div><div class="hub-stats"><div class="hub-stat">LC Int: <strong>0</strong></div><div class="hub-stat">Ext: <strong>8</strong></div></div></div>
    </div>
  </div>

  <!-- ROW 4: VENDOR BELUM DATANG + ISSUE LOG -->
  <div class="grid-35-65" style="margin-bottom:12px;">

    <!-- Vendor Belum Datang -->
    <div class="panel">
      <div class="panel-title">Vendor Belum Datang <span class="panel-badge badge-red">44 unit</span></div>
      <table class="data-table">
        <thead>
          <tr>
            <th>Stuffing</th>
            <th>Transport</th>
            <th>Vendor</th>
            <th class="num-cell">SI</th>
          </tr>
        </thead>
        <tbody>
          <tr><td>08:00</td><td>Sea</td><td>DUMMY</td><td class="num-cell">1</td></tr>
          <tr><td>08:00</td><td>Land</td><td>DUMMY-L</td><td class="num-cell">5</td></tr>
          <tr><td>08:00</td><td>Sea</td><td>WPS</td><td class="num-cell">1</td></tr>
          <tr><td>08:00</td><td>Sea</td><td>PT SML</td><td class="num-cell">1</td></tr>
          <tr><td>08:00</td><td>Land</td><td>LOGISLY</td><td class="num-cell">1</td></tr>
          <tr><td>08:00</td><td>Sea</td><td>KEMASINDO</td><td class="num-cell">2</td></tr>
          <tr><td>08:00</td><td>Land</td><td>PT.DANEX</td><td class="num-cell">3</td></tr>
          <tr><td>10:00</td><td>Sea</td><td>KEMASINDO</td><td class="num-cell">2</td></tr>
          <tr style="background:rgba(227,73,72,0.05);">
            <td style="font-weight:700;color:var(--text)" colspan="3">Grand Total</td>
            <td class="num-cell" style="font-weight:700;color:var(--accent-red)">44</td>
          </tr>
        </tbody>
      </table>
      <div class="sep"></div>
      <div style="font-size:10px;color:var(--text-muted);margin-bottom:8px;letter-spacing:.05em;text-transform:uppercase;">Special Remarks</div>
      <div style="display:flex;gap:10px;">
        <div class="kpi-card amber" style="flex:1;padding:10px 12px;">
          <div class="kpi-label">09:00–16:00</div>
          <div class="kpi-value amber" style="font-size:22px;">43</div>
        </div>
        <div class="kpi-card blue" style="flex:1;padding:10px 12px;">
          <div class="kpi-label">08:00–12:00</div>
          <div class="kpi-value blue" style="font-size:22px;">47</div>
        </div>
      </div>
    </div>

    <!-- Issue Log -->
    <div class="panel">
      <div class="panel-title">Issue Log & Quick Action</div>

      <div class="issue-row">
        <div class="issue-num">1</div>
        <div class="issue-text">Unit Internal NOPOL W8342QA — insiden tenggor POS LP, ada sobek di atap POS</div>
        <div class="issue-action">Koordinasi dengan Team HUB untuk progres perbaikan klaim</div>
        <div><span class="status-pill pill-process">On Progress</span></div>
      </div>

      <div class="issue-row">
        <div class="issue-num">2</div>
        <div class="issue-text">Penumpukan 2 armada external di pelabuhan Ketapang untuk tujuan Bali, 1 armada estimasi menyebrang malam</div>
        <div class="issue-action">Koordinasi dengan team Hub dan store untuk kondisi aktual posisi armada</div>
        <div><span class="status-pill pill-process">On Progress</span></div>
      </div>

      <div class="issue-row">
        <div class="issue-num">3</div>
        <div class="issue-text">Armada external stuffing 08:00 WIB — terlambat datang, ada 3 unit</div>
        <div class="issue-action">Reminder ke vendor datang sesuai stuffing</div>
        <div><span class="status-pill pill-done">Actioned</span></div>
      </div>

      <div class="issue-row">
        <div class="issue-num">4</div>
        <div class="issue-text">Vendor BS tujuan Bali dan MIF</div>
        <div class="issue-action">Monitoring posisi dan koordinasi vendor</div>
        <div><span class="status-pill pill-process">On Progress</span></div>
      </div>

      <div class="issue-row">
        <div class="issue-num">5</div>
        <div class="issue-text">Vendor OKE stuffing 08:00, armada baru bisa merapat ke DC estimasi jam 12 — kendala baru bisa menyebrang pelabuhan tadi malam</div>
        <div class="issue-action">Reminder ke vendor datang sesuai stuffing</div>
        <div><span class="status-pill pill-done">Actioned</span></div>
      </div>

    </div>
  </div>

  <!-- FOOTER -->
  <div style="text-align:center;padding:20px 0 10px;color:var(--text-muted);font-size:11px;border-top:1px solid var(--border);margin-top:10px;">
    NDC Sidoarjo — Transport Dashboard &nbsp;·&nbsp; Data diperbarui real-time dari sistem operasional
  </div>

</div>

<!-- SCRIPTS -->
<script>
document.getElementById('live-date').textContent = new Date().toLocaleDateString('id-ID', {day:'numeric',month:'short',year:'numeric'});

const darkColors = ['#3987e5','#9085e9','#5a5f72','#199e70','#eda100'];
const doughnutOpts = {
  responsive: true,
  maintainAspectRatio: true,
  cutout: '68%',
  plugins: { legend: { display: false }, tooltip: { callbacks: { label: ctx => ` ${ctx.label}: ${ctx.raw}` } } },
  animation: { animateRotate: true, duration: 900 }
};

new Chart(document.getElementById('chartInternal'), {
  type: 'doughnut',
  data: {
    labels: ['New','Checkin','Open','Close','Checkout'],
    datasets: [{ data: [63,21,9,20,86], backgroundColor: darkColors, borderWidth: 0, hoverOffset: 4 }]
  },
  options: doughnutOpts
});

new Chart(document.getElementById('chartExternal'), {
  type: 'doughnut',
  data: {
    labels: ['New','Open','Close','Checkout'],
    datasets: [{ data: [45,0,12,80], backgroundColor: ['#3987e5','#5a5f72','#199e70','#eda100'], borderWidth: 0, hoverOffset: 4 }]
  },
  options: doughnutOpts
});

new Chart(document.getElementById('chartTripType'), {
  type: 'bar',
  data: {
    labels: ['Customer','Store DK','Hub','Store LK','Others'],
    datasets: [{
      label: 'Trip',
      data: [133, 41, 12, 10, 3],
      backgroundColor: ['#3987e5','#199e70','#9085e9','#eda100','#5a5f72'],
      borderRadius: 4,
      borderSkipped: 'bottom'
    }]
  },
  options: {
    responsive: true,
    maintainAspectRatio: false,
    plugins: { legend: { display: false }, tooltip: { callbacks: { label: ctx => ` ${ctx.raw} unit` } } },
    scales: {
      x: { ticks: { color: '#8b90a0', font: { size: 10 } }, grid: { display: false }, border: { color: '#2c2c2a' } },
      y: { ticks: { color: '#8b90a0', font: { size: 10 }, stepSize: 30 }, grid: { color: '#1c2030' }, border: { color: '#2c2c2a' } }
    },
    animation: { duration: 900 }
  }
});
</script>
</body>
</html>
