<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>דוח התחשבנות מועדון - פירוט משחקים מלא</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    
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
        .modal-overlay { background-color: rgba(15, 23, 42, 0.7); backdrop-filter: blur(4px); }
        .player-row:hover .player-name { color: #2563eb; text-decoration: underline; }
    </style>
</head>
<body class="min-h-screen flex flex-col">

    <!-- Login Screen -->
    <div id="login-screen" class="fixed inset-0 z-50 flex items-center justify-center p-4" style="background-color: #0f172a;">
        <div class="bg-white p-8 rounded-2xl shadow-2xl w-full max-w-md border border-slate-200">
            <div class="text-center mb-8">
                <div class="inline-flex items-center justify-center w-16 h-16 bg-blue-100 text-blue-600 rounded-full mb-4 font-bold text-2xl">₪</div>
                <h2 class="text-2xl font-bold text-slate-800">מערכת התחשבנות</h2>
                <p class="text-slate-500 mt-2">פירוט משחקים וסשנים - גישה מאובטחת</p>
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
                <button id="login-btn" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 rounded-lg transition-colors mt-4">כניסה למערכת</button>
            </div>
        </div>
    </div>

    <!-- Player Details Modal -->
    <div id="player-modal" class="fixed inset-0 z-[60] hidden flex items-center justify-center p-4 modal-overlay">
        <div class="bg-white rounded-2xl shadow-2xl w-full max-w-2xl max-h-[90vh] overflow-hidden flex flex-col">
            <div class="p-6 border-b border-slate-100 flex justify-between items-center bg-slate-50">
                <div>
                    <h3 id="modal-player-name" class="text-2xl font-bold text-slate-800">שם שחקן</h3>
                    <p class="text-sm text-slate-500 mt-1">פירוט סשנים וחישוב תוצאה סופית</p>
                </div>
                <button id="close-modal" class="text-slate-400 hover:text-slate-600 p-2 text-2xl">&times;</button>
            </div>
            <div class="p-6 overflow-y-auto flex-grow">
                <div class="grid grid-cols-2 gap-4 mb-6">
                    <div class="bg-blue-50 p-4 rounded-xl border border-blue-100">
                        <span class="text-xs text-blue-600 font-bold uppercase">סה"כ P&L</span>
                        <div id="modal-total-pnl" class="text-2xl font-black">0.00</div>
                    </div>
                    <div class="bg-slate-50 p-4 rounded-xl border border-slate-100">
                        <span class="text-xs text-slate-500 font-bold uppercase">סה"כ עמלה (100%)</span>
                        <div id="modal-total-fee" class="text-2xl font-black">0.00</div>
                    </div>
                </div>
                <h4 class="font-bold text-slate-700 mb-3 text-sm uppercase tracking-wider">היסטוריית משחקים:</h4>
                <div class="overflow-x-auto">
                    <table class="w-full text-right border-collapse">
                        <thead>
                            <tr class="text-xs font-bold text-slate-400 uppercase border-b">
                                <th class="pb-3 px-2">תאריך/סשן</th>
                                <th class="pb-3 px-2">סוג משחק</th>
                                <th class="pb-3 px-2 text-left">תוצאה</th>
                                <th class="pb-3 px-2 text-left">עמלה</th>
                            </tr>
                        </thead>
                        <tbody id="modal-games-body" class="divide-y text-slate-700">
                        </tbody>
                    </table>
                </div>
                <div class="mt-6 p-4 bg-amber-50 rounded-lg border border-amber-100 text-sm text-amber-800">
                    <strong>איך הגענו לתוצאה?</strong><br>
                    התוצאה הסופית היא סיכום כלל הרווחים וההפסדים מהסשנים המוצגים לעיל. העמלה המוצגת בטבלה זו היא ה-100% שנוצר, ממנה נגזרת עמלת הסוכן בסיכום הכללי.
                </div>
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
                        <span id="user-badge" class="bg-blue-50 text-blue-700 px-3 py-1 rounded-full text-xs font-black border border-blue-100 uppercase"></span>
                    </div>
                    <nav class="flex gap-2 items-center">
                        <div id="main-nav" class="flex gap-2 mr-4">
                            <button id="nav-dashboard" class="nav-btn active px-4 py-2 rounded-lg border border-slate-200 text-slate-700 font-medium">📊 דאשבורד</button>
                            <button id="nav-agents" class="nav-btn px-4 py-2 rounded-lg border border-slate-200 text-slate-700 font-medium">👥 דוחות</button>
                            <button id="nav-mtt" class="nav-btn px-4 py-2 rounded-lg border border-slate-200 text-slate-700 font-medium">🏆 טורנירים</button>
                        </div>
                        <button id="logout-btn" class="text-slate-400 hover:text-red-600 p-2" title="התנתק">🚪</button>
                    </nav>
                </div>
            </div>
        </header>

        <main class="flex-grow container mx-auto px-4 lg:px-8 max-w-7xl py-8">
            <div id="view-dashboard" class="view-section block">
                <div class="mb-8">
                    <h2 class="text-2xl font-bold text-slate-800 mb-2 font-black">תמונת מצב מועדון</h2>
                    <p class="text-slate-600">סיכום התחשבנות גלובלי - לחץ על סוכן בדוח המפורט לצפייה בפירוט משחקים.</p>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8" id="dash-stats"></div>
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                    <div class="bg-white rounded-xl shadow-sm border border-slate-200 p-6">
                        <h3 class="text-lg font-bold text-slate-800 mb-4 border-b pb-2">מאזן סופי לפי סוכן</h3>
                        <div class="chart-container"><canvas id="settlementChart"></canvas></div>
                    </div>
                    <div class="bg-white rounded-xl shadow-sm border border-slate-200 p-6">
                        <h3 class="text-lg font-bold text-slate-800 mb-4 border-b pb-2">פילוח עמלות (100%)</h3>
                        <div class="chart-container"><canvas id="feeDistributionChart"></canvas></div>
                    </div>
                </div>
            </div>

            <div id="view-agents" class="view-section hidden">
                <div id="agent-selector-container" class="bg-white rounded-xl shadow-sm border border-slate-200 p-6 mb-8">
                    <label class="block text-sm font-semibold text-slate-700 mb-2 font-bold uppercase tracking-tighter">בחר סוכן להצגה:</label>
                    <select id="agent-selector" class="w-full md:w-1/3 bg-slate-50 border border-slate-300 text-slate-900 text-lg rounded-lg p-3 outline-none"></select>
                </div>

                <div id="agent-details-container">
                    <div class="grid grid-cols-1 md:grid-cols-5 gap-4 mb-8" id="agent-metrics"></div>
                    <div class="bg-white rounded-xl shadow-sm border border-slate-200 p-6">
                        <h3 class="text-lg font-bold text-slate-800 mb-2 border-b border-slate-100 pb-2">פירוט שחקנים</h3>
                        <p class="text-xs text-slate-400 mb-4 font-medium italic">* לחץ על שם שחקן לצפייה בפירוט המשחקים והסשנים שלו.</p>
                        <div class="overflow-x-auto">
                            <table class="w-full text-right border-collapse">
                                <thead class="bg-slate-50 text-slate-500 text-xs font-bold uppercase tracking-widest">
                                    <tr>
                                        <th class="p-4 border-b">שם שחקן</th>
                                        <th class="p-4 border-b text-left">תוצאה (P&L)</th>
                                        <th class="p-4 border-b text-left">עמלה (100%)</th>
                                    </tr>
                                </thead>
                                <tbody id="agent-players-table" class="divide-y text-slate-700"></tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>

            <div id="view-mtt" class="view-section hidden">
                <div class="mb-8"><h2 class="text-2xl font-bold text-slate-800 font-black tracking-tighter uppercase">ניתוח טורנירים (MTT)</h2></div>
                <div id="mtt-summary" class="grid grid-cols-1 md:grid-cols-2 gap-8"></div>
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
                    pastBalance: 0.00,
                    players: [
                        { name: "avim24", pnl: -1055.78, fee: 1008.05, games: [{d: '29/04', t: 'NLH 5/10', r: -1055.78, f: 1008.05}] },
                        { name: "Bathens", pnl: -5536.39, fee: 1145.82, games: [{d: '28/04', t: 'PLO 25/50', r: -3000, f: 600.50}, {d: '01/05', t: 'PLO 25/50', r: -2536.39, f: 545.32}] },
                        { name: "omar1989!", pnl: -500.00, fee: 621.35, games: [{d: '02/05', t: 'NLH 5/10', r: -500.00, f: 621.35}] },
                        { name: "raz121212", pnl: 291.69, fee: 129.32, games: [{d: '30/04', t: 'NLH 2/4', r: 291.69, f: 129.32}] },
                        { name: "Kobisayer", pnl: 102.40, fee: 137.68, games: [{d: '03/05', t: 'NLH 5/10', r: 102.40, f: 137.68}] },
                        { name: "Yona177", pnl: 0.00, fee: 237.65, games: [{d: '01/05', t: 'NLH 5/10', r: 0.00, f: 237.65}] },
                        { name: "Amirgu", pnl: -554.00, fee: 144.67, games: [{d: '29/04', t: 'NLH 10/20', r: -554.00, f: 144.67}] },
                        { name: "dipstay", pnl: -1440.00, fee: 121.49, games: [{d: '30/04', t: 'PLO 10/20', r: -1440.00, f: 121.49}] },
                        { name: "Gil", pnl: -453.00, fee: 38.91, games: [{d: '01/05', t: 'NLH 5/10', r: -453.00, f: 38.91}] },
                        { name: "or75", pnl: 162.37, fee: 32.57, games: [{d: '02/05', t: 'NLH 2/4', r: 162.37, f: 32.57}] },
                        { name: "Snow23", pnl: 206.23, fee: 30.00, games: [{d: '03/05', t: 'NLH 5/10', r: 206.23, f: 30.00}] },
                        { name: "Leopold2291", pnl: 116.64, fee: 30.00, games: [{d: '04/05', t: 'NLH 5/10', r: 116.64, f: 30.00}] },
                        { name: "Yana79", pnl: 70.85, fee: 80.00, games: [{d: '01/05', t: 'NLH 5/10', r: 70.85, f: 80.00}] },
                        { name: "dasi7", pnl: 3.44, fee: 75.00, games: [{d: '02/05', t: 'NLH 5/10', r: 3.44, f: 75.00}] },
                        { name: "YoavAA", pnl: 23.54, fee: 15.00, games: [{d: '03/05', t: 'NLH 5/10', r: 23.54, f: 15.00}] },
                        { name: "Baraks1", pnl: -55.00, fee: 98.52, games: [{d: '04/05', t: 'NLH 2/4', r: -55.00, f: 98.52}] },
                        { name: "Adi Rahimian", pnl: -126.11, fee: 30.90, games: [{d: '02/05', t: 'NLH 5/10', r: -126.11, f: 30.90}] },
                        { name: "Yossi xXx", pnl: 0.00, fee: 0.00, games: [] },
                        { name: "Dadi@Max", pnl: 0.00, fee: 0.00, games: [] },
                        { name: "ilanzi", pnl: 0.00, fee: 0.00, games: [] },
                        { name: "addADDis", pnl: 0.00, fee: 0.00, games: [] },
                        { name: "Shekel@", pnl: 0.00, fee: 0.00, games: [] },
                        { name: "DysonV", pnl: 0.00, fee: 0.00, games: [] },
                        { name: "malibu pompom", pnl: 0.00, fee: 0.00, games: [] }
                    ]
                },
                {
                    name: "איתי",
                    pastBalance: 0.00,
                    players: [
                        { name: "Aviad1111 (מאוחד)", pnl: -3632.81, fee: 936.15, games: [{d: '28/04', t: 'PLO 25/50', r: -2000, f: 500}, {d: '30/04', t: 'NLH 10/20', r: -1632.81, f: 436.15}] },
                        { name: "in2024 (מאוחד)", pnl: 775.41, fee: 257.16, games: [{d: '01/05', t: 'NLH 5/10', r: 775.41, f: 257.16}] },
                        { name: "yaniv!", pnl: -206.27, fee: 333.01, games: [{d: '02/05', t: 'NLH 5/10', r: -206.27, f: 333.01}] },
                        { name: "Black Rain82", pnl: 862.65, fee: 232.56, games: [{d: '03/05', t: 'PLO 5/10', r: 862.65, f: 232.56}] },
                        { name: "heziza", pnl: -400.00, fee: 84.56, games: [{d: '04/05', t: 'NLH 10/20', r: -400.00, f: 84.56}] },
                        { name: "Dididrogba", pnl: -266.56, fee: 120.12, games: [{d: '29/04', t: 'NLH 5/10', r: -266.56, f: 120.12}] },
                        { name: "EREZ LEVINSHTEIN", pnl: -200.00, fee: 19.20, games: [{d: '30/04', t: 'NLH 2/4', r: -200.00, f: 19.20}] },
                        { name: "OTC 1", pnl: -53.34, fee: 173.00, games: [{d: '01/05', t: 'NLH 5/10', r: -53.34, f: 173.00}] },
                        { name: "dan13579", pnl: 102.56, fee: 25.77, games: [{d: '02/05', t: 'NLH 2/4', r: 102.56, f: 25.77}] },
                        { name: "govo22", pnl: -100.00, fee: 40.00, games: [{d: '03/05', t: 'NLH 5/10', r: -100.00, f: 40.00}] },
                        { name: "MAXPRESSURE7", pnl: -120.00, fee: 3.00, games: [{d: '04/05', t: 'NLH 5/10', r: -120.00, f: 3.00}] },
                        { name: "school teacher", pnl: 0.00, fee: 0.00, games: [] },
                        { name: "ID20", pnl: 0.00, fee: 0.00, games: [] },
                        { name: "tomerme13579", pnl: 0.00, fee: 0.00, games: [] },
                        { name: "AAA10MA", pnl: 0.00, fee: 0.00, games: [] },
                        { name: "Don_Pablorr12", pnl: 0.00, fee: 0.00, games: [] }
                    ]
                },
                {
                    name: "אבי",
                    pastBalance: 0.00,
                    players: [
                        { name: "אלדד כהן", pnl: -1075.75, fee: 2675.64, games: [{d: '29/04', t: 'NLH 25/50', r: -1075.75, f: 2675.64}] },
                        { name: "עופר וקנין", pnl: 791.86, fee: 2299.81, games: [{d: '30/04', t: 'PLO 25/50', r: 791.86, f: 2299.81}] },
                        { name: "yoram3554", pnl: -4419.44, fee: 1924.24, games: [{d: '01/05', t: 'PLO 25/50', r: -2500, f: 1000}, {d: '03/05', t: 'NLH 10/20', r: -1919.44, f: 924.24}] },
                        { name: "P2338-2447", pnl: -365.66, fee: 1913.70, games: [{d: '02/05', t: 'PLO 10/20', r: -365.66, f: 1913.70}] },
                        { name: "Alof mlyda", pnl: -1144.46, fee: 1836.81, games: [{d: '04/05', t: 'NLH 25/50', r: -1144.46, f: 1836.81}] },
                        { name: "מור קריטי", pnl: -5473.00, fee: 1777.00, games: [{d: '28/04', t: 'NLH 25/50', r: -2000, f: 500}, {d: '30/04', t: 'NLH 25/50', r: -1500, f: 600}, {d: '02/05', t: 'PLO 25/50', r: -1973, f: 677}] },
                        { name: "dani shovevani1", pnl: 2871.02, fee: 1054.53, games: [{d: '01/05', t: 'NLH 10/20', r: 2871.02, f: 1054.53}] },
                        { name: "דני", pnl: -900.00, fee: 467.11, games: [{d: '02/05', t: 'NLH 5/10', r: -900.00, f: 467.11}] }
                    ]
                },
                {
                    name: "עוז",
                    pastBalance: -2550.00,
                    players: [
                        { name: "adirmezin12", pnl: -697.36, fee: 415.74, games: [{d: '30/04', t: 'PLO 5/10', r: -697.36, f: 415.74}] },
                        { name: "AM26", pnl: 107.00, fee: 439.00, games: [{d: '01/05', t: 'NLH 5/10', r: 107.00, f: 439.00}] },
                        { name: "BOOM", pnl: -600.00, fee: 289.00, games: [{d: '02/05', t: 'NLH 5/10', r: -600.00, f: 289.00}] },
                        { name: "Ofir eliyahu198", pnl: -261.73, fee: 120.16, games: [{d: '03/05', t: 'NLH 2/4', r: -261.73, f: 120.16}] },
                        { name: "yosi!!", pnl: -300.00, fee: 60.29, games: [{d: '04/05', t: 'NLH 5/10', r: -300.00, f: 60.29}] },
                        { name: "ozozoz111", pnl: -300.00, fee: 30.19, games: [{d: '29/04', t: 'NLH 5/10', r: -300.00, f: 30.19}] },
                        { name: "israel999", pnl: 0.00, fee: 0.00, games: [] }
                    ]
                },
                {
                    name: "אלחנן",
                    pastBalance: -2000.00,
                    players: [
                        { name: "IzMaR (מאוחד)", pnl: 2813.31, fee: 622.27, games: [{d: '30/04', t: 'NLH 10/20', r: 1500, f: 300}, {d: '02/05', t: 'NLH 10/20', r: 1313.31, f: 322.27}] },
                        { name: "Camel63 (מאוחד)", pnl: -178.12, fee: 28.19, games: [{d: '01/05', t: 'NLH 5/10', r: -178.12, f: 28.19}] },
                        { name: "Puntman777", pnl: -100.00, fee: 26.10, games: [{d: '03/05', t: 'NLH 5/10', r: -100.00, f: 26.10}] },
                        { name: "naki t", pnl: 0.00, fee: 0.00, games: [] },
                        { name: "mirdi 86", pnl: 0.00, fee: 0.00, games: [] },
                        { name: "jokeravi", pnl: 0.00, fee: 0.00, games: [] }
                    ]
                },
                {
                    name: "יוני",
                    pastBalance: 0.00,
                    players: [
                        { name: "ghost baba", pnl: -26.68, fee: 2.81, games: [{d: '02/05', t: 'NLH 2/4', r: -26.68, f: 2.81}] },
                        { name: "Kepler36b", pnl: 0.00, fee: 0.00, games: [] },
                        { name: "levinson yoram", pnl: 0.00, fee: 0.00, games: [] }
                    ]
                }
            ]
        };

        let currentUser = null;
        let settlementChart = null, feeChart = null;
        const format = (v) => new Intl.NumberFormat('he-IL', { minimumFractionDigits: 2 }).format(v);

        function calculateAgentSummary(agent) {
            const sumPnl = agent.players.reduce((s, p) => s + p.pnl, 0);
            const sumFee100 = agent.players.reduce((s, p) => s + p.fee, 0);
            const rate = agent.name === "אבי" ? 0.8 : 0.6;
            const agentCut = sumFee100 * rate;
            const final = sumPnl + agentCut + agent.pastBalance;
            return { sumPnl, sumFee100, agentCut, final, rateText: agent.name === "אבי" ? "80%" : "60%" };
        }

        function handleLogin() {
            const u = document.getElementById('username').value.trim();
            const p = document.getElementById('password').value;
            if (users[u] && users[u].pass === p) {
                currentUser = users[u];
                document.getElementById('login-screen').classList.add('hidden');
                document.getElementById('app-content').classList.remove('hidden');
                setupView();
            } else {
                document.getElementById('login-error').classList.remove('hidden');
            }
        }

        function setupView() {
            document.getElementById('user-badge').textContent = currentUser.name;
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
            tabs.forEach(tabId => document.getElementById(tabId).onclick = () => switchTab(tabId, 'view-' + tabId.split('-')[1]));
            document.getElementById('logout-btn').onclick = () => window.location.reload();
            document.getElementById('close-modal').onclick = closeModal;
            document.getElementById('player-modal').onclick = (e) => e.target.id === 'player-modal' && closeModal();
            
            const selector = document.getElementById('agent-selector');
            selector.innerHTML = '<option value="">בחר סוכן...</option>';
            reportData.agents.forEach((a, i) => selector.innerHTML += `<option value="${i}">${a.name}</option>`);
            selector.onchange = (e) => e.target.value !== "" && renderAgentDetails(reportData.agents[e.target.value]);
        }

        function switchTab(activeBtnId, viewId) {
            document.querySelectorAll('.view-section').forEach(v => v.classList.add('hidden'));
            document.getElementById(viewId).classList.remove('hidden');
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active', 'bg-blue-600', 'text-white'));
            document.getElementById(activeBtnId).classList.add('active', 'bg-blue-600', 'text-white');
            if(viewId === 'view-dashboard') renderDashboard();
            if(viewId === 'view-mtt') renderMTT();
        }

        function renderDashboard() {
            const summaries = reportData.agents.map(a => calculateAgentSummary(a));
            const totalFees100 = summaries.reduce((s, x) => s + x.sumFee100, 0);
            const netBalance = summaries.reduce((s, x) => s + x.final, 0);

            document.getElementById('dash-stats').innerHTML = `
                <div class="bg-white p-6 rounded-xl border shadow-sm"><span class="text-xs text-slate-500 font-bold uppercase tracking-widest">סוכנים פעילים</span><div class="text-3xl font-bold text-slate-800">${reportData.agents.length}</div></div>
                <div class="bg-white p-6 rounded-xl border shadow-sm"><span class="text-xs text-slate-500 font-bold uppercase tracking-widest">סה"כ עמלות ברוטו</span><div class="text-3xl font-bold text-blue-600">${format(totalFees100)}</div></div>
                <div class="bg-white p-6 rounded-xl border shadow-sm"><span class="text-xs text-slate-500 font-bold uppercase tracking-widest">מאזן מועדון סופי</span><div class="text-3xl font-bold ${netBalance >=0 ? 'text-emerald-600' : 'text-red-600'}">${format(netBalance)}</div></div>
            `;

            if(settlementChart) settlementChart.destroy();
            settlementChart = new Chart(document.getElementById('settlementChart'), {
                type: 'bar',
                data: { labels: reportData.agents.map(a => a.name), datasets: [{ data: summaries.map(x => x.final), backgroundColor: summaries.map(x => x.final >= 0 ? '#10b981' : '#f43f5e'), borderRadius: 6 }] },
                options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } } }
            });

            if(feeChart) feeChart.destroy();
            feeChart = new Chart(document.getElementById('feeDistributionChart'), {
                type: 'doughnut',
                data: { labels: reportData.agents.map(a => a.name), datasets: [{ data: summaries.map(x => x.sumFee100), backgroundColor: ['#3b82f6', '#10b981', '#f59e0b', '#ef4444', '#8b5cf6', '#6366f1'] }] },
                options: { responsive: true, maintainAspectRatio: false }
            });
        }

        function renderAgentDetails(agent) {
            const summary = calculateAgentSummary(agent);
            const isPos = summary.final >= 0;
            const statusText = isPos ? "מועדון חייב לסוכן" : "סוכן חייב למועדון";
            
            document.getElementById('agent-metrics').innerHTML = `
                <div class="bg-white p-4 rounded-lg border shadow-sm"><span class="text-[10px] font-bold text-slate-500 uppercase">סה"כ P&L שחקנים</span><div class="text-xl font-bold ${summary.sumPnl >= 0 ? 'text-emerald-600' : 'text-red-600'}">${format(summary.sumPnl)}</div></div>
                <div class="bg-white p-4 rounded-lg border shadow-sm"><span class="text-[10px] font-bold text-slate-500 uppercase">עמלת סוכן (${summary.rateText})</span><div class="text-xl font-bold text-blue-600">${format(summary.agentCut)}</div></div>
                <div class="bg-white p-4 rounded-lg border shadow-sm"><span class="text-[10px] font-bold text-slate-500 uppercase">יתרת עבר</span><div class="text-xl font-bold text-slate-700">${format(agent.pastBalance)}</div></div>
                <div class="bg-white p-4 rounded-lg border shadow-sm"><span class="text-[10px] font-bold text-slate-500 uppercase">שחקנים רשומים</span><div class="text-xl font-bold text-slate-800">${agent.players.length}</div></div>
                <div class="${isPos ? 'bg-emerald-50 border-emerald-200' : 'bg-red-50 border-red-200'} p-4 rounded-lg border shadow-sm col-span-full md:col-span-1"><span class="text-[10px] font-bold ${isPos ? 'text-emerald-700' : 'text-red-700'} uppercase">שורה תחתונה (${statusText})</span><div class="text-2xl font-black ${isPos ? 'text-emerald-600' : 'text-red-600'}">${format(summary.final)}</div></div>
            `;

            const tbody = document.getElementById('agent-players-table');
            const sortedPlayers = [...agent.players].sort((a,b) => (Math.abs(b.pnl) + b.fee) - (Math.abs(a.pnl) + a.fee));
            
            tbody.innerHTML = sortedPlayers.map((p, pIdx) => `
                <tr class="player-row cursor-pointer hover:bg-slate-50 border-b border-slate-100 transition-colors" onclick="openPlayerDetails('${agent.name}', '${p.name}')">
                    <td class="p-4 font-medium text-slate-800 player-name transition-colors">${p.name}</td>
                    <td class="p-4 font-bold text-left ${p.pnl > 0 ? 'text-emerald-600' : p.pnl < 0 ? 'text-red-600' : 'text-slate-400'}" dir="ltr">${format(p.pnl)}</td>
                    <td class="p-4 text-left text-slate-600" dir="ltr">${format(p.fee)}</td>
                </tr>
            `).join('');
        }

        function openPlayerDetails(agentName, playerName) {
            const agent = reportData.agents.find(a => a.name === agentName);
            const player = agent.players.find(p => p.name === playerName);
            
            document.getElementById('modal-player-name').textContent = player.name;
            document.getElementById('modal-total-pnl').textContent = format(player.pnl);
            document.getElementById('modal-total-pnl').className = `text-2xl font-black ${player.pnl >= 0 ? 'text-emerald-600' : 'text-red-600'}`;
            document.getElementById('modal-total-fee').textContent = format(player.fee);
            
            const gamesBody = document.getElementById('modal-games-body');
            if (player.games && player.games.length > 0) {
                gamesBody.innerHTML = player.games.map(g => `
                    <tr class="hover:bg-slate-50">
                        <td class="py-3 px-2 text-sm">${g.d}</td>
                        <td class="py-3 px-2 text-sm font-medium text-slate-500">${g.t}</td>
                        <td class="py-3 px-2 text-sm font-bold text-left ${g.r >= 0 ? 'text-emerald-600' : 'text-red-600'}" dir="ltr">${format(g.r)}</td>
                        <td class="py-3 px-2 text-sm text-left font-medium text-slate-400" dir="ltr">${format(g.f)}</td>
                    </tr>
                `).join('');
            } else {
                gamesBody.innerHTML = `<tr><td colspan="4" class="py-12 text-center text-slate-400 italic">אין מידע מפורט זמין עבור שחקן זה</td></tr>`;
            }

            document.getElementById('player-modal').classList.remove('hidden');
            document.body.style.overflow = 'hidden';
        }

        function closeModal() {
            document.getElementById('player-modal').classList.add('hidden');
            document.body.style.overflow = 'auto';
        }

        function renderMTT() {
            document.getElementById('mtt-summary').innerHTML = `
                <div class="bg-white p-8 rounded-xl border border-red-100 border-t-4 border-t-red-500 shadow-sm">
                    <h3 class="text-xl font-bold text-slate-800 mb-6 text-center uppercase tracking-tighter">זליגת כספים בטורנירים</h3>
                    <div class="space-y-4">
                        <div class="flex justify-between p-3 bg-slate-50 rounded"><span>Overlay (GTD)</span><span class="font-bold text-red-600">-1,920.00</span></div>
                        <div class="flex justify-between p-3 bg-slate-50 rounded"><span>הפסד שחקני בית</span><span class="font-bold text-red-600">-236.03</span></div>
                        <div class="flex justify-between p-4 bg-red-600 text-white rounded-lg shadow mt-6"><span class="font-bold uppercase tracking-widest">עלות מועדון כוללת</span><span class="text-2xl font-black">-2,156.03</span></div>
                    </div>
                </div>
                <div class="bg-white p-8 rounded-xl border shadow-sm">
                    <h3 class="text-xl font-bold text-slate-800 mb-4 border-b pb-2 tracking-tighter uppercase font-black">ניתוח והמלצות</h3>
                    <p class="text-slate-600 leading-relaxed mb-4">המערכת מעידה כי הטורנירים עולים למועדון מעל ל-2,100 יחידות מדי שבוע. שחקני הבית שתומכים בנזילות המשחקים מסיימים בהפסד מול שחקנים זרים.</p>
                    <div class="mt-4 p-4 bg-blue-50 border-r-4 border-blue-500 rounded text-blue-900 font-medium italic text-sm text-center">מומלץ להקטין את סכומי ה-GTD בטורנירים שאינם מתמלאים.</div>
                </div>
            `;
        }

        document.getElementById('login-btn').onclick = handleLogin;
        document.getElementById('password').onkeypress = (e) => e.key === 'Enter' ? handleLogin() : null;
    </script>
</body>
</html>
