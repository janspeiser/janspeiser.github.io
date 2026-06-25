# janspeiser.github.io<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Mechanische Schwingungen – Physik GK MV</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600&family=DM+Sans:wght@300;400;500;600&display=swap');

  :root {
    --bg: #0d1117;
    --surface: #161b22;
    --panel: #1c2230;
    --border: #2a3444;
    --c1: #4fc3f7;
    --c2: #f06292;
    --csum: #a5d6a7;
    --cv1: #80deea;
    --cv2: #f48fb1;
    --accent: #ffd54f;
    --text: #e6edf3;
    --muted: #7d8590;
    --grid: rgba(255,255,255,0.04);
    --axis: rgba(255,255,255,0.12);
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'DM Sans', sans-serif;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
  }

  header {
    background: var(--surface);
    border-bottom: 1px solid var(--border);
    padding: 14px 24px;
    display: flex;
    align-items: center;
    gap: 16px;
  }

  header h1 {
    font-size: 1rem;
    font-weight: 600;
    letter-spacing: 0.02em;
    color: var(--text);
  }

  header .badge {
    font-size: 0.7rem;
    background: rgba(79,195,247,0.15);
    color: var(--c1);
    border: 1px solid rgba(79,195,247,0.3);
    border-radius: 20px;
    padding: 2px 10px;
    font-weight: 500;
    letter-spacing: 0.05em;
  }

  .app {
    display: grid;
    grid-template-columns: 300px 1fr;
    flex: 1;
    overflow: hidden;
    height: calc(100vh - 53px);
  }

  /* ── LEFT PANEL ── */
  .left-panel {
    background: var(--surface);
    border-right: 1px solid var(--border);
    overflow-y: auto;
    display: flex;
    flex-direction: column;
    gap: 0;
  }

  .section {
    border-bottom: 1px solid var(--border);
    padding: 16px;
  }

  .section-title {
    font-size: 0.65rem;
    font-weight: 600;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 14px;
  }

  .slider-group {
    margin-bottom: 14px;
  }

  .slider-label {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    margin-bottom: 5px;
  }

  .slider-name {
    font-size: 0.82rem;
    color: var(--text);
  }

  .slider-value {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.8rem;
    font-weight: 600;
  }

  input[type="range"] {
    -webkit-appearance: none;
    width: 100%;
    height: 4px;
    border-radius: 2px;
    outline: none;
    cursor: pointer;
    transition: opacity 0.2s;
  }

  input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 14px;
    height: 14px;
    border-radius: 50%;
    cursor: pointer;
    transition: transform 0.15s;
  }

  input[type="range"]:hover::-webkit-slider-thumb { transform: scale(1.3); }

  .s1 input[type="range"] { background: linear-gradient(to right, var(--c1) 0%, var(--c1) var(--pct,50%), var(--border) var(--pct,50%)); }
  .s1 input[type="range"]::-webkit-slider-thumb { background: var(--c1); }
  .s1 .slider-value { color: var(--c1); }

  .s2 input[type="range"] { background: linear-gradient(to right, var(--c2) 0%, var(--c2) var(--pct,50%), var(--border) var(--pct,50%)); }
  .s2 input[type="range"]::-webkit-slider-thumb { background: var(--c2); }
  .s2 .slider-value { color: var(--c2); }

  .sp input[type="range"] { background: linear-gradient(to right, var(--accent) 0%, var(--accent) var(--pct,50%), var(--border) var(--pct,50%)); }
  .sp input[type="range"]::-webkit-slider-thumb { background: var(--accent); }
  .sp .slider-value { color: var(--accent); }

  /* ── EQUATION BOX ── */
  .eq-box {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 12px;
    margin-top: 4px;
  }

  .eq-row {
    display: flex;
    flex-direction: column;
    gap: 6px;
    margin-bottom: 8px;
  }

  .eq-row:last-child { margin-bottom: 0; }

  .eq-label {
    font-size: 0.67rem;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    color: var(--muted);
  }

  .eq-formula {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.82rem;
    color: var(--text);
    line-height: 1.4;
  }

  /* ── GRUNDGRÖSSEN ── */
  .metrics-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 8px;
  }

  .metric {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 10px;
  }

  .metric-label {
    font-size: 0.67rem;
    color: var(--muted);
    letter-spacing: 0.06em;
    text-transform: uppercase;
    margin-bottom: 4px;
  }

  .metric-symbol {
    font-size: 0.7rem;
    color: var(--muted);
  }

  .metric-value {
    font-family: 'JetBrains Mono', monospace;
    font-size: 1rem;
    font-weight: 600;
  }

  .metric-unit {
    font-size: 0.7rem;
    color: var(--muted);
    margin-left: 2px;
  }

  /* ── LEGEND ── */
  .legend {
    display: flex;
    flex-direction: column;
    gap: 7px;
  }

  .legend-item {
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 0.8rem;
  }

  .legend-dot {
    width: 24px;
    height: 3px;
    border-radius: 2px;
    flex-shrink: 0;
  }

  .legend-dot.dashed {
    background: repeating-linear-gradient(90deg, currentColor 0, currentColor 5px, transparent 5px, transparent 9px);
  }

  /* ── TOGGLE BUTTONS ── */
  .toggle-row {
    display: flex;
    gap: 6px;
    flex-wrap: wrap;
  }

  .toggle-btn {
    font-family: 'DM Sans', sans-serif;
    font-size: 0.72rem;
    font-weight: 500;
    padding: 5px 11px;
    border-radius: 20px;
    border: 1px solid var(--border);
    background: var(--panel);
    color: var(--muted);
    cursor: pointer;
    transition: all 0.2s;
    letter-spacing: 0.02em;
  }

  .toggle-btn.active {
    background: rgba(79,195,247,0.12);
    border-color: var(--c1);
    color: var(--c1);
  }

  .toggle-btn.active.c2 {
    background: rgba(240,98,146,0.12);
    border-color: var(--c2);
    color: var(--c2);
  }

  .toggle-btn.active.csum {
    background: rgba(165,214,167,0.12);
    border-color: var(--csum);
    color: var(--csum);
  }

  /* ── RIGHT: CANVAS AREA ── */
  .right-panel {
    display: flex;
    flex-direction: column;
    overflow: hidden;
  }

  .canvas-wrapper {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 0;
    overflow: hidden;
    padding: 16px;
    gap: 12px;
  }

  .chart-area {
    flex: 1;
    position: relative;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    overflow: hidden;
    min-height: 0;
  }

  .chart-label {
    position: absolute;
    top: 10px;
    left: 14px;
    font-size: 0.68rem;
    font-weight: 600;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--muted);
    z-index: 2;
    pointer-events: none;
  }

  canvas {
    width: 100%;
    height: 100%;
    display: block;
  }

  /* ── ANIMATION INDICATOR ── */
  .time-indicator {
    position: absolute;
    top: 10px;
    right: 14px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.72rem;
    color: var(--muted);
    z-index: 2;
  }

  /* ── PLAY CONTROLS ── */
  .controls-bar {
    background: var(--surface);
    border-top: 1px solid var(--border);
    padding: 10px 20px;
    display: flex;
    align-items: center;
    gap: 14px;
  }

  .play-btn {
    width: 36px;
    height: 36px;
    border-radius: 50%;
    border: 1px solid var(--border);
    background: var(--panel);
    color: var(--text);
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.2s;
    font-size: 1rem;
  }

  .play-btn:hover {
    background: rgba(79,195,247,0.15);
    border-color: var(--c1);
    color: var(--c1);
  }

  .speed-label {
    font-size: 0.75rem;
    color: var(--muted);
  }

  .speed-input {
    -webkit-appearance: none;
    width: 80px;
    height: 3px;
    border-radius: 2px;
    background: linear-gradient(to right, var(--c1) 0%, var(--c1) 50%, var(--border) 50%);
    cursor: pointer;
  }

  .speed-input::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 12px;
    height: 12px;
    border-radius: 50%;
    background: var(--c1);
    cursor: pointer;
  }

  .phase-big {
    margin-left: auto;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.8rem;
    color: var(--accent);
    background: rgba(255,213,79,0.08);
    border: 1px solid rgba(255,213,79,0.2);
    border-radius: 6px;
    padding: 6px 12px;
  }

  .scrollbar-thin::-webkit-scrollbar { width: 4px; }
  .scrollbar-thin::-webkit-scrollbar-track { background: transparent; }
  .scrollbar-thin::-webkit-scrollbar-thumb { background: var(--border); border-radius: 2px; }
