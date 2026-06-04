<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="description" content="Dashboard interativo de Vendas e Resgates - Sorrifácil">
<title>Dashboard — Sorrifácil Tubarão</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Sans:opsz,wght@9..40,300;9..40,400;9..40,500;9..40,600;9..40,700&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
:root {
  /* Cores Modernas e Profundas */
  --bg: #050507;
  --surface: rgba(18, 25, 33, 0.65);
  --surface-hover: rgba(26, 35, 46, 0.85);
  --surface2: rgba(255, 255, 255, 0.03);
  --border: rgba(255, 255, 255, 0.08);
  --border-light: rgba(255, 255, 255, 0.15);
  
  /* Cores de Destaque */
  --accent: #00e5b5;     /* Verde vibrante */
  --accent2: #ff6b6b;    /* Vermelho pastel */
  --accent3: #ffcf54;    /* Amarelo ouro */
  --accent4: #b497fa;    /* Roxo suave */
  --blue: #5b9dff;       /* Azul claro */
  --pink: #f472b6;       /* Rosa */
  --green: #1bd665;      /* Verde sucesso */
  --orange: #ff7f2a;     /* Laranja vibrante */
  --red: #f43f5e;        /* Vermelho alerta */
  
  /* Textos */
  --text: #f8fafc;
  --muted: #94a3b8;

  /* Especialidades */
  --c-cg: #5b9dff;
  --c-orto: #b497fa;
  --c-prot: #f472b6;
  --c-impl: #00e5b5;
  --c-endo: #ffcf54;
  --c-comp: #ff7f2a;
  --c-cir: #f43f5e;
  --c-ped: #1bd665;
  --c-perio: #e879f9;
}

* { margin:0; padding:0; box-sizing:border-box; }

body { 
  font-family:'DM Sans',sans-serif; 
  background-color: var(--bg);
  background-image: 
    radial-gradient(circle at 15% 50%, rgba(0, 229, 181, 0.03) 0%, transparent 50%),
    radial-gradient(circle at 85% 30%, rgba(180, 151, 250, 0.04) 0%, transparent 50%);
  background-attachment: fixed;
  color:var(--text); 
  padding: 0 0 40px 0; 
  min-height: 100vh;
  overflow-x: hidden; /* Evita rolagem horizontal indesejada no GitHub Pages */
}

/* ── CUSTOM SCROLLBAR (Para não quebrar o visual Dark) ── */
::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}
::-webkit-scrollbar-track {
  background: var(--bg);
}
::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.15);
  border-radius: 4px;
}
::-webkit-scrollbar-thumb:hover {
  background: rgba(255, 255, 255, 0.3);
}

/* ── NAVEGAÇÃO SUPERIOR (TABS) ── */
.top-nav {
  position: sticky;
  top: 0;
  z-index: 100;
  background: rgba(5, 5, 7, 0.85);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border-bottom: 1px solid var(--border);
  padding: 16px 32px;
  display: flex;
  justify-content: center;
  gap: 12px;
}
.tab-btn {
  background: var(--surface2);
  border: 1px solid var(--border);
  color: var(--muted);
  padding: 10px 24px;
  border-radius: 30px;
  font-family: 'Syne', sans-serif;
  font-weight: 700;
  font-size: 14px;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  gap: 8px;
}
.tab-btn:hover {
  background: rgba(255, 255, 255, 0.08);
  color: var(--text);
  transform: translateY(-1px);
}
.tab-btn.active {
  background: linear-gradient(135deg, rgba(255,255,255,0.1), rgba(255,255,255,0.02));
  color: #fff;
  border-color: var(--border-light);
  box-shadow: 0 4px 16px rgba(0,0,0,0.3);
}
.tab-indicator {
  width: 8px; height: 8px; border-radius: 50%;
}
.tab-btn:nth-child(1) .tab-indicator { background: var(--blue); }
.tab-btn:nth-child(2) .tab-indicator { background: var(--orange); }

/* ── VIEW CONTAINER ── */
.view {
  display: none;
  animation: fadeIn 0.4s ease forwards;
  padding: 28px 32px;
  max-width: 1600px;
  margin: 0 auto;
}
.view.active {
  display: block;
}

