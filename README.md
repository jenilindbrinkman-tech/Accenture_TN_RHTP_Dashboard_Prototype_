<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>RHTF Performance Dashboard — Tennessee Department of Health</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Source+Sans+3:wght@300;400;500;600;700&display=swap');

*{box-sizing:border-box;margin:0;padding:0}
body{font-family:'Source Sans 3',Arial,sans-serif;background:#EEF2F7;color:#1C2E4A;font-size:14px}

/* ── TOP HEADER ── */
.top-bar{background:#1C2E4A;padding:0 28px;display:flex;align-items:center;justify-content:space-between;height:52px}
.top-bar-left{display:flex;align-items:center;gap:14px}
.accenture-mark{font-size:13px;font-weight:600;color:#fff;letter-spacing:0.02em}
.accenture-mark span{color:#A100FF}
.divider-v{width:1px;height:22px;background:#2A3E5A}
.top-title{font-size:13px;font-weight:500;color:#7BA8C8}
.top-bar-right{display:flex;align-items:center;gap:20px}
.last-updated{font-size:10px;color:#4A6A88}
.source-badge{background:#162438;border:1px solid #2196E0;border-radius:3px;padding:3px 10px;font-size:10px;color:#2196E0;font-weight:600}

/* ── PAGE TITLE BAND ── */
.title-band{background:#1C2E4A;padding:16px 28px 0;border-top:1px solid #243A55}
.title-band h1{font-size:22px;font-weight:700;color:#fff;letter-spacing:-0.02em;margin-bottom:2px}
.title-band p{font-size:11px;color:#7BA8C8;margin-bottom:12px}
.title-rule{height:3px;background:#2196E0;width:48px;margin-bottom:0}

/* ── SUMMARY STATS ── */
.summary-row{background:#162438;border-top:1px solid #243A55;display:grid;grid-template-columns:repeat(6,1fr);padding:0 28px}
.sstat{padding:10px 0 10px 18px;border-right:1px solid #243A55}
.sstat:last-child{border-right:none}
.sstat-n{font-size:20px;font-weight:700;color:#2196E0;line-height:1}
.sstat-l{font-size:9px;color:#6A98B8;margin-top:3px;line-height:1.3}

/* ── TAB NAV ── */
.tab-nav{background:#1C2E4A;display:flex;padding:0 28px;gap:2px;border-top:1px solid #243A55}
.tab-btn{padding:10px 18px;font-size:11px;font-weight:600;color:#7BA8C8;border:none;background:transparent;cursor:pointer;border-bottom:3px solid transparent;letter-spacing:0.03em;text-transform:uppercase;white-space:nowrap;transition:color 0.15s}
.tab-btn:hover{color:#B8D8F0}
.tab-btn.active{color:#fff;border-bottom-color:#2196E0}

/* ── MAIN CONTENT ── */
.main{padding:20px 28px;display:none}
.main.active{display:block}

/* ── FOCUS AREA HEADER ── */
.focus-header{display:flex;align-items:flex-start;justify-content:space-between;margin-bottom:16px}
.focus-title{font-size:16px;font-weight:700;color:#1C2E4A}
.focus-desc{font-size:11px;color:#4A6A88;margin-top:2px;max-width:600px}
.focus-period{font-size:10px;color:#2196E0;font-weight:600;background:#E8F4FD;padding:4px 10px;border-radius:3px;border:1px solid #B8D8F0;white-space:nowrap}

/* ── INITIATIVE CARDS ── */
.init-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:12px}
.init-grid.two-col{grid-template-columns:repeat(2,1fr)}
.init-grid.one-col{grid-template-columns:1fr}

.icard{background:#fff;border:1px solid #D8E6F2;border-radius:4px;overflow:hidden}
.icard-head{background:#1C2E4A;padding:9px 12px;border-left:3px solid #2196E0;display:flex;align-items:flex-start;justify-content:space-between}
.icard-num{font-size:9px;font-weight:700;color:#2196E0;letter-spacing:0.06em;text-transform:uppercase;margin-bottom:2px}
.icard-name{font-size:11px;font-weight:600;color:#fff;line-height:1.35}
.stage-pill{background:#243A55;border:1px solid #2A4A6A;border-radius:10px;padding:2px 7px;font-size:9px;color:#7BA8C8;white-space:nowrap;flex-shrink:0;margin-left:8px;margin-top:1px}
.stage-pill.on-track{background:#0D3D2A;border-color:#1D9E75;color:#5DCAA5}
.stage-pill.pending{background:#3A2A10;border-color:#BA7517;color:#EF9F27}
.stage-pill.planning{background:#243A55;border-color:#2196E0;color:#7BA8C8}

.icard-body{padding:10px 12px}

/* KPI rows */
.kpi-row{display:flex;align-items:center;margin-bottom:8px;gap:8px}
.kpi-row:last-child{margin-bottom:0}
.kpi-info{flex:1;min-width:0}
.kpi-label{font-size:10px;color:#1C2E4A;font-weight:500;line-height:1.3;margin-bottom:2px}
.kpi-values{display:flex;align-items:center;gap:6px;font-size:9px;color:#5580A0}
.kpi-baseline{background:#F0F6FC;border-radius:2px;padding:1px 5px}
.kpi-target{background:#E8F4FD;border:1px solid #B8D8F0;border-radius:2px;padding:1px 5px;color:#2196E0;font-weight:600}
.kpi-progress{width:100%;height:5px;background:#EEF2F7;border-radius:3px;margin-top:4px;overflow:hidden}
.kpi-bar{height:100%;border-radius:3px;background:#2196E0;transition:width 0.6s ease}
.kpi-bar.green{background:#1D9E75}
.kpi-bar.amber{background:#BA7517}
.kpi-bar.red{background:#E24B4A}

/* data source tag */
.kpi-source{font-size:8px;color:#8AAABB;background:#F7FAFD;border:1px solid #D8E6F2;border-radius:2px;padding:1px 5px;white-space:nowrap;flex-shrink:0}

/* divider */
.kpi-divider{height:1px;background:#EEF2F7;margin:8px 0}

/* ── DATA SOURCE LEGEND ── */
.source-legend{margin-top:16px;background:#fff;border:1px solid #D8E6F2;border-radius:4px;padding:12px 16px}
.source-legend-title{font-size:10px;font-weight:700;color:#2196E0;text-transform:uppercase;letter-spacing:0.06em;margin-bottom:8px}
.source-items{display:flex;flex-wrap:wrap;gap:6px}
.source-item{font-size:9px;background:#F0F6FC;border:1px solid #D8E6F2;border-radius:3px;padding:3px 8px;color:#1C2E4A}
.source-item strong{color:#2196E0}

/* ── ALERT BANNER ── */
.alert-banner{background:#FFF8E8;border:1px solid #F0C060;border-left:3px solid #BA7517;border-radius:3px;padding:7px 12px;margin-bottom:14px;font-size:10px;color:#633806}
.alert-banner strong{color:#854F0B}

/* ── OVERVIEW TAB ── */
.ov-grid{display:grid;grid-template-columns:repeat(5,1fr);gap:10px;margin-bottom:16px}
.ov-area{background:#fff;border:1px solid #D8E6F2;border-radius:4px;padding:12px;border-top:3px solid #2196E0;cursor:pointer;transition:border-color 0.15s,box-shadow 0.15s}
.ov-area:hover{border-color:#B8D8F0;box-shadow:0 2px 8px rgba(33,150,224,0.1)}
.ov-area-n{font-size:22px;font-weight:700;color:#2196E0;line-height:1;margin-bottom:2px}
.ov-area-label{font-size:10px;font-weight:600;color:#1C2E4A;line-height:1.3;margin-bottom:6px}
.ov-area-sub{font-size:9px;color:#5580A0;line-height:1.4}
.ov-area-bar{height:4px;background:#EEF2F7;border-radius:2px;margin-top:8px;overflow:hidden}
.ov-area-fill{height:100%;background:#2196E0;border-radius:2px}

.ov-bottom{display:grid;grid-template-columns:2fr 1fr;gap:12px}
.ov-timeline{background:#fff;border:1px solid #D8E6F2;border-radius:4px;padding:14px}
.ov-timeline-title{font-size:11px;font-weight:700;color:#1C2E4A;margin-bottom:10px}
.stage-row{display:flex;align-items:center;margin-bottom:8px;gap:10px}
.stage-dot{width:10px;height:10px;border-radius:50%;flex-shrink:0}
.stage-dot.complete{background:#1D9E75}
.stage-dot.active{background:#2196E0}
.stage-dot.future{background:#D8E6F2}
.stage-text{font-size:10px;color:#1C2E4A;font-weight:500}
.stage-period{font-size:9px;color:#5580A0;margin-left:auto}
.stage-connector{width:1px;height:12px;background:#D8E6F2;margin-left:4px}

.ov-data-sources{background:#fff;border:1px solid #D8E6F2;border-radius:4px;padding:14px}
.ov-ds-title{font-size:11px;font-weight:700;color:#1C2E4A;margin-bottom:10px}
.ds-row{display:flex;align-items:center;justify-content:space-between;padding:5px 0;border-bottom:1px solid #EEF2F7;font-size:10px}
.ds-row:last-child{border-bottom:none}
.ds-agency{color:#1C2E4A;font-weight:500}
.ds-type{color:#5580A0;font-size:9px}
.ds-dot{width:8px;height:8px;border-radius:50%;flex-shrink:0}

/* ── SCROLLABLE AREA ── */
.scroll-area{max-height:calc(100vh - 220px);overflow-y:auto;padding-right:2px}
</style>
</head>
<body>

<!-- TOP BAR -->
<div class="top-bar">
  <div class="top-bar-left">
    <div class="accenture-mark">ac<span>></span>centure</div>
    <div class="divider-v"></div>
    <div class="top-title">RHTF Performance Dashboard — Sample Prototype</div>
  </div>
  <div class="top-bar-right">
    <div class="last-updated">Last refreshed: FY26 Q2 · Data as of Apr 2026</div>
    <div class="source-badge">CMS-RHT-26-001</div>
  </div>
</div>

<!-- PAGE TITLE -->
<div class="title-band">
  <h1>Tennessee Rural Health Transformation Fund</h1>
  <p>17 initiatives · 5 focus areas · 89 rural counties · FY2026–FY2031 · $206.9M Budget Period 1</p>
  <div class="title-rule"></div>
</div>

<!-- SUMMARY STATS -->
<div class="summary-row">
  <div class="sstat"><div class="sstat-n">17</div><div class="sstat-l">Active initiatives</div></div>
  <div class="sstat"><div class="sstat-n">89</div><div class="sstat-l">Rural counties impacted</div></div>
  <div class="sstat"><div class="sstat-n">$206.9M</div><div class="sstat-l">BP1 CMS allocation</div></div>
  <div class="sstat"><div class="sstat-n">68+</div><div class="sstat-l">Committed KPIs</div></div>
  <div class="sstat"><div class="sstat-n">Stage 0–1</div><div class="sstat-l">Current program stage</div></div>
  <div class="sstat"><div class="sstat-n">FY2031</div><div class="sstat-l">Target achievement year</div></div>
</div>

<!-- TAB NAV -->
<div class="tab-nav">
  <button class="tab-btn active" onclick="showTab('overview')">Overview</button>
  <button class="tab-btn" onclick="showTab('rht')">Rural Healthcare Transformation</button>
  <button class="tab-btn" onclick="showTab('mch')">Maternal &amp; Child Health</button>
  <button class="tab-btn" onclick="showTab('martha')">Make Rural TN Healthy Again</button>
  <button class="tab-btn" onclick="showTab('tech')">Technology Infrastructure</button>
  <button class="tab-btn" onclick="showTab('workforce')">Workforce Development</button>
</div>

<!-- ══════════════════════════════════════════════
     OVERVIEW TAB
══════════════════════════════════════════════ -->
<div class="main active" id="tab-overview">
  <div class="alert-banner"><strong>Sample Prototype:</strong> Baseline values are drawn directly from Tennessee's RHTF Application (CMS-RHT-26-001). Progress bars illustrate Stage 0–1 status. Live data connections to TN ArcGIS, TDH PRAMS, TennCare, and program data systems will replace sample values upon system integration.</div>

  <div class="ov-grid">
    <div class="ov-area" onclick="showTab('rht')">
      <div class="ov-area-n">6</div>
      <div class="ov-area-label">Rural Healthcare Transformation</div>
      <div class="ov-area-sub">Co-location · Last Mile · Memory Care · Dental · Capacity</div>
      <div class="ov-area-bar"><div class="ov-area-fill" style="width:18%"></div></div>
    </div>
    <div class="ov-area" onclick="showTab('mch')">
      <div class="ov-area-n">2</div>
      <div class="ov-area-label">Maternal &amp; Child Health</div>
      <div class="ov-area-sub">HRP MCH · Value-Based Payment</div>
      <div class="ov-area-bar"><div class="ov-area-fill" style="width:22%"></div></div>
    </div>
    <div class="ov-area" onclick="showTab('martha')">
      <div class="ov-area-n">3</div>
      <div class="ov-area-label">Make Rural TN Healthy Again</div>
      <div class="ov-area-sub">MaRTHA · Rural Improvement Grants · RNET</div>
      <div class="ov-area-bar"><div class="ov-area-fill" style="width:12%"></div></div>
    </div>
    <div class="ov-area" onclick="showTab('tech')">
      <div class="ov-area-n">4</div>
      <div class="ov-area-label">Technology Infrastructure</div>
      <div class="ov-area-sub">Health-Tech · HIE · TN Compass · eConsult · Catalyst</div>
      <div class="ov-area-bar"><div class="ov-area-fill" style="width:15%"></div></div>
    </div>
    <div class="ov-area" onclick="showTab('workforce')">
      <div class="ov-area-n">1</div>
      <div class="ov-area-label">Workforce Development</div>
      <div class="ov-area-sub">Comprehensive Health Workforce Pipeline</div>
      <div class="ov-area-bar"><div class="ov-area-fill" style="width:10%"></div></div>
    </div>
  </div>

  <div class="ov-bottom">
    <div class="ov-timeline">
      <div class="ov-timeline-title">Program Stage Timeline</div>
      <div class="stage-row"><div class="stage-dot complete"></div><div class="stage-text">Stage 0 — Planning &amp; Procurement</div><div class="stage-period">FY26 Q1–Q2</div></div>
      <div class="stage-connector"></div>
      <div class="stage-row"><div class="stage-dot active"></div><div class="stage-text">Stage 1 — Launch &amp; Early Implementation</div><div class="stage-period">FY26 Q2–Q3 ← Now</div></div>
      <div class="stage-connector"></div>
      <div class="stage-row"><div class="stage-dot future"></div><div class="stage-text">Stage 2 — Full Operations</div><div class="stage-period">FY26 Q4</div></div>
      <div class="stage-connector"></div>
      <div class="stage-row"><div class="stage-dot future"></div><div class="stage-text">Stage 3 — Baseline Data Collection &amp; Evaluation Onboarding</div><div class="stage-period">FY26 Q3–FY27 Q1</div></div>
      <div class="stage-connector"></div>
      <div class="stage-row"><div class="stage-dot future"></div><div class="stage-text">Stage 4 — Performance Monitoring &amp; CMS Reporting</div><div class="stage-period">FY27–FY31</div></div>
      <div class="stage-connector"></div>
      <div class="stage-row"><div class="stage-dot future"></div><div class="stage-text">Stage 5 — Closeout &amp; Sustainability Transition</div><div class="stage-period">FY31 Q1</div></div>
    </div>

    <div class="ov-data-sources">
      <div class="ov-ds-title">Connected Data Sources</div>
      <div class="ds-row"><div class="ds-agency">TDH PRAMS</div><div class="ds-dot" style="background:#1D9E75"></div></div>
      <div class="ds-row"><div class="ds-agency">TennCare Claims / HEDIS</div><div class="ds-dot" style="background:#1D9E75"></div></div>
      <div class="ds-row"><div class="ds-agency">TN ArcGIS Open Data Portal</div><div class="ds-dot" style="background:#2196E0"></div></div>
      <div class="ds-row"><div class="ds-agency">TN Dept of Education</div><div class="ds-dot" style="background:#2196E0"></div></div>
      <div class="ds-row"><div class="ds-agency">CMS Adult Health Care Quality</div><div class="ds-dot" style="background:#2196E0"></div></div>
      <div class="ds-row"><div class="ds-agency">HRTS / RedCap Program Data</div><div class="ds-dot" style="background:#BA7517"></div></div>
      <div class="ds-row"><div class="ds-agency">TIPQC Quality Improvement</div><div class="ds-dot" style="background:#BA7517"></div></div>
      <div class="ds-row"><div class="ds-agency">Program / Grantee Data</div><div class="ds-dot" style="background:#BA7517"></div></div>
      <div style="margin-top:10px;font-size:9px;color:#5580A0">
        <span style="display:inline-block;width:8px;height:8px;border-radius:50%;background:#1D9E75;margin-right:4px"></span>Live feed available &nbsp;
        <span style="display:inline-block;width:8px;height:8px;border-radius:50%;background:#2196E0;margin-right:4px"></span>API ready &nbsp;
        <span style="display:inline-block;width:8px;height:8px;border-radius:50%;background:#BA7517;margin-right:4px"></span>Manual / TBD
      </div>
    </div>
  </div>
</div>

<!-- ══════════════════════════════════════════════
     RURAL HEALTHCARE TRANSFORMATION TAB
══════════════════════════════════════════════ -->
<div class="main" id="tab-rht">
  <div class="focus-header">
    <div><div class="focus-title">Rural Healthcare Transformation</div><div class="focus-desc">Expand integrated care access, optimize rural health systems, address memory care, dental gaps, and rural capacity — anchored to HRP proven models.</div></div>
    <div class="focus-period">Target: FY2031 Q4</div>
  </div>
  <div class="scroll-area">
  <div class="init-grid">

    <!-- Initiative 1 -->
    <div class="icard">
      <div class="icard-head">
        <div><div class="icard-num">Initiative 1</div><div class="icard-name">HRP: Service Line Expansion &amp; Co-Location</div></div>
        <div class="stage-pill planning">Stage 0–1</div>
      </div>
      <div class="icard-body">
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Rural primary care clinics adding integrated behavioral health</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 0</span><span class="kpi-target">Target: 10 clinics</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
          </div>
          <div class="kpi-source">Program / RedCap</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Orgs connected to TN Community Compass / closed-loop referral</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: TBD 2025</span><span class="kpi-target">Target: +50%</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:5%"></div></div>
          </div>
          <div class="kpi-source">Program / RedCap</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Co-located sites operational (target: 35 sustained)</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 0</span><span class="kpi-target">Target: 35 sites by FY30</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
          </div>
          <div class="kpi-source">Program Data</div>
        </div>
      </div>
    </div>

    <!-- Initiative 2 -->
    <div class="icard">
      <div class="icard-head">
        <div><div class="icard-num">Initiative 2</div><div class="icard-name">Last Mile Teams</div></div>
        <div class="stage-pill planning">Stage 0</div>
      </div>
      <div class="icard-body">
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Rural ambulances procured</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 0</span><span class="kpi-target">Target: 89</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
          </div>
          <div class="kpi-source">Program Data</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Rural counties with active Community Paramedic Programs</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 0</span><span class="kpi-target">Target: 20 counties</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
          </div>
          <div class="kpi-source">Program Data</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">EMS staff certified in Neonatal Resuscitation Program</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 0</span><span class="kpi-target">Target: 8,000 staff</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
          </div>
          <div class="kpi-source">Program / Training Logs</div>
        </div>
      </div>
    </div>

    <!-- Initiative 3 -->
    <div class="icard">
      <div class="icard-head">
        <div><div class="icard-num">Initiative 3</div><div class="icard-name">Optimizing Rural Health Care</div></div>
        <div class="stage-pill on-track">Stage 1</div>
      </div>
      <div class="icard-body">
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Provider placements in previously unserved counties</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 0</span><span class="kpi-target">Target: 21 placements</span></div>
            <div class="kpi-progress"><div class="kpi-bar green" style="width:14%"></div></div>
          </div>
          <div class="kpi-source">Program Data</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Sites offering primary care</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 300 sites</span><span class="kpi-target">Target: 325 sites</span></div>
            <div class="kpi-progress"><div class="kpi-bar green" style="width:22%"></div></div>
          </div>
          <div class="kpi-source">TN ArcGIS · HRSA UDS</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Tobacco/nicotine quit rate among participants</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 8.8%</span><span class="kpi-target">Target: 13%</span></div>
            <div class="kpi-progress"><div class="kpi-bar green" style="width:30%"></div></div>
          </div>
          <div class="kpi-source">Program Data</div>
        </div>
      </div>
    </div>

    <!-- Initiative 4 -->
    <div class="icard">
      <div class="icard-head">
        <div><div class="icard-num">Initiative 4</div><div class="icard-name">Memory Care Assessment Network (MCAN)</div></div>
        <div class="stage-pill planning">Stage 0</div>
      </div>
      <div class="icard-body">
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Rural residents receiving confirmed dementia diagnosis via MACN+DN</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: TBD</span><span class="kpi-target">Target: 100/MACN by FY28</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
          </div>
          <div class="kpi-source">Program Data</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Dementia Navigators in rural health departments</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 0</span><span class="kpi-target">Target: 30 by FY28 Q4</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
          </div>
          <div class="kpi-source">Program Data</div>
        </div>
      </div>
    </div>

    <!-- Initiative 5 -->
    <div class="icard">
      <div class="icard-head">
        <div><div class="icard-num">Initiative 5</div><div class="icard-name">Rural Capacity Building</div></div>
        <div class="stage-pill planning">Stage 0–1</div>
      </div>
      <div class="icard-body">
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">MH providers co-located in rural primary care / EDs</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 11.4 FTEs</span><span class="kpi-target">Target: 63.4 FTEs</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:8%"></div></div>
          </div>
          <div class="kpi-source">Program Data</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Reduction in median ED boarding hours</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: Tracked</span><span class="kpi-target">Target: 40% reduction</span></div>
            <div class="kpi-progress"><div class="kpi-bar amber" style="width:5%"></div></div>
          </div>
          <div class="kpi-source">Program Data / HDDS</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Provider orgs with community health worker accreditation</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 0</span><span class="kpi-target">Target: 20 organizations</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
          </div>
          <div class="kpi-source">Program Data</div>
        </div>
      </div>
    </div>

    <!-- Initiative 6 -->
    <div class="icard">
      <div class="icard-head">
        <div><div class="icard-num">Initiative 6</div><div class="icard-name">Dental Pilot</div></div>
        <div class="stage-pill planning">Stage 0</div>
      </div>
      <div class="icard-body">
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Dental clinicians placed in rural/distressed counties</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 0</span><span class="kpi-target">Target: 25 clinicians</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
          </div>
          <div class="kpi-source">Program Data</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">New or renovated dental suites</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 0</span><span class="kpi-target">Target: 15 suites</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
          </div>
          <div class="kpi-source">Program Data</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">HRTS dental metrics integration</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: Not integrated</span><span class="kpi-target">Target: Full HRTS integration</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
          </div>
          <div class="kpi-source">HRTS System Data</div>
        </div>
      </div>
    </div>

  </div>

  <div class="source-legend">
    <div class="source-legend-title">Data Sources — Focus Area 1</div>
    <div class="source-items">
      <div class="source-item"><strong>TN ArcGIS Open Data</strong> — hospital/clinic locations, county boundaries, facility mapping</div>
      <div class="source-item"><strong>HRSA UDS</strong> — FQHC and primary care site counts</div>
      <div class="source-item"><strong>HRTS (TDH)</strong> — healthcare resource tracking system</div>
      <div class="source-item"><strong>RedCap</strong> — program-level data collection (HRP grantees)</div>
      <div class="source-item"><strong>TDH HDDS</strong> — hospital discharge data system (boarding hours, ED visits)</div>
      <div class="source-item"><strong>Program / Grantee Data</strong> — collected by implementation staff</div>
    </div>
  </div>
  </div>
</div>

<!-- ══════════════════════════════════════════════
     MATERNAL & CHILD HEALTH TAB
══════════════════════════════════════════════ -->
<div class="main" id="tab-mch">
  <div class="focus-header">
    <div><div class="focus-title">Maternal &amp; Child Health</div><div class="focus-desc">Reduce rural maternal and infant mortality, expand access in maternity care deserts, and advance value-based payment models for obstetric, hospital, and dental care.</div></div>
    <div class="focus-period">Target: FY2031 Q4</div>
  </div>
  <div class="scroll-area">
  <div class="init-grid two-col">

    <!-- Initiative 7 -->
    <div class="icard">
      <div class="icard-head">
        <div><div class="icard-num">Initiative 7</div><div class="icard-name">HRP: Maternal &amp; Child Health</div></div>
        <div class="stage-pill on-track">Stage 1</div>
      </div>
      <div class="icard-body">
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Maternity care desert counties served through MCH HRP grants</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 0 counties</span><span class="kpi-target">Target: 100% of desert counties</span></div>
            <div class="kpi-progress"><div class="kpi-bar green" style="width:20%"></div></div>
          </div>
          <div class="kpi-source">Program Data</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Birthing hospitals in rural TN participating in TIPQC quality improvement</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 61%</span><span class="kpi-target">Target: 73%</span></div>
            <div class="kpi-progress"><div class="kpi-bar green" style="width:35%"></div></div>
          </div>
          <div class="kpi-source">TIPQC / Program Data</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Fatal overdose rate — women aged 15–44 (per 100,000)</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 47/100k</span><span class="kpi-target">Target: 42/100k</span></div>
            <div class="kpi-progress"><div class="kpi-bar amber" style="width:15%"></div></div>
          </div>
          <div class="kpi-source">TDH PRAMS</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Women screened for postpartum depression by healthcare provider</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 84%</span><span class="kpi-target">Target: 91%</span></div>
            <div class="kpi-progress"><div class="kpi-bar green" style="width:28%"></div></div>
          </div>
          <div class="kpi-source">TDH PRAMS</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Rural maternal mortality rate (per 100,000) — 25% reduction target</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 53/100k</span><span class="kpi-target">Target: ≤40/100k</span></div>
            <div class="kpi-progress"><div class="kpi-bar amber" style="width:8%"></div></div>
          </div>
          <div class="kpi-source">TDH Vital Records</div>
        </div>
      </div>
    </div>

    <!-- Initiative 8 -->
    <div class="icard">
      <div class="icard-head">
        <div><div class="icard-num">Initiative 8</div><div class="icard-name">Value-Based Payment: Maternal, Hospitals, Dental</div></div>
        <div class="stage-pill planning">Stage 0–1</div>
      </div>
      <div class="icard-body">
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">ADT quality measure performance in HIP-QC (Admission, Discharge, Transfer)</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 41%</span><span class="kpi-target">Target: 70%</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:10%"></div></div>
          </div>
          <div class="kpi-source">TennCare Data</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Obstetric practices enrolled in maternal VBP program</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 0</span><span class="kpi-target">Target: 10 practices</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
          </div>
          <div class="kpi-source">TennCare Data</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Plan All-Cause Readmissions (PCR) rate</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 1.1445</span><span class="kpi-target">Target: &lt;1.0</span></div>
            <div class="kpi-progress"><div class="kpi-bar amber" style="width:12%"></div></div>
          </div>
          <div class="kpi-source">HEDIS</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Ambulatory Care Sensitive ED visits — Non-Traumatic Dental (EDV-AD)</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 180.75</span><span class="kpi-target">Target: 170</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:8%"></div></div>
          </div>
          <div class="kpi-source">CMS Adult Health Care Quality</div>
        </div>
      </div>
    </div>

  </div>
  <div class="source-legend">
    <div class="source-legend-title">Data Sources — Focus Area 2</div>
    <div class="source-items">
      <div class="source-item"><strong>TDH PRAMS</strong> — Pregnancy Risk Assessment Monitoring System (postpartum screening, overdose rate)</div>
      <div class="source-item"><strong>TDH Vital Records</strong> — maternal/infant mortality, birth statistics</div>
      <div class="source-item"><strong>TennCare Claims</strong> — ADT quality measures, VBP enrollment, readmissions</div>
      <div class="source-item"><strong>HEDIS</strong> — Plan All-Cause Readmissions measure</div>
      <div class="source-item"><strong>CMS Adult Health Care Quality Measures</strong> — dental-related ED visits (EDV-AD)</div>
      <div class="source-item"><strong>TIPQC</strong> — Tennessee Initiative for Perinatal Quality Care participation rates</div>
    </div>
  </div>
  </div>
</div>

<!-- ══════════════════════════════════════════════
     MARTHA TAB
══════════════════════════════════════════════ -->
<div class="main" id="tab-martha">
  <div class="focus-header">
    <div><div class="focus-title">Make Rural Tennessee Healthy Again (MaRTHA)</div><div class="focus-desc">Community-driven prevention, chronic disease management, nutrition security, active living environments, and transportation access for all 89 rural counties.</div></div>
    <div class="focus-period">Target: FY2031 Q4</div>
  </div>
  <div class="scroll-area">
  <div class="init-grid">

    <!-- Initiative 9 -->
    <div class="icard">
      <div class="icard-head">
        <div><div class="icard-num">Initiative 9</div><div class="icard-name">HRP: Make Rural Tennessee Healthy Again</div></div>
        <div class="stage-pill planning">Stage 0 (FY27 Q1)</div>
      </div>
      <div class="icard-body">
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Rural residents with an identified primary care provider</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: TBD</span><span class="kpi-target">Target: +10% increase</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
          </div>
          <div class="kpi-source">Program / Grantee Data</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Mobile health strategies across TN's 3 grand divisions</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 0</span><span class="kpi-target">Target: 5 strategies</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
          </div>
          <div class="kpi-source">Program / Grantee Data</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">% of TN public schools employing a full-time nurse</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 83% (2023–24)</span><span class="kpi-target">Target: Per grantee</span></div>
            <div class="kpi-progress"><div class="kpi-bar green" style="width:52%"></div></div>
          </div>
          <div class="kpi-source">TN Dept of Education</div>
        </div>
      </div>
    </div>

    <!-- Initiative 10 -->
    <div class="icard">
      <div class="icard-head">
        <div><div class="icard-num">Initiative 10</div><div class="icard-name">Rural Health Improvement Grants</div></div>
        <div class="stage-pill on-track">Stage 0–1</div>
      </div>
      <div class="icard-body">
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Rural counties funded for health-promoting environments (nutrition/active living)</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 45 counties</span><span class="kpi-target">Target: 89 counties</span></div>
            <div class="kpi-progress"><div class="kpi-bar green" style="width:51%"></div></div>
          </div>
          <div class="kpi-source">Program Data</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Rural Tennesseans living within 1 mile of a health-promoting environment</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 0%</span><span class="kpi-target">Target: 25%</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:3%"></div></div>
          </div>
          <div class="kpi-source">Program &amp; Census / ArcGIS</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">CHC / NGO partnerships implementing End-Zone PSE strategies</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 0</span><span class="kpi-target">Target: 3 per grantee</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
          </div>
          <div class="kpi-source">Program Data</div>
        </div>
      </div>
    </div>

    <!-- Initiative 11 -->
    <div class="icard">
      <div class="icard-head">
        <div><div class="icard-num">Initiative 11</div><div class="icard-name">HRP: Rural Non-Emergency Transportation (RNET)</div></div>
        <div class="stage-pill planning">Stage 0</div>
      </div>
      <div class="icard-body">
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Rural residents with access to coordinated NEMT system</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 0</span><span class="kpi-target">Target: Statewide coverage</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
          </div>
          <div class="kpi-source">Program Data / TN ArcGIS</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Missed appointments reduced due to transportation barriers</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: TBD</span><span class="kpi-target">Target: TBD via grantee</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
          </div>
          <div class="kpi-source">Program / TennCare Claims</div>
        </div>
      </div>
    </div>

  </div>
  <div class="source-legend">
    <div class="source-legend-title">Data Sources — Focus Area 3</div>
    <div class="source-items">
      <div class="source-item"><strong>TN Dept of Education</strong> — school nurse employment data</div>
      <div class="source-item"><strong>TN ArcGIS Open Data Portal</strong> — county boundaries, proximity analysis for health-promoting environments</div>
      <div class="source-item"><strong>US Census / ACS</strong> — rural population estimates, proximity to services</div>
      <div class="source-item"><strong>Program / Grantee Data</strong> — CHC CARE grant reporting, PSE implementations</div>
      <div class="source-item"><strong>TennCare Claims</strong> — missed appointment patterns, transportation-related utilization</div>
    </div>
  </div>
  </div>
</div>

<!-- ══════════════════════════════════════════════
     TECHNOLOGY INFRASTRUCTURE TAB
══════════════════════════════════════════════ -->
<div class="main" id="tab-tech">
  <div class="focus-header">
    <div><div class="focus-title">Technology Infrastructure</div><div class="focus-desc">Modernize rural health IT through HIE, closed-loop referral, telehealth, eConsult, and cybersecurity investments to expand interoperability across all 89 counties.</div></div>
    <div class="focus-period">Target: FY2031 Q4</div>
  </div>
  <div class="scroll-area">
  <div class="init-grid two-col">

    <!-- Initiative 12 -->
    <div class="icard">
      <div class="icard-head">
        <div><div class="icard-num">Initiative 12</div><div class="icard-name">HRP: Health-Tech Grants</div></div>
        <div class="stage-pill planning">Stage 0–1</div>
      </div>
      <div class="icard-body">
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">HRP awardees connected to or with roadmap to HIE</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: &lt;100%</span><span class="kpi-target">Target: 100% by FY31</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:10%"></div></div>
          </div>
          <div class="kpi-source">Program Data / HIE Registry</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Rural facilities modernized / right-sized / co-located</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 0 under RHTF</span><span class="kpi-target">Target: 50+ facilities</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
          </div>
          <div class="kpi-source">Program Data / TN ArcGIS</div>
        </div>
      </div>
    </div>

    <!-- Initiative 13 -->
    <div class="icard">
      <div class="icard-head">
        <div><div class="icard-num">Initiative 13</div><div class="icard-name">Statewide Health Information Exchange (HIE)</div></div>
        <div class="stage-pill planning">Stage 0–1</div>
      </div>
      <div class="icard-body">
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Eligible providers / HRP awardees connected to statewide HIE</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: Partial</span><span class="kpi-target">Target: 100% by FY31</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:15%"></div></div>
          </div>
          <div class="kpi-source">TDH HIE Registry</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">LTC Skilled facility HRTS users (increase 15%)</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 849 users</span><span class="kpi-target">Target: 977 users</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:12%"></div></div>
          </div>
          <div class="kpi-source">HRTS User Data</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">HRTS training events per year (25% increase)</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 40/year</span><span class="kpi-target">Target: 50/year</span></div>
            <div class="kpi-progress"><div class="kpi-bar green" style="width:20%"></div></div>
          </div>
          <div class="kpi-source">HRTS User Data</div>
        </div>
      </div>
    </div>

    <!-- Initiative 14 -->
    <div class="icard">
      <div class="icard-head">
        <div><div class="icard-num">Initiative 14</div><div class="icard-name">TN Community Compass (Closed-Loop Referral)</div></div>
        <div class="stage-pill on-track">Stage 1</div>
      </div>
      <div class="icard-body">
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Organizations connected to TN Community Compass (50% increase target)</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: TBD 2025</span><span class="kpi-target">Target: +50% connections</span></div>
            <div class="kpi-progress"><div class="kpi-bar green" style="width:18%"></div></div>
          </div>
          <div class="kpi-source">Program Data / RedCap</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">SDOH referral completion rate via Compass platform</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: TBD</span><span class="kpi-target">Target: TBD via evaluation</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
          </div>
          <div class="kpi-source">Platform Data / Program</div>
        </div>
      </div>
    </div>

    <!-- Initiative 15 & 16 -->
    <div class="icard">
      <div class="icard-head">
        <div><div class="icard-num">Initiatives 15–16</div><div class="icard-name">Rural Health Innovation Catalyst &amp; Statewide eConsult Platform</div></div>
        <div class="stage-pill planning">Stage 0–1</div>
      </div>
      <div class="icard-body">
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">e-Consultations provided without face-to-face conversion</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 17,600 referrals/yr</span><span class="kpi-target">Target: 10,000 eConsults/yr</span></div>
            <div class="kpi-progress"><div class="kpi-bar" style="width:5%"></div></div>
          </div>
          <div class="kpi-source">Program Data</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Average specialty care wait time</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 6–9 months</span><span class="kpi-target">Target: 3 days avg via eConsult</span></div>
            <div class="kpi-progress"><div class="kpi-bar amber" style="width:5%"></div></div>
          </div>
          <div class="kpi-source">Program Data</div>
        </div>
        <div class="kpi-divider"></div>
        <div class="kpi-row">
          <div class="kpi-info">
            <div class="kpi-label">Rural residents within 30 min of primary care or co-location site</div>
            <div class="kpi-values"><span class="kpi-baseline">Baseline: 68% (HRSA)</span><span class="kpi-target">Target: &gt;80% by FY31</span></div>
            <div class="kpi-progress"><div class="kpi-bar green" style="width:48%"></div></div>
          </div>
          <div class="kpi-source">HRSA / TN ArcGIS</div>
        </div>
      </div>
    </div>

  </div>
  <div class="source-legend">
    <div class="source-legend-title">Data Sources — Focus Area 4</div>
    <div class="source-items">
      <div class="source-item"><strong>TN ArcGIS Open Data Portal</strong> — facility locations, geographic access analysis (tnmap.tn.gov/arcgis)</div>
      <div class="source-item"><strong>HRTS (TDH)</strong> — healthcare resource tracking system; LTC user data; training event logs</div>
      <div class="source-item"><strong>TDH HIE Registry</strong> — provider connection status to statewide HIE</div>
      <div class="source-item"><strong>HRSA UDS / HPSA Data</strong> — primary care access and shortage area designations</div>
      <div class="source-item"><strong>TN Community Compass Platform</strong> — SDOH referral completion and connection data</div>
      <div class="source-item"><strong>Program / Grantee Data</strong> — eConsult volume, wait times, IT implementation milestones</div>
    </div>
  </div>
  </div>
</div>

<!-- ══════════════════════════════════════════════
     WORKFORCE DEVELOPMENT TAB
══════════════════════════════════════════════ -->
<div class="main" id="tab-workforce">
  <div class="focus-header">
    <div><div class="focus-title">Workforce Development</div><div class="focus-desc">Build a comprehensive rural health workforce pipeline from early exposure through advanced practice — residencies, apprenticeships, recruitment incentives, and academic health department partnerships.</div></div>
    <div class="focus-period">Target: FY2031 Q4</div>
  </div>
  <div class="scroll-area">
  <div class="init-grid one-col" style="max-width:720px">

    <!-- Initiative 17 -->
    <div class="icard">
      <div class="icard-head">
        <div><div class="icard-num">Initiative 17</div><div class="icard-name">Comprehensive Health Workforce Pipeline</div></div>
        <div class="stage-pill planning">Stage 0</div>
      </div>
      <div class="icard-body">
        <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px">
          <div>
            <div class="kpi-row">
              <div class="kpi-info">
                <div class="kpi-label">Rural recruits supported through incentive programs (residencies)</div>
                <div class="kpi-values"><span class="kpi-baseline">Baseline: 0</span><span class="kpi-target">Target: 250 residencies</span></div>
                <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
              </div>
              <div class="kpi-source">Program Data</div>
            </div>
            <div class="kpi-divider"></div>
            <div class="kpi-row">
              <div class="kpi-info">
                <div class="kpi-label">Students in early exposure or paid internships</div>
                <div class="kpi-values"><span class="kpi-baseline">Baseline: 0</span><span class="kpi-target">Target: 1,000 students</span></div>
                <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
              </div>
              <div class="kpi-source">Program Data</div>
            </div>
            <div class="kpi-divider"></div>
            <div class="kpi-row">
              <div class="kpi-info">
                <div class="kpi-label">Paid apprenticeship programs</div>
                <div class="kpi-values"><span class="kpi-baseline">Baseline: 38</span><span class="kpi-target">Target: 80</span></div>
                <div class="kpi-progress"><div class="kpi-bar green" style="width:48%"></div></div>
              </div>
              <div class="kpi-source">Program Data</div>
            </div>
          </div>
          <div>
            <div class="kpi-row">
              <div class="kpi-info">
                <div class="kpi-label">Rural residency and academic rural healthcare training programs</div>
                <div class="kpi-values"><span class="kpi-baseline">Baseline: 10</span><span class="kpi-target">Target: 50 programs</span></div>
                <div class="kpi-progress"><div class="kpi-bar green" style="width:20%"></div></div>
              </div>
              <div class="kpi-source">Program Data</div>
            </div>
            <div class="kpi-divider"></div>
            <div class="kpi-row">
              <div class="kpi-info">
                <div class="kpi-label">Provider placements in counties not currently served</div>
                <div class="kpi-values"><span class="kpi-baseline">Baseline: 0</span><span class="kpi-target">Target: Per county need</span></div>
                <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
              </div>
              <div class="kpi-source">Program Data / HRSA HPSA</div>
            </div>
            <div class="kpi-divider"></div>
            <div class="kpi-row">
              <div class="kpi-info">
                <div class="kpi-label">Rural behavioral health careers advanced (pipeline)</div>
                <div class="kpi-values"><span class="kpi-baseline">Baseline: TBD</span><span class="kpi-target">Target: TBD via grantee</span></div>
                <div class="kpi-progress"><div class="kpi-bar" style="width:0%"></div></div>
              </div>
              <div class="kpi-source">Program Data</div>
            </div>
          </div>
        </div>
      </div>
    </div>

  </div>
  <div class="source-legend">
    <div class="source-legend-title">Data Sources — Focus Area 5</div>
    <div class="source-items">
      <div class="source-item"><strong>Program / Grantee Data</strong> — residency placements, apprenticeship enrollment, early exposure participation</div>
      <div class="source-item"><strong>HRSA HPSA Data</strong> — health professional shortage area designations by county</div>
      <div class="source-item"><strong>TN ArcGIS Open Data</strong> — geographic distribution of providers; workforce shortage mapping</div>
      <div class="source-item"><strong>TDH Workforce Registry</strong> — licensed provider counts by county and specialty</div>
      <div class="source-item"><strong>TN Dept of Education / THEC</strong> — academic training program enrollment and graduation rates</div>
    </div>
  </div>
  </div>
</div>

<script>
function showTab(id){
  document.querySelectorAll('.tab-btn').forEach(b=>b.classList.remove('active'));
  document.querySelectorAll('.main').forEach(m=>{m.style.display='none';m.classList.remove('active')});
  const map={overview:'tab-overview',rht:'tab-rht',mch:'tab-mch',martha:'tab-martha',tech:'tab-tech',workforce:'tab-workforce'};
  document.getElementById(map[id]).style.display='block';
  document.querySelectorAll('.tab-btn').forEach(b=>{
    const labels={overview:'Overview',rht:'Rural Healthcare Transformation',mch:'Maternal & Child Health',martha:'Make Rural TN Healthy Again',tech:'Technology Infrastructure',workforce:'Workforce Development'};
    if(b.textContent.trim().replace(/&amp;/g,'&')===labels[id]||b.innerText===labels[id]){b.classList.add('active')}
  });
  const idx={overview:0,rht:1,mch:2,martha:3,tech:4,workforce:5};
  document.querySelectorAll('.tab-btn')[idx[id]].classList.add('active');
}
</script>

</body>
</html>