</style>
</head>
<body>

<header>
  <h1>Mechanische Schwingungen</h1>
  <span class="badge">Physik GK · MV</span>
</header>

<div class="app">

  <!-- LEFT PANEL -->
  <div class="left-panel scrollbar-thin">

    <!-- Schwingung 1 -->
    <div class="section">
      <div class="section-title">Schwingung 1</div>

      <div class="slider-group s1">
        <div class="slider-label">
          <span class="slider-name">Amplitude <i>A₁</i></span>
          <span class="slider-value" id="lbl-A1">1.0 m</span>
        </div>
        <input type="range" id="sl-A1" min="0.1" max="2" step="0.05" value="1">
      </div>

      <div class="slider-group s1">
        <div class="slider-label">
          <span class="slider-name">Frequenz <i>f₁</i></span>
          <span class="slider-value" id="lbl-f1">1.0 Hz</span>
        </div>
        <input type="range" id="sl-f1" min="0.2" max="3" step="0.05" value="1">
      </div>
    </div>

    <!-- Schwingung 2 -->
    <div class="section">
      <div class="section-title">Schwingung 2</div>

      <div class="slider-group s2">
        <div class="slider-label">
          <span class="slider-name">Amplitude <i>A₂</i></span>
          <span class="slider-value" id="lbl-A2">1.0 m</span>
        </div>
        <input type="range" id="sl-A2" min="0.1" max="2" step="0.05" value="1">
      </div>

      <div class="slider-group s2">
        <div class="slider-label">
          <span class="slider-name">Frequenz <i>f₂</i></span>
          <span class="slider-value" id="lbl-f2">1.0 Hz</span>
        </div>
        <input type="range" id="sl-f2" min="0.2" max="3" step="0.05" value="1">
      </div>
    </div>

    <!-- Phasenunterschied -->
    <div class="section">
      <div class="section-title">Phasenunterschied</div>

      <div class="slider-group sp">
        <div class="slider-label">
          <span class="slider-name">Δφ</span>
          <span class="slider-value" id="lbl-phi">0.00 rad (0°)</span>
        </div>
        <input type="range" id="sl-phi" min="0" max="6.2832" step="0.0628" value="0">
      </div>

      <div class="eq-box" style="margin-top:8px; padding:8px 12px;">
        <div style="font-size:0.72rem; color:var(--muted); margin-bottom:4px;">Phasenbeziehung</div>
        <div class="eq-formula" id="phase-text" style="color:var(--accent); font-size:0.78rem;">Gleichphasig (Δφ = 0)</div>
      </div>
    </div>

    <!-- Sichtbarkeit -->
    <div class="section">
      <div class="section-title">Anzeige</div>
      <div class="toggle-row">
        <button class="toggle-btn active" id="btn-s1" onclick="toggleVis('s1',this)">S₁ (x–t)</button>
        <button class="toggle-btn active c2" id="btn-s2" onclick="toggleVis('s2',this)">S₂ (x–t)</button>
        <button class="toggle-btn active csum" id="btn-ss" onclick="toggleVis('ss',this)">Superpos.</button>
        <button class="toggle-btn active" id="btn-v1" onclick="toggleVis('v1',this)">S₁ (v–t)</button>
        <button class="toggle-btn active c2" id="btn-v2" onclick="toggleVis('v2',this)">S₂ (v–t)</button>
      </div>
    </div>

    <!-- x/v Vergleich -->
    <div class="section">
      <div class="section-title">x(t) &amp; v(t) Vergleich</div>
      <div style="font-size:0.76rem; color:var(--muted); margin-bottom:10px; line-height:1.5;">
        Zeigt x₁ und v₁ normiert übereinander – der Phasenversatz von <span style="color:var(--accent); font-weight:600;">T/4</span> wird direkt sichtbar.
      </div>
      <div class="toggle-row">
        <button class="toggle-btn" id="btn-ovl" onclick="toggleOverlay(this)">Vergleichsdiagramm</button>
        <button class="toggle-btn" id="btn-t4"  onclick="toggleT4(this)">T/4 markieren</button>
      </div>
    </div>

    <!-- Schwingungsgleichungen -->
    <div class="section">
      <div class="section-title">Schwingungsgleichungen</div>
      <div class="eq-box">
        <div class="eq-row">
          <div class="eq-label" style="color:var(--c1)">Schwingung 1 – Ort</div>
          <div class="eq-formula" id="eq-x1"></div>
        </div>
        <div class="eq-row">
          <div class="eq-label" style="color:var(--cv1)">Schwingung 1 – Geschwindigkeit</div>
          <div class="eq-formula" id="eq-v1"></div>
        </div>
        <div class="eq-row" style="margin-top:8px; border-top:1px solid var(--border); padding-top:8px;">
          <div class="eq-label" style="color:var(--c2)">Schwingung 2 – Ort</div>
          <div class="eq-formula" id="eq-x2"></div>
        </div>
        <div class="eq-row">
          <div class="eq-label" style="color:var(--cv2)">Schwingung 2 – Geschwindigkeit</div>
          <div class="eq-formula" id="eq-v2"></div>
        </div>
      </div>
    </div>

    <!-- Grundgrößen -->
    <div class="section">
      <div class="section-title">Grundgrößen – Schwingung 1</div>
      <div class="metrics-grid">
        <div class="metric">
          <div class="metric-label">Amplitude</div>
          <div><span class="metric-value" id="mg-A1" style="color:var(--c1)">1.00</span><span class="metric-unit">m</span></div>
          <div class="metric-symbol">A₁</div>
        </div>
        <div class="metric">
          <div class="metric-label">Frequenz</div>
          <div><span class="metric-value" id="mg-f1" style="color:var(--c1)">1.00</span><span class="metric-unit">Hz</span></div>
          <div class="metric-symbol">f₁</div>
        </div>
        <div class="metric">
          <div class="metric-label">Periodendauer</div>
          <div><span class="metric-value" id="mg-T1" style="color:var(--c1)">1.00</span><span class="metric-unit">s</span></div>
          <div class="metric-symbol">T₁ = 1/f₁</div>
        </div>
        <div class="metric">
          <div class="metric-label">Kreisfrequenz</div>
          <div><span class="metric-value" id="mg-w1" style="color:var(--c1)">6.28</span><span class="metric-unit">rad/s</span></div>
          <div class="metric-symbol">ω₁ = 2πf₁</div>
        </div>
        <div class="metric">
          <div class="metric-label">v_max</div>
          <div><span class="metric-value" id="mg-vm1" style="color:var(--cv1)">6.28</span><span class="metric-unit">m/s</span></div>
          <div class="metric-symbol">v₁_max = ω₁·A₁</div>
        </div>
      </div>
    </div>

    <!-- Legende -->
    <div class="section">
      <div class="section-title">Legende</div>
      <div class="legend">
        <div class="legend-item"><div class="legend-dot" style="background:var(--c1)"></div>x₁(t) – Ort Schwingung 1</div>
        <div class="legend-item"><div class="legend-dot" style="background:var(--c2)"></div>x₂(t) – Ort Schwingung 2</div>
        <div class="legend-item"><div class="legend-dot" style="background:var(--csum)"></div>x_ges(t) – Superposition</div>
        <div class="legend-item"><div class="legend-dot dashed" style="color:var(--cv1)"></div>v₁(t) – Geschwindigkeit 1</div>
        <div class="legend-item"><div class="legend-dot dashed" style="color:var(--cv2)"></div>v₂(t) – Geschwindigkeit 2</div>
        <div style="margin-top:6px; padding-top:6px; border-top:1px solid var(--border); font-size:0.74rem; color:var(--muted);">Vergleichsdiagramm (normiert)</div>
        <div class="legend-item"><div class="legend-dot" style="background:var(--c1)"></div>x₁(t)/A₁ – Ort normiert</div>
        <div class="legend-item"><div class="legend-dot dashed" style="color:var(--cv1)"></div>v₁(t)/v₁_max – Gesch. normiert</div>
      </div>
    </div>

  </div><!-- /left-panel -->

  <!-- RIGHT PANEL -->
  <div class="right-panel">
    <div class="canvas-wrapper">

      <!-- Zeit-Ort Diagramm -->
      <div class="chart-area" style="flex:3">
        <div class="chart-label">Zeit–Ort-Diagramm &nbsp; x(t)</div>
        <div class="time-indicator" id="t-ind">t = 0.00 s</div>
        <canvas id="cvs-xt"></canvas>
      </div>

      <!-- Zeit-Geschwindigkeit Diagramm -->
      <div class="chart-area" style="flex:2">
        <div class="chart-label">Zeit–Geschwindigkeit-Diagramm &nbsp; v(t)</div>
        <canvas id="cvs-vt"></canvas>
      </div>

      <!-- Overlay: x₁ und v₁ normiert (T/4-Vergleich) -->
      <div class="chart-area" id="ovl-area" style="flex:2.5">
        <div class="chart-label">Vergleich x₁(t) &amp; v₁(t) &nbsp;–&nbsp; normiert</div>
        <canvas id="cvs-ovl"></canvas>
      </div>

    </div>

    <!-- Controls -->
    <div class="controls-bar">
      <button class="play-btn" id="play-btn" onclick="togglePlay()">▶</button>
      <button class="play-btn" onclick="resetTime()" title="Zurücksetzen">⟳</button>
      <span class="speed-label">Geschwindigkeit</span>
      <input type="range" class="speed-input" id="speed-sl" min="0.1" max="3" step="0.1" value="1" oninput="updateSpeed(this)">
      <span id="speed-val" style="font-family:'JetBrains Mono',monospace; font-size:0.75rem; color:var(--c1); min-width:32px;">×1.0</span>

      <div class="phase-big" id="phase-big">Δφ = 0.00 rad</div>
    </div>
  </div>

