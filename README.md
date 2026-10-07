<!DOCTYPE html>
<html lang="es">
<head>
<base target="_top">
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Torneo Las Flores</title>
<link href="https://fonts.googleapis.com/css2?family=Barlow+Semi+Condensed:wght@400;500;600;700&display=swap" rel="stylesheet">
<style>
:root{--bg:#0b1020;--card:#141c33;--line:#26325a;--txt:#e8ecf8;--mut:#8b97bd;--acc:#ffc857;--ok:#3dd6a0;--bad:#ff6b7a}
*{box-sizing:border-box}
body{margin:0;background:radial-gradient(1200px 500px at 50% -10%,#1b2650,var(--bg));color:var(--txt);font:500 16px/1.4 'Barlow Semi Condensed',system-ui,sans-serif;min-height:100vh}
h1,h2,h3{margin:0;font-weight:700}
.wrap{max-width:980px;margin:0 auto;padding:18px}
.card{background:var(--card);border:1px solid var(--line);border-radius:14px;padding:16px;margin-bottom:14px}
input,button{font:inherit;color:var(--txt)}
input{background:#0d1428;border:1px solid var(--line);border-radius:8px;padding:9px 11px;width:100%}
input:focus,button:focus-visible{outline:2px solid var(--acc);outline-offset:1px}
button{background:var(--acc);color:#1a1300;border:0;border-radius:8px;padding:9px 16px;font-weight:700;cursor:pointer}
button.sec{background:transparent;color:var(--txt);border:1px solid var(--line)}
button.del{background:transparent;color:var(--bad);border:1px solid #5a2a38;padding:5px 10px}
button:disabled{opacity:.5;cursor:wait}
.hide{display:none!important}
.top{display:flex;justify-content:space-between;align-items:center;gap:10px;margin-bottom:14px}
.logo{font-size:26px;letter-spacing:.3px}.logo b{color:var(--acc)}
.tabs{display:flex;gap:8px;margin-bottom:14px;flex-wrap:wrap}
.tabs button{background:var(--card);color:var(--mut);border:1px solid var(--line)}
.tabs button.on{background:var(--acc);color:#1a1300;border-color:var(--acc)}
.row{display:flex;gap:10px;flex-wrap:wrap;align-items:end}.row>div{flex:1;min-width:130px}
label{display:block;color:var(--mut);font-size:14px;margin-bottom:4px}
.jug{display:grid;grid-template-columns:52px 1fr 80px 34px;gap:8px;align-items:center;margin-top:8px}
.ph{width:52px;height:52px;border-radius:50%;border:1px dashed var(--mut);background:#0d1428 center/cover;cursor:pointer;display:grid;place-items:center;color:var(--mut);font-size:22px}
.team{display:flex;justify-content:space-between;gap:10px;align-items:start}
.avs{display:flex;flex-wrap:wrap;gap:8px;margin-top:10px}
.av{width:62px;text-align:center;font-size:12px;color:var(--mut)}
.av i{display:block;width:46px;height:46px;border-radius:50%;margin:0 auto 3px;background:#0d1428 center/cover;border:1px solid var(--line)}
.av b{color:var(--acc)}
.match{display:grid;grid-template-columns:62px 1fr 46px 14px 46px 1fr auto;gap:8px;align-items:center;padding:9px 0;border-top:1px solid var(--line)}
.match input{text-align:center;padding:6px 2px}
.match .l{text-align:right}.hr{color:var(--acc);font-weight:700}
.ronda{color:var(--mut);margin:14px 0 2px;font-size:15px}
table{width:100%;border-collapse:collapse}
th,td{padding:9px 6px;text-align:center;border-bottom:1px solid var(--line)}
th{color:var(--mut);font-weight:600}td:nth-child(2),th:nth-child(2){text-align:left}
tr:first-child td{color:var(--acc)}td.pt{font-weight:700;color:var(--acc)}
.scroll{overflow-x:auto}
.toast{position:fixed;bottom:18px;left:50%;transform:translateX(-50%);background:var(--ok);color:#052;padding:10px 18px;border-radius:10px;font-weight:700}
.toast.e{background:var(--bad);color:#2a0008}
.empty{color:var(--mut);text-align:center;padding:18px}
@media(max-width:600px){.match{grid-template-columns:50px 1fr 40px 10px 40px 1fr;}.match button{grid-column:1/-1}}
@media(prefers-reduced-motion:no-preference){.tabs button{transition:background .15s}}
</style>
</head>
<body>

<div id="vApp" class="wrap">
  <div class="top"><h1 class="logo">Torneo <b>Las Flores</b></h1></div>
  <div class="tabs">
    <button data-t="eq" class="on">Equipos</button>
    <button data-t="ll">Llaves y horarios</button>
    <button data-t="po">Posiciones</button>
  </div>

  <section id="tEq">
    <div class="card">
      <h3>Nuevo equipo</h3>
      <div style="margin:10px 0"><label>Nombre del equipo</label><input id="eqNom" maxlength="40"></div>
      <div id="jugs"></div>
      <div class="row" style="margin-top:12px">
        <button class="sec" id="bAddJ" style="flex:0">+ Jugador</button>
        <span id="cnt" style="color:var(--mut);flex:1"></span>
        <button id="bSaveEq" style="flex:0;white-space:nowrap">Guardar equipo</button>
      </div>
    </div>
    <div id="listEq"></div>
  </section>

  <section id="tLl" class="hide">
    <div class="card">
      <h3>Crear llaves de enfrentamiento</h3>
      <div class="row" style="margin-top:10px">
        <div><label>Partidos por equipo</label><input id="k" type="number" min="1" value="3"></div>
        <div><label>Fecha</label><input id="fecha" type="date"></div>
        <div><label>Hora de inicio</label><input id="hora" type="time" value="08:00"></div>
        <div><label>Descanso entre partidos (min)</label><input id="desc" type="number" min="0" value="5"></div>
        <button id="bGen" style="flex:0;white-space:nowrap">Generar llaves</button>
      </div>
      <p style="color:var(--mut);margin:10px 0 0">Cada partido: 10 min el primer tiempo y 15 min el segundo. Al generar se reemplazan las llaves anteriores.</p>
    </div>
    <div class="card" id="listPart"></div>
  </section>

  <section id="tPo" class="hide">
    <div class="card">
      <div class="row"><h3 style="flex:1">Tabla de posiciones</h3><button id="bPos" style="flex:0;white-space:nowrap">Generar tabla de posiciones</button></div>
      <div class="scroll" id="tabla" style="margin-top:10px"></div>
    </div>
  </section>
</div>
<div id="toast" class="toast hide"></div>

<script>
// ====== CONEXIÓN CON APPS SCRIPT ======
// Pega entre las comillas la URL de tu implementación de Apps Script (la que termina en /exec).
const API_URL = 'PEGA_AQUI_TU_URL';

let DATA = {equipos: [], partidos: []}, FILAS = [];
const $ = id => document.getElementById(id);
const esc = s => String(s == null ? '' : s).replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));

function run(fn, ...args) {
  // Dentro de Apps Script funciona igual que antes
  if (window.google && google.script && google.script.run) {
    return new Promise((ok, fail) => google.script.run
      .withSuccessHandler(ok)
      .withFailureHandler(e => fail(new Error((e && e.message || String(e)).replace(/^Error:\s*/, ''))))
      [fn](...args));
  }
  // Fuera de Apps Script (GitHub Pages, Vercel) llama a la URL /exec
  return llamar(fn, args);
}
async function llamar(fn, args) {
  const url = API_URL.trim();
  if (!/^https:\/\/script\.google\.com\/.+\/exec$/.test(url)) throw Object.assign(new Error('Falta la URL de Apps Script.'), {ayuda: 'Pégala en index.html, donde dice API_URL: es la que termina en /exec.'});
  let r;
  try {
    r = await (await fetch(url, {method: 'POST', headers: {'Content-Type': 'text/plain;charset=utf-8'}, body: JSON.stringify({fn, args})})).json();
  } catch (e) {
    throw Object.assign(new Error('No se pudo conectar con Apps Script.'), {ayuda: 'Revisa la URL y vuelve a implementar con una versión nueva y acceso "Cualquier usuario".'});
  }
  if (!r || !r.ok) throw new Error(String(r && r.error || 'Error del servidor.').replace(/^Error:\s*/, ''));
  return r.data;
}
function toast(msg, err) {
  const t = $('toast'); t.textContent = msg; t.className = 'toast' + (err ? ' e' : '');
  clearTimeout(toast.h); toast.h = setTimeout(() => t.classList.add('hide'), 2800);
}
async function accion(btn, fn) {
  btn.disabled = true;
  try { await fn(); } catch (e) { toast(e.message, true); }
  btn.disabled = false;
}

document.querySelectorAll('.tabs button').forEach(b => b.onclick = () => {
  document.querySelectorAll('.tabs button').forEach(x => x.classList.toggle('on', x === b));
  ['eq', 'll', 'po'].forEach(k => $('t' + k[0].toUpperCase() + k[1]).classList.toggle('hide', k !== b.dataset.t));
});

async function cargar() { DATA = await run('obtenerDatos'); pintarEquipos(); pintarPartidos(); }

// ----- Equipos -----
function nuevoForm() { $('eqNom').value = ''; FILAS = []; addJ(); }
function addJ() {
  if (FILAS.length >= 15) return toast('Máximo 15 jugadores por equipo.', true);
  FILAS.push({nombre: '', dorsal: '', foto: ''}); pintarJugs();
}
function pintarJugs() {
  $('jugs').innerHTML = FILAS.map((j, i) => `<div class="jug">
    <label class="ph" style="${j.foto ? `background-image:url(${j.foto});border-style:solid` : ''}" title="Foto">${j.foto ? '' : '+'}<input type="file" accept="image/*" hidden data-i="${i}"></label>
    <input placeholder="Nombre del jugador" value="${esc(j.nombre)}" data-n="${i}" maxlength="40">
    <input placeholder="N°" type="number" min="0" value="${esc(j.dorsal)}" data-d="${i}">
    <button class="del" data-x="${i}" title="Quitar">×</button></div>`).join('');
  $('cnt').textContent = FILAS.length + ' / 15 jugadores';
  $('bAddJ').disabled = FILAS.length >= 15;
}
$('jugs').addEventListener('input', e => {
  const t = e.target;
  if (t.dataset.n != null) FILAS[t.dataset.n].nombre = t.value;
  if (t.dataset.d != null) FILAS[t.dataset.d].dorsal = t.value;
});
$('jugs').addEventListener('change', async e => {
  const t = e.target;
  if (t.type === 'file' && t.files[0]) { FILAS[t.dataset.i].foto = await reducir(t.files[0]); pintarJugs(); }
});
$('jugs').addEventListener('click', e => { if (e.target.dataset.x != null) { FILAS.splice(e.target.dataset.x, 1); pintarJugs(); } });
$('bAddJ').onclick = addJ;

function reducir(file) { // recorta a cuadrado de 120 px para que quepa en una celda de Sheets
  return new Promise(ok => {
    const img = new Image();
    img.onload = () => {
      const s = Math.min(img.width, img.height), c = document.createElement('canvas');
      c.width = c.height = 120;
      c.getContext('2d').drawImage(img, (img.width - s) / 2, (img.height - s) / 2, s, s, 0, 0, 120, 120);
      ok(c.toDataURL('image/jpeg', 0.72));
    };
    img.src = URL.createObjectURL(file);
  });
}

$('bSaveEq').onclick = () => accion($('bSaveEq'), async () => {
  const js = FILAS.filter(j => j.nombre.trim());
  DATA = await run('guardarEquipo', $('eqNom').value, js);
  nuevoForm(); pintarEquipos(); pintarPartidos(); toast('Equipo guardado');
});

function pintarEquipos() {
  $('listEq').innerHTML = DATA.equipos.length ? DATA.equipos.map(e => `<div class="card">
    <div class="team"><div><h3>${esc(e.Nombre)}</h3><span style="color:var(--mut)">${e.jugadores.length} jugadores</span></div>
    <button class="del" data-del="${e.ID}">Eliminar</button></div>
    <div class="avs">${e.jugadores.map(j => `<div class="av"><i style="${j.Foto ? `background-image:url(${j.Foto})` : ''}"></i><b>${esc(j.Dorsal)}</b> ${esc(j.Nombre)}</div>`).join('')}</div></div>`).join('')
    : '<div class="empty">Aún no hay equipos. Registra el primero arriba.</div>';
}
$('listEq').onclick = e => {
  const id = e.target.dataset.del;
  if (id && confirm('¿Eliminar este equipo y sus partidos?')) accion(e.target, async () => { DATA = await run('eliminarEquipo', id); pintarEquipos(); pintarPartidos(); });
};

// ----- Llaves -----
$('bGen').onclick = () => accion($('bGen'), async () => {
  if (DATA.partidos.length && !confirm('Se reemplazarán las llaves y resultados actuales. ¿Continuar?')) return;
  DATA = await run('generarLlaves', {k: $('k').value, fecha: $('fecha').value, hora: $('hora').value, descanso: $('desc').value});
  pintarPartidos(); toast('Llaves generadas');
});
function pintarPartidos() {
  const nom = id => (DATA.equipos.find(e => e.ID === id) || {Nombre: '?'}).Nombre;
  if (!DATA.partidos.length) { $('listPart').innerHTML = '<div class="empty">Sin llaves todavía. Define los partidos por equipo y genera.</div>'; return; }
  let r = '', h = '';
  DATA.partidos.forEach(p => {
    if (p.Ronda !== r) { r = p.Ronda; h += `<div class="ronda">${esc(r)} · ${esc(p.Fecha)}</div>`; }
    h += `<div class="match" data-id="${p.ID}"><span class="hr">${esc(p.Hora)}</span><span class="l">${esc(nom(p.LocalID))}</span>
      <input type="number" min="0" value="${esc(p.GolesLocal)}"><span>-</span><input type="number" min="0" value="${esc(p.GolesVisita)}">
      <span>${esc(nom(p.VisitaID))}</span><button class="sec">Guardar</button></div>`;
  });
  $('listPart').innerHTML = h;
}
$('listPart').onclick = e => {
  if (e.target.tagName !== 'BUTTON') return;
  const m = e.target.closest('.match'), i = m.querySelectorAll('input');
  accion(e.target, async () => { DATA = await run('guardarResultado', m.dataset.id, i[0].value, i[1].value); pintarPartidos(); toast('Resultado guardado'); });
};

// ----- Posiciones -----
$('bPos').onclick = () => accion($('bPos'), async () => {
  const t = await run('generarPosiciones');
  $('tabla').innerHTML = t.length ? `<table><tr><th>#</th><th>Equipo</th><th>PJ</th><th>PG</th><th>PE</th><th>PP</th><th>GF</th><th>GC</th><th>DG</th><th>Pts</th></tr>` +
    t.map(x => `<tr><td>${x.Pos}</td><td>${esc(x.Equipo)}</td><td>${x.PJ}</td><td>${x.PG}</td><td>${x.PE}</td><td>${x.PP}</td><td>${x.GF}</td><td>${x.GC}</td><td>${x.DG > 0 ? '+' : ''}${x.DG}</td><td class="pt">${x.Pts}</td></tr>`).join('') + '</table>'
    : '<div class="empty">No hay equipos registrados.</div>';
  toast('Tabla generada');
});

// ----- Inicio -----
$('fecha').value = new Date().toISOString().slice(0, 10);
nuevoForm();
cargar().catch(e => { toast(e.message, true); $('listEq').innerHTML = '<div class="empty">' + esc(e.message + (e.ayuda ? ' ' + e.ayuda : '')) + '</div>'; });
</script>
</body>
</html>
