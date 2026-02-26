<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>IDS using Machine Learning — UNSW-NB15</title>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;700&family=Syne:wght@400;600;700;800&family=Inter:wght@300;400;500&display=swap" rel="stylesheet"/>
<style>
  :root {
    --bg: #0a0e1a;
    --surface: #111827;
    --surface2: #1a2235;
    --border: #1e2d45;
    --accent: #00d4ff;
    --accent2: #7c3aed;
    --accent3: #10b981;
    --accent4: #f59e0b;
    --danger: #ef4444;
    --text: #e2e8f0;
    --muted: #64748b;
    --mono: 'JetBrains Mono', monospace;
    --sans: 'Inter', sans-serif;
    --display: 'Syne', sans-serif;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--sans);
    font-size: 15px;
    line-height: 1.7;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* Grid background */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(0,212,255,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,212,255,0.03) 1px, transparent 1px);
    background-size: 40px 40px;
    pointer-events: none;
    z-index: 0;
  }

  .container {
    max-width: 900px;
    margin: 0 auto;
    padding: 60px 32px;
    position: relative;
    z-index: 1;
  }

  /* ── HERO ── */
  .hero {
    text-align: center;
    padding: 60px 0 50px;
    position: relative;
  }

  .hero-badge {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    background: rgba(0,212,255,0.08);
    border: 1px solid rgba(0,212,255,0.25);
    border-radius: 999px;
    padding: 6px 18px;
    font-family: var(--mono);
    font-size: 12px;
    color: var(--accent);
    letter-spacing: 0.1em;
    text-transform: uppercase;
    margin-bottom: 28px;
    animation: fadeUp 0.6s ease both;
  }

  .hero-badge::before {
    content: '';
    width: 6px; height: 6px;
    border-radius: 50%;
    background: var(--accent);
    box-shadow: 0 0 8px var(--accent);
    animation: pulse 2s infinite;
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.3; }
  }

  .hero h1 {
    font-family: var(--display);
    font-size: clamp(2rem, 5vw, 3.2rem);
    font-weight: 800;
    line-height: 1.1;
    letter-spacing: -0.02em;
    background: linear-gradient(135deg, #ffffff 0%, var(--accent) 50%, var(--accent2) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    margin-bottom: 20px;
    animation: fadeUp 0.6s 0.1s ease both;
  }

  .hero-desc {
    max-width: 620px;
    margin: 0 auto 36px;
    color: #94a3b8;
    font-size: 16px;
    line-height: 1.8;
    animation: fadeUp 0.6s 0.2s ease both;
  }

  .hero-tags {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 10px;
    animation: fadeUp 0.6s 0.3s ease both;
  }

  .tag {
    font-family: var(--mono);
    font-size: 11px;
    padding: 5px 14px;
    border-radius: 6px;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    font-weight: 700;
  }

  .tag-blue  { background: rgba(0,212,255,0.1);  color: var(--accent);  border: 1px solid rgba(0,212,255,0.2); }
  .tag-purple{ background: rgba(124,58,237,0.1); color: #a78bfa;        border: 1px solid rgba(124,58,237,0.2);}
  .tag-green { background: rgba(16,185,129,0.1); color: var(--accent3); border: 1px solid rgba(16,185,129,0.2);}
  .tag-amber { background: rgba(245,158,11,0.1); color: var(--accent4); border: 1px solid rgba(245,158,11,0.2);}

  /* ── STATS ROW ── */
  .stats-row {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 16px;
    margin: 50px 0;
    animation: fadeUp 0.6s 0.35s ease both;
  }

  .stat-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 22px 18px;
    text-align: center;
    transition: border-color 0.2s, transform 0.2s;
  }

  .stat-card:hover {
    border-color: var(--accent);
    transform: translateY(-3px);
  }

  .stat-card .num {
    font-family: var(--display);
    font-size: 1.8rem;
    font-weight: 800;
    color: var(--accent);
    display: block;
    line-height: 1;
    margin-bottom: 6px;
  }

  .stat-card .lbl {
    font-size: 11px;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 0.07em;
    font-family: var(--mono);
  }

  /* ── SECTION ── */
  .section {
    margin-bottom: 52px;
    animation: fadeUp 0.6s ease both;
  }

  .section-header {
    display: flex;
    align-items: center;
    gap: 12px;
    margin-bottom: 24px;
    padding-bottom: 14px;
    border-bottom: 1px solid var(--border);
  }

  .section-icon {
    width: 36px; height: 36px;
    border-radius: 8px;
    display: flex; align-items: center; justify-content: center;
    font-size: 16px;
    flex-shrink: 0;
  }

  .icon-blue   { background: rgba(0,212,255,0.12);  }
  .icon-purple { background: rgba(124,58,237,0.12); }
  .icon-green  { background: rgba(16,185,129,0.12); }
  .icon-amber  { background: rgba(245,158,11,0.12); }
  .icon-red    { background: rgba(239,68,68,0.12);  }

  .section-header h2 {
    font-family: var(--display);
    font-size: 1.25rem;
    font-weight: 700;
    letter-spacing: -0.01em;
  }

  /* ── CARDS ── */
  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 24px;
    margin-bottom: 16px;
  }

  .card h3 {
    font-family: var(--display);
    font-size: 1rem;
    font-weight: 700;
    margin-bottom: 14px;
    display: flex;
    align-items: center;
    gap: 8px;
  }

  /* ── TABLE ── */
  table {
    width: 100%;
    border-collapse: collapse;
    font-size: 14px;
    border-radius: 10px;
    overflow: hidden;
  }

  thead tr {
    background: var(--surface2);
  }

  th {
    padding: 12px 16px;
    text-align: left;
    font-family: var(--mono);
    font-size: 11px;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 0.08em;
    font-weight: 400;
  }

  td {
    padding: 11px 16px;
    border-top: 1px solid var(--border);
    color: var(--text);
  }

  tbody tr:hover td { background: rgba(255,255,255,0.02); }

  /* ── LIST ── */
  .list { list-style: none; }

  .list li {
    padding: 7px 0;
    display: flex;
    align-items: flex-start;
    gap: 10px;
    color: #94a3b8;
    font-size: 14px;
    border-bottom: 1px solid rgba(255,255,255,0.04);
  }

  .list li:last-child { border: none; }

  .list li::before {
    content: '▸';
    color: var(--accent);
    flex-shrink: 0;
    margin-top: 1px;
    font-size: 12px;
  }

  /* ── CODE BLOCK ── */
  pre {
    background: #060a12;
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 16px 20px;
    font-family: var(--mono);
    font-size: 13px;
    color: var(--accent3);
    overflow-x: auto;
    margin: 12px 0;
  }

  /* ── WORKFLOW STEPS ── */
  .step {
    display: flex;
    gap: 20px;
    margin-bottom: 28px;
  }

  .step-num {
    width: 32px; height: 32px;
    border-radius: 8px;
    background: linear-gradient(135deg, var(--accent2), var(--accent));
    display: flex; align-items: center; justify-content: center;
    font-family: var(--mono);
    font-size: 13px;
    font-weight: 700;
    flex-shrink: 0;
    color: white;
  }

  .step-content h3 {
    font-family: var(--display);
    font-weight: 700;
    font-size: 1rem;
    margin-bottom: 10px;
    color: var(--text);
  }

  /* ── RESULT BADGE ── */
  .result-badge {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    padding: 4px 12px;
    border-radius: 999px;
    font-family: var(--mono);
    font-size: 12px;
    font-weight: 700;
  }

  .badge-green { background: rgba(16,185,129,0.15); color: var(--accent3); border: 1px solid rgba(16,185,129,0.3); }
  .badge-blue  { background: rgba(0,212,255,0.12);  color: var(--accent);  border: 1px solid rgba(0,212,255,0.25);}

  /* ── ACCURACY BARS ── */
  .acc-item {
    margin-bottom: 14px;
  }

  .acc-label {
    display: flex;
    justify-content: space-between;
    margin-bottom: 6px;
    font-size: 13px;
    color: #94a3b8;
  }

  .acc-label strong { color: var(--text); font-family: var(--mono); }

  .acc-bar-bg {
    height: 8px;
    background: var(--surface2);
    border-radius: 4px;
    overflow: hidden;
  }

  .acc-bar {
    height: 100%;
    border-radius: 4px;
    background: linear-gradient(90deg, var(--accent2), var(--accent));
    transition: width 1s ease;
  }

  /* ── TECH GRID ── */
  .tech-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(130px, 1fr));
    gap: 12px;
  }

  .tech-item {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 14px 16px;
    text-align: center;
    font-family: var(--mono);
    font-size: 12px;
    color: var(--accent);
    font-weight: 700;
    transition: border-color 0.2s, background 0.2s;
  }

  .tech-item:hover {
    border-color: var(--accent2);
    background: rgba(124,58,237,0.08);
  }

  .tech-item span {
    display: block;
    color: var(--muted);
    font-size: 10px;
    font-weight: 400;
    margin-top: 4px;
    letter-spacing: 0.05em;
  }

  /* ── PERF PILLS ── */
  .perf-high   { color: var(--accent3); }
  .perf-strong { color: var(--accent);  }
  .perf-good   { color: var(--accent4); }
  .perf-mod    { color: #f97316; }
  .perf-low    { color: var(--danger);  }

  /* ── CONCLUSION ── */
  .conclusion-box {
    background: linear-gradient(135deg, rgba(0,212,255,0.06), rgba(124,58,237,0.06));
    border: 1px solid rgba(0,212,255,0.2);
    border-radius: 14px;
    padding: 32px;
    text-align: center;
  }

  .conclusion-box p {
    color: #94a3b8;
    max-width: 580px;
    margin: 0 auto 18px;
    line-height: 1.8;
  }

  .conclusion-box .highlight {
    font-size: 1.5rem;
    font-family: var(--display);
    font-weight: 800;
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  /* ── DIVIDER ── */
  hr {
    border: none;
    border-top: 1px solid var(--border);
    margin: 48px 0;
  }

  /* ── INLINE CODE ── */
  code {
    font-family: var(--mono);
    font-size: 12px;
    background: rgba(0,212,255,0.08);
    color: var(--accent);
    padding: 2px 7px;
    border-radius: 4px;
    border: 1px solid rgba(0,212,255,0.15);
  }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(18px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  @media (max-width: 600px) {
    .stats-row { grid-template-columns: repeat(2, 1fr); }
    .container { padding: 40px 20px; }
  }
</style>
</head>
<body>
<div class="container">

  <!-- ── HERO ── -->
  <div class="hero">
    <div class="hero-badge">Machine Learning · Cybersecurity · UNSW-NB15</div>
    <h1>Intrusion Detection System<br>using Machine Learning</h1>
    <p class="hero-desc">
      A high-performance, ensemble learning-based Intrusion Detection System (IDS) trained on the 
      UNSW-NB15 dataset. The system identifies malicious network traffic in real-time by classifying 
      connections as normal or attack, and further categorising attack types — combining the power of 
      Random Forest and XGBoost for robust, production-ready threat detection.
    </p>
    <div class="hero-tags">
      <span class="tag tag-blue">Python 3.x</span>
      <span class="tag tag-purple">Scikit-Learn</span>
      <span class="tag tag-green">XGBoost</span>
      <span class="tag tag-amber">UNSW-NB15</span>
      <span class="tag tag-blue">Binary Classification</span>
      <span class="tag tag-purple">Multi-Class Classification</span>
    </div>
  </div>

  <!-- ── STATS ── -->
  <div class="stats-row">
    <div class="stat-card">
      <span class="num">87.33%</span>
      <span class="lbl">Best Accuracy</span>
    </div>
    <div class="stat-card">
      <span class="num">257K</span>
      <span class="lbl">Total Records</span>
    </div>
    <div class="stat-card">
      <span class="num">64</span>
      <span class="lbl">Features</span>
    </div>
    <div class="stat-card">
      <span class="num">9</span>
      <span class="lbl">Attack Types</span>
    </div>
  </div>

  <!-- ── DATASET ── -->
  <div class="section">
    <div class="section-header">
      <div class="section-icon icon-amber">🗂️</div>
      <h2>Dataset — UNSW-NB15</h2>
    </div>

    <div class="card">
      <h3>📋 About the Dataset</h3>
      <p style="color:#94a3b8; font-size:14px; margin-bottom:16px;">
        The <strong style="color:var(--text)">UNSW-NB15</strong> dataset is a modern, realistic network intrusion dataset created in 2015 
        by the <strong style="color:var(--text)">Australian Centre for Cyber Security (ACCS)</strong> at the University of New South Wales (UNSW Canberra). 
        It was generated using the IXIA PerfectStorm tool with a blend of real modern traffic and synthetic attack traffic, 
        designed specifically to overcome the limitations of ageing datasets like KDD99 and NSL-KDD.
      </p>

      <table>
        <thead><tr><th>Property</th><th>Value</th></tr></thead>
        <tbody>
          <tr><td>Total Features (raw)</td><td>45</td></tr>
          <tr><td>Total Records</td><td>~257,000</td></tr>
          <tr><td>Training Set</td><td>175,341 records</td></tr>
          <tr><td>Testing Set</td><td>82,332 records</td></tr>
          <tr><td>Features after encoding</td><td>64</td></tr>
        </tbody>
      </table>
    </div>

    <div class="card">
      <h3>🎯 Target Variables</h3>
      <table>
        <thead><tr><th>Column</th><th>Type</th><th>Values</th></tr></thead>
        <tbody>
          <tr><td><code>label</code></td><td>Binary</td><td>0 = Normal traffic · 1 = Attack traffic</td></tr>
          <tr><td><code>attack_cat</code></td><td>Multi-class</td><td>Generic, Exploits, Fuzzers, DoS, Reconnaissance, Backdoor, Analysis, Shellcode, Worms</td></tr>
        </tbody>
      </table>
    </div>

    <div class="card">
      <h3>📐 Feature Types &amp; Key Predictors</h3>
      <ul class="list">
        <li>Basic connection features — duration, protocol, service, state</li>
        <li>Content features — packets, bytes, TTL, load</li>
        <li>Statistical traffic features</li>
        <li>Flow-based features</li>
      </ul>
      <p style="margin-top:14px; font-size:13px; color:var(--muted); margin-bottom:8px; font-family: var(--mono);">TOP PREDICTIVE FEATURES DISCOVERED</p>
      <div style="display:flex; flex-wrap:wrap; gap:8px;">
        <code>sbytes</code><code>dbytes</code><code>sttl</code><code>rate</code>
        <code>sload</code><code>dload</code><code>dur</code><code>dmean</code>
      </div>
    </div>
  </div>

  <!-- ── WORKFLOW ── -->
  <div class="section">
    <div class="section-header">
      <div class="section-icon icon-purple">⚙️</div>
      <h2>Project Workflow</h2>
    </div>

    <div class="step">
      <div class="step-num">1</div>
      <div class="step-content">
        <h3>Data Inspection &amp; Cleaning</h3>
        <ul class="list">
          <li>Checked dataset structure using <code>head()</code>, <code>info()</code>, <code>describe()</code></li>
          <li>Verified no missing values and no duplicate records</li>
          <li>Validated all data types for consistency</li>
        </ul>
        <div style="margin-top:12px; display:flex; gap:10px;">
          <span class="result-badge badge-green">✓ No Missing Values</span>
          <span class="result-badge badge-green">✓ No Duplicates</span>
        </div>
      </div>
    </div>

    <div class="step">
      <div class="step-num">2</div>
      <div class="step-content">
        <h3>Exploratory Data Analysis (EDA)</h3>
        <ul class="list">
          <li>Protocol, service, and state distribution analysis</li>
          <li>Attack category distribution visualisation</li>
          <li>Tools used: Matplotlib, Seaborn, Plotly</li>
        </ul>
        <p style="margin-top:10px; font-size:13px; color:#f97316;">
          ⚠ Key findings: Class imbalance present · Some attack categories are rare · Certain protocols dominate traffic
        </p>
      </div>
    </div>

    <div class="step">
      <div class="step-num">3</div>
      <div class="step-content">
        <h3>Feature Engineering &amp; Encoding</h3>
        <ul class="list">
          <li>Removed unnecessary columns (<code>id</code>)</li>
          <li>Label Encoding applied to <code>proto</code></li>
          <li>One-Hot Encoding applied to <code>service</code> and <code>state</code></li>
          <li>Aligned train and test datasets to identical feature spaces</li>
        </ul>
        <div style="margin-top:12px; display:flex; gap:10px; flex-wrap:wrap;">
          <span class="result-badge badge-blue">Train → (175341, 64)</span>
          <span class="result-badge badge-blue">Test → (82332, 64)</span>
        </div>
      </div>
    </div>
  </div>

  <!-- ── MODEL RESULTS ── -->
  <div class="section">
    <div class="section-header">
      <div class="section-icon icon-blue">🤖</div>
      <h2>Model Implementation &amp; Results</h2>
    </div>

    <div class="card" style="margin-bottom:16px;">
      <h3>🔵 Binary Classification — Normal vs Attack</h3>

      <div class="acc-item">
        <div class="acc-label"><span>Random Forest</span><strong>87.29%</strong></div>
        <div class="acc-bar-bg"><div class="acc-bar" style="width:87.29%"></div></div>
      </div>
      <div class="acc-item">
        <div class="acc-label"><span>XGBoost</span><strong>87.33%</strong></div>
        <div class="acc-bar-bg"><div class="acc-bar" style="width:87.33%"></div></div>
      </div>

      <p style="font-size:13px; color:#94a3b8; margin-top:12px;">
        Random Forest achieved a recall of <strong style="color:var(--accent3)">0.99</strong> on the attack class. 
        XGBoost marginally outperforms while both show excellent generalisation. 
        XGBoost parameters: <code>n_estimators=100</code> · <code>learning_rate=0.1</code> · <code>eval_metric=logloss</code>
      </p>

      <p style="font-size:12px; color:var(--muted); margin-top:14px; font-family:var(--mono);">RANDOM FOREST CONFUSION MATRIX</p>
      <pre>[[27208  9792]
 [  676 44656]]</pre>
    </div>

    <div class="card">
      <h3>🔷 Multi-Class Classification — Attack Categories</h3>
      <p style="font-size:13px; color:#94a3b8; margin-bottom:16px;">
        Model: Optimised Random Forest &nbsp;·&nbsp; <strong style="color:var(--accent)">Accuracy: 82.72%</strong>
      </p>
      <table>
        <thead><tr><th>Attack Category</th><th>Performance</th></tr></thead>
        <tbody>
          <tr><td>Generic</td><td class="perf-high">● Very High</td></tr>
          <tr><td>Normal</td><td class="perf-strong">● Strong</td></tr>
          <tr><td>Exploits</td><td class="perf-good">● Good</td></tr>
          <tr><td>Fuzzers</td><td class="perf-mod">● Moderate</td></tr>
          <tr><td>DoS</td><td class="perf-mod">● Moderate</td></tr>
          <tr><td>Analysis</td><td class="perf-low">● Low (Imbalanced class)</td></tr>
          <tr><td>Backdoor</td><td class="perf-low">● Low (Rare class)</td></tr>
        </tbody>
      </table>
      <p style="font-size:13px; color:#f97316; margin-top:14px;">
        ⚠ Binary classification outperforms multi-class due to class imbalance in rare attack types.
      </p>
    </div>
  </div>

  <!-- ── FINAL RESULTS ── -->
  <div class="section">
    <div class="section-header">
      <div class="section-icon icon-green">📊</div>
      <h2>Final Results Summary</h2>
    </div>
    <table>
      <thead>
        <tr><th>Model</th><th>Task</th><th>Accuracy</th></tr>
      </thead>
      <tbody>
        <tr>
          <td>Random Forest</td>
          <td>Binary Classification</td>
          <td><span class="result-badge badge-green">87.29%</span></td>
        </tr>
        <tr>
          <td>XGBoost</td>
          <td>Binary Classification</td>
          <td><span class="result-badge badge-green">87.33% ★</span></td>
        </tr>
        <tr>
          <td>Random Forest (Optimised)</td>
          <td>Multi-Class Classification</td>
          <td><span class="result-badge badge-blue">82.72%</span></td>
        </tr>
      </tbody>
    </table>
  </div>

  <!-- ── TECHNOLOGIES ── -->
  <div class="section">
    <div class="section-header">
      <div class="section-icon icon-amber">🛠️</div>
      <h2>Technologies Used</h2>
    </div>
    <div class="tech-grid">
      <div class="tech-item">Python 3.x<span>Core Language</span></div>
      <div class="tech-item">Pandas<span>Data Manipulation</span></div>
      <div class="tech-item">NumPy<span>Numerical Computing</span></div>
      <div class="tech-item">Matplotlib<span>Visualisation</span></div>
      <div class="tech-item">Seaborn<span>Statistical Plots</span></div>
      <div class="tech-item">Plotly<span>Interactive Charts</span></div>
      <div class="tech-item">Scikit-Learn<span>ML Framework</span></div>
      <div class="tech-item">XGBoost<span>Gradient Boosting</span></div>
    </div>
  </div>

  <!-- ── CONCLUSION ── -->
  <div class="conclusion-box">
    <p class="highlight">🚀 Conclusion</p>
    <p>
      This project successfully demonstrates that <strong style="color:var(--text)">ensemble learning methods</strong> are highly 
      effective for modern intrusion detection. Using the UNSW-NB15 dataset, the system achieves 
      strong performance across both binary and multi-class threat classification.
    </p>
    <div style="display:flex; justify-content:center; gap:14px; flex-wrap:wrap;">
      <span class="result-badge badge-green">~87% Malicious Traffic Detection</span>
      <span class="result-badge badge-blue">~82% Attack Category Classification</span>
    </div>
    <p style="margin-top:18px; font-size:13px;">
      Key insight: Feature importance analysis and handling class imbalance are critical levers 
      for improving rare attack detection in real-world IDS deployments.
    </p>
  </div>

</div>
</body>
</html>