</div><!-- /app -->

<script>
// ── STATE ──
const state = {
  A1: 1, f1: 1,
  A2: 1, f2: 1,
  phi: 0,
  t: 0,
  playing: false,
  speed: 1,
  showOverlay: false,   // combined x+v overlay for S1
  showT4: false,        // T/4 annotation
  vis: { s1: true, s2: true, ss: true, v1: true, v2: true }
};

let lastTime = null;
let rafId = null;

// ── CANVAS SETUP ──
const cvsXT  = document.getElementById('cvs-xt');
const cvsVT  = document.getElementById('cvs-vt');
const cvsOVL = document.getElementById('cvs-ovl');
const ctxXT  = cvsXT.getContext('2d');
const ctxVT  = cvsVT.getContext('2d');
const ctxOVL = cvsOVL.getContext('2d');

function resizeCanvases() {
  [cvsXT, cvsVT, cvsOVL].forEach(c => {
    c.width  = c.parentElement.clientWidth  * devicePixelRatio;
    c.height = c.parentElement.clientHeight * devicePixelRatio;
    c.style.width  = c.parentElement.clientWidth  + 'px';
    c.style.height = c.parentElement.clientHeight + 'px';
  });
  draw();
}
window.addEventListener('resize', resizeCanvases);

// ── SLIDERS ──
function setupSliders() {
  function bind(id, setter) {
    const el = document.getElementById(id);
    el.addEventListener('input', () => { setter(parseFloat(el.value)); updateSliderFill(el); updateUI(); draw(); });
    updateSliderFill(el);
  }
  bind('sl-A1', v => { state.A1 = v; document.getElementById('lbl-A1').textContent = v.toFixed(2) + ' m'; });
  bind('sl-f1', v => { state.f1 = v; document.getElementById('lbl-f1').textContent = v.toFixed(2) + ' Hz'; });
  bind('sl-A2', v => { state.A2 = v; document.getElementById('lbl-A2').textContent = v.toFixed(2) + ' m'; });
  bind('sl-f2', v => { state.f2 = v; document.getElementById('lbl-f2').textContent = v.toFixed(2) + ' Hz'; });
  bind('sl-phi', v => {
    state.phi = v;
    document.getElementById('lbl-phi').textContent = v.toFixed(2) + ' rad (' + (v*180/Math.PI).toFixed(0) + '°)';
  });
}

