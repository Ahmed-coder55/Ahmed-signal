 <!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ahmed Signal Pro — Quotex</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;500;700&family=Syne:wght@400;600;800&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #080b0f;
    --surface: #0d1117;
    --surface2: #131920;
    --border: #1e2830;
    --accent: #00c896;
    --accent2: #0ea5e9;
    --danger: #f43f5e;
    --warn: #f59e0b;
    --text: #e2e8f0;
    --muted: #64748b;
    --up: #00c896;
    --down: #f43f5e;
  }
  * { margin:0; padding:0; box-sizing:border-box; }
  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'JetBrains Mono', monospace;
    min-height: 100vh;
    overflow-x: hidden;
  }
  body::before {
    content:'';
    position:fixed; inset:0;
    background-image:
      linear-gradient(rgba(0,200,150,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,200,150,0.03) 1px, transparent 1px);
    background-size: 40px 40px;
    pointer-events: none; z-index:0;
  }

  /* ── LICENSE GATE ── */
  #licenseGate {
    position: fixed; inset: 0;
    background: var(--bg);
    z-index: 999;
    display: flex; align-items: center; justify-content: center;
    padding: 20px;
  }
  .gate-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 20px;
    padding: 40px 36px;
    width: 100%; max-width: 420px;
    position: relative;
    overflow: hidden;
    animation: fadeDown 0.5s ease both;
  }
  .gate-card::before {
    content:'';
    position:absolute; top:0; left:0; right:0; height:2px;
    background: linear-gradient(90deg, transparent, var(--accent), transparent);
  }
  .gate-logo {
    display:flex; align-items:center; gap:12px; margin-bottom:28px;
  }
  .gate-logo-icon {
    width:44px; height:44px;
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    border-radius:10px;
    display:flex; align-items:center; justify-content:center;
    font-size:20px;
  }
  .gate-title { font-family:'Syne',sans-serif; font-size:22px; font-weight:800; }
  .gate-sub { font-size:10px; color:var(--muted); letter-spacing:2px; text-transform:uppercase; margin-top:2px; }

  .gate-label { font-size:10px; color:var(--muted); letter-spacing:2px; text-transform:uppercase; margin-bottom:10px; }
  .gate-input {
    width:100%;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 14px 16px;
    color: var(--text);
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    letter-spacing: 1px;
    outline: none;
    transition: border-color 0.2s;
    margin-bottom: 14px;
  }
  .gate-input:focus { border-color: var(--accent); }
  .gate-input::placeholder { color: var(--muted); }
  .gate-input.error { border-color: var(--danger); }
  .gate-input.success { border-color: var(--accent); }

  .gate-btn {
    width:100%; padding:14px;
    background: linear-gradient(135deg, rgba(0,200,150,0.2), rgba(14,165,233,0.2));
    border: 1px solid var(--accent);
    border-radius: 10px;
    color: var(--accent);
    font-family:'Syne',sans-serif;
    font-size:14px; font-weight:700;
    letter-spacing:2px; text-transform:uppercase;
    cursor:pointer; transition:all 0.2s;
  }
  .gate-btn:hover {
    background: linear-gradient(135deg, rgba(0,200,150,0.3), rgba(14,165,233,0.3));
    box-shadow: 0 0 20px rgba(0,200,150,0.2);
  }
  .gate-btn:disabled { opacity:0.5; cursor:not-allowed; }

  .gate-msg {
    margin-top:12px; font-size:11px; text-align:center;
    min-height:16px; transition: color 0.2s;
  }
  .gate-msg.error { color: var(--danger); }
  .gate-msg.success { color: var(--accent); }

  .key-type-badge {
    display:inline-block; margin-top:16px;
    padding:6px 14px; border-radius:20px;
    font-size:11px; letter-spacing:1px; text-transform:uppercase;
    border:1px solid var(--accent); color:var(--accent);
    background: rgba(0,200,150,0.08);
  }
  .key-expiry { font-size:10px; color:var(--muted); margin-top:6px; text-align:center; }

  .gate-divider { border:none; border-top:1px solid var(--border); margin:20px 0; }
  .gate-hint { font-size:10px; color:var(--muted); text-align:center; line-height:1.7; }

  /* ── SESSION BANNER ── */
  #sessionBanner {
    display:none;
    background: rgba(0,200,150,0.08);
    border:1px solid rgba(0,200,150,0.25);
    border-radius:10px;
    padding:10px 16px;
    margin-bottom:16px;
    font-size:11px;
    display:none; align-items:center; gap:10px;
    animation: fadeDown 0.4s ease both;
  }
  .banner-key { color:var(--accent); flex:1; }
  .banner-expiry { color:var(--muted); }
  .banner-type {
    padding:3px 8px; border-radius:4px; font-size:10px; letter-spacing:1px;
    background:rgba(0,200,150,0.15); color:var(--accent);
  }
  .banner-logout {
    background:none; border:none; color:var(--muted); cursor:pointer;
    font-size:11px; letter-spacing:1px; text-transform:uppercase;
    transition:color 0.2s;
  }
  .banner-logout:hover { color:var(--danger); }

  /* ── APP ── */
  .wrapper { position:relative; z-index:1; max-width:1200px; margin:0 auto; padding:20px; }
  header {
    display:flex; align-items:center; justify-content:space-between;
    padding:16px 24px;
    border:1px solid var(--border);
    background:var(--surface);
    border-radius:12px;
    margin-bottom:20px;
    animation:fadeDown 0.5s ease both;
  }
  .logo { display:flex; align-items:center; gap:12px; }
  .logo-icon {
    width:36px; height:36px;
    background:linear-gradient(135deg, var(--accent), var(--accent2));
    border-radius:8px;
    display:flex; align-items:center; justify-content:center; font-size:16px;
  }
  .logo-text { font-family:'Syne',sans-serif; font-weight:800; font-size:18px; letter-spacing:-0.5px; }
  .logo-sub { font-size:10px; color:var(--muted); letter-spacing:2px; text-transform:uppercase; }
  .header-right { display:flex; align-items:center; gap:16px; }
  .live-badge {
    display:flex; align-items:center; gap:6px;
    background:rgba(0,200,150,0.1);
    border:1px solid rgba(0,200,150,0.3);
    padding:4px 10px; border-radius:20px;
    font-size:11px; color:var(--accent);
  }
  .live-dot {
    width:6px; height:6px; border-radius:50%;
    background:var(--accent); animation:pulse 1.5s infinite;
  }
  .clock { font-size:13px; color:var(--muted); }

  .pair-section { margin-bottom:20px; animation:fadeDown 0.5s 0.1s ease both; }
  .section-label { font-size:10px; color:var(--muted); letter-spacing:2px; text-transform:uppercase; margin-bottom:10px; }
  .pair-grid { display:flex; flex-wrap:wrap; gap:8px; }
  .pair-btn {
    padding:8px 16px;
    background:var(--surface); border:1px solid var(--border);
    border-radius:8px; color:var(--muted);
    font-family:'JetBrains Mono',monospace; font-size:12px;
    cursor:pointer; transition:all 0.2s; letter-spacing:0.5px;
  }
  .pair-btn:hover { border-color:var(--accent); color:var(--accent); }
  .pair-btn.active { background:rgba(0,200,150,0.12); border-color:var(--accent); color:var(--accent); }

  .tf-row { display:flex; gap:8px; margin-bottom:20px; animation:fadeDown 0.5s 0.15s ease both; }
  .tf-btn {
    padding:8px 20px;
    background:var(--surface); border:1px solid var(--border);
    border-radius:8px; color:var(--muted);
    font-family:'JetBrains Mono',monospace; font-size:12px;
    cursor:pointer; transition:all 0.2s;
  }
  .tf-btn:hover { border-color:var(--accent2); color:var(--accent2); }
  .tf-btn.active { background:rgba(14,165,233,0.12); border-color:var(--accent2); color:var(--accent2); }

  .signal-card {
    background:var(--surface); border:1px solid var(--border);
    border-radius:16px; padding:28px; margin-bottom:20px;
    animation:fadeDown 0.5s 0.2s ease both;
    position:relative; overflow:hidden;
  }
  .signal-card::before {
    content:''; position:absolute; top:0; left:0; right:0; height:2px;
    background:linear-gradient(90deg, transparent, var(--accent), transparent);
    opacity:0; transition:opacity 0.4s;
  }
  .signal-card.has-signal::before { opacity:1; }
  .signal-top { display:flex; align-items:flex-start; justify-content:space-between; margin-bottom:24px; }
  .signal-pair { font-family:'Syne',sans-serif; font-size:28px; font-weight:800; letter-spacing:-1px; }
  .signal-tf { font-size:11px; color:var(--muted); margin-top:2px; }
  .direction-badge {
    padding:10px 28px; border-radius:10px;
    font-family:'Syne',sans-serif; font-size:22px; font-weight:800;
    letter-spacing:2px; transition:all 0.3s;
  }
  .direction-badge.up { background:rgba(0,200,150,0.15); border:2px solid var(--up); color:var(--up); }
  .direction-badge.down { background:rgba(244,63,94,0.15); border:2px solid var(--down); color:var(--down); }
  .direction-badge.wait { background:rgba(245,158,11,0.1); border:2px solid var(--warn); color:var(--warn); font-size:14px; padding:10px 16px; }

  .strength-section { margin-bottom:24px; }
  .strength-label { display:flex; justify-content:space-between; font-size:11px; color:var(--muted); margin-bottom:8px; }
  .strength-value { color:var(--text); font-weight:500; }
  .strength-bar-bg { height:6px; background:var(--border); border-radius:3px; overflow:hidden; }
  .strength-bar-fill { height:100%; border-radius:3px; background:linear-gradient(90deg, var(--accent2), var(--accent)); transition:width 0.8s cubic-bezier(0.4,0,0.2,1); width:0%; }

  .indicators-grid { display:grid; grid-template-columns:repeat(3,1fr); gap:12px; margin-bottom:24px; }
  .ind-card { background:var(--surface2); border:1px solid var(--border); border-radius:10px; padding:14px; transition:border-color 0.3s; }
  .ind-card.bullish { border-color:rgba(0,200,150,0.4); }
  .ind-card.bearish { border-color:rgba(244,63,94,0.4); }
  .ind-card.neutral { border-color:rgba(245,158,11,0.3); }
  .ind-name { font-size:10px; color:var(--muted); letter-spacing:1px; text-transform:uppercase; margin-bottom:6px; }
  .ind-value { font-size:15px; font-weight:500; margin-bottom:4px; }
  .ind-signal { font-size:10px; letter-spacing:1px; }
  .ind-signal.bullish { color:var(--up); }
  .ind-signal.bearish { color:var(--down); }
  .ind-signal.neutral { color:var(--warn); }

  .confluence-row { display:flex; align-items:center; gap:16px; padding:16px; background:var(--surface2); border:1px solid var(--border); border-radius:10px; }
  .conf-score-big { font-family:'Syne',sans-serif; font-size:40px; font-weight:800; min-width:80px; text-align:center; line-height:1; }
  .conf-details { flex:1; }
  .conf-title { font-size:10px; color:var(--muted); letter-spacing:2px; text-transform:uppercase; margin-bottom:6px; }
  .conf-bars { display:flex; flex-direction:column; gap:4px; }
  .conf-bar-row { display:flex; align-items:center; gap:8px; font-size:10px; }
  .conf-bar-label { color:var(--muted); width:60px; }
  .conf-mini-bg { flex:1; height:4px; background:var(--border); border-radius:2px; overflow:hidden; }
  .conf-mini-fill { height:100%; border-radius:2px; transition:width 0.7s ease; }
  .fill-up { background:var(--up); }
  .fill-down { background:var(--down); }
  .fill-neutral { background:var(--warn); }

  .stats-row { display:grid; grid-template-columns:repeat(4,1fr); gap:12px; margin-bottom:20px; animation:fadeDown 0.5s 0.25s ease both; }
  .stat-card { background:var(--surface); border:1px solid var(--border); border-radius:10px; padding:16px; text-align:center; }
  .stat-val { font-family:'Syne',sans-serif; font-size:22px; font-weight:700; margin-bottom:4px; }
  .stat-label { font-size:10px; color:var(--muted); letter-spacing:1px; text-transform:uppercase; }

  .analyze-btn {
    width:100%; padding:16px;
    background:linear-gradient(135deg, rgba(0,200,150,0.2), rgba(14,165,233,0.2));
    border:1px solid var(--accent); border-radius:10px;
    color:var(--accent);
    font-family:'Syne',sans-serif; font-size:15px; font-weight:700;
    letter-spacing:2px; cursor:pointer; transition:all 0.2s;
    margin-bottom:20px; text-transform:uppercase;
    animation:fadeDown 0.5s 0.3s ease both;
  }
  .analyze-btn:hover { background:linear-gradient(135deg,rgba(0,200,150,0.3),rgba(14,165,233,0.3)); box-shadow:0 0 20px rgba(0,200,150,0.2); transform:translateY(-1px); }
  .analyze-btn:active { transform:translateY(0); }
  .analyze-btn:disabled { opacity:0.5; cursor:not-allowed; transform:none; }

  .history-section { background:var(--surface); border:1px solid var(--border); border-radius:16px; padding:20px; animation:fadeDown 0.5s 0.35s ease both; }
  .history-header { display:flex; justify-content:space-between; align-items:center; margin-bottom:14px; }
  .clear-btn { font-size:10px; color:var(--muted); background:none; border:none; cursor:pointer; letter-spacing:1px; text-transform:uppercase; transition:color 0.2s; }
  .clear-btn:hover { color:var(--danger); }
  .history-list { display:flex; flex-direction:column; gap:8px; max-height:220px; overflow-y:auto; }
  .history-item { display:flex; align-items:center; gap:12px; padding:10px 12px; background:var(--surface2); border:1px solid var(--border); border-radius:8px; font-size:12px; animation:slideIn 0.3s ease both; }
  .h-dir { font-weight:700; font-size:11px; letter-spacing:1px; min-width:50px; }
  .h-dir.up { color:var(--up); }
  .h-dir.down { color:var(--down); }
  .h-pair { color:var(--text); min-width:80px; }
  .h-tf { color:var(--muted); min-width:40px; }
  .h-score { color:var(--accent2); margin-left:auto; }
  .h-time { color:var(--muted); font-size:10px; }
  .empty-history { text-align:center; color:var(--muted); font-size:12px; padding:20px; }

  .loading-overlay {
    position:absolute; inset:0;
    background:rgba(8,11,15,0.85);
    border-radius:16px;
    display:none; align-items:center; justify-content:center;
    flex-direction:column; gap:12px; z-index:10;
    backdrop-filter:blur(4px);
  }
  .loading-overlay.active { display:flex; }
  .spinner { width:40px; height:40px; border:2px solid var(--border); border-top-color:var(--accent); border-radius:50%; animation:spin 0.8s linear infinite; }
  .loading-text { font-size:12px; color:var(--muted); letter-spacing:2px; text-transform:uppercase; }

  .disclaimer { margin-top:16px; padding:12px 16px; background:rgba(245,158,11,0.06); border:1px solid rgba(245,158,11,0.2); border-radius:8px; font-size:10px; color:var(--muted); line-height:1.6; animation:fadeDown 0.5s 0.4s ease both; }

  @keyframes fadeDown { from{opacity:0;transform:translateY(-10px)} to{opacity:1;transform:translateY(0)} }
  @keyframes slideIn { from{opacity:0;transform:translateX(-8px)} to{opacity:1;transform:translateX(0)} }
  @keyframes pulse { 0%,100%{opacity:1} 50%{opacity:0.3} }
  @keyframes spin { to{transform:rotate(360deg)} }
  @keyframes glow { 0%,100%{box-shadow:0 0 10px rgba(0,200,150,0.2)} 50%{box-shadow:0 0 25px rgba(0,200,150,0.5)} }
  .signal-card.has-signal { animation:glow 2s ease infinite; }
  ::-webkit-scrollbar { width:4px; }
  ::-webkit-scrollbar-track { background:var(--surface2); }
  ::-webkit-scrollbar-thumb { background:var(--border); border-radius:2px; }

  @media(max-width:600px) {
    .indicators-grid { grid-template-columns:repeat(2,1fr); }
    .stats-row { grid-template-columns:repeat(2,1fr); }
    .signal-pair { font-size:20px; }
  }
