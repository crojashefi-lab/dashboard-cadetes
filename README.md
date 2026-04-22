<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Dashboard Cadetes – Coquimbo Unido 2026</title>
<link href="https://fonts.googleapis.com/css2?family=Rajdhani:wght@500;600;700&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
:root {
  --bg:     #0A0A0A;
  --bg2:    #111111;
  --bg3:    #1A1A1A;
  --card:   #141414;
  --border: #2A2A2A;
  --gold:   #F5C400;
  --gold2:  #FFD740;
  --text:   #F0F0F0;
  --muted:  #777777;
  --dim:    #444444;
  --green:  #22C55E;
  --yellow: #F5C400;
  --red:    #EF4444;
  --r:      8px;
}
* { box-sizing: border-box; margin: 0; padding: 0; }
body { background: var(--bg); color: var(--text); font-family: 'Inter', sans-serif; font-size: 14px; min-height: 100vh; }

/* ── HEADER ── */
.header {
  background: #000;
  border-bottom: 2px solid var(--gold);
  padding: 0 28px;
  height: 64px;
  display: flex; align-items: center; justify-content: space-between; gap: 20px;
  position: sticky; top: 0; z-index: 200;
}
.brand { display: flex; align-items: center; gap: 14px; }
.brand-stripe { width: 3px; height: 36px; background: var(--gold); }
.brand-text h1 {
  font-family: 'Rajdhani', sans-serif; font-weight: 700; font-size: 20px;
  letter-spacing: 2px; text-transform: uppercase; line-height: 1;
  color: #fff;
}
.brand-text h1 em { color: var(--gold); font-style: normal; }
.brand-text p { font-size: 10px; color: var(--muted); letter-spacing: 2px; text-transform: uppercase; margin-top: 3px; }
.header-mid { text-align: right; flex: 1; }
.header-mid strong { display: block; font-size: 13px; font-weight: 600; color: var(--text); }
.header-mid span { font-size: 10px; color: var(--muted); }
.header-right { display: flex; align-items: center; gap: 10px; }
.update-time { font-size: 11px; color: var(--muted); white-space: nowrap; }
.btn-ref {
  background: var(--gold); color: #000;
  font-family: 'Rajdhani', sans-serif; font-weight: 700; font-size: 13px;
  letter-spacing: 1px; text-transform: uppercase;
  border: none; border-radius: 5px; padding: 8px 18px; cursor: pointer;
  transition: background .15s;
  white-space: nowrap;
}
.btn-ref:hover { background: var(--gold2); }

/* ── NAV ── */
.nav {
  background: #000; border-bottom: 1px solid var(--border);
  display: flex; padding: 0 28px; overflow-x: auto; gap: 0;
}
.nav-tab {
  font-family: 'Rajdhani', sans-serif; font-weight: 600;
  font-size: 13px; letter-spacing: 2px; text-transform: uppercase;
  color: var(--muted); padding: 14px 22px; cursor: pointer;
  border-bottom: 2px solid transparent; white-space: nowrap;
  transition: color .15s, border-color .15s;
}
.nav-tab:hover { color: var(--text); }
.nav-tab.active { color: var(--gold); border-bottom-color: var(--gold); }

/* ── MAIN ── */
.main { padding: 24px 28px; max-width: 1440px; margin: 0 auto; }
.panel { display: none; animation: fadein .25s ease; }
.panel.active { display: block; }
@keyframes fadein { from { opacity: 0; transform: translateY(6px); } to { opacity: 1; transform: translateY(0); } }

/* ── LOADING ── */
.loading { display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 280px; gap: 14px; }
.spinner { width: 36px; height: 36px; border: 2px solid var(--border); border-top-color: var(--gold); border-radius: 50%; animation: spin .7s linear infinite; }
@keyframes spin { to { transform: rotate(360deg); } }
.loading p { font-size: 12px; color: var(--muted); letter-spacing: 1px; }

/* ── KPIS ── */
.kpis { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 12px; margin-bottom: 24px; }
.kpi {
  background: var(--card); border: 1px solid var(--border);
  border-radius: var(--r); padding: 18px 20px;
  border-top: 2px solid var(--border); position: relative;
}
.kpi.gold  { border-top-color: var(--gold); }
.kpi.green { border-top-color: var(--green); }
.kpi.red   { border-top-color: var(--red); }
.kpi-label { font-size: 10px; font-weight: 600; letter-spacing: 2px; text-transform: uppercase; color: var(--muted); margin-bottom: 10px; }
.kpi-val   { font-family: 'Rajdhani', sans-serif; font-size: 42px; font-weight: 700; line-height: 1; color: var(--text); }
.kpi-sub   { font-size: 11px; color: var(--dim); margin-top: 5px; }

/* ── SECTION TITLE ── */
.stitle {
  font-family: 'Rajdhani', sans-serif; font-weight: 700;
  font-size: 12px; letter-spacing: 3px; text-transform: uppercase;
  color: var(--gold); margin: 24px 0 12px;
  display: flex; align-items: center; gap: 12px;
}
.stitle::after { content: ''; flex: 1; height: 1px; background: var(--border); }

