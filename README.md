# venz-aktivis
VIPER_TOOLS

import { useState, useEffect, useRef } from "react";

const COLORS = {
  bg: "#020d04",
  bgCard: "#050f06",
  border: "#0f4a12",
  borderBright: "#16a820",
  green: "#00ff41",
  greenDim: "#00cc33",
  greenDark: "#0a4010",
  cyan: "#00ffcc",
  cyanDim: "#00bb99",
  yellow: "#ffe600",
  yellowDim: "#cc9900",
  red: "#ff2020",
  magenta: "#ff00ff",
  white: "#d0f0d4",
  dimText: "#2a7a30",
};

const glitchStyle = `
  @import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=VT323&family=Orbitron:wght@400;700;900&display=swap');

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: ${COLORS.bg};
    color: ${COLORS.green};
    font-family: 'Share Tech Mono', monospace;
    overflow: hidden;
  }

  ::-webkit-scrollbar { width: 4px; }
  ::-webkit-scrollbar-track { background: #020d04; }
  ::-webkit-scrollbar-thumb { background: ${COLORS.borderBright}; border-radius: 2px; }

  @keyframes scanline {
    0% { transform: translateY(-100%); }
    100% { transform: translateY(100vh); }
  }
  @keyframes blink {
    0%, 100% { opacity: 1; }
    50% { opacity: 0; }
  }
  @keyframes glitch {
    0%, 100% { text-shadow: 2px 0 ${COLORS.red}, -2px 0 ${COLORS.cyan}; }
    25% { text-shadow: -2px 0 ${COLORS.red}, 2px 0 ${COLORS.cyan}; }
    50% { text-shadow: 2px 2px ${COLORS.red}, -2px -2px ${COLORS.cyan}; }
    75% { text-shadow: -2px 2px ${COLORS.red}, 2px -2px ${COLORS.cyan}; }
  }
  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(8px); }
    to { opacity: 1; transform: translateY(0); }
  }
  @keyframes borderPulse {
    0%, 100% { border-color: ${COLORS.borderBright}; box-shadow: 0 0 6px ${COLORS.greenDark}; }
    50% { border-color: ${COLORS.green}; box-shadow: 0 0 16px #00ff4133; }
  }
  @keyframes matrixRain {
    0% { opacity: 1; }
    100% { opacity: 0; transform: translateY(20px); }
  }
  @keyframes slideIn {
    from { opacity: 0; transform: translateX(-20px); }
    to { opacity: 1; transform: translateX(0); }
  }
  @keyframes titleGlitch {
    0%, 90%, 100% { clip-path: none; transform: none; }
    91% { clip-path: inset(20% 0 60% 0); transform: translateX(-4px); }
    93% { clip-path: inset(60% 0 20% 0); transform: translateX(4px); }
    95% { clip-path: inset(40% 0 40% 0); transform: translateX(-2px); }
  }

  .viper-app {
    width: 100vw;
    height: 100vh;
    background: ${COLORS.bg};
    position: relative;
    overflow: hidden;
  }

  .scanlines {
    position: fixed;
    inset: 0;
    background: repeating-linear-gradient(
      0deg,
      transparent,
      transparent 2px,
      rgba(0,0,0,0.08) 2px,
      rgba(0,0,0,0.08) 4px
    );
    pointer-events: none;
    z-index: 100;
  }

  .scanline-move {
    position: fixed;
    left: 0; right: 0;
    height: 3px;
    background: linear-gradient(transparent, rgba(0,255,65,0.06), transparent);
    animation: scanline 5s linear infinite;
    pointer-events: none;
    z-index: 101;
  }

  .crt-corner {
    position: fixed;
    width: 40px; height: 40px;
    border-color: ${COLORS.borderBright};
    border-style: solid;
    opacity: 0.4;
    pointer-events: none;
    z-index: 102;
  }
  .crt-tl { top: 8px; left: 8px; border-width: 2px 0 0 2px; }
  .crt-tr { top: 8px; right: 8px; border-width: 2px 2px 0 0; }
  .crt-bl { bottom: 8px; left: 8px; border-width: 0 0 2px 2px; }
  .crt-br { bottom: 8px; right: 8px; border-width: 0 2px 2px 0; }

  .main-layout {
    display: flex;
    flex-direction: column;
    height: 100vh;
    padding: 12px 16px;
    gap: 10px;
    animation: fadeIn 0.4s ease;
  }

  .topbar {
    display: flex;
    align-items: center;
    justify-content: space-between;
    border-bottom: 1px solid ${COLORS.border};
    padding-bottom: 8px;
  }

  .logo-text {
    font-family: 'Orbitron', monospace;
    font-size: 22px;
    font-weight: 900;
    color: ${COLORS.green};
    animation: glitch 4s infinite;
    letter-spacing: 4px;
  }

  .status-badge {
    font-size: 10px;
    color: ${COLORS.green};
    border: 1px solid ${COLORS.borderBright};
    padding: 2px 8px;
    animation: borderPulse 2s infinite;
  }

  .topbar-right {
    display: flex;
    flex-direction: column;
    align-items: flex-end;
    gap: 2px;
    font-size: 10px;
    color: ${COLORS.dimText};
  }

  .content-area {
    flex: 1;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }

  /* ─── HOME GRID ─── */
  .home-grid {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-template-rows: 1fr 1fr;
    gap: 10px;
    flex: 1;
    animation: fadeIn 0.3s ease;
  }

  .cat-card {
    border: 1px solid ${COLORS.border};
    background: ${COLORS.bgCard};
    cursor: pointer;
    position: relative;
    overflow: hidden;
    transition: all 0.15s;
    display: flex;
    flex-direction: column;
    padding: 16px;
    animation: fadeIn 0.4s ease both;
  }
  .cat-card:hover {
    border-color: ${COLORS.borderBright};
    box-shadow: 0 0 20px #00ff4122, inset 0 0 20px #00ff410a;
    transform: translateY(-1px);
  }
  .cat-card::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, rgba(0,255,65,0.04) 0%, transparent 60%);
    pointer-events: none;
  }

  .cat-icon {
    font-size: 28px;
    margin-bottom: 8px;
    filter: drop-shadow(0 0 8px currentColor);
  }
  .cat-label {
    font-family: 'Orbitron', monospace;
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 2px;
    margin-bottom: 6px;
  }
  .cat-desc {
    font-size: 9px;
    color: ${COLORS.dimText};
    line-height: 1.5;
    flex: 1;
  }
  .cat-count {
    font-size: 9px;
    margin-top: 10px;
    padding-top: 8px;
    border-top: 1px solid ${COLORS.border};
    color: ${COLORS.dimText};
  }
  .cat-arrow {
    position: absolute;
    bottom: 12px;
    right: 14px;
    font-size: 16px;
    opacity: 0;
    transition: opacity 0.15s, right 0.15s;
  }
  .cat-card:hover .cat-arrow {
    opacity: 1;
    right: 10px;
  }

  /* ─── SUB MENU PAGE ─── */
  .sub-page {
    display: flex;
    flex-direction: column;
    flex: 1;
    gap: 10px;
    animation: slideIn 0.25s ease;
  }

  .sub-header {
    display: flex;
    align-items: center;
    gap: 12px;
    border-bottom: 1px solid ${COLORS.border};
    padding-bottom: 10px;
  }

  .back-btn {
    background: none;
    border: 1px solid ${COLORS.border};
    color: ${COLORS.green};
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    padding: 4px 12px;
    cursor: pointer;
    transition: all 0.15s;
    letter-spacing: 1px;
  }
  .back-btn:hover {
    border-color: ${COLORS.borderBright};
    background: ${COLORS.greenDark};
    box-shadow: 0 0 10px #00ff4120;
  }

  .sub-title {
    font-family: 'Orbitron', monospace;
    font-size: 14px;
    font-weight: 700;
    letter-spacing: 3px;
  }

  .sub-icon { font-size: 20px; }

  .menu-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 8px;
    flex: 1;
    overflow-y: auto;
    padding-right: 4px;
  }

  .menu-item {
    border: 1px solid ${COLORS.border};
    background: ${COLORS.bgCard};
    cursor: pointer;
    display: flex;
    align-items: flex-start;
    gap: 12px;
    padding: 14px 16px;
    transition: all 0.15s;
    animation: fadeIn 0.3s ease both;
    position: relative;
    overflow: hidden;
  }
  .menu-item:hover {
    border-color: ${COLORS.borderBright};
    background: #050f0600;
    box-shadow: 0 0 14px #00ff4118, inset 0 0 14px #00ff4108;
  }
  .menu-item::after {
    content: '';
    position: absolute;
    left: 0; top: 0; bottom: 0;
    width: 2px;
    background: ${COLORS.green};
    opacity: 0;
    transition: opacity 0.15s;
  }
  .menu-item:hover::after { opacity: 1; }

  .item-num {
    font-family: 'VT323', monospace;
    font-size: 28px;
    line-height: 1;
    opacity: 0.5;
    min-width: 28px;
    transition: opacity 0.15s;
  }
  .menu-item:hover .item-num { opacity: 1; }

  .item-body { flex: 1; }
  .item-name {
    font-size: 12px;
    color: ${COLORS.white};
    margin-bottom: 3px;
    letter-spacing: 1px;
  }
  .item-desc {
    font-size: 9px;
    color: ${COLORS.dimText};
    line-height: 1.4;
  }
  .item-icon { font-size: 16px; align-self: center; margin-left: 4px; }

  /* ─── TERMINAL ─── */
  .terminal-overlay {
    position: fixed;
    inset: 0;
    background: rgba(2,13,4,0.96);
    z-index: 200;
    display: flex;
    flex-direction: column;
    animation: fadeIn 0.2s ease;
  }

  .terminal-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 10px 16px;
    border-bottom: 1px solid ${COLORS.border};
    background: #030a04;
  }

  .terminal-title {
    font-family: 'Orbitron', monospace;
    font-size: 11px;
    color: ${COLORS.green};
    letter-spacing: 2px;
  }

  .close-btn {
    background: none;
    border: 1px solid ${COLORS.border};
    color: ${COLORS.red};
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    padding: 3px 10px;
    cursor: pointer;
    transition: all 0.15s;
  }
  .close-btn:hover {
    border-color: ${COLORS.red};
    background: rgba(255,32,32,0.1);
  }

  .terminal-body {
    flex: 1;
    overflow-y: auto;
    padding: 16px;
    font-size: 12px;
    line-height: 1.7;
  }

  .term-line { display: block; }
  .term-line.green { color: ${COLORS.green}; }
  .term-line.cyan { color: ${COLORS.cyan}; }
  .term-line.yellow { color: ${COLORS.yellow}; }
  .term-line.red { color: ${COLORS.red}; }
  .term-line.magenta { color: ${COLORS.magenta}; }
  .term-line.dim { color: ${COLORS.dimText}; }
  .term-line.white { color: ${COLORS.white}; }

  .cursor {
    display: inline-block;
    width: 8px;
    height: 14px;
    background: ${COLORS.green};
    animation: blink 1s infinite;
    vertical-align: middle;
    margin-left: 2px;
  }

  .terminal-input-bar {
    display: flex;
    align-items: center;
    padding: 10px 16px;
    border-top: 1px solid ${COLORS.border};
    gap: 8px;
    background: #030a04;
  }

  .term-prompt {
    color: ${COLORS.green};
    font-size: 12px;
    white-space: nowrap;
  }

  .term-input {
    flex: 1;
    background: none;
    border: none;
    outline: none;
    color: ${COLORS.white};
    font-family: 'Share Tech Mono', monospace;
    font-size: 12px;
    caret-color: ${COLORS.green};
  }

  /* ─── STATUSBAR ─── */
  .statusbar {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 9px;
    color: ${COLORS.dimText};
    border-top: 1px solid ${COLORS.border};
    padding-top: 8px;
  }

  .status-dot {
    display: inline-block;
    width: 6px; height: 6px;
    border-radius: 50%;
    background: ${COLORS.green};
    margin-right: 5px;
    animation: blink 2s infinite;
  }
`;