</style>
</head>
<body>

<!-- ══════════════════════════════════════════════════════════ -->
<!--  LICENSE GATE                                              -->
<!-- ══════════════════════════════════════════════════════════ -->
<div id="licenseGate">
  <div class="gate-card">
    <div class="gate-logo">
      <div class="gate-logo-icon">⚡</div>
      <div>
        <div class="gate-title">Ahmed Signal Pro</div>
        <div class="gate-sub">License Activation</div>
      </div>
    </div>
    <div class="gate-label">Enter Your License Key</div>
    <input type="text" class="gate-input" id="keyInput"
           placeholder="XXXX-XXXXX-XXXXX-XXXXX-XXXXX"
           autocomplete="off" spellcheck="false"
           oninput="formatKeyInput(this)" />
    <button class="gate-btn" id="activateBtn" onclick="activateKey()">
      🔓 ACTIVATE
    </button>
    <div class="gate-msg" id="gateMsg"></div>
    <hr class="gate-divider">
    <div class="gate-hint">
      Keys are available in 7-day, 30-day &amp; lifetime plans.<br>
      Contact the admin to get your key.
    </div>
  </div>
</div>

<!-- ══════════════════════════════════════════════════════════ -->
<!--  MAIN APP                                                  -->
<!-- ══════════════════════════════════════════════════════════ -->
<div class="wrapper" id="app" style="display:none">

  <header>
    <div class="logo">
      <div class="logo-icon">⚡</div>
      <div>
        <div class="logo-text">Ahmed Signal Pro</div>
        <div class="logo-sub">Quotex · Binary Options</div>
      </div>
    </div>
    <div class="header-right">
      <div class="live-badge"><div class="live-dot"></div>LIVE</div>
      <div class="clock" id="clock">--:--:--</div>
    </div>
  </header>

  <!-- Session banner -->
  <div id="sessionBanner">
    <span class="banner-key" id="bannerKey">—</span>
    <span class="banner-type" id="bannerType">—</span>
    <span class="banner-expiry" id="bannerExpiry"></span>
    <button class="banner-logout" onclick="logout()">Logout</button>
  </div>

  <!-- PAIR SELECTOR -->
  <div class="pair-section">
    <div class="section-label">Currency Pair</div>
    <div class="pair-grid" id="pairGrid">
      <button class="pair-btn active" data-pair="EUR/USD">EUR/USD</button>
      <button class="pair-btn" data-pair="GBP/USD">GBP/USD</button>
      <button class="pair-btn" data-pair="USD/JPY">USD/JPY</button>
      <button class="pair-btn" data-pair="AUD/USD">AUD/USD</button>
      <button class="pair-btn" data-pair="EUR/GBP">EUR/GBP</button>
      <button class="pair-btn" data-pair="USD/CHF">USD/CHF</button>
      <button class="pair-btn" data-pair="GBP/JPY">GBP/JPY</button>
      <button class="pair-btn" data-pair="NZD/USD">NZD/USD</button>
    </div>
  </div>

  <!-- TIMEFRAME -->
  <div class="tf-row">
    <button class="tf-btn active" data-tf="1M">1 MIN</button>
    <button class="tf-btn" data-tf="3M">3 MIN</button>
    <button class="tf-btn" data-tf="5M">5 MIN</button>
  </div>

  <!-- SIGNAL CARD -->
  <div class="signal-card" id="signalCard">
    <div class="loading-overlay" id="loadingOverlay">
      <div class="spinner"></div>
      <div class="loading-text">Analyzing Market...</div>
    </div>
    <div class="signal-top">
      <div>
        <div class="signal-pair" id="displayPair">EUR/USD</div>
        <div class="signal-tf" id="displayTF">1 MIN · Quotex</div>
      </div>
      <div class="signal-direction">
        <div class="direction-badge wait" id="dirBadge">WAITING</div>
      </div>
    </div>
    <div class="strength-section">
      <div class="strength-label">
        <span>Signal Strength</span>
        <span class="strength-value" id="strengthVal">—</span>
      </div>
      <div class="strength-bar-bg">
        <div class="strength-bar-fill" id="strengthBar"></div>
      </div>
    </div>
    <div class="section-label">Indicators</div>
    <div class="indicators-grid">
      <div class="ind-card neutral"><div class="ind-name">EMA 9/21</div><div class="ind-value" id="emaVal">—</div><div class="ind-signal neutral" id="emaSig">NEUTRAL</div></div>
      <div class="ind-card neutral"><div class="ind-name">RSI (14)</div><div class="ind-value" id="rsiVal">—</div><div class="ind-signal neutral" id="rsiSig">NEUTRAL</div></div>
      <div class="ind-card neutral"><div class="ind-name">MACD</div><div class="ind-value" id="macdVal">—</div><div class="ind-signal neutral" id="macdSig">NEUTRAL</div></div>
      <div class="ind-card neutral"><div class="ind-name">Bollinger</div><div class="ind-value" id="bbVal">—</div><div class="ind-signal neutral" id="bbSig">NEUTRAL</div></div>
      <div class="ind-card neutral"><div class="ind-name">Stochastic</div><div class="ind-value" id="stochVal">—</div><div class="ind-signal neutral" id="stochSig">NEUTRAL</div></div>
      <div class="ind-card neutral"><div class="ind-name">Candle Pattern</div><div class="ind-value" id="candleVal" style="font-size:12px">—</div><div class="ind-signal neutral" id="candleSig">NEUTRAL</div></div>
    </div>
    <div class="confluence-row">
      <div class="conf-score-big" id="confScore">—</div>
      <div class="conf-details">
        <div class="conf-title">Confluence Score</div>
        <div class="conf-bars">
          <div class="conf-bar-row"><span class="conf-bar-label">Bullish</span><div class="conf-mini-bg"><div class="conf-mini-fill fill-up" id="bullBar" style="width:0%"></div></div></div>
          <div class="conf-bar-row"><span class="conf-bar-label">Bearish</span><div class="conf-mini-bg"><div class="conf-mini-fill fill-down" id="bearBar" style="width:0%"></div></div></div>
          <div class="conf-bar-row"><span class="conf-bar-label">Neutral</span><div class="conf-mini-bg"><div class="conf-mini-fill fill-neutral" id="neutBar" style="width:0%"></div></div></div>
        </div>
      </div>
    </div>
  </div>

  <button class="analyze-btn" id="analyzeBtn" onclick="runAnalysis()">⚡ ANALYZE SIGNAL</button>

  <div class="stats-row">
    <div class="stat-card"><div class="stat-val" id="statTotal">0</div><div class="stat-label">Total Signals</div></div>
    <div class="stat-card"><div class="stat-val" style="color:var(--up)" id="statUp">0</div><div class="stat-label">BUY Signals</div></div>
    <div class="stat-card"><div class="stat-val" style="color:var(--down)" id="statDown">0</div><div class="stat-label">SELL Signals</div></div>
    <div class="stat-card"><div class="stat-val" style="color:var(--accent2)" id="statAvgStr">—</div><div class="stat-label">Avg Strength</div></div>
  </div>

  <div class="history-section">
    <div class="history-header">
      <div class="section-label" style="margin:0">Signal History</div>
      <button class="clear-btn" onclick="clearHistory()">Clear All</button>
    </div>
    <div class="history-list" id="historyList">
      <div class="empty-history">No signals yet — tap Analyze to begin</div>
    </div>
  </div>

  <div class="disclaimer">
    ⚠ This dashboard is for educational reference only. Signals are generated from simulated multi-indicator analysis and do not represent financial advice. Binary options trading involves significant risk. Always trade responsibly and manage your risk carefully.
  </div>