function updateSliderFill(el) {
  const min = parseFloat(el.min), max = parseFloat(el.max), val = parseFloat(el.value);
  el.style.setProperty('--pct', ((val - min) / (max - min) * 100) + '%');
}

// ── FORMULAS ──
function fmtNum(v) { return v.toFixed(2); }

function updateUI() {
  const { A1, f1, A2, f2, phi } = state;
  const w1 = 2 * Math.PI * f1, w2 = 2 * Math.PI * f2;
  const T1 = 1 / f1;

  document.getElementById('mg-A1').textContent = fmtNum(A1);
  document.getElementById('mg-f1').textContent = fmtNum(f1);
  document.getElementById('mg-T1').textContent = fmtNum(T1);
  document.getElementById('mg-w1').textContent = fmtNum(w1);
  document.getElementById('mg-vm1').textContent = fmtNum(w1 * A1);

  const phiDeg = phi * 180 / Math.PI;
  let phTxt = '';
  if (phi < 0.01) phTxt = 'Gleichphasig (Δφ = 0)';
  else if (Math.abs(phi - Math.PI) < 0.1) phTxt = 'Gegenphasig (Δφ = π)';
  else if (Math.abs(phi - Math.PI/2) < 0.1) phTxt = 'Phasenversatz π/2 (90°)';
  else phTxt = `Δφ = ${fmtNum(phi)} rad = ${phiDeg.toFixed(0)}°`;
  document.getElementById('phase-text').textContent = phTxt;
  document.getElementById('phase-big').textContent = `Δφ = ${fmtNum(phi)} rad`;

  // ── Equations with proper units on every quantity ──
  const phiStr = phi < 0.01 ? '' : ` + ${fmtNum(phi)} rad`;
  const w1s = fmtNum(w1), w2s = fmtNum(w2);
  // x1: x₁(t) = A₁ · sin(ω₁ · t)  with units shown inline
  document.getElementById('eq-x1').innerHTML =
    `x<sub>1</sub>(t) = <span style="color:#4fc3f7">${fmtNum(A1)} m</span> · sin(<span style="color:#4fc3f7">${w1s} rad/s</span> · t)`;
  document.getElementById('eq-v1').innerHTML =
    `v<sub>1</sub>(t) = <span style="color:#80deea">${fmtNum(w1*A1)} m/s</span> · cos(<span style="color:#80deea">${w1s} rad/s</span> · t)`;
  document.getElementById('eq-x2').innerHTML =
    `x<sub>2</sub>(t) = <span style="color:#f06292">${fmtNum(A2)} m</span> · sin(<span style="color:#f06292">${w2s} rad/s</span> · t${phi < 0.01 ? '' : ` + <span style="color:#ffd54f">${fmtNum(phi)} rad</span>`})`;
  document.getElementById('eq-v2').innerHTML =
    `v<sub>2</sub>(t) = <span style="color:#f48fb1">${fmtNum(w2*A2)} m/s</span> · cos(<span style="color:#f48fb1">${w2s} rad/s</span> · t${phi < 0.01 ? '' : ` + <span style="color:#ffd54f">${fmtNum(phi)} rad</span>`})`;
}