// ─── DATA ───────────────────────────────────────────────────
const CATEGORIES = [
  {
    id: "network",
    label: "NETWORK",
    icon: "🌐",
    color: COLORS.green,
    desc: "Scan & analisa jaringan target secara real-time",
    items: [
      { num: "01", name: "Quick Scan", desc: "Quick network info: ping, DNS, port check", icon: "⚡", action: "quick_info" },
      { num: "02", name: "Multi Scan", desc: "Scan banyak target sekaligus (parallel)", icon: "🔁", action: "multi_scan" },
      { num: "03", name: "Trace Route", desc: "Lacak rute paket ke target (max 20 hop)", icon: "🛤️", action: "trace_route" },
      { num: "04", name: "Ping Target", desc: "ICMP ping test dengan jumlah paket custom", icon: "📡", action: "ping_target" },
      { num: "05", name: "Port Scan", desc: "Threaded port scan + banner grab", icon: "🔍", action: "port_scan" },
    ],
  },
  {
    id: "webintel",
    label: "WEB INTEL",
    icon: "🕸️",
    color: COLORS.cyan,
    desc: "Intelijen & analisa keamanan web target",
    items: [
      { num: "06", name: "Web Info", desc: "HTTP header + DNS info website", icon: "🌍", action: "web_info" },
      { num: "07", name: "HTTP Header", desc: "Analisa security headers & fingerprint", icon: "📋", action: "http_header" },
      { num: "08", name: "Directory Scan", desc: "Brute-force path & file sensitif", icon: "📁", action: "dir_scan" },
      { num: "09", name: "Subdomain Scan", desc: "Enumerasi subdomain aktif", icon: "🌿", action: "subdomain" },
      { num: "10", name: "SSL Check", desc: "Cek sertifikat SSL & expiry date", icon: "🔐", action: "ssl_check" },
    ],
  },
  {
    id: "osint",
    label: "OSINT",
    icon: "🔎",
    color: COLORS.yellow,
    desc: "Open source intelligence gathering",
    items: [
      { num: "11", name: "WHOIS Lookup", desc: "Registrar, nama server, tanggal expire", icon: "📜", action: "whois" },
      { num: "12", name: "GEO IP", desc: "Lokasi geografis IP/domain target", icon: "📍", action: "geo_ip" },
      { num: "13", name: "DNS Lookup", desc: "DNS record lengkap: A, MX, NS, dll", icon: "🗂️", action: "dns_lookup" },
    ],
  },
  {
    id: "system",
    label: "SYSTEM",
    icon: "💻",
    color: COLORS.green,
    desc: "Informasi & monitoring sistem lokal",
    items: [
      { num: "14", name: "Firewall Check", desc: "Status iptables / UFW / nftables", icon: "🛡️", action: "firewall" },
      { num: "15", name: "System Info", desc: "OS, CPU, RAM, Disk, IP lokal & publik", icon: "🖥️", action: "sysinfo" },
    ],
  },
  {
    id: "advanced",
    label: "ADVANCED",
    icon: "⚙️",
    color: COLORS.magenta,
    desc: "Alat scan lanjutan & laporan keamanan",
    items: [
      { num: "16", name: "Nmap Scan", desc: "Fast / SV / OS / Full / UDP / Stealth SYN", icon: "🎯", action: "nmap" },
      { num: "17", name: "Full Report", desc: "Laporan keamanan lengkap 5 tahap", icon: "📊", action: "full_report" },
      { num: "18", name: "Save Output", desc: "Simpan hasil scan ke scan_result.txt", icon: "💾", action: "save_output" },
    ],
  },
  {
    id: "aitools",
    label: "AI TOOLS",
    icon: "🤖",
    color: COLORS.cyan,
    desc: "Fitur berbasis AI & otomasi",
    items: [
      { num: "19", name: "AI Voice", desc: "Text-to-speech via pyttsx3", icon: "🔊", action: "ai_voice" },
      { num: "20", name: "Fast Scan", desc: "Ping banyak host paralel super cepat", icon: "💨", action: "fast_scan" },
    ],
  },
];

