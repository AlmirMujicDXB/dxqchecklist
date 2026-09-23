<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
  <meta name="mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
  <meta name="apple-mobile-web-app-title" content="DXQ Checklist">
  <meta name="theme-color" content="#0f2b46">
  <meta name="description" content="DXQ Workshop Technician daily checklist with Excel export and WhatsApp sharing.">
  <link rel="manifest" href="./manifest.json">
  <link rel="apple-touch-icon" href="./icon-192.png">
  <link rel="icon" type="image/png" sizes="192x192" href="./icon-192.png">
  <link rel="icon" type="image/png" sizes="512x512" href="./icon-512.png">
  <title>DXQ Workshop Technician Checklist</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
    html { -webkit-text-size-adjust: 100%; text-size-adjust: 100%; }
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Inter', 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
      background: #eef2f7;
      background-image:
        radial-gradient(circle at 20% 10%, rgba(31, 110, 156, 0.06), transparent 40%),
        radial-gradient(circle at 90% 90%, rgba(15, 43, 70, 0.05), transparent 45%);
      color: #0f2b46;
      min-height: 100vh;
      padding: 8px 6px calc(30px + env(safe-area-inset-bottom));
      line-height: 1.4;
      -webkit-font-smoothing: antialiased;
      overscroll-behavior-y: contain;
    }
    .card {
      max-width: 100%; margin: 0 auto; background: #ffffff; border-radius: 16px;
      box-shadow: 0 1px 2px rgba(15,43,70,0.04), 0 8px 24px rgba(15,43,70,0.08);
      overflow: hidden; border: 1px solid rgba(15,43,70,0.06);
    }

    /* HEADER */
    .header {
      background: linear-gradient(135deg, #0f2b46 0%, #1a4b6b 55%, #1f6e9c 100%);
      color: #ffffff; padding: 16px 16px 14px; position: relative; overflow: hidden;
    }
    .header::after {
      content: ""; position: absolute; top: -60%; right: -10%; width: 200px; height: 200px;
      background: radial-gradient(circle, rgba(255,255,255,0.10), transparent 65%); pointer-events: none;
    }
    .header-top {
      display: flex; align-items: center; justify-content: space-between; gap: 10px;
      margin-bottom: 6px; position: relative; z-index: 1;
    }
    .brand {
      display: flex; align-items: center; gap: 8px; font-size: 0.66rem; font-weight: 600;
      letter-spacing: 0.12em; text-transform: uppercase; color: rgba(255,255,255,0.75);
    }
    .brand-mark {
      display: inline-flex; width: 24px; height: 24px; align-items: center; justify-content: center;
      border-radius: 6px; background: rgba(255,255,255,0.14); border: 1px solid rgba(255,255,255,0.22);
      font-size: 0.65rem; font-weight: 800; letter-spacing: 0.02em;
    }
    .badge-live {
      display: inline-flex; align-items: center; gap: 5px; font-size: 0.62rem; font-weight: 600;
      letter-spacing: 0.06em; text-transform: uppercase; padding: 4px 9px; border-radius: 999px;
      background: rgba(37, 211, 102, 0.16); color: #7ef0a8; border: 1px solid rgba(37, 211, 102, 0.3);
    }
    .badge-live .dot {
      width: 5px; height: 5px; border-radius: 50%; background: #25d366;
      box-shadow: 0 0 0 3px rgba(37, 211, 102, 0.22); animation: pulse 1.8s infinite;
    }
    @keyframes pulse {
      0%, 100% { box-shadow: 0 0 0 3px rgba(37, 211, 102, 0.22); }
      50% { box-shadow: 0 0 0 6px rgba(37, 211, 102, 0.08); }
    }
    .header h1 {
      font-size: 1.15rem; font-weight: 700; letter-spacing: -0.02em;
      position: relative; z-index: 1; line-height: 1.2;
    }
    .header p {
      font-size: 0.76rem; color: rgba(255,255,255,0.72); margin-top: 3px;
      position: relative; z-index: 1;
    }

    /* PROFILE STRIP */
    .profile-strip {
      background: linear-gradient(135deg, #f0fbf4 0%, #dcf6e5 100%);
      border-bottom: 1px solid #b3e2c1;
      padding: 12px 16px;
      display: flex; align-items: center; gap: 12px;
    }
    .profile-avatar {
      width: 46px; height: 46px; border-radius: 50%;
      background: linear-gradient(135deg, #1f6e9c 0%, #0f2b46 100%);
      color: #ffffff;
      display: flex; align-items: center; justify-content: center;
      font-size: 1.1rem; font-weight: 800; letter-spacing: -0.02em;
      flex-shrink: 0;
      box-shadow: 0 4px 12px rgba(31, 110, 156, 0.28);
      border: 2px solid #ffffff;
    }
    .profile-info { display: flex; flex-direction: column; gap: 1px; min-width: 0; flex: 1; }
    .profile-info .lbl {
      font-size: 0.6rem; font-weight: 700; letter-spacing: 0.12em;
      text-transform: uppercase; color: #0b6b3b;
    }
    .profile-info .name {
      font-size: 1.05rem; font-weight: 800; color: #0f2b46;
      letter-spacing: -0.01em;
    }
    .profile-badge {
      font-size: 0.6rem; font-weight: 700; letter-spacing: 0.06em;
      text-transform: uppercase; color: #0b6b3b;
      background: #ffffff; border: 1px solid #b3e2c1;
      padding: 3px 9px; border-radius: 999px;
      white-space: nowrap;
    }

    /* LIVE DATE */
    .live-date {
      padding: 10px 16px; background: linear-gradient(135deg, #eaf4fd 0%, #ddebfa 100%);
      border-bottom: 1px solid #cfe3f5; display: flex; align-items: center;
      justify-content: space-between; gap: 10px; flex-wrap: wrap;
    }
    .live-date .left { display: flex; flex-direction: column; gap: 1px; }
    .live-date .lbl {
      font-size: 0.58rem; font-weight: 700; letter-spacing: 0.12em;
      text-transform: uppercase; color: #5b7391;
    }
    .live-date .dt { font-size: 0.9rem; font-weight: 700; color: #0f2b46; }
    .live-date .time {
      font-size: 0.78rem; font-weight: 600; color: #1f6e9c; background: #ffffff;
      padding: 4px 10px; border-radius: 999px; border: 1px solid #d6e4f1;
      font-variant-numeric: tabular-nums;
    }

    /* TODAY CARD */
    .today-card {
      margin: 10px 12px 0; padding: 11px 13px; border-radius: 12px;
      background: linear-gradient(135deg, #f5fbf6 0%, #e8f8ee 100%);
      border: 1.5px solid #b3e2c1; display: flex; flex-wrap: wrap;
      align-items: center; justify-content: space-between; gap: 8px;
    }
    .today-card .info { display: flex; flex-direction: column; gap: 1px; }
    .today-card .info .t1 {
      font-size: 0.62rem; font-weight: 700; letter-spacing: 0.1em;
      text-transform: uppercase; color: #0b6b3b;
    }
    .today-card .info .t2 { font-size: 0.84rem; font-weight: 600; color: #0f2b46; }
    .today-card .prog {
      font-size: 0.78rem; font-weight: 700; color: #0b6b3b; background: #ffffff;
      padding: 5px 12px; border-radius: 999px; border: 1px solid #b3e2c1;
      font-variant-numeric: tabular-nums;
    }
    .today-card .prog.full { color: #ffffff; background: #0b6b3b; border-color: #0b6b3b; }

    /* MENU */
    .menu {
      padding: 10px 12px; background: #f7fafd; border-bottom: 1px solid #e2eaf2;
      display: flex; flex-wrap: wrap; gap: 8px; align-items: flex-end;
    }
    .menu .field {
      flex: 1 1 100px; min-width: 90px; background: #ffffff;
      border: 1px solid #dde7f0; border-radius: 10px; padding: 8px 10px;
      transition: border-color 0.15s;
    }
    .menu .field:focus-within {
      border-color: #1f6e9c; box-shadow: 0 0 0 3px rgba(31, 110, 156, 0.1);
    }
    .menu .field label {
      display: block; font-size: 0.58rem; font-weight: 700; letter-spacing: 0.1em;
      text-transform: uppercase; color: #5b7391; margin-bottom: 2px;
    }
    .menu .field select {
      width: 100%; padding: 2px 0; border: none; background: transparent;
      font-size: 16px; font-weight: 600; color: #0f2b46; outline: none;
      font-family: inherit; -webkit-appearance: none; appearance: none; cursor: pointer;
      background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%235b7391' stroke-width='2.2' stroke-linecap='round' stroke-linejoin='round'%3e%3cpolyline points='6 9 12 15 18 9'%3e%3c/polyline%3e%3c/svg%3e");
      background-repeat: no-repeat; background-position: right 2px center;
      background-size: 12px; padding-right: 18px;
    }
    .menu .field.tech-field {
      border-color: #b3e2c1;
      background: linear-gradient(135deg, #f6fdf9 0%, #eefaf2 100%);
    }
    .menu .field.tech-field label { color: #0b6b3b; }
    .menu .field.tech-field select { color: #0b6b3b; font-weight: 800; }

    .saved-indicator {
      font-size: 0.66rem; font-weight: 600; color: #0b6b3b;
      display: inline-flex; align-items: center; gap: 5px; opacity: 0;
      transition: opacity 0.3s; margin-left: auto; padding-bottom: 6px;
    }
    .saved-indicator.show { opacity: 1; }
    .saved-indicator::before {
      content: ""; width: 5px; height: 5px; border-radius: 50%; background: #25b46a;
    }

    /* BACKUP PANEL */
    .backup-panel {
      padding: 10px 12px; background: #f0f7fd;
      border-bottom: 1px solid #d3e6f5;
      display: flex; flex-wrap: wrap; align-items: center; gap: 10px;
    }
    .backup-panel .bp-left {
      display: flex; align-items: center; gap: 9px; flex: 1 1 100%;
    }
    .backup-panel .bp-icon {
      width: 30px; height: 30px; border-radius: 8px;
      background: #1f6e9c; color: #ffffff;
      display: flex; align-items: center; justify-content: center;
      font-size: 0.65rem; font-weight: 800; flex-shrink: 0;
    }
    .backup-panel .bp-info { display: flex; flex-direction: column; gap: 1px; min-width: 0; }
    .backup-panel .bp-info .t1 {
      font-size: 0.62rem; font-weight: 700; letter-spacing: 0.1em;
      text-transform: uppercase; color: #1f6e9c;
    }
    .backup-panel .bp-info .t2 { font-size: 0.72rem; color: #5b7391; word-break: break-word; }
    .backup-panel .bp-info .t2 strong { color: #0f2b46; font-weight: 700; }

    /* HINT */
    .hint {
      padding: 8px 12px; background: #eaf4fd; color: #1f6e9c;
      font-size: 0.72rem; font-weight: 600; border-bottom: 1px solid #d3e6f5;
      display: flex; align-items: center; gap: 7px; line-height: 1.35;
    }
    .hint .i {
      flex-shrink: 0; width: 16px; height: 16px; border-radius: 50%;
      background: #1f6e9c; color: #fff; font-size: 0.7rem; font-style: italic;
      font-weight: 800; display: inline-flex; align-items: center; justify-content: center;
    }

    /* TABLE — always visible */
    .table-wrap {
      overflow-x: auto; -webkit-overflow-scrolling: touch;
      background: #ffffff; padding: 0 0 6px;
      max-height: 60vh;
      min-height: 280px;
    }
    table {
      border-collapse: separate; border-spacing: 0;
      width: max-content; min-width: 100%; font-size: 0.72rem;
    }
    thead th {
      position: sticky; top: 0; z-index: 5; background: #0f2b46; color: #ffffff;
      font-weight: 700; text-align: center; padding: 8px 4px;
      border-right: 1px solid rgba(255,255,255,0.08);
      border-bottom: 2px solid #1f6e9c; font-size: 0.62rem; white-space: nowrap;
    }
    thead th:first-child {
      position: sticky; left: 0; z-index: 7; text-align: left;
      background: #0b2136; min-width: 165px; max-width: 200px;
      padding-left: 10px; padding-right: 8px;
      border-right: 2px solid #1f6e9c;
      font-size: 0.6rem;
    }
    thead th.month-title {
      background: #1f6e9c; font-size: 0.7rem; letter-spacing: 0.12em;
      text-transform: uppercase; border-right: 2px solid #0f2b46;
    }
    thead th .dow {
      display: block; font-size: 0.52rem; font-weight: 500;
      color: rgba(255,255,255,0.7); letter-spacing: 0.06em; text-transform: uppercase;
    }
    thead th.weekend { background: #1a3a56; }
    thead th.today-col {
      background: #1f6e9c; outline: 2px solid #25b46a; outline-offset: -3px;
    }
    thead th .day-btn {
      display: block; width: 100%; background: transparent; border: none;
      color: inherit; font: inherit; font-weight: 700; padding: 3px 2px;
      cursor: pointer; font-family: inherit; border-radius: 5px;
      transition: background 0.12s; min-width: 30px;
      touch-action: manipulation;
    }
    thead th .day-btn:active { background: rgba(255,255,255,0.22); }
    thead th .day-btn .num { display: block; font-size: 0.78rem; line-height: 1.1; }

    tbody td {
      padding: 0; border-right: 1px solid #e8eef4; border-bottom: 1px solid #e8eef4;
      background: #ffffff; text-align: center; height: 44px;
    }
    tbody td:first-child {
      position: sticky; left: 0; z-index: 3; background: #fbfdff;
      text-align: left; padding: 8px 10px; font-weight: 500; color: #223c55;
      font-size: 0.7rem; line-height: 1.3; border-right: 2px solid #d6e4f1;
      min-width: 165px; max-width: 200px; white-space: normal;
    }
    tbody tr:nth-child(even) td:first-child { background: #f4f9fd; }
    tbody td.weekend { background: #f6f9fc; }
    tbody tr:nth-child(even) td.weekend { background: #eef4fa; }
    tbody td.today-col { background: #f0fbf4; }
    tbody tr:nth-child(even) td.today-col { background: #e9f8f0; }

    .day-cell {
      position: relative; width: 40px; min-width: 40px;
      cursor: pointer; user-select: none; -webkit-user-select: none;
      touch-action: manipulation; transition: background 0.15s;
    }
    .day-cell:active { background: #e3f0fa !important; }
    .day-cell input[type="checkbox"] {
      position: absolute; inset: 0; width: 100%; height: 100%;
      opacity: 0; margin: 0; cursor: pointer; z-index: 2;
      touch-action: manipulation;
    }
    .day-box {
      width: 24px; height: 24px; margin: 0 auto; border-radius: 7px;
      border: 2px solid #c4d6e6; background: #ffffff;
      transition: background 0.15s, border-color 0.15s, transform 0.15s;
      pointer-events: none;
      display: flex; align-items: center; justify-content: center;
    }
    .day-box::after {
      content: ""; width: 12px; height: 12px; border-radius: 3px;
      background: transparent; transition: background 0.15s;
    }
    .day-cell input:checked ~ .day-box {
      background: #25b46a; border-color: #25b46a; transform: scale(1.05);
    }
    .day-cell input:checked ~ .day-box::after {
      background: #ffffff;
    }

    /* ACTIONS — three small buttons on one row */
    .actions {
      padding: 12px 12px 16px; background: #ffffff; border-top: 1px solid #e8eef4;
    }
    .actions-row {
      display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 8px;
    }
    .btn {
      font-family: inherit; font-size: 0.78rem; font-weight: 700;
      border: none; border-radius: 10px; padding: 11px 6px;
      min-height: 48px; cursor: pointer; display: inline-flex;
      align-items: center; justify-content: center; gap: 5px;
      transition: transform 0.1s, box-shadow 0.15s, background 0.15s;
      -webkit-appearance: none; appearance: none;
      touch-action: manipulation; width: 100%;
      letter-spacing: 0.01em;
      text-decoration: none;
      text-align: center;
      line-height: 1.15;
    }
    .btn:active { transform: scale(0.96); }
    .btn-excel {
      background: linear-gradient(135deg, #1f7a3e 0%, #145c2c 100%);
      color: #ffffff; box-shadow: 0 3px 10px rgba(20, 92, 44, 0.28);
    }
    .btn-wa {
      background: linear-gradient(135deg, #25d366 0%, #1da851 100%);
      color: #ffffff; box-shadow: 0 3px 10px rgba(29, 168, 81, 0.28);
    }
    .btn-secondary {
      background: #ffffff; color: #1a4b6b; border: 1.5px solid #d6e0ea;
    }
    .btn .lbl {
      display: block;
    }

    /* LEGEND */
    .legend {
      padding: 10px 12px 12px; display: flex; flex-wrap: wrap;
      align-items: center; justify-content: space-between; gap: 8px;
      font-size: 0.68rem; color: #5b7391; background: #fafcfd;
      border-top: 1px solid #e8eef4;
    }
    .legend .item { display: inline-flex; align-items: center; gap: 5px; }
    .legend .swatch {
      width: 12px; height: 12px; border-radius: 4px;
      border: 2px solid #c4d6e6; background: #ffffff; display: inline-block;
    }
    .legend .swatch.green { background: #25b46a; border-color: #25b46a; }
    .legend .note {
      font-weight: 600; color: #1f6e9c; display: inline-flex;
      align-items: center; gap: 5px;
    }
    .legend .note::before {
      content: ""; width: 5px; height: 5px; border-radius: 50%; background: #1f6e9c;
    }
    .data-panel {
      padding: 10px 12px 14px; background: #f4f9fd;
      border-top: 1px solid #e2eaf2; display: flex; flex-wrap: wrap;
      gap: 8px; align-items: center; font-size: 0.68rem; color: #5b7391;
    }
    .data-panel .info {
      flex: 1 1 100%; display: flex; flex-direction: column;
      gap: 1px; line-height: 1.45;
    }
    .data-panel .info strong { color: #0f2b46; font-weight: 700; }
    .data-panel .storage-status {
      display: inline-flex; align-items: center; gap: 5px; font-weight: 600;
    }
    .data-panel .storage-status.ok { color: #0b6b3b; }
    .data-panel .storage-status.warn { color: #a05a00; }
    .data-panel .storage-status::before {
      content: ""; width: 6px; height: 6px; border-radius: 50%; background: currentColor;
    }

    /* MODAL */
    .modal-backdrop {
      position: fixed; inset: 0; background: rgba(15, 43, 70, 0.55);
      display: none; align-items: flex-end; justify-content: center;
      z-index: 100; padding: 0;
    }
    .modal-backdrop.show { display: flex; }
    .modal {
      background: #ffffff; border-radius: 22px 22px 0 0;
      max-width: 100%; width: 100%; max-height: 92vh; overflow-y: auto;
      -webkit-overflow-scrolling: touch;
      box-shadow: 0 -12px 40px rgba(15, 43, 70, 0.35);
      padding-bottom: env(safe-area-inset-bottom);
    }
    .modal-header::before {
      content: ""; display: block; width: 40px; height: 4px;
      background: #d6e4f1; border-radius: 4px; margin: 8px auto 4px;
    }
    .modal-header {
      padding: 4px 16px 12px; border-bottom: 1px solid #e8eef4;
      display: flex; align-items: center; justify-content: space-between;
      gap: 10px; position: sticky; top: 0; background: #ffffff;
      border-radius: 22px 22px 0 0; z-index: 2;
    }
    .modal-header h3 { font-size: 1rem; font-weight: 700; color: #0f2b46; }
    .modal-header .close {
      background: #f2f6fa; border: none; width: 36px; height: 36px;
      border-radius: 50%; font-size: 1.35rem; line-height: 1; color: #5b7391;
      cursor: pointer; display: flex; align-items: center; justify-content: center;
      font-family: inherit; flex-shrink: 0;
      touch-action: manipulation;
    }
    .modal-header .close:active { background: #e2eaf2; }
    .modal-body { padding: 12px 16px 18px; }
    .modal-task {
      display: flex; align-items: flex-start; gap: 12px;
      padding: 14px 14px; border: 1.5px solid #e2eaf2; border-radius: 12px;
      margin-bottom: 8px; cursor: pointer; user-select: none;
      transition: 0.15s; position: relative;
      min-height: 52px;
    }
    .modal-task:active { transform: scale(0.99); background: #f4f9fd; }
    .modal-task.completed {
      background: linear-gradient(135deg, #e8f8ee 0%, #d6f2e0 100%);
      border-color: #6ecb8f;
    }
    .modal-task input {
      position: absolute; inset: 0; opacity: 0; cursor: pointer; z-index: 2;
    }
    .modal-task .check {
      width: 24px; height: 24px; border-radius: 7px;
      border: 2px solid #c4d6e6; background: #ffffff; flex-shrink: 0;
      margin-top: 1px; transition: 0.15s;
      display: flex; align-items: center; justify-content: center;
    }
    .modal-task.completed .check {
      background: #25b46a; border-color: #25b46a;
    }
    .modal-task .check::after {
      content: ""; width: 12px; height: 12px; border-radius: 3px; background: transparent;
    }
    .modal-task.completed .check::after { background: #ffffff; }
    .modal-task .text {
      font-size: 0.86rem; color: #223c55; font-weight: 500;
      line-height: 1.35; pointer-events: none;
    }
    .modal-task.completed .text { color: #0b6b3b; font-weight: 600; }
    .modal-actions {
      display: grid; grid-template-columns: 1fr 1fr; gap: 8px; padding-top: 8px;
      position: sticky; bottom: 0; background: #ffffff; padding-bottom: 4px;
    }
    .modal-actions button {
      padding: 15px 12px; border-radius: 12px; min-height: 50px;
      border: 1.5px solid #d6e0ea; background: #ffffff;
      color: #1a4b6b; font-weight: 700; font-size: 0.92rem;
      font-family: inherit; cursor: pointer;
      touch-action: manipulation;
    }
    .modal-actions button:active { background: #f2f6fa; }
    .modal-actions button.primary {
      background: #1f6e9c; border-color: #1f6e9c; color: #ffffff;
    }
    .modal-actions button.primary:active { background: #16557a; }

    /* Switching overlay */
    .switching-overlay {
      position: fixed; inset: 0; background: rgba(15, 43, 70, 0.35);
      display: none; align-items: center; justify-content: center;
      z-index: 200;
    }
    .switching-overlay.show { display: flex; }
    .switching-card {
      background: #ffffff; border-radius: 16px; padding: 18px 22px;
      box-shadow: 0 20px 40px rgba(15, 43, 70, 0.25);
      display: flex; align-items: center; gap: 12px;
      font-size: 0.9rem; font-weight: 700; color: #0f2b46;
    }
    .switching-card .spin {
      width: 22px; height: 22px; border-radius: 50%;
      border: 3px solid #d6e4f1; border-top-color: #1f6e9c;
      animation: spin 0.7s linear infinite;
    }
    @keyframes spin { to { transform: rotate(360deg); } }

    /* Toast */
    .toast {
      position: fixed; left: 50%; bottom: 90px;
      transform: translateX(-50%) translateY(20px);
      background: #0f2b46; color: #ffffff;
      font-size: 0.85rem; font-weight: 600;
      padding: 12px 20px; border-radius: 12px;
      box-shadow: 0 12px 30px rgba(15, 43, 70, 0.35);
      opacity: 0; pointer-events: none;
      transition: opacity 0.25s, transform 0.25s;
      z-index: 300;
      max-width: 90vw;
      text-align: center;
    }
    .toast.show {
      opacity: 1;
      transform: translateX(-50%) translateY(0);
    }

    /* PWA update banner */
    .update-banner {
      position: fixed; left: 12px; right: 12px; bottom: 12px;
      background: #0b6b3b; color: #ffffff;
      font-size: 0.85rem; font-weight: 600;
      padding: 12px 16px; border-radius: 12px;
      box-shadow: 0 12px 30px rgba(11, 107, 59, 0.35);
      display: none; align-items: center; justify-content: space-between; gap: 10px;
      z-index: 250;
    }
    .update-banner.show { display: flex; }
    .update-banner button {
      background: #ffffff; color: #0b6b3b; border: none;
      padding: 8px 14px; border-radius: 8px;
      font-weight: 700; font-size: 0.82rem; cursor: pointer;
      font-family: inherit;
      touch-action: manipulation;
    }

    /* ---------- TABLET & DESKTOP ---------- */
    @media (min-width: 700px) {
      body { padding: 16px 10px 40px; }
      .card { max-width: 1200px; border-radius: 18px; }
      .header { padding: 20px 22px 18px; }
      .header h1 { font-size: 1.35rem; }
      .header p { font-size: 0.85rem; }
      .profile-strip { padding: 14px 22px; }
      .profile-avatar { width: 52px; height: 52px; font-size: 1.25rem; }
      .profile-info .name { font-size: 1.2rem; }
      .live-date { padding: 12px 20px; }
      .live-date .dt { font-size: 1.05rem; }
      .today-card { margin: 14px 18px 0; padding: 14px 16px; }
      .menu { padding: 14px 18px; gap: 10px; }
      .menu .field { flex: 1 1 130px; min-width: 110px; padding: 8px 11px; }
      .menu .field select { font-size: 16px; }
      .backup-panel { padding: 14px 18px; }
      .hint { padding: 10px 20px; font-size: 0.78rem; }
      thead th:first-child,
      tbody td:first-child { min-width: 220px; max-width: 280px; font-size: 0.78rem; padding: 10px 14px; }
      .day-cell { width: 38px; min-width: 38px; }
      .day-box { width: 22px; height: 22px; }
      .table-wrap { max-height: 65vh; }
      .actions { padding: 16px 18px 18px; }
      .actions-row { gap: 12px; }
      .btn { min-height: 52px; font-size: 0.95rem; padding: 14px 12px; }
      .data-panel { padding: 14px 20px 18px; }
      .data-panel .info { flex: 1 1 200px; }
      .legend { padding: 12px 20px 16px; font-size: 0.72rem; }
      .modal-backdrop { align-items: center; padding: 16px; }
      .modal { border-radius: 18px; max-width: 460px; max-height: 90vh; }
      .modal-header::before { display: none; }
      .modal-header { border-radius: 18px 18px 0 0; padding: 16px 20px 12px; }
    }

    @media (max-width: 360px) {
      thead th:first-child,
      tbody td:first-child { min-width: 140px; max-width: 160px; font-size: 0.66rem; }
      .day-cell { width: 36px; min-width: 36px; }
      .btn { font-size: 0.7rem; min-height: 44px; padding: 10px 4px; }
      .actions-row { gap: 6px; }
      .header h1 { font-size: 1.05rem; }
    }
  </style>
</head>
<body>
<div class="card">

  <header class="header">
    <div class="header-top">
      <div class="brand">
        <span class="brand-mark">DXQ</span>
        DXQ Workshop
      </div>
      <span class="badge-live"><span class="dot"></span><span id="liveMonth">—</span></span>
    </div>
    <h1>DXQ Workshop Technician Checklist</h1>
    <p>Each technician has their own separate checklist.</p>
  </header>

  <!-- PROFILE STRIP -->
  <div class="profile-strip">
    <div class="profile-avatar" id="profileAvatar">S</div>
    <div class="profile-info">
      <span class="lbl">Active Technician</span>
      <span class="name" id="profileName">Sule</span>
    </div>
    <span class="profile-badge">Saved separately</span>
  </div>

  <!-- LIVE DATE -->
  <div class="live-date">
    <div class="left">
      <span class="lbl">Today</span>
      <span class="dt" id="liveDate">—</span>
    </div>
    <span class="time" id="liveTime">—</span>
  </div>

  <!-- TODAY CARD -->
  <div class="today-card">
    <div class="info">
      <span class="t1">Today's progress</span>
      <span class="t2" id="todayLabel">—</span>
    </div>
    <span class="prog" id="todayProgress">0 / 6</span>
  </div>

  <!-- MENU -->
  <div class="menu">
    <div class="field tech-field">
      <label for="userSelect">Technician</label>
      <select id="userSelect">
        <option value="Sule">Sule</option>
        <option value="Abass">Abass</option>
        <option value="Ahmed">Ahmed</option>
        <option value="Eric">Eric</option>
        <option value="Jay">Jay</option>
        <option value="Kevin">Kevin</option>
        <option value="Rommel">Rommel</option>
        <option value="Ricky">Ricky</option>
      </select>
    </div>
    <div class="field">
      <label for="monthSelect">Month</label>
      <select id="monthSelect">
        <option value="0">January</option>
        <option value="1">February</option>
        <option value="2">March</option>
        <option value="3">April</option>
        <option value="4">May</option>
        <option value="5">June</option>
        <option value="6">July</option>
        <option value="7">August</option>
        <option value="8">September</option>
        <option value="9">October</option>
        <option value="10">November</option>
        <option value="11">December</option>
      </select>
    </div>
    <div class="field">
      <label for="yearSelect">Year</label>
      <select id="yearSelect"></select>
    </div>
    <span class="saved-indicator" id="savedIndicator">Saved</span>
  </div>

  <!-- BACKUP PANEL -->
  <div class="backup-panel">
    <div class="bp-left">
      <div class="bp-icon">XLS</div>
      <div class="bp-info">
        <span class="t1">Full Checklist Excel Export</span>
        <span class="t2" id="backupInfo">Tap <strong>Export</strong> below to save the whole month.</span>
      </div>
    </div>
  </div>

  <div class="hint"><span class="i">i</span> Tap any box to tick, or tap a day number to open the day popup.</div>

  <!-- TABLE — ALWAYS VISIBLE -->
  <div class="table-wrap">
    <table id="taskTable">
      <thead><tr id="headerRow"></tr></thead>
      <tbody id="tableBody"></tbody>
    </table>
  </div>

  <!-- ACTIONS — three small buttons on one row -->
  <div class="actions">
    <div class="actions-row">
      <button type="button" class="btn btn-wa" id="whatsappBtn">
        <span class="lbl">Share</span>
      </button>
      <button type="button" class="btn btn-excel" id="exportExcelBtn">
        <span class="lbl">Export</span>
      </button>
      <button type="button" class="btn btn-secondary" id="copyDayBtn">
        <span class="lbl">Copy</span>
      </button>
    </div>
  </div>

  <div class="legend">
    <span class="item"><span class="swatch"></span> Not done</span>
    <span class="item"><span class="swatch green"></span> Completed</span>
    <span class="note">Excel · WhatsApp · Copy</span>
  </div>

  <div class="data-panel">
    <div class="info">
      <div><strong>Local database:</strong> each technician's data is stored separately on this device.</div>
      <div id="storageInfo">Checking storage…</div>
    </div>
    <span class="storage-status ok" id="storageStatus">Active</span>
  </div>

</div>

<!-- DAY POPUP -->
<div class="modal-backdrop" id="modalBackdrop">
  <div class="modal">
    <div class="modal-header">
      <h3 id="modalTitle">Day tasks</h3>
      <button type="button" class="close" id="modalClose" aria-label="Close">×</button>
    </div>
    <div class="modal-body">
      <div id="modalTasks"></div>
      <div class="modal-actions">
        <button type="button" id="modalClear">Clear day</button>
        <button type="button" id="modalDone" class="primary">Done</button>
      </div>
    </div>
  </div>
</div>

<!-- SWITCHING OVERLAY -->
<div class="switching-overlay" id="switchingOverlay">
  <div class="switching-card">
    <div class="spin"></div>
    <span id="switchingText">Switching technician…</span>
  </div>
</div>

<!-- TOAST -->
<div class="toast" id="toast"></div>

<!-- PWA UPDATE BANNER -->
<div class="update-banner" id="updateBanner">
  <span>New version available</span>
  <button type="button" id="updateBtn">Update now</button>
</div>

<script>
  (function() {
    /* =========================================================
       PWA — SERVICE WORKER REGISTRATION
       ========================================================= */
    let swRegistration = null;

    if ('serviceWorker' in navigator && location.protocol !== 'file:') {
      window.addEventListener('load', function() {
        navigator.serviceWorker.register('./sw.js', { scope: './' })
          .then(function(reg) {
            swRegistration = reg;

            reg.addEventListener('updatefound', function() {
              const newWorker = reg.installing;
              if (!newWorker) return;
              newWorker.addEventListener('statechange', function() {
                if (newWorker.state === 'installed' && navigator.serviceWorker.controller) {
                  document.getElementById('updateBanner').classList.add('show');
                }
              });
            });
          })
          .catch(function(err) {
            console.warn('Service worker registration failed:', err);
          });
      });

      let refreshing = false;
      navigator.serviceWorker.addEventListener('controllerchange', function() {
        if (refreshing) return;
        refreshing = true;
        window.location.reload();
      });
    }

    const updateBtn = document.getElementById('updateBtn');
    if (updateBtn) {
      updateBtn.addEventListener('click', function() {
        if (swRegistration && swRegistration.waiting) {
          swRegistration.waiting.postMessage({ type: 'SKIP_WAITING' });
        } else {
          window.location.reload();
        }
      });
    }

    /* =========================================================
       CONFIG
       ========================================================= */
    const STORAGE_KEY = 'dxq_workshop_technician_checklist_v16';
    const CURRENT_USER_KEY = 'dxq_current_user_v16';
    const TASKS = [
      "Prioritize today's scheduled services on your Hubtiger board.",
      "Verify that your board has at least 4 hours of work allocated at the start of duty.",
      "Report to Team Leader or Assistant Workshop Manager by 3 PM if any jobs are not on track for completion.",
      "Ensure the workstation is neat and tidy, and that all tools are kept in their correct places.",
      "Sign in and out on the tool inventory checklist, and lock your cabinet before leaving.",
      "Confirm all Hubtiger jobs are completed and have been final-checked by a senior mechanic or team leader."
    ];
    const DAY_NAMES_SHORT = ['Sun','Mon','Tue','Wed','Thu','Fri','Sat'];
    const DAY_NAMES_LONG  = ['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday'];
    const MONTH_NAMES = ['January','February','March','April','May','June','July','August','September','October','November','December'];
    const TECHNICIANS = ['Sule','Abass','Ahmed','Eric','Jay','Kevin','Rommel','Ricky'];

    /* =========================================================
       ELEMENTS
       ========================================================= */
    const userSelect      = document.getElementById('userSelect');
    const monthSelect     = document.getElementById('monthSelect');
    const yearSelect      = document.getElementById('yearSelect');
    const headerRow       = document.getElementById('headerRow');
    const tableBody       = document.getElementById('tableBody');
    const liveMonth       = document.getElementById('liveMonth');
    const liveDate        = document.getElementById('liveDate');
    const liveTime        = document.getElementById('liveTime');
    const todayLabel      = document.getElementById('todayLabel');
    const todayProgress   = document.getElementById('todayProgress');
    const savedIndicator  = document.getElementById('savedIndicator');
    const storageStatus   = document.getElementById('storageStatus');
    const storageInfo     = document.getElementById('storageInfo');
    const modalBackdrop   = document.getElementById('modalBackdrop');
    const modalTitle      = document.getElementById('modalTitle');
    const modalTasks      = document.getElementById('modalTasks');
    const backupInfo      = document.getElementById('backupInfo');
    const profileAvatar   = document.getElementById('profileAvatar');
    const profileName     = document.getElementById('profileName');
    const switchingOverlay = document.getElementById('switchingOverlay');
    const switchingText   = document.getElementById('switchingText');
    const toast           = document.getElementById('toast');

    /* =========================================================
       TOAST
       ========================================================= */
    let toastTimer = null;
    function showToast(msg) {
      toast.textContent = msg;
      toast.classList.add('show');
      clearTimeout(toastTimer);
      toastTimer = setTimeout(function() { toast.classList.remove('show'); }, 1800);
    }

    /* =========================================================
       PER-TECHNICIAN DATABASE
       ========================================================= */
    let storageAvailable = (function() {
      try {
        const k = '__dxq_test__';
        localStorage.setItem(k, '1');
        localStorage.removeItem(k);
        return true;
      } catch (e) { return false; }
    })();

    function emptyProfile() {
      return {
        view: { year: new Date().getFullYear(), month: new Date().getMonth() },
        months: {},
        lastUpdated: null
      };
    }

    function emptyDatabase() {
      const db = {
        version: 16,
        currentUser: TECHNICIANS[0],
        profiles: {},
        backupMeta: null
      };
      TECHNICIANS.forEach(function(name) {
        db.profiles[name] = emptyProfile();
      });
      return db;
    }

    let db = emptyDatabase();

    function currentProfile() { return db.profiles[db.currentUser]; }
    function monthKey(y, m) { return y + '-' + m; }
    function daysInMonth(y, m) { return new Date(y, m + 1, 0).getDate(); }

    function getGrid(y, m) {
      const prof = currentProfile();
      const key = monthKey(y, m);
      if (!prof.months[key]) {
        const days = daysInMonth(y, m);
        prof.months[key] = TASKS.map(function() { return Array(days).fill(false); });
      }
      return prof.months[key];
    }

    function readFromStorage() {
      if (!storageAvailable) return null;
      try {
        const raw = localStorage.getItem(STORAGE_KEY);
        if (!raw) return null;
        const parsed = JSON.parse(raw);
        if (parsed && parsed.profiles && parsed.currentUser) return parsed;
      } catch (e) { console.warn('read error', e); }
      return null;
    }

    function writeToStorage() {
      if (!storageAvailable) {
        storageStatus.textContent = 'Unavailable';
        storageStatus.className = 'storage-status warn';
        storageInfo.textContent = 'Local storage is blocked. Ticks will not be saved.';
        return;
      }
      const prof = currentProfile();
      prof.lastUpdated = new Date().toISOString();
      TECHNICIANS.forEach(function(name) {
        if (!db.profiles[name]) db.profiles[name] = emptyProfile();
      });
      try {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(db));
        localStorage.setItem(CURRENT_USER_KEY, db.currentUser);
        flashSaved();
        updateStorageInfo();
      } catch (e) {
        storageStatus.textContent = 'Error';
        storageStatus.className = 'storage-status warn';
        storageInfo.textContent = 'Could not save (storage full or blocked).';
      }
    }

    let savedTimer = null;
    function flashSaved() {
      savedIndicator.classList.add('show');
      clearTimeout(savedTimer);
      savedTimer = setTimeout(function() { savedIndicator.classList.remove('show'); }, 1400);
    }

    function updateStorageInfo() {
      const prof = currentProfile();
      if (!prof.lastUpdated) {
        storageInfo.textContent = db.currentUser + ' — no changes saved yet.';
      } else {
        const d = new Date(prof.lastUpdated);
        const stamp = d.toLocaleString('en-GB', {
          day: '2-digit', month: 'short', year: 'numeric',
          hour: '2-digit', minute: '2-digit'
        });
        storageInfo.textContent = db.currentUser + ' — last saved: ' + stamp;
      }
    }

    /* =========================================================
       LIVE DATE
       ========================================================= */
    function updateLiveDate() {
      const now = new Date();
      const dow = DAY_NAMES_LONG[now.getDay()];
      const d = now.getDate();
      const m = MONTH_NAMES[now.getMonth()];
      const y = now.getFullYear();
      liveDate.textContent = dow + ', ' + d + ' ' + m + ' ' + y;

      const hh = String(now.getHours()).padStart(2, '0');
      const mm = String(now.getMinutes()).padStart(2, '0');
      const ss = String(now.getSeconds()).padStart(2, '0');
      liveTime.textContent = hh + ':' + mm + ':' + ss;
    }
    updateLiveDate();
    setInterval(updateLiveDate, 1000);

    /* =========================================================
       TODAY HELPERS
       ========================================================= */
    function getTodayParts() {
      const t = new Date();
      return { year: t.getFullYear(), month: t.getMonth(), dayIdx: t.getDate() - 1 };
    }
    function isToday(year, month, dayIdx) {
      const t = getTodayParts();
      return t.year === year && t.month === month && t.dayIdx === dayIdx;
    }
    function updateTodayProgress() {
      const t = getTodayParts();
      const grid = getGrid(t.year, t.month);
      let count = 0;
      for (let i = 0; i < TASKS.length; i++) if (grid[i] && grid[i][t.dayIdx]) count++;
      todayProgress.textContent = count + ' / ' + TASKS.length;
      todayProgress.classList.toggle('full', count === TASKS.length);
      const dow = DAY_NAMES_LONG[new Date(t.year, t.month, t.dayIdx + 1).getDay()];
      todayLabel.textContent = dow + ', ' + (t.dayIdx + 1) + ' ' + MONTH_NAMES[t.month] + ' ' + t.year;
    }

    function isoDate(parts) {
      return parts.year + '-' +
             String(parts.month + 1).padStart(2, '0') + '-' +
             String(parts.dayIdx + 1).padStart(2, '0');
    }

    /* =========================================================
       PROFILE STRIP
       ========================================================= */
    function updateProfileStrip() {
      const name = db.currentUser;
      profileAvatar.textContent = name.charAt(0).toUpperCase();
      profileName.textContent = name;
    }

    /* =========================================================
       BUILD TABLE
       ========================================================= */
    function buildTable(year, month) {
      liveMonth.textContent = MONTH_NAMES[month] + ' ' + year;
      headerRow.innerHTML = '';
      tableBody.innerHTML = '';
      const days = daysInMonth(year, month);
      const grid = getGrid(year, month);

      const thTitle = document.createElement('th');
      thTitle.className = 'month-title';
      thTitle.textContent = MONTH_NAMES[month].toUpperCase() + ' ' + year;
      thTitle.style.textAlign = 'left';
      thTitle.style.paddingLeft = '10px';
      headerRow.appendChild(thTitle);

      for (let d = 1; d <= days; d++) {
        const th = document.createElement('th');
        const dayIdx = d - 1;
        const date = new Date(year, month, d);
        const dow = date.getDay();
        const isWeekend = (dow === 0 || dow === 6);
        if (isWeekend) th.classList.add('weekend');
        if (isToday(year, month, dayIdx)) th.classList.add('today-col');

        const btn = document.createElement('button');
        btn.className = 'day-btn';
        btn.type = 'button';
        btn.dataset.day = dayIdx;

        const numSpan = document.createElement('span');
        numSpan.className = 'num';
        numSpan.textContent = d;

        const dowSpan = document.createElement('span');
        dowSpan.className = 'dow';
        dowSpan.textContent = DAY_NAMES_SHORT[dow];

        btn.appendChild(numSpan);
        btn.appendChild(dowSpan);
        th.appendChild(btn);
        btn.addEventListener('click', function(e) {
          e.preventDefault();
          openDayModal(year, month, dayIdx);
        });
        headerRow.appendChild(th);
      }

      TASKS.forEach(function(taskText, tIdx) {
        const tr = document.createElement('tr');
        const tdTask = document.createElement('td');
        tdTask.textContent = (tIdx + 1) + '. ' + taskText;
        tr.appendChild(tdTask);

        for (let d = 0; d < days; d++) {
          const td = document.createElement('td');
          const dayNum = d + 1;
          const date = new Date(year, month, dayNum);
          const dow = date.getDay();
          const isWeekend = (dow === 0 || dow === 6);
          const isTodayCell = isToday(year, month, d);

          td.className = 'day-cell' + (isWeekend ? ' weekend' : '') + (isTodayCell ? ' today-col' : '');
          td.dataset.task = tIdx;
          td.dataset.day = d;

          const cb = document.createElement('input');
          cb.type = 'checkbox';
          cb.checked = !!grid[tIdx][d];
          cb.setAttribute('aria-label', 'Task ' + (tIdx + 1) + ' day ' + dayNum);

          const box = document.createElement('div');
          box.className = 'day-box';

          cb.addEventListener('change', function() {
            grid[tIdx][d] = this.checked;
            writeToStorage();
            if (isTodayCell) updateTodayProgress();
          });

          td.addEventListener('click', function(e) {
            if (e.target === cb) return;
            e.preventDefault();
            cb.checked = !cb.checked;
            cb.dispatchEvent(new Event('change'));
          });

          td.appendChild(cb);
          td.appendChild(box);
          tr.appendChild(td);
        }
        tableBody.appendChild(tr);
      });
    }

    /* =========================================================
       DAY POPUP
       ========================================================= */
    let modalCtx = { year: 0, month: 0, day: 0 };
    function openDayModal(year, month, dayIdx) {
      modalCtx = { year: year, month: month, day: dayIdx };
      const grid = getGrid(year, month);
      const dayNum = dayIdx + 1;
      const date = new Date(year, month, dayNum);
      const dow = DAY_NAMES_SHORT[date.getDay()];
      modalTitle.textContent = dow + ', ' + dayNum + ' ' + MONTH_NAMES[month] + ' ' + year;

      modalTasks.innerHTML = '';
      TASKS.forEach(function(taskText, tIdx) {
        const wrap = document.createElement('label');
        wrap.className = 'modal-task' + (grid[tIdx][dayIdx] ? ' completed' : '');
        const cb = document.createElement('input');
        cb.type = 'checkbox';
        cb.checked = !!grid[tIdx][dayIdx];
        const check = document.createElement('div');
        check.className = 'check';
        const text = document.createElement('div');
        text.className = 'text';
        text.textContent = (tIdx + 1) + '. ' + taskText;
        cb.addEventListener('change', function() {
          grid[tIdx][dayIdx] = this.checked;
          if (this.checked) wrap.classList.add('completed');
          else wrap.classList.remove('completed');
          syncTableCheckbox(tIdx, dayIdx, this.checked);
          writeToStorage();
          if (isToday(year, month, dayIdx)) updateTodayProgress();
        });
        wrap.appendChild(cb);
        wrap.appendChild(check);
        wrap.appendChild(text);
        modalTasks.appendChild(wrap);
      });
      modalBackdrop.classList.add('show');
      document.body.style.overflow = 'hidden';
    }
    function syncTableCheckbox(taskIdx, dayIdx, checked) {
      const td = tableBody.querySelector('.day-cell[data-task="' + taskIdx + '"][data-day="' + dayIdx + '"]');
      if (td) {
        const cb = td.querySelector('input[type="checkbox"]');
        if (cb) cb.checked = checked;
      }
    }
    function closeDayModal() {
      modalBackdrop.classList.remove('show');
      document.body.style.overflow = '';
    }

    /* =========================================================
       BUILD TODAY'S SHARE/COPY TEXT
       ========================================================= */
    function buildTodayText() {
      const t = getTodayParts();
      const grid = getGrid(t.year, t.month);
      const dayIdx = t.dayIdx;
      const dayNum = dayIdx + 1;
      const date = new Date(t.year, t.month, dayNum);
      const dow = DAY_NAMES_LONG[date.getDay()];
      const dateStr = dow + ', ' + dayNum + ' ' + MONTH_NAMES[t.month] + ' ' + t.year;
      let done = 0;
      TASKS.forEach(function(_, tIdx) { if (grid[tIdx][dayIdx]) done++; });

      let msg = 'DXQ WORKSHOP TECHNICIAN CHECKLIST\n';
      msg += 'Date: ' + dateStr + '\n';
      msg += 'Technician: ' + db.currentUser + '\n';
      msg += '------------------------------------\n';
      TASKS.forEach(function(task, tIdx) {
        const isDone = grid[tIdx][dayIdx];
        msg += (tIdx + 1) + '. ' + task + ' [' + (isDone ? 'DONE' : 'NOT DONE') + ']\n';
      });
      msg += '------------------------------------\n';
      msg += 'Progress: ' + done + '/' + TASKS.length + ' tasks completed\n';
      msg += done === TASKS.length
        ? 'All tasks completed. Great work.\n'
        : 'Remaining: ' + (TASKS.length - done) + ' task(s) pending.\n';
      return msg;
    }

    function copyToday() {
      const text = buildTodayText();
      if (navigator.clipboard && navigator.clipboard.writeText) {
        navigator.clipboard.writeText(text).then(function() {
          showToast('Copied to clipboard');
        }).catch(function() {
          fallbackCopy(text);
          showToast('Copied to clipboard');
        });
        return;
      }
      fallbackCopy(text);
      showToast('Copied to clipboard');
    }

    function fallbackCopy(text) {
      const ta = document.createElement('textarea');
      ta.value = text;
      ta.setAttribute('readonly', '');
      ta.style.position = 'fixed';
      ta.style.top = '0';
      ta.style.left = '0';
      ta.style.width = '1px';
      ta.style.height = '1px';
      ta.style.opacity = '0';
      ta.style.pointerEvents = 'none';
      document.body.appendChild(ta);
      ta.focus();
      ta.select();
      try { document.execCommand('copy'); } catch (e) {}
      document.body.removeChild(ta);
    }

    function sendToWhatsApp() {
      const text = buildTodayText();
      const encoded = encodeURIComponent(text);
      const url = 'https://wa.me/?text=' + encoded;
      const a = document.createElement('a');
      a.href = url;
      a.target = '_blank';
      a.rel = 'noopener noreferrer';
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
    }

    /* =========================================================
       EXCEL EXPORT
       ========================================================= */
    function escXml(s) {
      return String(s)
        .replace(/&/g, '&amp;')
        .replace(/</g, '&lt;')
        .replace(/>/g, '&gt;')
        .replace(/"/g, '&quot;')
        .replace(/'/g, '&apos;');
    }
    function colLetter(n) {
      let s = '';
      while (n > 0) {
        const rem = (n - 1) % 26;
        s = String.fromCharCode(65 + rem) + s;
        n = Math.floor((n - 1) / 26);
      }
      return s;
    }

    function buildFullWorkbook() {
      const prof = currentProfile();
      const year = prof.view.year;
      const month = prof.view.month;
      const days = daysInMonth(year, month);
      const grid = getGrid(year, month);

      const sheetRows = [];
      let r = 1;

      sheetRows.push(
        '<row r="' + r + '">' +
        '<c r="A' + r + '" t="inlineStr" s="1"><is><t>DXQ Workshop Technician Checklist - Full Report</t></is></c>' +
        '</row>'
      );
      r++;
      sheetRows.push(
        '<row r="' + r + '">' +
        '<c r="A' + r + '" t="inlineStr"><is><t>Generated: ' + escXml(new Date().toLocaleString('en-GB')) + '</t></is></c>' +
        '</row>'
      );
      r++;
      sheetRows.push(
        '<row r="' + r + '">' +
        '<c r="A' + r + '" t="inlineStr" s="2"><is><t>Technician</t></is></c>' +
        '<c r="B' + r + '" t="inlineStr"><is><t>' + escXml(db.currentUser) + '</t></is></c>' +
        '</row>'
      );
      r++;
      sheetRows.push(
        '<row r="' + r + '">' +
        '<c r="A' + r + '" t="inlineStr" s="2"><is><t>Month</t></is></c>' +
        '<c r="B' + r + '" t="inlineStr"><is><t>' + escXml(MONTH_NAMES[month] + ' ' + year) + '</t></is></c>' +
        '</row>'
      );
      r += 2;

      let headerCells = '<c r="A' + r + '" t="inlineStr" s="3"><is><t>Task</t></is></c>';
      for (let d = 1; d <= days; d++) {
        const ref = colLetter(d + 1) + r;
        headerCells += '<c r="' + ref + '" t="inlineStr" s="3"><is><t>' + d + '</t></is></c>';
      }
      const totalCol = colLetter(days + 2) + r;
      headerCells += '<c r="' + totalCol + '" t="inlineStr" s="3"><is><t>Total</t></is></c>';
      sheetRows.push('<row r="' + r + '">' + headerCells + '</row>');
      r++;

      TASKS.forEach(function(taskText, tIdx) {
        let cells = '<c r="A' + r + '" t="inlineStr"><is><t>' + escXml((tIdx + 1) + '. ' + taskText) + '</t></is></c>';
        let done = 0;
        for (let d = 0; d < days; d++) {
          const ref = colLetter(d + 2) + r;
          const val = grid[tIdx][d] ? 'DONE' : 'NOT_DONE';
          if (grid[tIdx][d]) done++;
          cells += '<c r="' + ref + '" t="inlineStr"><is><t>' + val + '</t></is></c>';
        }
        const totalRef = colLetter(days + 2) + r;
        cells += '<c r="' + totalRef + '" t="inlineStr" s="3"><is><t>' + done + '/' + days + '</t></is></c>';
        sheetRows.push('<row r="' + r + '">' + cells + '</row>');
        r++;
      });

      let sumCells = '<c r="A' + r + '" t="inlineStr" s="3"><is><t>Total per day</t></is></c>';
      let grandTotal = 0;
      for (let d = 0; d < days; d++) {
        let done = 0;
        for (let t = 0; t < TASKS.length; t++) if (grid[t][d]) done++;
        grandTotal += done;
        const ref = colLetter(d + 2) + r;
        sumCells += '<c r="' + ref + '" t="inlineStr" s="3"><is><t>' + done + '/' + TASKS.length + '</t></is></c>';
      }
      const grandRef = colLetter(days + 2) + r;
      sumCells += '<c r="' + grandRef + '" t="inlineStr" s="3"><is><t>' + grandTotal + '/' + (TASKS.length * days) + '</t></is></c>';
      sheetRows.push('<row r="' + r + '">' + sumCells + '</row>');

      const sheetXml =
        '<?xml version="1.0" encoding="UTF-8" standalone="yes"?>' +
        '<worksheet xmlns="http://schemas.openxmlformats.org/spreadsheetml/2006/main">' +
          '<sheetData>' + sheetRows.join('') + '</sheetData>' +
        '</worksheet>';

      const stylesXml =
        '<?xml version="1.0" encoding="UTF-8" standalone="yes"?>' +
        '<styleSheet xmlns="http://schemas.openxmlformats.org/spreadsheetml/2006/main">' +
          '<fonts count="3">' +
            '<font><sz val="11"/><name val="Calibri"/></font>' +
            '<font><b/><sz val="14"/><name val="Calibri"/></font>' +
            '<font><b/><sz val="11"/><color rgb="FFFFFFFF"/><name val="Calibri"/></font>' +
          '</fonts>' +
          '<fills count="3">' +
            '<fill><patternFill patternType="none"/></fill>' +
            '<fill><patternFill patternType="gray125"/></fill>' +
            '<fill><patternFill patternType="solid"><fgColor rgb="FF1F6E9C"/><bgColor indexed="64"/></patternFill></fill>' +
          '</fills>' +
          '<borders count="1"><border><left/><right/><top/><bottom/><diagonal/></border></borders>' +
          '<cellStyleXfs count="1"><xf numFmtId="0" fontId="0" fillId="0" borderId="0"/></cellStyleXfs>' +
          '<cellXfs count="4">' +
            '<xf numFmtId="0" fontId="0" fillId="0" borderId="0" xfId="0"/>' +
            '<xf numFmtId="0" fontId="1" fillId="0" borderId="0" xfId="0" applyFont="1"/>' +
            '<xf numFmtId="0" fontId="1" fillId="0" borderId="0" xfId="0" applyFont="1"/>' +
            '<xf numFmtId="0" fontId="2" fillId="2" borderId="0" xfId="0" applyFont="1" applyFill="1"/>' +
          '</cellXfs>' +
          '<cellStyles count="1"><cellStyle name="Normal" xfId="0" builtinId="0"/></cellStyles>' +
        '</styleSheet>';

      const workbookXml =
        '<?xml version="1.0" encoding="UTF-8" standalone="yes"?>' +
        '<workbook xmlns="http://schemas.openxmlformats.org/spreadsheetml/2006/main" ' +
        'xmlns:r="http://schemas.openxmlformats.org/officeDocument/2006/relationships">' +
          '<sheets><sheet name="Checklist" sheetId="1" r:id="rId1"/></sheets>' +
        '</workbook>';

      const workbookRels =
        '<?xml version="1.0" encoding="UTF-8" standalone="yes"?>' +
        '<Relationships xmlns="http://schemas.openxmlformats.org/package/2006/relationships">' +
          '<Relationship Id="rId1" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/worksheet" Target="worksheets/sheet1.xml"/>' +
          '<Relationship Id="rId2" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/styles" Target="styles.xml"/>' +
        '</Relationships>';

      const contentTypes =
        '<?xml version="1.0" encoding="UTF-8" standalone="yes"?>' +
        '<Types xmlns="http://schemas.openxmlformats.org/package/2006/content-types">' +
          '<Default Extension="rels" ContentType="application/vnd.openxmlformats-package.relationships+xml"/>' +
          '<Default Extension="xml" ContentType="application/xml"/>' +
          '<Override PartName="/xl/workbook.xml" ContentType="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet.main+xml"/>' +
          '<Override PartName="/xl/worksheets/sheet1.xml" ContentType="application/vnd.openxmlformats-officedocument.spreadsheetml.worksheet+xml"/>' +
          '<Override PartName="/xl/styles.xml" ContentType="application/vnd.openxmlformats-officedocument.spreadsheetml.styles+xml"/>' +
        '</Types>';

      const rootRels =
        '<?xml version="1.0" encoding="UTF-8" standalone="yes"?>' +
        '<Relationships xmlns="http://schemas.openxmlformats.org/package/2006/relationships">' +
          '<Relationship Id="rId1" Type="http://schemas.openxmlformats.org/officeDocument/2006/relationships/officeDocument" Target="xl/workbook.xml"/>' +
        '</Relationships>';

      return { contentTypes, rootRels, workbookXml, workbookRels, sheetXml, stylesXml };
    }

    function crc32(bytes) {
      let table = crc32.table;
      if (!table) {
        table = crc32.table = new Uint32Array(256);
        for (let i = 0; i < 256; i++) {
          let c = i;
          for (let k = 0; k < 8; k++) c = (c & 1) ? (0xEDB88320 ^ (c >>> 1)) : (c >>> 1);
          table[i] = c >>> 0;
        }
      }
      let crc = 0xFFFFFFFF;
      for (let i = 0; i < bytes.length; i++) {
        crc = (crc >>> 8) ^ table[(crc ^ bytes[i]) & 0xFF];
      }
      return (crc ^ 0xFFFFFFFF) >>> 0;
    }

    function strToU8(str) { return new TextEncoder().encode(str); }

    function buildZip(files) {
      const fileEntries = [];
      let offset = 0;
      const chunks = [];

      files.forEach(function(f) {
        const nameBytes = strToU8(f.name);
        const dataBytes = strToU8(f.data);
        const crc = crc32(dataBytes);
        const size = dataBytes.length;

        const lfh = new DataView(new ArrayBuffer(30));
        lfh.setUint32(0, 0x04034b50, true);
        lfh.setUint16(4, 20, true);
        lfh.setUint16(6, 0, true);
        lfh.setUint16(8, 0, true);
        lfh.setUint16(10, 0, true);
        lfh.setUint16(12, 0, true);
        lfh.setUint32(14, crc, true);
        lfh.setUint32(18, size, true);
        lfh.setUint32(22, size, true);
        lfh.setUint16(26, nameBytes.length, true);
        lfh.setUint16(28, 0, true);

        chunks.push(new Uint8Array(lfh.buffer));
        chunks.push(nameBytes);
        chunks.push(dataBytes);

        fileEntries.push({ nameBytes, crc, size, offset });
        offset += 30 + nameBytes.length + size;
      });

      const cdStart = offset;
      let cdSize = 0;
      fileEntries.forEach(function(f) {
        const cdh = new DataView(new ArrayBuffer(46));
        cdh.setUint32(0, 0x02014b50, true);
        cdh.setUint16(4, 20, true);
        cdh.setUint16(6, 20, true);
        cdh.setUint16(8, 0, true);
        cdh.setUint16(10, 0, true);
        cdh.setUint16(12, 0, true);
        cdh.setUint16(14, 0, true);
        cdh.setUint32(16, f.crc, true);
        cdh.setUint32(20, f.size, true);
        cdh.setUint32(24, f.size, true);
        cdh.setUint16(28, f.nameBytes.length, true);
        cdh.setUint16(30, 0, true);
        cdh.setUint16(32, 0, true);
        cdh.setUint16(34, 0, true);
        cdh.setUint16(36, 0, true);
        cdh.setUint32(38, 0, true);
        cdh.setUint32(42, f.offset, true);
        chunks.push(new Uint8Array(cdh.buffer));
        chunks.push(f.nameBytes);
        cdSize += 46 + f.nameBytes.length;
      });

      const eocd = new DataView(new ArrayBuffer(22));
      eocd.setUint32(0, 0x06054b50, true);
      eocd.setUint16(4, 0, true);
      eocd.setUint16(6, 0, true);
      eocd.setUint16(8, fileEntries.length, true);
      eocd.setUint16(10, fileEntries.length, true);
      eocd.setUint32(12, cdSize, true);
      eocd.setUint32(16, cdStart, true);
      eocd.setUint16(20, 0, true);
      chunks.push(new Uint8Array(eocd.buffer));

      let total = 0;
      chunks.forEach(function(c) { total += c.length; });
      const out = new Uint8Array(total);
      let pos = 0;
      chunks.forEach(function(c) { out.set(c, pos); pos += c.length; });
      return out;
    }

    function exportFullExcel() {
      try {
        const wb = buildFullWorkbook();
        const files = [
          { name: '[Content_Types].xml', data: wb.contentTypes },
          { name: '_rels/.rels',         data: wb.rootRels },
          { name: 'xl/workbook.xml',     data: wb.workbookXml },
          { name: 'xl/_rels/workbook.xml.rels', data: wb.workbookRels },
          { name: 'xl/worksheets/sheet1.xml',   data: wb.sheetXml },
          { name: 'xl/styles.xml',       data: wb.stylesXml }
        ];
        const zipBytes = buildZip(files);
        const blob = new Blob([zipBytes], { type: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet' });
        const url = URL.createObjectURL(blob);
        const a = document.createElement('a');
        const today = getTodayParts();
        const safeUser = String(db.currentUser).replace(/[^\w\-]+/g, '_');
        const filename = safeUser + '-' + isoDate(today) + '.xlsx';
        a.href = url;
        a.download = filename;
        a.style.display = 'none';
        document.body.appendChild(a);
        a.click();
        setTimeout(function() {
          document.body.removeChild(a);
          URL.revokeObjectURL(url);
        }, 1500);

        try {
          db.backupMeta = {
            filename: filename,
            exportedAt: new Date().toISOString(),
            user: db.currentUser
          };
          localStorage.setItem(STORAGE_KEY, JSON.stringify(db));
        } catch (e) {}
        updateBackupInfo();
        showToast('Saved: ' + filename);
      } catch (e) {
        alert('Could not create Excel file: ' + e.message);
      }
    }

    function updateBackupInfo() {
      let meta = db.backupMeta;
      if (!meta || !meta.exportedAt) {
        backupInfo.innerHTML = 'Tap <strong>Export</strong> below to save ' + db.currentUser + '\'s month.';
      } else {
        const d = new Date(meta.exportedAt);
        const stamp = d.toLocaleString('en-GB', {
          day: '2-digit', month: 'short', year: 'numeric',
          hour: '2-digit', minute: '2-digit'
        });
        const who = meta.user && meta.user !== db.currentUser ? ' (' + meta.user + ')' : '';
        backupInfo.innerHTML = 'Last export: <strong>' + escXml(stamp) + who + '</strong>';
      }
    }

    /* =========================================================
       SWITCH TECHNICIAN
       ========================================================= */
    function switchTechnician(newUser) {
      if (!TECHNICIANS.includes(newUser)) return;
      if (newUser === db.currentUser) return;

      const prevProf = currentProfile();
      prevProf.lastUpdated = new Date().toISOString();
      try {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(db));
      } catch (e) {}

      switchingText.textContent = 'Loading ' + newUser + '...';
      switchingOverlay.classList.add('show');

      setTimeout(function() {
        db.currentUser = newUser;
        db.profiles[newUser] = db.profiles[newUser] || emptyProfile();

        const prof = currentProfile();
        monthSelect.value = prof.view.month;
        yearSelect.value = prof.view.year;

        updateProfileStrip();
        buildTable(prof.view.year, prof.view.month);
        updateTodayProgress();
        updateStorageInfo();
        updateBackupInfo();

        try {
          localStorage.setItem(CURRENT_USER_KEY, db.currentUser);
          localStorage.setItem(STORAGE_KEY, JSON.stringify(db));
        } catch (e) {}

        switchingOverlay.classList.remove('show');
      }, 220);
    }

    /* =========================================================
       INIT
       ========================================================= */
    function init() {
      const thisYear = new Date().getFullYear();
      yearSelect.innerHTML = '';
      for (let y = thisYear - 5; y <= thisYear + 5; y++) {
        const opt = document.createElement('option');
        opt.value = y; opt.textContent = y;
        yearSelect.appendChild(opt);
      }

      const saved = readFromStorage();
      if (saved) {
        db = saved;
        TECHNICIANS.forEach(function(name) {
          if (!db.profiles[name]) db.profiles[name] = emptyProfile();
        });
        if (!TECHNICIANS.includes(db.currentUser)) db.currentUser = TECHNICIANS[0];
      } else {
        db = emptyDatabase();
        try {
          const savedUser = localStorage.getItem(CURRENT_USER_KEY);
          if (savedUser && TECHNICIANS.includes(savedUser)) db.currentUser = savedUser;
        } catch (e) {}
      }

      userSelect.value = db.currentUser;
      const prof = currentProfile();
      monthSelect.value = prof.view.month;
      yearSelect.value = prof.view.year;

      updateProfileStrip();
      buildTable(prof.view.year, prof.view.month);
      updateTodayProgress();
      updateStorageInfo();
      updateBackupInfo();

      if (!storageAvailable) {
        storageStatus.textContent = 'Unavailable';
        storageStatus.className = 'storage-status warn';
        storageInfo.textContent = 'This browser blocks local storage.';
      }

      userSelect.addEventListener('change', function() {
        switchTechnician(userSelect.value);
      });

      monthSelect.addEventListener('change', function() {
        const m = parseInt(monthSelect.value, 10);
        const prof = currentProfile();
        prof.view.month = m;
        buildTable(prof.view.year, m);
        writeToStorage();
      });
      yearSelect.addEventListener('change', function() {
        const y = parseInt(yearSelect.value, 10);
        const prof = currentProfile();
        prof.view.year = y;
        buildTable(y, prof.view.month);
        writeToStorage();
      });

      document.getElementById('exportExcelBtn').addEventListener('click', exportFullExcel);
      document.getElementById('whatsappBtn').addEventListener('click', sendToWhatsApp);
      document.getElementById('copyDayBtn').addEventListener('click', copyToday);

      document.getElementById('modalClose').addEventListener('click', closeDayModal);
      document.getElementById('modalDone').addEventListener('click', closeDayModal);
      modalBackdrop.addEventListener('click', function(e) {
        if (e.target === modalBackdrop) closeDayModal();
      });
      document.getElementById('modalClear').addEventListener('click', function() {
        const grid = getGrid(modalCtx.year, modalCtx.month);
        TASKS.forEach(function(_, tIdx) { grid[tIdx][modalCtx.day] = false; });
        modalTasks.querySelectorAll('.modal-task').forEach(function(el) {
          el.classList.remove('completed');
          const cb = el.querySelector('input[type="checkbox"]');
          if (cb) cb.checked = false;
        });
        TASKS.forEach(function(_, tIdx) { syncTableCheckbox(tIdx, modalCtx.day, false); });
        if (isToday(modalCtx.year, modalCtx.month, modalCtx.day)) updateTodayProgress();
        writeToStorage();
      });

      window.addEventListener('beforeunload', function() {
        try {
          const prof = currentProfile();
          prof.lastUpdated = new Date().toISOString();
          localStorage.setItem(STORAGE_KEY, JSON.stringify(db));
          localStorage.setItem(CURRENT_USER_KEY, db.currentUser);
        } catch (e) {}
      });
    }

    init();
  })();
</script>
</body>
</html>