// ── OSCILLATOR FUNCTIONS ──
function x1(t) { return state.A1 * Math.sin(2 * Math.PI * state.f1 * t); }
function x2(t) { return state.A2 * Math.sin(2 * Math.PI * state.f2 * t + state.phi); }
function xsum(t) { return x1(t) + x2(t); }
function v1(t) { const w=2*Math.PI*state.f1; return state.A1*w*Math.cos(w*t); }
function v2(t) { const w=2*Math.PI*state.f2; return state.A2*w*Math.cos(w*t+state.phi); }
// normalised v1 (amplitude = 1) for overlay
function v1norm(t) { const w=2*Math.PI*state.f1; return Math.cos(w*t); }

// ── CORE DRAW GRAPH ──
function drawGraph(ctx, canvas, funcs, tWindow, yMax, colors, dashes, showTimeMarker, yLabel) {
  const W = canvas.width, H = canvas.height;
  const pad = { l: 56, r: 24, t: 36, b: 40 };
  const gW = W - pad.l - pad.r;
  const gH = H - pad.t - pad.b;

  ctx.clearRect(0, 0, W, H);

  const toX = rel => pad.l + rel * gW;          // rel in [0,1]
  const toY = v   => pad.t + (1 - (v / yMax + 1) / 2) * gH;
  const tStart = state.t - tWindow / 2;

  // ── Grid ──
  ctx.save();
  ctx.strokeStyle = 'rgba(255,255,255,0.04)';
  ctx.lineWidth = 1;
  for (let i = 0; i <= 10; i++) {
    const x = toX(i/10);
    ctx.beginPath(); ctx.moveTo(x, pad.t); ctx.lineTo(x, pad.t+gH); ctx.stroke();
  }
  for (let i = 0; i <= 4; i++) {
    const y = pad.t + (i/4)*gH;
    ctx.beginPath(); ctx.moveTo(pad.l, y); ctx.lineTo(pad.l+gW, y); ctx.stroke();
  }
  ctx.restore();

  // ── Axes ──
  ctx.save();
  ctx.strokeStyle = 'rgba(255,255,255,0.18)';
  ctx.lineWidth = 1.5;
  ctx.beginPath(); ctx.moveTo(pad.l, toY(0)); ctx.lineTo(pad.l+gW, toY(0)); ctx.stroke();
  ctx.beginPath(); ctx.moveTo(pad.l, pad.t);  ctx.lineTo(pad.l, pad.t+gH);  ctx.stroke();
  ctx.restore();

  // ── Y-labels ──
  ctx.save();
  ctx.fillStyle = 'rgba(125,133,144,0.9)';
  ctx.font = `${9.5*devicePixelRatio}px JetBrains Mono`;
  ctx.textAlign = 'right';
  ctx.fillText(`+${yMax.toFixed(1)}`, pad.l-6, pad.t+10);
  ctx.fillText('0',                    pad.l-6, toY(0)+4);
  ctx.fillText(`-${yMax.toFixed(1)}`, pad.l-6, pad.t+gH);
  ctx.restore();

  // ── X-labels ──
  ctx.save();
  ctx.fillStyle = 'rgba(125,133,144,0.9)';
  ctx.font = `${9*devicePixelRatio}px JetBrains Mono`;
  ctx.textAlign = 'center';
  for (let i = 0; i <= 5; i++) {
    const tVal = tStart + (i/5)*tWindow;
    ctx.fillText(tVal.toFixed(1)+'s', toX(i/5), pad.t+gH+15*devicePixelRatio);
  }
  ctx.restore();

  // ── Axis labels ──
  ctx.save();
  ctx.fillStyle = 'rgba(125,133,144,0.75)';
  ctx.font = `italic ${11*devicePixelRatio}px DM Sans`;
  ctx.textAlign = 'left';
  ctx.fillText('t', pad.l+gW+5, toY(0)+4);
  ctx.textAlign = 'center';
  ctx.fillText(yLabel || 'y', pad.l, pad.t-10);
  ctx.restore();

  // ── Curves ──
  const steps = Math.min(gW * 1.5, 1200);
  funcs.forEach((fn, i) => {
    if (!fn) return;
    ctx.save();
    ctx.strokeStyle = colors[i];
    ctx.lineWidth = 2.5 * devicePixelRatio;
    ctx.lineJoin = 'round';
    if (dashes && dashes[i]) ctx.setLineDash([8*devicePixelRatio, 5*devicePixelRatio]);
    ctx.beginPath();
    for (let s = 0; s <= steps; s++) {
      const t = tStart + (s/steps)*tWindow;
      const cx = toX((t-tStart)/tWindow);
      const cy = toY(fn(t));
      s === 0 ? ctx.moveTo(cx,cy) : ctx.lineTo(cx,cy);
    }
    ctx.stroke();
    ctx.restore();
  });

  // ── Time marker ──
  if (showTimeMarker) {
    ctx.save();
    ctx.strokeStyle = 'rgba(255,213,79,0.65)';
    ctx.lineWidth = 2 * devicePixelRatio;
    ctx.setLineDash([6*devicePixelRatio, 4*devicePixelRatio]);
    const mx = toX(0.5);
    ctx.beginPath(); ctx.moveTo(mx, pad.t-4); ctx.lineTo(mx, pad.t+gH+4); ctx.stroke();
    ctx.restore();
  }

  return { pad, gW, gH, toX, toY, tStart, tWindow, steps };
}