</div>

<script>
// ══════════════════════════════════════════════════════════════
//  LICENSE KEY DATABASE
//  Format: KEY -> { type, days (null=lifetime), label }
// ══════════════════════════════════════════════════════════════
const LICENSE_DB = {
  // ── 7-DAY KEYS ──
  '7DAY-2F4WT-6HIHN-6MQMO-I4WE1': { type:'7day', days:7,   label:'7-Day' },
  '7DAY-7OJNW-H19FJ-HWSTO-4MVBL': { type:'7day', days:7,   label:'7-Day' },
  '7DAY-AEWEP-GM0QJ-BYNNL-GM398': { type:'7day', days:7,   label:'7-Day' },
  '7DAY-UYQ1B-5RFRC-RDCAT-UXMTN': { type:'7day', days:7,   label:'7-Day' },
  '7DAY-JRMU0-0X84J-94OFJ-Q9804': { type:'7day', days:7,   label:'7-Day' },
  // ── 30-DAY KEYS ──
  '30DAY-WWAFR-YR5GH-4IMVK-2MYDH': { type:'30day', days:30, label:'30-Day' },
  '30DAY-WB3W6-F87BT-21PYQ-02ZX7': { type:'30day', days:30, label:'30-Day' },
  '30DAY-O3K3O-H8A2P-KU6ZK-U4C60': { type:'30day', days:30, label:'30-Day' },
  '30DAY-5IP2X-1AOV9-60PTW-TLE81': { type:'30day', days:30, label:'30-Day' },
  '30DAY-9XUCD-FZK7W-ZFK5N-A6TDI': { type:'30day', days:30, label:'30-Day' },
  // ── LIFETIME KEYS ──
  'LIFE-VP7A5-4BUT8-5VO91-DL875':  { type:'lifetime', days:null, label:'Lifetime' },
  'LIFE-SVG7C-QM0LR-RKZ33-WMEF1':  { type:'lifetime', days:null, label:'Lifetime' },
  'LIFE-K349Q-IF28P-GP5S4-HG8OT':  { type:'lifetime', days:null, label:'Lifetime' },
  'LIFE-IGJCG-27FL8-NI7QQ-C4YUV':  { type:'lifetime', days:null, label:'Lifetime' },
  'LIFE-5R3N2-PQEL3-IPMZN-GG2SW':  { type:'lifetime', days:null, label:'Lifetime' },
};

