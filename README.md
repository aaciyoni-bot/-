<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>דוח התחשבנות מועדון - כניסה מאובטחת</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    
    <!-- Chosen Palette: Professional Slate & Blue (Login focused) -->
    
    <!-- Application Structure Plan: 
         1. Login Screen: A secure entry point that validates credentials. 
         2. Conditional Navigation: Admin sees all tabs; Agents see only their personal report tab.
         3. Filtered Data: If an agent logs in, the JS automatically filters the 'reportData' to include only their specific index, hiding the selector and dashboard entirely.
         4. Session Management: Simple 'currentUser' state in memory to handle view logic. -->

    <style>
        body { font-family: system-ui, -apple-system, sans-serif; background-color: #f8fafc; color: #1e293b; }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 800px;
            margin-left: auto;
            margin-right: auto;
            height: 350px;
            max-height: 400px;
        }
        .nav-btn.active { background-color: #2563eb; color: white; border-color: #2563eb; }
        .nav-btn { transition: all 0.2s ease; }
        #login-overlay { background: linear-gradient(135deg, #1e293b 0%, #334155 100%); }
    </style>
</head>
<body class="min-h-screen flex flex-col">

    <!-- Login Screen -->
    <div id="login-screen" class="fixed inset-0 z-50 flex items-center justify-center p-4" style="background-color: #1e293b;">
        <div class="bg-white p-8 rounded-2xl shadow-2xl w-full max-w-md border border-slate-200">
            <div class="text-center mb-8">
                <div class="inline-flex items-center justify-center w-16 h-16 bg-blue-100 text-blue-600 rounded-full mb-4">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z" />
                    </svg>
                </div>
                <h2 class="text-2xl font-bold text-slate-800">כניסה למערכת דוחות</h2>
                <p class="text-slate-500 mt-2">אנא הכנס שם משתמש וסיסמה כדי לצפות בנתונים</p>
            </div>
            <div class="space-y-4">
                <div>
                    <label class="block text-sm font-medium text-slate-700 mb-1">שם משתמש</label>
                    <input type="text" id="username" class="w-full p-3 border border-slate-300 rounded-lg focus:ring-2 focus:ring-blue-500 outline-none" placeholder="הכנס שם">
                </div>
                <div>
                    <label class="block text-sm font-medium text-slate-700 mb-1">סיסמה</label>
                    <input type="password" id="password" class="w-full p-3 border border-slate-300 rounded-lg focus:ring-2 focus:ring-blue-500 outline-none" placeholder="••••••••">
                </div>
                <div id="login-error" class="text-red-500 text-sm hidden text-center font-medium">שם משתמש או סיסמה שגויים</div>
                <button id="login-btn" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 rounded-lg transition-colors mt-4">כניסה</button>
            </div>
        </div>
    </div>

    <!-- Application Content (Hidden until login) -->
    <div id="app-content" class="hidden flex flex-col min-h-screen">
        <header class="bg-white shadow-sm border-b border-slate-200 py-4">
            <div class="container mx-auto px-4 lg:px-8 max-w-7xl">
                <div class="flex flex-col md:flex-row justify-between items-center gap-4">
                    <div class="flex items-center gap-3">
                        <h1 class="text-2xl font-bold text-slate-800 tracking-tight">💼 דוח התחשבנות</h1>
                        <span id="user-badge" class="bg-slate-100 text-slate-600 px-3 py-1 rounded-full text-xs font-bold border border-slate-200 uppercase">מנהל</span>
                    </div>
                    <nav class="flex gap-2 items-center">
                        <div id="main-nav" class="flex gap-2 mr-4">
                            <button id="nav-dashboard" class="nav-btn active px-4 py-2 rounded-lg border border-slate-200 text-slate-700 font-medium whitespace-nowrap">📊 דאשבורד</button>
                            <button id="nav-agents" class="nav-btn px-4 py-2 rounded-lg border border-slate-200 text-slate-700 font-medium whitespace-nowrap">👥 דוחות</button>
                            <button id="nav-mtt" class="nav-btn px-4 py-2 rounded-lg border border-slate-200 text-slate-700 font-medium whitespace-nowrap">🏆 טורנירים</button>
                        </div>
                        <button id="logout-btn" class="text-slate-400 hover:text-red-600 p-2 transition-colors" title="התנתק">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1" />
                            </svg>
                        </button>
                    </nav>
                </div>
            </div>
        </header>

        <main class="flex-grow container mx-auto px-4 lg:px-8 max-w-7xl py-8">
            <!-- Same View Blocks as before -->
            <div id="view-dashboard" class="view-section block">
                <div class="mb-8">
                    <h2 class="text-2xl font-bold text-slate-800 mb-2">תמונת מצב מועדון</h2>
                    <p class="text-slate-600">סיכום התחשבנות גלובלי של כלל הסוכנים הפעילים.</p>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8" id="dash-stats"></div>
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                    <div class="bg-white rounded-xl shadow-sm border border-slate-200 p-6">
                        <h3 class="text-lg font-bold text-slate-800 mb-4">מאזן סופי לפי סוכן</h3>
                        <div class="chart-container"><canvas id="settlementChart"></canvas></div>
                    </div>
                    <div class="bg-white rounded-xl shadow-sm border border-slate-200 p-6">
                        <h3 class="text-lg font-bold text-slate-800 mb-4">פילוח עמלות סוכנים</h3>
                        <div class="chart-container"><canvas id="feeDistributionChart"></canvas></div>
                    </div>
                </div>
            </div>

            <div id="view-agents" class="view-section hidden">
                <div id="agent-selector-container" class="bg-white rounded-xl shadow-sm border border-slate-200 p-6 mb-8">
                    <label class="block text-sm font-semibold text-slate-700 mb-2">צפה בדוח של:</label>
                    <select id="agent-selector" class="w-full md:w-1/3 bg-slate-50 border border-slate-300 text-slate-900 text-lg rounded-lg p-3 outline-none"></select>
                </div>

                <div id="agent-details-container">
                    <div class="grid grid-cols-1 md:grid-cols-4 gap-4 mb-8" id="agent-metrics"></div>
                    <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                        <div class="lg:col-span-2 bg-white rounded-xl shadow-sm border border-slate-200 p-6">
                            <h3 class="text-lg font-bold text-slate-800 mb-4 border-b border-slate-100 pb-2">פירוט שחקנים</h3>
                            <div class="overflow-x-auto"><table class="w-full text-right"><thead class="bg-slate-50 text-slate-500 text-sm"><tr><th class="p-4 border-b">שחקן</th><th class="p-4 border-b">P&L</th><th class="p-4 border-b">עמלה</th></tr></thead><tbody id="agent-players-table" class="divide-y"></tbody></table></div>
                        </div>
                        <div class="bg-white rounded-xl shadow-sm border border-slate-200 p-6">
                            <h3 class="text-lg font-bold text-slate-800 mb-4 border-b border-slate-100 pb-2">ביצועי שיא</h3>
                            <div class="chart-container" style="height: 300px;"><canvas id="agentPlayersChart"></canvas></div>
                        </div>
                    </div>
                </div>
            </div>

            <div id="view-mtt" class="view-section hidden">
                <div class="mb-8"><h2 class="text-2xl font-bold text-slate-800">ניתוח טורנירים (MTT)</h2></div>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-8" id="mtt-summary"></div>
            </div>
        </main>
    </div>

    <script>
        // Database of users and passwords
        const users = {
            'admin': { pass: 'admin123', role: 'admin', name: 'אדמין' },
            'חיים': { pass: 'חיים177', role: 'agent', name: 'חיים', agentIndex: 0 },
            'איתי': { pass: 'איתי2024', role: 'agent', name: 'איתי', agentIndex: 1 },
            'אבי': { pass: 'אבי2026', role: 'agent', name: 'אבי', agentIndex: 2 },
            'עוז': { pass: 'עוז999', role: 'agent', name: 'עוז', agentIndex: 3 },
            'אלחנן': { pass: 'אלחנן86', role: 'agent', name: 'אלחנן', agentIndex: 4 },
            'יוני': { pass: 'יוני25', role: 'agent', name: 'יוני', agentIndex: 5 }
        };

        const reportData = {
            agents: [
                { name: "חיים", pnl: -8743.12, fee: 2386.16, pastBalance: 0.00, finalSettlement: -6356.96, status: "סוכן חייב למועדון", players: [{ name: "raz121212", pnl: 291.69, fee: 129.32 }, { name: "Snow23", pnl: 206.23, fee: 30.00 }, { name: "or75", pnl: 162.37, fee: 32.57 }, { name: "Leopold2291", pnl: 116.64, fee: 30.00 }, { name: "Kobisayer", pnl: 102.40, fee: 137.68 }, { name: "avim24", pnl: -1055.78, fee: 1008.05 }, { name: "Yana79", pnl: 70.85, fee: 80.00 }, { name: "Yona177", pnl: 0.00, fee: 237.65 }, { name: "YoavAA", pnl: 23.54, fee: 15.00 }, { name: "dasi7", pnl: 3.44, fee: 75.00 }, { name: "Gil", pnl: -453.00, fee: 38.91 }, { name: "Amirgu", pnl: -554.00, fee: 144.67 }, { name: "omar1989!", pnl: -500.00, fee: 621.35 }, { name: "dipstay", pnl: -1440.00, fee: 121.49 }, { name: "Bathens", pnl: -5536.39, fee: 1145.82 }] },
                { name: "איתי", pnl: -3238.36, fee: 1334.72, pastBalance: 0.00, finalSettlement: -1903.64, status: "סוכן חייב למועדון", players: [{ name: "Black Rain82", pnl: 862.65, fee: 232.56 }, { name: "in2024 (כולל in2026)", pnl: 775.41, fee: 257.16 }, { name: "dan13579", pnl: 102.56, fee: 25.77 }, { name: "OTC 1", pnl: -53.34, fee: 173.00 }, { name: "MAXPRESSURE7", pnl: -120.00, fee: 3.00 }, { name: "yaniv!", pnl: -206.27, fee: 333.01 }, { name: "Aviad1111 (כולל Dani1)", pnl: -3632.81, fee: 936.15 }, { name: "EREZ LEVINSHTEIN", pnl: -200.00, fee: 19.20 }] },
                { name: "אבי", pnl: -9715.43, fee: 11159.07, pastBalance: 0.00, finalSettlement: 1443.64, status: "מועדון חייב לסוכן", players: [{ name: "dani shovevani1", pnl: 2871.02, fee: 1054.53 }, { name: "עופר וקנין", pnl: 791.86, fee: 2299.81 }, { name: "P2338-2447", pnl: -365.66, fee: 1913.70 }, { name: "דני", pnl: -900.00, fee: 467.11 }, { name: "אלדד כהן", pnl: -1075.75, fee: 2675.64 }, { name: "yoram3554", pnl: -4419.44, fee: 1924.24 }, { name: "מור קריטי", pnl: -5473.00, fee: 1777.00 }] },
                { name: "עוז", pnl: -2052.09, fee: 812.63, pastBalance: -2550.00, finalSettlement: -3789.46, status: "סוכן חייב למועדון", players: [{ name: "AM26", pnl: 107.00, fee: 439.00 }, { name: "Ofir eliyahu198", pnl: -261.73, fee: 120.16 }, { name: "ozozoz111", pnl: -300.00, fee: 30.19 }, { name: "yosi!!", pnl: -300.00, fee: 60.29 }, { name: "BOOM", pnl: -600.00, fee: 289.00 }, { name: "adirmezin12", pnl: -697.36, fee: 415.74 }] },
                { name: "אלחנן", pnl: 2535.19, fee: 405.94, pastBalance: -2000.00, finalSettlement: 941.13, status: "מועדון חייב לסוכן", players: [{ name: "IzMaR (מאוחד)", pnl: 2813.31, fee: 622.27 }, { name: "Camel63 (כולל Camel)", pnl: -178.12, fee: 28.19 }, { name: "Puntman777", pnl: -100.00, fee: 26.10 }] },
                { name: "יוני", pnl: -26.68, fee: 1.69, pastBalance: 0.00, finalSettlement: -24.99, status: "סוכן חייב למועדון", players: [{ name: "ghost baba", pnl: -26.68, fee: 1.69 }] }
            ]
        };

        let currentUser = null;
        let settlementChart = null, feeChart = null, playerChart = null;

        const format = (v) => new Intl.NumberFormat('he-IL', { minimumFractionDigits: 2 }).format(v);

        function handleLogin() {
            const u = document.getElementById('username').value.trim();
            const p = document.getElementById('password').value;
            const error = document.getElementById('login-error');

            if (users[u] && users[u].pass === p) {
                currentUser = users[u];
                error.classList.add('hidden');
                document.getElementById('login-screen').classList.add('hidden');
                document.getElementById('app-content').classList.remove('hidden');
                setupView();
            } else {
                error.classList.remove('hidden');
            }
        }

        function setupView() {
            const badge = document.getElementById('user-badge');
            const mainNav = document.getElementById('main-nav');
            badge.textContent = currentUser.name;

            if (currentUser.role === 'admin') {
                showDashboard();
            } else {
                // Hide global views for agents
                document.getElementById('nav-dashboard').classList.add('hidden');
                document.getElementById('nav-mtt').classList.add('hidden');
                document.getElementById('agent-selector-container').classList.add('hidden');
                
                // Switch to agents view directly and load their index
                switchTab('nav-agents', 'view-agents');
                renderAgentDetails(reportData.agents[currentUser.agentIndex]);
            }
            
            initNavigation();
        }

        function initNavigation() {
            const tabs = ['nav-dashboard', 'nav-agents', 'nav-mtt'];
            tabs.forEach(tabId => {
                document.getElementById(tabId).onclick = (e) => {
                    const viewId = 'view-' + tabId.split('-')[1];
                    switchTab(tabId, viewId);
                };
            });

            document.getElementById('logout-btn').onclick = () => window.location.reload();

            // Setup agent selector for admin
            const selector = document.getElementById('agent-selector');
            selector.innerHTML = '<option value="">בחר סוכן להצגה...</option>';
            reportData.agents.forEach((a, i) => {
                selector.innerHTML += `<option value="${i}">${a.name}</option>`;
            });
            selector.onchange = (e) => {
                if(e.target.value !== "") renderAgentDetails(reportData.agents[e.target.value]);
            };
        }

        function switchTab(activeBtnId, viewId) {
            document.querySelectorAll('.view-section').forEach(v => v.classList.add('hidden'));
            document.getElementById(viewId).classList.remove('hidden');
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active', 'bg-blue-600', 'text-white'));
            const activeBtn = document.getElementById(activeBtnId);
            activeBtn.classList.add('active', 'bg-blue-600', 'text-white');

            if(viewId === 'view-dashboard') renderDashboard();
            if(viewId === 'view-mtt') renderMTT();
        }

        function renderDashboard() {
            const totalFees = reportData.agents.reduce((s, a) => s + a.fee, 0);
            const net = reportData.agents.reduce((s, a) => s + a.finalSettlement, 0);
            
            document.getElementById('dash-stats').innerHTML = `
                <div class="bg-white p-6 rounded-xl border">
                    <span class="text-xs text-slate-500 font-bold uppercase">סה"כ סוכנים</span>
                    <div class="text-3xl font-bold text-slate-800">${reportData.agents.length}</div>
                </div>
                <div class="bg-white p-6 rounded-xl border">
                    <span class="text-xs text-slate-500 font-bold uppercase">סה"כ עמלות שיוצרו</span>
                    <div class="text-3xl font-bold text-blue-600">${format(totalFees)}</div>
                </div>
                <div class="bg-white p-6 rounded-xl border">
                    <span class="text-xs text-slate-500 font-bold uppercase">מאזן מועדון סופי</span>
                    <div class="text-3xl font-bold ${net >=0 ? 'text-emerald-600' : 'text-red-600'}">${format(net)}</div>
                </div>
            `;

            if(settlementChart) settlementChart.destroy();
            settlementChart = new Chart(document.getElementById('settlementChart'), {
                type: 'bar',
                data: {
                    labels: reportData.agents.map(a => a.name),
                    datasets: [{ data: reportData.agents.map(a => a.finalSettlement), backgroundColor: reportData.agents.map(a => a.finalSettlement >= 0 ? '#10b981' : '#f43f5e'), borderRadius: 6 }]
                },
                options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } } }
            });

            if(feeChart) feeChart.destroy();
            feeChart = new Chart(document.getElementById('feeDistributionChart'), {
                type: 'doughnut',
                data: {
                    labels: reportData.agents.map(a => a.name),
                    datasets: [{ data: reportData.agents.map(a => a.fee), backgroundColor: ['#3b82f6', '#10b981', '#f59e0b', '#ef4444', '#8b5cf6', '#6366f1'] }]
                },
                options: { responsive: true, maintainAspectRatio: false }
            });
        }

        function renderAgentDetails(agent) {
            const isPos = agent.finalSettlement >= 0;
            document.getElementById('agent-metrics').innerHTML = `
                <div class="bg-slate-50 p-4 rounded-lg border">
                    <span class="text-[10px] font-bold text-slate-500 uppercase">P&L שחקנים</span>
                    <div class="text-xl font-bold ${agent.pnl >= 0 ? 'text-emerald-600' : 'text-red-600'}">${format(agent.pnl)}</div>
                </div>
                <div class="bg-slate-50 p-4 rounded-lg border">
                    <span class="text-[10px] font-bold text-slate-500 uppercase">עמלת סוכן</span>
                    <div class="text-xl font-bold text-blue-600">${format(agent.fee)}</div>
                </div>
                <div class="bg-slate-50 p-4 rounded-lg border">
                    <span class="text-[10px] font-bold text-slate-500 uppercase">יתרת עבר</span>
                    <div class="text-xl font-bold text-slate-700">${format(agent.pastBalance)}</div>
                </div>
                <div class="${isPos ? 'bg-emerald-50 border-emerald-200' : 'bg-red-50 border-red-200'} p-4 rounded-lg border">
                    <span class="text-[10px] font-bold ${isPos ? 'text-emerald-700' : 'text-red-700'} uppercase">שורת התחשבנות</span>
                    <div class="text-2xl font-black ${isPos ? 'text-emerald-600' : 'text-red-600'}">${format(agent.finalSettlement)}</div>
                </div>
            `;

            const tbody = document.getElementById('agent-players-table');
            tbody.innerHTML = agent.players.map(p => `
                <tr class="hover:bg-slate-50">
                    <td class="p-4 font-medium text-slate-700">${p.name}</td>
                    <td class="p-4 font-bold ${p.pnl >=0 ? 'text-emerald-600' : 'text-red-600'}">${format(p.pnl)}</td>
                    <td class="p-4 text-slate-500">${format(p.fee)}</td>
                </tr>
            `).join('');

            const topP = [...agent.players].sort((a,b) => Math.abs(b.pnl) - Math.abs(a.pnl)).slice(0, 6);
            if(playerChart) playerChart.destroy();
            playerChart = new Chart(document.getElementById('agentPlayersChart'), {
                type: 'bar',
                data: {
                    labels: topP.map(p => p.name),
                    datasets: [{ data: topP.map(p => p.pnl), backgroundColor: topP.map(p => p.pnl >= 0 ? '#10b981' : '#f43f5e'), borderRadius: 4 }]
                },
                options: { indexAxis: 'y', responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } } }
            });
        }

        function renderMTT() {
            document.getElementById('mtt-summary').innerHTML = `
                <div class="bg-white p-8 rounded-xl border border-red-100 border-t-4 border-t-red-500 shadow-sm">
                    <h3 class="text-xl font-bold text-slate-800 mb-6">סיכום זליגת כספים</h3>
                    <div class="space-y-4">
                        <div class="flex justify-between p-3 bg-slate-50 rounded"><span>Overlay (GTD)</span><span class="font-bold text-red-600">-1,920.00</span></div>
                        <div class="flex justify-between p-3 bg-slate-50 rounded"><span>הפסד שחקני בית</span><span class="font-bold text-red-600">-236.03</span></div>
                        <div class="flex justify-between p-4 bg-red-600 text-white rounded-lg shadow mt-6">
                            <span class="font-bold">עלות מועדון כוללת</span><span class="text-2xl font-black">-2,156.03</span>
                        </div>
                    </div>
                </div>
                <div class="bg-white p-8 rounded-xl border shadow-sm">
                    <h3 class="text-xl font-bold text-slate-800 mb-4">המלצות ניהול</h3>
                    <p class="text-slate-600 leading-relaxed">הנתונים מראים כי הטורנירים הנוכחיים אינם כלכליים. שחקני הבית לא מצליחים למשוך את כספי ה-GTD בחזרה פנימה.</p>
                    <div class="mt-6 p-4 bg-blue-50 border-r-4 border-blue-500 rounded text-blue-900 font-medium">
                        מומלץ להקטין GTD בפרירולים ולהגדיל עלויות Re-Entry.
                    </div>
                </div>
            `;
        }

        document.getElementById('login-btn').onclick = handleLogin;
        document.getElementById('password').onkeypress = (e) => e.key === 'Enter' ? handleLogin() : null;

    </script>
</body>
</html>