/* ── TABLE ── */
.tw { overflow-x: auto; border-radius: var(--r); border: 1px solid var(--border); margin-bottom: 24px; }
table { width: 100%; border-collapse: collapse; }
thead tr { background: #0D0D0D; }
thead th {
  padding: 10px 16px; text-align: left;
  font-size: 10px; font-weight: 600; letter-spacing: 2px; text-transform: uppercase;
  color: var(--muted); white-space: nowrap;
  border-bottom: 1px solid var(--border);
}
tbody tr { border-bottom: 1px solid var(--border); transition: background .1s; }
tbody tr:last-child { border-bottom: none; }
tbody tr:hover { background: var(--bg3); }
tbody td { padding: 11px 16px; font-size: 13px; white-space: nowrap; color: var(--text); }

/* ── BADGES ── */
.badge {
  display: inline-block; padding: 2px 9px; border-radius: 4px;
  font-size: 11px; font-weight: 600; letter-spacing: 0.5px;
}
.b13     { background: rgba(255,255,255,.07); color: #aaa; }
.b14     { background: rgba(245,196,0,.15);   color: var(--gold); }
.b-green { background: rgba(34,197,94,.12);   color: var(--green); }
.b-yel   { background: rgba(245,196,0,.12);   color: var(--gold); }
.b-red   { background: rgba(239,68,68,.12);   color: var(--red); }

/* ── CHART BOX ── */
.cbox {
  background: var(--card); border: 1px solid var(--border);
  border-radius: var(--r); padding: 20px 20px 16px; margin-bottom: 20px;
}
.cbox canvas { max-height: 240px; }
.cbox-title { font-family: 'Rajdhani', sans-serif; font-weight: 600; font-size: 12px; letter-spacing: 2px; text-transform: uppercase; color: var(--muted); margin-bottom: 16px; }

/* ── GRID ── */
.g2 { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
@media (max-width: 720px) { .g2 { grid-template-columns: 1fr; } .header-mid { display: none; } }

/* ── FILTER BAR ── */
.fbar { display: flex; gap: 12px; flex-wrap: wrap; margin-bottom: 20px; align-items: center; }
.fbar label { font-size: 10px; font-weight: 600; letter-spacing: 2px; text-transform: uppercase; color: var(--muted); }
.fbar select {
  background: var(--bg3); border: 1px solid var(--border);
  color: var(--text); font-family: 'Inter', sans-serif; font-size: 13px;
  padding: 7px 12px; border-radius: 6px; outline: none;
}
.fbar select:focus { border-color: var(--gold); }

/* ── FICHA STAT ── */
.fstats { display: grid; grid-template-columns: repeat(auto-fit, minmax(140px,1fr)); gap: 10px; margin-bottom: 20px; }
.fstat  { background: var(--bg3); border: 1px solid var(--border); border-radius: var(--r); padding: 16px; text-align: center; }
.fstat .v { font-family: 'Rajdhani', sans-serif; font-size: 32px; font-weight: 700; color: var(--gold); }
.fstat .l { font-size: 10px; font-weight: 600; letter-spacing: 1.5px; text-transform: uppercase; color: var(--muted); margin-top: 4px; }

/* ── EMPTY ── */
.empty { text-align: center; padding: 48px 20px; color: var(--muted); font-size: 13px; }

/* ── ERROR BOX ── */
.error-box {
  background: rgba(239,68,68,.08); border: 1px solid rgba(239,68,68,.3);
  border-radius: 8px; padding: 20px 24px; margin-bottom: 20px;
  color: var(--text); font-size: 13px; line-height: 1.7;
}
.error-box strong { color: var(--red); }

/* ── RESPONSIVE MÓVIL ── */
@media (max-width: 600px) {
  .header { padding: 0 12px; height: auto; min-height: 56px; flex-wrap: wrap; gap: 8px; padding-top: 10px; padding-bottom: 10px; }
  .brand-text h1 { font-size: 16px; letter-spacing: 1px; }
  .brand-text p { display: none; }
  .header-mid { display: none; }
  .update-time { font-size: 10px; }
  .btn-ref { font-size: 11px; padding: 6px 12px; }
  .main { padding: 12px; }
  .nav { padding: 0 8px; }
  .nav-tab { font-size: 11px; padding: 12px 10px; letter-spacing: 1px; }
  .kpi-val { font-size: 32px; }
  .fstats { grid-template-columns: repeat(auto-fit, minmax(110px,1fr)); }
}
</style>
</head>
<body>

<div class="header">
  <div class="brand">
    <div class="brand-stripe"></div>
    <div class="brand-text">
      <h1>COQUIMBO <em>UNIDO</em></h1>
      <p>Dashboard Cadetes · Temporada 2026</p>
    </div>
  </div>
  <div class="header-mid">
    <strong>Claudio Rojas Hidalgo</strong>
    <span>Lic. Pedagogía en Educación Física · MSc Medicina en Ciencias del Deporte</span>
  </div>
  <div class="header-right">
    <span class="update-time" id="upd">—</span>
    <button class="btn-ref" onclick="cargarDatos()">↻ Actualizar</button>
  </div>
</div>

<div class="nav">
  <div class="nav-tab active" onclick="switchTab('hoy',this)">Hoy</div>
  <div class="nav-tab" onclick="switchTab('semana',this)">Semana</div>
  <div class="nav-tab" onclick="switchTab('jugador',this)">Por Jugador</div>
  <div class="nav-tab" onclick="switchTab('eval',this)">Evaluaciones</div>
  <div class="nav-tab" onclick="switchTab('fatiga',this)">Fatiga Mensual</div>
  <div class="nav-tab" onclick="switchTab('phv',this)">PHV · Maduración</div>
</div>

<div class="main">

  <!-- HOY -->
  <div class="panel active" id="panel-hoy">
    <div class="loading" id="load-hoy"><div class="spinner"></div><p>Cargando...</p></div>
    <div id="cont-hoy" style="display:none">
      <div class="kpis" id="kpis-hoy"></div>
      <div class="stitle">Registros de hoy</div>
      <div class="tw"><table>
        <thead><tr>
          <th>Nombre</th><th>Cat.</th>
          <th>Ejercicio 1</th><th>S</th><th>R</th><th>kg</th>
          <th>Ejercicio 2</th><th>S</th><th>R</th><th>kg</th>
          <th>Ejercicio 3</th><th>S</th><th>R</th><th>kg</th>
          <th>Vol. kg</th><th>RPE</th>
        </tr></thead>
        <tbody id="tb-hoy"></tbody>
      </table></div>
    </div>
  </div>

  <!-- SEMANA -->
  <div class="panel" id="panel-semana">
    <div class="loading" id="load-sem"><div class="spinner"></div><p>Calculando...</p></div>
    <div id="cont-sem" style="display:none">
      <div class="kpis" id="kpis-sem"></div>
      <div class="g2">
        <div class="cbox"><div class="cbox-title">Volumen semanal por deportista (kg)</div><canvas id="cv-vol"></canvas></div>
        <div class="cbox"><div class="cbox-title">RPE promedio por deportista</div><canvas id="cv-rpe"></canvas></div>
      </div>
      <div class="stitle">Detalle semanal</div>
      <div class="tw"><table>
        <thead><tr><th>Nombre</th><th>Cat.</th><th>Sesiones</th><th>Vol. Total (kg)</th><th>RPE Prom.</th><th>Estado</th></tr></thead>
        <tbody id="tb-sem"></tbody>
      </table></div>
    </div>
  </div>

  <!-- FICHA JUGADOR -->
  <div class="panel" id="panel-jugador">
    <div class="fbar" style="align-items:center;gap:16px;flex-wrap:wrap">
      <div style="display:flex;align-items:center;gap:10px">
        <label>Categoría</label>
        <select id="sel-cat-jug" onchange="filtrarNombres()">
          <option value="">Todas</option>
          <option>Sub 13</option>
          <option>Sub 14</option>
        </select>
      </div>
      <div style="display:flex;align-items:center;gap:10px">
        <label>Jugador</label>
        <select id="sel-jug" onchange="renderJugador()" style="min-width:200px">
          <option value="">Seleccionar...</option>
        </select>
      </div>
    </div>
    <div id="cont-jug"><div class="empty">Selecciona una categoría y luego el jugador para ver su ficha completa</div></div>
  </div>

  <!-- EVALUACIONES -->
  <div class="panel" id="panel-eval">
    <div class="loading" id="load-eval"><div class="spinner"></div><p>Cargando...</p></div>
    <div id="cont-eval" style="display:none">
      <div class="fbar" style="justify-content:space-between;align-items:center">
        <div style="display:flex;gap:12px;align-items:center;flex-wrap:wrap">
          <label>Categoría</label>
          <select id="sel-cat-ev" onchange="renderEval()">
            <option value="">Todas</option><option>Sub 13</option><option>Sub 14</option>
          </select>
        </div>
        <button class="btn-ref" onclick="exportEvalPDF()" style="display:flex;align-items:center;gap:6px">⬇ Descargar PDF</button>
      </div>
      <div class="stitle">Velocidad 10m (seg) — menor es mejor</div>
      <div class="tw"><table>
        <thead><tr><th>Nombre</th><th>Cat.</th><th>E1</th><th>E2</th><th>E3</th><th>Δ E1→E2</th></tr></thead>
        <tbody id="tb-v10"></tbody>
      </table></div>
      <div class="stitle">Velocidad 20m (seg) — menor es mejor</div>
      <div class="tw"><table>
        <thead><tr><th>Nombre</th><th>Cat.</th><th>E1</th><th>E2</th><th>E3</th><th>Δ E1→E2</th></tr></thead>
        <tbody id="tb-v20"></tbody>
      </table></div>
      <div class="stitle">Drop Jump — RSI (fuerza reactiva)</div>
      <div class="tw"><table>
        <thead><tr><th>Nombre</th><th>Cat.</th><th>RSI E1</th><th>RSI E2</th><th>RSI E3</th><th>Δ RSI %</th></tr></thead>
        <tbody id="tb-rsi"></tbody>
      </table></div>
    </div>
  </div>

  <!-- FATIGA MENSUAL -->
  <div class="panel" id="panel-fatiga">
    <div class="loading" id="load-fat"><div class="spinner"></div><p>Cargando...</p></div>
    <div id="cont-fat" style="display:none">
      <div class="fbar" style="justify-content:space-between;align-items:center;margin-bottom:16px">
        <div style="display:flex;gap:12px;align-items:center;flex-wrap:wrap">
          <label>Categoría</label>
          <select id="sel-cat-fat" onchange="renderFatiga()">
            <option value="">Todas</option><option>Sub 13</option><option>Sub 14</option>
          </select>
        </div>
        <button class="btn-ref" onclick="exportFatigaPDF()" style="display:flex;align-items:center;gap:6px">⬇ Descargar PDF</button>
      </div>
      <div class="stitle">Índice de Fatiga Mensual — CMJ</div>
      <div class="tw"><table>
        <thead><tr>
          <th>Nombre</th><th>Cat.</th>
          <th>Abr</th><th>May</th><th>Jun</th><th>Jul</th><th>Ago</th>
          <th>Sep</th><th>Oct</th><th>Nov</th><th>Dic</th>
          <th>Var %</th><th>Estado</th>
        </tr></thead>
        <tbody id="tb-fat"></tbody>
      </table></div>
    </div>
  </div>

  <!-- PHV · MADURACIÓN -->
  <div class="panel" id="panel-phv">
    <div class="fbar" style="justify-content:space-between;align-items:center">
      <div style="display:flex;gap:12px;align-items:center;flex-wrap:wrap">
        <label>Categoría</label>
        <select id="sel-cat-phv" onchange="renderPHV()">
          <option value="">Todas</option><option>Sub 13</option><option>Sub 14</option>
        </select>
        <label>Clasificación</label>
        <select id="sel-cls-phv" onchange="renderPHV()">
          <option value="">Todas</option><option>Pre-PHV</option><option>Circa-PHV</option><option>Post-PHV</option>
        </select>
      </div>
      <button class="btn-ref" onclick="exportPHVpdf()" style="display:flex;align-items:center;gap:6px">
        ⬇ Descargar PDF
      </button>
    </div>

    <!-- KPIs maduración -->
    <div class="kpis" id="kpis-phv"></div>

    <!-- Gráficos -->
    <div class="g2" style="margin-bottom:20px">
      <div class="cbox">
        <div class="cbox-title">Distribución PHV — Offset (años desde pico)</div>
        <canvas id="cv-phv-offset" style="max-height:220px"></canvas>
      </div>
      <div class="cbox">
        <div class="cbox-title">Clasificación por categoría</div>
        <canvas id="cv-phv-cls" style="max-height:220px"></canvas>
      </div>
    </div>

    <!-- Leyenda interpretación -->
    <div style="display:flex;gap:10px;flex-wrap:wrap;margin-bottom:16px">
      <div style="background:rgba(34,197,94,.12);border:1px solid #22C55E;border-radius:6px;padding:8px 14px;font-size:12px">
        <span style="color:#22C55E;font-weight:700">🟢 Pre-PHV</span><br><span style="color:#777">Ventana de desarrollo amplia · Priorizar carga técnica</span>
      </div>
      <div style="background:rgba(245,196,0,.1);border:1px solid #F5C400;border-radius:6px;padding:8px 14px;font-size:12px">
        <span style="color:#F5C400;font-weight:700">🟡 Circa-PHV</span><br><span style="color:#777">Período sensible · Monitoreo intensivo de carga</span>
      </div>
      <div style="background:rgba(239,68,68,.1);border:1px solid #EF4444;border-radius:6px;padding:8px 14px;font-size:12px">
        <span style="color:#EF4444;font-weight:700">🔴 Post-PHV</span><br><span style="color:#777">Pico superado · Carga con criterio adulto progresivo</span>
      </div>
    </div>

    <!-- Tabla PHV -->
    <div class="stitle">Detalle individual — PHV &amp; Maduración</div>
    <div id="phv-pdf-zone">
    <div class="tw" id="tw-phv">
      <table>
        <thead><tr>
          <th>Nombre</th><th>Cat.</th><th>Edad</th><th>Talla (cm)</th><th>Masa (kg)</th>
          <th>Offset PHV</th><th>Edad PHV est.</th><th>Cm por crecer</th><th>Est. proyectada</th>
          <th>Clasificación</th><th>Interpretación</th>
        </tr></thead>
        <tbody id="tb-phv"></tbody>
      </table>
    </div>
    </div>

    <!-- Cruce CMJ × PHV (se llena cuando hay datos de evaluación) -->
    <div class="stitle" id="cruce-title" style="display:none">Cruce PHV × Salto CMJ / Drop Jump</div>
    <div class="tw" id="tw-cruce" style="display:none">
      <table>
        <thead><tr>
          <th>Nombre</th><th>Cat.</th><th>Clasificación PHV</th><th>Offset PHV</th>
          <th>CMJ (E1)</th><th>RSI DJ (E1)</th><th>Contexto madurativo</th>
        </tr></thead>
        <tbody id="tb-cruce"></tbody>
      </table>
    </div>
    <div id="cruce-info" style="display:none;background:var(--bg3);border:1px solid var(--border);border-radius:8px;padding:14px 18px;font-size:12px;color:var(--muted);margin-bottom:20px;line-height:1.8">
      📌 <strong style="color:var(--gold)">Cómo interpretar el cruce PHV × Salto:</strong><br>
      · <strong>Pre-PHV:</strong> valores de CMJ y RSI bajos son esperables — el foco debe estar en la técnica, no en la potencia absoluta.<br>
      · <strong>Circa-PHV:</strong> período de máxima sensibilidad neural. Ganancias rápidas son posibles pero la sobrecarga puede generar lesiones en fisis abiertas.<br>
      · <strong>Post-PHV:</strong> el sistema neuro-muscular está maduro. Se puede comenzar a interpretar CMJ y RSI con baremos adultos progresivos.
    </div>
  </div>

</div>

<script>
// ── CONFIG ────────────────────────────────────────────────────────────────
const SHEET_ID = "1DW-XlsqyfjbUygmGt9bIGb9MPzv-iEt2vTMFb0WiCtg";
let DATA = { reg: [], ev: [], fat: [], phv: [] };
let CH   = {};

// ── DATOS PHV embebidos ──
const PHV_DATA = [{"nombre": "Agustin Gomez", "cat": "Sub 13", "edad": 13.2, "masa": 70.0, "talla": 171.9, "offset": 0.32, "edadPHV": 12.89, "cmCrecer": 15.0, "estProyec": 186.9, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Agustin Luque", "cat": "Sub 13", "edad": 13.3, "masa": 56.5, "talla": 172.0, "offset": 0.32, "edadPHV": 12.99, "cmCrecer": 14.9, "estProyec": 186.9, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Alejandro Toranzo", "cat": "Sub 13", "edad": 12.7, "masa": 60.0, "talla": 158.6, "offset": -0.49, "edadPHV": 13.19, "cmCrecer": 23.0, "estProyec": 181.6, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Alonso Rocco", "cat": "Sub 13", "edad": 12.0, "masa": 42.3, "talla": 149.2, "offset": -1.81, "edadPHV": 13.81, "cmCrecer": 32.0, "estProyec": 181.2, "clasif": "Pre-PHV", "interp": "VERDE - Desarrollo"}, {"nombre": "Benjamin Leyton", "cat": "Sub 13", "edad": 13.1, "masa": 66.3, "talla": 184.1, "offset": 0.58, "edadPHV": 12.53, "cmCrecer": 12.1, "estProyec": 196.2, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Benjamin Vallejos", "cat": "Sub 13", "edad": 13.1, "masa": 60.6, "talla": 166.9, "offset": -0.19, "edadPHV": 13.29, "cmCrecer": 20.1, "estProyec": 187.0, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Bruno Olivares", "cat": "Sub 13", "edad": 12.8, "masa": 50.6, "talla": 162.1, "offset": -0.4, "edadPHV": 13.2, "cmCrecer": 22.0, "estProyec": 184.1, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Cristian Castillo", "cat": "Sub 13", "edad": 13.1, "masa": 49.5, "talla": 150.2, "offset": -1.1, "edadPHV": 14.2, "cmCrecer": 24.3, "estProyec": 174.5, "clasif": "Pre-PHV", "interp": "VERDE - Desarrollo"}, {"nombre": "Dylan Navea", "cat": "Sub 13", "edad": 12.8, "masa": 39.6, "talla": 151.4, "offset": -1.63, "edadPHV": 14.43, "cmCrecer": 27.9, "estProyec": 179.3, "clasif": "Pre-PHV", "interp": "VERDE - Desarrollo"}, {"nombre": "Emiliano Montalban", "cat": "Sub 13", "edad": 13.4, "masa": 47.4, "talla": 161.4, "offset": -0.33, "edadPHV": 13.73, "cmCrecer": 22.0, "estProyec": 183.4, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Esaid Rojas", "cat": "Sub 13", "edad": 12.5, "masa": 53.8, "talla": 160.5, "offset": -0.8, "edadPHV": 13.3, "cmCrecer": 25.5, "estProyec": 186.0, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Felipe Valdivia", "cat": "Sub 13", "edad": 13.2, "masa": 49.8, "talla": 162.0, "offset": -0.61, "edadPHV": 13.81, "cmCrecer": 23.8, "estProyec": 185.8, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Giacomo Zandonai", "cat": "Sub 13", "edad": 12.4, "masa": 48.6, "talla": 162.5, "offset": -0.56, "edadPHV": 12.96, "cmCrecer": 23.0, "estProyec": 185.5, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Jose Galvez", "cat": "Sub 13", "edad": 12.7, "masa": 40.7, "talla": 145.8, "offset": -1.66, "edadPHV": 14.36, "cmCrecer": 27.9, "estProyec": 173.7, "clasif": "Pre-PHV", "interp": "VERDE - Desarrollo"}, {"nombre": "Leon Peralta", "cat": "Sub 13", "edad": 13.2, "masa": 63.9, "talla": 171.3, "offset": 0.58, "edadPHV": 12.63, "cmCrecer": 12.1, "estProyec": 183.4, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Maickol Gavilan", "cat": "Sub 13", "edad": 12.9, "masa": 53.6, "talla": 164.5, "offset": -0.4, "edadPHV": 13.3, "cmCrecer": 22.0, "estProyec": 186.5, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Manuel Plaza", "cat": "Sub 13", "edad": 13.0, "masa": 49.8, "talla": 158.3, "offset": -0.6, "edadPHV": 13.6, "cmCrecer": 23.8, "estProyec": 182.1, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Martín Díaz", "cat": "Sub 13", "edad": 11.9, "masa": 45.4, "talla": 155.7, "offset": -1.37, "edadPHV": 13.27, "cmCrecer": 29.6, "estProyec": 185.3, "clasif": "Pre-PHV", "interp": "VERDE - Desarrollo"}, {"nombre": "Martín Santander", "cat": "Sub 13", "edad": 12.5, "masa": 47.3, "talla": 158.7, "offset": -0.89, "edadPHV": 13.39, "cmCrecer": 26.2, "estProyec": 184.9, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Martin Concha", "cat": "Sub 13", "edad": 13.5, "masa": 58.0, "talla": 160.5, "offset": -0.34, "edadPHV": 13.84, "cmCrecer": 21.1, "estProyec": 181.6, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Milenko Araya", "cat": "Sub 13", "edad": 13.1, "masa": 53.8, "talla": 170.2, "offset": -0.02, "edadPHV": 13.12, "cmCrecer": 19.0, "estProyec": 189.2, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Renato Ugarte", "cat": "Sub 13", "edad": 12.7, "masa": 57.3, "talla": 162.3, "offset": -0.59, "edadPHV": 13.29, "cmCrecer": 23.8, "estProyec": 186.1, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Rodrigo Maluenda", "cat": "Sub 13", "edad": 13.0, "masa": 63.9, "talla": 171.7, "offset": 0.22, "edadPHV": 12.79, "cmCrecer": 15.9, "estProyec": 187.6, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Salvador Sanchez", "cat": "Sub 13", "edad": 13.1, "masa": 58.1, "talla": 165.5, "offset": -0.34, "edadPHV": 13.44, "cmCrecer": 21.1, "estProyec": 186.6, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Sebastián Quezada", "cat": "Sub 13", "edad": 13.2, "masa": 42.8, "talla": 163.2, "offset": -0.71, "edadPHV": 13.91, "cmCrecer": 23.0, "estProyec": 186.2, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Vicente Arancibia", "cat": "Sub 13", "edad": 13.0, "masa": 55.9, "talla": 167.0, "offset": -0.02, "edadPHV": 13.02, "cmCrecer": 18.0, "estProyec": 185.0, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Aaron Díaz", "cat": "Sub 14", "edad": 14.0, "masa": 56.5, "talla": 166.5, "offset": 0.54, "edadPHV": 13.46, "cmCrecer": 13.0, "estProyec": 179.5, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Agustin Torres", "cat": "Sub 14", "edad": 13.6, "masa": 60.1, "talla": 163.1, "offset": -0.06, "edadPHV": 13.66, "cmCrecer": 19.0, "estProyec": 182.1, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Alexis Segovia", "cat": "Sub 14", "edad": 13.6, "masa": 58.6, "talla": 168.7, "offset": 0.36, "edadPHV": 13.24, "cmCrecer": 14.9, "estProyec": 183.6, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Bastian Villarroel", "cat": "Sub 14", "edad": 13.9, "masa": 55.5, "talla": 159.0, "offset": 0.01, "edadPHV": 13.9, "cmCrecer": 16.6, "estProyec": 175.6, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Benjamin Herrera", "cat": "Sub 14", "edad": 14.3, "masa": 55.6, "talla": 170.9, "offset": 0.96, "edadPHV": 13.35, "cmCrecer": 9.7, "estProyec": 180.6, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Benjamin Mendez", "cat": "Sub 14", "edad": 13.9, "masa": 58.0, "talla": 173.5, "offset": 0.78, "edadPHV": 13.13, "cmCrecer": 11.3, "estProyec": 184.8, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Cristian Cerezo", "cat": "Sub 14", "edad": 13.8, "masa": 52.6, "talla": 166.2, "offset": 0.3, "edadPHV": 13.51, "cmCrecer": 14.9, "estProyec": 181.1, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Cristian Rojas", "cat": "Sub 14", "edad": 14.2, "masa": 45.8, "talla": 154.7, "offset": -0.25, "edadPHV": 14.45, "cmCrecer": 18.0, "estProyec": 172.7, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Cristobal Sarmiento", "cat": "Sub 14", "edad": 14.1, "masa": 53.3, "talla": 163.1, "offset": 0.28, "edadPHV": 13.82, "cmCrecer": 15.9, "estProyec": 179.0, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Cristobal Vega", "cat": "Sub 14", "edad": 13.3, "masa": 61.5, "talla": 166.1, "offset": 0.17, "edadPHV": 13.13, "cmCrecer": 15.9, "estProyec": 182.0, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Donkan Collao", "cat": "Sub 14", "edad": 13.9, "masa": 53.6, "talla": 169.0, "offset": 0.46, "edadPHV": 13.44, "cmCrecer": 13.0, "estProyec": 182.0, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Emiliano Santander", "cat": "Sub 14", "edad": 13.4, "masa": 54.3, "talla": 171.0, "offset": 0.36, "edadPHV": 13.05, "cmCrecer": 14.9, "estProyec": 185.9, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Emilio Rodriguez", "cat": "Sub 14", "edad": 14.1, "masa": 62.9, "talla": 165.5, "offset": 0.5, "edadPHV": 13.6, "cmCrecer": 13.0, "estProyec": 178.5, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Gaspar Olivares", "cat": "Sub 14", "edad": 13.5, "masa": 53.8, "talla": 167.0, "offset": 0.36, "edadPHV": 13.15, "cmCrecer": 14.9, "estProyec": 181.9, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Gerardo Cortes", "cat": "Sub 14", "edad": 13.8, "masa": 62.5, "talla": 171.6, "offset": 0.86, "edadPHV": 12.94, "cmCrecer": 10.5, "estProyec": 182.1, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Gregorio Gonzalez", "cat": "Sub 14", "edad": 14.0, "masa": 48.8, "talla": 160.7, "offset": 0.31, "edadPHV": 13.69, "cmCrecer": 14.9, "estProyec": 175.6, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Ignacio Rojas", "cat": "Sub 14", "edad": 14.2, "masa": 54.5, "talla": 157.4, "offset": 0.49, "edadPHV": 13.71, "cmCrecer": 13.0, "estProyec": 170.4, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Jaime Silva", "cat": "Sub 14", "edad": 13.9, "masa": 59.5, "talla": 169.5, "offset": 0.74, "edadPHV": 13.16, "cmCrecer": 11.3, "estProyec": 180.8, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Joe Riquelme", "cat": "Sub 14", "edad": 14.3, "masa": 49.5, "talla": 159.1, "offset": -0.15, "edadPHV": 14.45, "cmCrecer": 16.1, "estProyec": 175.3, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "José Oyarzo", "cat": "Sub 14", "edad": 13.4, "masa": 49.8, "talla": 161.0, "offset": -0.72, "edadPHV": 14.12, "cmCrecer": 21.4, "estProyec": 182.4, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Manuel Espinoza", "cat": "Sub 14", "edad": 14.2, "masa": 56.2, "talla": 170.1, "offset": 0.4, "edadPHV": 13.81, "cmCrecer": 14.0, "estProyec": 184.1, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Martin Guerrero", "cat": "Sub 14", "edad": 13.9, "masa": 57.6, "talla": 172.0, "offset": 0.87, "edadPHV": 13.04, "cmCrecer": 9.7, "estProyec": 181.7, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Matias Providel", "cat": "Sub 14", "edad": 14.0, "masa": 62.2, "talla": 173.8, "offset": 1.08, "edadPHV": 12.92, "cmCrecer": 8.3, "estProyec": 182.1, "clasif": "Post-PHV", "interp": "ROJO - Riesgo"}, {"nombre": "Matias Rojas", "cat": "Sub 14", "edad": 13.9, "masa": 60.8, "talla": 168.5, "offset": 0.53, "edadPHV": 13.37, "cmCrecer": 13.0, "estProyec": 181.5, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Maximiliano Araya", "cat": "Sub 14", "edad": 14.2, "masa": 55.0, "talla": 168.3, "offset": 0.47, "edadPHV": 13.73, "cmCrecer": 14.0, "estProyec": 182.3, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Pablo Aguila", "cat": "Sub 14", "edad": 14.1, "masa": 62.4, "talla": 167.4, "offset": 0.81, "edadPHV": 13.29, "cmCrecer": 10.5, "estProyec": 177.9, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Samuel Larez", "cat": "Sub 14", "edad": 14.4, "masa": 58.2, "talla": 167.7, "offset": 0.85, "edadPHV": 13.56, "cmCrecer": 9.7, "estProyec": 177.4, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}, {"nombre": "Yamir Sepulveda", "cat": "Sub 14", "edad": 14.0, "masa": 50.3, "talla": 162.4, "offset": 0.05, "edadPHV": 13.95, "cmCrecer": 16.6, "estProyec": 179.0, "clasif": "Circa-PHV", "interp": "AMARILLO - Atención"}];

// ── JSONP fetch (sin CORS) ─────────────────────────────────────────────────
function fetchSheet(hoja) {
  return new Promise(resolve => {
    const cb  = '_cb_' + Math.random().toString(36).slice(2);
    const tag = document.createElement('script');
    tag.src   = `https://docs.google.com/spreadsheets/d/${SHEET_ID}/gviz/tq?tqx=out:json&tq=select+*&sheet=${encodeURIComponent(hoja)}&callback=${cb}`;

    window[cb] = function(data) {
      try { delete window[cb]; } catch(e) {}
      try { if (tag.parentNode) tag.parentNode.removeChild(tag); } catch(e) {}
      try {
        const table = data && data.table;
        if (!table) { resolve([]); return; }
        // Limpiar nombres de columna (espacios, saltos de línea)
        const cols = table.cols.map(c => (c.label || '').trim().replace(/\s+/g,' '));
        // parsedNumHeaders indica cuántas filas son encabezado — saltar las extras
        const skip = Math.max(0, (table.parsedNumHeaders || 1) - 1);
        const rows = (table.rows || []).slice(skip);
        const result = rows.map(r =>
          Object.fromEntries(cols.map((c, i) => {
            const cell = r.c && r.c[i];
            return [c, cell ? (cell.v !== null && cell.v !== undefined ? cell.v : (cell.f || '')) : ''];
          }))
        );
        resolve(result);
      } catch(e) {
        console.error('fetchSheet parse error:', hoja, e);
        resolve([]);
      }
    };

    tag.onerror = () => {
      try { delete window[cb]; } catch(e) {}
      resolve([]);
    };
    document.body.appendChild(tag);
    // Timeout 10s
    setTimeout(() => {
      if (window[cb]) {
        delete window[cb];
        try { if (tag.parentNode) tag.parentNode.removeChild(tag); } catch(e) {}
        resolve([]);
      }
    }, 10000);
  });
}

// Fetch especial para CONFIG: lee por índice de columna (B=Sub13, C=Sub14)
// ignorando los títulos de fila 1 y 2
function fetchConfig() {
  return new Promise(resolve => {
    const cb  = '_cb_' + Math.random().toString(36).slice(2);
    const tag = document.createElement('script');
    tag.src   = `https://docs.google.com/spreadsheets/d/${SHEET_ID}/gviz/tq?tqx=out:json&tq=select+*&sheet=CONFIG&callback=${cb}&headers=0`;
    window[cb] = data => {
      delete window[cb];
      tag.parentNode && document.body.removeChild(tag);
      try {
        const nomina = [];
        (data.table.rows||[]).forEach((r, idx) => {
          // Filas 0 y 1 son título y encabezado — saltar
          if (idx < 2) return;
          const n13 = r.c?.[1]?.v?.toString().trim() || '';
          const n14 = r.c?.[2]?.v?.toString().trim() || '';
          if (n13) nomina.push({ nombre: n13, cat: 'Sub 13' });
          if (n14) nomina.push({ nombre: n14, cat: 'Sub 14' });
        });
        resolve(nomina);
      } catch { resolve([]); }
    };
    tag.onerror = () => { delete window[cb]; resolve([]); };
    document.body.appendChild(tag);
    setTimeout(() => { window[cb] && (delete window[cb], resolve([])); }, 8000);
  });
}

// ── FECHA ─────────────────────────────────────────────────────────────────
function parseFecha(v) {
  if (!v) return null;
  // gviz: "Date(2026,3,17)" — mes 0-based
  const m = String(v).match(/Date\((\d+),(\d+),(\d+)\)/);
  if (m) return new Date(+m[1], +m[2], +m[3]);
  if (typeof v === 'string' && v.match(/^\d{4}-\d{2}-\d{2}/)) return new Date(v + 'T00:00:00');
  return new Date(v);
}
function fmtFecha(v) {
  const d = parseFecha(v); if (!d||isNaN(d)) return v||'';
  return d.toLocaleDateString('es-CL',{day:'2-digit',month:'2-digit',year:'numeric'});
}
function dHoy() { const d=new Date(); return new Date(d.getFullYear(),d.getMonth(),d.getDate()); }
function esHoy(v) {
  const d=parseFecha(v); if(!d||isNaN(d)) return false;
  const h=dHoy(); return d.getTime()===h.getTime();
}
function esSemana(v) {
  const d=parseFecha(v); if(!d||isNaN(d)) return false;
  const h=dHoy(), dia=h.getDay()||7;
  const lun=new Date(h); lun.setDate(h.getDate()-dia+1);
  const dom=new Date(lun); dom.setDate(lun.getDate()+6);
  return d>=lun && d<=dom;
}

// ── SEMÁFORO ──────────────────────────────────────────────────────────────
function semCarga(v) {
  if(v>8000) return ['b-red','🔴 Sobrecarga'];
  if(v>4000) return ['b-yel','🟡 Moderada'];
  return ['b-green','🟢 Normal'];
}
function semFatiga(v) {
  if(v<-10) return ['b-red','🔴 Fatiga alta'];
  if(v<-5)  return ['b-yel','🟡 Alerta'];
  return ['b-green','🟢 Normal'];
}
function catBadge(c) { return c==='Sub 13'?`<span class="badge b13">Sub 13</span>`:`<span class="badge b14">Sub 14</span>`; }

// Helper: busca una clave en un objeto ignorando espacios extra
function get(obj, key) {
  if (obj[key] !== undefined && obj[key] !== '') return obj[key];
  // Buscar con espacios al final o inicio
  const found = Object.keys(obj).find(k => k.trim() === key.trim());
  return found !== undefined ? obj[found] : '';
}
function delta(v1,v2,menorMejor=false) {
  const a=parseFloat(v1),b=parseFloat(v2);
  if(isNaN(a)||isNaN(b)||a===0) return '<span style="color:var(--dim)">—</span>';
  const d=((b-a)/a*100).toFixed(1);
  const ok=menorMejor?parseFloat(d)<0:parseFloat(d)>0;
  return `<span style="color:${ok?'var(--green)':'var(--red)'};font-weight:600">${d>0?'+':''}${d}%</span>`;
}

// ── CARGAR ────────────────────────────────────────────────────────────────
async function cargarDatos() {
  document.getElementById('upd').textContent = 'Actualizando...';

  // Cada fetch es independiente — si uno falla, los demás siguen
  const safe = fn => fn.catch(e => { console.warn('fetch error:', e); return []; });

  const [cfg, reg, ev, fat] = await Promise.all([
    safe(fetchConfig()),
    safe(fetchSheet('REGISTRO_ENTRENAMIENTO')),
    safe(fetchSheet('EVALUACIONES')),
    safe(fetchSheet('FATIGA_MENSUAL'))
  ]);

  console.log('CONFIG:', cfg.length, '| REG:', reg.length, '| EV:', ev.length, '| FAT:', fat.length);
  if (reg.length > 0) console.log('Muestra REG[0]:', JSON.stringify(reg[0]).substring(0,200));

  DATA.nomina = cfg;
  DATA.reg = reg.filter(r => get(r,'Fecha') || get(r,'Nombre deportista'));
  DATA.ev  = ev.filter(r  => get(r,'Nombre') || get(r,'Categoría'));
  DATA.fat = fat.filter(r => get(r,'Nombre') || get(r,'Categoría'));

  const hora = new Date().toLocaleTimeString('es-CL',{hour:'2-digit',minute:'2-digit'});
  document.getElementById('upd').textContent = `Actualizado ${hora} · R:${DATA.reg.length} E:${DATA.ev.length} F:${DATA.fat.length}`;

  renderHoy(); renderSemana(); poblarSelectJugador(); renderEval(); renderFatiga();
}

// ── HOY ───────────────────────────────────────────────────────────────────
function renderHoy() {
  // También mostrar fecha de mañana en pruebas: buscar cualquier dato reciente
  const hoy = DATA.reg.filter(r => esHoy(r['Fecha']));
  // Si no hay datos de hoy, buscar los últimos 2 días
  let rows = hoy;
  if (!rows.length) {
    const ayer = new Date(dHoy()); ayer.setDate(ayer.getDate()-1);
    rows = DATA.reg.filter(r => {
      const d = parseFecha(r['Fecha']);
      return d && d >= ayer;
    });
  }

  document.getElementById('load-hoy').style.display = 'none';
  document.getElementById('cont-hoy').style.display = 'block';

  const s13 = rows.filter(r=>get(r,'Categoría')==='Sub 13').length;
  const s14 = rows.filter(r=>get(r,'Categoría')==='Sub 14').length;
  const vol  = rows.reduce((a,r)=>a+(parseFloat(get(r,'Volumen Total (kg)'))||0),0);
  const rpes = rows.map(r=>parseFloat(get(r,'RPE Sesión'))).filter(v=>!isNaN(v));
  const rpe  = rpes.length?(rpes.reduce((a,b)=>a+b)/rpes.length).toFixed(1):'—';

  document.getElementById('kpis-hoy').innerHTML = `
    <div class="kpi gold"><div class="kpi-label">Registros hoy</div>
      <div class="kpi-val">${rows.length}</div>
      <div class="kpi-sub">Sub 13: ${s13} · Sub 14: ${s14}</div></div>
    <div class="kpi green"><div class="kpi-label">Volumen total</div>
      <div class="kpi-val">${vol>0?Math.round(vol).toLocaleString('es-CL'):'—'}</div>
      <div class="kpi-sub">kilogramos acumulados</div></div>
    <div class="kpi"><div class="kpi-label">RPE promedio</div>
      <div class="kpi-val">${rpe}</div>
      <div class="kpi-sub">percepción de esfuerzo</div></div>`;

  document.getElementById('tb-hoy').innerHTML = rows.length
    ? rows.map(r=>`<tr>
        <td><strong>${get(r,'Nombre deportista')||''}</strong></td>
        <td>${catBadge(get(r,'Categoría'))}</td>
        <td>${get(r,'Ejercicio 1')||'—'}</td><td>${get(r,'Series')||''}</td><td>${get(r,'Reps')||''}</td><td>${get(r,'Carga (kg)')||''}</td>
        <td>${get(r,'Ejercicio 2')||'—'}</td><td>${r['Series.1']||r['Series2']||''}</td><td>${r['Reps.1']||r['Reps2']||''}</td><td>${r['Carga (kg).1']||''}</td>
        <td>${get(r,'Ejercicio 3')||'—'}</td><td>${r['Series.2']||''}</td><td>${r['Reps.2']||''}</td><td>${r['Carga (kg).2']||''}</td>
        <td><strong>${get(r,'Volumen Total (kg)')?Math.round(parseFloat(get(r,'Volumen Total (kg)'))).toLocaleString('es-CL')+' kg':'—'}</strong></td>
        <td>${get(r,'RPE Sesión')||'—'}</td>
      </tr>`).join('')
    : `<tr><td colspan="16" class="empty">Sin registros para hoy</td></tr>`;
}

// ── SEMANA ────────────────────────────────────────────────────────────────
function renderSemana() {
  const rows = DATA.reg.filter(r => esSemana(r['Fecha']));
  document.getElementById('load-sem').style.display = 'none';
  document.getElementById('cont-sem').style.display = 'block';

  const por = {};
  rows.forEach(r => {
    const k = get(r,'Nombre deportista')||'?';
    if (!por[k]) por[k] = { cat:get(r,'Categoría'), n:0, vol:0, rpes:[] };
    por[k].n++;
    por[k].vol += parseFloat(get(r,'Volumen Total (kg)'))||0;
    const rp = parseFloat(get(r,'RPE Sesión'));
    if (!isNaN(rp)) por[k].rpes.push(rp);
  });
  const jugs = Object.entries(por).sort((a,b)=>b[1].vol-a[1].vol);
  const volT = jugs.reduce((a,[,v])=>a+v.vol,0);

  document.getElementById('kpis-sem').innerHTML = `
    <div class="kpi gold"><div class="kpi-label">Registros semana</div>
      <div class="kpi-val">${rows.length}</div>
      <div class="kpi-sub">sesiones registradas</div></div>
    <div class="kpi green"><div class="kpi-label">Volumen acumulado</div>
      <div class="kpi-val">${Math.round(volT).toLocaleString('es-CL')}</div>
      <div class="kpi-sub">kg totales esta semana</div></div>
    <div class="kpi"><div class="kpi-label">Deportistas activos</div>
      <div class="kpi-val">${jugs.length}</div>
      <div class="kpi-sub">con al menos 1 sesión</div></div>`;

  const labels  = jugs.map(([k])=>k.split(' ').pop());
  const vols    = jugs.map(([,v])=>Math.round(v.vol));
  const rpeAvgs = jugs.map(([,v])=>v.rpes.length?+(v.rpes.reduce((a,b)=>a+b)/v.rpes.length).toFixed(1):0);

  if(CH.vol) CH.vol.destroy();
  CH.vol = new Chart(document.getElementById('cv-vol'),{
    type:'bar',
    data:{ labels, datasets:[{ data:vols, backgroundColor:'rgba(245,196,0,0.75)', borderColor:'#F5C400', borderWidth:1, borderRadius:4 }] },
    options:{ responsive:true, plugins:{legend:{display:false}},
      scales:{ x:{ticks:{color:'#666',font:{size:10}}}, y:{ticks:{color:'#666'}, grid:{color:'#1e1e1e'}} } }
  });
  if(CH.rpe) CH.rpe.destroy();
  CH.rpe = new Chart(document.getElementById('cv-rpe'),{
    type:'bar',
    data:{ labels, datasets:[{
      data:rpeAvgs,
      backgroundColor:rpeAvgs.map(v=>v>=8?'rgba(239,68,68,.7)':v>=6?'rgba(245,196,0,.7)':'rgba(34,197,94,.7)'),
      borderWidth:1, borderRadius:4
    }] },
    options:{ responsive:true, plugins:{legend:{display:false}},
      scales:{ x:{ticks:{color:'#666',font:{size:10}}}, y:{min:0,max:10,ticks:{color:'#666'}, grid:{color:'#1e1e1e'}} } }
  });

  document.getElementById('tb-sem').innerHTML = jugs.length
    ? jugs.map(([nm,v])=>{
        const rp = v.rpes.length?(v.rpes.reduce((a,b)=>a+b)/v.rpes.length).toFixed(1):'—';
        const [sc,st] = semCarga(v.vol);
        return `<tr>
          <td><strong>${nm}</strong></td><td>${catBadge(v.cat)}</td>
          <td style="text-align:center">${v.n}</td>
          <td><strong>${Math.round(v.vol).toLocaleString('es-CL')} kg</strong></td>
          <td style="text-align:center">${rp}</td>
          <td><span class="badge ${sc}">${st}</span></td>
        </tr>`;
      }).join('')
    : `<tr><td colspan="6" class="empty">Sin registros esta semana</td></tr>`;
}

// ── JUGADOR — FICHA INTEGRAL ──────────────────────────────────────────────
function poblarSelectJugador() {
  filtrarNombres();
}

function filtrarNombres() {
  const cat = document.getElementById('sel-cat-jug')?.value || '';
  const sel = document.getElementById('sel-jug');

  // Fuente primaria: NOMINA_DATA embebida. Si el Sheet agrega nuevos,
  // se fusionan automáticamente sin duplicados.
  const fromSheet = (DATA.nomina||[]).filter(n =>
    !NOMINA_DATA.find(e => e.nombre.toLowerCase() === n.nombre.toLowerCase())
  );
  let lista = [...NOMINA_DATA, ...fromSheet];

  if (cat) lista = lista.filter(n => n.cat === cat);
  lista = lista.slice().sort((a,b) => a.nombre.localeCompare(b.nombre, 'es'));

  sel.innerHTML = '<option value="">Seleccionar jugador...</option>'
    + lista.map(n => `<option value="${n.nombre}">${n.nombre}</option>`).join('');

  document.getElementById('cont-jug').innerHTML =
    '<div class="empty">Selecciona un jugador para ver su ficha completa</div>';
}

function renderJugador() {
  const nm   = document.getElementById('sel-jug').value;
  const cont = document.getElementById('cont-jug');
  if (!nm) { cont.innerHTML = '<div class="empty">Selecciona un jugador para ver su ficha completa</div>'; return; }

  // Datos de cada fuente
  const regs = DATA.reg.filter(r=>get(r,'Nombre deportista')===nm)
    .sort((a,b)=>(parseFecha(get(a,'Fecha'))||0)-(parseFecha(get(b,'Fecha'))||0));
  const ev   = DATA.ev.find(r=>(get(r,'Nombre')||'').trim().toLowerCase()===nm.toLowerCase()) || {};
  const fat  = DATA.fat.find(r=>(get(r,'Nombre')||'').trim().toLowerCase()===nm.toLowerCase()) || {};
  const phv  = PHV_DATA.find(r=>r.nombre.toLowerCase()===nm.toLowerCase()) || null;

  // Categoría
  const nomEntry = (DATA.nomina||[]).find(n=>n.nombre.toLowerCase()===nm.toLowerCase());
  const cat = nomEntry?.cat || ev['Categoría'] || fat['Categoría'] || '';

  // Resumen entrenamiento
  const vol  = regs.reduce((a,r)=>a+(parseFloat(get(r,'Volumen Total (kg)'))||0),0);
  const rpes = regs.map(r=>parseFloat(get(r,'RPE Sesión'))).filter(v=>!isNaN(v));
  const rpe  = rpes.length?(rpes.reduce((a,b)=>a+b)/rpes.length).toFixed(1):'—';

  // Fatiga meses
  const meses = ['Abr','May','Jun','Jul','Ago','Sep','Oct','Nov','Dic'];
  const fatVals = meses.map(m=>fat[m]||'').filter(v=>v!=='');

  // Bloque PHV
  const phvHTML = phv ? `
    <div class="stitle" style="margin-top:24px">PHV · Maduración Biológica</div>
    <div class="fstats">
      <div class="fstat"><div class="v" style="color:${phvColor(phv.offset)}">${phv.offset??'—'}</div><div class="l">Offset PHV</div></div>
      <div class="fstat"><div class="v">${phv.talla??'—'}</div><div class="l">Talla (cm)</div></div>
      <div class="fstat"><div class="v">${phv.masa??'—'}</div><div class="l">Masa (kg)</div></div>
      <div class="fstat"><div class="v">${phv.edad??'—'}</div><div class="l">Edad</div></div>
      <div class="fstat"><div class="v">${phv.cmCrecer??'—'}</div><div class="l">Cm por crecer</div></div>
      <div class="fstat"><div class="v">${phv.estProyec??'—'}</div><div class="l">Est. proyectada</div></div>
    </div>
    <div style="margin-bottom:16px">${phvBadge(phv.clasif)} <span style="color:var(--muted);font-size:12px;margin-left:8px">${phv.interp}</span></div>` : '';

  // Bloque Evaluaciones
  const evHTML = `
    <div class="stitle" style="margin-top:24px">Evaluaciones</div>
    <div class="fstats">
      <div class="fstat"><div class="v">${ev['VEL10 E1']||'—'}</div><div class="l">Vel 10m E1 (s)</div></div>
      <div class="fstat"><div class="v">${ev['VEL10 E2']||'—'}</div><div class="l">Vel 10m E2 (s)</div></div>
      <div class="fstat"><div class="v">${ev['VEL20 E1']||'—'}</div><div class="l">Vel 20m E1 (s)</div></div>
      <div class="fstat"><div class="v">${ev['VEL20 E2']||'—'}</div><div class="l">Vel 20m E2 (s)</div></div>
      <div class="fstat"><div class="v">${ev['RSI E1']||'—'}</div><div class="l">RSI DJ E1</div></div>
      <div class="fstat"><div class="v">${ev['RSI E2']||'—'}</div><div class="l">RSI DJ E2</div></div>
    </div>`;

  // Bloque Fatiga
  const fatHTML = fatVals.length ? `
    <div class="stitle" style="margin-top:24px">Fatiga Mensual CMJ</div>
    <div class="fstats">
      ${meses.map(m=>fat[m]?`<div class="fstat"><div class="v" style="font-size:22px">${fat[m]}</div><div class="l">${m}</div></div>`:'').join('')}
    </div>` : '';

  // Bloque entrenamiento
  const ejC = {};
  regs.forEach(r=>{ for(let i=1;i<=5;i++){ const e=r['Ejercicio '+i]; if(e) ejC[e]=(ejC[e]||0)+1; }});
  const top = Object.entries(ejC).sort((a,b)=>b[1]-a[1]).slice(0,3)
    .map(([e,c])=>`${e} <span style="color:var(--gold)">(${c}x)</span>`).join(' &nbsp;·&nbsp; ')||'—';

  cont.innerHTML = `
    <div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:12px;margin-bottom:20px">
      <div>
        <div style="font-family:'Rajdhani',sans-serif;font-size:28px;font-weight:700;color:#fff">${nm}</div>
        <div style="color:var(--muted);font-size:12px;letter-spacing:1px">${catBadge(cat)}</div>
      </div>
      <button class="btn-ref" onclick="exportFichaJugadorPDF('${nm.replace(/'/g,"\\'")}')">⬇ Descargar PDF</button>
    </div>
    <div class="fstats">
      <div class="fstat"><div class="v">${regs.length}</div><div class="l">Sesiones</div></div>
      <div class="fstat"><div class="v">${vol>0?Math.round(vol).toLocaleString('es-CL'):'—'}</div><div class="l">Vol. total kg</div></div>
      <div class="fstat"><div class="v">${rpe}</div><div class="l">RPE promedio</div></div>
    </div>
    ${phvHTML}
    ${evHTML}
    ${fatHTML}
    <div class="stitle" style="margin-top:24px">Ejercicios más frecuentes</div>
    <div style="background:var(--bg3);border:1px solid var(--border);border-radius:var(--r);padding:14px 18px;font-size:13px;color:var(--muted);margin-bottom:20px;line-height:1.8">${top}</div>
    ${regs.length ? `
    <div class="cbox" id="box-jug-chart">
      <div class="cbox-title">Volumen por sesión (kg)</div>
      <canvas id="cv-jug"></canvas>
    </div>
    <div class="stitle">Historial de entrenamiento</div>
    <div class="tw"><table>
      <thead><tr><th>Fecha</th><th>Ejercicio 1</th><th>Ejercicio 2</th><th>Ejercicio 3</th><th>Vol. kg</th><th>RPE</th></tr></thead>
      <tbody>${regs.slice().reverse().map(r=>`<tr>
        <td>${fmtFecha(get(r,'Fecha'))}</td>
        <td>${get(r,'Ejercicio 1')||'—'}</td><td>${get(r,'Ejercicio 2')||'—'}</td><td>${get(r,'Ejercicio 3')||'—'}</td>
        <td>${get(r,'Volumen Total (kg)')?Math.round(parseFloat(get(r,'Volumen Total (kg)'))).toLocaleString('es-CL')+' kg':'—'}</td>
        <td>${get(r,'RPE Sesión')||'—'}</td>
      </tr>`).join('')}</tbody>
    </table></div>` : '<div class="empty" style="margin-top:0">Sin registros de entrenamiento aún</div>'}`;

  if (regs.length) {
    if(CH.jug) CH.jug.destroy();
    CH.jug = new Chart(document.getElementById('cv-jug'),{
      type:'line',
      data:{ labels:regs.map(r=>fmtFecha(get(r,'Fecha'))),
        datasets:[{ data:regs.map(r=>parseFloat(get(r,'Volumen Total (kg)'))||0),
          borderColor:'#F5C400', backgroundColor:'rgba(245,196,0,0.08)',
          tension:0.4, fill:true, pointBackgroundColor:'#F5C400', pointRadius:4 }] },
      options:{ responsive:true, plugins:{legend:{display:false}},
        scales:{ x:{ticks:{color:'#666',font:{size:10}}}, y:{ticks:{color:'#666'}, grid:{color:'#1e1e1e'}} } }
    });
  }
}

// ── PDF FICHA JUGADOR ─────────────────────────────────────────────────────
function exportFichaJugadorPDF(nm) {
  const cont = document.getElementById('cont-jug');
  const fecha = new Date().toLocaleDateString('es-CL');
  const win = window.open('','_blank','width=900,height=700');
  const strip = h => h.replace(/<canvas[^>]*><\/canvas>/g,'')
    .replace(/<button[^>]*>.*?<\/button>/gs,'')
    .replace(/style="[^"]*color:var\([^)]+\)[^"]*"/g,'')
    .replace(/var\(--gold\)/g,'#c8a000')
    .replace(/var\(--muted\)/g,'#666')
    .replace(/var\(--green\)/g,'#16a34a')
    .replace(/var\(--red\)/g,'#dc2626');
  win.document.write(`<!DOCTYPE html><html><head><meta charset="UTF-8">
  <title>Ficha ${nm}</title>
  <style>
    body{font-family:Arial,sans-serif;font-size:12px;color:#111;margin:30px}
    h1{font-size:20px;margin-bottom:2px} h2{font-size:12px;color:#555;font-weight:normal;margin-bottom:20px}
    .fstats{display:flex;flex-wrap:wrap;gap:10px;margin-bottom:16px}
    .fstat{background:#f5f5f5;border-radius:6px;padding:12px 16px;text-align:center;min-width:90px}
    .v{font-size:24px;font-weight:700;color:#111} .l{font-size:10px;color:#666;text-transform:uppercase;letter-spacing:1px;margin-top:4px}
    .stitle{font-size:11px;font-weight:700;letter-spacing:2px;text-transform:uppercase;color:#111;margin:20px 0 8px;border-bottom:1px solid #ddd;padding-bottom:4px}
    table{width:100%;border-collapse:collapse;font-size:11px;margin-bottom:16px}
    th{background:#111;color:#fff;padding:6px 10px;text-align:left;font-size:10px;text-transform:uppercase}
    td{padding:6px 10px;border-bottom:1px solid #eee}
    .badge{display:inline-block;padding:2px 8px;border-radius:4px;font-size:11px;font-weight:600}
    footer{margin-top:20px;font-size:10px;color:#999;border-top:1px solid #ddd;padding-top:8px}
    @media print{body{margin:12px}}
  </style></head><body>
  <h1>${nm}</h1>
  <h2>Ficha integral · Temporada 2026 · Fecha: ${fecha} · Metodólogo: Claudio Rojas Hidalgo</h2>
  ${strip(cont.innerHTML)}
  <footer>Dashboard Cadetes Coquimbo Unido 2026 · Datos desde Google Sheets</footer>
  <script>window.onload=()=>{window.print();}<\/script>
  </body></html>`);
  win.document.close();
}

// ── EVALUACIONES ──────────────────────────────────────────────────────────
function renderEval() {
  const loadEl = document.getElementById('load-eval');
  const contEl = document.getElementById('cont-eval');
  if (loadEl) loadEl.style.display = 'none';
  if (contEl) contEl.style.display = 'block';
  const cat = document.getElementById('sel-cat-ev')?.value||'';
  const evs = cat ? DATA.ev.filter(r=>get(r,'Categoría')===cat) : DATA.ev;
  const empty = `<tr><td colspan="6" class="empty">Sin datos de evaluación aún — ingresa datos en la hoja EVALUACIONES del Sheet</td></tr>`;
  const row = (r,c1,c2,c3)=>`<tr>
    <td><strong>${get(r,'Nombre')||''}</strong></td><td>${catBadge(get(r,'Categoría'))}</td>
    <td>${r[c1]||'—'}</td><td>${r[c2]||'—'}</td><td>${r[c3]||'—'}</td>
    <td>${delta(r[c1],r[c2],true)}</td></tr>`;
  document.getElementById('tb-v10').innerHTML = evs.length ? evs.map(r=>row(r,'VEL10 E1','VEL10 E2','VEL10 E3')).join('') : empty;
  document.getElementById('tb-v20').innerHTML = evs.length ? evs.map(r=>row(r,'VEL20 E1','VEL20 E2','VEL20 E3')).join('') : empty;
  document.getElementById('tb-rsi').innerHTML = evs.length
    ? evs.map(r=>`<tr>
        <td><strong>${get(r,'Nombre')||''}</strong></td><td>${catBadge(get(r,'Categoría'))}</td>
        <td>${r['RSI E1']||'—'}</td><td>${r['RSI E2']||'—'}</td><td>${r['RSI E3']||'—'}</td>
        <td>${delta(r['RSI E1'],r['RSI E2'])}</td></tr>`).join('') : empty;
}

// ── FATIGA ────────────────────────────────────────────────────────────────
function renderFatiga() {
  const loadEl = document.getElementById('load-fat');
  const contEl = document.getElementById('cont-fat');
  if (loadEl) loadEl.style.display = 'none';
  if (contEl) contEl.style.display = 'block';
  if (!DATA.fat) return;
  const cat = document.getElementById('sel-cat-fat')?.value||'';
  const meses = ['Abr','May','Jun','Jul','Ago','Sep','Oct','Nov','Dic'];
  const rows = cat ? DATA.fat.filter(r=>get(r,'Categoría')===cat) : DATA.fat;
  document.getElementById('tb-fat').innerHTML = rows.length
    ? rows.map(r=>{
        const vs = meses.map(m=>r[m]||'');
        const ns = vs.map(v=>parseFloat(v)).filter(v=>!isNaN(v));
        let varP='—', sem='';
        if (ns.length>=2) {
          const d=((ns[ns.length-1]-ns[ns.length-2])/ns[ns.length-2]*100).toFixed(1);
          const [sc,st]=semFatiga(parseFloat(d));
          varP=`<span style="font-weight:600;color:${parseFloat(d)<-5?'var(--red)':parseFloat(d)<0?'var(--yellow)':'var(--green)'}">${d>0?'+':''}${d}%</span>`;
          sem=`<span class="badge ${sc}">${st}</span>`;
        }
        return `<tr>
          <td><strong>${get(r,'Nombre')||''}</strong></td><td>${catBadge(get(r,'Categoría'))}</td>
          ${vs.map(v=>`<td style="text-align:center">${v||'—'}</td>`).join('')}
          <td style="text-align:center">${varP}</td><td>${sem}</td></tr>`;
      }).join('')
    : `<tr><td colspan="14" class="empty">Sin datos de fatiga aún — ingresa datos en la hoja FATIGA_MENSUAL del Sheet</td></tr>`;
}

// ── EXPORTAR PDF EVALUACIONES ─────────────────────────────────────────────
function exportEvalPDF() {
  const cat = document.getElementById('sel-cat-ev')?.value||'Todas';
  const fecha = new Date().toLocaleDateString('es-CL');
  const stripBadge = html => html.replace(/<span[^>]*>/g,'').replace(/<\/span>/g,'').replace(/<strong>/g,'').replace(/<\/strong>/g,'');
  const win = window.open('','_blank','width=1100,height=750');
  win.document.write(`<!DOCTYPE html><html><head><meta charset="UTF-8">
  <title>Evaluaciones Cadetes – ${cat}</title>
  <style>
    body{font-family:Arial,sans-serif;font-size:12px;color:#111;margin:30px}
    h1{font-size:18px;margin-bottom:4px}
    h2{font-size:13px;font-weight:normal;color:#555;margin-bottom:20px}
    h3{font-size:12px;font-weight:700;margin:22px 0 8px;color:#111;text-transform:uppercase;letter-spacing:1px;border-bottom:2px solid #111;padding-bottom:4px}
    table{width:100%;border-collapse:collapse;font-size:11px;margin-bottom:20px}
    th{background:#111;color:#fff;padding:7px 10px;text-align:left;font-size:10px;letter-spacing:1px;text-transform:uppercase}
    td{padding:7px 10px;border-bottom:1px solid #e0e0e0}
    tr:nth-child(even){background:#f9f9f9}
    footer{margin-top:24px;font-size:10px;color:#999;border-top:1px solid #ddd;padding-top:10px}
    @media print{body{margin:15px}}
  </style></head><body>
  <h1>Informe de Evaluaciones — Cadetes Coquimbo Unido 2026</h1>
  <h2>Categoría: ${cat} &nbsp;·&nbsp; Fecha: ${fecha} &nbsp;·&nbsp; Metodólogo: Claudio Rojas Hidalgo</h2>
  <h3>Velocidad 10m (seg) — menor es mejor</h3>
  <table><thead><tr><th>Nombre</th><th>Cat.</th><th>E1</th><th>E2</th><th>E3</th><th>Δ E1→E2</th></tr></thead>
    <tbody>${stripBadge(document.getElementById('tb-v10').innerHTML)}</tbody></table>
  <h3>Velocidad 20m (seg) — menor es mejor</h3>
  <table><thead><tr><th>Nombre</th><th>Cat.</th><th>E1</th><th>E2</th><th>E3</th><th>Δ E1→E2</th></tr></thead>
    <tbody>${stripBadge(document.getElementById('tb-v20').innerHTML)}</tbody></table>
  <h3>Drop Jump — RSI (fuerza reactiva)</h3>
  <table><thead><tr><th>Nombre</th><th>Cat.</th><th>RSI E1</th><th>RSI E2</th><th>RSI E3</th><th>Δ RSI %</th></tr></thead>
    <tbody>${stripBadge(document.getElementById('tb-rsi').innerHTML)}</tbody></table>
  <footer>Datos desde Google Sheets COQUIMBO_UNIDO_CADETES_2026 · Evaluaciones T1/T2/T3 · Temporada 2026</footer>
  <script>window.onload=()=>{window.print();}<\/script>
  </body></html>`);
  win.document.close();
}

// ── EXPORTAR PDF FATIGA MENSUAL ───────────────────────────────────────────
function exportFatigaPDF() {
  const cat = document.getElementById('sel-cat-fat')?.value||'Todas';
  const fecha = new Date().toLocaleDateString('es-CL');
  const stripBadge = html => html.replace(/<span[^>]*class="badge[^"]*"[^>]*>/g,'').replace(/<\/span>/g,'').replace(/<strong>/g,'').replace(/<\/strong>/g,'');
  const win = window.open('','_blank','width=1200,height=750');
  win.document.write(`<!DOCTYPE html><html><head><meta charset="UTF-8">
  <title>Fatiga Mensual Cadetes – ${cat}</title>
  <style>
    body{font-family:Arial,sans-serif;font-size:12px;color:#111;margin:30px}
    h1{font-size:18px;margin-bottom:4px}
    h2{font-size:13px;font-weight:normal;color:#555;margin-bottom:20px}
    table{width:100%;border-collapse:collapse;font-size:11px}
    th{background:#111;color:#fff;padding:7px 10px;text-align:left;font-size:10px;letter-spacing:1px;text-transform:uppercase}
    td{padding:7px 10px;border-bottom:1px solid #e0e0e0;text-align:center}
    td:first-child{text-align:left;font-weight:600}
    td:nth-child(2){text-align:left}
    tr:nth-child(even){background:#f9f9f9}
    .note{margin-top:16px;font-size:11px;color:#555;line-height:1.7}
    footer{margin-top:24px;font-size:10px;color:#999;border-top:1px solid #ddd;padding-top:10px}
    @media print{body{margin:15px}}
  </style></head><body>
  <h1>Índice de Fatiga Mensual (CMJ) — Cadetes Coquimbo Unido 2026</h1>
  <h2>Categoría: ${cat} &nbsp;·&nbsp; Fecha: ${fecha} &nbsp;·&nbsp; Metodólogo: Claudio Rojas Hidalgo</h2>
  <table>
    <thead><tr><th>Nombre</th><th>Cat.</th><th>Abr</th><th>May</th><th>Jun</th><th>Jul</th><th>Ago</th><th>Sep</th><th>Oct</th><th>Nov</th><th>Dic</th><th>Var %</th><th>Estado</th></tr></thead>
    <tbody>${stripBadge(document.getElementById('tb-fat').innerHTML)}</tbody>
  </table>
  <div class="note">
    <strong>Criterio semáforo:</strong> &nbsp;🟢 Normal (variación &gt; -5%) &nbsp;·&nbsp; 🟡 Alerta (-5% a -10%) &nbsp;·&nbsp; 🔴 Fatiga alta (&lt; -10%)<br>
    <strong>Metodología:</strong> Test CMJ inicio/fin de mes — variación porcentual entre últimas dos mediciones registradas.
  </div>
  <footer>Datos desde Google Sheets COQUIMBO_UNIDO_CADETES_2026 · Hoja FATIGA_MENSUAL · Temporada 2026</footer>
  <script>window.onload=()=>{window.print();}<\/script>
  </body></html>`);
  win.document.close();
}

// ── PHV ───────────────────────────────────────────────────────────────────
function phvBadge(c) {
  if(!c) return '<span class="badge b13">—</span>';
  if(c==='Pre-PHV')   return '<span class="badge b-green">🟢 Pre-PHV</span>';
  if(c==='Circa-PHV') return '<span class="badge b-yel">🟡 Circa-PHV</span>';
  if(c==='Post-PHV')  return '<span class="badge b-red">🔴 Post-PHV</span>';
  return `<span class="badge b13">${c}</span>`;
}
function phvColor(offset) {
  if(offset===null) return '#555';
  if(offset < -1)   return '#22C55E';
  if(offset < 1)    return '#F5C400';
  return '#EF4444';
}

function renderPHV() {
  const cat = document.getElementById('sel-cat-phv')?.value||'';
  const cls = document.getElementById('sel-cls-phv')?.value||'';
  let rows = PHV_DATA.slice();
  if(cat) rows = rows.filter(r=>r.cat===cat);
  if(cls) rows = rows.filter(r=>r.clasif===cls);

  const pre   = PHV_DATA.filter(r=>r.clasif==='Pre-PHV').length;
  const circa = PHV_DATA.filter(r=>r.clasif==='Circa-PHV').length;
  const post  = PHV_DATA.filter(r=>r.clasif==='Post-PHV').length;
  const avgOff= (PHV_DATA.reduce((a,r)=>a+(r.offset||0),0)/PHV_DATA.length).toFixed(2);

  document.getElementById('kpis-phv').innerHTML = `
    <div class="kpi gold"><div class="kpi-label">Total evaluados</div>
      <div class="kpi-val">${PHV_DATA.length}</div>
      <div class="kpi-sub">Sub 13: ${PHV_DATA.filter(r=>r.cat==='Sub 13').length} · Sub 14: ${PHV_DATA.filter(r=>r.cat==='Sub 14').length}</div></div>
    <div class="kpi green"><div class="kpi-label">Pre-PHV</div>
      <div class="kpi-val">${pre}</div>
      <div class="kpi-sub">Ventana de desarrollo</div></div>
    <div class="kpi"><div class="kpi-label">Circa-PHV</div>
      <div class="kpi-val">${circa}</div>
      <div class="kpi-sub">Período sensible</div></div>
    <div class="kpi red"><div class="kpi-label">Post-PHV</div>
      <div class="kpi-val">${post}</div>
      <div class="kpi-sub">Pico superado</div></div>
    <div class="kpi"><div class="kpi-label">Offset promedio</div>
      <div class="kpi-val" style="font-size:32px">${avgOff}</div>
      <div class="kpi-sub">años desde PHV (grupo)</div></div>`;

  // Gráfico offset horizontal
  const sorted = [...rows].sort((a,b)=>(a.offset||0)-(b.offset||0));
  if(CH.phvOff) CH.phvOff.destroy();
  CH.phvOff = new Chart(document.getElementById('cv-phv-offset'),{
    type:'bar',
    data:{
      labels: sorted.map(r=>r.nombre.split(' ').pop()),
      datasets:[{
        data: sorted.map(r=>r.offset),
        backgroundColor: sorted.map(r=>phvColor(r.offset)+'BB'),
        borderColor: sorted.map(r=>phvColor(r.offset)),
        borderWidth:1, borderRadius:3
      }]
    },
    options:{
      indexAxis:'y', responsive:true,
      plugins:{legend:{display:false}, tooltip:{callbacks:{label:ctx=>`Offset: ${ctx.parsed.x} años`}}},
      scales:{
        x:{ticks:{color:'#666'}, grid:{color:'#1e1e1e'},
           title:{display:true,text:'Años desde PHV',color:'#666',font:{size:10}}},
        y:{ticks:{color:'#888',font:{size:10}}, grid:{display:false}}
      }
    }
  });

  // Gráfico dona clasificación
  const s13pre=PHV_DATA.filter(r=>r.cat==='Sub 13'&&r.clasif==='Pre-PHV').length;
  const s13cir=PHV_DATA.filter(r=>r.cat==='Sub 13'&&r.clasif==='Circa-PHV').length;
  const s13pos=PHV_DATA.filter(r=>r.cat==='Sub 13'&&r.clasif==='Post-PHV').length;
  const s14pre=PHV_DATA.filter(r=>r.cat==='Sub 14'&&r.clasif==='Pre-PHV').length;
  const s14cir=PHV_DATA.filter(r=>r.cat==='Sub 14'&&r.clasif==='Circa-PHV').length;
  const s14pos=PHV_DATA.filter(r=>r.cat==='Sub 14'&&r.clasif==='Post-PHV').length;
  if(CH.phvCls) CH.phvCls.destroy();
  CH.phvCls = new Chart(document.getElementById('cv-phv-cls'),{
    type:'bar',
    data:{
      labels:['Pre-PHV','Circa-PHV','Post-PHV'],
      datasets:[
        {label:'Sub 13', data:[s13pre,s13cir,s13pos], backgroundColor:'rgba(245,196,0,0.7)', borderRadius:3},
        {label:'Sub 14', data:[s14pre,s14cir,s14pos], backgroundColor:'rgba(34,197,94,0.5)', borderRadius:3}
      ]
    },
    options:{responsive:true,
      plugins:{legend:{labels:{color:'#888',font:{size:11}}}},
      scales:{
        x:{ticks:{color:'#888'}, grid:{display:false}},
        y:{ticks:{color:'#666',stepSize:1}, grid:{color:'#1e1e1e'}}
      }
    }
  });

  // Tabla detalle
  document.getElementById('tb-phv').innerHTML = rows.length
    ? rows.map(r=>`<tr>
        <td><strong>${r.nombre}</strong></td>
        <td>${catBadge(r.cat)}</td>
        <td style="text-align:center">${r.edad??'—'}</td>
        <td style="text-align:center">${r.talla??'—'}</td>
        <td style="text-align:center">${r.masa??'—'}</td>
        <td style="text-align:center;font-weight:600;color:${phvColor(r.offset)}">${r.offset??'—'}</td>
        <td style="text-align:center">${r.edadPHV??'—'}</td>
        <td style="text-align:center">${r.cmCrecer??'—'}</td>
        <td style="text-align:center">${r.estProyec??'—'}</td>
        <td>${phvBadge(r.clasif)}</td>
        <td style="font-size:12px;color:var(--muted)">${r.interp}</td>
      </tr>`).join('')
    : `<tr><td colspan="11" class="empty">Sin datos para el filtro seleccionado</td></tr>`;

  // Cruce con datos de evaluación
  renderCrucePhv();
}

function renderCrucePhv() {
  if(!DATA.ev.length) return;
  const rows = PHV_DATA.map(p=>{
    const ev = DATA.ev.find(e=>(e['Nombre']||'').trim().toLowerCase()===p.nombre.toLowerCase());
    const cmj = ev ? (ev['CMJ E1']||ev['CMJ_E1']||'') : '';
    const rsi = ev ? (ev['RSI E1']||ev['RSI_E1']||'') : '';
    if(!cmj && !rsi) return null;
    let ctx = '';
    if(p.clasif==='Pre-PHV')   ctx='CMJ bajo esperado — foco técnico, no potencia';
    if(p.clasif==='Circa-PHV') ctx='Período sensible — no sobrecargar en pliométrico';
    if(p.clasif==='Post-PHV')  ctx='Sistema maduro — puede interpretar con baremos adultos';
    return {p, cmj, rsi, ctx};
  }).filter(Boolean);
  if(!rows.length) return;
  document.getElementById('cruce-title').style.display='';
  document.getElementById('tw-cruce').style.display='';
  document.getElementById('cruce-info').style.display='';
  document.getElementById('tb-cruce').innerHTML = rows.map(({p,cmj,rsi,ctx})=>`<tr>
    <td><strong>${p.nombre}</strong></td><td>${catBadge(p.cat)}</td>
    <td>${phvBadge(p.clasif)}</td>
    <td style="text-align:center;color:${phvColor(p.offset)};font-weight:600">${p.offset}</td>
    <td style="text-align:center">${cmj||'—'}</td>
    <td style="text-align:center">${rsi||'—'}</td>
    <td style="font-size:12px;color:var(--muted)">${ctx}</td>
  </tr>`).join('');
}

// ── EXPORTAR PDF ──────────────────────────────────────────────────────────
function exportPHVpdf() {
  const cat = document.getElementById('sel-cat-phv')?.value||'Todas';
  const fecha = new Date().toLocaleDateString('es-CL');
  const zone = document.getElementById('phv-pdf-zone');
  const win = window.open('','_blank','width=1000,height=700');
  win.document.write(`<!DOCTYPE html><html><head><meta charset="UTF-8">
  <title>Informe PHV Cadetes – ${cat}</title>
  <style>
    body{font-family:Arial,sans-serif;font-size:12px;color:#111;margin:30px}
    h1{font-size:18px;margin-bottom:4px}
    h2{font-size:13px;font-weight:normal;color:#555;margin-bottom:20px}
    table{width:100%;border-collapse:collapse;font-size:11px}
    th{background:#111;color:#fff;padding:7px 10px;text-align:left;font-size:10px;letter-spacing:1px;text-transform:uppercase}
    td{padding:7px 10px;border-bottom:1px solid #e0e0e0}
    tr:nth-child(even){background:#f9f9f9}
    .pre{color:#16a34a;font-weight:700} .circa{color:#ca8a04;font-weight:700} .post{color:#dc2626;font-weight:700}
    footer{margin-top:24px;font-size:10px;color:#999;border-top:1px solid #ddd;padding-top:10px}
    @media print{body{margin:15px}}
  </style></head><body>
  <h1>Informe PHV / Maduración Biológica — Cadetes Coquimbo Unido 2026</h1>
  <h2>Categoría: ${cat} &nbsp;·&nbsp; Fecha: ${fecha} &nbsp;·&nbsp; Metodólogo: Claudio Rojas Hidalgo</h2>
  <table>
    <thead><tr><th>Nombre</th><th>Cat.</th><th>Edad</th><th>Talla</th><th>Masa</th><th>Offset PHV</th><th>Cm por crecer</th><th>Est. proyectada</th><th>Clasificación</th><th>Interpretación</th></tr></thead>
    <tbody>
    ${(document.getElementById('tb-phv').innerHTML||'').replace(/<span[^>]*>/g,'').replace(/<\/span>/g,'').replace(/<strong>/g,'').replace(/<\/strong>/g,'')}
    </tbody>
  </table>
  <footer>Modelo Mirwald (2002) / Moore — Datos evaluación nutrición Coquimbo Unido · Programa Metodología Deportiva 2026</footer>
  <script>window.onload=()=>{window.print();}<\/script>
  </body></html>`);
  win.document.close();
}

// ── TABS ──────────────────────────────────────────────────────────────────
function switchTab(id, el) {
  document.querySelectorAll('.panel').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('.nav-tab').forEach(t=>t.classList.remove('active'));
  document.getElementById('panel-'+id).classList.add('active');
  el.classList.add('active');
  if(id==='phv') renderPHV();
  if(id==='fatiga') renderFatiga();
}

// ── INIT ──────────────────────────────────────────────────────────────────
Chart.defaults.color = '#555';
Chart.defaults.font.family = "'Inter', sans-serif";
cargarDatos();
</script>
</body>
</html>
