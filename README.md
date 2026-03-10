<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Konversi Bilangan</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet"/>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: 'Inter', sans-serif;
      min-height: 100vh;
      background: linear-gradient(135deg, #667eea 0%, #ffffff 100%);
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 20px;
    }

    .card {
      background: white;
      border-radius: 20px;
      padding: 32px 28px;
      width: 100%;
      max-width: 400px;
      box-shadow: 0 20px 60px rgba(0,0,0,0.2);
    }

    .title {
      text-align: center;
      margin-bottom: 28px;
    }

    .title .icon {
      font-size: 36px;
      display: block;
      margin-bottom: 8px;
    }

    .title h1 {
      font-size: 20px;
      font-weight: 700;
      color: #1a1a2e;
    }
    .title h2 {
    font-size: 16px;
    font-weight: 700;
    color: #1a1a2e;
    }
    .title p {
      font-size: 13px;
      color: #999;
      margin-top: 4px;
    }

    /* Basis tabs */
    .tabs {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 6px;
      margin-bottom: 20px;
      background: #f4f4f8;
      padding: 5px;
      border-radius: 12px;
    }

    .tab {
      padding: 8px 4px;
      border: none;
      border-radius: 8px;
      background: transparent;
      font-family: 'Inter', sans-serif;
      font-size: 11px;
      font-weight: 600;
      color: #999;
      cursor: pointer;
      transition: all .2s;
      line-height: 1.4;
      text-align: center;
    }

    .tab.active {
      background: white;
      color: #667eea;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    }

    /* Input */
    .input-wrap {
      position: relative;
      margin-bottom: 8px;
    }

    input {
      width: 100%;
      padding: 14px 16px;
      border: 2px solid #eee;
      border-radius: 12px;
      font-size: 18px;
      font-family: 'Inter', sans-serif;
      font-weight: 600;
      color: #1a1a2e;
      outline: none;
      transition: border-color .2s;
    }

    input:focus { border-color: #667eea; }
    input::placeholder { color: #ccc; font-weight: 400; font-size: 15px; }

    .error {
      font-size: 12px;
      color: #e74c3c;
      margin-bottom: 16px;
      min-height: 18px;
      padding-left: 4px;
    }

    /* Results */
    .results { display: flex; flex-direction: column; gap: 10px; }

    .result-item {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 14px 16px;
      border-radius: 12px;
      background: #f8f8fc;
      transition: all .25s;
      cursor: pointer;
    }

    .result-item:hover { background: #f0f0fa; transform: translateX(3px); }

    .result-item.active-basis {
      background: linear-gradient(135deg, #667eea22, #764ba222);
      border: 1.5px solid #667eea55;
    }

    .result-left { display: flex; align-items: center; gap: 10px; }

    .badge {
      width: 36px;
      height: 36px;
      border-radius: 8px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 11px;
      font-weight: 700;
      flex-shrink: 0;
    }

    .badge-dec { background: #fff3e0; color: #f57c00; }
    .badge-bin { background: #e8f5e9; color: #388e3c; }
    .badge-oct { background: #fce4ec; color: #c2185b; }
    .badge-hex { background: #e3f2fd; color: #1976d2; }

    .result-name { font-size: 12px; color: #999; font-weight: 500; }
    .result-val  { font-size: 16px; font-weight: 700; color: #1a1a2e; word-break: break-all; text-align: right; max-width: 55%; }

    .copied-tip {
      font-size: 11px;
      color: #667eea;
      text-align: center;
      margin-top: 10px;
      opacity: 0;
      transition: opacity .3s;
    }
    .copied-tip.show { opacity: 1; }
  </style>
</head>
<body>
<div class="card">

  <div class="title">
    <h1>Konversi Bilangan</h1>
    <h2>By Kelompok 9</h2>
    <p>DEC · BIN · OCT · HEX</p>
  </div>

  <!-- Basis Tabs -->
  <div class="tabs">
    <button class="tab active" data-basis="10" onclick="setTab(this)">DEC<br>Basis 10</button>
    <button class="tab" data-basis="2"  onclick="setTab(this)">BIN<br>Basis 2</button>
    <button class="tab" data-basis="8"  onclick="setTab(this)">OCT<br>Basis 8</button>
    <button class="tab" data-basis="16" onclick="setTab(this)">HEX<br>Basis 16</button>
  </div>

  <!-- Input -->
  <div class="input-wrap">
    <input id="inp" type="text" placeholder="Masukkan nilai..." oninput="konversi()" autofocus/>
  </div>
  <div class="error" id="err"></div>

  <!-- Results -->
  <div class="results">
    <div class="result-item active-basis" id="box-dec" onclick="salin('dec')">
      <div class="result-left">
        <div class="badge badge-dec">10</div>
        <div class="result-name">Desimal</div>
      </div>
      <div class="result-val" id="v-dec">—</div>
    </div>
    <div class="result-item" id="box-bin" onclick="salin('bin')">
      <div class="result-left">
        <div class="badge badge-bin">2</div>
        <div class="result-name">Biner</div>
      </div>
      <div class="result-val" id="v-bin">—</div>
    </div>
    <div class="result-item" id="box-oct" onclick="salin('oct')">
      <div class="result-left">
        <div class="badge badge-oct">8</div>
        <div class="result-name">Oktal</div>
      </div>
      <div class="result-val" id="v-oct">—</div>
    </div>
    <div class="result-item" id="box-hex" onclick="salin('hex')">
      <div class="result-left">
        <div class="badge badge-hex">16</div>
        <div class="result-name">Heksadesimal</div>
      </div>
      <div class="result-val" id="v-hex">—</div>
    </div>
  </div>

  <div class="copied-tip" id="tip">✓ Tersalin ke clipboard!</div>
</div>

<script>
  let basis = 10;
  const valid = { 10:/^[0-9]+$/, 2:/^[01]+$/, 8:/^[0-7]+$/, 16:/^[0-9a-fA-F]+$/ };
  const boxMap = { dec:10, bin:2, oct:8, hex:16 };

  function setTab(el) {
    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
    el.classList.add('active');
    basis = parseInt(el.dataset.basis);
    document.getElementById('inp').value = '';
    document.getElementById('err').textContent = '';
    reset();
    highlightBasis();
    document.getElementById('inp').focus();
  }

  function highlightBasis() {
    Object.entries(boxMap).forEach(([k, b]) => {
      document.getElementById('box-'+k).classList.toggle('active-basis', b === basis);
    });
  }

  function reset() {
    ['dec','bin','oct','hex'].forEach(k => document.getElementById('v-'+k).textContent = '—');
  }

  function konversi() {
    const raw = document.getElementById('inp').value.trim();
    const err = document.getElementById('err');
    err.textContent = '';
    if (!raw) { reset(); return; }
    if (!valid[basis].test(raw)) {
      err.textContent = `⚠ Karakter tidak valid untuk basis ${basis}.`;
      reset(); return;
    }
    const dec = parseInt(raw, basis);
    document.getElementById('v-dec').textContent = dec.toString(10);
    document.getElementById('v-bin').textContent = dec.toString(2);
    document.getElementById('v-oct').textContent = dec.toString(8);
    document.getElementById('v-hex').textContent = dec.toString(16).toUpperCase();
  }

  function salin(key) {
    const val = document.getElementById('v-'+key).textContent;
    if (val === '—') return;
    navigator.clipboard.writeText(val).then(() => {
      const tip = document.getElementById('tip');
      tip.classList.add('show');
      setTimeout(() => tip.classList.remove('show'), 1500);
    });
  }

  highlightBasis();
</script>
</body>
</html>