// ─── TERMINAL SIMULATION ────────────────────────────────────
const SIMULATIONS = {
  quick_info: (t) => [
    { t: "cyan", v: `[*] QUICK NETWORK INFO :: ${t}` },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan", v: `  Target : ${t}` },
    { t: "green", v: `  IP     : 192.168.${Math.floor(Math.random()*255)}.${Math.floor(Math.random()*255)}` },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan", v: "\n[PING TEST - 4 packets]" },
    { t: "green", v: `  64 bytes from ${t}: icmp_seq=1 time=12.4 ms` },
    { t: "green", v: `  64 bytes from ${t}: icmp_seq=2 time=11.8 ms` },
    { t: "green", v: `  64 bytes from ${t}: icmp_seq=3 time=13.1 ms` },
    { t: "green", v: `  64 bytes from ${t}: icmp_seq=4 time=12.9 ms` },
    { t: "green", v: `  rtt min/avg/max = 11.8/12.5/13.1 ms` },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan", v: "\n[QUICK PORT CHECK]" },
    { t: "green", v: "  OPEN    80   (HTTP)" },
    { t: "green", v: "  OPEN    443  (HTTPS)" },
    { t: "red",   v: "  CLOSED  22   (SSH)" },
    { t: "red",   v: "  CLOSED  21   (FTP)" },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan", v: "[+] Quick scan selesai." },
  ],
  port_scan: (t) => [
    { t: "cyan", v: `[*] PORT SCAN :: ${t}` },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan", v: "  Mode   : Threaded + Banner Grab" },
    { t: "cyan", v: "  Ports  : 24 (common)" },
    { t: "dim", v: "" },
    { t: "red",   v: "  [CLOSED]   21   FTP" },
    { t: "red",   v: "  [CLOSED]   22   SSH" },
    { t: "red",   v: "  [CLOSED]   23   TELNET" },
    { t: "red",   v: "  [CLOSED]   25   SMTP" },
    { t: "green", v: "  [OPEN]     80   HTTP           | Apache/2.4.52" },
    { t: "red",   v: "  [CLOSED]  110   POP3" },
    { t: "red",   v: "  [CLOSED]  143   IMAP" },
    { t: "green", v: "  [OPEN]    443   HTTPS          | nginx/1.22.0" },
    { t: "red",   v: "  [CLOSED]  445   SMB" },
    { t: "red",   v: "  [CLOSED] 3306   MYSQL" },
    { t: "red",   v: "  [CLOSED] 5432   PGSQL" },
    { t: "green", v: "  [OPEN]   8080   HTTP-ALT       | Tomcat/9.0" },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan", v: "[+] Selesai. 3/24 port terbuka." },
    { t: "green", v: "    Open: [80, 443, 8080]" },
  ],
  trace_route: (t) => [
    { t: "cyan", v: `[*] TRACE ROUTE :: ${t} (max 20 hop)` },
    { t: "dim", v: "─".repeat(42) },
    ...Array.from({length: 8}, (_, i) => ({
      t: i < 2 ? "red" : "green",
      v: `  ${i+1}  ${i < 2 ? "* * *" : `192.168.${i}.${Math.floor(Math.random()*254)+1}  ${Math.floor(Math.random()*30)+5} ms`}`,
    })),
    { t: "green", v: `  9  ${t}  24.3 ms  24.1 ms  24.7 ms` },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan", v: "[+] Traceroute selesai." },
  ],
  ping_target: (t) => [
    { t: "cyan", v: `[*] PING :: ${t} x5` },
    { t: "dim", v: "─".repeat(42) },
    ...Array.from({length: 5}, (_, i) => ({
      t: "green",
      v: `  64 bytes: icmp_seq=${i+1} ttl=64 time=${(Math.random()*20+5).toFixed(1)} ms`,
    })),
    { t: "green", v: `  --- ${t} ping statistics ---` },
    { t: "green", v: "  5 packets transmitted, 5 received, 0% packet loss" },
    { t: "cyan", v: "[+] Ping selesai." },
  ],
  multi_scan: (t) => {
    const targets = t.split(/\s+/).filter(Boolean);
    return [
      { t: "cyan", v: `[*] FAST SCAN :: ${targets.length} target parallel` },
      { t: "dim", v: "─".repeat(42) },
      ...targets.map((host, i) => ({
        t: Math.random() > 0.3 ? "green" : "red",
        v: Math.random() > 0.3
          ? `  [UP]   ${host.padEnd(22)} -> 192.168.${i}.${Math.floor(Math.random()*254)+1}  (${Math.floor(Math.random()*30)+3}ms)`
          : `  [DOWN] ${host}`,
      })),
      { t: "dim", v: "─".repeat(42) },
      { t: "cyan", v: `[+] ${Math.ceil(targets.length * 0.7)}/${targets.length} host aktif.` },
    ];
  },
  http_header: (t) => [
    { t: "cyan", v: `[*] HTTP HEADER :: ${t}` },
    { t: "dim", v: "─".repeat(42) },
    { t: "green", v: "  HTTP/1.1 200 OK" },
    { t: "white", v: "  Date: Thu, 01 May 2025 12:00:00 GMT" },
    { t: "yellow", v: "  Server: nginx/1.22.0  <- fingerprint" },
    { t: "white", v: "  Content-Type: text/html; charset=UTF-8" },
    { t: "green", v: "  Strict-Transport-Security: max-age=31536000" },
    { t: "green", v: "  X-Frame-Options: SAMEORIGIN" },
    { t: "green", v: "  X-Content-Type-Options: nosniff" },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan", v: "\n[SECURITY HEADERS]" },
    { t: "green",   v: "  [OK]  HSTS" },
    { t: "green",   v: "  [OK]  Clickjacking Protect" },
    { t: "green",   v: "  [OK]  MIME Sniff Protect" },
    { t: "red",     v: "  [!!]  CSP — MISSING" },
    { t: "red",     v: "  [!!]  XSS Protect — MISSING" },
    { t: "red",     v: "  [!!]  Permissions Policy — MISSING" },
    { t: "cyan", v: "\n[+] Header scan selesai." },
  ],
  dir_scan: (t) => [
    { t: "cyan", v: `[*] DIR SCAN :: ${t}` },
    { t: "cyan", v: "  Paths  : 35 | Threaded" },
    { t: "dim", v: "─".repeat(42) },
    { t: "red",     v: `  [404 MISS]  ${t}/admin` },
    { t: "green",   v: `  [200 FOUND] ${t}/robots.txt  [128B]` },
    { t: "yellow",  v: `  [301 REDIR] ${t}/dashboard` },
    { t: "cyan",    v: `  [403 FORBID]${t}/.git/config` },
    { t: "green",   v: `  [200 FOUND] ${t}/sitemap.xml  [2048B]` },
    { t: "red",     v: `  [404 MISS]  ${t}/phpmyadmin` },
    { t: "magenta", v: `  [401 AUTH]  ${t}/wp-admin` },
    { t: "red",     v: `  [404 MISS]  ${t}/.env` },
    { t: "green",   v: `  [200 FOUND] ${t}/api/v1  [512B]` },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan", v: "[+] Selesai. 3 path ditemukan." },
    { t: "green", v: `    ${t}/robots.txt` },
    { t: "green", v: `    ${t}/sitemap.xml` },
    { t: "green", v: `    ${t}/api/v1` },
  ],
  subdomain: (t) => {
    const subs = ["www","api","mail","dev","beta","cdn","shop","app","admin","staging"];
    const found = subs.filter(() => Math.random() > 0.5);
    return [
      { t: "cyan", v: `[*] SUBDOMAIN SCAN :: ${t}` },
      { t: "cyan", v: `  Words : 45 subdomain` },
      { t: "dim", v: "─".repeat(42) },
      ...found.map(s => ({
        t: "green",
        v: `  [FOUND] ${(s+"."+t).padEnd(35)} -> 104.${Math.floor(Math.random()*255)}.${Math.floor(Math.random()*255)}.${Math.floor(Math.random()*255)}`,
      })),
      ...subs.filter(s => !found.includes(s)).map(s => ({ t: "dim", v: `  [     ] ${s}.${t}` })),
      { t: "dim", v: "─".repeat(42) },
      { t: "cyan", v: `[+] Selesai. ${found.length} subdomain aktif.` },
    ];
  },
  ssl_check: (t) => [
    { t: "cyan", v: `[*] SSL CHECK :: ${t}:443` },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan", v: `  subject= CN=${t}` },
    { t: "cyan", v: "  issuer= C=US, O=Let's Encrypt, CN=R3" },
    { t: "green", v: "  Issued  : notBefore=Jan  1 00:00:00 2025 GMT" },
    { t: "green", v: "  Expired : notAfter=Apr  1 00:00:00 2026 GMT (329 hari lagi)" },
    { t: "yellow", v: "  Protocol: TLSv1.3" },
    { t: "yellow", v: "  Cipher  : TLS_AES_256_GCM_SHA384" },
    { t: "green", v: "  [OK] Verify return code: 0 (ok)" },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan", v: "[+] SSL Check selesai." },
  ],
  whois: (t) => [
    { t: "cyan", v: `[*] WHOIS :: ${t}` },
    { t: "dim", v: "─".repeat(42) },
    { t: "yellow", v: `  Registrar: GoDaddy.com, LLC` },
    { t: "white",  v: `  Registered On: 2010-03-15` },
    { t: "yellow", v: `  Expires On: 2026-03-15` },
    { t: "yellow", v: `  Updated On: 2024-02-10` },
    { t: "yellow", v: `  Status: clientTransferProhibited` },
    { t: "yellow", v: `  Name Server: ns1.${t}` },
    { t: "yellow", v: `  Name Server: ns2.${t}` },
    { t: "white",  v: `  Registrant: REDACTED FOR PRIVACY` },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan", v: "[+] WHOIS selesai." },
  ],
  geo_ip: (t) => [
    { t: "cyan", v: `[*] GEO IP :: ${t}` },
    { t: "dim", v: "─".repeat(42) },
    { t: "green", v: `  IP        : 104.21.${Math.floor(Math.random()*255)}.${Math.floor(Math.random()*255)}` },
    { t: "green", v: `  Hostname  : ${t}` },
    { t: "green", v: `  Kota      : Jakarta` },
    { t: "green", v: `  Region    : DKI Jakarta` },
    { t: "green", v: `  Negara    : ID` },
    { t: "green", v: `  Lokasi    : -6.2146,106.8451` },
    { t: "green", v: `  ISP/ORG   : AS13335 Cloudflare, Inc.` },
    { t: "green", v: `  Postal    : 10110` },
    { t: "green", v: `  Timezone  : Asia/Jakarta` },
    { t: "cyan",  v: `\n  Maps: https://maps.google.com/?q=-6.2146,106.8451` },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan", v: "[+] GeoIP selesai." },
  ],
  dns_lookup: (t) => [
    { t: "cyan", v: `[*] DNS LOOKUP :: ${t}` },
    { t: "dim", v: "─".repeat(42) },
    { t: "green", v: `  [DNS] ${t} -> 104.21.45.12` },
    { t: "cyan",  v: `  [IPv6] 2606:4700:3034::6815:2d0c` },
    { t: "cyan",  v: "\n[DNS Records]" },
    { t: "white", v: `  ${t}.  300  IN  A      104.21.45.12` },
    { t: "white", v: `  ${t}.  300  IN  AAAA   2606:4700:3034::6815:2d0c` },
    { t: "white", v: `  ${t}.  300  IN  MX     10 mail.${t}.` },
    { t: "white", v: `  ${t}.  300  IN  NS     ns1.${t}.` },
    { t: "white", v: `  ${t}.  300  IN  NS     ns2.${t}.` },
    { t: "white", v: `  ${t}.  300  IN  TXT    "v=spf1 include:_spf.google.com ~all"` },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan", v: "[+] DNS Lookup selesai." },
  ],
  firewall: () => [
    { t: "cyan", v: "[*] FIREWALL CHECK" },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan", v: "[iptables -L -n -v]" },
    { t: "white", v: "  Chain INPUT (policy ACCEPT 0 packets)" },
    { t: "white", v: "  Chain FORWARD (policy DROP 0 packets)" },
    { t: "white", v: "  Chain OUTPUT (policy ACCEPT 0 packets)" },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan", v: "[UFW]" },
    { t: "green", v: "  Status: active" },
    { t: "white", v: "  To           Action  From" },
    { t: "green", v: "  22/tcp       ALLOW   Anywhere" },
    { t: "green", v: "  80/tcp       ALLOW   Anywhere" },
    { t: "green", v: "  443/tcp      ALLOW   Anywhere" },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan", v: "[+] Firewall check selesai." },
  ],
  sysinfo: () => [
    { t: "cyan", v: "[*] SYSTEM INFO" },
    { t: "dim", v: "─".repeat(42) },
    { t: "green", v: "  OS       : Linux 5.15.0-aarch64" },
    { t: "green", v: "  Version  : #1 SMP Fri Jan 10 12:00:00 UTC 2025" },
    { t: "green", v: "  Node     : localhost" },
    { t: "green", v: "  Machine  : aarch64" },
    { t: "green", v: "  CPU      : ARMv8" },
    { t: "green", v: "  Local IP : 192.168.1.5" },
    { t: "cyan",  v: "  Public IP: 182.1.xx.xx" },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan", v: "\n[MEMORY]" },
    { t: "white", v: "  total        used        free" },
    { t: "white", v: "  7.6Gi       2.1Gi       4.8Gi" },
    { t: "cyan",  v: "\n[DISK]" },
    { t: "white", v: "  /dev/sda1   32G    12G   20G   37% /" },
    { t: "cyan",  v: "\n  Uptime : up 3 days, 4:22, 1 user, load: 0.12" },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan", v: "[+] System info selesai." },
  ],
  nmap: (t) => [
    { t: "cyan", v: `[*] NMAP SCAN :: ${t}` },
    { t: "cyan", v: "  Mode: Fast scan (-F --open)" },
    { t: "dim", v: "─".repeat(42) },
    { t: "white", v: "  Starting Nmap 7.93 ( https://nmap.org )" },
    { t: "green", v: `  Nmap scan report for ${t}` },
    { t: "white", v: "  Host is up (0.024s latency)." },
    { t: "white", v: "  Not shown: 94 closed ports" },
    { t: "white", v: "  PORT     STATE  SERVICE" },
    { t: "green", v: "  80/tcp   open   http" },
    { t: "green", v: "  443/tcp  open   https" },
    { t: "green", v: "  8080/tcp open   http-proxy" },
    { t: "white", v: `\n  Nmap done: 1 IP address (1 host up) scanned in 1.24 seconds` },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan", v: "[+] Nmap scan selesai." },
  ],
  full_report: (t) => [
    { t: "cyan", v: `[*] FULL SECURITY REPORT :: ${t}` },
    { t: "dim", v: "─".repeat(42) },
    { t: "yellow", v: "[ 1/5 ] DNS" },
    { t: "green",  v: `  ${t} -> 104.21.45.12 [OK]` },
    { t: "yellow", v: "\n[ 2/5 ] PING" },
    { t: "green",  v: "  Status: AKTIF (12.4ms avg)" },
    { t: "yellow", v: "\n[ 3/5 ] PORT SCAN" },
    { t: "green",  v: "  OPEN  80  (HTTP)" },
    { t: "green",  v: "  OPEN  443 (HTTPS)" },
    { t: "cyan",   v: "  [3 port terbuka dari 24]" },
    { t: "yellow", v: "\n[ 4/5 ] HTTP HEADER" },
    { t: "white",  v: "  HTTP/1.1 200 OK" },
    { t: "yellow", v: "  Server: nginx/1.22.0  <- fingerprint" },
    { t: "yellow", v: "\n[ 5/5 ] SSL" },
    { t: "green",  v: "  Expired: Apr 2026 (329 hari lagi)" },
    { t: "green",  v: "  Verify return code: 0 (ok)" },
    { t: "dim", v: "─".repeat(42) },
    { t: "green", v: "[+] Full Security Report Selesai!" },
    { t: "green", v: "    Saved -> scan_result.txt" },
  ],
  save_output: (t) => [
    { t: "cyan", v: `[*] SAVE OUTPUT :: ${t}` },
    { t: "dim", v: "─".repeat(42) },
    { t: "white", v: "  Scanning ports..." },
    { t: "green", v: "  OPEN 80 (HTTP)" },
    { t: "green", v: "  OPEN 443 (HTTPS)" },
    { t: "white", v: "  Writing to scan_result.txt..." },
    { t: "green", v: "[+] Saved -> scan_result.txt" },
  ],
  ai_voice: () => [
    { t: "cyan", v: "[*] AI VOICE MODE" },
    { t: "dim", v: "─".repeat(42) },
    { t: "yellow", v: "  pyttsx3 engine initialized." },
    { t: "white",  v: '  Input: "Hello from VIPER"' },
    { t: "green",  v: "  [SPEAKING] Text-to-speech aktif..." },
    { t: "cyan",   v: "[+] Voice output selesai." },
  ],
  web_info: (t) => [
    { t: "cyan", v: `[*] WEB INFO :: ${t}` },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan",   v: "[HTTP HEADER]" },
    { t: "green",  v: "  HTTP/1.1 200 OK" },
    { t: "yellow", v: "  Server: nginx/1.22.0  <- fingerprint" },
    { t: "white",  v: "  Content-Type: text/html; charset=UTF-8" },
    { t: "green",  v: "  Strict-Transport-Security: max-age=31536000" },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan",   v: "[DNS]" },
    { t: "green",  v: `  ${t} -> 104.21.45.12` },
    { t: "dim", v: "─".repeat(42) },
    { t: "cyan",   v: "[+] Web info selesai." },
  ],
  fast_scan: (t) => {
    const targets = t.split(/\s+/).filter(Boolean);
    return [
      { t: "cyan", v: `[*] FAST SCAN :: ${targets.length} target` },
      { t: "dim", v: "─".repeat(42) },
      ...targets.map((host, i) => {
        const up = Math.random() > 0.3;
        return {
          t: up ? "green" : "red",
          v: up
            ? `  [UP]   ${host.padEnd(22)} -> 192.168.${i+1}.${Math.floor(Math.random()*254)+1}  (${Math.floor(Math.random()*20)+2}ms)`
            : `  [DOWN] ${host}`,
        };
      }),
      { t: "dim", v: "─".repeat(42) },
      { t: "cyan", v: `[+] ${Math.ceil(targets.length * 0.7)}/${targets.length} host aktif.` },
    ];
  },
};