// ── OVERLAY DIAGRAM (x₁ normalised + v₁ normalised + T/4 annotation) ──
function drawOverlay() {
  const canvas = cvsOVL, ctx = ctxOVL;
  const W = canvas.width, H = canvas.height;
  const pad = { l: 56, r: 24, t: 36, b: 40 };
  const gW = W - pad.l - pad.r;
  const gH = H - pad.t - pad.b;

  ctx.clearRect(0, 0, W, H);

  const { f1 } = state;
  const T1 = 1 / f1;
  // show 2 full periods centred on t
  const tWindow = Math.max(2 * T1, 3);
  const tStart  = state.t - tWindow / 2;

  const toX = rel => pad.l + rel * gW;
  const toY = v   => pad.t + (1 - (v + 1) / 2) * gH;  // yMax = 1 (normalised)

  // Grid
  ctx.save();
  ctx.strokeStyle = 'rgba(255,255,255,0.04)';
  ctx.lineWidth = 1;
  for (let i = 0; i <= 10; i++) { const x=toX(i/10); ctx.beginPath(); ctx.moveTo(x,pad.t); ctx.lineTo(x,pad.t+gH); ctx.stroke(); }
  for (let i = 0; i <= 4; i++) { const y=pad.t+(i/4)*gH; ctx.beginPath(); ctx.moveTo(pad.l,y); ctx.lineTo(pad.l+gW,y); ctx.stroke(); }
  ctx.restore();

  // Axes
  ctx.save();
  ctx.strokeStyle = 'rgba(255,255,255,0.18)';
  ctx.lineWidth = 1.5;
  ctx.beginPath(); ctx.moveTo(pad.l, toY(0)); ctx.lineTo(pad.l+gW, toY(0)); ctx.stroke();
  ctx.beginPath(); ctx.moveTo(pad.l, pad.t);  ctx.lineTo(pad.l, pad.t+gH);  ctx.stroke();
  ctx.restore();

  // Y-labels
  ctx.save();
  ctx.fillStyle = 'rgba(125,133,144,0.9)';
  ctx.font = `${9.5*devicePixelRatio}px JetBrains Mono`;
  ctx.textAlign = 'right';
  ctx.fillText('+1', pad.l-6, pad.t+10);
  ctx.fillText('0',  pad.l-6, toY(0)+4);
  ctx.fillText('-1', pad.l-6, pad.t+gH);
  ctx.restore();

  // X-labels
  ctx.save();
  ctx.fillStyle = 'rgba(125,133,144,0.9)';
  ctx.font = `${9*devicePixelRatio}px JetBrains Mono`;
  ctx.textAlign = 'center';
  for (let i = 0; i <= 5; i++) {
    const tVal = tStart + (i/5)*tWindow;
    ctx.fillText(tVal.toFixed(2)+'s', toX(i/5), pad.t+gH+15*devicePixelRatio);
  }
  ctx.restore();

  // Axis labels
  ctx.save();
  ctx.fillStyle = 'rgba(125,133,144,0.7)';
  ctx.font = `italic ${11*devicePixelRatio}px DM Sans`;
  ctx.textAlign = 'left';
  ctx.fillText('t', pad.l+gW+5, toY(0)+4);
  ctx.textAlign = 'center';
  ctx.fillText('norm.', pad.l, pad.t-10);
  ctx.restore();

  const steps = Math.min(gW*1.5, 1200);

  // Plot x1 normalised (sin)
  ctx.save();
  ctx.strokeStyle = '#4fc3f7';
  ctx.lineWidth = 2.5 * devicePixelRatio;
  ctx.lineJoin = 'round';
  ctx.beginPath();
  for (let s = 0; s <= steps; s++) {
    const t = tStart + (s/steps)*tWindow;
    const cx = toX((t-tStart)/tWindow);
    const cy = toY(Math.sin(2*Math.PI*f1*t));
    s === 0 ? ctx.moveTo(cx,cy) : ctx.lineTo(cx,cy);
  }
  ctx.stroke();
  ctx.restore();

  // Plot v1 normalised (cos) – dashed
  ctx.save();
  ctx.strokeStyle = '#80deea';
  ctx.lineWidth = 2.5 * devicePixelRatio;
  ctx.lineJoin = 'round';
  ctx.setLineDash([8*devicePixelRatio, 5*devicePixelRatio]);
  ctx.beginPath();
  for (let s = 0; s <= steps; s++) {
    const t = tStart + (s/steps)*tWindow;
    const cx = toX((t-tStart)/tWindow);
    const cy = toY(Math.cos(2*Math.PI*f1*t));
    s === 0 ? ctx.moveTo(cx,cy) : ctx.lineTo(cx,cy);
  }
  ctx.stroke();
  ctx.restore();

  // ── T/4 annotation ──
  if (state.showT4) {
    // Find a zero-crossing of sin (x goes through 0 upward) within visible range
    // sin = 0 at t = n/f1; find the one closest to tStart + tWindow*0.2
    const tRef = Math.ceil((tStart + tWindow*0.15) * f1) / f1;
    // At tRef, sin = 0 ascending → cos = 1 (max of v)
    const xRef  = toX((tRef - tStart) / tWindow);
    const xRef4 = toX((tRef + T1/4 - tStart) / tWindow);

    if (xRef >= pad.l && xRef4 <= pad.l+gW) {
      // Shaded region
      ctx.save();
      ctx.fillStyle = 'rgba(255,213,79,0.07)';
      ctx.fillRect(xRef, pad.t, xRef4 - xRef, gH);
      ctx.restore();

      // Bracket lines at tRef and tRef + T/4
      ctx.save();
      ctx.strokeStyle = 'rgba(255,213,79,0.5)';
      ctx.lineWidth = 1.5 * devicePixelRatio;
      ctx.setLineDash([5*devicePixelRatio, 4*devicePixelRatio]);
      [xRef, xRef4].forEach(x => {
        ctx.beginPath(); ctx.moveTo(x, pad.t); ctx.lineTo(x, pad.t+gH); ctx.stroke();
      });
      ctx.restore();

      // Horizontal double-arrow between the two lines at mid-height
      const arrowY = pad.t + gH * 0.12;
      ctx.save();
      ctx.strokeStyle = '#ffd54f';
      ctx.lineWidth = 1.8 * devicePixelRatio;
      ctx.setLineDash([]);
      ctx.beginPath();
      ctx.moveTo(xRef + 6*devicePixelRatio, arrowY);
      ctx.lineTo(xRef4 - 6*devicePixelRatio, arrowY);
      ctx.stroke();
      // Arrowheads
      const ah = 5 * devicePixelRatio;
      ctx.beginPath();
      ctx.moveTo(xRef + 6*devicePixelRatio, arrowY);
      ctx.lineTo(xRef + 6*devicePixelRatio + ah, arrowY - ah);
      ctx.lineTo(xRef + 6*devicePixelRatio + ah, arrowY + ah);
      ctx.closePath();
      ctx.fillStyle = '#ffd54f';
      ctx.fill();
      ctx.beginPath();
      ctx.moveTo(xRef4 - 6*devicePixelRatio, arrowY);
      ctx.lineTo(xRef4 - 6*devicePixelRatio - ah, arrowY - ah);
      ctx.lineTo(xRef4 - 6*devicePixelRatio - ah, arrowY + ah);
      ctx.closePath();
      ctx.fill();
      ctx.restore();

      // Label "T/4"
      ctx.save();
      ctx.fillStyle = '#ffd54f';
      ctx.font = `bold ${11*devicePixelRatio}px JetBrains Mono`;
      ctx.textAlign = 'center';
      ctx.fillText('T/4', (xRef + xRef4)/2, arrowY - 8*devicePixelRatio);
      ctx.restore();

      // Annotation: where x = 0 mark
      ctx.save();
      ctx.fillStyle = '#4fc3f7';
      ctx.font = `${9*devicePixelRatio}px JetBrains Mono`;
      ctx.textAlign = 'center';
      ctx.fillText('x₁ = 0', xRef, pad.t + gH*0.55 + 12*devicePixelRatio);
      ctx.restore();

      // Annotation: where v = max
      ctx.save();
      ctx.fillStyle = '#80deea';
      ctx.font = `${9*devicePixelRatio}px JetBrains Mono`;
      ctx.textAlign = 'center';
      ctx.fillText('v₁ = v_max', xRef, toY(1) - 7*devicePixelRatio);
      ctx.restore();

      // Annotation: where x = max (tRef + T/4)
      ctx.save();
      ctx.fillStyle = '#4fc3f7';
      ctx.font = `${9*devicePixelRatio}px JetBrains Mono`;
      ctx.textAlign = 'center';
      ctx.fillText('x₁ = A', xRef4, toY(1) - 7*devicePixelRatio);
      ctx.restore();

      // Annotation: where v = 0 at tRef + T/4
      ctx.save();
      ctx.fillStyle = '#80deea';
      ctx.font = `${9*devicePixelRatio}px JetBrains Mono`;
      ctx.textAlign = 'center';
      ctx.fillText('v₁ = 0', xRef4, pad.t + gH*0.55 + 12*devicePixelRatio);
      ctx.restore();
    }
  }

  // Time marker
  ctx.save();
  ctx.strokeStyle = 'rgba(255,213,79,0.65)';
  ctx.lineWidth = 2 * devicePixelRatio;
  ctx.setLineDash([6*devicePixelRatio, 4*devicePixelRatio]);
  const mx = toX(0.5);
  ctx.beginPath(); ctx.moveTo(mx, pad.t-4); ctx.lineTo(mx, pad.t+gH+4); ctx.stroke();
  ctx.restore();

  // Legend inside overlay
  const lx = pad.l + gW - 160*devicePixelRatio, ly = pad.t + 12*devicePixelRatio;
  ctx.save();
  ctx.font = `${9*devicePixelRatio}px JetBrains Mono`;
  ctx.fillStyle = '#4fc3f7';
  ctx.fillRect(lx, ly, 22*devicePixelRatio, 2.5*devicePixelRatio);
  ctx.fillText('x₁(t) norm.', lx + 28*devicePixelRatio, ly + 4*devicePixelRatio);
  ctx.fillStyle = '#80deea';
  // draw dashed line for legend
  [0,1,2].forEach(k => {
    ctx.fillRect(lx + k*10*devicePixelRatio, ly+16*devicePixelRatio, 6*devicePixelRatio, 2.5*devicePixelRatio);
  });
  ctx.fillText('v₁(t) norm.', lx + 28*devicePixelRatio, ly + 20*devicePixelRatio);
  ctx.restore();
}