/* ── HEADER ── */
.header { display:flex; align-items:center; justify-content:space-between; margin-bottom:32px; padding-bottom:20px; border-bottom:1px solid var(--border); flex-wrap: wrap; gap: 16px; }
.logo { width:52px; height:52px; border-radius:14px; display:flex; align-items:center; justify-content:center; font-family:'Syne',sans-serif; font-weight:800; font-size:20px; color:#fff; box-shadow: 0 8px 24px rgba(0,0,0,0.4); flex-shrink: 0; }
.logo.vendas { background:linear-gradient(135deg,var(--blue),var(--accent)); }
.logo.resgates { background:linear-gradient(135deg,var(--orange),var(--red)); }
.htitle { font-family:'Syne',sans-serif; font-size:24px; font-weight:800; letter-spacing: -0.5px; }
.hsub { font-size:13px; color:var(--muted); margin-top:4px; font-weight: 500; }
.hbadge { background:var(--surface2); border:1px solid var(--border); border-radius:10px; padding:10px 16px; font-size:13px; color:var(--muted); backdrop-filter: blur(10px); -webkit-backdrop-filter: blur(10px); }
.hbadge strong { color:var(--text); font-weight: 700; }

/* ── SECTION LABEL ── */
.sec { font-family:'Syne',sans-serif; font-size:11px; font-weight:700; letter-spacing:2px; text-transform:uppercase; color:var(--muted); margin:36px 0 16px; display: flex; align-items: center; gap: 12px; }
.sec::after { content: ''; flex: 1; height: 1px; background: linear-gradient(90deg, var(--border), transparent); }

/* ── KPI STRIP ── */
.kpi-strip { display:grid; grid-template-columns:repeat(5,1fr); gap:16px; margin-bottom:8px; }
.kpi { 
  background: var(--surface); 
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border: 1px solid var(--border); 
  border-radius: 16px; 
  padding: 22px 20px; 
  position: relative; 
  overflow: hidden; 
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.kpi:hover {
  transform: translateY(-3px);
  box-shadow: 0 12px 30px -10px var(--kc);
  border-color: var(--border-light);
}
.kpi::before { content:''; position:absolute; top:0; left:0; right:0; height:3px; background:var(--kc,var(--accent)); box-shadow: 0 0 12px var(--kc); }
.kpi-lbl { font-size:11px; font-weight:600; color:var(--muted); text-transform:uppercase; letter-spacing:1px; margin-bottom:8px; }
.kpi-val { font-family:'Syne',sans-serif; font-size:26px; font-weight:800; line-height:1; letter-spacing:-0.5px; }
.kpi-sub { font-size:12px; color:var(--muted); margin-top:8px; }

/* ── CARDS ── */
.card { 
  background: var(--surface); 
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border: 1px solid var(--border); 
  border-radius: 18px; 
  padding: 24px; 
  transition: all 0.3s ease;
  overflow: hidden;
}
.card:hover {
  border-color: var(--border-light);
  box-shadow: 0 10px 40px rgba(0,0,0,0.2);
}
.ctitle { font-family:'Syne',sans-serif; font-size:12px; font-weight:700; text-transform:uppercase; letter-spacing:1px; color:var(--text); margin-bottom:20px; display:flex; align-items:center; gap:8px; }
.cdot { width:8px; height:8px; border-radius:50%; background:var(--accent); flex-shrink:0; box-shadow: 0 0 8px currentColor; }

/* ── GRIDS ── */
.g2 { display:grid; grid-template-columns:1fr 1fr; gap:16px; }
.g3 { display:grid; grid-template-columns:1fr 1fr 1fr; gap:16px; }
.g4 { display:grid; grid-template-columns:repeat(4,1fr); gap:16px; }
.g52 { display:grid; grid-template-columns:5fr 3fr; gap:16px; }
.g53 { display:grid; grid-template-columns:5fr 3fr 3fr; gap:16px; }
.g35 { display:grid; grid-template-columns:3fr 5fr; gap:16px; }

/* ── VAL ROWS ── */
.val-row, .vrow, .extremo-row { display:flex; justify-content:space-between; align-items:baseline; padding:10px 0; border-bottom:1px solid var(--border); transition: background 0.2s; }
.val-row:last-child, .vrow:last-child, .extremo-row:last-child { border-bottom:none; }
.val-row:hover, .vrow:hover, .extremo-row:hover { background: rgba(255,255,255,0.015); border-radius: 4px; padding-left: 4px; padding-right: 4px; margin: 0 -4px; }
.val-lbl, .vlbl, .extremo-key { font-size:13px; color:var(--muted); font-weight: 500; }
.val-num, .vnum { font-family:'Syne',sans-serif; font-size:16px; font-weight:700; }
.val-sub, .vsub { font-size:11px; color:var(--muted); margin-top: 2px; }
.extremo-v { font-weight: 700; font-size: 13px; }

/* ── TABLE GERAL ── */
.table-wrapper { overflow-x: auto; width: 100%; -webkit-overflow-scrolling: touch; }
table { width:100%; border-collapse:collapse; font-size:13px; min-width: 600px; } /* Min-width para manter as tabelas legíveis */
th { font-size:11px; color:var(--muted); text-transform:uppercase; letter-spacing:1px; font-weight:700; padding:12px 10px; border-bottom: 2px solid var(--border); white-space:nowrap; }
td { padding: 12px 10px; border-bottom: 1px solid var(--border); transition: background 0.2s; }
tbody tr:hover td { background: rgba(255,255,255,0.02); }
tbody tr:last-child td { border-bottom: none; }

/* ── MINI BAR ── */
.mbar-wrap, .bbar-w, .faixa-bar-w { height:6px; background:rgba(255,255,255,0.05); border-radius:4px; overflow:hidden; position:relative; }
.mbar, .bbar { height:100%; border-radius:4px; position:relative; }
.bbar { background:linear-gradient(90deg,var(--accent),var(--blue)); box-shadow: 0 0 10px var(--accent); }
.faixa-bar-w { height: 20px; }
.faixa-bar { height:100%; border-radius:4px; display:flex; align-items:center; padding-left:10px; font-size:11px; font-weight:800; color:#fff; text-shadow: 0 1px 2px rgba(0,0,0,0.5); }

/* ── PILL ── */
.pill { display:inline-flex; align-items:center; padding:4px 10px; border-radius:20px; font-size:11px; font-weight:700; text-transform: uppercase; letter-spacing: 0.5px; white-space: nowrap; }
.pill-g { background:rgba(27,214,101,.15); color:var(--green); border: 1px solid rgba(27,214,101,.3); }
.pill-y { background:rgba(255,207,84,.15); color:var(--accent3); border: 1px solid rgba(255,207,84,.3); }
.pill-r { background:rgba(244,63,94,.15); color:var(--red); border: 1px solid rgba(244,63,94,.3); }
.pill-o { background:rgba(255,127,42,.15); color:var(--orange); border: 1px solid rgba(255,127,42,.3); }
.pill-b { background:rgba(91,157,255,.15); color:var(--blue); border: 1px solid rgba(91,157,255,.3); }
.pill-p { background:rgba(180,151,250,.15); color:var(--accent4); border: 1px solid rgba(180,151,250,.3); }

/* ── BAIRRO ── */
.brow { display:flex; align-items:center; gap:12px; padding:8px 0; transition: transform 0.2s; }
.brow:hover { transform: translateX(4px); }
.brank { font-size:11px; font-weight:700; color:var(--muted); width:16px; }
.bname { font-size:13px; font-weight:500; flex:1; }
.bcnt { font-size:13px; font-weight:700; width:16px; text-align:right; }

/* ── INSIGHT BOX ── */
.ibox { margin-top:16px; padding:14px 16px; border-radius:12px; font-size:13px; color:var(--muted); line-height:1.6; display: flex; gap: 10px; align-items: flex-start; }
.ibox strong { color:var(--text); font-weight: 700; }

/* ── ESPECIALIDADE DOT ── */
.esp-dot { width:10px; height:10px; border-radius:50%; display:inline-block; flex-shrink:0; box-shadow: 0 0 8px currentColor; }

/* ── EXTREMOS CARDS ── */
.extremo-card { background:rgba(0,0,0,0.2); border-radius:14px; padding:20px; transition: all 0.3s; }
.extremo-card:hover { transform: translateY(-2px); }
.extremo-tag { font-size:11px; font-weight:800; text-transform:uppercase; letter-spacing:1px; margin-bottom:12px; display:flex; align-items:center; gap:6px; }
.extremo-val { font-family:'Syne',sans-serif; font-size:28px; font-weight:800; line-height:1; margin-bottom:6px; letter-spacing:-0.5px; }

/* Específicos Tabela Canais */
.row-canal td { padding: 16px 10px 8px; font-family: 'Syne', sans-serif; font-size: 14px; font-weight: 800; border-top: 1px solid rgba(255,255,255,0.1); }
.row-sub td { padding: 6px 10px; font-size: 13px; }
.row-sub td:first-child { padding-left: 24px; color: var(--muted); }
.row-total td { padding: 14px 10px; font-weight: 800; background: rgba(255,255,255,0.03); border-top: 1px solid rgba(255,255,255,0.15); border-bottom: 1px solid rgba(255,255,255,0.15); font-family: 'Syne', sans-serif; font-size: 14px;}
.tipo-chip { display: inline-flex; align-items: center; gap: 6px; padding: 3px 8px; border-radius: 6px; font-size: 11px; font-weight: 700; letter-spacing: 0.5px; text-transform: uppercase; }
.chip-int { background: rgba(180,151,250,.15); color: var(--accent4); border: 1px solid rgba(180,151,250,0.3); }
.chip-man { background: rgba(0,229,181,.15); color: var(--accent); border: 1px solid rgba(0,229,181,0.3); }
.chip-tot { background: rgba(255,255,255,.1); color: var(--text); border: 1px solid rgba(255,255,255,0.2); }

/* Ajuste Fundo Transparente para Tabelas */
table { width: 100%; border-collapse: collapse; font-size: 13px; background-color: transparent; color: var(--text); }
th { font-size: 11px; color: var(--muted); text-transform: uppercase; letter-spacing: 1px; font-weight: 700; padding: 12px 10px; border-bottom: 2px solid var(--border); white-space: nowrap; background-color: transparent; }
td { padding: 12px 10px; border-bottom: 1px solid var(--border); transition: background 0.2s; background-color: transparent; }
tbody tr:hover td { background: rgba(255,255,255,0.02); }
tbody tr:last-child td { border-bottom: none; }

@keyframes fadeIn { from{opacity:0;transform:translateY(10px)} to{opacity:1;transform:none} }
.kpi, .card { animation:fadeIn 0.5s cubic-bezier(0.16, 1, 0.3, 1) backwards; }
.kpi:nth-child(1) { animation-delay: 0.05s; }
.kpi:nth-child(2) { animation-delay: 0.1s; }
.kpi:nth-child(3) { animation-delay: 0.15s; }
.kpi:nth-child(4) { animation-delay: 0.2s; }
.kpi:nth-child(5) { animation-delay: 0.25s; }

/* ── RESPONSIVIDADE PARA GIT/MOBILE ── */
@media (max-width: 1200px) {
  .kpi-strip { grid-template-columns: repeat(3, 1fr); }
  .g4 { grid-template-columns: repeat(2, 1fr); }
}

@media (max-width: 992px) {
  .kpi-strip { grid-template-columns: repeat(2, 1fr); }
  .g3, .g52, .g53, .g35 { grid-template-columns: 1fr; }
}

@media (max-width: 768px) {
  body { padding: 0 0 20px 0; }
  .view { padding: 16px; }
  .g2, .kpi-strip, .g4 { grid-template-columns: 1fr; }
  .header { flex-direction: column; align-items: flex-start; gap: 16px; }
  .top-nav { flex-direction: column; align-items: stretch; padding: 12px; }
  .tab-btn { justify-content: center; }
  .hbadge { width: 100%; text-align: center; }
  
  /* Ajuste de tabelas para celular */
  .row-canal td { font-size: 12px; }
  
  .val-row { flex-wrap: wrap; }
  .val-lbl { width: 100%; margin-bottom: 4px; }
  .val-row > div:nth-child(2) { text-align: left !important; }
}
</style>
</head>
<body>

<!-- NAVEGAÇÃO -->
<nav class="top-nav">
  <button class="tab-btn active" onclick="switchTab('vendas')">
    <span class="tab-indicator"></span> Dashboard Vendas
  </button>
  <button class="tab-btn" onclick="switchTab('resgates')">
    <span class="tab-indicator"></span> Análise de Resgates
  </button>
</nav>

<!-- ==========================================
     VIEW: VENDAS
=========================================== -->
<div id="vendas-view" class="view active">
  <div class="header">
    <div style="display:flex;align-items:center;gap:18px">
      <div class="logo vendas">SF</div>
      <div>
        <div class="htitle">Sorrifácil Tubarão</div>
        <div class="hsub">Dashboard de Vendas &amp; Performance</div>
      </div>
    </div>
    <div class="hbadge">106 registros &nbsp;·&nbsp; Atualizado em <strong style="color:var(--blue)">Jun/2026</strong></div>
  </div>

  <div class="sec">Resumo Geral</div>
  <div class="kpi-strip">
    <div class="kpi" style="--kc:var(--blue)">
      <div class="kpi-lbl">Total de Leads</div>
      <div class="kpi-val">106</div>
      <div class="kpi-sub">Base total analisada</div>
    </div>
    <div class="kpi" style="--kc:var(--accent3)">
      <div class="kpi-lbl">Orçamentos Abertos</div>
      <div class="kpi-val" style="color:var(--accent3)">60</div>
      <div class="kpi-sub">Pipeline: <strong style="color:var(--text)">R$ 576.642</strong></div>
    </div>
    <div class="kpi" style="--kc:var(--green)">
      <div class="kpi-lbl">Efetivados</div>
      <div class="kpi-val" style="color:var(--green)">46</div>
      <div class="kpi-sub">Receita: <strong style="color:var(--text)">R$ 181.453</strong></div>
    </div>
    <div class="kpi" style="--kc:var(--accent4)">
      <div class="kpi-lbl">Ticket Médio Geral</div>
      <div class="kpi-val" style="color:var(--accent4);">R$ 4.032</div>
      <div class="kpi-sub">Por fechamento efetivado</div>
    </div>
    <div class="kpi" style="--kc:var(--accent)">
      <div class="kpi-lbl">Conversão Geral</div>
      <div class="kpi-val">43,4%</div>
      <div class="kpi-sub">Mediana: <strong style="color:var(--text)">2 dias</strong> p/ fechar</div>
    </div>
  </div>

  <div class="sec">Tipo de Cadastro — Integração vs. Manual</div>
  <div class="g2">
    <!-- INTEGRAÇÃO -->
    <div class="card">
      <div class="ctitle"><span class="cdot" style="background:var(--accent4); color:var(--accent4)"></span>Integração (Automático via CRM)</div>
      <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:20px">
        <div style="background:rgba(0,0,0,0.2);border-radius:12px;padding:16px; border:1px solid rgba(255,255,255,0.05)">
          <div style="font-size:11px;font-weight:700;color:var(--muted);margin-bottom:6px">LEADS</div>
          <div style="font-family:'Syne',sans-serif;font-size:32px;font-weight:800;color:var(--accent4)">49</div>
          <div style="font-size:12px;color:var(--muted);margin-top:4px">17 efetivados · 32 abertos</div>
        </div>
        <div style="background:rgba(0,0,0,0.2);border-radius:12px;padding:16px; border:1px solid rgba(255,255,255,0.05)">
          <div style="font-size:11px;font-weight:700;color:var(--muted);margin-bottom:6px">CONVERSÃO</div>
          <div style="font-family:'Syne',sans-serif;font-size:32px;font-weight:800;color:var(--orange)">34,7%</div>
          <div style="margin-top:6px"><span class="pill pill-r">Abaixo da média</span></div>
        </div>
      </div>
      <div class="val-row">
        <div><div class="val-lbl">Orçamento Total</div></div>
        <div style="text-align:right"><div class="val-num">R$ 397.263</div><div class="val-sub">soma de todos os orçamentos</div></div>
      </div>
      <div class="val-row">
        <div><div class="val-lbl">Pipeline em Aberto</div></div>
        <div style="text-align:right"><div class="val-num" style="color:var(--accent3)">R$ 287.184</div><div class="val-sub">32 orçamentos aguardando</div></div>
      </div>
      <div class="val-row">
        <div><div class="val-lbl">Receita Efetivada</div></div>
        <div style="text-align:right"><div class="val-num" style="color:var(--accent4)">R$ 77.760</div><div class="val-sub">17 vendas fechadas</div></div>
      </div>
      <div class="val-row">
        <div><div class="val-lbl">Ticket Médio</div></div>
        <div style="text-align:right"><div class="val-num" style="color:var(--green)">R$ 4.860</div><div class="val-sub">por venda efetivada</div></div>
      </div>
      <div class="ibox" style="background:rgba(180,151,250,.08);border:1px solid rgba(180,151,250,.2)">
        <span style="font-size:18px">💡</span>
        <div>Apesar do ticket maior (R$ 4.860), a taxa de conversão é <strong>16pp inferior</strong> ao manual. Volume de abertos represado: R$ 287k de pipeline.</div>
      </div>
    </div>

    <!-- MANUAL -->
    <div class="card">
      <div class="ctitle"><span class="cdot" style="background:var(--accent); color:var(--accent)"></span>Manual (Inserção Humana)</div>
      <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:20px">
        <div style="background:rgba(0,0,0,0.2);border-radius:12px;padding:16px; border:1px solid rgba(255,255,255,0.05)">
          <div style="font-size:11px;font-weight:700;color:var(--muted);margin-bottom:6px">LEADS</div>
          <div style="font-family:'Syne',sans-serif;font-size:32px;font-weight:800;color:var(--accent)">57</div>
          <div style="font-size:12px;color:var(--muted);margin-top:4px">29 efetivados · 28 abertos</div>
        </div>
        <div style="background:rgba(0,0,0,0.2);border-radius:12px;padding:16px; border:1px solid rgba(255,255,255,0.05)">
          <div style="font-size:11px;font-weight:700;color:var(--muted);margin-bottom:6px">CONVERSÃO</div>
          <div style="font-family:'Syne',sans-serif;font-size:32px;font-weight:800;color:var(--green)">50,9%</div>
          <div style="margin-top:6px"><span class="pill pill-g">Acima da média</span></div>
        </div>
      </div>
      <div class="val-row">
        <div><div class="val-lbl">Orçamento Total</div></div>
        <div style="text-align:right"><div class="val-num">R$ 512.116</div><div class="val-sub">soma de todos os orçamentos</div></div>
      </div>
      <div class="val-row">
        <div><div class="val-lbl">Pipeline em Aberto</div></div>
        <div style="text-align:right"><div class="val-num" style="color:var(--accent3)">R$ 289.458</div><div class="val-sub">28 orçamentos aguardando</div></div>
      </div>
      <div class="val-row">
        <div><div class="val-lbl">Receita Efetivada</div></div>
        <div style="text-align:right"><div class="val-num" style="color:var(--accent)">R$ 103.692</div><div class="val-sub">29 vendas fechadas</div></div>
      </div>
      <div class="val-row">
        <div><div class="val-lbl">Ticket Médio</div></div>
        <div style="text-align:right"><div class="val-num" style="color:var(--green)">R$ 3.575</div><div class="val-sub">por venda efetivada</div></div>
      </div>
      <div class="ibox" style="background:rgba(0,229,181,.08);border:1px solid rgba(0,229,181,.2)">
        <span style="font-size:18px">✅</span>
        <div>Melhor conversão (50,9%) e maior receita absoluta (R$ 103k). Ticket menor que integração — vendas de procedimentos menores com mais volume.</div>
      </div>
    </div>
  </div>

  <div style="margin-top:16px" class="card">
    <div class="ctitle"><span class="cdot" style="background:var(--blue); color:var(--blue)"></span>Comparativo Direto — Integração vs. Manual</div>
    <div class="table-wrapper">
      <table>
        <thead>
          <tr>
            <th style="text-align:left; color:var(--muted);">Métrica</th>
            <th style="text-align:right; color:var(--accent4);">🤖 Integração</th>
            <th style="text-align:right; color:var(--accent);">✍️ Manual</th>
            <th style="text-align:right; color:var(--muted);">Diferença</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td style="color:var(--muted); font-weight:500;">Total de Leads</td>
            <td style="text-align:right;font-weight:700">49</td>
            <td style="text-align:right;font-weight:700">57</td>
            <td style="text-align:right;color:var(--green);font-weight:700">+8 manual</td>
          </tr>
          <tr>
            <td style="color:var(--muted); font-weight:500;">Leads Efetivados</td>
            <td style="text-align:right;font-weight:700">17</td>
            <td style="text-align:right;font-weight:700">29</td>
            <td style="text-align:right;color:var(--green);font-weight:700">+12 manual</td>
          </tr>
          <tr>
            <td style="color:var(--muted); font-weight:500;">Taxa de Conversão</td>
            <td style="text-align:right;font-weight:700;color:var(--orange)">34,7%</td>
            <td style="text-align:right;font-weight:700;color:var(--green)">50,9%</td>
            <td style="text-align:right;color:var(--green);font-weight:700">+16,2pp manual</td>
          </tr>
          <tr>
            <td style="color:var(--muted); font-weight:500;">Receita Efetivada</td>
            <td style="text-align:right;font-family:'Syne',sans-serif;font-weight:800;color:var(--accent4); font-size:15px;">R$ 77.760</td>
            <td style="text-align:right;font-family:'Syne',sans-serif;font-weight:800;color:var(--accent); font-size:15px;">R$ 103.692</td>
            <td style="text-align:right;color:var(--green);font-weight:700">+R$ 25.932 manual</td>
          </tr>
          <tr>
            <td style="color:var(--muted); font-weight:500;">Ticket Médio</td>
            <td style="text-align:right;font-family:'Syne',sans-serif;font-weight:800;color:var(--accent4); font-size:15px;">R$ 4.860</td>
            <td style="text-align:right;font-family:'Syne',sans-serif;font-weight:800;color:var(--accent); font-size:15px;">R$ 3.575</td>
            <td style="text-align:right;color:var(--accent4);font-weight:700">+R$ 1.285 integração</td>
          </tr>
          <tr>
            <td style="color:var(--muted); font-weight:500;">Pipeline Aberto (R$)</td>
            <td style="text-align:right;font-family:'Syne',sans-serif;font-weight:800;color:var(--accent3); font-size:15px;">R$ 287.184</td>
            <td style="text-align:right;font-family:'Syne',sans-serif;font-weight:800;color:var(--accent3); font-size:15px;">R$ 289.458</td>
            <td style="text-align:right;color:var(--muted);font-weight:700">Equilibrado</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

  <div class="sec">Origem dos Leads — por Canal</div>
  <div class="card">
    <div class="ctitle"><span class="cdot" style="background:var(--pink); color:var(--pink)"></span>Performance por Canal — Integração vs. Manual</div>
    <div class="table-wrapper">
    <table style="border-collapse: collapse;">
      <thead>
        <tr>
          <th style="text-align:left;padding-left:0;min-width:180px">Canal / Tipo</th>
          <th style="text-align:right;">Leads</th>
          <th style="text-align:right;">Efet.</th>
          <th style="text-align:right;">Abertos</th>
          <th style="text-align:right;">Conv.%</th>
          <th style="text-align:right;">Receita (R$)</th>
          <th style="text-align:right;">Ticket Médio (R$)</th>
          <th style="text-align:right;">Pipeline Aberto (R$)</th>
        </tr>
      </thead>
      <tbody>
        <!-- WHATSAPP -->
        <tr class="row-canal">
          <td style="text-align:left; padding-left:0;"><span style="font-size:18px; margin-right:8px;">💬</span> WhatsApp</td>
          <td style="text-align:right;"><span style="color:var(--text)">51</span></td>
          <td style="text-align:right;"><span style="color:var(--green)">22</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3)">29</span></td>
          <td style="text-align:right;"><span class="pill pill-b">43,1%</span></td>
          <td style="text-align:right;"><span style="color:var(--accent);font-family:'Syne',sans-serif; font-size:16px;">82.395</span></td>
          <td style="text-align:right;"><span>3.745</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3)">240.562</span></td>
        </tr>
        <tr class="row-sub">
          <td style="text-align:left;"><span class="tipo-chip chip-int">🤖 Integração</span></td>
          <td style="text-align:right;"><span style="font-weight:600">32</span></td>
          <td style="text-align:right;"><span style="color:var(--green); font-weight:700">13</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3); font-weight:700">19</span></td>
          <td style="text-align:right;"><span style="color:var(--orange); font-weight:700">40,6%</span></td>
          <td style="text-align:right;"><span style="color:var(--accent); font-weight:700">51.259</span></td>
          <td style="text-align:right;"><span style="font-weight:600">3.943</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3); font-weight:700">126.043</span></td>
        </tr>
        <tr class="row-sub">
          <td style="text-align:left;"><span class="tipo-chip chip-man">✍️ Manual</span></td>
          <td style="text-align:right;"><span style="font-weight:600">19</span></td>
          <td style="text-align:right;"><span style="color:var(--green); font-weight:700">9</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3); font-weight:700">10</span></td>
          <td style="text-align:right;"><span style="color:var(--green); font-weight:700">47,4%</span></td>
          <td style="text-align:right;"><span style="color:var(--accent); font-weight:700">31.135</span></td>
          <td style="text-align:right;"><span style="font-weight:600">3.459</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3); font-weight:700">114.519</span></td>
        </tr>
        <tr><td colspan="8" style="padding:4px; border:none;"></td></tr>

        <!-- FACEBOOK -->
        <tr class="row-canal">
          <td style="text-align:left; padding-left:0;"><span style="font-size:18px; margin-right:8px;">📘</span> Facebook</td>
          <td style="text-align:right;"><span style="color:var(--text)">18</span></td>
          <td style="text-align:right;"><span style="color:var(--green)">6</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3)">12</span></td>
          <td style="text-align:right;"><span class="pill pill-y">33,3%</span></td>
          <td style="text-align:right;"><span style="color:var(--accent);font-family:'Syne',sans-serif; font-size:16px;">32.163</span></td>
          <td style="text-align:right;"><span>5.360</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3)">143.013</span></td>
        </tr>
        <tr class="row-sub">
          <td style="text-align:left;"><span class="tipo-chip chip-int">🤖 Integração</span></td>
          <td style="text-align:right;"><span style="font-weight:600">7</span></td>
          <td style="text-align:right;"><span style="color:var(--green); font-weight:700">2</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3); font-weight:700">5</span></td>
          <td style="text-align:right;"><span style="color:var(--orange); font-weight:700">28,6%</span></td>
          <td style="text-align:right;"><span style="color:var(--accent); font-weight:700">17.700</span></td>
          <td style="text-align:right;"><span style="font-weight:600">8.850</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3); font-weight:700">65.290</span></td>
        </tr>
        <tr class="row-sub">
          <td style="text-align:left;"><span class="tipo-chip chip-man">✍️ Manual</span></td>
          <td style="text-align:right;"><span style="font-weight:600">11</span></td>
          <td style="text-align:right;"><span style="color:var(--green); font-weight:700">4</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3); font-weight:700">7</span></td>
          <td style="text-align:right;"><span style="color:var(--green); font-weight:700">36,4%</span></td>
          <td style="text-align:right;"><span style="color:var(--accent); font-weight:700">14.463</span></td>
          <td style="text-align:right;"><span style="font-weight:600">3.615</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3); font-weight:700">77.723</span></td>
        </tr>
        <tr><td colspan="8" style="padding:4px; border:none;"></td></tr>

        <!-- INSTAGRAM -->
        <tr class="row-canal">
          <td style="text-align:left; padding-left:0;"><span style="font-size:18px; margin-right:8px;">📸</span> Instagram</td>
          <td style="text-align:right;"><span style="color:var(--text)">16</span></td>
          <td style="text-align:right;"><span style="color:var(--green)">5</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3)">11</span></td>
          <td style="text-align:right;"><span class="pill pill-y">31,2%</span></td>
          <td style="text-align:right;"><span style="color:var(--accent);font-family:'Syne',sans-serif; font-size:16px;">16.535</span></td>
          <td style="text-align:right;"><span>4.133</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3)">134.711</span></td>
        </tr>
        <tr class="row-sub">
          <td style="text-align:left;"><span class="tipo-chip chip-int">🤖 Integração</span></td>
          <td style="text-align:right;"><span style="font-weight:600">10</span></td>
          <td style="text-align:right;"><span style="color:var(--green); font-weight:700">2</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3); font-weight:700">8</span></td>
          <td style="text-align:right;"><span style="color:var(--red); font-weight:700">20,0%</span></td>
          <td style="text-align:right;"><span style="color:var(--accent); font-weight:700">8.800</span></td>
          <td style="text-align:right;"><span style="font-weight:600">8.800</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3); font-weight:700">95.851</span></td>
        </tr>
        <tr class="row-sub">
          <td style="text-align:left;"><span class="tipo-chip chip-man">✍️ Manual</span></td>
          <td style="text-align:right;"><span style="font-weight:600">6</span></td>
          <td style="text-align:right;"><span style="color:var(--green); font-weight:700">3</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3); font-weight:700">3</span></td>
          <td style="text-align:right;"><span style="color:var(--green); font-weight:700">50,0%</span></td>
          <td style="text-align:right;"><span style="color:var(--accent); font-weight:700">7.735</span></td>
          <td style="text-align:right;"><span style="font-weight:600">2.578</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3); font-weight:700">38.860</span></td>
        </tr>
        <tr><td colspan="8" style="padding:4px; border:none;"></td></tr>

        <!-- FACHADA -->
        <tr class="row-canal">
          <td style="text-align:left; padding-left:0;"><span style="font-size:18px; margin-right:8px;">🏢</span> Fachada</td>
          <td style="text-align:right;"><span style="color:var(--text)">12</span></td>
          <td style="text-align:right;"><span style="color:var(--green)">7</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3)">5</span></td>
          <td style="text-align:right;"><span class="pill pill-g">58,3%</span></td>
          <td style="text-align:right;"><span style="color:var(--accent);font-family:'Syne',sans-serif; font-size:16px;">37.451</span></td>
          <td style="text-align:right;"><span>5.350</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3)">40.746</span></td>
        </tr>
        <tr class="row-sub">
          <td style="text-align:left;"><span class="tipo-chip chip-int" style="opacity:.4">🤖 Integração</span></td>
          <td colspan="7" style="color:var(--muted);font-size:12px;font-style:italic;text-align:center;">sem registros</td>
        </tr>
        <tr class="row-sub">
          <td style="text-align:left;"><span class="tipo-chip chip-man">✍️ Manual</span></td>
          <td style="text-align:right;"><span style="font-weight:600">12</span></td>
          <td style="text-align:right;"><span style="color:var(--green); font-weight:700">7</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3); font-weight:700">5</span></td>
          <td style="text-align:right;"><span style="color:var(--green); font-weight:700">58,3%</span></td>
          <td style="text-align:right;"><span style="color:var(--accent); font-weight:700">37.451</span></td>
          <td style="text-align:right;"><span style="font-weight:600">5.350</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3); font-weight:700">40.746</span></td>
        </tr>
        <tr><td colspan="8" style="padding:4px; border:none;"></td></tr>

        <!-- GOOGLE -->
        <tr class="row-canal">
          <td style="text-align:left; padding-left:0;"><span style="font-size:18px; margin-right:8px;">🔍</span> Google</td>
          <td style="text-align:right;"><span style="color:var(--text)">4</span></td>
          <td style="text-align:right;"><span style="color:var(--green)">3</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3)">1</span></td>
          <td style="text-align:right;"><span class="pill pill-g">75,0%</span></td>
          <td style="text-align:right;"><span style="color:var(--accent);font-family:'Syne',sans-serif; font-size:16px;">3.180</span></td>
          <td style="text-align:right;"><span>1.060</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3)">7.820</span></td>
        </tr>
        <tr class="row-sub">
          <td style="text-align:left;"><span class="tipo-chip chip-int" style="opacity:.4">🤖 Integração</span></td>
          <td colspan="7" style="color:var(--muted);font-size:12px;font-style:italic;text-align:center;">sem registros</td>
        </tr>
        <tr class="row-sub">
          <td style="text-align:left;"><span class="tipo-chip chip-man">✍️ Manual</span></td>
          <td style="text-align:right;"><span style="font-weight:600">4</span></td>
          <td style="text-align:right;"><span style="color:var(--green); font-weight:700">3</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3); font-weight:700">1</span></td>
          <td style="text-align:right;"><span style="color:var(--green); font-weight:700">75,0%</span></td>
          <td style="text-align:right;"><span style="color:var(--accent); font-weight:700">3.180</span></td>
          <td style="text-align:right;"><span style="font-weight:600">1.060</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3); font-weight:700">7.820</span></td>
        </tr>
        <tr><td colspan="8" style="padding:4px; border:none;"></td></tr>

        <!-- INDICAÇÃO -->
        <tr class="row-canal">
          <td style="text-align:left; padding-left:0;"><span style="font-size:18px; margin-right:8px;">🤝</span> Indicação</td>
          <td style="text-align:right;"><span style="color:var(--text)">3</span></td>
          <td style="text-align:right;"><span style="color:var(--green)">1</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3)">2</span></td>
          <td style="text-align:right;"><span class="pill pill-y">33,3%</span></td>
          <td style="text-align:right;"><span style="color:var(--accent);font-family:'Syne',sans-serif; font-size:16px;">160</span></td>
          <td style="text-align:right;"><span>160</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3)">9.790</span></td>
        </tr>
        <tr class="row-sub">
          <td style="text-align:left;"><span class="tipo-chip chip-int" style="opacity:.4">🤖 Integração</span></td>
          <td colspan="7" style="color:var(--muted);font-size:12px;font-style:italic;text-align:center;">sem registros</td>
        </tr>
        <tr class="row-sub">
          <td style="text-align:left;"><span class="tipo-chip chip-man">✍️ Manual</span></td>
          <td style="text-align:right;"><span style="font-weight:600">3</span></td>
          <td style="text-align:right;"><span style="color:var(--green); font-weight:700">1</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3); font-weight:700">2</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3); font-weight:700">33,3%</span></td>
          <td style="text-align:right;"><span style="color:var(--accent); font-weight:700">160</span></td>
          <td style="text-align:right;"><span style="font-weight:600">160</span></td>
          <td style="text-align:right;"><span style="color:var(--accent3); font-weight:700">9.790</span></td>
        </tr>

        <!-- TOTAL GERAL -->
        <tr class="row-total">
          <td style="text-align:left;"><span class="tipo-chip chip-tot" style="font-size:12px;">📊 TOTAL GERAL</span></td>
          <td style="text-align:right; color:var(--text); font-size:16px;">104</td>
          <td style="text-align:right; color:var(--green); font-size:16px;">44</td>
          <td style="text-align:right; color:var(--accent3); font-size:16px;">60</td>
          <td style="text-align:right;"><span class="pill pill-b">42,3%</span></td>
          <td style="text-align:right; color:var(--accent); font-size:18px;">171.884</td>
          <td style="text-align:right; font-size:16px;">3.906</td>
          <td style="text-align:right; color:var(--accent3); font-size:16px;">576.642</td>
        </tr>
      </tbody>
    </table>
    </div>

    <!-- INSIGHTS -->
    <div style="display:grid;grid-template-columns:repeat(auto-fit, minmax(300px, 1fr));gap:16px;margin-top:24px">
      <div class="ibox" style="background:rgba(180,151,250,.08);border:1px solid rgba(180,151,250,.2); margin-top:0;">
        <span style="font-size:18px">🤖</span>
        <div><strong>Instagram via Integração</strong> tem a pior conversão: 20%. 10 leads, só 2 fechados, R$ 95k de pipeline represado.</div>
      </div>
      <div class="ibox" style="background:rgba(0,229,181,.08);border:1px solid rgba(0,229,181,.2); margin-top:0;">
        <span style="font-size:18px">✍️</span>
        <div><strong>Instagram Manual</strong> converte 50% — 2,5× mais que a integração. Mesma origem, tratamento diferente, resultado diferente.</div>
      </div>
      <div class="ibox" style="background:rgba(255,207,84,.08);border:1px solid rgba(255,207,84,.2); margin-top:0;">
        <span style="font-size:18px">💰</span>
        <div><strong>Facebook Integração</strong> tem o maior ticket: R$ 8.850 — mas só 28,6% de conversão. Leads caros e de alta intenção sendo perdidos.</div>
      </div>
    </div>
  </div>

  <div class="sec">Análise Demográfica</div>
  <div class="g3">
    <!-- GÊNERO -->
    <div class="card">
      <div class="ctitle"><span class="cdot" style="background:var(--pink); color:var(--pink)"></span>Gênero</div>
      <!-- Feminino -->
      <div style="background:rgba(0,0,0,0.2);border-radius:12px;padding:16px;margin-bottom:12px; border:1px solid rgba(255,255,255,0.05)">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px">
          <div style="font-size:14px;font-weight:700">♀ Feminino</div>
          <span class="pill" style="background:rgba(244,114,182,.15);color:var(--pink); border:1px solid rgba(244,114,182,.3)">43,8% conv.</span>
        </div>
        <div class="val-row" style="padding:6px 0"><div class="val-lbl">Leads / Efetivadas</div><div class="val-num">64 &nbsp;/&nbsp; <span style="color:var(--green)">28</span></div></div>
        <div class="val-row" style="padding:6px 0"><div class="val-lbl">Receita Efetivada</div><div class="val-num" style="color:var(--pink)">R$ 103.584</div></div>
        <div class="val-row" style="padding:6px 0; border:none;"><div class="val-lbl">Ticket Médio</div><div class="val-num">R$ 3.836</div></div>
      </div>
      <!-- Masculino -->
      <div style="background:rgba(0,0,0,0.2);border-radius:12px;padding:16px; border:1px solid rgba(255,255,255,0.05)">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px">
          <div style="font-size:14px;font-weight:700">♂ Masculino</div>
          <span class="pill" style="background:rgba(91,157,255,.15);color:var(--blue); border:1px solid rgba(91,157,255,.3)">42,9% conv.</span>
        </div>
        <div class="val-row" style="padding:6px 0"><div class="val-lbl">Leads / Efetivados</div><div class="val-num">42 &nbsp;/&nbsp; <span style="color:var(--green)">18</span></div></div>
        <div class="val-row" style="padding:6px 0"><div class="val-lbl">Receita Efetivada</div><div class="val-num" style="color:var(--blue)">R$ 77.868</div></div>
        <div class="val-row" style="padding:6px 0; border:none;"><div class="val-lbl">Ticket Médio</div><div class="val-num">R$ 4.326</div></div>
      </div>
      <div class="ibox" style="background:rgba(91,157,255,.08);border:1px solid rgba(91,157,255,.2)">
        <span style="font-size:16px">ℹ️</span>
        <div>Conversão quase idêntica entre gêneros. <strong>Masculino tem ticket R$ 490 maior</strong>, mas feminino representa 57% da receita total.</div>
      </div>
    </div>

    <!-- FAIXA ETÁRIA -->
    <div class="card">
      <div class="ctitle"><span class="cdot" style="background:var(--accent3); color:var(--accent3)"></span>Faixa Etária — Receita &amp; Ticket</div>
      <div class="table-wrapper">
        <table>
          <thead>
            <tr>
              <th style="text-align:left; padding-left:0;">Faixa</th>
              <th style="text-align:center;">Efet.</th>
              <th style="text-align:right;">Receita</th>
              <th style="text-align:right;">Ticket</th>
              <th style="text-align:right;">Total</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td style="padding-left:0; font-weight:700;">&lt;18</td>
              <td style="text-align:center;font-weight:700;color:var(--green)">7</td>
              <td style="text-align:right;font-weight:700">4.960</td>
              <td style="text-align:right;color:var(--muted);font-weight:600">827</td>
              <td style="text-align:right;color:var(--muted)">11</td>
            </tr>
            <tr>
              <td style="padding-left:0; font-weight:700;">18-29</td>
              <td style="text-align:center;font-weight:700;color:var(--green)">9</td>
              <td style="text-align:right;font-weight:700">38.468</td>
              <td style="text-align:right;font-weight:700">4.274</td>
              <td style="text-align:right;color:var(--muted)">25</td>
            </tr>
            <tr>
              <td style="padding-left:0; font-weight:700;">30-39</td>
              <td style="text-align:center;font-weight:700;color:var(--green)">12</td>
              <td style="text-align:right;font-weight:700">39.405</td>
              <td style="text-align:right;font-weight:700">3.283</td>
              <td style="text-align:right;color:var(--muted)">26</td>
            </tr>
            <tr>
              <td style="padding-left:0; font-weight:700;">40-49</td>
              <td style="text-align:center;font-weight:700;color:var(--green)">5</td>
              <td style="text-align:right;font-weight:700">22.957</td>
              <td style="text-align:right;font-weight:700">4.591</td>
              <td style="text-align:right;color:var(--muted)">14</td>
            </tr>
            <tr>
              <td style="padding-left:0; font-weight:700;">50-59</td>
              <td style="text-align:center;font-weight:700;color:var(--green)">7</td>
              <td style="text-align:right;font-weight:700">15.900</td>
              <td style="text-align:right;color:var(--muted);font-weight:600">2.271</td>
              <td style="text-align:right;color:var(--muted)">14</td>
            </tr>
            <tr>
              <td style="padding-left:0; font-weight:700;">60+</td>
              <td style="text-align:center;font-weight:700;color:var(--green)">6</td>
              <td style="text-align:right;font-family:'Syne',sans-serif;font-weight:800;color:var(--accent3);font-size:14px;">59.760</td>
              <td style="text-align:right;font-family:'Syne',sans-serif;font-weight:800;color:var(--accent3);font-size:14px;">9.960</td>
              <td style="text-align:right;color:var(--muted)">16</td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="ibox" style="background:rgba(255,207,84,.08);border:1px solid rgba(255,207,84,.2)">
        <span style="font-size:18px">🏆</span>
        <div><strong>60+ anos tem o maior ticket médio: R$ 9.960</strong> — quase 3× a média geral. Menos volume, mas alta receita por venda (implantes/próteses).</div>
      </div>
    </div>

    <!-- MUNICÍPIOS -->
    <div class="card">
      <div class="ctitle"><span class="cdot" style="background:var(--orange); color:var(--orange)"></span>Municípios</div>
      <div class="table-wrapper">
        <table>
          <thead>
            <tr>
              <th style="text-align:left; padding-left:0;">Município</th>
              <th style="text-align:center;">Efet.</th>
              <th style="text-align:right;">Receita</th>
              <th style="text-align:right;">Ticket</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td style="padding-left:0; display:flex;align-items:center;gap:8px;"><span class="esp-dot" style="background:var(--accent)"></span>Tubarão</td>
              <td style="text-align:center;font-weight:700;color:var(--green)">35</td>
              <td style="text-align:right;font-weight:800;color:var(--accent)">124.847</td>
              <td style="text-align:right;font-weight:700">3.567</td>
            </tr>
            <tr>
              <td style="padding-left:0; display:flex;align-items:center;gap:8px;"><span class="esp-dot" style="background:var(--accent4)"></span>Capivari de Baixo</td>
              <td style="text-align:center;font-weight:700;color:var(--green)">4</td>
              <td style="text-align:right;font-family:'Syne',sans-serif;font-weight:800;color:var(--accent3)">38.100</td>
              <td style="text-align:right;font-family:'Syne',sans-serif;font-weight:800;color:var(--accent3)">9.525</td>
            </tr>
            <tr>
              <td style="padding-left:0; display:flex;align-items:center;gap:8px;"><span class="esp-dot" style="background:var(--orange)"></span>Imbituba</td>
              <td style="text-align:center;font-weight:700;color:var(--green)">1</td>
              <td style="text-align:right;font-weight:800;color:var(--orange)">8.667</td>
              <td style="text-align:right;font-weight:700;color:var(--orange)">8.667</td>
            </tr>
            <tr>
              <td style="padding-left:0; display:flex;align-items:center;gap:8px;"><span class="esp-dot" style="background:var(--green)"></span>Gravatal</td>
              <td style="text-align:center;font-weight:700;color:var(--green)">2</td>
              <td style="text-align:right;font-weight:700">6.480</td>
              <td style="text-align:right;font-weight:700">3.240</td>
            </tr>
            <tr>
              <td style="padding-left:0; display:flex;align-items:center;gap:8px;"><span class="esp-dot" style="background:var(--pink)"></span>Pescaria Brava</td>
              <td style="text-align:center;font-weight:700;color:var(--green)">1</td>
              <td style="text-align:right;font-weight:700">2.220</td>
              <td style="text-align:right;font-weight:700">2.220</td>
            </tr>
            <tr>
              <td style="padding-left:0; color:var(--muted); display:flex;align-items:center;gap:8px;"><span class="esp-dot" style="background:var(--muted)"></span>Demais cidades</td>
              <td style="text-align:center;font-weight:700;color:var(--green)">3</td>
              <td style="text-align:right;font-weight:700;color:var(--muted)">1.434</td>
              <td style="text-align:right;font-weight:700;color:var(--muted)">478</td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="ibox" style="background:rgba(255,127,42,.08);border:1px solid rgba(255,127,42,.2)">
        <span style="font-size:18px">🌟</span>
        <div><strong>Capivari de Baixo: ticket de R$ 9.525</strong> com apenas 4 efetivados — o maior ticket entre municípios com +1 venda. Alto potencial.</div>
      </div>
    </div>
  </div>

  <div class="sec">Bairros em Tubarão &amp; Tempo de Fechamento</div>
  <div class="g2">
    <div class="card">
      <div class="ctitle"><span class="cdot" style="background:var(--accent4); color:var(--accent4)"></span>Top 10 Bairros — Tubarão (por volume)</div>
      <div class="brow"><div class="brank">1</div><div class="bname">Vila Moema</div><div class="bbar-w"><div class="bbar" style="width:100%"></div></div><div class="bcnt">7</div></div>
      <div class="brow"><div class="brank">1</div><div class="bname">Passagem</div><div class="bbar-w"><div class="bbar" style="width:100%"></div></div><div class="bcnt">7</div></div>
      <div class="brow"><div class="brank">3</div><div class="bname">São Martinho</div><div class="bbar-w"><div class="bbar" style="width:85%"></div></div><div class="bcnt">6</div></div>
      <div class="brow"><div class="brank">3</div><div class="bname">Centro</div><div class="bbar-w"><div class="bbar" style="width:85%"></div></div><div class="bcnt">6</div></div>
      <div class="brow"><div class="brank">5</div><div class="bname">Oficinas</div><div class="bbar-w"><div class="bbar" style="width:71%"></div></div><div class="bcnt">5</div></div>
      <div class="brow"><div class="brank">5</div><div class="bname">Humaitá de Cima</div><div class="bbar-w"><div class="bbar" style="width:71%"></div></div><div class="bcnt">5</div></div>
      <div class="brow"><div class="brank">5</div><div class="bname">Dehon</div><div class="bbar-w"><div class="bbar" style="width:71%"></div></div><div class="bcnt">5</div></div>
      <div class="brow"><div class="brank">8</div><div class="bname">Vila Esperança</div><div class="bbar-w"><div class="bbar" style="width:57%"></div></div><div class="bcnt">4</div></div>
      <div class="brow"><div class="brank">9</div><div class="bname">Sto. Antônio de Pádua</div><div class="bbar-w"><div class="bbar" style="width:43%"></div></div><div class="bcnt">3</div></div>
      <div class="brow"><div class="brank">9</div><div class="bname">Morrotes</div><div class="bbar-w"><div class="bbar" style="width:43%"></div></div><div class="bcnt">3</div></div>
      <div class="ibox" style="background:rgba(180,151,250,.08);border:1px solid rgba(180,151,250,.2)">
        <span style="font-size:18px">📍</span>
        <div>Os top 4 bairros (Vila Moema, Passagem, São Martinho, Centro) concentram 26 leads — <strong>32,5% do total da cidade</strong>.</div>
      </div>
    </div>

    <div class="card">
      <div class="ctitle"><span class="cdot" style="background:var(--accent); color:var(--accent)"></span>Tempo de Fechamento (Efetivados)</div>
      <div style="display:flex;align-items:baseline;gap:12px;margin-bottom:24px">
        <div style="font-family:'Syne',sans-serif;font-size:64px;font-weight:800;color:var(--accent);line-height:1; letter-spacing:-2px; text-shadow: 0 0 20px rgba(0,229,181,0.4);">2</div>
        <div>
          <div style="font-size:24px;font-weight:700;color:var(--text)">dias</div>
          <div style="font-size:12px;color:var(--muted);margin-top:4px;">mediana · 43 casos</div>
        </div>
      </div>
      <div class="val-row"><div class="val-lbl">Média aritmética</div><div class="val-num">25 dias</div></div>
      <div class="val-row"><div class="val-lbl">Fechamento no mesmo dia</div><div><span class="pill pill-g">11 casos</span></div></div>
      <div class="val-row"><div class="val-lbl">Até 7 dias (ciclo rápido)</div><div><span class="pill pill-g">34 de 43 (79%)</span></div></div>
      <div class="val-row"><div class="val-lbl">Mais de 30 dias</div><div><span class="pill pill-r">5 casos</span></div></div>
      <div class="val-row"><div class="val-lbl">Caso mais longo</div><div class="val-num">518 dias</div></div>
      <div class="ibox" style="background:rgba(0,229,181,.08);border:1px solid rgba(0,229,181,.2)">
        <span style="font-size:18px">⚡</span>
        <div>Ciclo de vendas extremamente curto. <strong>79% fecha em ≤ 7 dias</strong>. A média de 25 dias é puxada por outliers — a unidade fecha rápido.</div>
      </div>
    </div>
  </div>
