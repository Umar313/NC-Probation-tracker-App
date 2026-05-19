<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Probation Tracker — Netcracker Technology</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Sora:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --nc-navy: #0A1628;
    --nc-blue: #1A3A6B;
    --nc-accent: #1E6FD9;
    --nc-bright: #3B9EFF;
    --nc-teal: #00C5A1;
    --nc-amber: #F59E0B;
    --nc-red: #EF4444;
    --nc-green: #10B981;
    --nc-bg: #F0F4FB;
    --nc-card: #FFFFFF;
    --nc-text: #0A1628;
    --nc-muted: #64748B;
    --nc-border: #DDE4EF;
    --nc-hover: #EAF1FB;
    --radius: 12px;
    --radius-sm: 8px;
    --shadow: 0 1px 3px rgba(10,22,40,0.08), 0 4px 16px rgba(10,22,40,0.06);
    --shadow-lg: 0 8px 32px rgba(10,22,40,0.12);
  }

  body {
    font-family: 'Sora', sans-serif;
    background: var(--nc-bg);
    color: var(--nc-text);
    min-height: 100vh;
    font-size: 14px;
    line-height: 1.6;
  }

  /* ── LAYOUT ── */
  .layout { display: flex; min-height: 100vh; }

  /* ── SIDEBAR ── */
  .sidebar {
    width: 240px; flex-shrink: 0;
    background: var(--nc-navy);
    display: flex; flex-direction: column;
    position: fixed; top: 0; left: 0; bottom: 0;
    z-index: 100;
  }
  .sidebar-logo {
    padding: 24px 20px 16px;
    border-bottom: 1px solid rgba(255,255,255,0.08);
  }
  .logo-mark {
    font-size: 11px; font-weight: 600; letter-spacing: 0.12em;
    color: var(--nc-bright); text-transform: uppercase; margin-bottom: 4px;
  }
  .logo-title { font-size: 16px; font-weight: 700; color: #fff; line-height: 1.2; }
  .sidebar-nav { flex: 1; padding: 12px 10px; overflow-y: auto; }
  .nav-section { margin-bottom: 24px; }
  .nav-label { font-size: 10px; font-weight: 600; letter-spacing: 0.12em; text-transform: uppercase;
    color: rgba(255,255,255,0.35); padding: 0 10px 8px; }
  .nav-item {
    display: flex; align-items: center; gap: 10px;
    padding: 9px 10px; border-radius: var(--radius-sm);
    color: rgba(255,255,255,0.6); cursor: pointer;
    transition: all 0.15s; font-size: 13px; font-weight: 400;
    margin-bottom: 2px; border: none; background: none; width: 100%; text-align: left;
  }
  .nav-item:hover { background: rgba(255,255,255,0.07); color: #fff; }
  .nav-item.active { background: rgba(59,158,255,0.18); color: var(--nc-bright); font-weight: 500; }
  .nav-item .ni-icon { width: 16px; height: 16px; flex-shrink: 0; opacity: 0.85; }
  .nav-badge {
    margin-left: auto; background: var(--nc-accent); color: #fff;
    font-size: 10px; font-weight: 600; padding: 1px 6px; border-radius: 10px;
  }
  .nav-badge.warn { background: var(--nc-amber); color: #1a1a1a; }
  .sidebar-footer {
    padding: 16px 20px;
    border-top: 1px solid rgba(255,255,255,0.08);
  }
  .user-pill { display: flex; align-items: center; gap: 10px; }
  .user-avatar {
    width: 32px; height: 32px; border-radius: 50%;
    background: linear-gradient(135deg, var(--nc-accent), var(--nc-teal));
    display: flex; align-items: center; justify-content: center;
    font-size: 11px; font-weight: 700; color: #fff; flex-shrink: 0;
  }
  .user-name { font-size: 12px; font-weight: 500; color: rgba(255,255,255,0.85); }
  .user-role { font-size: 10px; color: rgba(255,255,255,0.4); }

  /* ── MAIN ── */
  .main { margin-left: 240px; flex: 1; display: flex; flex-direction: column; }
  .topbar {
    background: #fff; border-bottom: 1px solid var(--nc-border);
    padding: 0 28px; height: 56px;
    display: flex; align-items: center; justify-content: space-between;
    position: sticky; top: 0; z-index: 50;
  }
  .topbar-title { font-size: 15px; font-weight: 600; color: var(--nc-text); }
  .topbar-actions { display: flex; align-items: center; gap: 10px; }
  .btn {
    display: inline-flex; align-items: center; gap: 7px;
    padding: 8px 16px; border-radius: var(--radius-sm);
    font-family: 'Sora', sans-serif; font-size: 13px; font-weight: 500;
    cursor: pointer; transition: all 0.15s; border: none;
  }
  .btn-primary { background: var(--nc-accent); color: #fff; }
  .btn-primary:hover { background: #1558b8; }
  .btn-outline {
    background: #fff; color: var(--nc-text);
    border: 1px solid var(--nc-border);
  }
  .btn-outline:hover { background: var(--nc-hover); }
  .btn-sm { padding: 5px 12px; font-size: 12px; }
  .btn-icon { padding: 7px; border-radius: var(--radius-sm); background: transparent; border: 1px solid var(--nc-border); }
  .btn-icon:hover { background: var(--nc-hover); }

  /* ── PAGE SECTIONS ── */
  .page { display: none; padding: 28px; }
  .page.active { display: block; }

  /* ── STAT CARDS ── */
  .stats-row { display: grid; grid-template-columns: repeat(4, 1fr); gap: 16px; margin-bottom: 24px; }
  .stat-card {
    background: var(--nc-card); border-radius: var(--radius);
    border: 1px solid var(--nc-border); padding: 20px;
    box-shadow: var(--shadow);
  }
  .stat-label { font-size: 11px; font-weight: 600; letter-spacing: 0.08em; text-transform: uppercase; color: var(--nc-muted); margin-bottom: 8px; }
  .stat-value { font-size: 28px; font-weight: 700; color: var(--nc-text); line-height: 1; margin-bottom: 4px; }
  .stat-sub { font-size: 11px; color: var(--nc-muted); }
  .stat-dot { display: inline-block; width: 8px; height: 8px; border-radius: 50%; margin-right: 5px; }

  /* ── SECTION HEADER ── */
  .section-header {
    display: flex; align-items: center; justify-content: space-between;
    margin-bottom: 16px;
  }
  .section-title { font-size: 15px; font-weight: 600; }
  .section-sub { font-size: 12px; color: var(--nc-muted); margin-top: 2px; }

  /* ── FILTERS ── */
  .filters {
    display: flex; align-items: center; gap: 10px;
    background: var(--nc-card); border: 1px solid var(--nc-border);
    border-radius: var(--radius); padding: 14px 18px;
    margin-bottom: 16px; flex-wrap: wrap;
  }
  .filter-label { font-size: 12px; font-weight: 500; color: var(--nc-muted); }
  .filter-select, .filter-input {
    padding: 6px 12px; border-radius: var(--radius-sm);
    border: 1px solid var(--nc-border); font-family: 'Sora', sans-serif;
    font-size: 12px; color: var(--nc-text); background: var(--nc-bg);
    outline: none; cursor: pointer;
  }
  .filter-select:focus, .filter-input:focus { border-color: var(--nc-accent); }
  .filter-chip {
    display: inline-flex; align-items: center; gap: 5px;
    padding: 4px 10px; border-radius: 20px; font-size: 11px; font-weight: 600;
    cursor: pointer; border: 1px solid transparent; transition: all 0.15s;
  }
  .chip-all { background: var(--nc-navy); color: #fff; }
  .chip-active { background: #DBEAFE; color: #1D4ED8; border-color: #BFDBFE; }
  .chip-completed { background: #D1FAE5; color: #065F46; border-color: #A7F3D0; }
  .chip-extended { background: #FEF3C7; color: #92400E; border-color: #FDE68A; }
  .chip-overdue { background: #FEE2E2; color: #991B1B; border-color: #FECACA; }

  /* ── TABLE ── */
  .table-wrap { background: var(--nc-card); border-radius: var(--radius); border: 1px solid var(--nc-border); box-shadow: var(--shadow); overflow: hidden; }
  table { width: 100%; border-collapse: collapse; }
  thead { background: #F8FAFC; }
  th {
    text-align: left; padding: 10px 16px;
    font-size: 11px; font-weight: 600; letter-spacing: 0.07em; text-transform: uppercase;
    color: var(--nc-muted); border-bottom: 1px solid var(--nc-border);
    white-space: nowrap;
  }
  td { padding: 13px 16px; border-bottom: 1px solid var(--nc-border); font-size: 13px; vertical-align: middle; }
  tr:last-child td { border-bottom: none; }
  tbody tr { transition: background 0.1s; }
  tbody tr:hover { background: #F8FAFC; }

  /* ── BADGES ── */
  .badge {
    display: inline-flex; align-items: center; gap: 5px;
    padding: 3px 9px; border-radius: 20px; font-size: 11px; font-weight: 600;
  }
  .badge-active { background: #DBEAFE; color: #1D4ED8; }
  .badge-completed { background: #D1FAE5; color: #065F46; }
  .badge-extended { background: #FEF3C7; color: #92400E; }
  .badge-overdue { background: #FEE2E2; color: #991B1B; }
  .badge-pending { background: #F3F4F6; color: #374151; }
  .badge-referral { background: #EDE9FE; color: #5B21B6; }

  /* ── PROGRESS BAR ── */
  .progress-wrap { display: flex; align-items: center; gap: 10px; }
  .progress-bar {
    flex: 1; height: 6px; background: var(--nc-border); border-radius: 3px; overflow: hidden;
    min-width: 80px;
  }
  .progress-fill { height: 100%; border-radius: 3px; transition: width 0.5s; }
  .progress-label { font-size: 11px; font-weight: 500; color: var(--nc-muted); min-width: 28px; text-align: right; font-family: 'JetBrains Mono', monospace; }

  /* ── AVATAR ── */
  .avatar-stack { display: flex; align-items: center; gap: 8px; }
  .avatar {
    width: 28px; height: 28px; border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    font-size: 10px; font-weight: 700; color: #fff; flex-shrink: 0;
    border: 2px solid #fff;
  }
  .av-blue { background: linear-gradient(135deg, #3B82F6, #1D4ED8); }
  .av-teal { background: linear-gradient(135deg, #14B8A6, #0D9488); }
  .av-purple { background: linear-gradient(135deg, #8B5CF6, #6D28D9); }
  .av-amber { background: linear-gradient(135deg, #F59E0B, #D97706); }
  .av-rose { background: linear-gradient(135deg, #F43F5E, #BE123C); }
  .av-green { background: linear-gradient(135deg, #10B981, #059669); }
  .av-navy { background: linear-gradient(135deg, #1A3A6B, #0A1628); }
  .av-orange { background: linear-gradient(135deg, #F97316, #EA580C); }
  .av-red { background: linear-gradient(135deg, #EF4444, #DC2626); }

  .avatar-name { font-size: 13px; font-weight: 500; }
  .avatar-dept { font-size: 11px; color: var(--nc-muted); }

  /* ── MODAL ── */
  .modal-overlay {
    display: none; position: fixed; inset: 0;
    background: rgba(10,22,40,0.55); backdrop-filter: blur(4px);
    z-index: 1000; justify-content: center; align-items: center; padding: 20px;
  }
  .modal-overlay.open { display: flex; }
  .modal {
    background: var(--nc-card); border-radius: 16px; box-shadow: var(--shadow-lg);
    width: 100%; max-width: 620px; max-height: 90vh; overflow-y: auto;
    animation: fadeUp 0.2s ease;
  }
  .modal-lg { max-width: 780px; }
  @keyframes fadeUp { from { opacity: 0; transform: translateY(12px); } to { opacity: 1; transform: translateY(0); } }
  .modal-header {
    padding: 22px 24px 18px; border-bottom: 1px solid var(--nc-border);
    display: flex; align-items: flex-start; justify-content: space-between;
  }
  .modal-title { font-size: 16px; font-weight: 700; margin-bottom: 3px; }
  .modal-sub { font-size: 12px; color: var(--nc-muted); }
  .modal-body { padding: 24px; }
  .modal-footer {
    padding: 16px 24px; border-top: 1px solid var(--nc-border);
    display: flex; gap: 10px; justify-content: flex-end;
  }
  .close-btn {
    background: none; border: none; cursor: pointer;
    color: var(--nc-muted); padding: 4px; border-radius: 6px;
    font-size: 18px; line-height: 1; transition: color 0.15s;
  }
  .close-btn:hover { color: var(--nc-text); }

  /* ── FORM ── */
  .form-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 18px; }
  .form-group { display: flex; flex-direction: column; gap: 6px; }
  .form-group.span-2 { grid-column: 1 / -1; }
  label { font-size: 12px; font-weight: 600; color: var(--nc-text); }
  .form-hint { font-size: 11px; color: var(--nc-muted); margin-top: 2px; }
  input[type=text], input[type=email], input[type=date], input[type=number], select, textarea {
    padding: 9px 12px; border-radius: var(--radius-sm);
    border: 1px solid var(--nc-border); font-family: 'Sora', sans-serif;
    font-size: 13px; color: var(--nc-text); background: var(--nc-bg);
    outline: none; transition: border-color 0.15s; width: 100%;
  }
  input:focus, select:focus, textarea:focus { border-color: var(--nc-accent); background: #fff; }
  textarea { resize: vertical; min-height: 80px; }
  .radio-group { display: flex; gap: 10px; flex-wrap: wrap; }
  .radio-btn {
    display: flex; align-items: center; gap: 7px;
    padding: 8px 14px; border-radius: var(--radius-sm);
    border: 1.5px solid var(--nc-border); cursor: pointer;
    font-size: 12px; font-weight: 500; transition: all 0.15s;
    background: var(--nc-bg);
  }
  .radio-btn input { display: none; }
  .radio-btn.selected { border-color: var(--nc-accent); background: #EBF4FF; color: var(--nc-accent); }
  .radio-btn.selected-green { border-color: var(--nc-green); background: #D1FAE5; color: #065F46; }
  .radio-btn.selected-amber { border-color: var(--nc-amber); background: #FEF3C7; color: #92400E; }
  .section-divider { border: none; border-top: 1px solid var(--nc-border); margin: 20px 0; }

  /* ── NOTIFICATION PREVIEW ── */
  .notif-preview {
    background: #F8FAFC; border-radius: var(--radius-sm);
    border: 1px solid var(--nc-border); padding: 14px;
    font-size: 12px; color: var(--nc-muted); margin-top: 8px;
    font-family: 'JetBrains Mono', monospace; line-height: 1.7;
  }
  .notif-preview strong { color: var(--nc-text); font-weight: 600; }

  /* ── TIMELINE ── */
  .timeline { display: flex; flex-direction: column; gap: 0; }
  .tl-item { display: flex; gap: 16px; position: relative; }
  .tl-item:not(:last-child) .tl-line { position: absolute; left: 15px; top: 32px; bottom: 0; width: 1px; background: var(--nc-border); }
  .tl-dot {
    width: 30px; height: 30px; border-radius: 50%; flex-shrink: 0;
    display: flex; align-items: center; justify-content: center;
    font-size: 11px; font-weight: 700; color: #fff; margin-top: 4px; position: relative; z-index: 1;
  }
  .tl-content { flex: 1; padding-bottom: 20px; }
  .tl-title { font-size: 13px; font-weight: 600; margin-bottom: 2px; }
  .tl-time { font-size: 11px; color: var(--nc-muted); }
  .tl-body { font-size: 12px; color: var(--nc-muted); margin-top: 4px; background: #F8FAFC; border-radius: 6px; padding: 8px 10px; border: 1px solid var(--nc-border); }

  /* ── NOTIFICATION CENTER ── */
  .notif-list { display: flex; flex-direction: column; gap: 10px; }
  .notif-card {
    background: var(--nc-card); border: 1px solid var(--nc-border);
    border-radius: var(--radius); padding: 16px 18px;
    display: flex; gap: 14px; align-items: flex-start;
    box-shadow: var(--shadow);
  }
  .notif-card.unread { border-left: 3px solid var(--nc-accent); }
  .notif-icon {
    width: 36px; height: 36px; border-radius: 10px; flex-shrink: 0;
    display: flex; align-items: center; justify-content: center; font-size: 16px;
  }
  .ni-reminder { background: #DBEAFE; }
  .ni-form { background: #D1FAE5; }
  .ni-overdue { background: #FEE2E2; }
  .ni-completed { background: #EDE9FE; }
  .notif-text { flex: 1; }
  .notif-title { font-size: 13px; font-weight: 600; margin-bottom: 3px; }
  .notif-body { font-size: 12px; color: var(--nc-muted); line-height: 1.5; }
  .notif-time { font-size: 11px; color: var(--nc-muted); margin-top: 5px; }
  .notif-actions { display: flex; gap: 8px; margin-top: 10px; }

  /* ── TABS ── */
  .tabs { display: flex; border-bottom: 1px solid var(--nc-border); margin-bottom: 20px; gap: 0; }
  .tab-btn {
    padding: 10px 18px; font-size: 13px; font-weight: 500;
    color: var(--nc-muted); border: none; background: none;
    cursor: pointer; border-bottom: 2px solid transparent;
    margin-bottom: -1px; transition: all 0.15s; font-family: 'Sora', sans-serif;
  }
  .tab-btn.active { color: var(--nc-accent); border-bottom-color: var(--nc-accent); }
  .tab-btn:hover:not(.active) { color: var(--nc-text); }
  .tab-panel { display: none; }
  .tab-panel.active { display: block; }

  /* ── DETAIL CARDS ── */
  .detail-card {
    background: var(--nc-card); border: 1px solid var(--nc-border);
    border-radius: var(--radius); padding: 20px;
    box-shadow: var(--shadow); margin-bottom: 16px;
  }
  .detail-card-title { font-size: 13px; font-weight: 700; margin-bottom: 14px; display: flex; align-items: center; gap: 8px; }
  .dl-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; }
  .dl-item dt { font-size: 11px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.07em; color: var(--nc-muted); margin-bottom: 3px; }
  .dl-item dd { font-size: 13px; font-weight: 500; }

  /* ── ATS SYNC ── */
  .ats-banner {
    background: linear-gradient(135deg, var(--nc-navy), var(--nc-blue));
    border-radius: var(--radius); padding: 18px 22px;
    display: flex; align-items: center; gap: 16px; color: #fff;
    margin-bottom: 24px;
  }
  .ats-icon { font-size: 28px; flex-shrink: 0; }
  .ats-text { flex: 1; }
  .ats-title { font-size: 14px; font-weight: 700; margin-bottom: 3px; }
  .ats-sub { font-size: 12px; opacity: 0.7; }
  .ats-status {
    display: flex; align-items: center; gap: 6px;
    font-size: 12px; font-weight: 600; color: var(--nc-teal);
    background: rgba(0,197,161,0.12); padding: 6px 12px; border-radius: 20px;
  }
  .pulse {
    width: 8px; height: 8px; border-radius: 50%; background: var(--nc-teal);
    animation: pulse 2s infinite;
  }
  @keyframes pulse { 0%,100%{opacity:1;transform:scale(1)} 50%{opacity:0.5;transform:scale(0.85)} }

  /* ── EMPTY STATE ── */
  .empty { text-align: center; padding: 48px 24px; color: var(--nc-muted); }
  .empty-icon { font-size: 36px; margin-bottom: 12px; }
  .empty-text { font-size: 14px; font-weight: 500; margin-bottom: 6px; color: var(--nc-text); }
  .empty-sub { font-size: 12px; }

  /* ── TOAST ── */
  .toast-wrap { position: fixed; bottom: 24px; right: 24px; z-index: 9999; display: flex; flex-direction: column; gap: 10px; }
  .toast {
    background: var(--nc-navy); color: #fff;
    padding: 12px 18px; border-radius: var(--radius-sm); font-size: 13px;
    box-shadow: var(--shadow-lg); display: flex; align-items: center; gap: 10px;
    animation: slideIn 0.25s ease; max-width: 320px;
  }
  .toast.success { background: #065F46; border-left: 3px solid var(--nc-teal); }
  .toast.warn { background: #78350F; border-left: 3px solid var(--nc-amber); }
  .toast.info { background: #1E3A5F; border-left: 3px solid var(--nc-bright); }
  @keyframes slideIn { from { opacity: 0; transform: translateX(20px); } to { opacity: 1; transform: translateX(0); } }

  /* ── MISC ── */
  .tag { display: inline-flex; align-items: center; gap: 4px; padding: 2px 8px; border-radius: 4px; font-size: 10px; font-weight: 700; letter-spacing: 0.05em; text-transform: uppercase; background: var(--nc-bg); border: 1px solid var(--nc-border); color: var(--nc-muted); }
  .overdue-row { background: #FFF9F9 !important; }
  .overdue-row td:first-child { border-left: 3px solid var(--nc-red); }
  .search-wrap { position: relative; flex: 1; max-width: 240px; }
  .search-icon { position: absolute; left: 10px; top: 50%; transform: translateY(-50%); font-size: 14px; color: var(--nc-muted); pointer-events: none; }
  .search-input { padding-left: 32px !important; width: 100%; }
  .days-left { font-family: 'JetBrains Mono', monospace; font-size: 12px; }
  .days-left.urgent { color: var(--nc-red); font-weight: 700; }
  .days-left.warn { color: var(--nc-amber); font-weight: 700; }
  .days-left.ok { color: var(--nc-green); }

  /* ── COLLAPSIBLE NOTIF SECTION ── */
  .notif-section-header {
    display: flex; align-items: center; gap: 10px;
    margin: 20px 0 12px; font-size: 12px; font-weight: 700;
    text-transform: uppercase; letter-spacing: 0.08em; color: var(--nc-muted);
  }
  .notif-section-line { flex: 1; height: 1px; background: var(--nc-border); }

  .reminder-chip {
    background: #EFF6FF; border: 1px solid #BFDBFE; color: #1D4ED8;
    font-size: 11px; font-weight: 600; padding: 3px 10px; border-radius: 20px;
    display: inline-flex; align-items: center; gap: 5px;
  }
  .form-toggle {
    display: flex; align-items: center; justify-content: space-between;
    padding: 10px 14px; border-radius: var(--radius-sm);
    border: 1px solid var(--nc-border); background: var(--nc-bg);
    cursor: pointer; transition: border-color 0.15s;
    margin-bottom: 10px;
  }
  .form-toggle:hover { border-color: var(--nc-accent); }
  .toggle-switch {
    width: 36px; height: 20px; border-radius: 10px; background: var(--nc-border);
    position: relative; transition: background 0.2s; flex-shrink: 0;
  }
  .toggle-switch.on { background: var(--nc-green); }
  .toggle-switch::after {
    content: ''; position: absolute; width: 14px; height: 14px;
    background: #fff; border-radius: 50%; top: 3px; left: 3px; transition: left 0.2s;
  }
  .toggle-switch.on::after { left: 19px; }

  @media (max-width: 1024px) {
    .stats-row { grid-template-columns: repeat(2, 1fr); }
  }
</style>
</head>
<body>

<div class="layout">

<!-- ── SIDEBAR ── -->
<aside class="sidebar">
  <div class="sidebar-logo">
    <div class="logo-mark">Netcracker Technology</div>
    <div class="logo-title">Probation Tracker</div>
  </div>
  <nav class="sidebar-nav">
    <div class="nav-section">
      <div class="nav-label">Overview</div>
      <button class="nav-item active" onclick="navigate('dashboard')">
        <svg class="ni-icon" fill="none" stroke="currentColor" stroke-width="1.8" viewBox="0 0 24 24"><rect x="3" y="3" width="7" height="7" rx="1"/><rect x="14" y="3" width="7" height="7" rx="1"/><rect x="3" y="14" width="7" height="7" rx="1"/><rect x="14" y="14" width="7" height="7" rx="1"/></svg>
        Dashboard
      </button>
      <button class="nav-item" onclick="navigate('joiners')">
        <svg class="ni-icon" fill="none" stroke="currentColor" stroke-width="1.8" viewBox="0 0 24 24"><circle cx="9" cy="7" r="4"/><path d="M3 21v-2a4 4 0 0 1 4-4h4a4 4 0 0 1 4 4v2"/><path d="M16 3.13a4 4 0 0 1 0 7.75"/><path d="M21 21v-2a4 4 0 0 0-3-3.87"/></svg>
        New Joiners
        <span class="nav-badge">12</span>
      </button>
    </div>
    <div class="nav-section">
      <div class="nav-label">Actions</div>
      <button class="nav-item" onclick="navigate('notifications')">
        <svg class="ni-icon" fill="none" stroke="currentColor" stroke-width="1.8" viewBox="0 0 24 24"><path d="M18 8a6 6 0 0 0-12 0c0 7-3 9-3 9h18s-3-2-3-9"/><path d="M13.73 21a2 2 0 0 1-3.46 0"/></svg>
        Notifications
        <span class="nav-badge warn">5</span>
      </button>
      <button class="nav-item" onclick="navigate('forms')">
        <svg class="ni-icon" fill="none" stroke="currentColor" stroke-width="1.8" viewBox="0 0 24 24"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/><polyline points="14 2 14 8 20 8"/><line x1="16" y1="13" x2="8" y2="13"/><line x1="16" y1="17" x2="8" y2="17"/><polyline points="10 9 9 9 8 9"/></svg>
        Probation Forms
      </button>
      <button class="nav-item" onclick="navigate('ats')">
        <svg class="ni-icon" fill="none" stroke="currentColor" stroke-width="1.8" viewBox="0 0 24 24"><circle cx="12" cy="12" r="3"/><path d="M12 1v4M12 19v4M4.22 4.22l2.83 2.83M16.95 16.95l2.83 2.83M1 12h4M19 12h4M4.22 19.78l2.83-2.83M16.95 7.05l2.83-2.83"/></svg>
        ATS Sync
      </button>
    </div>
    <div class="nav-section">
      <div class="nav-label">Reports</div>
      <button class="nav-item" onclick="navigate('reports')">
        <svg class="ni-icon" fill="none" stroke="currentColor" stroke-width="1.8" viewBox="0 0 24 24"><line x1="18" y1="20" x2="18" y2="10"/><line x1="12" y1="20" x2="12" y2="4"/><line x1="6" y1="20" x2="6" y2="14"/></svg>
        Analytics & Reports
      </button>
    </div>
  </nav>
  <div class="sidebar-footer">
    <div class="user-pill">
      <div class="user-avatar">HR</div>
      <div>
        <div class="user-name">Priya Sharma</div>
        <div class="user-role">HR Recruiter · Admin</div>
      </div>
    </div>
  </div>
</aside>

<!-- ── MAIN ── -->
<main class="main">

<!-- TOPBAR -->
<div class="topbar">
  <div class="topbar-title" id="topbar-title">Dashboard</div>
  <div class="topbar-actions">
    <button class="btn btn-outline btn-sm" onclick="sendRemindersAll()">📨 Send All Reminders</button>
    <button class="btn btn-primary btn-sm" onclick="openModal('add-modal')">
      <svg width="13" height="13" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>
      Add New Joiner
    </button>
  </div>
</div>

<!-- ══════════════ DASHBOARD ══════════════ -->
<section class="page active" id="page-dashboard">
  <div class="stats-row">
    <div class="stat-card">
      <div class="stat-label">Total Tracked</div>
      <div class="stat-value">12</div>
      <div class="stat-sub"><span class="stat-dot" style="background:#3B9EFF"></span>Active probation periods</div>
    </div>
    <div class="stat-card">
      <div class="stat-label">Completed</div>
      <div class="stat-value" style="color:#10B981">6</div>
      <div class="stat-sub"><span class="stat-dot" style="background:#10B981"></span>Successfully cleared</div>
    </div>
    <div class="stat-card">
      <div class="stat-label">Extended</div>
      <div class="stat-value" style="color:#F59E0B">3</div>
      <div class="stat-sub"><span class="stat-dot" style="background:#F59E0B"></span>Awaiting reassessment</div>
    </div>
    <div class="stat-card">
      <div class="stat-label">Overdue / At Risk</div>
      <div class="stat-value" style="color:#EF4444">2</div>
      <div class="stat-sub"><span class="stat-dot" style="background:#EF4444"></span>Require immediate action</div>
    </div>
  </div>

  <div class="ats-banner">
    <div class="ats-icon">🔗</div>
    <div class="ats-text">
      <div class="ats-title">ATS Integration Active</div>
      <div class="ats-sub">Probation status updates sync automatically to your company ATS. Last synced 2 min ago.</div>
    </div>
    <div class="ats-status"><div class="pulse"></div>Live Sync</div>
  </div>

  <div class="section-header">
    <div>
      <div class="section-title">At-Risk & Overdue</div>
      <div class="section-sub">Joiners nearing 180-day deadline without a manager response</div>
    </div>
    <button class="btn btn-outline btn-sm" onclick="navigate('joiners')">View all →</button>
  </div>

  <div class="table-wrap" style="margin-bottom:28px">
    <table>
      <thead>
        <tr>
          <th>Employee</th>
          <th>Joining Date</th>
          <th>Days Left</th>
          <th>Hiring Manager</th>
          <th>Reminders Sent</th>
          <th>Status</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr class="overdue-row">
          <td>
            <div class="avatar-stack">
              <div class="avatar av-red">RK</div>
              <div><div class="avatar-name">Rahul Kapoor</div><div class="avatar-dept">Engineering · Java</div></div>
            </div>
          </td>
          <td>Nov 12, 2024</td>
          <td><span class="days-left urgent">OVERDUE</span></td>
          <td>Arjun Mehta</td>
          <td><span class="reminder-chip">📬 6 sent</span></td>
          <td><span class="badge badge-overdue">Overdue</span></td>
          <td><button class="btn btn-outline btn-sm" onclick="openFormModal('Rahul Kapoor', 'Arjun Mehta', 'Nov 12, 2024')">Fill Form</button></td>
        </tr>
        <tr class="overdue-row">
          <td>
            <div class="avatar-stack">
              <div class="avatar av-orange">ST</div>
              <div><div class="avatar-name">Sneha Tiwari</div><div class="avatar-dept">Product · Analytics</div></div>
            </div>
          </td>
          <td>Nov 18, 2024</td>
          <td><span class="days-left urgent">3 days</span></td>
          <td>Deepak Rao</td>
          <td><span class="reminder-chip">📬 4 sent</span></td>
          <td><span class="badge badge-overdue">At Risk</span></td>
          <td><button class="btn btn-outline btn-sm" onclick="openFormModal('Sneha Tiwari', 'Deepak Rao', 'Nov 18, 2024')">Fill Form</button></td>
        </tr>
      </tbody>
    </table>
  </div>

  <div class="section-header">
    <div>
      <div class="section-title">Upcoming Deadlines (Next 30 Days)</div>
    </div>
  </div>
  <div class="table-wrap">
    <table>
      <thead>
        <tr>
          <th>Employee</th>
          <th>Deadline</th>
          <th>Days Left</th>
          <th>Hiring Manager</th>
          <th>Referral</th>
          <th>Status</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><div class="avatar-stack"><div class="avatar av-purple">AP</div><div><div class="avatar-name">Anika Patel</div><div class="avatar-dept">Cloud · DevOps</div></div></div></td>
          <td>Jun 5, 2025</td>
          <td><span class="days-left warn">18 days</span></td>
          <td>Neha Joshi</td>
          <td><span class="badge badge-referral">👤 Referral</span></td>
          <td><span class="badge badge-active">Active</span></td>
        </tr>
        <tr>
          <td><div class="avatar-stack"><div class="avatar av-teal">MS</div><div><div class="avatar-name">Mohit Sharma</div><div class="avatar-dept">QA · Automation</div></div></div></td>
          <td>Jun 14, 2025</td>
          <td><span class="days-left warn">27 days</span></td>
          <td>Arjun Mehta</td>
          <td>—</td>
          <td><span class="badge badge-active">Active</span></td>
        </tr>
      </tbody>
    </table>
  </div>
</section>

<!-- ══════════════ JOINERS ══════════════ -->
<section class="page" id="page-joiners">
  <div class="filters">
    <span class="filter-label">Filter:</span>
    <span class="filter-chip chip-all" onclick="filterTable('all')">All (12)</span>
    <span class="filter-chip chip-active" onclick="filterTable('active')">Active (6)</span>
    <span class="filter-chip chip-completed" onclick="filterTable('completed')">Completed (3)</span>
    <span class="filter-chip chip-extended" onclick="filterTable('extended')">Extended (2)</span>
    <span class="filter-chip chip-overdue" onclick="filterTable('overdue')">Overdue (1)</span>
    <div style="flex:1"></div>
    <div class="search-wrap">
      <span class="search-icon">🔍</span>
      <input class="filter-input search-input" type="text" placeholder="Search name or manager…" oninput="searchTable(this.value)">
    </div>
    <select class="filter-select" onchange="filterByDept(this.value)">
      <option value="">All Departments</option>
      <option>Engineering</option>
      <option>Product</option>
      <option>Cloud</option>
      <option>QA</option>
      <option>HR</option>
      <option>Finance</option>
    </select>
  </div>

  <div class="table-wrap">
    <table id="joiners-table">
      <thead>
        <tr>
          <th>Employee</th>
          <th>Joining Date</th>
          <th>180-Day Deadline</th>
          <th>Progress</th>
          <th>Days Left</th>
          <th>Hiring Manager</th>
          <th>Referral By</th>
          <th>Reminders</th>
          <th>Status</th>
          <th></th>
        </tr>
      </thead>
      <tbody id="joiners-tbody"></tbody>
    </table>
  </div>
</section>

<!-- ══════════════ NOTIFICATIONS ══════════════ -->
<section class="page" id="page-notifications">
  <div class="tabs">
    <button class="tab-btn active" onclick="switchTab('pending')">Pending Actions</button>
    <button class="tab-btn" onclick="switchTab('sent')">Sent Log</button>
    <button class="tab-btn" onclick="switchTab('reminders')">Weekly Reminders</button>
  </div>

  <div class="tab-panel active" id="tab-pending">
    <div class="notif-section-header"><span>Overdue — Requires Immediate Action</span><div class="notif-section-line"></div></div>
    <div class="notif-list">
      <div class="notif-card unread">
        <div class="notif-icon ni-overdue">⚠️</div>
        <div class="notif-text">
          <div class="notif-title">Probation Form Overdue — Rahul Kapoor</div>
          <div class="notif-body">Hiring Manager <strong>Arjun Mehta</strong> has not submitted the probation assessment for <strong>Rahul Kapoor</strong> (Engineering · Java). Deadline was May 10, 2025. 6 weekly reminders have been sent.</div>
          <div class="notif-time">🕐 Overdue by 8 days</div>
          <div class="notif-actions">
            <button class="btn btn-primary btn-sm" onclick="openFormModal('Rahul Kapoor','Arjun Mehta','Nov 12, 2024'); showToast('Form opened for Rahul Kapoor','info')">📋 Open Form</button>
            <button class="btn btn-outline btn-sm" onclick="showToast('Escalation email sent to HRBP and senior management','warn')">📤 Escalate to HRBP</button>
          </div>
        </div>
      </div>
      <div class="notif-card unread">
        <div class="notif-icon ni-reminder">📬</div>
        <div class="notif-title">Weekly Reminder Due — Sneha Tiwari</div>
        <div class="notif-text">
          <div class="notif-body">Send weekly reminder to <strong>Deepak Rao</strong> for <strong>Sneha Tiwari</strong>'s probation form. Deadline in 3 days.</div>
          <div class="notif-time">📅 Due: Today</div>
          <div class="notif-actions">
            <button class="btn btn-primary btn-sm" onclick="showToast('Reminder sent to Deepak Rao','success')">Send Reminder</button>
          </div>
        </div>
      </div>
    </div>

    <div class="notif-section-header" style="margin-top:24px"><span>Upcoming This Week</span><div class="notif-section-line"></div></div>
    <div class="notif-list">
      <div class="notif-card">
        <div class="notif-icon ni-reminder">📬</div>
        <div class="notif-text">
          <div class="notif-title">Weekly Reminder — Anika Patel (18 days left)</div>
          <div class="notif-body">Schedule weekly reminder to <strong>Neha Joshi</strong> for Anika Patel's probation form. Note: Employee referral profile — <strong>Vikram Singh</strong> referred this candidate.</div>
          <div class="notif-time">📅 Scheduled: Mon, May 19</div>
          <div class="notif-actions">
            <button class="btn btn-outline btn-sm" onclick="showToast('Reminder scheduled for May 19','success')">Schedule</button>
            <button class="btn btn-outline btn-sm" onclick="showToast('Sent immediately to Neha Joshi','success')">Send Now</button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div class="tab-panel" id="tab-sent">
    <div class="notif-list">
      <div class="notif-card">
        <div class="notif-icon ni-completed">✅</div>
        <div class="notif-text">
          <div class="notif-title">Probation Completed — Priyanka Das</div>
          <div class="notif-body">3 notifications sent:<br>• <strong>Priyanka Das</strong>: "You have successfully completed the probation with Netcracker Technology."<br>• <strong>HR Recruiter & HRBP</strong>: Status report dispatched.<br>• ATS updated to "Probation Cleared".</div>
          <div class="notif-time">✔ Sent May 15, 2025 at 10:22 AM</div>
        </div>
      </div>
      <div class="notif-card">
        <div class="notif-icon ni-completed">🎉</div>
        <div class="notif-text">
          <div class="notif-title">Referral Notification Sent — Arjun Verma (referrer)</div>
          <div class="notif-body">"Thanks for referring <strong>Karan Malhotra</strong>, he has successfully completed the probation period with Netcracker Technology."</div>
          <div class="notif-time">✔ Sent May 8, 2025 at 9:15 AM</div>
        </div>
      </div>
      <div class="notif-card">
        <div class="notif-icon ni-reminder">📬</div>
        <div class="notif-text">
          <div class="notif-title">Weekly Reminder #4 — Rahul Kapoor (to Arjun Mehta)</div>
          <div class="notif-body">Subject: Action Required — Probation Assessment Pending for Rahul Kapoor<br>"Please complete the probation form for Rahul Kapoor before the deadline…"</div>
          <div class="notif-time">✔ Sent May 12, 2025 at 8:00 AM</div>
        </div>
      </div>
    </div>
  </div>

  <div class="tab-panel" id="tab-reminders">
    <div class="detail-card">
      <div class="detail-card-title">🔁 Automatic Weekly Reminder Configuration</div>
      <p style="font-size:13px;color:var(--nc-muted);margin-bottom:16px">Reminders are sent every Monday at 8:00 AM IST to the Hiring Manager until the probation form is submitted. Configure triggers below.</p>
      <div class="dl-grid">
        <div class="dl-item">
          <dt>Trigger — First Reminder</dt>
          <dd>Day 150 (30 days before deadline)</dd>
        </div>
        <div class="dl-item">
          <dt>Frequency</dt>
          <dd>Every 7 days until submitted</dd>
        </div>
        <div class="dl-item">
          <dt>Escalation Trigger</dt>
          <dd>After 3 unanswered reminders → HRBP notified</dd>
        </div>
        <div class="dl-item">
          <dt>Overdue Action</dt>
          <dd>After deadline → daily reminders + senior flag</dd>
        </div>
      </div>
      <hr class="section-divider">
      <div class="form-toggle" onclick="this.querySelector('.toggle-switch').classList.toggle('on')">
        <div>
          <div style="font-size:13px;font-weight:600">Send Day-150 Kickoff Email</div>
          <div style="font-size:11px;color:var(--nc-muted)">Notify Hiring Manager that the window is approaching</div>
        </div>
        <div class="toggle-switch on"></div>
      </div>
      <div class="form-toggle" onclick="this.querySelector('.toggle-switch').classList.toggle('on')">
        <div>
          <div style="font-size:13px;font-weight:600">Escalate After 3 Missed Reminders</div>
          <div style="font-size:11px;color:var(--nc-muted)">Notify HRBP and copy Head of HR</div>
        </div>
        <div class="toggle-switch on"></div>
      </div>
      <div class="form-toggle" onclick="this.querySelector('.toggle-switch').classList.toggle('on')">
        <div>
          <div style="font-size:13px;font-weight:600">CC HR Recruiter on All Reminders</div>
        </div>
        <div class="toggle-switch"></div>
      </div>
    </div>
  </div>
</section>

<!-- ══════════════ FORMS ══════════════ -->
<section class="page" id="page-forms">
  <div class="stats-row" style="grid-template-columns:repeat(3,1fr)">
    <div class="stat-card"><div class="stat-label">Forms Pending</div><div class="stat-value" style="color:#F59E0B">3</div></div>
    <div class="stat-card"><div class="stat-label">Submitted This Month</div><div class="stat-value" style="color:#10B981">5</div></div>
    <div class="stat-card"><div class="stat-label">Overdue Forms</div><div class="stat-value" style="color:#EF4444">1</div></div>
  </div>
  <div class="table-wrap">
    <table>
      <thead>
        <tr><th>Employee</th><th>Hiring Manager</th><th>Deadline</th><th>Form Status</th><th></th></tr>
      </thead>
      <tbody>
        <tr class="overdue-row">
          <td><div class="avatar-stack"><div class="avatar av-red">RK</div><div><div class="avatar-name">Rahul Kapoor</div><div class="avatar-dept">Engineering</div></div></div></td>
          <td>Arjun Mehta</td>
          <td>May 10, 2025</td>
          <td><span class="badge badge-overdue">Overdue</span></td>
          <td><button class="btn btn-primary btn-sm" onclick="openFormModal('Rahul Kapoor','Arjun Mehta','Nov 12, 2024')">Fill Form</button></td>
        </tr>
        <tr>
          <td><div class="avatar-stack"><div class="avatar av-orange">ST</div><div><div class="avatar-name">Sneha Tiwari</div><div class="avatar-dept">Product</div></div></div></td>
          <td>Deepak Rao</td>
          <td>May 21, 2025</td>
          <td><span class="badge badge-pending">Pending</span></td>
          <td><button class="btn btn-outline btn-sm" onclick="openFormModal('Sneha Tiwari','Deepak Rao','Nov 18, 2024')">Fill Form</button></td>
        </tr>
        <tr>
          <td><div class="avatar-stack"><div class="avatar av-purple">AP</div><div><div class="avatar-name">Anika Patel</div><div class="avatar-dept">Cloud</div></div></div></td>
          <td>Neha Joshi</td>
          <td>Jun 5, 2025</td>
          <td><span class="badge badge-active">Not Due Yet</span></td>
          <td><button class="btn btn-outline btn-sm" onclick="openFormModal('Anika Patel','Neha Joshi','Dec 7, 2024')">Fill Form</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</section>

<!-- ══════════════ ATS SYNC ══════════════ -->
<section class="page" id="page-ats">
  <div class="ats-banner">
    <div class="ats-icon">🔗</div>
    <div class="ats-text">
      <div class="ats-title">ATS Integration — Netcracker HireSync</div>
      <div class="ats-sub">All probation status changes are pushed to ATS in real time. Manual sync available below.</div>
    </div>
    <div class="ats-status"><div class="pulse"></div>Connected</div>
  </div>
  <div class="detail-card">
    <div class="detail-card-title">📋 Recent ATS Updates</div>
    <div class="table-wrap" style="box-shadow:none;border:none">
      <table>
        <thead><tr><th>Employee</th><th>Status Pushed</th><th>ATS Record ID</th><th>Synced At</th><th>By</th></tr></thead>
        <tbody>
          <tr><td>Priyanka Das</td><td><span class="badge badge-completed">Probation Cleared</span></td><td><span style="font-family:monospace;font-size:11px">ATS-2025-00182</span></td><td>May 15, 2025</td><td>Auto-sync</td></tr>
          <tr><td>Karan Malhotra</td><td><span class="badge badge-completed">Probation Cleared</span></td><td><span style="font-family:monospace;font-size:11px">ATS-2025-00174</span></td><td>May 8, 2025</td><td>Auto-sync</td></tr>
          <tr><td>Rohit Gupta</td><td><span class="badge badge-extended">Extended 30 Days</span></td><td><span style="font-family:monospace;font-size:11px">ATS-2025-00160</span></td><td>Apr 30, 2025</td><td>Priya Sharma</td></tr>
        </tbody>
      </table>
    </div>
  </div>
  <div class="detail-card">
    <div class="detail-card-title">⚙️ ATS Configuration</div>
    <div class="dl-grid">
      <div class="dl-item"><dt>ATS System</dt><dd>Netcracker HireSync v4.2</dd></div>
      <div class="dl-item"><dt>Sync Mode</dt><dd>Real-time webhook + daily batch</dd></div>
      <div class="dl-item"><dt>Fields Synced</dt><dd>Status, Extension Days, Completion Date, Notes</dd></div>
      <div class="dl-item"><dt>Auth Method</dt><dd>OAuth 2.0 · Token valid</dd></div>
    </div>
    <button class="btn btn-primary" style="margin-top:16px" onclick="showToast('Manual sync triggered — 12 records pushed to ATS','success')">🔄 Trigger Manual Sync</button>
  </div>
</section>

<!-- ══════════════ REPORTS ══════════════ -->
<section class="page" id="page-reports">
  <div class="stats-row">
    <div class="stat-card"><div class="stat-label">Pass Rate</div><div class="stat-value" style="color:#10B981">78%</div><div class="stat-sub">6 of 9 completed on time</div></div>
    <div class="stat-card"><div class="stat-label">Avg Days to Complete</div><div class="stat-value">162</div><div class="stat-sub">Out of 180 days</div></div>
    <div class="stat-card"><div class="stat-label">Referral Completion Rate</div><div class="stat-value" style="color:#8B5CF6">91%</div><div class="stat-sub">Higher than non-referral</div></div>
    <div class="stat-card"><div class="stat-label">Forms Overdue (YTD)</div><div class="stat-value" style="color:#EF4444">4</div><div class="stat-sub">Down from 9 last year</div></div>
  </div>
  <div class="detail-card">
    <div class="detail-card-title">📤 Export & Share Report</div>
    <p style="font-size:13px;color:var(--nc-muted);margin-bottom:16px">Send consolidated probation status report to HR Recruiter and HRBP.</p>
    <div class="form-grid">
      <div class="form-group">
        <label>HR Recruiter</label>
        <input type="email" value="priya.sharma@netcracker.com">
      </div>
      <div class="form-group">
        <label>HRBP</label>
        <input type="email" value="ritu.nair@netcracker.com">
      </div>
      <div class="form-group">
        <label>Period</label>
        <select><option>May 2025</option><option>Apr 2025</option><option>Q1 2025</option></select>
      </div>
      <div class="form-group">
        <label>Format</label>
        <select><option>PDF Summary</option><option>Excel Sheet</option><option>Email Digest</option></select>
      </div>
    </div>
    <button class="btn btn-primary" style="margin-top:16px" onclick="showToast('Report sent to Priya Sharma & Ritu Nair','success')">📨 Send Report</button>
  </div>
</section>

</main>
</div>

<!-- ══════════════ MODALS ══════════════ -->

<!-- ADD NEW JOINER -->
<div class="modal-overlay" id="add-modal">
  <div class="modal">
    <div class="modal-header">
      <div>
        <div class="modal-title">Add New Joiner</div>
        <div class="modal-sub">Register a new employee for probation tracking (180 days)</div>
      </div>
      <button class="close-btn" onclick="closeModal('add-modal')">✕</button>
    </div>
    <div class="modal-body">
      <div class="form-grid">
        <div class="form-group">
          <label>Full Name *</label>
          <input type="text" id="add-name" placeholder="e.g. Riya Nair">
        </div>
        <div class="form-group">
          <label>Employee ID *</label>
          <input type="text" id="add-empid" placeholder="EMP-XXXX">
        </div>
        <div class="form-group">
          <label>Department *</label>
          <select id="add-dept">
            <option value="">Select department</option>
            <option>Engineering</option><option>Product</option><option>Cloud</option>
            <option>QA</option><option>HR</option><option>Finance</option><option>Sales</option>
          </select>
        </div>
        <div class="form-group">
          <label>Designation</label>
          <input type="text" id="add-desig" placeholder="e.g. Senior Developer">
        </div>
        <div class="form-group">
          <label>Date of Joining *</label>
          <input type="date" id="add-doj">
        </div>
        <div class="form-group">
          <label>Official Email *</label>
          <input type="email" id="add-email" placeholder="employee@netcracker.com">
        </div>
        <div class="form-group">
          <label>Hiring Manager *</label>
          <select id="add-manager">
            <option value="">Select manager</option>
            <option>Arjun Mehta</option><option>Deepak Rao</option><option>Neha Joshi</option>
            <option>Priya Nair</option><option>Sunil Verma</option>
          </select>
        </div>
        <div class="form-group">
          <label>HR Recruiter *</label>
          <select id="add-recruiter">
            <option>Priya Sharma</option><option>Ankita Das</option><option>Rohan Singh</option>
          </select>
        </div>
        <div class="form-group">
          <label>HRBP *</label>
          <select id="add-hrbp">
            <option>Ritu Nair</option><option>Sonal Mehta</option>
          </select>
        </div>
        <div class="form-group">
          <label>Is Employee Referral?</label>
          <select id="add-referral" onchange="toggleReferralField(this.value)">
            <option value="no">No</option>
            <option value="yes">Yes — Employee Referral</option>
          </select>
        </div>
        <div class="form-group span-2" id="referrer-field" style="display:none">
          <label>Referred By (Employee Name & ID)</label>
          <input type="text" id="add-referrer" placeholder="e.g. Vikram Singh (EMP-0021)">
          <div class="form-hint">A congratulatory notification will be sent to this employee upon probation completion.</div>
        </div>
      </div>
    </div>
    <div class="modal-footer">
      <button class="btn btn-outline" onclick="closeModal('add-modal')">Cancel</button>
      <button class="btn btn-primary" onclick="addJoiner()">Add & Start Tracking</button>
    </div>
  </div>
</div>

<!-- PROBATION FORM MODAL -->
<div class="modal-overlay" id="form-modal">
  <div class="modal modal-lg">
    <div class="modal-header">
      <div>
        <div class="modal-title" id="form-modal-title">Probation Assessment Form</div>
        <div class="modal-sub" id="form-modal-sub">Complete this form to update the probation status</div>
      </div>
      <button class="close-btn" onclick="closeModal('form-modal')">✕</button>
    </div>
    <div class="modal-body">

      <!-- STATUS SELECTION -->
      <div class="form-group" style="margin-bottom:18px">
        <label>Probation Outcome *</label>
        <div class="radio-group" id="status-group">
          <label class="radio-btn selected-green" id="rb-completed" onclick="selectStatus('completed')">
            <input type="radio" name="status" value="completed"> ✅ Completed — Cleared
          </label>
          <label class="radio-btn" id="rb-extended" onclick="selectStatus('extended')">
            <input type="radio" name="status" value="extended"> 🔄 Extended
          </label>
        </div>
      </div>

      <!-- EXTENSION OPTIONS -->
      <div id="extension-opts" style="display:none;margin-bottom:18px">
        <label style="font-size:12px;font-weight:600;margin-bottom:8px;display:block">Extension Duration *</label>
        <div class="radio-group">
          <label class="radio-btn" id="ext-30" onclick="selectExt('30')"><input type="radio" name="ext" value="30"> 30 Days</label>
          <label class="radio-btn" id="ext-60" onclick="selectExt('60')"><input type="radio" name="ext" value="60"> 60 Days</label>
          <label class="radio-btn" id="ext-90" onclick="selectExt('90')"><input type="radio" name="ext" value="90"> 90 Days</label>
        </div>
      </div>

      <div class="form-grid">
        <div class="form-group">
          <label>Performance Rating</label>
          <select id="form-rating">
            <option>Exceeds Expectations</option>
            <option>Meets Expectations</option>
            <option>Below Expectations</option>
            <option>Needs Improvement</option>
          </select>
        </div>
        <div class="form-group">
          <label>Assessment Date</label>
          <input type="date" id="form-date">
        </div>
        <div class="form-group span-2">
          <label>Hiring Manager Comments</label>
          <textarea id="form-comments" placeholder="Summarize the employee's performance, areas of strength, and any concerns…"></textarea>
        </div>
        <div class="form-group span-2">
          <label>Goals Achieved During Probation</label>
          <textarea id="form-goals" placeholder="List key goals and whether they were met…" style="min-height:60px"></textarea>
        </div>
      </div>

      <hr class="section-divider">

      <!-- NOTIFICATION PREVIEW -->
      <div class="form-group">
        <label>📨 Notifications that will be triggered on submission</label>
        <div class="notif-preview" id="notif-preview">
          <strong>→ To Employee:</strong><br>
          "Congratulations! You have successfully completed the probation with Netcracker Technology."<br><br>
          <strong>→ To HR Recruiter & HRBP:</strong><br>
          Status report: [Employee Name] — Probation Completed on [Date]<br><br>
          <strong>→ ATS Update:</strong> Record will be updated to <strong>Probation Cleared</strong>
        </div>
      </div>

      <div id="referral-notif-preview" style="display:none;margin-top:10px">
        <div class="notif-preview" style="border-left:3px solid #8B5CF6">
          <strong>→ To Referrer (Employee):</strong><br>
          "Thanks for referring <strong id="referral-name-preview">[Candidate Name]</strong>, they have successfully completed the probation period with Netcracker Technology."
        </div>
      </div>

    </div>
    <div class="modal-footer">
      <button class="btn btn-outline" onclick="closeModal('form-modal')">Cancel</button>
      <button class="btn btn-primary" onclick="submitForm()">Submit & Send Notifications</button>
    </div>
  </div>
</div>

<!-- TOAST CONTAINER -->
<div class="toast-wrap" id="toast-wrap"></div>

<script>
// ────────────────────────────────────────────
// DATA
// ────────────────────────────────────────────
const today = new Date('2025-05-18');

const joiners = [
  { id:1, name:'Rahul Kapoor',   initials:'RK', color:'av-red',    dept:'Engineering',  desig:'Java Developer',     doj:'2024-11-12', manager:'Arjun Mehta',  recruiter:'Priya Sharma', hrbp:'Ritu Nair',    referral:false, referrer:'',             reminders:6, status:'overdue'   },
  { id:2, name:'Sneha Tiwari',   initials:'ST', color:'av-orange', dept:'Product',       desig:'Business Analyst',   doj:'2024-11-18', manager:'Deepak Rao',   recruiter:'Priya Sharma', hrbp:'Ritu Nair',    referral:false, referrer:'',             reminders:4, status:'active'    },
  { id:3, name:'Anika Patel',    initials:'AP', color:'av-purple', dept:'Cloud',         desig:'DevOps Engineer',    doj:'2024-12-07', manager:'Neha Joshi',   recruiter:'Ankita Das',   hrbp:'Sonal Mehta',  referral:true,  referrer:'Vikram Singh', reminders:2, status:'active'    },
  { id:4, name:'Mohit Sharma',   initials:'MS', color:'av-teal',   dept:'QA',            desig:'QA Automation Engr', doj:'2024-12-14', manager:'Arjun Mehta',  recruiter:'Priya Sharma', hrbp:'Ritu Nair',    referral:false, referrer:'',             reminders:1, status:'active'    },
  { id:5, name:'Priyanka Das',   initials:'PD', color:'av-green',  dept:'Engineering',   desig:'Full Stack Dev',     doj:'2024-11-15', manager:'Sunil Verma',  recruiter:'Rohan Singh',  hrbp:'Ritu Nair',    referral:false, referrer:'',             reminders:0, status:'completed' },
  { id:6, name:'Karan Malhotra', initials:'KM', color:'av-blue',   dept:'Cloud',         desig:'Cloud Architect',    doj:'2024-11-08', manager:'Deepak Rao',   recruiter:'Priya Sharma', hrbp:'Sonal Mehta',  referral:true,  referrer:'Arjun Verma',  reminders:0, status:'completed' },
  { id:7, name:'Rohit Gupta',    initials:'RG', color:'av-amber',  dept:'Finance',       desig:'Financial Analyst',  doj:'2024-12-01', manager:'Priya Nair',   recruiter:'Ankita Das',   hrbp:'Ritu Nair',    referral:false, referrer:'',             reminders:3, status:'extended'  },
  { id:8, name:'Divya Menon',    initials:'DM', color:'av-rose',   dept:'HR',            desig:'HR Generalist',      doj:'2024-12-20', manager:'Sunil Verma',  recruiter:'Priya Sharma', hrbp:'Sonal Mehta',  referral:true,  referrer:'Neha Joshi',   reminders:1, status:'active'    },
  { id:9, name:'Aman Tiwari',    initials:'AT', color:'av-navy',   dept:'Engineering',   desig:'Backend Developer',  doj:'2025-01-06', manager:'Arjun Mehta',  recruiter:'Rohan Singh',  hrbp:'Ritu Nair',    referral:false, referrer:'',             reminders:0, status:'active'    },
  { id:10,name:'Sana Sheikh',    initials:'SS', color:'av-teal',   dept:'Product',       desig:'Product Manager',    doj:'2025-01-13', manager:'Neha Joshi',   recruiter:'Ankita Das',   hrbp:'Sonal Mehta',  referral:false, referrer:'',             reminders:0, status:'active'    },
  { id:11,name:'Vikas Nair',     initials:'VN', color:'av-purple', dept:'QA',            desig:'Manual Tester',      doj:'2024-11-25', manager:'Deepak Rao',   recruiter:'Priya Sharma', hrbp:'Ritu Nair',    referral:false, referrer:'',             reminders:0, status:'extended'  },
  { id:12,name:'Pooja Rao',      initials:'PR', color:'av-green',  dept:'Engineering',   desig:'DevOps Engineer',    doj:'2024-10-20', manager:'Sunil Verma',  recruiter:'Rohan Singh',  hrbp:'Sonal Mehta',  referral:true,  referrer:'Karan Bhat',   reminders:0, status:'completed' },
];

// ────────────────────────────────────────────
// HELPERS
// ────────────────────────────────────────────
function daysFromJoining(doj) {
  const d = new Date(doj);
  return Math.floor((today - d) / 86400000);
}
function deadline(doj) {
  const d = new Date(doj);
  d.setDate(d.getDate() + 180);
  return d;
}
function daysLeft(doj) {
  const dl = deadline(doj);
  return Math.floor((dl - today) / 86400000);
}
function formatDate(d) {
  return new Date(d).toLocaleDateString('en-IN', { day:'numeric', month:'short', year:'numeric' });
}
function progressPct(doj) {
  const days = daysFromJoining(doj);
  return Math.min(100, Math.round((days / 180) * 100));
}
function progressColor(status, pct) {
  if (status === 'completed') return '#10B981';
  if (status === 'extended')  return '#F59E0B';
  if (status === 'overdue')   return '#EF4444';
  if (pct >= 90) return '#EF4444';
  if (pct >= 80) return '#F59E0B';
  return '#3B9EFF';
}
function daysLeftHtml(j) {
  if (j.status === 'completed') return '<span class="badge badge-completed">Cleared</span>';
  if (j.status === 'extended')  return '<span class="badge badge-extended">Extended</span>';
  const dl = daysLeft(j.doj);
  if (dl < 0) return '<span class="days-left urgent">OVERDUE</span>';
  if (dl <= 7) return `<span class="days-left urgent">${dl}d</span>`;
  if (dl <= 30) return `<span class="days-left warn">${dl}d</span>`;
  return `<span class="days-left ok">${dl}d</span>`;
}
function statusBadge(s) {
  const m = { active:'badge-active', completed:'badge-completed', extended:'badge-extended', overdue:'badge-overdue' };
  const l = { active:'Active', completed:'Completed', extended:'Extended', overdue:'Overdue' };
  return `<span class="badge ${m[s]}">${l[s]}</span>`;
}

// ────────────────────────────────────────────
// RENDER JOINERS TABLE
// ────────────────────────────────────────────
let currentFilter = 'all';
let currentSearch = '';

function renderJoiners(list) {
  const tbody = document.getElementById('joiners-tbody');
  if (!list.length) {
    tbody.innerHTML = `<tr><td colspan="10"><div class="empty"><div class="empty-icon">🔍</div><div class="empty-text">No records found</div><div class="empty-sub">Try a different filter or search</div></div></td></tr>`;
    return;
  }
  tbody.innerHTML = list.map(j => {
    const pct = progressPct(j.doj);
    const pc  = progressColor(j.status, pct);
    const isOverdue = j.status === 'overdue' || (daysLeft(j.doj) < 0 && j.status === 'active');
    return `<tr ${isOverdue ? 'class="overdue-row"' : ''}>
      <td><div class="avatar-stack">
        <div class="avatar ${j.color}">${j.initials}</div>
        <div><div class="avatar-name">${j.name}</div><div class="avatar-dept">${j.dept} · ${j.desig}</div></div>
      </div></td>
      <td>${formatDate(j.doj)}</td>
      <td>${formatDate(deadline(j.doj))}</td>
      <td>
        <div class="progress-wrap">
          <div class="progress-bar"><div class="progress-fill" style="width:${pct}%;background:${pc}"></div></div>
          <span class="progress-label">${pct}%</span>
        </div>
      </td>
      <td>${daysLeftHtml(j)}</td>
      <td style="font-size:12px">${j.manager}</td>
      <td>${j.referral ? `<span class="badge badge-referral">👤 ${j.referrer}</span>` : '—'}</td>
      <td>${j.reminders > 0 ? `<span class="reminder-chip">📬 ${j.reminders}</span>` : '<span style="color:var(--nc-muted);font-size:11px">None</span>'}</td>
      <td>${statusBadge(j.status)}</td>
      <td><button class="btn btn-outline btn-sm" onclick="openFormModal('${j.name}','${j.manager}','${j.doj}'${j.referral ? ",'"+j.referrer+"'" : ''})">Assess</button></td>
    </tr>`;
  }).join('');
}

function getFilteredList() {
  return joiners.filter(j => {
    const matchFilter = currentFilter === 'all' || j.status === currentFilter;
    const matchSearch = !currentSearch || j.name.toLowerCase().includes(currentSearch) || j.manager.toLowerCase().includes(currentSearch);
    return matchFilter && matchSearch;
  });
}

function filterTable(f) {
  currentFilter = f;
  renderJoiners(getFilteredList());
}
function searchTable(s) {
  currentSearch = s.toLowerCase();
  renderJoiners(getFilteredList());
}
function filterByDept(dept) {
  renderJoiners(dept ? joiners.filter(j => j.dept === dept) : getFilteredList());
}

// ────────────────────────────────────────────
// NAVIGATION
// ────────────────────────────────────────────
const titles = { dashboard:'Dashboard', joiners:'New Joiners', notifications:'Notification Center', forms:'Probation Forms', ats:'ATS Sync', reports:'Analytics & Reports' };

function navigate(page) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
  document.getElementById('page-' + page).classList.add('active');
  document.querySelector(`[onclick="navigate('${page}')"]`).classList.add('active');
  document.getElementById('topbar-title').textContent = titles[page] || page;
  if (page === 'joiners') renderJoiners(getFilteredList());
}

// ────────────────────────────────────────────
// MODALS
// ────────────────────────────────────────────
function openModal(id) { document.getElementById(id).classList.add('open'); }
function closeModal(id) { document.getElementById(id).classList.remove('open'); }

document.querySelectorAll('.modal-overlay').forEach(o => {
  o.addEventListener('click', e => { if (e.target === o) o.classList.remove('open'); });
});

// ────────────────────────────────────────────
// PROBATION FORM
// ────────────────────────────────────────────
let formEmployee = '', formReferrer = '';

function openFormModal(empName, manager, doj, referrer='') {
  formEmployee = empName;
  formReferrer = referrer;
  document.getElementById('form-modal-title').textContent = `Probation Assessment — ${empName}`;
  document.getElementById('form-modal-sub').textContent = `Hiring Manager: ${manager} · Joining Date: formatDate(doj)`;
  document.getElementById('form-date').value = new Date().toISOString().split('T')[0];
  selectStatus('completed');
  // Show referral preview
  const rp = document.getElementById('referral-notif-preview');
  if (referrer) {
    rp.style.display = 'block';
    document.getElementById('referral-name-preview').textContent = empName;
  } else {
    rp.style.display = 'none';
  }
  openModal('form-modal');
}

function selectStatus(s) {
  document.querySelectorAll('#status-group .radio-btn').forEach(b => b.classList.remove('selected','selected-green','selected-amber'));
  const opts = document.getElementById('extension-opts');
  if (s === 'completed') {
    document.getElementById('rb-completed').classList.add('selected-green');
    opts.style.display = 'none';
    updateNotifPreview('completed');
  } else {
    document.getElementById('rb-extended').classList.add('selected-amber');
    opts.style.display = 'block';
    updateNotifPreview('extended');
  }
}

function selectExt(days) {
  ['30','60','90'].forEach(d => {
    const el = document.getElementById('ext-'+d);
    el.classList.remove('selected','selected-amber');
    if (d === days) el.classList.add('selected-amber');
  });
  updateNotifPreview('extended', days);
}

function updateNotifPreview(status, extDays='') {
  const preview = document.getElementById('notif-preview');
  if (status === 'completed') {
    preview.innerHTML = `<strong>→ To Employee (${formEmployee || '[Employee]'}):</strong><br>"Congratulations! You have successfully completed the probation with Netcracker Technology."<br><br><strong>→ To HR Recruiter & HRBP:</strong><br>Status Report: ${formEmployee || '[Employee]'} — Probation <span style="color:#065F46;font-weight:700">COMPLETED</span> on [Assessment Date].<br><br><strong>→ ATS Update:</strong> Record → <strong style="color:#065F46">Probation Cleared</strong>`;
  } else {
    const newDl = extDays ? ` New deadline: +${extDays} days from today.` : '';
    preview.innerHTML = `<strong>→ To Employee (${formEmployee || '[Employee]'}):</strong><br>"Your probation period has been extended by ${extDays || '[X]'} days.${newDl}"<br><br><strong>→ To HR Recruiter & HRBP:</strong><br>Status Report: ${formEmployee || '[Employee]'} — Probation <span style="color:#92400E;font-weight:700">EXTENDED ${extDays || ''}d</span>.<br><br><strong>→ ATS Update:</strong> Record → <strong style="color:#92400E">Probation Extended ${extDays || ''}d</strong>`;
  }
}

function submitForm() {
  // Determine selected status
  const isCompleted = document.getElementById('rb-completed').classList.contains('selected-green');
  const newStatus   = isCompleted ? 'completed' : 'extended';

  // Find extension days if extended
  let extDays = '';
  if (!isCompleted) {
    ['30','60','90'].forEach(d => {
      if (document.getElementById('ext-'+d).classList.contains('selected-amber')) extDays = d;
    });
    if (!extDays) { showToast('⚠️ Please select an extension duration', 'warn'); return; }
  }

  // Update joiner in array
  const idx = joiners.findIndex(j => j.name === formEmployee);
  if (idx !== -1) {
    joiners[idx].status = newStatus;
    if (newStatus === 'extended' && extDays) joiners[idx].extDays = parseInt(extDays);
    joiners[idx].assessedOn = new Date().toISOString().split('T')[0];
    saveJoiners();
    updateStatCards();
    renderJoiners(getFilteredList());
  }

  showToast('✅ Probation status updated and notifications dispatched!', 'success');
  setTimeout(() => showToast('📨 Email sent to employee, HR Recruiter & HRBP', 'info'), 1200);
  if (formReferrer && newStatus === 'completed') setTimeout(() => showToast(`🎉 Referral notification sent to ${formReferrer}`, 'success'), 2400);
  setTimeout(() => showToast('🔗 ATS record updated successfully', 'info'), 3200);
  closeModal('form-modal');
}

// ────────────────────────────────────────────
// REFERRAL TOGGLE
// ────────────────────────────────────────────
function toggleReferralField(v) {
  document.getElementById('referrer-field').style.display = v === 'yes' ? 'block' : 'none';
}

// ────────────────────────────────────────────
// PERSISTENCE — localStorage
// ────────────────────────────────────────────
const STORAGE_KEY = 'nc_probation_joiners_v1';

function saveJoiners() {
  try { localStorage.setItem(STORAGE_KEY, JSON.stringify(joiners)); } catch(e) {}
}

function loadSavedJoiners() {
  try {
    const saved = localStorage.getItem(STORAGE_KEY);
    if (saved) {
      const parsed = JSON.parse(saved);
      joiners.length = 0;
      parsed.forEach(j => joiners.push(j));
    }
  } catch(e) {}
}

// ────────────────────────────────────────────
// ADD JOINER
// ────────────────────────────────────────────
const AVATAR_COLORS = ['av-blue','av-teal','av-purple','av-amber','av-rose','av-green','av-navy','av-orange','av-red'];

function addJoiner() {
  const name     = document.getElementById('add-name').value.trim();
  const empid    = document.getElementById('add-empid').value.trim();
  const dept     = document.getElementById('add-dept').value;
  const desig    = document.getElementById('add-desig').value.trim() || 'Employee';
  const doj      = document.getElementById('add-doj').value;
  const email    = document.getElementById('add-email').value.trim();
  const manager  = document.getElementById('add-manager').value;
  const recruiter= document.getElementById('add-recruiter').value;
  const hrbp     = document.getElementById('add-hrbp').value;
  const isRef    = document.getElementById('add-referral').value === 'yes';
  const referrer = isRef ? document.getElementById('add-referrer').value.trim() : '';

  // Validation
  if (!name)    { showToast('⚠️ Employee name is required', 'warn'); return; }
  if (!doj)     { showToast('⚠️ Date of joining is required', 'warn'); return; }
  if (!dept)    { showToast('⚠️ Please select a department', 'warn'); return; }
  if (!manager) { showToast('⚠️ Please select a hiring manager', 'warn'); return; }
  if (isRef && !referrer) { showToast('⚠️ Please enter the referrer name', 'warn'); return; }

  // Build initials
  const parts = name.split(' ').filter(Boolean);
  const initials = (parts[0]?.[0] || '') + (parts[1]?.[0] || parts[0]?.[1] || '');

  // Pick color based on name hash
  const colorIndex = name.split('').reduce((a, c) => a + c.charCodeAt(0), 0) % AVATAR_COLORS.length;

  const newJoiner = {
    id       : Date.now(),
    name, initials: initials.toUpperCase(),
    color    : AVATAR_COLORS[colorIndex],
    dept, desig,
    doj, email,
    manager, recruiter, hrbp,
    referral : isRef,
    referrer,
    reminders: 0,
    status   : 'active'
  };

  joiners.unshift(newJoiner); // add to top
  saveJoiners();
  updateStatCards();

  // Reset form
  ['add-name','add-empid','add-desig','add-email','add-referrer'].forEach(id => {
    document.getElementById(id).value = '';
  });
  document.getElementById('add-dept').value = '';
  document.getElementById('add-manager').value = '';
  document.getElementById('add-doj').value = '';
  document.getElementById('add-referral').value = 'no';
  document.getElementById('referrer-field').style.display = 'none';

  closeModal('add-modal');

  // Navigate to joiners tab and show new entry
  navigate('joiners');
  showToast(`✅ ${name} added! 180-day tracking started.`, 'success');
  setTimeout(() => showToast('📬 Reminder scheduled for Day 150', 'info'), 1000);
}

// ────────────────────────────────────────────
// UPDATE STAT CARDS DYNAMICALLY
// ────────────────────────────────────────────
function updateStatCards() {
  const total     = joiners.length;
  const completed = joiners.filter(j => j.status === 'completed').length;
  const extended  = joiners.filter(j => j.status === 'extended').length;
  const overdue   = joiners.filter(j => j.status === 'overdue' || (j.status === 'active' && daysLeft(j.doj) < 0)).length;

  const cards = document.querySelectorAll('#page-dashboard .stat-card .stat-value');
  if (cards[0]) cards[0].textContent = total;
  if (cards[1]) cards[1].textContent = completed;
  if (cards[2]) cards[2].textContent = extended;
  if (cards[3]) cards[3].textContent = overdue;

  // Update sidebar badge
  const badge = document.querySelector('[onclick="navigate(\'joiners\')"] .nav-badge');
  if (badge) badge.textContent = total;
}

// ────────────────────────────────────────────
// TABS
// ────────────────────────────────────────────
function switchTab(id) {
  document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
  document.querySelectorAll('.tab-panel').forEach(p => p.classList.remove('active'));
  document.querySelector(`[onclick="switchTab('${id}')"]`).classList.add('active');
  document.getElementById('tab-'+id).classList.add('active');
}

// ────────────────────────────────────────────
// TOAST
// ────────────────────────────────────────────
function showToast(msg, type='info') {
  const wrap = document.getElementById('toast-wrap');
  const t = document.createElement('div');
  t.className = `toast ${type}`;
  t.textContent = msg;
  wrap.appendChild(t);
  setTimeout(() => t.remove(), 4000);
}

function sendRemindersAll() {
  showToast('📬 Weekly reminders sent to 3 hiring managers', 'success');
}

// ────────────────────────────────────────────
// INIT
// ────────────────────────────────────────────
loadSavedJoiners();
renderJoiners(joiners);
updateStatCards();
</script>
</body>
</html>