const MULTI_TARGET_ACTIONS = ["multi_scan", "fast_scan"];
const NO_TARGET_ACTIONS = ["firewall", "sysinfo", "ai_voice"];

// ─── CLOCK ───────────────────────────────────────────────────
function useClock() {
  const [time, setTime] = useState(new Date());
  useEffect(() => {
    const t = setInterval(() => setTime(new Date()), 1000);
    return () => clearInterval(t);
  }, []);
  return time;
}

// ─── APP ─────────────────────────────────────────────────────
export default function ViperDashboard() {
  const [page, setPage] = useState("home"); // "home" | cat.id
  const [terminal, setTerminal] = useState(null); // { action, lines, input, waiting }
  const [inputVal, setInputVal] = useState("");
  const [termLines, setTermLines] = useState([]);
  const [waitTarget, setWaitTarget] = useState(false);
  const [pendingAction, setPendingAction] = useState(null);
  const termRef = useRef(null);
  const inputRef = useRef(null);
  const clock = useClock();

  const activeCat = CATEGORIES.find(c => c.id === page);

  function openTerminal(action, label) {
    if (NO_TARGET_ACTIONS.includes(action)) {
      const sim = SIMULATIONS[action];
      const lines = sim ? sim("") : [{ t: "cyan", v: `[*] ${label}` }];
      setTermLines([{ t: "cyan", v: `VIPER@root:# ${label.toLowerCase()}` }, ...lines]);
      setTerminal({ action, label });
      setWaitTarget(false);
      setPendingAction(null);
    } else {
      setTermLines([
        { t: "cyan", v: `VIPER@root:# ${label.toLowerCase()}` },
        { t: "yellow", v: MULTI_TARGET_ACTIONS.includes(action)
          ? "[?] Masukkan targets (pisah spasi):"
          : "[?] Masukkan target (domain/IP):" },
      ]);
      setTerminal({ action, label });
      setWaitTarget(true);
      setPendingAction(action);
    }
    setInputVal("");
    setTimeout(() => inputRef.current?.focus(), 100);
  }

  function handleInput(e) {
    if (e.key !== "Enter") return;
    const val = inputVal.trim();
    if (!val) return;

    if (waitTarget) {
      const sim = SIMULATIONS[pendingAction];
      const newLines = sim ? sim(val) : [{ t: "green", v: `[+] Running ${pendingAction} on ${val}` }];
      setTermLines(prev => [
        ...prev,
        { t: "white", v: `  > ${val}` },
        { t: "dim", v: "" },
        ...newLines,
      ]);
      setWaitTarget(false);
      setPendingAction(null);
      setInputVal("");
    }
  }

  useEffect(() => {
    if (termRef.current) {
      termRef.current.scrollTop = termRef.current.scrollHeight;
    }
  }, [termLines]);

  const colorMap = { green: COLORS.green, cyan: COLORS.cyan, yellow: COLORS.yellow, red: COLORS.red, magenta: COLORS.magenta, dim: COLORS.dimText, white: COLORS.white };

  return (
    <>
      <style>{glitchStyle}</style>
      <div className="viper-app">
        <div className="scanlines" />
        <div className="scanline-move" />
        <div className="crt-corner crt-tl" />
        <div className="crt-corner crt-tr" />
        <div className="crt-corner crt-bl" />
        <div className="crt-corner crt-br" />

        <div className="main-layout">
          {/* TOPBAR */}
          <div className="topbar">
            <div style={{ display: "flex", alignItems: "center", gap: 16 }}>
              <span className="logo-text">VIPER</span>
              <div style={{ display: "flex", flexDirection: "column", gap: 2 }}>
                <span className="status-badge">● SYSTEM ACTIVE</span>
                <span style={{ fontSize: 9, color: COLORS.dimText, letterSpacing: 1 }}>v7.3 VIPER DOWN · VENZBJIRR</span>
              </div>
            </div>
            <div className="topbar-right">
              <span style={{ color: COLORS.green, fontSize: 11, fontFamily: "'VT323', monospace" }}>
                {clock.toLocaleTimeString("en-GB")}
              </span>
              <span>{clock.toLocaleDateString("en-GB")}</span>
              <span>VIPER@root:#</span>
            </div>
          </div>

          {/* CONTENT */}
          <div className="content-area">
            {page === "home" ? (
              <div className="home-grid">
                {CATEGORIES.map((cat, idx) => (
                  <div
                    key={cat.id}
                    className="cat-card"
                    style={{ animationDelay: `${idx * 0.06}s`, borderColor: COLORS.border }}
                    onClick={() => setPage(cat.id)}
                  >
                    <div className="cat-icon" style={{ color: cat.color }}>{cat.icon}</div>
                    <div className="cat-label" style={{ color: cat.color }}>{cat.label}</div>
                    <div className="cat-desc">{cat.desc}</div>
                    <div className="cat-count" style={{ color: cat.color }}>
                      {cat.items.length} fitur tersedia
                    </div>
                    <span className="cat-arrow" style={{ color: cat.color }}>→</span>
                  </div>
                ))}
              </div>
            ) : (
              <div className="sub-page">
                <div className="sub-header">
                  <button className="back-btn" onClick={() => setPage("home")}>← BACK</button>
                  <span className="sub-icon" style={{ color: activeCat?.color }}>{activeCat?.icon}</span>
                  <span className="sub-title" style={{ color: activeCat?.color }}>
                    {activeCat?.label}
                  </span>
                  <span style={{ fontSize: 9, color: COLORS.dimText, marginLeft: 8 }}>
                    {activeCat?.desc}
                  </span>
                </div>
                <div className="menu-grid">
                  {activeCat?.items.map((item, idx) => (
                    <div
                      key={item.action}
                      className="menu-item"
                      style={{ animationDelay: `${idx * 0.07}s` }}
                      onClick={() => openTerminal(item.action, item.name)}
                    >
                      <span className="item-num" style={{ color: activeCat.color }}>{item.num}</span>
                      <div className="item-body">
                        <div className="item-name">{item.name}</div>
                        <div className="item-desc">{item.desc}</div>
                      </div>
                      <span className="item-icon">{item.icon}</span>
                    </div>
                  ))}
                </div>
              </div>
            )}
          </div>

          {/* STATUSBAR */}
          <div className="statusbar">
            <span><span className="status-dot" />VIPER DOWN ACTIVE · {page === "home" ? "MAIN MENU" : activeCat?.label}</span>
            <span>Codename: VIPER-X · Termux CLI · System: Linux aarch64</span>
            <span>Script by: VENZBJIRR</span>
          </div>
        </div>

        {/* TERMINAL OVERLAY */}
        {terminal && (
          <div className="terminal-overlay">
            <div className="terminal-header">
              <span className="terminal-title">
                ⚡ VIPER DOWN :: {terminal.label.toUpperCase()}
              </span>
              <button className="close-btn" onClick={() => { setTerminal(null); setTermLines([]); setWaitTarget(false); }}>
                ✕ CLOSE
              </button>
            </div>
            <div className="terminal-body" ref={termRef}>
              {termLines.map((line, i) => (
                <span
                  key={i}
                  className="term-line"
                  style={{
                    color: colorMap[line.t] || COLORS.white,
                    display: "block",
                    animationDelay: `${i * 0.02}s`,
                    animation: "fadeIn 0.2s ease both",
                  }}
                >
                  {line.v || "\u00A0"}
                </span>
              ))}
              {waitTarget && <span className="cursor" />}
            </div>
            <div className="terminal-input-bar">
              <span className="term-prompt">VIPER@root:#</span>
              <input
                ref={inputRef}
                className="term-input"
                value={inputVal}
                onChange={e => setInputVal(e.target.value)}
                onKeyDown={handleInput}
                placeholder={waitTarget ? "ketik target lalu Enter..." : ""}
                disabled={!waitTarget}
                spellCheck={false}
                autoComplete="off"
              />
            </div>
          </div>
        )}
      </div>
    </>
  );
}