// ══════════════════════════════════════════════════════════════
//  SESSION MANAGEMENT  (localStorage)
// ══════════════════════════════════════════════════════════════
const LS_KEY = 'asp_session';

function saveSession(key, meta) {
  const now = Date.now();
  const expiry = meta.days ? now + meta.days * 86400000 : null;
  localStorage.setItem(LS_KEY, JSON.stringify({ key, type: meta.type, label: meta.label, expiry, activatedAt: now }));
}

function loadSession() {
  try {
    const raw = localStorage.getItem(LS_KEY);
    if (!raw) return null;
    const s = JSON.parse(raw);
    if (s.expiry && Date.now() > s.expiry) {
      localStorage.removeItem(LS_KEY);
      return null;
    }
    return s;
  } catch { return null; }
}

function logout() {
  localStorage.removeItem(LS_KEY);
  location.reload();
}

// ══════════════════════════════════════════════════════════════
//  KEY INPUT FORMATTER
// ══════════════════════════════════════════════════════════════
function formatKeyInput(el) {
  let val = el.value.toUpperCase().replace(/[^A-Z0-9-]/g,'');
  el.value = val;
}

// ══════════════════════════════════════════════════════════════
//  ACTIVATE KEY
// ══════════════════════════════════════════════════════════════
function activateKey() {
  const input = document.getElementById('keyInput');
  const msg   = document.getElementById('gateMsg');
  const btn   = document.getElementById('activateBtn');
  const key   = input.value.trim().toUpperCase();

  msg.textContent = ''; msg.className = 'gate-msg';
  input.className = 'gate-input';

  if (!key) { msg.textContent = 'Please enter your license key.'; msg.className='gate-msg error'; return; }

  btn.disabled = true;
  btn.textContent = 'Checking...';

  setTimeout(() => {
    const meta = LICENSE_DB[key];
    if (!meta) {
      input.className = 'gate-input error';
      msg.textContent = '✗ Invalid license key. Please check and try again.';
      msg.className = 'gate-msg error';
      btn.disabled = false;
      btn.textContent = '🔓 ACTIVATE';
      return;
    }

    saveSession(key, meta);
    msg.textContent = '✓ License activated! Loading dashboard...';
    msg.className = 'gate-msg success';
    btn.textContent = '✓ SUCCESS';

    setTimeout(() => launchApp(loadSession()), 800);
  }, 700);
}

