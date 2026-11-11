<!DOCTYPE html>
<html lang="en" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NetPulse // Automated Network Topology & Traffic Analyzer</title>
    
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        cyber: {
                            dark: '#0a0f1d',
                            card: 'rgba(15, 23, 42, 0.75)',
                            border: 'rgba(56, 189, 248, 0.2)',
                            cyan: '#06b6d4',
                            cyanGlow: '#22d3ee',
                            mint: '#10b981',
                            mintGlow: '#34d399',
                            alert: '#f43f5e'
                        }
                    },
                    fontFamily: {
                        mono: ['Fira Code', 'JetBrains Mono', 'ui-monospace', 'SFMono-Regular', 'Menlo', 'Monaco', 'Consolas', 'monospace']
                    }
                }
            }
        }
    </script>
    
    <!-- Chart.js for real-time graphs -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Fira+Code:wght@300;400;500;600;700&family=Inter:wght@300;400;500;600;700&display=swap');
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: #060913;
            background-image: 
                radial-gradient(circle at 15% 15%, rgba(6, 182, 212, 0.08) 0%, transparent 40%),
                radial-gradient(circle at 85% 85%, rgba(16, 185, 129, 0.06) 0%, transparent 40%),
                linear-gradient(rgba(255, 255, 255, 0.01) 1px, transparent 1px),
                linear-gradient(90deg, rgba(255, 255, 255, 0.01) 1px, transparent 1px);
            background-size: 100% 100%, 100% 100%, 30px 30px, 30px 30px;
            color: #e2e8f0;
            overflow-x: hidden;
        }

        /* Glassmorphic styling */
        .glass-panel {
            background: rgba(13, 20, 36, 0.75);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(56, 189, 248, 0.15);
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
        }

        .glass-panel-glow {
            box-shadow: 0 0 20px rgba(6, 182, 212, 0.15);
        }

        /* Custom Scrollbars */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: rgba(10, 15, 29, 0.6);
        }
        ::-webkit-scrollbar-thumb {
            background: rgba(6, 182, 212, 0.3);
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: rgba(6, 182, 212, 0.6);
        }

        /* Glowing text & elements */
        .text-glow-cyan {
            text-shadow: 0 0 10px rgba(34, 211, 238, 0.5);
        }
        .text-glow-mint {
            text-shadow: 0 0 10px rgba(52, 211, 153, 0.5);
        }
        
        /* Pulse Animation */
        @keyframes radar-sweep {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        .radar-sweep {
            animation: radar-sweep 4s linear infinite;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col">

    <!-- Top Navigation Bar -->
    <header class="glass-panel border-b border-cyber-border sticky top-0 z-50 px-6 py-3">
        <div class="flex items-center justify-between">
            <div class="flex items-center gap-3">
                <div class="relative flex items-center justify-center w-10 h-10 rounded-xl bg-gradient-to-br from-cyan-500/20 to-emerald-500/20 border border-cyber-cyan/40">
                    <i data-lucide="activity" class="w-6 h-6 text-cyber-cyan"></i>
                    <span class="absolute -top-1 -right-1 w-3 h-3 bg-cyber-mint rounded-full animate-ping"></span>
                    <span class="absolute -top-1 -right-1 w-3 h-3 bg-cyber-mint rounded-full"></span>
                </div>
                <div>
                    <div class="flex items-center gap-2">
                        <h1 class="font-bold text-lg tracking-wider text-white">NETPULSE<span class="text-cyber-cyan">//</span>ANALYZER</h1>
                        <span class="px-2 py-0.5 text-[10px] font-mono bg-cyber-cyan/10 border border-cyber-cyan/30 text-cyber-cyan rounded-full">v2.4 LOCAL</span>
                    </div>
                    <p class="text-xs text-slate-400 font-mono">INTERFACE: eth0 • SUBNET: 192.168.1.0/24</p>
                </div>
            </div>

            <!-- Global Status Bar -->
            <div class="hidden md:flex items-center gap-6 text-xs font-mono">
                <div class="flex items-center gap-2 bg-slate-900/60 px-3 py-1.5 rounded-lg border border-slate-800">
                    <i data-lucide="wifi" class="w-4 h-4 text-cyber-mint"></i>
                    <span class="text-slate-400">STATUS:</span>
                    <span class="text-cyber-mint font-bold" id="status-indicator">ACTIVE SNIFFING</span>
                </div>
                <div class="flex items-center gap-2 bg-slate-900/60 px-3 py-1.5 rounded-lg border border-slate-800">
                    <i data-lucide="cpu" class="w-4 h-4 text-cyber-cyan"></i>
                    <span class="text-slate-400">PACKETS/SEC:</span>
                    <span class="text-cyber-cyan font-bold" id="pps-counter">142 pps</span>
                </div>
                <div class="flex items-center gap-2 bg-slate-900/60 px-3 py-1.5 rounded-lg border border-slate-800">
                    <i data-lucide="hard-drive-download" class="w-4 h-4 text-emerald-400"></i>
                    <span class="text-slate-400">BANDWIDTH:</span>
                    <span class="text-emerald-400 font-bold" id="bandwidth-counter">4.21 Mbps</span>
                </div>
            </div>

            <!-- Quick Action Controls -->
            <div class="flex items-center gap-3">
                <button id="btn-scan" onclick="triggerDiscovery()" class="flex items-center gap-2 px-4 py-2 text-xs font-semibold rounded-lg bg-cyan-500/10 hover:bg-cyan-500/20 text-cyber-cyan border border-cyber-cyan/40 transition-all duration-200 active:scale-95">
                    <i data-lucide="radar" class="w-4 h-4"></i>
                    <span>SCAN SUBNET</span>
                </button>
                <button id="btn-toggle-capture" onclick="toggleCapture()" class="flex items-center gap-2 px-4 py-2 text-xs font-semibold rounded-lg bg-emerald-500/10 hover:bg-emerald-500/20 text-cyber-mint border border-cyber-mint/40 transition-all duration-200 active:scale-95">
                    <i data-lucide="pause-circle" id="icon-toggle" class="w-4 h-4"></i>
                    <span id="label-toggle">PAUSE CAPTURE</span>
                </button>
            </div>
        </div>
    </header>

    <!-- Main Dashboard Body -->
    <main class="flex-1 p-4 md:p-6 grid grid-cols-1 lg:grid-cols-12 gap-6">

        <!-- LEFT COLUMN: Topology Canvas & Controls (7 Cols) -->
        <section class="lg:col-span-7 flex flex-col gap-6">
            
            <!-- Network Topology Map Container -->
            <div class="glass-panel rounded-2xl p-4 flex flex-col h-[520px] relative overflow-hidden group">
                <!-- Header -->
                <div class="flex items-center justify-between mb-3 z-10">
                    <div class="flex items-center gap-2">
                        <i data-lucide="network" class="w-5 h-5 text-cyber-cyan"></i>
                        <h2 class="font-semibold text-white tracking-wide">Network Topology Map</h2>
                        <span class="text-xs text-slate-400 font-mono" id="node-count-badge">(9 Hosts Discovered)</span>
                    </div>
                    <div class="flex items-center gap-2">
                        <button onclick="resetCanvasView()" class="p-1.5 text-xs bg-slate-800/80 hover:bg-slate-700 text-slate-300 rounded-lg border border-slate-700" title="Recenter View">
                            <i data-lucide="locate-fixed" class="w-4 h-4"></i>
                        </button>
                        <button onclick="toggleLayoutMode()" class="px-2.5 py-1 text-xs font-mono bg-slate-800/80 hover:bg-slate-700 text-slate-300 rounded-lg border border-slate-700">
                            LAYOUT: <span id="layout-label" class="text-cyber-cyan">RADIAL</span>
                        </button>
                    </div>
                </div>

                <!-- Canvas Surface -->
                <div class="relative flex-1 rounded-xl overflow-hidden bg-slate-950/80 border border-slate-800/80">
                    <canvas id="topologyCanvas" class="w-full h-full cursor-grab active:cursor-grabbing"></canvas>

                    <!-- Scan overlay sweep effect -->
                    <div id="scan-overlay" class="absolute inset-0 pointer-events-none hidden flex items-center justify-center">
                        <div class="w-[600px] h-[600px] rounded-full border border-cyber-cyan/20 radar-sweep bg-gradient-to-r from-cyan-500/10 to-transparent"></div>
                        <span class="absolute text-cyber-cyan font-mono text-xs bg-slate-900/90 border border-cyber-cyan/40 px-3 py-1 rounded-full animate-pulse">ARP / ICMP DISCOVERY SWEEP IN PROGRESS...</span>
                    </div>

                    <!-- Map Overlay Legend -->
                    <div class="absolute bottom-3 left-3 flex items-center gap-4 bg-slate-900/80 backdrop-blur border border-slate-800 px-3 py-1.5 rounded-lg text-[11px] font-mono text-slate-400">
                        <div class="flex items-center gap-1.5">
                            <span class="w-2.5 h-2.5 rounded-full bg-cyber-cyan inline-block"></span> Gateway/Router
                        </div>
                        <div class="flex items-center gap-1.5">
                            <span class="w-2.5 h-2.5 rounded-full bg-cyber-mint inline-block"></span> End Host
                        </div>
                        <div class="flex items-center gap-1.5">
                            <span class="w-2.5 h-2.5 rounded-full bg-purple-400 inline-block"></span> Server
                        </div>
                        <div class="flex items-center gap-1.5">
                            <span class="w-2.5 h-2.5 rounded-full bg-amber-400 inline-block"></span> IoT / Unrecognized
                        </div>
                    </div>
                </div>
            </div>

            <!-- Device Detail Inspector Panel (Dynamic populated on node click) -->
            <div class="glass-panel rounded-2xl p-5 flex flex-col gap-4" id="device-inspector">
                <div class="flex items-center justify-between border-b border-slate-800 pb-3">
                    <div class="flex items-center gap-3">
                        <div class="p-2.5 rounded-xl bg-cyan-500/10 border border-cyber-cyan/30 text-cyber-cyan" id="inspector-icon-bg">
                            <i data-lucide="router" class="w-5 h-5" id="inspector-icon"></i>
                        </div>
                        <div>
                            <h3 class="font-bold text-white text-base" id="inspector-name">Gateway Router (192.168.1.1)</h3>
                            <p class="text-xs text-slate-400 font-mono" id="inspector-mac">MAC: 00:1A:2B:3C:4D:5E • Vendor: Cisco Systems</p>
                        </div>
                    </div>
                    <span class="px-3 py-1 text-xs font-mono rounded-full bg-emerald-500/10 border border-emerald-500/30 text-cyber-mint" id="inspector-status">ONLINE // STABLE</span>
                </div>

                <div class="grid grid-cols-2 sm:grid-cols-4 gap-4 text-xs font-mono">
                    <div class="bg-slate-900/50 p-3 rounded-xl border border-slate-800">
                        <span class="text-slate-500 block mb-1">TX PACKETS</span>
                        <span class="text-white text-sm font-bold" id="inspector-tx">48,291 pkts</span>
                    </div>
                    <div class="bg-slate-900/50 p-3 rounded-xl border border-slate-800">
                        <span class="text-slate-500 block mb-1">RX PACKETS</span>
                        <span class="text-white text-sm font-bold" id="inspector-rx">59,102 pkts</span>
                    </div>
                    <div class="bg-slate-900/50 p-3 rounded-xl border border-slate-800">
                        <span class="text-slate-500 block mb-1">OPEN PORTS</span>
                        <span class="text-cyber-cyan text-sm font-bold" id="inspector-ports">80, 443, 22, 53</span>
                    </div>
                    <div class="bg-slate-900/50 p-3 rounded-xl border border-slate-800">
                        <span class="text-slate-500 block mb-1">THREAT SCORE</span>
                        <span class="text-emerald-400 text-sm font-bold" id="inspector-threat">0.0 (LOW)</span>
                    </div>
                </div>

                <div class="flex items-center justify-between text-xs pt-1">
                    <div class="flex items-center gap-2">
                        <button onclick="pingDevice()" class="px-3 py-1.5 rounded-lg bg-slate-800 hover:bg-slate-700 border border-slate-700 text-slate-200 font-mono flex items-center gap-1.5 transition">
                            <i data-lucide="send" class="w-3.5 h-3.5 text-cyber-cyan"></i> Send ICMP Ping
                        </button>
                        <button onclick="simulateThreat()" class="px-3 py-1.5 rounded-lg bg-rose-500/10 hover:bg-rose-500/20 border border-rose-500/30 text-rose-300 font-mono flex items-center gap-1.5 transition">
                            <i data-lucide="shield-alert" class="w-3.5 h-3.5 text-rose-400"></i> Inject Anomaly
                        </button>
                    </div>
                    <span class="text-slate-500 font-mono text-[11px]" id="inspector-lastseen">Last Active: Just now</span>
                </div>
            </div>

        </section>

        <!-- RIGHT COLUMN: Analytics, Charts & Real-time Logs (5 Cols) -->
        <section class="lg:col-span-5 flex flex-col gap-6">

            <!-- Real-time Metrics & Charts Container -->
            <div class="glass-panel rounded-2xl p-5 flex flex-col gap-5">
                <div class="flex items-center justify-between">
                    <div class="flex items-center gap-2">
                        <i data-lucide="bar-chart-3" class="w-5 h-5 text-cyber-mint"></i>
                        <h2 class="font-semibold text-white tracking-wide">Traffic Telemetry</h2>
                    </div>
                    <span class="text-xs text-slate-400 font-mono">Live 1-sec sampling</span>
                </div>

                <!-- Bandwidth Chart (Line) -->
                <div>
                    <div class="flex justify-between text-xs text-slate-400 font-mono mb-1">
                        <span>BANDWIDTH OVER TIME (KB/s)</span>
                        <span class="text-cyber-cyan" id="live-throughput-label">410 KB/s</span>
                    </div>
                    <div class="h-36 w-full">
                        <canvas id="bandwidthChart"></canvas>
                    </div>
                </div>

                <!-- Protocol Breakdown (Doughnut / Bar Grid) -->
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 items-center">
                    <div class="h-36 relative">
                        <canvas id="protocolChart"></canvas>
                    </div>
                    <div class="space-y-2 font-mono text-xs">
                        <div class="flex justify-between items-center p-2 rounded bg-slate-900/60 border border-slate-800">
                            <span class="flex items-center gap-2 text-slate-300"><span class="w-2.5 h-2.5 rounded-full bg-cyan-400"></span> HTTPS (443)</span>
                            <span class="font-bold text-cyan-400" id="pct-https">45%</span>
                        </div>
                        <div class="flex justify-between items-center p-2 rounded bg-slate-900/60 border border-slate-800">
                            <span class="flex items-center gap-2 text-slate-300"><span class="w-2.5 h-2.5 rounded-full bg-emerald-400"></span> DNS (53)</span>
                            <span class="font-bold text-emerald-400" id="pct-dns">25%</span>
                        </div>
                        <div class="flex justify-between items-center p-2 rounded bg-slate-900/60 border border-slate-800">
                            <span class="flex items-center gap-2 text-slate-300"><span class="w-2.5 h-2.5 rounded-full bg-purple-400"></span> TCP / SSH</span>
                            <span class="font-bold text-purple-400" id="pct-tcp">20%</span>
                        </div>
                        <div class="flex justify-between items-center p-2 rounded bg-slate-900/60 border border-slate-800">
                            <span class="flex items-center gap-2 text-slate-300"><span class="w-2.5 h-2.5 rounded-full bg-amber-400"></span> UDP / ICMP</span>
                            <span class="font-bold text-amber-400" id="pct-other">10%</span>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Live Traffic Stream Log -->
            <div class="glass-panel rounded-2xl p-5 flex flex-col h-[340px]">
                <div class="flex items-center justify-between mb-3">
                    <div class="flex items-center gap-2">
                        <i data-lucide="list-filter" class="w-5 h-5 text-cyber-cyan"></i>
                        <h2 class="font-semibold text-white tracking-wide">Live Packet Sniffer Log</h2>
                    </div>
                    <div class="flex items-center gap-2">
                        <input type="text" id="log-filter" onkeyup="filterLogs()" placeholder="Filter IP/Proto..." class="bg-slate-900 border border-slate-800 rounded px-2.5 py-1 text-xs text-slate-200 font-mono focus:outline-none focus:border-cyber-cyan w-32">
                        <button onclick="clearLogs()" class="p-1 text-slate-400 hover:text-white rounded" title="Clear log screen">
                            <i data-lucide="trash-2" class="w-4 h-4"></i>
                        </button>
                    </div>
                </div>

                <!-- Table Header -->
                <div class="grid grid-cols-12 gap-2 font-mono text-[11px] font-bold text-slate-400 border-b border-slate-800 pb-2 px-2">
                    <span class="col-span-3">TIMESTAMP</span>
                    <span class="col-span-3">SRC IP</span>
                    <span class="col-span-3">DST IP</span>
                    <span class="col-span-2">PROTO</span>
                    <span class="col-span-1 text-right">SIZE</span>
                </div>

                <!-- Scrollable Log Output -->
                <div id="log-container" class="flex-1 overflow-y-auto font-mono text-xs divide-y divide-slate-800/40 pr-1 mt-1">
                    <!-- Dynamic Log Rows Appended Here -->
                </div>
            </div>

        </section>

    </main>

    <!-- Global Threat Alert Toast Notification (Hidden by default) -->
    <div id="alert-toast" class="fixed bottom-6 right-6 z-50 transform translate-y-24 opacity-0 transition-all duration-300 pointer-events-none">
        <div class="glass-panel border-rose-500/50 bg-slate-900/90 p-4 rounded-xl shadow-2xl flex items-center gap-4 max-w-md">
            <div class="p-3 bg-rose-500/20 text-rose-400 rounded-lg">
                <i data-lucide="shield-alert" class="w-6 h-6 animate-bounce"></i>
            </div>
            <div>
                <h4 class="font-bold text-rose-400 text-sm" id="toast-title">SECURITY ANOMALY DETECTED</h4>
                <p class="text-xs text-slate-300" id="toast-msg">High volumetric SYN-Flood pattern identified from 192.168.1.105</p>
            </div>
            <button onclick="closeToast()" class="text-slate-400 hover:text-white p-1">
                <i data-lucide="x" class="w-4 h-4"></i>
            </button>
        </div>
    </div>

    <!-- Application Script Logic -->
    <script>
        // Initialize Lucide Icons
        lucide.createIcons();

        // Data State
        let isCapturing = true;
        let isScanning = false;
        let layoutMode = 'RADIAL'; // RADIAL or GRID
        let selectedNodeId = 'router';
        let packetCountTotal = 1420;
        let totalBytes = 5420000;

        // Devices Data Structure
        const devices = [
            { id: 'router', name: 'Gateway Router', ip: '192.168.1.1', mac: '00:1A:2B:3C:4D:5E', type: 'router', ports: '80, 443, 22, 53', threat: 0.0, vendor: 'Cisco Systems', tx: 120400, rx: 184000, x: 0, y: 0 },
            { id: 'server-1', name: 'Primary NAS Server', ip: '192.168.1.10', mac: 'A4:83:E7:11:22:33', type: 'server', ports: '80, 443, 22, 8080', threat: 0.1, vendor: 'Synology Inc.', tx: 95400, rx: 142000, x: 0, y: 0 },
            { id: 'server-2', name: 'Local DNS Server', ip: '192.168.1.53', mac: 'B2:44:D1:99:88:77', type: 'server', ports: '53, 953', threat: 0.0, vendor: 'Raspberry Pi Foundation', tx: 34000, rx: 32000, x: 0, y: 0 },
            { id: 'dev-workstation', name: 'Dev-Workstation-Pro', ip: '192.168.1.101', mac: '70:85:C2:AB:CD:EF', type: 'host', ports: '22, 3000, 5173', threat: 0.0, vendor: 'Apple Inc.', tx: 45000, rx: 89000, x: 0, y: 0 },
            { id: 'laptop-wifi', name: 'User-Laptop-MacBook', ip: '192.168.1.102', mac: 'F4:D4:88:12:34:56', type: 'host', ports: 'None', threat: 0.0, vendor: 'Apple Inc.', tx: 12000, rx: 34000, x: 0, y: 0 },
            { id: 'iot-camera', name: 'IP Security Camera', ip: '192.168.1.150', mac: '00:0E:8E:77:66:55', type: 'iot', ports: '554 (RTSP)', threat: 0.4, vendor: 'Hikvision', tx: 89000, rx: 2000, x: 0, y: 0 },
            { id: 'iot-tv', name: 'Living Room Smart TV', ip: '192.168.1.160', mac: 'AC:C1:EE:44:55:66', type: 'iot', ports: '8008, 8009', threat: 0.0, vendor: 'Samsung Electronics', tx: 4000, rx: 78000, x: 0, y: 0 },
            { id: 'suspect-host', name: 'Unknown-Android-Device', ip: '192.168.1.205', mac: 'DE:AD:BE:EF:99:00', type: 'iot', ports: 'Unknown', threat: 0.8, vendor: 'Generic Device', tx: 55000, rx: 12000, x: 0, y: 0 },
            { id: 'guest-phone', name: 'Guest-iPhone-13', ip: '192.168.1.210', mac: 'BC:D1:D3:55:66:77', type: 'host', ports: 'None', threat: 0.0, vendor: 'Apple Inc.', tx: 3000, rx: 9000, x: 0, y: 0 }
        ];

        // Global Canvas Context setup
        const canvas = document.getElementById('topologyCanvas');
        const ctx = canvas.getContext('2d');
        let width = 0;
        let height = 0;
        let scale = 1;
        let offsetX = 0;
        let offsetY = 0;
        let draggedNode = null;
        let isDragging = false;
        let dragStartX = 0, dragStartY = 0;

        // Animated Traffic Pulses traveling along links
        let pulses = [];

        function resizeCanvas() {
            const parent = canvas.parentElement;
            canvas.width = parent.clientWidth;
            canvas.height = parent.clientHeight;
            width = canvas.width;
            height = canvas.height;
            calculateLayout();
        }

        window.addEventListener('resize', resizeCanvas);

        function calculateLayout() {
            const centerX = width / 2;
            const centerY = height / 2;

            if (layoutMode === 'RADIAL') {
                devices[0].x = centerX;
                devices[0].y = centerY;

                const radius = Math.min(width, height) * 0.35;
                const count = devices.length - 1;
                for (let i = 1; i < devices.length; i++) {
                    const angle = ((i - 1) / count) * Math.PI * 2;
                    devices[i].x = centerX + Math.cos(angle) * radius;
                    devices[i].y = centerY + Math.sin(angle) * radius;
                }
            } else {
                // Grid layout
                const cols = 3;
                const spacingX = width / (cols + 1);
                const spacingY = height / (Math.ceil(devices.length / cols) + 1);
                devices.forEach((dev, idx) => {
                    const r = Math.floor(idx / cols);
                    const c = idx % cols;
                    dev.x = spacingX * (c + 1);
                    dev.y = spacingY * (r + 1);
                });
            }
        }

        // Spawn interactive packet pulse visual effect
        function spawnPulse(targetId) {
            const target = devices.find(d => d.id === targetId);
            const gateway = devices[0];
            if (target && target.id !== 'router') {
                pulses.push({
                    fromX: gateway.x,
                    fromY: gateway.y,
                    toX: target.x,
                    toY: target.y,
                    progress: 0,
                    speed: 0.015 + Math.random() * 0.02,
                    color: target.threat > 0.5 ? '#f43f5e' : (Math.random() > 0.5 ? '#06b6d4' : '#10b981')
                });
            }
        }

        // Main Topology Canvas Render Loop
        function renderTopology() {
            ctx.clearRect(0, 0, width, height);
            ctx.save();
            ctx.translate(offsetX, offsetY);
            ctx.scale(scale, scale);

            const gateway = devices[0];

            // 1. Draw Links to Router
            for (let i = 1; i < devices.length; i++) {
                const dev = devices[i];
                ctx.beginPath();
                ctx.moveTo(gateway.x, gateway.y);
                ctx.lineTo(dev.x, dev.y);
                
                // Link styling based on status
                if (dev.threat > 0.5) {
                    ctx.strokeStyle = 'rgba(244, 63, 94, 0.4)';
                    ctx.setLineDash([4, 4]);
                } else {
                    ctx.strokeStyle = 'rgba(56, 189, 248, 0.2)';
                    ctx.setLineDash([]);
                }
                ctx.lineWidth = 1.5;
                ctx.stroke();
                ctx.setLineDash([]);
            }

            // 2. Render Animated Packet Traffic Pulses
            pulses.forEach((p, index) => {
                p.progress += p.speed;
                const currentX = p.fromX + (p.toX - p.fromX) * p.progress;
                const currentY = p.fromY + (p.toY - p.fromY) * p.progress;

                ctx.beginPath();
                ctx.arc(currentX, currentY, 3.5, 0, Math.PI * 2);
                ctx.fillStyle = p.color;
                ctx.shadowColor = p.color;
                ctx.shadowBlur = 8;
                ctx.fill();
                ctx.shadowBlur = 0;

                if (p.progress >= 1) {
                    pulses.splice(index, 1);
                }
            });

            // 3. Render Nodes
            devices.forEach(dev => {
                const isSelected = dev.id === selectedNodeId;

                // Selection Glow
                if (isSelected) {
                    ctx.beginPath();
                    ctx.arc(dev.x, dev.y, 26, 0, Math.PI * 2);
                    ctx.fillStyle = 'rgba(6, 182, 212, 0.15)';
                    ctx.fill();
                    ctx.strokeStyle = '#22d3ee';
                    ctx.lineWidth = 2;
                    ctx.stroke();
                }

                // Node Base Outer Ring
                ctx.beginPath();
                ctx.arc(dev.x, dev.y, 18, 0, Math.PI * 2);
                
                let mainColor = '#10b981'; // Host (mint)
                if (dev.type === 'router') mainColor = '#06b6d4'; // Router (cyan)
                if (dev.type === 'server') mainColor = '#c084fc'; // Server (purple)
                if (dev.type === 'iot') mainColor = '#fbbf24'; // IoT (amber)
                if (dev.threat > 0.5) mainColor = '#f43f5e'; // Alert (rose)

                ctx.fillStyle = 'rgba(15, 23, 42, 0.9)';
                ctx.fill();
                ctx.strokeStyle = mainColor;
                ctx.lineWidth = 2.5;
                ctx.stroke();

                // Inner Dot Indicator
                ctx.beginPath();
                ctx.arc(dev.x, dev.y, 6, 0, Math.PI * 2);
                ctx.fillStyle = mainColor;
                ctx.fill();

                // Device IP / Label
                ctx.font = '11px "Fira Code", monospace';
                ctx.fillStyle = isSelected ? '#ffffff' : '#94a3b8';
                ctx.textAlign = 'center';
                ctx.fillText(dev.ip, dev.x, dev.y + 32);

                ctx.font = '10px Inter, sans-serif';
                ctx.fillStyle = '#64748b';
                ctx.fillText(dev.name, dev.x, dev.y + 45);
            });

            ctx.restore();
            requestAnimationFrame(renderTopology);
        }

        // Canvas Interaction Handling (Drag & Click Selection)
        canvas.addEventListener('mousedown', (e) => {
            const rect = canvas.getBoundingClientRect();
            const clickX = (e.clientX - rect.left - offsetX) / scale;
            const clickY = (e.clientY - rect.top - offsetY) / scale;

            // Check if node clicked
            let clicked = null;
            devices.forEach(d => {
                const dist = Math.hypot(d.x - clickX, d.y - clickY);
                if (dist <= 22) clicked = d;
            });

            if (clicked) {
                selectedNodeId = clicked.id;
                draggedNode = clicked;
                updateInspector(clicked);
            } else {
                isDragging = true;
                dragStartX = e.clientX - offsetX;
                dragStartY = e.clientY - offsetY;
            }
        });

        canvas.addEventListener('mousemove', (e) => {
            if (draggedNode) {
                const rect = canvas.getBoundingClientRect();
                draggedNode.x = (e.clientX - rect.left - offsetX) / scale;
                draggedNode.y = (e.clientY - rect.top - offsetY) / scale;
            } else if (isDragging) {
                offsetX = e.clientX - dragStartX;
                offsetY = e.clientY - dragStartY;
            }
        });

        window.addEventListener('mouseup', () => {
            draggedNode = null;
            isDragging = false;
        });

        function resetCanvasView() {
            offsetX = 0;
            offsetY = 0;
            scale = 1;
            calculateLayout();
        }

        function toggleLayoutMode() {
            layoutMode = layoutMode === 'RADIAL' ? 'GRID' : 'RADIAL';
            document.getElementById('layout-label').innerText = layoutMode;
            calculateLayout();
        }

        // Update Details Panel
        function updateInspector(dev) {
            document.getElementById('inspector-name').innerText = `${dev.name} (${dev.ip})`;
            document.getElementById('inspector-mac').innerText = `MAC: ${dev.mac} • Vendor: ${dev.vendor}`;
            document.getElementById('inspector-ports').innerText = dev.ports;
            document.getElementById('inspector-tx').innerText = `${(dev.tx / 1000).toFixed(1)}k pkts`;
            document.getElementById('inspector-rx').innerText = `${(dev.rx / 1000).toFixed(1)}k pkts`;
            
            const threatElem = document.getElementById('inspector-threat');
            if (dev.threat > 0.5) {
                threatElem.innerText = `${dev.threat.toFixed(1)} (CRITICAL)`;
                threatElem.className = 'text-rose-400 text-sm font-bold';
            } else {
                threatElem.innerText = `${dev.threat.toFixed(1)} (LOW)`;
                threatElem.className = 'text-emerald-400 text-sm font-bold';
            }

            // Update Icon
            const iconElem = document.getElementById('inspector-icon');
            let iconName = 'laptop';
            if (dev.type === 'router') iconName = 'router';
            if (dev.type === 'server') iconName = 'server';
            if (dev.type === 'iot') iconName = 'cpu';
            iconElem.setAttribute('data-lucide', iconName);
            lucide.createIcons();
        }

        // Chart.js Telemetry Initializations
        let bandwidthChart, protocolChart;

        function initCharts() {
            // Bandwidth Line Chart
            const ctxBandwidth = document.getElementById('bandwidthChart').getContext('2d');
            bandwidthChart = new Chart(ctxBandwidth, {
                type: 'line',
                data: {
                    labels: Array(15).fill(''),
                    datasets: [{
                        label: 'KB/s',
                        data: [120, 180, 150, 220, 310, 280, 410, 390, 450, 420, 490, 410, 460, 500, 420],
                        borderColor: '#06b6d4',
                        backgroundColor: 'rgba(6, 182, 212, 0.1)',
                        borderWidth: 2,
                        fill: true,
                        tension: 0.3,
                        pointRadius: 0
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: { legend: { display: false } },
                    scales: {
                        x: { display: false },
                        y: { 
                            grid: { color: 'rgba(255, 255, 255, 0.05)' },
                            ticks: { color: '#64748b', font: { family: 'Fira Code', size: 10 } }
                        }
                    }
                }
            });

            // Protocol Doughnut Chart
            const ctxProtocol = document.getElementById('protocolChart').getContext('2d');
            protocolChart = new Chart(ctxProtocol, {
                type: 'doughnut',
                data: {
                    labels: ['HTTPS', 'DNS', 'TCP', 'UDP/ICMP'],
                    datasets: [{
                        data: [45, 25, 20, 10],
                        backgroundColor: ['#22d3ee', '#34d399', '#c084fc', '#fbbf24'],
                        borderWidth: 0
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    cutout: '70%',
                    plugins: { legend: { display: false } }
                }
            });
        }

        // Live Packet Log Generator
        const protocols = ['HTTPS', 'DNS', 'TCP', 'UDP', 'ICMP', 'SSH'];

        function generatePacketLog() {
            if (!isCapturing) return;

            const randomDev = devices[Math.floor(Math.random() * (devices.length - 1)) + 1];
            const proto = protocols[Math.floor(Math.random() * protocols.length)];
            const size = Math.floor(Math.random() * 1200) + 64;
            const now = new Date().toTimeString().split(' ')[0] + '.' + Math.floor(Math.random()*900+100);

            // Update stats
            packetCountTotal++;
            totalBytes += size;
            document.getElementById('pps-counter').innerText = `${Math.floor(Math.random() * 40 + 120)} pps`;
            document.getElementById('bandwidth-counter').innerText = `${((size * 8 * 120) / 1000000).toFixed(2)} Mbps`;

            // Visual pulse trigger
            if (Math.random() > 0.4) {
                spawnPulse(randomDev.id);
            }

            // Create DOM row
            const container = document.getElementById('log-container');
            const row = document.createElement('div');
            row.className = 'grid grid-cols-12 gap-2 py-1.5 px-2 hover:bg-slate-800/40 transition rounded log-entry';
            
            let protoColor = 'text-cyan-400';
            if (proto === 'DNS') protoColor = 'text-emerald-400';
            if (proto === 'TCP') protoColor = 'text-purple-400';
            if (proto === 'ICMP') protoColor = 'text-amber-400';

            row.innerHTML = `
                <span class="col-span-3 text-slate-500">${now}</span>
                <span class="col-span-3 text-slate-300">${randomDev.ip}</span>
                <span class="col-span-3 text-slate-300">192.168.1.1</span>
                <span class="col-span-2 font-bold ${protoColor}">${proto}</span>
                <span class="col-span-1 text-right text-slate-400">${size}B</span>
            `;

            container.insertBefore(row, container.firstChild);

            // Limit row count
            if (container.children.length > 40) {
                container.removeChild(container.lastChild);
            }

            // Live update line chart
            const newData = bandwidthChart.data.datasets[0].data;
            newData.shift();
            const currentKbps = Math.floor(Math.random() * 150 + 350);
            newData.push(currentKbps);
            bandwidthChart.update();
            document.getElementById('live-throughput-label').innerText = `${currentKbps} KB/s`;
        }

        // Search Filter for Logs
        function filterLogs() {
            const query = document.getElementById('log-filter').value.toLowerCase();
            const rows = document.querySelectorAll('.log-entry');
            rows.forEach(row => {
                const text = row.innerText.toLowerCase();
                row.style.display = text.includes(query) ? 'grid' : 'none';
            });
        }

        function clearLogs() {
            document.getElementById('log-container').innerHTML = '';
        }

        // Pause/Resume Sniffing
        function toggleCapture() {
            isCapturing = !isCapturing;
            const btnLabel = document.getElementById('label-toggle');
            const btnIcon = document.getElementById('icon-toggle');
            const statusInd = document.getElementById('status-indicator');

            if (isCapturing) {
                btnLabel.innerText = 'PAUSE CAPTURE';
                btnIcon.setAttribute('data-lucide', 'pause-circle');
                statusInd.innerText = 'ACTIVE SNIFFING';
                statusInd.className = 'text-cyber-mint font-bold';
            } else {
                btnLabel.innerText = 'RESUME CAPTURE';
                btnIcon.setAttribute('data-lucide', 'play-circle');
                statusInd.innerText = 'PAUSED';
                statusInd.className = 'text-amber-400 font-bold';
            }
            lucide.createIcons();
        }

        // Subnet Discovery Trigger Simulation
        function triggerDiscovery() {
            if (isScanning) return;
            isScanning = true;
            const overlay = document.getElementById('scan-overlay');
            overlay.classList.remove('hidden');

            setTimeout(() => {
                overlay.classList.add('hidden');
                isScanning = false;
                showToast("SUBNET DISCOVERY COMPLETE", "Discovered 9 active hosts and updated topology map.");
            }, 4000);
        }

        // Interactive Actions inside Inspector
        function pingDevice() {
            const currentDev = devices.find(d => d.id === selectedNodeId);
            if (!currentDev) return;
            showToast(`ICMP ECHO SENT -> ${currentDev.ip}`, `RTT: 1.4ms • TTL: 64 • Packets: 4/4 Received`);
            spawnPulse(currentDev.id);
        }

        function simulateThreat() {
            const currentDev = devices.find(d => d.id === selectedNodeId);
            if (!currentDev) return;
            
            currentDev.threat = 0.9;
            updateInspector(currentDev);
            showToast("SECURITY ANOMALY INJECTED", `Unusual high frequency traffic detected from ${currentDev.name} (${currentDev.ip})`);
        }

        // Custom Toast Notification System
        function showToast(title, message) {
            const toast = document.getElementById('alert-toast');
            document.getElementById('toast-title').innerText = title;
            document.getElementById('toast-msg').innerText = message;
            
            toast.classList.remove('translate-y-24', 'opacity-0', 'pointer-events-none');
            
            setTimeout(() => {
                closeToast();
            }, 5000);
        }

        function closeToast() {
            const toast = document.getElementById('alert-toast');
            toast.classList.add('translate-y-24', 'opacity-0', 'pointer-events-none');
        }

        // Initialization
        window.onload = function() {
            resizeCanvas();
            initCharts();
            updateInspector(devices[0]);
            
            // Start Canvas Animation Loop
            requestAnimationFrame(renderTopology);

            // Packet Sniffer Interval Simulation
            setInterval(generatePacketLog, 600);
        };
    </script>
</body>
</html>