</div>

<!-- ==========================================
     VIEW: RESGATES
=========================================== -->
<div id="resgates-view" class="view">
  <div class="header">
    <div style="display:flex;align-items:center;gap:18px">
      <div class="logo resgates">↩</div>
      <div>
        <div class="htitle">Resgates — Sorrifácil Tubarão</div>
        <div class="hsub">Análise de tratamentos resgatados · Todos os registros são do tipo Resgate</div>
      </div>
    </div>
    <div class="hbadge">292 registros &nbsp;·&nbsp; Faturados em <strong style="color:var(--orange)">Mai/2026</strong></div>
  </div>

  <div class="sec">Resumo Geral</div>
  <div class="kpi-strip">
    <div class="kpi" style="--kc:var(--orange)">
      <div class="kpi-lbl">Total Resgates</div>
      <div class="kpi-val">292</div>
      <div class="kpi-sub">Todos faturados em Mai/2026</div>
    </div>
    <div class="kpi" style="--kc:var(--accent)">
      <div class="kpi-lbl">Receita Total</div>
      <div class="kpi-val" style="color:var(--accent);">R$ 119.743</div>
      <div class="kpi-sub">Valor efetivamente faturado</div>
    </div>
    <div class="kpi" style="--kc:var(--accent4)">
      <div class="kpi-lbl">Ticket Médio</div>
      <div class="kpi-val" style="color:var(--accent4);">R$ 414</div>
      <div class="kpi-sub">Por procedimento resgatado</div>
    </div>
    <div class="kpi" style="--kc:var(--accent3)">
      <div class="kpi-lbl">Tempo Médio</div>
      <div class="kpi-val" style="color:var(--accent3)">607 dias</div>
      <div class="kpi-sub">Avaliação → Faturamento</div>
    </div>
    <div class="kpi" style="--kc:var(--red)">
      <div class="kpi-lbl">Mediana Tempo</div>
      <div class="kpi-val" style="color:var(--red)">606 dias</div>
      <div class="kpi-sub">~20 meses de espera típica</div>
    </div>
  </div>

  <div class="sec">Extremos — Mais Antigo e Mais Recente</div>
  <div class="g2">
    <div class="card" style="border-color: rgba(244,63,94,0.3); box-shadow: 0 8px 32px rgba(244,63,94,0.05);">
      <div class="ctitle"><span class="cdot" style="background:var(--red); box-shadow: 0 0 10px var(--red)"></span>Caso Mais Antigo</div>
      <div class="extremo-card" style="border:1px solid rgba(244,63,94,0.2)">
        <div class="extremo-tag" style="color:var(--red)">⏳ Avaliado há mais tempo</div>
        <div class="extremo-val" style="color:var(--red); text-shadow: 0 0 16px rgba(244,63,94,0.4)">1.981 dias</div>
        <div style="font-size:12px;color:var(--muted);margin-bottom:16px">≈ 5 anos e 5 meses esperando</div>
        <div class="extremo-row"><span class="extremo-key">Especialidade</span><span class="extremo-v">Implante</span></div>
        <div class="extremo-row"><span class="extremo-key">Data da Avaliação</span><span class="extremo-v">18/12/2020</span></div>
        <div class="extremo-row"><span class="extremo-key">Data do Faturamento</span><span class="extremo-v">22/05/2026</span></div>
        <div class="extremo-row"><span class="extremo-key">Valor Faturado</span><span class="extremo-v" style="color:var(--accent)">R$ 500,00</span></div>
      </div>
      <div class="ibox" style="background:rgba(244,63,94,.08);border:1px solid rgba(244,63,94,.2)">
        <span style="font-size:18px">🚨</span>
        <div>Paciente de <strong>implante avaliado em dezembro de 2020</strong> e faturado apenas em maio de 2026. Mais de 5 anos no pipeline.</div>
      </div>
    </div>

    <div class="card" style="border-color: rgba(27,214,101,0.3); box-shadow: 0 8px 32px rgba(27,214,101,0.05);">
      <div class="ctitle"><span class="cdot" style="background:var(--green); box-shadow: 0 0 10px var(--green)"></span>Caso Mais Recente</div>
      <div class="extremo-card" style="border:1px solid rgba(27,214,101,0.2)">
        <div class="extremo-tag" style="color:var(--green)">⚡ Avaliação mais recente resgatada</div>
        <div class="extremo-val" style="color:var(--green); text-shadow: 0 0 16px rgba(27,214,101,0.4)">10 dias</div>
        <div style="font-size:12px;color:var(--muted);margin-bottom:16px">Avaliado e faturado quase imediatamente</div>
        <div class="extremo-row"><span class="extremo-key">Especialidade</span><span class="extremo-v">Implante</span></div>
        <div class="extremo-row"><span class="extremo-key">Data da Avaliação</span><span class="extremo-v">28/04/2026</span></div>
        <div class="extremo-row"><span class="extremo-key">Data do Faturamento</span><span class="extremo-v">08/05/2026</span></div>
        <div class="extremo-row"><span class="extremo-key">Valor Faturado</span><span class="extremo-v" style="color:var(--accent)">R$ 2.940,00</span></div>
      </div>
      <div class="ibox" style="background:rgba(27,214,101,.08);border:1px solid rgba(27,214,101,.2)">
        <span style="font-size:18px">✅</span>
        <div>Avaliação de <strong>implante em abril/2026</strong> convertida em 10 dias — ciclo rápido, provavelmente paciente que já tinha decidido.</div>
      </div>
    </div>
  </div>

  <div class="sec">Distribuição por Tempo de Resgate</div>
  <div class="g35">
    <div class="card">
      <div class="ctitle"><span class="cdot" style="background:var(--accent3)"></span>Faixas de Tempo</div>
      <div class="brow"><div class="brank" style="color:var(--green); width:70px">≤ 30 dias</div><div class="faixa-bar-w" style="flex:1"><div class="faixa-bar" style="width:49.6%;background:linear-gradient(90deg,#059669,#1bd665)">61</div></div><div class="bcnt" style="color:var(--green); width:30px">61</div><div style="font-size:11px;color:var(--muted);width:40px;text-align:right;">20,9%</div></div>
      <div class="brow"><div class="brank" style="color:#34d399; width:70px">31 – 90 d.</div><div class="faixa-bar-w" style="flex:1"><div class="faixa-bar" style="width:37.4%;background:linear-gradient(90deg,#047857,#34d399)">46</div></div><div class="bcnt" style="color:#34d399; width:30px">46</div><div style="font-size:11px;color:var(--muted);width:40px;text-align:right;">15,8%</div></div>
      <div class="brow"><div class="brank" style="color:var(--accent3); width:70px">91 – 180 d.</div><div class="faixa-bar-w" style="flex:1"><div class="faixa-bar" style="width:2.4%;background:linear-gradient(90deg,#b45309,#fbbf24);min-width:32px">3</div></div><div class="bcnt" style="color:var(--accent3); width:30px">3</div><div style="font-size:11px;color:var(--muted);width:40px;text-align:right;">1,0%</div></div>
      <div class="brow"><div class="brank" style="color:var(--orange); width:70px">6m – 1 a.</div><div class="faixa-bar-w" style="flex:1"><div class="faixa-bar" style="width:12.2%;background:linear-gradient(90deg,#9a3412,#ff7f2a)">15</div></div><div class="bcnt" style="color:var(--orange); width:30px">15</div><div style="font-size:11px;color:var(--muted);width:40px;text-align:right;">5,1%</div></div>
      <div class="brow"><div class="brank" style="color:var(--red); width:70px">1 – 2 a.</div><div class="faixa-bar-w" style="flex:1"><div class="faixa-bar" style="width:35.8%;background:linear-gradient(90deg,#be123c,#f43f5e)">44</div></div><div class="bcnt" style="color:var(--red); width:30px">44</div><div style="font-size:11px;color:var(--muted);width:40px;text-align:right;">15,1%</div></div>
      <div class="brow"><div class="brank" style="color:#fb7185; width:70px">+ 2 anos</div><div class="faixa-bar-w" style="flex:1"><div class="faixa-bar" style="width:100%;background:linear-gradient(90deg,#9f1239,#fb7185)">123</div></div><div class="bcnt" style="color:#fb7185; width:30px">123</div><div style="font-size:11px;color:var(--muted);width:40px;text-align:right;">42,1%</div></div>
      
      <div class="ibox" style="background:rgba(244,63,94,.08);border:1px solid rgba(244,63,94,.2)">
        <span style="font-size:18px">🔴</span>
        <div><strong style="color:var(--text)">42,1% dos resgates vieram de avaliações com +2 anos</strong> — 123 procedimentos dormentes por mais de 730 dias antes de fechar.</div>
      </div>
    </div>

    <div class="card">
      <div class="ctitle"><span class="cdot" style="background:var(--accent4)"></span>Ano da Avaliação Original × Receita</div>
      <div style="height:240px; width: 100%;"><canvas id="anoChart"></canvas></div>
      <div style="margin-top:16px;display:grid;grid-template-columns:1fr 1fr;gap:12px">
        <div style="background:rgba(0,0,0,0.2);border-radius:12px;padding:14px; border:1px solid rgba(255,255,255,0.05)">
          <div style="font-size:11px;font-weight:700;color:var(--muted);margin-bottom:6px">AVALIAÇÕES 2026 (atual)</div>
          <div style="font-family:'Syne',sans-serif;font-size:20px;font-weight:800;color:var(--accent)">R$ 75.377</div>
          <div style="font-size:12px;color:var(--muted)">107 procedimentos · ticket R$ 704</div>
        </div>
        <div style="background:rgba(0,0,0,0.2);border-radius:12px;padding:14px; border:1px solid rgba(255,255,255,0.05)">
          <div style="font-size:11px;font-weight:700;color:var(--muted);margin-bottom:6px">AVALIAÇÕES ANTERIORES</div>
          <div style="font-family:'Syne',sans-serif;font-size:20px;font-weight:800;color:var(--red)">R$ 44.366</div>
          <div style="font-size:12px;color:var(--muted)">185 procedimentos · ticket R$ 240</div>
        </div>
      </div>
    </div>
  </div>

  <div class="sec">Análise por Especialidade</div>
  <div class="card">
    <div class="ctitle"><span class="cdot" style="background:var(--accent)"></span>Performance por Especialidade</div>
    <div class="table-wrapper">
    <table>
      <thead>
        <tr>
          <th style="text-align:left;padding-left:0;min-width:160px">Especialidade</th>
          <th style="text-align:right;">Qtd</th>
          <th style="text-align:right;">% Total</th>
          <th style="text-align:right;">Receita (R$)</th>
          <th style="text-align:right;">% Receita</th>
          <th style="text-align:right;">Ticket Médio</th>
          <th style="text-align:right;">Ticket Mín</th>
          <th style="text-align:right;">Ticket Máx</th>
          <th style="text-align:right;">Tempo Médio</th>
          <th style="text-align:right;">Tempo Mediana</th>
          <th style="text-align:right;">Avaliação Antiga</th>
          <th style="text-align:right;">Avaliação Recente</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td style="text-align:left; padding-left:0; font-weight: 600;"><span class="esp-dot" style="background:var(--c-cg); margin-right:8px;"></span>Clínico Geral</td>
          <td style="text-align:right; font-weight:700">139</td>
          <td style="text-align:right;"><span class="pill pill-b">47,6%</span></td>
          <td style="text-align:right; font-family:'Syne',sans-serif;font-weight:800;color:var(--c-cg);font-size:14px;">20.104</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">16,8%</td>
          <td style="text-align:right; font-weight:700">R$ 147</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">R$ 4</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">R$ 630</td>
          <td style="text-align:right;"><span class="pill pill-r">656 dias</span></td>
          <td style="text-align:right; color:var(--muted);font-size:12px">700 dias</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">31/05/2021</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">27/04/2026</td>
        </tr>
        <tr>
          <td style="text-align:left; padding-left:0; font-weight: 600;"><span class="esp-dot" style="background:var(--c-orto); margin-right:8px;"></span>Ortodontia</td>
          <td style="text-align:right; font-weight:700">44</td>
          <td style="text-align:right;"><span class="pill pill-p">15,1%</span></td>
          <td style="text-align:right; font-family:'Syne',sans-serif;font-weight:800;color:var(--c-orto);font-size:14px;">14.129</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">11,8%</td>
          <td style="text-align:right; font-weight:700">R$ 321</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">R$ 6</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">R$ 1.616</td>
          <td style="text-align:right;"><span class="pill pill-r">520 dias</span></td>
          <td style="text-align:right; color:var(--muted);font-size:12px">354 dias</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">09/05/2022</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">24/04/2026</td>
        </tr>
        <tr>
          <td style="text-align:left; padding-left:0; font-weight: 600;"><span class="esp-dot" style="background:var(--c-prot); margin-right:8px;"></span>Prótese</td>
          <td style="text-align:right; font-weight:700">33</td>
          <td style="text-align:right;"><span class="pill" style="background:rgba(244,114,182,.15);color:var(--c-prot);border:1px solid rgba(244,114,182,.3)">11,3%</span></td>
          <td style="text-align:right; font-family:'Syne',sans-serif;font-weight:800;color:var(--c-prot);font-size:14px;">14.099</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">11,8%</td>
          <td style="text-align:right; font-weight:700">R$ 427</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">R$ 45</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">R$ 2.200</td>
          <td style="text-align:right;"><span class="pill pill-r">865 dias</span></td>
          <td style="text-align:right; color:var(--muted);font-size:12px">1.148 dias</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">27/01/2021</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">22/04/2026</td>
        </tr>
        <tr>
          <td style="text-align:left; padding-left:0; font-weight: 600;"><span class="esp-dot" style="background:var(--c-impl); margin-right:8px;"></span>Implante</td>
          <td style="text-align:right; font-weight:700">26</td>
          <td style="text-align:right;"><span class="pill pill-g">8,9%</span></td>
          <td style="text-align:right; font-family:'Syne',sans-serif;font-weight:800;color:var(--c-impl);font-size:16px;">55.910</td>
          <td style="text-align:right;"><span class="pill pill-g">46,7%</span></td>
          <td style="text-align:right; font-family:'Syne',sans-serif;font-weight:800;color:var(--c-impl)">R$ 2.150</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">R$ 49</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">R$ 9.692</td>
          <td style="text-align:right;"><span class="pill pill-g">273 dias</span></td>
          <td style="text-align:right; color:var(--muted);font-size:12px">40 dias</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">18/12/2020</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">28/04/2026</td>
        </tr>
        <tr>
          <td style="text-align:left; padding-left:0; font-weight: 600;"><span class="esp-dot" style="background:var(--c-endo); margin-right:8px;"></span>Endodontia</td>
          <td style="text-align:right; font-weight:700">22</td>
          <td style="text-align:right;"><span class="pill pill-y">7,5%</span></td>
          <td style="text-align:right; font-family:'Syne',sans-serif;font-weight:800;color:var(--c-endo);font-size:14px;">10.330</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">8,6%</td>
          <td style="text-align:right; font-weight:700">R$ 469</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">R$ 12</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">R$ 1.500</td>
          <td style="text-align:right;"><span class="pill pill-r">680 dias</span></td>
          <td style="text-align:right; color:var(--muted);font-size:12px">610 dias</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">06/12/2021</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">22/04/2026</td>
        </tr>
        <tr>
          <td style="text-align:left; padding-left:0; font-weight: 600;"><span class="esp-dot" style="background:var(--c-comp); margin-right:8px;"></span>Complementares</td>
          <td style="text-align:right; font-weight:700">16</td>
          <td style="text-align:right;"><span class="pill pill-o">5,5%</span></td>
          <td style="text-align:right; font-family:'Syne',sans-serif;font-weight:800;color:var(--c-comp);font-size:14px;">1.619</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">1,4%</td>
          <td style="text-align:right; font-weight:700">R$ 101</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">R$ 4</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">R$ 242</td>
          <td style="text-align:right;"><span class="pill pill-y">344 dias</span></td>
          <td style="text-align:right; color:var(--muted);font-size:12px">60 dias</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">08/08/2022</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">28/04/2026</td>
        </tr>
        <tr>
          <td style="text-align:left; padding-left:0; font-weight: 600;"><span class="esp-dot" style="background:var(--c-cir); margin-right:8px;"></span>Cirurgia</td>
          <td style="text-align:right; font-weight:700">6</td>
          <td style="text-align:right;"><span class="pill pill-r">2,1%</span></td>
          <td style="text-align:right; font-family:'Syne',sans-serif;font-weight:800;color:var(--c-cir);font-size:14px;">2.050</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">1,7%</td>
          <td style="text-align:right; font-weight:700">R$ 410</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">R$ 22</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">R$ 678</td>
          <td style="text-align:right;"><span class="pill pill-r">937 dias</span></td>
          <td style="text-align:right; color:var(--muted);font-size:12px">913 dias</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">12/06/2021</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">20/10/2025</td>
        </tr>
        <tr>
          <td style="text-align:left; padding-left:0; font-weight: 600;"><span class="esp-dot" style="background:var(--c-ped); margin-right:8px;"></span>Odonto Pediatria</td>
          <td style="text-align:right; font-weight:700">5</td>
          <td style="text-align:right;"><span class="pill pill-g">1,7%</span></td>
          <td style="text-align:right; font-family:'Syne',sans-serif;font-weight:800;color:var(--c-ped);font-size:14px;">700</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">0,6%</td>
          <td style="text-align:right; font-weight:700">R$ 140</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">R$ 50</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">R$ 200</td>
          <td style="text-align:right;"><span class="pill pill-y">243 dias</span></td>
          <td style="text-align:right; color:var(--muted);font-size:12px">222 dias</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">19/08/2025</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">03/10/2025</td>
        </tr>
        <tr>
          <td style="text-align:left; padding-left:0; font-weight: 600;"><span class="esp-dot" style="background:var(--c-perio); margin-right:8px;"></span>Periodontia</td>
          <td style="text-align:right; font-weight:700">1</td>
          <td style="text-align:right;"><span class="pill" style="background:rgba(232,121,249,.15);color:var(--c-perio);border:1px solid rgba(232,121,249,.3)">0,3%</span></td>
          <td style="text-align:right; font-family:'Syne',sans-serif;font-weight:800;color:var(--c-perio);font-size:14px;">800</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">0,7%</td>
          <td style="text-align:right; font-weight:700">R$ 800</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">R$ 800</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">R$ 800</td>
          <td style="text-align:right;"><span class="pill pill-y">235 dias</span></td>
          <td style="text-align:right; color:var(--muted);font-size:12px">235 dias</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">19/09/2025</td>
          <td style="text-align:right; color:var(--muted);font-size:12px">19/09/2025</td>
        </tr>
      </tbody>
      <tfoot>
        <tr class="row-total">
          <td style="text-align:left; padding-left:0;">TOTAL GERAL</td>
          <td style="text-align:right;">292</td>
          <td style="text-align:right;">100%</td>
          <td style="text-align:right; color:var(--accent); font-size:16px;">119.743</td>
          <td style="text-align:right;">100%</td>
          <td style="text-align:right; color:var(--accent); font-size:16px;">R$ 414</td>
          <td style="text-align:right; color:var(--muted); font-size:13px;">—</td>
          <td style="text-align:right; color:var(--muted); font-size:13px;">—</td>
          <td style="text-align:right; color:var(--accent3); font-size:14px;">607 dias</td>
          <td style="text-align:right; color:var(--accent3); font-size:14px;">606 dias</td>
          <td style="text-align:right; color:var(--muted); font-size:13px;">18/12/2020</td>
          <td style="text-align:right; color:var(--muted); font-size:13px;">28/04/2026</td>
        </tr>
      </tfoot>
    </table>
    </div>
  </div>

  <div class="sec">Visão Gráfica por Especialidade</div>
  <div class="g3">
    <div class="card">
      <div class="ctitle"><span class="cdot" style="background:var(--blue)"></span>% de Procedimentos</div>
      <div style="height:220px; width:100%"><canvas id="qtdChart"></canvas></div>
    </div>
    <div class="card">
      <div class="ctitle"><span class="cdot" style="background:var(--accent)"></span>% da Receita</div>
      <div style="height:220px; width:100%"><canvas id="receitaChart"></canvas></div>
      <div class="ibox" style="background:rgba(0,229,181,.08);border:1px solid rgba(0,229,181,.2)">
        <span style="font-size:18px">💰</span>
        <div><strong>Implante: 8,9% dos procedimentos → 46,7% da receita.</strong> Alta concentração de valor.</div>
      </div>
    </div>
    <div class="card">
      <div class="ctitle"><span class="cdot" style="background:var(--accent3)"></span>Tempo Médio por Especialidade (dias)</div>
      <div style="height:220px; width:100%"><canvas id="tempoChart"></canvas></div>
      <div class="ibox" style="background:rgba(244,63,94,.08);border:1px solid rgba(244,63,94,.2)">
        <span style="font-size:18px">⏰</span>
        <div><strong>Cirurgia (937d) e Prótese (865d)</strong> têm o maior tempo médio antes de resgatar.</div>
      </div>
    </div>
  </div>

  <div class="sec">Insights Estratégicos</div>
  <div class="g4">
    <div class="card" style="border-color:rgba(0,229,181,.3); box-shadow: 0 4px 20px rgba(0,229,181,0.05)">
      <div style="font-size:24px;margin-bottom:12px; text-shadow: 0 0 10px rgba(0,229,181,0.4)">🏆</div>
      <div style="font-family:'Syne',sans-serif;font-size:14px;font-weight:800;margin-bottom:8px">Implante domina receita</div>
      <div style="font-size:13px;color:var(--muted);line-height:1.6">8,9% dos resgates mas <strong style="color:var(--text)">46,7% da receita</strong>. Ticket médio de R$ 2.150 — 5× maior que a média geral. Priorizar resgate de implantes é estrategicamente correto.</div>
    </div>
    <div class="card" style="border-color:rgba(244,63,94,.3); box-shadow: 0 4px 20px rgba(244,63,94,0.05)">
      <div style="font-size:24px;margin-bottom:12px; text-shadow: 0 0 10px rgba(244,63,94,0.4)">🕰</div>
      <div style="font-family:'Syne',sans-serif;font-size:14px;font-weight:800;margin-bottom:8px">42% com +2 anos dormentes</div>
      <div style="font-size:13px;color:var(--muted);line-height:1.6">123 procedimentos esperaram mais de <strong style="color:var(--text)">2 anos</strong> para fechar. Clínico Geral (65), Prótese (17) e Orto (18) concentram o backlog.</div>
    </div>
    <div class="card" style="border-color:rgba(255,207,84,.3); box-shadow: 0 4px 20px rgba(255,207,84,0.05)">
      <div style="font-size:24px;margin-bottom:12px; text-shadow: 0 0 10px rgba(255,207,84,0.4)">⚡</div>
      <div style="font-family:'Syne',sans-serif;font-size:14px;font-weight:800;margin-bottom:8px">36,7% fecham em ≤ 90 dias</div>
      <div style="font-size:13px;color:var(--muted);line-height:1.6"><strong style="color:var(--text)">107 resgates</strong> ocorreram com menos de 3 meses entre avaliação e faturamento — indicando que parte é quase conversão natural.</div>
    </div>
    <div class="card" style="border-color:rgba(91,157,255,.3); box-shadow: 0 4px 20px rgba(91,157,255,0.05)">
      <div style="font-size:24px;margin-bottom:12px; text-shadow: 0 0 10px rgba(91,157,255,0.4)">📈</div>
      <div style="font-family:'Syne',sans-serif;font-size:14px;font-weight:800;margin-bottom:8px">Avaliações 2026 têm 3× mais ticket</div>
      <div style="font-size:13px;color:var(--muted);line-height:1.6">Ticket médio de avaliações em <strong style="color:var(--text)">2026: R$ 704</strong> vs. R$ 240 das avaliações anteriores. Perfil de procedimento mais complexo recentemente.</div>
    </div>
  </div>