// ══════════════════════════════════════════════════════════════
//  LAUNCH APP
// ══════════════════════════════════════════════════════════════
function launchApp(session) {
  document.getElementById('licenseGate').style.display = 'none';
  const app = document.getElementById('app');
  app.style.display = 'block';

  // Show session banner
  const banner = document.getElementById('sessionBanner');
  banner.style.display = 'flex';
  document.getElementById('bannerKey').textContent = session.key;
  document.getElementById('bannerType').textContent = session.label;

  if (session.expiry) {
    const daysLeft = Math.ceil((session.expiry - Date.now()) / 86400000);
    document.getElementById('bannerExpiry').textContent = `Expires in ${daysLeft} day${daysLeft !== 1 ? 's' : ''}`;
  } else {
    document.getElementById('bannerExpiry').textContent = '∞ Lifetime Access';
  }
}

// ══════════════════════════════════════════════════════════════
//  ON LOAD — check existing session
// ══════════════════════════════════════════════════════════════
window.addEventListener('DOMContentLoaded', () => {
  const session = loadSession();
  if (session) {
    launchApp(session);
  }
});

// ══════════════════════════════════════════════════════════════
//  CLOCK
// ══════════════════════════════════════════════════════════════
function updateClock() {
  const el = document.getElementById('clock');
  if (el) el.textContent = new Date().toTimeString().slice(0,8);
}
setInterval(updateClock, 1000); updateClock();

