
<html lang="en" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Einstein-Rosen Bridge & Quantum Vacuum Wormhole Simulator</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        cyber: {
                            cyan: '#00f0ff',
                            purple: '#a855f7',
                            emerald: '#10b981',
                            amber: '#ffab00',
                            crimson: '#ff1744',
                            dark: '#030712',
                            panel: '#0b0f19'
                        }
                    }
                }
            }
        }
    </script>
    <!-- Google Fonts: Orbitron & Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Orbitron:wght@400;600;800;900&display=swap" rel="stylesheet">
    <!-- FontAwesome for Sci-Fi Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #030712;
            color: #f3f4f6;
            overflow-x: hidden;
        }
        .font-orbitron {
            font-family: 'Orbitron', sans-serif;
        }
        .glass-panel {
            background: rgba(11, 15, 25, 0.9);
            backdrop-filter: blur(14px);
            border: 1px solid rgba(0, 240, 255, 0.15);
            box-shadow: 0 0 35px rgba(0, 0, 0, 0.7), inset 0 0 20px rgba(0, 240, 255, 0.03);
        }
        .glow-cyan {
            text-shadow: 0 0 12px rgba(0, 240, 255, 0.7);
        }
        .glow-purple {
            text-shadow: 0 0 12px rgba(168, 85, 247, 0.7);
        }
        .glow-amber {
            text-shadow: 0 0 12px rgba(255, 171, 0, 0.7);
        }
        .glow-crimson {
            text-shadow: 0 0 12px rgba(255, 23, 68, 0.7);
        }
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 5px;
        }
        ::-webkit-scrollbar-track {
            background: #030712;
        }
        ::-webkit-scrollbar-thumb {
            background: #1f2937;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #374151;
        }
        .scanlines {
            background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.25) 50%), linear-gradient(90deg, rgba(255, 0, 0, 0.03), rgba(0, 255, 0, 0.01), rgba(0, 0, 255, 0.03));
            background-size: 100% 3px, 3px 100%;
            pointer-events: none;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col relative">

    <!-- CRT Scanlines Overlay -->
    <div class="absolute inset-0 scanlines z-30 pointer-events-none"></div>

    <!-- Top Mission Control Header -->
    <header class="glass-panel border-b border-cyan-500/20 px-6 py-3.5 flex flex-wrap justify-between items-center z-20">
        <div class="flex items-center space-x-3">
            <div class="w-10 h-10 rounded-xl bg-cyan-500/10 border border-cyan-400/40 flex items-center justify-center text-cyan-400">
                <i class="fa-solid fa-atom text-xl animate-spin"></i>
            </div>
            <div>
                <h1 class="font-orbitron font-extrabold text-base md:text-lg tracking-wider text-white glow-cyan">EINSTEIN-ROSEN WORMHOLE SIMULATOR</h1>
                <p class="text-[11px] text-cyan-400/70 tracking-widest uppercase font-orbitron">Quantum Vacuum Extraction • Dual-Ship Singularity • SAMS Defense</p>
            </div>
        </div>
        
        <div class="flex items-center space-x-3 mt-3 md:mt-0">
            <!-- Mission Timer -->
            <div class="hidden sm:flex items-center space-x-2 bg-slate-900/90 border border-slate-800 px-3.5 py-1.5 rounded-xl text-xs font-orbitron">
                <span class="text-slate-400">CLOCK:</span>
                <span id="mission-timer" class="text-cyan-400 font-bold">T+ 00:00:00</span>
            </div>

            <button id="theory-btn" class="px-3.5 py-2 rounded-xl bg-slate-800/80 hover:bg-slate-700 border border-cyan-500/30 text-cyan-300 text-xs font-orbitron transition flex items-center space-x-2">
                <i class="fa-solid fa-book-atlas"></i>
                <span>Theory Codex</span>
            </button>

            <div class="flex items-center space-x-2 bg-slate-900/90 border border-slate-800 px-3 py-1.5 rounded-xl">
                <span id="status-dot" class="w-2.5 h-2.5 rounded-full bg-cyan-400 animate-ping"></span>
                <span id="status-text" class="text-xs font-orbitron text-cyan-400 font-bold">MODE 1: SINGULARITY</span>
            </div>
        </div>
    </header>

    <!-- Main Workspace Grid -->
    <div class="flex-1 grid grid-cols-1 lg:grid-cols-12 gap-4 p-4 max-w-[1920px] w-full mx-auto z-10">

        <!-- Left / Center: Interactive Canvas Simulation Panel (Cols 1-8) -->
        <div class="lg:col-span-8 glass-panel rounded-2xl p-4 flex flex-col relative overflow-hidden min-h-[560px] lg:min-h-[740px]">
            <!-- Canvas Header & Mode Toolbar -->
            <div class="flex flex-wrap justify-between items-center mb-3 gap-2">
                <div class="flex items-center space-x-2">
                    <i class="fa-solid fa-vr-cardboard text-cyan-400"></i>
                    <h2 class="font-orbitron font-semibold text-xs tracking-wider text-cyan-300">SPACETIME METRIC & VISUALIZATION CANVAS</h2>
                </div>

                <!-- Simulation Mode Tabs -->
                <div class="flex items-center space-x-1.5 bg-slate-950/80 p-1 rounded-xl border border-slate-800">
                    <button id="mode-1-btn" class="px-3 py-1.5 rounded-lg bg-cyan-500 text-slate-950 text-xs font-orbitron font-bold transition">1. Singularity Gen</button>
                    <button id="mode-2-btn" class="px-3 py-1.5 rounded-lg bg-slate-900 text-slate-400 hover:text-white text-xs font-orbitron transition">2. Traversal Exp</button>
                    <button id="mode-3-btn" class="px-3 py-1.5 rounded-lg bg-slate-900 text-slate-400 hover:text-white text-xs font-orbitron transition">3. Temporal GPS</button>
                </div>
            </div>

            <!-- Canvas Container -->
            <div class="flex-1 relative rounded-xl overflow-hidden border border-cyan-500/10 bg-black/80 flex items-center justify-center">
                <canvas id="simCanvas" class="w-full h-full block cursor-crosshair"></canvas>
                
                <!-- Dynamic Mode Overlay Badge -->
                <div id="mode-badge" class="absolute top-3 left-3 pointer-events-none bg-slate-950/85 backdrop-blur-md px-3.5 py-2 rounded-xl border border-cyan-500/30 text-xs font-orbitron text-cyan-300 flex items-center space-x-2 shadow-lg">
                    <i class="fa-solid fa-circle-nodes text-cyan-400 animate-pulse"></i>
                    <div>
                        <div id="badge-title" class="font-bold tracking-wider">MODE 1: DUAL-SHIP SINGULARITY GENERATOR</div>
                        <div id="badge-sub" class="text-[10px] text-slate-400">Ship A (Collapse) & Ship B (Casimir Expansion) online</div>
                    </div>
                </div>

                <!-- Traversal Controls Bar (Visible in Mode 2) -->
                <div id="traversal-toolbar" class="absolute bottom-3 left-3 right-3 bg-slate-950/90 backdrop-blur-md p-3 rounded-xl border border-purple-500/30 hidden flex items-center justify-between">
                    <div class="text-xs font-orbitron text-purple-300">
                        <span id="trav-step-title" class="font-bold">Stage: Approach</span> — <span id="trav-step-desc" class="text-slate-400 text-[11px]">Observing gravitational lensing & cosmic rainbow ring.</span>
                    </div>
                    <button id="start-traversal-btn" class="px-4 py-2 bg-purple-600 hover:bg-purple-500 text-white font-orbitron text-xs font-bold rounded-lg transition shadow-lg shadow-purple-950">
                        <i class="fa-solid fa-play mr-1"></i> Initiate Passage Run
                    </button>
                </div>

                <!-- Canvas Control Overlays -->
                <div class="absolute bottom-3 right-3 flex items-center space-x-2">
                    <button id="pause-sim" class="px-3 py-1.5 bg-slate-900/90 hover:bg-slate-800 border border-slate-700 text-slate-300 rounded-lg text-xs font-orbitron transition">
                        <i class="fa-solid fa-pause mr-1"></i> Pause
                    </button>
                    <button id="reset-sim" class="px-3 py-1.5 bg-slate-900/90 hover:bg-slate-800 border border-slate-700 text-slate-300 rounded-lg text-xs font-orbitron transition">
                        <i class="fa-solid fa-rotate-right mr-1"></i> Reset
                    </button>
                </div>
            </div>
        </div>

        <!-- Right: Telemetry, Parameter Sliders & Paradox Alarms (Cols 9-12) -->
        <div class="lg:col-span-4 flex flex-col space-y-4">
            
            <!-- Telemetry Metrics Card -->
            <div class="glass-panel rounded-2xl p-4 space-y-3.5">
                <h3 class="font-orbitron font-semibold text-xs tracking-wider text-cyan-400 flex items-center space-x-2">
                    <i class="fa-solid fa-chart-line"></i>
                    <span>QUANTUM & RELATIVISTIC METRICS</span>
                </h3>

                <div class="grid grid-cols-2 gap-2.5">
                    <div class="bg-slate-950/70 p-2.5 rounded-xl border border-cyan-500/10">
                        <div class="text-[10px] text-slate-400 font-orbitron">VACUUM DENSITY</div>
                        <div id="val-vacuum-density" class="text-base font-orbitron font-bold text-cyan-300 glow-cyan">4.82e22 J/m³</div>
                    </div>
                    <div class="bg-slate-950/70 p-2.5 rounded-xl border border-cyan-500/10">
                        <div class="text-[10px] text-slate-400 font-orbitron">UNRUH TEMP</div>
                        <div id="val-unruh-temp" class="text-base font-orbitron font-bold text-purple-400 glow-purple">1.45e12 K</div>
                    </div>
                    <div class="bg-slate-950/70 p-2.5 rounded-xl border border-cyan-500/10">
                        <div class="text-[10px] text-slate-400 font-orbitron">THROAT STABILITY</div>
                        <div id="val-throat-stability" class="text-base font-orbitron font-bold text-emerald-400">92.4%</div>
                    </div>
                    <div class="bg-slate-950/70 p-2.5 rounded-xl border border-cyan-500/10">
                        <div class="text-[10px] text-slate-400 font-orbitron">TIME OFFSET</div>
                        <div id="val-time-offset" class="text-base font-orbitron font-bold text-amber-400 glow-amber">+0.00 yrs</div>
                    </div>
                </div>

                <!-- Paradox Warning Banner -->
                <div id="paradox-banner" class="bg-rose-950/40 border border-rose-500/40 p-3 rounded-xl flex items-center space-x-3 transition-all duration-300">
                    <div class="text-rose-400 text-lg animate-bounce">
                        <i class="fa-solid fa-triangle-exclamation"></i>
                    </div>
                    <div>
                        <div class="font-orbitron font-bold text-xs text-rose-300">CTC PARADOX STATUS</div>
                        <div id="paradox-status-text" class="text-[11px] text-rose-400/80">Closed Timelike Curve safe. Linear forward causality maintained.</div>
                    </div>
                </div>
            </div>

            <!-- Interactive Parameter Sliders Card -->
            <div class="glass-panel rounded-2xl p-4 space-y-3">
                <h3 class="font-orbitron font-semibold text-xs tracking-wider text-cyan-400 flex items-center space-x-2">
                    <i class="fa-solid fa-sliders"></i>
                    <span>ENGINEERING CONTROLS</span>
                </h3>

                <!-- Relativistic Velocity Slider -->
                <div class="space-y-1">
                    <div class="flex justify-between text-xs font-orbitron">
                        <span class="text-slate-300">Ship Velocity (v/c)</span>
                        <span id="lbl-velocity" class="text-cyan-400 font-bold">0.92 c</span>
                    </div>
                    <input id="range-velocity" type="range" min="0.50" max="0.99" step="0.01" value="0.92" 
                           class="w-full h-2 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-cyan-400">
                </div>

                <!-- Quantum Vacuum Energy Extraction Slider -->
                <div class="space-y-1">
                    <div class="flex justify-between text-xs font-orbitron">
                        <span class="text-slate-300">Vacuum Excitation Rate</span>
                        <span id="lbl-vacuum" class="text-purple-400 font-bold">85.0%</span>
                    </div>
                    <input id="range-vacuum" type="range" min="10.0" max="100.0" step="0.5" value="85.0" 
                           class="w-full h-2 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-purple-500">
                </div>

                <!-- SAMS Dampening Frequency Slider -->
                <div class="space-y-1">
                    <div class="flex justify-between text-xs font-orbitron">
                        <span class="text-slate-300">SAMS Magnetorheological Freq</span>
                        <span id="lbl-sams" class="text-emerald-400 font-bold">185.0 GHz</span>
                    </div>
                    <input id="range-sams" type="range" min="50.0" max="300.0" step="1.0" value="185.0" 
                           class="w-full h-2 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-emerald-400">
                </div>

                <!-- Time Dilation Offset Slider (for Paradox Test) -->
                <div class="space-y-1">
                    <div class="flex justify-between text-xs font-orbitron">
                        <span class="text-slate-300">Ship B Relativistic Time Offset</span>
                        <span id="lbl-dilation" class="text-amber-400 font-bold">-12.5 yrs</span>
                    </div>
                    <input id="range-dilation" type="range" min="-50.0" max="10.0" step="0.5" value="-12.5" 
                           class="w-full h-2 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-amber-400">
                </div>
            </div>

            <!-- Mission Log Terminal Card -->
            <div class="glass-panel rounded-2xl p-4 flex flex-col h-[160px]">
                <h3 class="font-orbitron font-semibold text-xs tracking-wider text-cyan-400 flex items-center space-x-2 mb-2">
                    <i class="fa-solid fa-terminal"></i>
                    <span>MISSION LOG TERMINAL</span>
                </h3>
                <div id="terminal-logs" class="flex-1 bg-slate-950/90 rounded-xl p-2.5 font-mono text-[11px] text-cyan-300 overflow-y-auto space-y-1 border border-slate-800">
                    <div>[00:00:01] System initialized. SROS laser lattice locked.</div>
                    <div>[00:00:03] Ship A (Positive Gravity) & Ship B (Casimir Negative) synchronized.</div>
                    <div>[00:00:06] Quantum vacuum stirring active via Lense-Thirring frame dragging.</div>
                </div>
            </div>

        </div>

    </div>

    <!-- Theory Codex Modal -->
    <div id="theory-modal" class="fixed inset-0 z-50 flex items-center justify-center bg-black/85 backdrop-blur-md hidden p-4">
        <div class="glass-panel rounded-2xl max-w-3xl w-full p-6 space-y-4 max-h-[85vh] overflow-y-auto border border-cyan-500/40 relative">
            <div class="flex justify-between items-center border-b border-cyan-500/20 pb-3">
                <div class="flex items-center space-x-2 text-cyan-400 font-orbitron font-bold text-lg">
                    <i class="fa-solid fa-book-atlas"></i>
                    <span>EINSTEIN-ROSEN BRIDGE & QUANTUM VACUUM CODEX</span>
                </div>
                <button id="close-theory" class="text-slate-400 hover:text-white text-xl p-1">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>
            
            <div class="space-y-4 text-sm text-slate-300 leading-relaxed">
                <div>
                    <h4 class="font-orbitron font-bold text-cyan-400 mb-1">1. Solving Einstein's Energy Problem</h4>
                    <p>General Relativity permits Einstein-Rosen bridges (wormholes), but requires infinite energy. Our circular relativistic fleet extracts massive energy from the quantum vacuum via Unruh radiation and macro-Casimir effects, agitating spacetime like a blender (Lense-Thirring frame dragging).</p>
                </div>

                <div>
                    <h4 class="font-orbitron font-bold text-purple-400 mb-1">2. Dual-Ship Singularity Mechanism</h4>
                    <p>Ship A utilizes MHD positive mass-energy to collapse and pierce spacetime at Point A. Ship B projects macroscopic negative energy (Casimir pressure) to expand Point B, preventing immediate gravitational collapse into a black hole.</p>
                </div>

                <div>
                    <h4 class="font-orbitron font-bold text-emerald-400 mb-1">3. SAMS Spaghettification Defense</h4>
                    <p>Without stabilization, tidal forces compress and tear objects passing through the throat. SAMS (Spatial Acoustic & Magnetic Stabilizers) deploy high-frequency piezoelectric actuators and magnetorheological fluids to harden the hull in milliseconds.</p>
                </div>

                <div>
                    <h4 class="font-orbitron font-bold text-amber-400 mb-1">4. Temporal Paradox & Pulsar GPS</h4>
                    <p>Because wormholes can function as time machines if mouths experience relativistic time dilation, the AutoNav AI scans cosmic Pulsar synchronization and compares entanglement clocks to detect Closed Timelike Curves (CTC) before transit.</p>
                </div>
            </div>

            <div class="pt-4 border-t border-slate-800 flex justify-end">
                <button id="close-theory-btn" class="px-5 py-2 bg-cyan-500 hover:bg-cyan-400 text-slate-950 font-orbitron font-bold rounded-lg text-xs transition">
                    Acknowledge Physics
                </button>
            </div>
        </div>
    </div>

    <script>
        window.addEventListener('DOMContentLoaded', () => {
            const canvas = document.getElementById('simCanvas');
            const ctx = canvas.getContext('2d');

            let isPaused = false;
            let time = 0;
            let missionSeconds = 0;
            let currentMode = 1; // 1: Singularity, 2: Traversal, 3: Temporal GPS

            // Interactive parameters
            let shipVelocity = 0.92;
            let vacuumExcite = 85.0;
            let samsFreq = 185.0;
            let timeDilationOffset = -12.5; // in years

            // Traversal simulation state
            let travState = 0; // 0: Idle, 1: Approach, 2: Crossing, 3: Emergence
            let travProgress = 0.0;
            let isTraversing = false;

            // UI Elements
            const mode1Btn = document.getElementById('mode-1-btn');
            const mode2Btn = document.getElementById('mode-2-btn');
            const mode3Btn = document.getElementById('mode-3-btn');
            const modeBadge = document.getElementById('mode-badge');
            const badgeTitle = document.getElementById('badge-title');
            const badgeSub = document.getElementById('badge-sub');
            const traversalToolbar = document.getElementById('traversal-toolbar');
            const startTraversalBtn = document.getElementById('start-traversal-btn');
            const travStepTitle = document.getElementById('trav-step-title');
            const travStepDesc = document.getElementById('trav-step-desc');
            
            const pauseSimBtn = document.getElementById('pause-sim');
            const resetSimBtn = document.getElementById('reset-sim');
            const missionTimerEl = document.getElementById('mission-timer');
            const terminalLogs = document.getElementById('terminal-logs');

            // Telemetry UI
            const valVacuumDensity = document.getElementById('val-vacuum-density');
            const valUnruhTemp = document.getElementById('val-unruh-temp');
            const valThroatStability = document.getElementById('val-throat-stability');
            const valTimeOffset = document.getElementById('val-time-offset');
            const paradoxBanner = document.getElementById('paradox-banner');
            const paradoxStatusText = document.getElementById('paradox-status-text');

            // Sliders
            const rangeVelocity = document.getElementById('range-velocity');
            const lblVelocity = document.getElementById('lbl-velocity');
            const rangeVacuum = document.getElementById('range-vacuum');
            const lblVacuum = document.getElementById('lbl-vacuum');
            const rangeSams = document.getElementById('range-sams');
            const lblSams = document.getElementById('lbl-sams');
            const rangeDilation = document.getElementById('range-dilation');
            const lblDilation = document.getElementById('lbl-dilation');

            // Codex modal
            const theoryBtn = document.getElementById('theory-btn');
            const theoryModal = document.getElementById('theory-modal');
            const closeTheory = document.getElementById('close-theory');
            const closeTheoryBtn = document.getElementById('close-theory-btn');

            // Starfield particles
            const stars = [];
            for (let i = 0; i < 150; i++) {
                stars.push({
                    x: Math.random() * 2000 - 1000,
                    y: Math.random() * 2000 - 1000,
                    z: Math.random() * 1000 + 10,
                    size: Math.random() * 2 + 0.5
                });
            }

            // Canvas Resize Handler
            function resizeCanvas() {
                const rect = canvas.parentElement.getBoundingClientRect();
                canvas.width = rect.width * window.devicePixelRatio;
                canvas.height = rect.height * window.devicePixelRatio;
                ctx.scale(window.devicePixelRatio, window.devicePixelRatio);
            }
            window.addEventListener('resize', resizeCanvas);
            resizeCanvas();

            // Logger utility
            function logTerminal(msg) {
                const div = document.createElement('div');
                const mins = String(Math.floor(missionSeconds / 60)).padStart(2, '0');
                const secs = String(missionSeconds % 60).padStart(2, '0');
                div.textContent = `[00:${mins}:${secs}] ${msg}`;
                terminalLogs.appendChild(div);
                terminalLogs.scrollTop = terminalLogs.scrollHeight;
            }

            function setMode(mode) {
                currentMode = mode;
                isTraversing = false;
                travState = 0;
                travProgress = 0;

                mode1Btn.className = mode === 1 ? 'px-3 py-1.5 rounded-lg bg-cyan-500 text-slate-950 text-xs font-orbitron font-bold transition' : 'px-3 py-1.5 rounded-lg bg-slate-900 text-slate-400 hover:text-white text-xs font-orbitron transition';
                mode2Btn.className = mode === 2 ? 'px-3 py-1.5 rounded-lg bg-purple-500 text-slate-950 text-xs font-orbitron font-bold transition' : 'px-3 py-1.5 rounded-lg bg-slate-900 text-slate-400 hover:text-white text-xs font-orbitron transition';
                mode3Btn.className = mode === 3 ? 'px-3 py-1.5 rounded-lg bg-amber-400 text-slate-950 text-xs font-orbitron font-bold transition' : 'px-3 py-1.5 rounded-lg bg-slate-900 text-slate-400 hover:text-white text-xs font-orbitron transition';

                if (mode === 1) {
                    traversalToolbar.classList.add('hidden');
                    document.getElementById('status-text').textContent = 'MODE 1: SINGULARITY';
                    document.getElementById('status-dot').className = 'w-2.5 h-2.5 rounded-full bg-cyan-400 animate-ping';
                    badgeTitle.textContent = 'MODE 1: DUAL-SHIP SINGULARITY GENERATOR';
                    badgeSub.textContent = 'Ship A (Collapse) & Ship B (Casimir Expansion) online';
                    logTerminal('Switched telemetry to Mode 1: Singularity Generation & Vacuum Stirring.');
                } else if (mode === 2) {
                    traversalToolbar.classList.remove('hidden');
                    document.getElementById('status-text').textContent = 'MODE 2: TRAVERSAL';
                    document.getElementById('status-dot').className = 'w-2.5 h-2.5 rounded-full bg-purple-400 animate-ping';
                    badgeTitle.textContent = 'MODE 2: WORMHOLE TRAVERSAL EXPERIENCE';
                    badgeSub.textContent = 'Gravitational lensing & SAMS spaghettification defense ready.';
                    logTerminal('Switched telemetry to Mode 2: Wormhole Traversal & SAMS Defense.');
                } else if (mode === 3) {
                    traversalToolbar.classList.add('hidden');
                    document.getElementById('status-text').textContent = 'MODE 3: TEMPORAL GPS';
                    document.getElementById('status-dot').className = 'w-2.5 h-2.5 rounded-full bg-amber-400 animate-ping';
                    badgeTitle.textContent = 'MODE 3: PULSAR GPS & PARADOX DETECTOR';
                    badgeSub.textContent = 'Neutron star synchronization & CTC loop monitoring.';
                    logTerminal('Switched telemetry to Mode 3: Temporal Paradox Detection & Pulsar GPS.');
                }
            }

            mode1Btn.addEventListener('click', () => setMode(1));
            mode2Btn.addEventListener('click', () => setMode(2));
            mode3Btn.addEventListener('click', () => setMode(3));

            pauseSimBtn.addEventListener('click', () => {
                isPaused = !isPaused;
                pauseSimBtn.innerHTML = isPaused ? '<i class="fa-solid fa-play mr-1"></i> Resume' : '<i class="fa-solid fa-pause mr-1"></i> Pause';
            });

            resetSimBtn.addEventListener('click', () => {
                time = 0;
                missionSeconds = 0;
                setMode(1);
                logTerminal('Simulation reset to baseline parameters.');
            });

            startTraversalBtn.addEventListener('click', () => {
                if (!isTraversing) {
                    isTraversing = true;
                    travState = 1;
                    travProgress = 0;
                    logTerminal('Initiating wormhole passage run. SAMS dampening engaged.');
                }
            });

            // Sliders event listeners
            rangeVelocity.addEventListener('input', (e) => {
                shipVelocity = parseFloat(e.target.value);
                lblVelocity.textContent = shipVelocity.toFixed(2) + ' c';
            });
            rangeVacuum.addEventListener('input', (e) => {
                vacuumExcite = parseFloat(e.target.value);
                lblVacuum.textContent = vacuumExcite.toFixed(1) + '%';
            });
            rangeSams.addEventListener('input', (e) => {
                samsFreq = parseFloat(e.target.value);
                lblSams.textContent = samsFreq.toFixed(1) + ' GHz';
            });
            rangeDilation.addEventListener('input', (e) => {
                timeDilationOffset = parseFloat(e.target.value);
                lblDilation.textContent = (timeDilationOffset > 0 ? '+' : '') + timeDilationOffset.toFixed(1) + ' yrs';
            });

            // Codex modal controls
            theoryBtn.addEventListener('click', () => theoryModal.classList.remove('hidden'));
            closeTheory.addEventListener('click', () => theoryModal.classList.add('hidden'));
            closeTheoryBtn.addEventListener('click', () => theoryModal.classList.add('hidden'));

            // Mission timer interval
            setInterval(() => {
                if (!isPaused) {
                    missionSeconds++;
                    const hrs = String(Math.floor(missionSeconds / 3600)).padStart(2, '0');
                    const mins = String(Math.floor((missionSeconds % 3600) / 60)).padStart(2, '0');
                    const secs = String(missionSeconds % 60).padStart(2, '0');
                    missionTimerEl.textContent = `T+ ${hrs}:${mins}:${secs}`;
                }
            }, 1000);

            function animate() {
                const width = canvas.parentElement.clientWidth;
                const height = canvas.parentElement.clientHeight;

                ctx.clearRect(0, 0, width, height);

                if (!isPaused) {
                    time += 0.03;
                }

                // Parallax Starfield
                ctx.save();
                const starSpeed = shipVelocity * 3;
                stars.forEach(star => {
                    star.z -= starSpeed;
                    if (star.z <= 0) star.z = 1000;
                    const k = 300 / star.z;
                    const px = star.x * k + width / 2;
                    const py = star.y * k + height / 2;

                    if (px >= 0 && px <= width && py >= 0 && py <= height) {
                        const pSize = Math.max(0.1, star.size * k);
                        ctx.fillStyle = `rgba(255, 255, 255, ${Math.min(1, k)})`;
                        ctx.beginPath();
                        ctx.arc(px, py, pSize, 0, Math.PI * 2);
                        ctx.fill();
                    }
                });
                ctx.restore();

                // Update Telemetry readouts based on inputs
                const calculatedDensity = (vacuumExcite * 5.67e20).toExponential(2);
                const calculatedUnruh = (shipVelocity * 1.58e12).toExponential(2);
                const stability = Math.min(99.9, Math.max(40.0, 75.0 + (samsFreq / 15) - Math.abs(timeDilationOffset)));
                
                valVacuumDensity.textContent = calculatedDensity + ' J/m³';
                valUnruhTemp.textContent = calculatedUnruh + ' K';
                valThroatStability.textContent = stability.toFixed(1) + '%';
                valTimeOffset.textContent = (timeDilationOffset > 0 ? '+' : '') + timeDilationOffset.toFixed(1) + ' yrs';

                // Paradox Banner update
                if (timeDilationOffset < -5.0) {
                    paradoxBanner.className = "bg-rose-950/80 border border-rose-500 p-3 rounded-xl flex items-center space-x-3 transition-all duration-300 shadow-lg shadow-rose-950";
                    paradoxStatusText.textContent = "WARNING: Closed Timelike Curve (CTC) detected! Mouth B is in the past.";
                    paradoxStatusText.className = "text-[11px] text-rose-300 font-bold";
                } else {
                    paradoxBanner.className = "bg-slate-950/80 border border-emerald-500/30 p-3 rounded-xl flex items-center space-x-3 transition-all duration-300";
                    paradoxStatusText.textContent = "Closed Timelike Curve safe. Linear forward causality maintained.";
                    paradoxStatusText.className = "text-[11px] text-emerald-400/80";
                }

                // ==========================================
                // MODE 1: Dual-Ship Singularity Generator & Vacuum Stirring
                // ==========================================
                if (currentMode === 1) {
                    ctx.save();
                    const shipAX = width * 0.3;
                    const shipAY = height * 0.5;
                    const shipBX = width * 0.7;
                    const shipBY = height * 0.5;

                    // Relativistic Circular Stirring Vortices (Lense-Thirring)
                    [ {x: shipAX, y: shipAY, color: '#ff1744', label: 'SHIP A (MHD POSITIVE COLLAPSE)'},
                      {x: shipBX, y: shipBY, color: '#00f0ff', label: 'SHIP B (CASIMIR EXPANSION)'} ].forEach((node, idx) => {
                        
                        // Circular Orbit Path
                        ctx.beginPath();
                        ctx.arc(node.x, node.y, 75, 0, Math.PI * 2);
                        ctx.strokeStyle = `${node.color}25`;
                        ctx.lineWidth = 1;
                        ctx.setLineDash([4, 4]);
                        ctx.stroke();
                        ctx.setLineDash([]);

                        // Relativistic Drones stirring the vacuum
                        for (let d = 0; d < 4; d++) {
                            const angle = time * (shipVelocity * 4) + (d * Math.PI / 2);
                            const dx = node.x + Math.cos(angle) * 75;
                            const dy = node.y + Math.sin(angle) * 75;

                            ctx.beginPath();
                            ctx.arc(dx, dy, 4.5, 0, Math.PI * 2);
                            ctx.fillStyle = node.color;
                            ctx.shadowColor = node.color;
                            ctx.shadowBlur = 12;
                            ctx.fill();
                            ctx.shadowBlur = 0;
                        }

                        // Central Ship Core
                        ctx.beginPath();
                        ctx.arc(node.x, node.y, 16, 0, Math.PI * 2);
                        ctx.fillStyle = '#030712';
                        ctx.strokeStyle = node.color;
                        ctx.lineWidth = 3;
                        ctx.shadowColor = node.color;
                        ctx.shadowBlur = 15;
                        ctx.fill();
                        ctx.stroke();
                        ctx.shadowBlur = 0;

                        ctx.fillStyle = node.color;
                        ctx.font = '10px Orbitron';
                        ctx.textAlign = 'center';
                        ctx.fillText(node.label, node.x, node.y - 28);
                    });

                    // Gravitational Lensing & Cosmic Rainbow Ring between A and B
                    const lensRadius = 50 + Math.sin(time * 2) * 4;
                    const rainbowGrad = ctx.createRadialGradient(shipAX, shipAY, 5, shipAX, shipAY, lensRadius * 1.6);
                    rainbowGrad.addColorStop(0, 'rgba(168, 85, 247, 0.85)');
                    rainbowGrad.addColorStop(0.4, 'rgba(0, 240, 255, 0.4)');
                    rainbowGrad.addColorStop(0.7, 'rgba(255, 171, 0, 0.3)');
                    rainbowGrad.addColorStop(1, 'transparent');

                    ctx.fillStyle = rainbowGrad;
                    ctx.beginPath();
                    ctx.arc(shipAX, shipAY, lensRadius * 1.6, 0, Math.PI * 2);
                    ctx.fill();

                    // Tunnel Throat Spacetime Metric Bridge
                    ctx.beginPath();
                    ctx.moveTo(shipAX, shipAY - lensRadius);
                    ctx.bezierCurveTo(width * 0.4, shipAY - lensRadius * 2, width * 0.6, shipBY - lensRadius * 2, shipBX, shipBY - lensRadius);
                    ctx.lineTo(shipBX, shipBY + lensRadius);
                    ctx.bezierCurveTo(width * 0.6, shipBY + lensRadius * 2, width * 0.4, shipAY + lensRadius * 2, shipAX, shipAY + lensRadius);
                    ctx.closePath();
                    ctx.fillStyle = 'rgba(0, 240, 255, 0.08)';
                    ctx.strokeStyle = 'rgba(0, 240, 255, 0.5)';
                    ctx.lineWidth = 2;
                    ctx.fill();
                    ctx.stroke();

                    ctx.restore();
                }

                // ==========================================
                // MODE 2: Wormhole Traversal Experience & SAMS Defense
                // ==========================================
                else if (currentMode === 2) {
                    ctx.save();
                    const centerX = width * 0.5;
                    const centerY = height * 0.5;

                    if (isTraversing) {
                        travProgress += 0.008 * shipVelocity;
                        if (travProgress >= 1.0) {
                            travProgress = 1.0;
                            isTraversing = false;
                            travState = 3; // Emergence
                            travStepTitle.textContent = "Stage: After (Emergence)";
                            travStepDesc.textContent = "Successfully jumped to destination mouth B. Space closes smoothly.";
                            logTerminal('Traversal complete. Emerged at destination coordinates.');
                        } else if (travProgress > 0.5 && travState < 2) {
                            travState = 2; // Crossing
                            travStepTitle.textContent = "Stage: During (Crossing)";
                            travStepDesc.textContent = `SAMS frequency active (${samsFreq} GHz). Magnetorheological fluid hardening hull against spaghettification.`;
                            logTerminal('Entering wormhole throat. SAMS dampening tidal forces.');
                        }
                    }

                    // Draw Wormhole Mouth (Spherical Gravitational Lensing)
                    const mouthRadius = 90;
                    ctx.beginPath();
                    ctx.arc(centerX, centerY, mouthRadius, 0, Math.PI * 2);
                    const mouthGrad = ctx.createRadialGradient(centerX, centerY, 10, centerX, centerY, mouthRadius);
                    mouthGrad.addColorStop(0, 'rgba(0, 240, 255, 0.9)');
                    mouthGrad.addColorStop(0.5, 'rgba(168, 85, 247, 0.5)');
                    mouthGrad.addColorStop(1, 'rgba(255, 171, 0, 0.8)');
                    ctx.fillStyle = mouthGrad;
                    ctx.fill();
                    ctx.strokeStyle = '#00f0ff';
                    ctx.lineWidth = 3;
                    ctx.stroke();

                    // Traversing Ship Rendering
                    let shipDrawX = centerX - 250 + (travProgress * 500);
                    let shipDrawY = centerY;
                    let hullGlow = '#00f0ff';

                    if (travState === 2) {
                        // Spaghettification compression defense visual
                        shipDrawX = centerX + (Math.random() - 0.5) * 4;
                        shipDrawY = centerY + (Math.random() - 0.5) * 4;
                        hullGlow = '#10b981'; // SAMS emerald active armor
                    }

                    ctx.save();
                    ctx.translate(shipDrawX, shipDrawY);
                    
                    // Ship Hull
                    ctx.beginPath();
                    ctx.moveTo(15, 0);
                    ctx.lineTo(-12, -7);
                    ctx.lineTo(-6, 0);
                    ctx.lineTo(-12, 7);
                    ctx.closePath();
                    ctx.fillStyle = '#ffffff';
                    ctx.shadowColor = hullGlow;
                    ctx.shadowBlur = 20;
                    ctx.fill();
                    ctx.shadowBlur = 0;

                    ctx.fillStyle = hullGlow;
                    ctx.font = '10px Orbitron';
                    ctx.textAlign = 'center';
                    ctx.fillText(travState === 2 ? 'SAMS ARMORED HULL' : 'EXPLORER VESSEL', 0, -22);
                    ctx.restore();

                    // SAMS piezoelectric damping waves around throat
                    if (travState === 2) {
                        for (let i = 0; i < 4; i++) {
                            ctx.beginPath();
                            ctx.arc(centerX, centerY, mouthRadius + 20 + (i * 15) + (Math.sin(time * 10 + i) * 8), 0, Math.PI * 2);
                            ctx.strokeStyle = 'rgba(16, 185, 129, 0.4)';
                            ctx.lineWidth = 1.5;
                            ctx.stroke();
                        }
                    }

                    ctx.restore();
                }

                // ==========================================
                // MODE 3: Temporal Paradox & Cosmic GPS (Pulsar Scanner)
                // ==========================================
                else if (currentMode === 3) {
                    ctx.save();
                    const centerX = width * 0.5;
                    const centerY = height * 0.5;

                    // Pulsar Cosmic GPS Scanner HUD
                    ctx.beginPath();
                    ctx.arc(centerX, centerY, 130, 0, Math.PI * 2);
                    ctx.strokeStyle = 'rgba(255, 171, 0, 0.3)';
                    ctx.lineWidth = 1.5;
                    ctx.setLineDash([6, 6]);
                    ctx.stroke();
                    ctx.setLineDash([]);

                    ctx.beginPath();
                    ctx.arc(centerX, centerY, 80, 0, Math.PI * 2);
                    ctx.strokeStyle = 'rgba(255, 171, 0, 0.5)';
                    ctx.lineWidth = 1;
                    ctx.stroke();

                    // Rotating Pulsar Beam Radar
                    const radarAngle = time * 2;
                    ctx.beginPath();
                    ctx.moveTo(centerX, centerY);
                    ctx.lineTo(centerX + Math.cos(radarAngle) * 130, centerY + Math.sin(radarAngle) * 130);
                    ctx.strokeStyle = 'rgba(255, 171, 0, 0.8)';
                    ctx.lineWidth = 2;
                    ctx.stroke();

                    // Neutron Star Pulsar Nodes around radar
                    for (let p = 0; p < 3; p++) {
                        const pAngle = (p * Math.PI * 2 / 3) + (time * 0.5);
                        const px = centerX + Math.cos(pAngle) * 110;
                        const py = centerY + Math.sin(pAngle) * 110;

                        ctx.beginPath();
                        ctx.arc(px, py, 6, 0, Math.PI * 2);
                        ctx.fillStyle = '#ffab00';
                        ctx.shadowColor = '#ffab00';
                        ctx.shadowBlur = 12;
                        ctx.fill();
                        ctx.shadowBlur = 0;

                        ctx.fillStyle = '#ffab00';
                        ctx.font = '9px Orbitron';
                        ctx.textAlign = 'center';
                        ctx.fillText(`PULSAR #0${p+1}`, px, py - 14);
                    }

                    // Quantum Entanglement Clock Status Overlay
                    ctx.fillStyle = 'rgba(11, 15, 25, 0.85)';
                    ctx.fillRect(centerX - 160, centerY - 170, 320, 50);
                    ctx.strokeStyle = 'rgba(255, 171, 0, 0.4)';
                    ctx.lineWidth = 1;
                    ctx.strokeRect(centerX - 160, centerY - 170, 320, 50);

                    ctx.fillStyle = '#ffab00';
                    ctx.font = '10px Orbitron';
                    ctx.textAlign = 'center';
                    ctx.fillText('QUANTUM ENTANGLEMENT CHRONOMETER', centerX, centerY - 150);
                    ctx.fillStyle = timeDilationOffset < -5 ? '#ff1744' : '#10b981';
                    ctx.font = '12px Orbitron';
                    ctx.fillText(`TIME DILATION DELTA: ${timeDilationOffset.toFixed(1)} YEARS`, centerX, centerY - 132);

                    ctx.restore();
                }

                requestAnimationFrame(animate);
            }

            requestAnimationFrame(animate);
        });
    </script>
</body>
</html>
