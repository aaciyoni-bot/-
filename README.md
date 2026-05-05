<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>דוח התחשבנות מועדון - גרסה סופית ומבוקרת</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    
    <!-- Chosen Palette: Professional Slate & Blue (High Accuracy Focus) -->
    
    <!-- Application Structure Plan: 
         1. Secure Login: Admin/Agent access.
         2. Audited Data: reportData now contains the 100% Fee values for every player, verified against CSV exports.
         3. Dynamic Calculation: The UI calculates Agent Commission (60%/80%) on-the-fly to ensure transparency.
         4. Combined Views: All merged players (like in2024+in2026) are consolidated into single logical entries as requested. -->

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
    </style>
</head>
<body class="min-h-screen flex flex-col">

    <!-- Login Screen -->
    <div id="login-screen" class="fixed inset-0 z-50 flex items-center justify-center p-4" style="background-color: #0f172a;">
        <div class="bg-white p-8 rounded-2xl shadow-2xl w-full max-w-md border border-slate-200">
            <div class="text-center mb-8">
                <div class="inline-flex items-center justify-center w-16 h-16 bg-blue-100 text-blue-600 rounded-full mb-4 font-bold text-2xl">
                    $
                </div>
                <h2 class="text-2xl font-bold text-slate-800">כניסה למערכת מאובטחת</h2>
                <p class="text-slate-500 mt-2">נתוני התחשבנות כספית - לשימוש פנימי בלבד</p>
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
                <div id="login-error" class="text-red-500 text-sm hidden text-center font-medium">פרטי התחברות שגויים</div>
                <button id="login-btn" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 rounded-lg transition-colors mt-4">התחבר למערכת</button>
            </div>
        </div>
    </div>

    <!-- Application Content -->
    <div id="app-content" class="hidden flex flex-col min-h-screen">
        <header class="bg-white shadow-sm border-b border-slate-200 py-4">
            <div class="container mx-auto px-4 lg:px-8 max-w-7xl">
                <div class="flex flex-col md:flex-row justify-between items-center gap-4">
                    <div class="flex items-center gap-3">
                        <h1 class="text-2xl font-bold text-slate-800 tracking-tight">💼 התחשבנות סוכנים</h1>
                        <span id="user-badge" class="bg-blue-50 text-blue-700 px-3 py-1 rounded-full text-xs font-black uppercase border border-blue-100"></span>
                    </div>
                    <nav class="flex gap-2 items-center">
                        <div id="main-nav" class="flex gap-2 mr-4">
                            <button id="nav-dashboard" class="nav-btn active px-4 py-2 rounded-lg border border-slate-200 text-slate-700 font-medium whitespace-nowrap">📊 דאשבורד</button>
                            <button id="nav-agents" class="nav-btn px-4 py-2 rounded-lg border border-slate-200 text-slate-700 font-medium whitespace-nowrap">👥 דוחות</button>
                            <button id="nav-mtt" class="nav-btn px-4 py-2 rounded-lg border border-slate-200 text-slate-700 font-medium whitespace-nowrap">🏆 טורנירים</button>
                        </div>
                        <button id="logout-btn" class="text-slate-400 hover:text-red-600 p-2" title="התנתק">
                            🚪
                        </button>
                    </nav>
                </div>
            </div>
        </header>

        <main class="flex-grow container mx-auto px-4 lg:px-8 max-w-7xl py-8">
            <div id="view-dashboard" class="view-section block">
                <div class="mb-8">
                    <h2 class="text-2xl font-bold text-slate-800 mb-2">תמונת מצב מועדון</h2>
                    <p class="text-slate-600">סיכום התחשבנות גלובלי - כלל הנתונים נבדקו מול קבצי הייצוא.</p>
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
                    <div class="bg-white rounded-xl shadow-sm border border-slate-200 p-6">
                        <h3 class="text-lg font-bold text-slate-800 mb-4 border-b border-slate-100 pb-2">פירוט שחקנים (עמלות ב-100%)</h3>
                        <div class="overflow-x-auto">
                            <table class="w-full text-right border-collapse">
                                <thead class="bg-slate-50 text-slate-500 text-sm">
                                    <tr>
                                        <th class="p-4 border-b text-right">שם שחקן</th>
                                        <th class="p-4 border-b text-left">תוצאה (P&L)</th>
                                        <th class="p-4 border-b text-left">עמלה שיוצרה (100%)</th>
                                    </tr>
                                </thead>
                                <tbody id="agent-players-table" class="divide-y text-slate-700"></tbody>
                            </table>
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
                {
                    name: "חיים",
                    pnl: -8743.12,
                    totalFee100: 3976.93, // Sum of verified players 100% fees
                    pastBalance: 0.00,
                    finalSettlement: -6356.96,
                    status: "סוכן חייב למועדון",
                    players: [
                        { name: "avim24", pnl: -1055.78, fee: 1008.05 },
                        { name: "Bathens", pnl: -5536.39, fee: 1145.82 },
                        { name: "omar1989!", pnl: -500.00, fee: 621.35 },
                        { name: "raz121212", pnl: 291.69, fee: 129.32 },
                        { name: "Kobisayer", pnl: 102.40, fee: 137.68 },
                        { name: "Yona177", pnl: 0.00, fee: 237.65 },
                        { name: "Amirgu", pnl: -554.00, fee: 144.67 },
                        { name: "dipstay", pnl: -1440.00, fee: 121.49 },
                        { name: "Gil", pnl: -453.00, fee: 38.91 },
                        { name: "or75", pnl: 162.37, fee: 32.57 },
                        { name: "Snow23", pnl: 206.23, fee: 30.00 },
                        { name: "Leopold2291", pnl: 116.64, fee: 30.00 },
                        { name: "Yana79", pnl: 70.85, fee: 80.00 },
                        { name: "dasi7", pnl: 3.44, fee: 75.00 },
                        { name: "YoavAA", pnl: 23.54, fee: 15.00 },
                        { name: "Baraks1", pnl: -55.00, fee: 98.52 },
                        { name: "Adi Rahimian", pnl: -126.11, fee: 30.90 }
                    ]
                },
                {
                    name: "איתי",
                    pnl: -3238.36,
                    totalFee100: 2224.53,
                    pastBalance: 0.00,
                    finalSettlement: -1903.64,
                    status: "סוכן חייב למועדון",
                    players: [
                        { name: "Aviad1111 (מאוחד)", pnl: -3632.81, fee: 936.15 },
                        { name: "in2024 (מאוחד)", pnl: 775.41, fee: 257.16 },
                        { name: "yaniv!", pnl: -206.27, fee: 333.01 },
                        { name: "Black Rain82", pnl: 862.65, fee: 232.56 },
                        { name: "heziza", pnl: -400.00, fee: 84.56 },
                        { name: "Dididrogba", pnl: -266.56, fee: 120.12 },
                        { name: "EREZ LEVINSHTEIN", pnl: -200.00, fee: 19.20 },
                        { name: "OTC 1", pnl: -53.34, fee: 173.00 },
                        { name: "dan13579", pnl: 102.56, fee: 25.77 },
                        { name: "govo22", pnl: -100.00, fee: 40.00 },
                        { name: "MAXPRESSURE7", pnl: -120.00, fee: 3.00 }
                    ]
                },
                {
                    name: "אבי",
                    pnl: -9715.43,
                    totalFee100: 13948.84,
                    pastBalance: 0.00,
                    finalSettlement: 1443.64,
                    status: "מועדון חייב לסוכן",
                    players: [
                        { name: "אלדד כהן", pnl: -1075.75, fee: 2675.64 },
                        { name: "עופר וקנין", pnl: 791.86, fee: 2299.81 },
                        { name: "yoram3554", pnl: -4419.44, fee: 1924.24 },
                        { name: "P2338-2447", pnl: -365.66, fee: 1913.70 },
                        { name: "Alof mlyda", pnl: -1144.46, fee: 1836.81 },
                        { name: "מור קריטי", pnl: -5473.00, fee: 1777.00 },
                        { name: "dani shovevani1", pnl: 2871.02, fee: 1054.53 },
                        { name: "דני", pnl: -900.00, fee: 467.11 }
                    ]
                },
                {
                    name: "עוז",
                    pnl: -2052.09,
                    totalFee100: 1354.38,
                    pastBalance: -2550.00,
                    finalSettlement: -3789.46,
                    status: "סוכן חייב למועדון",
                    players: [
                        { name: "AM26", pnl: 107.00, fee: 439.00 },
                        { name: "BOOM", pnl: -600.00, fee: 289.00 },
                        { name: "adirmezin12", pnl: -697.36, fee: 415.74 },
                        { name: "Ofir eliyahu198", pnl: -261.73, fee: 120.16 },
                        { name: "yosi!!", pnl: -300.00, fee: 60.29 },
                        { name: "ozozoz111", pnl: -300.00, fee: 30.19 },
                        { name: "israel999", pnl: 0.00, fee: 0.00 }
                    ]
                },
                {
                    name: "אלחנן",
                    pnl: 2535.19,
                    totalFee100: 676.56,
                    pastBalance: -2000.00,
                    finalSettlement: 941.13,
                    status: "מועדון חייב לסוכן",
                    players: [
                        { name: "IzMaR (מאוחד)", pnl: 2813.31, fee: 622.27 },
                        { name: "Camel63 (מאוחד)", pnl: -178.12, fee: 28.19 },
                        { name: "Puntman777", pnl: -100.00, fee: 26.10 },
                        { name: "naki t", pnl: 0.00, fee: 0.00 }
                    ]
                },
                {
                    name: "יוני",
                    pnl: -26.68,
                    totalFee100: 2.81,
                    pastBalance: 0.00,
                    finalSettlement: -24.99,
                    status: "סוכן חייב למועדון",
                    players: [
                        { name: "ghost baba", pnl: -26.68, fee: 2.81 },
                        { name: "Kepler36b", pnl: 0.00, fee: 0.00 },
                        { name: "levinson yoram", pnl: 0.00, fee: 0.00 }
                    ]
                }
            ]
        };

        let currentUser = null;
        let settlementChart = null, feeChart = null;

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
            badge.textContent = currentUser.name;

            if (currentUser.role === 'admin') {
                switchTab('nav-dashboard', 'view-dashboard');
            } else {
                document.getElementById('nav-dashboard').classList.add('hidden');
                document.getElementById('nav-mtt').classList.add('hidden');
                document.getElementById('agent-selector-container').classList.add('hidden');
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
            const totalFees = reportData.agents.reduce((s, a) => s + a.totalFee100, 0);
            const net = reportData.agents.reduce((s, a) => s + a.finalSettlement, 0);
            document.getElementById('dash-stats').innerHTML = `
                <div class="bg-white p-6 rounded-xl border"><span class="text-xs text-slate-500 font-bold uppercase tracking-widest">סה"כ סוכנים</span><div class="text-3xl font-bold text-slate-800">${reportData.agents.length}</div></div>
                <div class="bg-white p-6 rounded-xl border"><span class="text-xs text-slate-500 font-bold uppercase tracking-widest">סה"כ עמלות ברוטו (100%)</span><div class="text-3xl font-bold text-blue-600">${format(totalFees)}</div></div>
                <div class="bg-white p-6 rounded-xl border"><span class="text-xs text-slate-500 font-bold uppercase tracking-widest">מאזן מועדון סופי</span><div class="text-3xl font-bold ${net >=0 ? 'text-emerald-600' : 'text-red-600'}">${format(net)}</div></div>
            `;
            if(settlementChart) settlementChart.destroy();
            settlementChart = new Chart(document.getElementById('settlementChart'), {
                type: 'bar',
                data: { labels: reportData.agents.map(a => a.name), datasets: [{ data: reportData.agents.map(a => a.finalSettlement), backgroundColor: reportData.agents.map(a => a.finalSettlement >= 0 ? '#10b981' : '#f43f5e'), borderRadius: 6 }] },
                options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } } }
            });
            if(feeChart) feeChart.destroy();
            feeChart = new Chart(document.getElementById('feeDistributionChart'), {
                type: 'doughnut',
                data: { labels: reportData.agents.map(a => a.name), datasets: [{ data: reportData.agents.map(a => a.totalFee100), backgroundColor: ['#3b82f6', '#10b981', '#f59e0b', '#ef4444', '#8b5cf6', '#6366f1'] }] },
                options: { responsive: true, maintainAspectRatio: false }
            });
        }

        function renderAgentDetails(agent) {
            const isPos = agent.finalSettlement >= 0;
            const rate = agent.name === "אבי" ? 0.8 : 0.6;
            const rateText = agent.name === "אבי" ? "80%" : "60%";
            
            document.getElementById('agent-metrics').innerHTML = `
                <div class="bg-slate-50 p-4 rounded-lg border"><span class="text-[10px] font-bold text-slate-500 uppercase">P&L שחקנים</span><div class="text-xl font-bold ${agent.pnl >= 0 ? 'text-emerald-600' : 'text-red-600'}">${format(agent.pnl)}</div></div>
                <div class="bg-slate-50 p-4 rounded-lg border"><span class="text-[10px] font-bold text-slate-500 uppercase">עמלת סוכן (${rateText})</span><div class="text-xl font-bold text-blue-600">${format(agent.totalFee100 * rate)}</div></div>
                <div class="bg-slate-50 p-4 rounded-lg border"><span class="text-[10px] font-bold text-slate-500 uppercase">יתרת עבר</span><div class="text-xl font-bold text-slate-700">${format(agent.pastBalance)}</div></div>
                <div class="${isPos ? 'bg-emerald-50 border-emerald-200' : 'bg-red-50 border-red-200'} p-4 rounded-lg border"><span class="text-[10px] font-bold ${isPos ? 'text-emerald-700' : 'text-red-700'} uppercase">שורת התחשבנות</span><div class="text-2xl font-black ${isPos ? 'text-emerald-600' : 'text-red-600'}">${format(agent.finalSettlement)}</div></div>
            `;
            const tbody = document.getElementById('agent-players-table');
            tbody.innerHTML = agent.players.map(p => `
                <tr class="hover:bg-slate-50 border-b border-slate-100 transition-colors">
                    <td class="p-4 font-medium text-slate-800">${p.name}</td>
                    <td class="p-4 font-bold text-left ${p.pnl >=0 ? 'text-emerald-600' : 'text-red-600'}" dir="ltr">${format(p.pnl)}</td>
                    <td class="p-4 text-left text-slate-600" dir="ltr">${format(p.fee)}</td>
                </tr>
            `).join('');
        }

        function renderMTT() {
            document.getElementById('mtt-summary').innerHTML = `
                <div class="bg-white p-8 rounded-xl border border-red-100 border-t-4 border-t-red-500 shadow-sm">
                    <h3 class="text-xl font-bold text-slate-800 mb-6 text-center">סיכום זליגת כספים</h3>
                    <div class="space-y-4">
                        <div class="flex justify-between p-3 bg-slate-50 rounded"><span>Overlay (GTD)</span><span class="font-bold text-red-600">-1,920.00</span></div>
                        <div class="flex justify-between p-3 bg-slate-50 rounded"><span>הפסד שחקני בית</span><span class="font-bold text-red-600">-236.03</span></div>
                        <div class="flex justify-between p-4 bg-red-600 text-white rounded-lg shadow mt-6"><span class="font-bold">עלות מועדון כוללת</span><span class="text-2xl font-black">-2,156.03</span></div>
                    </div>
                </div>
                <div class="bg-white p-8 rounded-xl border shadow-sm">
                    <h3 class="text-xl font-bold text-slate-800 mb-4">המלצות ניהול</h3>
                    <p class="text-slate-600 leading-relaxed">הנתונים מראים כי הטורנירים הנוכחיים אינם כלכליים. שחקני הבית לא מצליחים למשוך את כספי ה-GTD בחזרה פנימה.</p>
                    <div class="mt-6 p-4 bg-blue-50 border-r-4 border-blue-500 rounded text-blue-900 font-medium">מומלץ להקטין GTD בפרירולים ולהגדיל עלויות Re-Entry.</div>
                </div>
            `;
        }

        document.getElementById('login-btn').onclick = handleLogin;
        document.getElementById('password').onkeypress = (e) => e.key === 'Enter' ? handleLogin() : null;
    </script>
</body>
</html>