// ══════════════════════════════════════════════════════════════
//  PAIR / TF SELECTORS
// ══════════════════════════════════════════════════════════════
let selectedPair = 'EUR/USD';
let selectedTF   = '1M';

document.addEventListener('click', e => {
  const pb = e.target.closest('.pair-btn');
  if (pb) {
    document.querySelectorAll('.pair-btn').forEach(b=>b.classList.remove('active'));
    pb.classList.add('active');
    selectedPair = pb.dataset.pair;
    document.getElementById('displayPair').textContent = selectedPair;
    resetSignalCard();
  }
  const tb = e.target.closest('.tf-btn');
  if (tb) {
    document.querySelectorAll('.tf-btn').forEach(b=>b.classList.remove('active'));
    tb.classList.add('active');
    selectedTF = tb.dataset.tf;
    document.getElementById('displayTF').textContent = selectedTF + ' · Quotex';
    resetSignalCard();
  }
});

// ══════════════════════════════════════════════════════════════
//  SIGNAL ENGINE
// ══════════════════════════════════════════════════════════════
let history2 = [];
let stats = { total:0, up:0, down:0, strengths:[] };
let isAnalyzing = false;

function seededRandom(seed) {
  let s = seed * 9301 + 49297;
  return function() { s=(s*9301+49297)%233280; return s/233280; };
}