</div>

<script>
// Lógica das Abas (Tabs)
function switchTab(tabId) {
  // Esconde todas views
  document.querySelectorAll('.view').forEach(el => el.classList.remove('active'));
  // Remove ativo dos botões
  document.querySelectorAll('.tab-btn').forEach(el => el.classList.remove('active'));
  
  // Mostra a selecionada
  document.getElementById(tabId + '-view').classList.add('active');
  document.querySelector(`[onclick="switchTab('${tabId}')"]`).classList.add('active');
  
  // Rola para o topo de forma suave ao trocar
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

// Inicialização dos Gráficos (Chart.js)
const COLORS = {
  cg:   '#5b9dff',
  orto: '#b497fa',
  prot: '#f472b6',
  impl: '#00e5b5',
  endo: '#ffcf54',
  comp: '#ff7f2a',
  cir:  '#f43f5e',
  ped:  '#1bd665',
  perio:'#e879f9'
};
const LABELS = ['Clínico Geral','Ortodontia','Prótese','Implante','Endodontia','Complementares','Cirurgia','Odonto Ped.','Periodontia'];
const COLORS_ARR = Object.values(COLORS);

const tooltip_cfg = {
  backgroundColor: 'rgba(18, 25, 33, 0.95)',
  titleColor: '#f8fafc',
  bodyColor: '#94a3b8',
  borderColor: 'rgba(255,255,255,0.1)',
  borderWidth: 1,
  padding: 12,
  titleFont: { family: 'Syne', size: 14, weight: 'bold' },
  bodyFont: { family: 'DM Sans', size: 13 },
  boxPadding: 6
};

// Plugin de Legenda Customizada
const chartOptions = {
  responsive: true, maintainAspectRatio: false, cutout: '65%',
  plugins: {
    legend: { position: 'right', labels: { color: '#94a3b8', font: { size: 11, family: 'DM Sans' }, padding: 12, usePointStyle: true, pointStyle: 'circle' } },
    tooltip: { ...tooltip_cfg }
  },
  layout: { padding: 10 }
};

// 1. DONUT - Qtd procedimentos
new Chart(document.getElementById('qtdChart'), {
  type: 'doughnut',
  data: {
    labels: LABELS,
    datasets: [{
      data: [139,44,33,26,22,16,6,5,1],
      backgroundColor: COLORS_ARR,
      borderColor: '#050507',
      borderWidth: 3,
      hoverOffset: 6
    }]
  },
  options: chartOptions
});

// 2. DONUT - Receita
new Chart(document.getElementById('receitaChart'), {
  type: 'doughnut',
  data: {
    labels: LABELS,
    datasets: [{
      data: [20104,14129,14099,55910,10330,1619,2050,700,800],
      backgroundColor: COLORS_ARR,
      borderColor: '#050507',
      borderWidth: 3,
      hoverOffset: 6
    }]
  },
  options: {
    ...chartOptions,
    plugins: {
      ...chartOptions.plugins,
      tooltip: {
        ...tooltip_cfg,
        callbacks: {
          label: (ctx) => ` R$ ${ctx.raw.toLocaleString('pt-BR',{minimumFractionDigits:0})}`
        }
      }
    }
  }
});

// 3. BAR - Tempo médio
new Chart(document.getElementById('tempoChart'), {
  type: 'bar',
  data: {
    labels: ['Cirurgia','Prótese','Endodontia','Clín. Geral','Orto','Complementares','Periodontia','Odonto Ped.','Implante'],
    datasets: [{
      label: 'Tempo Médio (dias)',
      data: [937,865,680,656,520,344,235,243,273],
      backgroundColor: [
        'rgba(244,63,94,0.9)', 'rgba(244,63,94,0.7)', 'rgba(244,63,94,0.5)',
        'rgba(244,63,94,0.4)', 'rgba(255,207,84,0.7)', 'rgba(255,207,84,0.5)',
        'rgba(27,214,101,0.5)', 'rgba(27,214,101,0.7)', 'rgba(0,229,181,0.9)'
      ],
      borderRadius: 6,
      borderWidth: 1,
      borderColor: 'rgba(255,255,255,0.1)'
    }]
  },
  options: {
    indexAxis: 'y',
    responsive: true, maintainAspectRatio: false,
    plugins: {
      legend: { display: false },
      tooltip: { ...tooltip_cfg, callbacks: { label: (ctx)=>` ${ctx.raw} dias` } }
    },
    scales: {
      x: { grid: { color: 'rgba(255,255,255,0.03)' }, ticks: { color: '#94a3b8', font: { size: 11 } } },
      y: { grid: { display: false }, ticks: { color: '#94a3b8', font: { size: 11 } } }
    }
  }
});

// 4. BAR - Ano avaliação
new Chart(document.getElementById('anoChart'), {
  type: 'bar',
  data: {
    labels: ['2020','2021','2022','2023','2024','2025','2026'],
    datasets: [
      {
        label: 'Receita (R$)',
        data: [500,2440,7508,15537,11739,6640,75377],
        backgroundColor: [
          'rgba(244,63,94,0.4)','rgba(244,63,94,0.5)','rgba(244,63,94,0.7)',
          'rgba(255,207,84,0.5)','rgba(255,207,84,0.8)','rgba(91,157,255,0.7)',
          'rgba(0,229,181,0.9)'
        ],
        borderRadius: 6,
        yAxisID: 'y'
      },
      {
        label: 'Qtd',
        data: [1,9,28,71,45,28,107],
        type: 'line',
        borderColor: '#b497fa',
        backgroundColor: 'rgba(180,151,250,0.15)',
        borderWidth: 3,
        pointRadius: 5,
        pointBackgroundColor: '#b497fa',
        pointBorderColor: '#050507',
        pointBorderWidth: 2,
        tension: 0.4,
        yAxisID: 'y1'
      }
    ]
  },
  options: {
    responsive: true, maintainAspectRatio: false,
    plugins: {
      legend: { labels: { color: '#94a3b8', font: { size: 11, family: 'DM Sans' }, usePointStyle: true, padding: 20 } },
      tooltip: { ...tooltip_cfg }
    },
    scales: {
      x: { grid: { color: 'rgba(255,255,255,0.03)' }, ticks: { color: '#94a3b8', font: { size: 11 } } },
      y: { grid: { color: 'rgba(255,255,255,0.03)' }, ticks: { color: '#94a3b8', font: { size: 11 } }, position: 'left' },
      y1: { grid: { display: false }, ticks: { color: '#b497fa', font: { size: 11 } }, position: 'right' }
    }
  }
});
</script>
</body>
</html>