// ── MAIN DRAW ──
function draw() {
  const { A1, A2, f1, f2, vis } = state;
  const w1 = 2*Math.PI*f1, w2 = 2*Math.PI*f2;
  const tWin = Math.max(3 / Math.min(f1, f2), 4);
  const A_MAX = 2.0, F_MAX = 3.0;
  const yMaxXT = A_MAX * 2 + 0.2;
  const vMax   = A_MAX * 2 * Math.PI * F_MAX;

  drawGraph(ctxXT, cvsXT,
    [vis.s1 ? x1 : null, vis.s2 ? x2 : null, vis.ss ? xsum : null],
    tWin, yMaxXT, ['#4fc3f7','#f06292','#a5d6a7'], null, true, 'x / m');

  drawGraph(ctxVT, cvsVT,
    [vis.v1 ? v1 : null, vis.v2 ? v2 : null],
    tWin, vMax, ['#80deea','#f48fb1'], [true, true], true, 'v / (m/s)');

  if (state.showOverlay) drawOverlay();

  document.getElementById('t-ind').textContent = `t = ${state.t.toFixed(2)} s`;
}

// ── ANIMATION ──
function togglePlay() {
  state.playing = !state.playing;
  document.getElementById('play-btn').textContent = state.playing ? '⏸' : '▶';
  if (state.playing) { lastTime = null; rafId = requestAnimationFrame(loop); }
  else { cancelAnimationFrame(rafId); }
}
function resetTime() { state.t = 0; draw(); }
function updateSpeed(el) {
  state.speed = parseFloat(el.value);
  document.getElementById('speed-val').textContent = `×${state.speed.toFixed(1)}`;
  updateSliderFill(el);
}
function loop(ts) {
  if (lastTime !== null) state.t += (ts - lastTime) / 1000 * state.speed;
  lastTime = ts;
  draw();
  if (state.playing) rafId = requestAnimationFrame(loop);
}

// ── VISIBILITY TOGGLES ──
function toggleVis(key, btn) {
  state.vis[key] = !state.vis[key];
  btn.classList.toggle('active');
  draw();
}

function toggleOverlay(btn) {
  state.showOverlay = !state.showOverlay;
  btn.classList.toggle('active');
  document.getElementById('ovl-area').style.display = state.showOverlay ? 'block' : 'none';
  resizeCanvases();
}

function toggleT4(btn) {
  state.showT4 = !state.showT4;
  btn.classList.toggle('active');
  draw();
}

// ── INIT ──
setupSliders();
updateUI();
document.getElementById('ovl-area').style.display = 'none';
setTimeout(() => { resizeCanvases(); }, 50);
</script>
</body>
</html>