function simulateIndicators(pair, tf) {
  const seed = pair.charCodeAt(0)+pair.charCodeAt(4)+
    new Date().getMinutes()+new Date().getSeconds()%30+
    (tf==='1M'?1:tf==='3M'?3:5);
  const rng = seededRandom(seed);
  const rsi  = Math.round(20+rng()*60);
  const ema9  = 1.1000+(rng()-0.5)*0.02;
  const ema21 = ema9-(rng()-0.48)*0.005;
  const macdH = (rng()-0.5)*0.002;
  const stoch = Math.round(rng()*100);
  const bbPos = rng();
  const patterns=['Bullish Engulf','Bearish Engulf','Doji','Hammer','Shooting Star','Pin Bar','Morning Star','Evening Star'];
  const pi = Math.floor(rng()*patterns.length);
  const pattern = patterns[pi];
  const patB=[0,3,6].includes(pi), patBr=[1,4,7].includes(pi);

  let bull=0, bear=0, neut=0;
  const r={};

  const emaCross=ema9>ema21;
  r.ema={value:emaCross?'9 > 21':'9 < 21', signal:emaCross?'BULLISH':'BEARISH', class:emaCross?'bullish':'bearish'};
  emaCross?bull++:bear++;

  let rsiSig, rsiClass;
  if(rsi<35){rsiSig='OVERSOLD';rsiClass='bullish';bull++;}
  else if(rsi>65){rsiSig='OVERBOUGHT';rsiClass='bearish';bear++;}
  else{rsiSig='NEUTRAL';rsiClass='neutral';neut++;}
  r.rsi={value:rsi.toString(),signal:rsiSig,class:rsiClass};

  const mb=macdH>0;
  r.macd={value:macdH.toFixed(5),signal:mb?'BULLISH':'BEARISH',class:mb?'bullish':'bearish'};
  mb?bull++:bear++;

  let bbSig,bbClass,bbLabel;
  if(bbPos<0.15){bbSig='BOUNCE UP';bbClass='bullish';bbLabel='At Lower Band';bull++;}
  else if(bbPos>0.85){bbSig='BOUNCE DOWN';bbClass='bearish';bbLabel='At Upper Band';bear++;}
  else{bbSig='MID RANGE';bbClass='neutral';bbLabel='Mid Band';neut++;}
  r.bb={value:bbLabel,signal:bbSig,class:bbClass};

  let stSig,stClass;
  if(stoch<25){stSig='OVERSOLD';stClass='bullish';bull++;}
  else if(stoch>75){stSig='OVERBOUGHT';stClass='bearish';bear++;}
  else{stSig='NEUTRAL';stClass='neutral';neut++;}
  r.stoch={value:stoch.toString(),signal:stSig,class:stClass};

  let cSig,cClass;
  if(patB){cSig='BULLISH';cClass='bullish';bull++;}
  else if(patBr){cSig='BEARISH';cClass='bearish';bear++;}
  else{cSig='NEUTRAL';cClass='neutral';neut++;}
  r.candle={value:pattern,signal:cSig,class:cClass};

  const total=bull+bear+neut;
  const dir=bull>bear?'UP':(bear>bull?'DOWN':'WAIT');
  const strength=Math.round((Math.max(bull,bear)/total)*100);

  return{results:r,bullCount:bull,bearCount:bear,neutCount:neut,total,direction:dir,strength,confScore:Math.max(bull,bear)};
}

