<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>third time.</title>
  <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;1,300&family=Inter:wght@300;400&display=swap" rel="stylesheet" />
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    /* ── Backgrounds ── */
    .bg-layer {
      position: fixed; inset: 0;
      background-size: 400% 400%;
      animation: gradBG 18s ease infinite;
      transition: opacity 2.8s ease;
      z-index: 0;
    }
    @keyframes gradBG {
      0%   { background-position: 0% 50%; }
      50%  { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }
    .bg-idle  { background: linear-gradient(-45deg, #0a0a0a, #1a1a1a, #2a2a2a, #151515); opacity: 1; }
    .bg-work  { background: linear-gradient(-45deg, #120700, #1e1000, #2a1500, #160900); opacity: 0; }
    .bg-break { background: linear-gradient(-45deg, #00080e, #00111d, #001a2b, #000e19); opacity: 0; }

    #particles { position: fixed; inset: 0; z-index: 1; pointer-events: none; }

    body {
      font-family: 'Inter', sans-serif;
      font-weight: 300;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      color: #fff;
    }

    .brand-title {
      position: fixed; top: 38px; left: 48px;
      font-family: 'Cormorant Garamond', serif;
      font-weight: 300; font-size: 30px;
      letter-spacing: 2px; opacity: 0.6;
      z-index: 10; color: #fff;
    }

    /* ── Card ── */
    .glass-card {
      position: relative; z-index: 10;
      background: rgba(255,255,255,0.06);
      backdrop-filter: blur(26px);
      -webkit-backdrop-filter: blur(26px);
      border: 1px solid rgba(255,255,255,0.09);
      padding: 38px 44px 44px;
      border-radius: 26px;
      box-shadow: 0 50px 100px rgba(0,0,0,0.85);
      width: 365px;
      text-align: center;
    }

    /* ── Status ── */
    #status {
      font-size: 10px; letter-spacing: 4.5px;
      text-transform: uppercase;
      margin-bottom: 6px;
      transition: color 2.2s ease;
      min-height: 14px;
    }
    .s-idle  { color: #3a3a3a; }
    .s-work  { color: rgba(255,162,55,0.7); }
    .s-break { color: rgba(95,162,255,0.7); }
    .s-easy  { color: rgba(150,210,160,0.65); }

    /* ── Ring ── */
    .timer-wrapper {
      position: relative;
      width: 208px; height: 208px;
      margin: 6px auto 20px;
      display: flex; align-items: center; justify-content: center;
    }

    .progress-ring {
      position: absolute; inset: 0;
      transform: rotate(-90deg);
      overflow: visible;
    }

    .ring-track { fill: none; stroke: rgba(255,255,255,0.035); stroke-width: 1; }

    .ring-fill {
      fill: none; stroke-width: 1; stroke-linecap: round;
      stroke-dasharray: 565.5;
      stroke-dashoffset: 565.5;
      opacity: 0;
      transition: stroke-dashoffset 0.65s ease, opacity 1.4s ease, stroke 2.2s ease, filter 2.2s ease;
    }
    .ring-fill.rw {
      stroke: rgba(255,162,55,0.38);
      filter: drop-shadow(0 0 5px rgba(255,162,55,0.2));
      opacity: 1;
    }
    .ring-fill.rb {
      stroke: rgba(95,162,255,0.48);
      filter: drop-shadow(0 0 5px rgba(95,162,255,0.25));
      opacity: 1;
    }

    /* ── Clock ── */
    #clockWrap { position: relative; z-index: 2; }

    #display {
      font-size: 60px; font-weight: 300;
      letter-spacing: -2px; color: #fff;
      font-variant-numeric: tabular-nums; line-height: 1;
      transition: filter 1.2s ease, opacity 1.2s ease;
      user-select: none;
    }
    #display.blurred { filter: blur(10px); opacity: 0.12; }
    .timer-wrapper:hover #display.blurred { filter: blur(0); opacity: 1; }

    /* ── Fuzzy break text ── */
    #fuzzyText {
      display: none;
      font-family: 'Cormorant Garamond', serif;
      font-style: italic;
      font-weight: 300;
      font-size: 20px;
      letter-spacing: 2.5px;
      color: rgba(95,162,255,0.48);
      line-height: 1.5;
    }

    /* ── Settings ── */
    .settings-block { margin-bottom: 24px; transition: opacity 0.7s ease; }

    .settings-row {
      display: flex; justify-content: space-between; align-items: center;
      padding: 10px 0;
      border-bottom: 1px solid rgba(255,255,255,0.06);
    }

    label { font-size: 10px; color: #777; letter-spacing: 1.5px; text-transform: uppercase; }

    select {
      background: transparent; color: rgba(255,255,255,0.65); border: none;
      font-family: 'Inter', sans-serif; font-size: 10px; letter-spacing: 0.5px;
      outline: none; cursor: pointer; text-align: right;
    }
    select option { background: #141414; color: #fff; }

    /* ── Button ── */
    #actionBtn {
      background: rgba(255,255,255,0.05);
      color: rgba(255,255,255,0.55);
      border: 1px solid rgba(255,255,255,0.08);
      padding: 15px; width: 100%;
      font-size: 10px; font-family: 'Inter', sans-serif;
      font-weight: 400; letter-spacing: 3.5px; text-transform: uppercase;
      border-radius: 12px; cursor: pointer;
      transition: all 0.3s ease;
    }
    #actionBtn:hover {
      background: rgba(255,255,255,0.09);
      color: rgba(255,255,255,0.85);
      border-color: rgba(255,255,255,0.14);
      transform: translateY(-1px);
    }
    #actionBtn:active { transform: translateY(0); }
    #actionBtn.breaking {
      background: rgba(95,162,255,0.07);
      color: rgba(95,162,255,0.7);
      border-color: rgba(95,162,255,0.15);
    }
    #actionBtn.breaking:hover {
      background: rgba(95,162,255,0.12);
      color: rgba(130,185,255,0.85);
    }

    /* ── Below card ── */
    .below-card {
      position: relative; z-index: 10;
      margin-top: 20px;
      display: flex; flex-direction: column;
      align-items: center; gap: 12px;
    }

    #sessionLog {
      font-size: 10px; letter-spacing: 2.5px;
      text-transform: uppercase;
      color: rgba(255,255,255,0.16);
      min-height: 13px;
    }

    /* ── Weekly dots ── */
    .week-dots { display: flex; gap: 13px; align-items: flex-end; }
    .dot-wrap  { display: flex; flex-direction: column; align-items: center; gap: 5px; }
    .dot {
      width: 5px; height: 5px; border-radius: 50%;
      background: rgba(255,255,255,0.07);
      transition: background 1s ease, box-shadow 1s ease;
    }
    .dot.active { background: rgba(255,152,45,0.32); }
    .dot.today  { background: rgba(255,152,45,0.65); box-shadow: 0 0 7px rgba(255,152,45,0.3); }
    .dot-label { font-size: 8px; color: rgba(255,255,255,0.13); letter-spacing: 1px; text-transform: uppercase; }
    .dot-label.today-lbl { color: rgba(255,152,45,0.38); }
  </style>
</head>
<body>

<div class="bg-layer bg-idle" id="bgIdle"></div>
<div class="bg-layer bg-work" id="bgWork"></div>
<div class="bg-layer bg-break" id="bgBreak"></div>
<canvas id="particles"></canvas>

<h1 class="brand-title">third time.</h1>

<div class="glass-card">
  <div id="status" class="s-idle">ready</div>

  <div class="timer-wrapper" id="timerWrapper">
    <svg class="progress-ring" viewBox="0 0 208 208">
      <circle class="ring-track" cx="104" cy="104" r="90"/>
      <circle class="ring-fill"  cx="104" cy="104" r="90" id="ringFill"/>
    </svg>
    <div id="clockWrap">
      <div id="display">00:00</div>
      <div id="fuzzyText">rest a moment</div>
    </div>
  </div>

  <div class="settings-block" id="settingsBlock">
    <div class="settings-row">
      <label for="fraction">Break Ratio</label>
      <select id="fraction">
        <option value="2">1/2 — lazy</option>
        <option value="3" selected>1/3 — standard</option>
        <option value="4">1/4 — industrious</option>
        <option value="5">1/5 — hard</option>
        <option value="6">1/6 — grinding</option>
      </select>
    </div>
    <div class="settings-row">
      <label for="radioSel">Radio</label>
      <select id="radioSel">
        <option value="https://ice1.somafm.com/groovesalad-256-mp3">Groove Salad</option>
        <option value="https://ice1.somafm.com/deepspaceone-128-mp3">Deep Space One</option>
        <option value="https://ice1.somafm.com/dronezone-256-mp3">Drone Zone</option>
        <option value="https://ice1.somafm.com/sonicuniverse-256-mp3">Sonic Universe</option>
      </select>
    </div>
  </div>

  <button id="actionBtn">begin</button>
</div>

<div class="below-card">
  <div id="sessionLog"></div>
  <div class="week-dots" id="weekDots"></div>
</div>

<script>
// ── Constants ─────────────────────────────────────────────────────────────────
const CIRC         = 565.5;   // 2π × 90
const MIN_LOG_SECS = 120;     // sessions under 2 min: don't persist
const STRESS_SECS  = 300;     // sessions under 5 min = short
const STRESS_N     = 2;       // 2 consecutive short = stressed
const STORE        = 'tt_v4';
const DAY_LABELS   = ['M','T','W','T','F','S','S'];

// ── State ─────────────────────────────────────────────────────────────────────
let mode              = 'idle';
let tickInterval      = null;
let workStart         = null;
let breakTotal        = 0;
let breakStart        = null;
let audioReady        = false;
let musicPauseTimeout = null;
let recentDurations   = []; // in-memory only, for stress detection

// ── DOM ───────────────────────────────────────────────────────────────────────
const statusEl    = document.getElementById('status');
const displayEl   = document.getElementById('display');
const fuzzyEl     = document.getElementById('fuzzyText');
const ringFillEl  = document.getElementById('ringFill');
const timerWrap   = document.getElementById('timerWrapper');
const settingsEl  = document.getElementById('settingsBlock');
const fractionSel = document.getElementById('fraction');
const radioSel    = document.getElementById('radioSel');
const btnEl       = document.getElementById('actionBtn');
const logEl       = document.getElementById('sessionLog');
const dotsEl      = document.getElementById('weekDots');
const bgIdle      = document.getElementById('bgIdle');
const bgWork      = document.getElementById('bgWork');
const bgBreak     = document.getElementById('bgBreak');

// ── Audio context + gain nodes ────────────────────────────────────────────────
const audioCtx       = new (window.AudioContext || window.webkitAudioContext)();
const musicEl        = new Audio();
musicEl.crossOrigin  = 'anonymous';

const musicGain      = audioCtx.createGain();
const idleGain       = audioCtx.createGain();
const breakAlphaGain = audioCtx.createGain();

musicGain.gain.value      = 0;
idleGain.gain.value       = 0;
breakAlphaGain.gain.value = 0;

musicGain.connect(audioCtx.destination);
idleGain.connect(audioCtx.destination);
breakAlphaGain.connect(audioCtx.destination);

// ── Gain fader ────────────────────────────────────────────────────────────────
function fadeTo(node, value, secs) {
  node.gain.cancelScheduledValues(audioCtx.currentTime);
  node.gain.setValueAtTime(node.gain.value, audioCtx.currentTime);
  node.gain.linearRampToValueAtTime(value, audioCtx.currentTime + secs);
}

// ── Build isochronic + pink noise layer ───────────────────────────────────────
function buildIsoLayer(carrierHz, beatHz, outputGain) {
  // Pink noise (Kellet's algorithm)
  const SR  = audioCtx.sampleRate;
  const buf = audioCtx.createBuffer(1, SR * 12, SR);
  const dat = buf.getChannelData(0);
  let b0=0, b1=0, b2=0, b3=0, b4=0, b5=0, b6=0;
  for (let i = 0; i < dat.length; i++) {
    const w = Math.random() * 2 - 1;
    b0 = 0.99886*b0 + w*0.0555179;
    b1 = 0.99332*b1 + w*0.0750759;
    b2 = 0.96900*b2 + w*0.1538520;
    b3 = 0.86650*b3 + w*0.3104856;
    b4 = 0.55000*b4 + w*0.5329522;
    b5 = -0.7616*b5 - w*0.0168980;
    dat[i] = (b0+b1+b2+b3+b4+b5+b6+w*0.5362) * 0.065;
    b6 = w * 0.115926;
  }
  const noiseNode = audioCtx.createBufferSource();
  noiseNode.buffer = buf; noiseNode.loop = true;
  const noiseGn = audioCtx.createGain();
  noiseGn.gain.value = 0.42;
  noiseNode.connect(noiseGn);
  noiseGn.connect(outputGain);
  noiseNode.start();

  // Isochronic carrier modulated at beatHz
  const carrier = audioCtx.createOscillator();
  carrier.type = 'sine';
  carrier.frequency.value = carrierHz;

  const amOsc = audioCtx.createOscillator();
  amOsc.type = 'sine';
  amOsc.frequency.value = beatHz;

  const amDepth = audioCtx.createGain();
  amDepth.gain.value = 0.36;

  const cGain = audioCtx.createGain();
  cGain.gain.value = 0.45;

  amOsc.connect(amDepth);
  amDepth.connect(cGain.gain);
  carrier.connect(cGain);
  cGain.connect(outputGain);

  carrier.start();
  amOsc.start();
}

// ── Init audio (called on first user gesture) ─────────────────────────────────
let musicMediaSource = null;
function initAudio() {
  if (audioReady) return;
  audioReady = true;
  buildIsoLayer(200, 12, idleGain);       // idle: 12 Hz → high-alpha/low-beta (ready state)
  buildIsoLayer(180,  9, breakAlphaGain); // break: 9 Hz → alpha (calm recovery)
  if (!musicMediaSource) {
    musicMediaSource = audioCtx.createMediaElementSource(musicEl);
    musicMediaSource.connect(musicGain);
  }
}

// ── Music helpers ─────────────────────────────────────────────────────────────
function startMusic() {
  if (musicPauseTimeout) { clearTimeout(musicPauseTimeout); musicPauseTimeout = null; }
  musicEl.src = radioSel.value;
  musicEl.load();
  musicEl.play().catch(() => {});
  fadeTo(musicGain, 0.72, 5);
}

function stopMusic() {
  fadeTo(musicGain, 0, 3);
  musicPauseTimeout = setTimeout(() => { musicEl.pause(); musicPauseTimeout = null; }, 3400);
}

// ── Singing bowl ──────────────────────────────────────────────────────────────
function playBowl() {
  const master = audioCtx.createGain();
  master.connect(audioCtx.destination);
  [
    { f: 220,  g: 0.5,  d: 5.5 },
    { f: 588,  g: 0.25, d: 4.0 },
    { f: 1100, g: 0.11, d: 2.6 },
    { f: 1760, g: 0.05, d: 1.6 },
  ].forEach(({ f, g, d }) => {
    const osc = audioCtx.createOscillator();
    const gn  = audioCtx.createGain();
    osc.type = 'sine'; osc.frequency.value = f;
    gn.gain.setValueAtTime(g, audioCtx.currentTime);
    gn.gain.exponentialRampToValueAtTime(0.0001, audioCtx.currentTime + d);
    osc.connect(gn); gn.connect(master);
    osc.start(); osc.stop(audioCtx.currentTime + d + 0.1);
  });
}

// ── Notifications ─────────────────────────────────────────────────────────────
if (Notification.permission !== 'granted') Notification.requestPermission();
function notify(msg) {
  playBowl();
  if (Notification.permission === 'granted') new Notification('third time.', { body: msg });
}

// ── Utility ───────────────────────────────────────────────────────────────────
function fmt(s) {
  s = Math.max(0, Math.round(s));
  return `${Math.floor(s/60).toString().padStart(2,'0')}:${(s%60).toString().padStart(2,'0')}`;
}

function setBg(st) {
  bgIdle.style.opacity  = st === 'idle'  ? '1' : '0';
  bgWork.style.opacity  = st === 'work'  ? '1' : '0';
  bgBreak.style.opacity = st === 'break' ? '1' : '0';
}

function setRing(fraction, cls) {
  ringFillEl.style.strokeDashoffset = CIRC * (1 - Math.max(0, Math.min(1, fraction)));
  ringFillEl.classList.remove('rw', 'rb');
  if (cls) ringFillEl.classList.add(cls);
}

function setTab(st, secs) {
  if      (st === 'work')  document.title = `● ${fmt(secs)}  deep work`;
  else if (st === 'break') document.title = `◌ ${fmt(secs)}  resting`;
  else                     document.title = 'third time.';
}

function fuzzyLabel(remaining, total) {
  const p = remaining / total;
  if (p > 0.65) return 'rest a moment';
  if (p > 0.35) return 'settling in';
  if (p > 0.12) return 'almost there';
  return "whenever you're ready";
}

function lockSettings() {
  settingsEl.style.opacity = '0.18';
  settingsEl.style.pointerEvents = 'none';
  fractionSel.disabled = radioSel.disabled = true;
}
function unlockSettings() {
  settingsEl.style.opacity = '1';
  settingsEl.style.pointerEvents = 'auto';
  fractionSel.disabled = radioSel.disabled = false;
}

function isStressed() {
  if (recentDurations.length < STRESS_N) return false;
  return recentDurations.slice(-STRESS_N).every(d => d < STRESS_SECS);
}

// ── Settings persistence ──────────────────────────────────────────────────────
function saveSettings() {
  localStorage.setItem('tt_frac', fractionSel.value);
  localStorage.setItem('tt_radio', radioSel.value);
}
function loadSettings() {
  const f = localStorage.getItem('tt_frac');
  const r = localStorage.getItem('tt_radio');
  if (f) fractionSel.value = f;
  if (r) radioSel.value = r;
}
fractionSel.addEventListener('change', saveSettings);
radioSel.addEventListener('change', saveSettings);

// ── Session persistence ───────────────────────────────────────────────────────
function todayKey() { return new Date().toISOString().slice(0, 10); }

function loadData() {
  try { return JSON.parse(localStorage.getItem(STORE)) || {}; }
  catch { return {}; }
}

function saveSession(secs) {
  recentDurations.push(secs);
  if (recentDurations.length > 8) recentDurations.shift();
  if (secs < MIN_LOG_SECS) return;
  const data = loadData(), key = todayKey();
  if (!data[key]) data[key] = { total: 0, sessions: [] };
  data[key].sessions.push({ ts: Date.now(), secs: Math.round(secs) });
  data[key].total += Math.round(secs);
  localStorage.setItem(STORE, JSON.stringify(data));
}

// ── Render log + weekly dots ──────────────────────────────────────────────────
function renderLog() {
  const data  = loadData();
  const today = data[todayKey()];

  if (today && today.sessions.length > 0) {
    const n  = today.sessions.length;
    const tm = Math.floor(today.total / 60);
    const h  = Math.floor(tm / 60);
    const m  = tm % 60;
    logEl.textContent = `${n} session${n !== 1 ? 's' : ''}  ·  ${h > 0 ? h + 'h ' : ''}${m}m`;
  } else {
    logEl.textContent = '';
  }

  // Weekly dots — Mon-Sun of current week
  const now    = new Date();
  const dow    = now.getDay();
  const toMon  = dow === 0 ? -6 : 1 - dow;
  const monday = new Date(now);
  monday.setDate(now.getDate() + toMon);
  const todayStr = todayKey();

  let html = '';
  for (let i = 0; i < 7; i++) {
    const d   = new Date(monday);
    d.setDate(monday.getDate() + i);
    const key      = d.toISOString().slice(0, 10);
    const isToday  = key === todayStr;
    const hasData  = !!(data[key] && data[key].total >= 60);
    const dotCls   = isToday ? 'dot today' : (hasData ? 'dot active' : 'dot');
    const lblCls   = isToday ? 'dot-label today-lbl' : 'dot-label';
    html += `<div class="dot-wrap"><div class="${dotCls}"></div><div class="${lblCls}">${DAY_LABELS[i]}</div></div>`;
  }
  dotsEl.innerHTML = html;
}

// ── Particles ─────────────────────────────────────────────────────────────────
const canvas = document.getElementById('particles');
const pCtx   = canvas.getContext('2d');
function resizeCanvas() { canvas.width = innerWidth; canvas.height = innerHeight; }
resizeCanvas();
window.addEventListener('resize', resizeCanvas);

function mkP() {
  return {
    x: Math.random() * canvas.width,
    y: Math.random() * canvas.height,
    r: Math.random() * 0.9 + 0.2,
    vx: (Math.random() - 0.5) * 0.09,
    vy: -(Math.random() * 0.14 + 0.025),
    a: Math.random() * 0.2 + 0.025,
  };
}
const ptcls = Array.from({ length: 80 }, mkP);

(function animP() {
  pCtx.clearRect(0, 0, canvas.width, canvas.height);
  ptcls.forEach(p => {
    pCtx.beginPath();
    pCtx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
    pCtx.fillStyle = `rgba(255,255,255,${p.a})`;
    pCtx.fill();
    p.x += p.vx; p.y += p.vy;
    if (p.y < -4 || p.x < -4 || p.x > canvas.width + 4)
      Object.assign(p, mkP(), { y: canvas.height + 4 });
  });
  requestAnimationFrame(animP);
})();

// ── Reset to idle ─────────────────────────────────────────────────────────────
function resetToIdle() {
  clearInterval(tickInterval);
  mode = 'idle';

  displayEl.className   = '';
  displayEl.textContent = '00:00';
  displayEl.style.display = 'block';
  fuzzyEl.style.display   = 'none';

  statusEl.textContent = 'ready';
  statusEl.className   = 's-idle';
  btnEl.textContent    = 'begin';
  btnEl.classList.remove('breaking');

  setBg('idle');
  setRing(0, null);
  setTab('idle', 0);
  unlockSettings();

  // Idle preparatory tone fades in
  fadeTo(idleGain, 0.10, 5);

  renderLog();
}

// ── Main button ───────────────────────────────────────────────────────────────
btnEl.addEventListener('click', () => {
  if (audioCtx.state === 'suspended') audioCtx.resume();
  initAudio();
  clearInterval(tickInterval);

  if (mode === 'idle' || mode === 'breaking') {
    // ── Begin work ────────────────────────────────────────────────────────────
    mode      = 'working';
    workStart = Date.now();

    fadeTo(idleGain, 0, 2.5);
    fadeTo(breakAlphaGain, 0, 2);
    startMusic();

    const stressed = isStressed();
    statusEl.textContent = stressed ? 'easy does it' : 'deep work';
    statusEl.className   = stressed ? 's-easy' : 's-work';

    setBg('work');
    setRing(1, 'rw');
    lockSettings();

    // Hide clock during work, show on hover
    displayEl.classList.add('blurred');
    displayEl.style.display = 'block';
    fuzzyEl.style.display   = 'none';

    const ratio = fractionSel.options[fractionSel.selectedIndex].text.split('—')[0].trim();
    btnEl.textContent = stressed ? `rest when ready · ${ratio}` : `take break · ${ratio}`;
    btnEl.classList.remove('breaking');

    tickInterval = setInterval(() => {
      const elapsed = (Date.now() - workStart) / 1000;
      displayEl.textContent = fmt(elapsed);
      setTab('work', elapsed);
    }, 500);

  } else if (mode === 'working') {
    // ── Begin break ───────────────────────────────────────────────────────────
    const workedSecs = (Date.now() - workStart) / 1000;
    saveSession(workedSecs);

    mode       = 'breaking';
    const div  = parseInt(fractionSel.value);
    breakTotal = Math.max(10, Math.floor(workedSecs / div));
    breakStart = Date.now();

    stopMusic();
    setTimeout(() => playBowl(), 1600);
    setTimeout(() => fadeTo(breakAlphaGain, 0.09, 5), 2000);

    statusEl.textContent = 'recharging';
    statusEl.className   = 's-break';
    btnEl.textContent    = 'end break early';
    btnEl.classList.add('breaking');

    setBg('break');
    setRing(1, 'rb');

    // Fuzzy text instead of numbers
    displayEl.style.display = 'none';
    displayEl.classList.remove('blurred');
    fuzzyEl.style.display   = 'block';
    fuzzyEl.textContent     = 'rest a moment';

    tickInterval = setInterval(() => {
      const elapsed   = (Date.now() - breakStart) / 1000;
      const remaining = breakTotal - elapsed;

      fuzzyEl.textContent = fuzzyLabel(remaining, breakTotal);
      setRing(Math.max(0, remaining / breakTotal), 'rb');
      setTab('break', remaining);

      if (remaining <= 0) {
        clearInterval(tickInterval);
        notify('break is over. return to focus.');
        fadeTo(breakAlphaGain, 0, 2.5);
        resetToIdle();
      }
    }, 500);
  }
});

// ── Init ──────────────────────────────────────────────────────────────────────
loadSettings();
renderLog();
</script>
</body>
</html>