function runAnalysis() {
  if(isAnalyzing) return;
  isAnalyzing=true;
  const btn=document.getElementById('analyzeBtn');
  const ov=document.getElementById('loadingOverlay');
  btn.disabled=true; ov.classList.add('active');
  setTimeout(()=>{
    const data=simulateIndicators(selectedPair,selectedTF);
    renderSignal(data); addToHistory(data); updateStats(data);
    ov.classList.remove('active'); btn.disabled=false; isAnalyzing=false;
  },1400);
}

function renderSignal(data) {
  const{results:r,bullCount,bearCount,neutCount,total,direction,strength,confScore}=data;
  const card=document.getElementById('signalCard');
  const badge=document.getElementById('dirBadge');
  badge.className='direction-badge';
  if(direction==='UP'){badge.className+=' up';badge.textContent='▲ CALL';}
  else if(direction==='DOWN'){badge.className+=' down';badge.textContent='▼ PUT';}
  else{badge.className+=' wait';badge.textContent='⏸ WAIT';}
  card.classList.add('has-signal');
  document.getElementById('strengthVal').textContent=strength+'%';
  document.getElementById('strengthBar').style.width=strength+'%';
  setInd('ema',r.ema); setInd('rsi',r.rsi); setInd('macd',r.macd);
  setInd('bb',r.bb);   setInd('stoch',r.stoch); setInd('candle',r.candle);
  const sc=document.getElementById('confScore');
  sc.textContent=confScore+'/6';
  sc.style.color=direction==='UP'?'var(--up)':direction==='DOWN'?'var(--down)':'var(--warn)';
  document.getElementById('bullBar').style.width=((bullCount/total)*100)+'%';
  document.getElementById('bearBar').style.width=((bearCount/total)*100)+'%';
  document.getElementById('neutBar').style.width=((neutCount/total)*100)+'%';
}

function setInd(key,data) {
  const v=document.getElementById(key+'Val');
  const s=document.getElementById(key+'Sig');
  const c=v.closest('.ind-card');
  v.textContent=data.value; s.textContent=data.signal;
  s.className='ind-signal '+data.class;
  c.className='ind-card '+data.class;
}

function addToHistory(data) {
  const now=new Date().toTimeString().slice(0,8);
  history2.unshift({dir:data.direction,pair:selectedPair,tf:selectedTF,score:data.confScore+'/6',time:now});
  if(history2.length>20) history2.pop();
  renderHistory();
}

function renderHistory() {
  const list=document.getElementById('historyList');
  if(!history2.length){list.innerHTML='<div class="empty-history">No signals yet — tap Analyze to begin</div>';return;}
  list.innerHTML=history2.map(h=>`
    <div class="history-item">
      <span class="h-dir ${h.dir==='UP'?'up':h.dir==='DOWN'?'down':''}">${h.dir==='UP'?'▲ CALL':h.dir==='DOWN'?'▼ PUT':'⏸ WAIT'}</span>
      <span class="h-pair">${h.pair}</span>
      <span class="h-tf">${h.tf}</span>
      <span class="h-score">${h.score}</span>
      <span class="h-time">${h.time}</span>
    </div>`).join('');
}

function clearHistory(){
  history2=[]; stats={total:0,up:0,down:0,strengths:[]};
  updateStatsDisplay(); renderHistory();
}

function updateStats(data){
  stats.total++;
  if(data.direction==='UP') stats.up++;
  else if(data.direction==='DOWN') stats.down++;
  stats.strengths.push(data.strength);
  updateStatsDisplay();
}

function updateStatsDisplay(){
  document.getElementById('statTotal').textContent=stats.total;
  document.getElementById('statUp').textContent=stats.up;
  document.getElementById('statDown').textContent=stats.down;
  const avg=stats.strengths.length?Math.round(stats.strengths.reduce((a,b)=>a+b,0)/stats.strengths.length):null;
  document.getElementById('statAvgStr').textContent=avg!==null?avg+'%':'—';
}

function resetSignalCard(){
  document.getElementById('dirBadge').className='direction-badge wait';
  document.getElementById('dirBadge').textContent='WAITING';
  document.getElementById('strengthVal').textContent='—';
  document.getElementById('strengthBar').style.width='0%';
  document.getElementById('confScore').textContent='—';
  document.getElementById('confScore').style.color='';
  ['bullBar','bearBar','neutBar'].forEach(id=>document.getElementById(id).style.width='0%');
  document.getElementById('signalCard').classList.remove('has-signal');
  ['ema','rsi','macd','bb','stoch','candle'].forEach(k=>{
    document.getElementById(k+'Val').textContent='—';
    const s=document.getElementById(k+'Sig');
    s.textContent='NEUTRAL'; s.className='ind-signal neutral';
    s.closest('.ind-card').className='ind-card neutral';
  });
}
</script>
</body>
</html>
