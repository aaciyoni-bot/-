<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>מערכת התחשבנות - הודעות ובונוסים</title>
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
        .announcement-card { transition: transform 0.2s; }
        .announcement-card:hover { transform: translateY(-2px); }
    </style>
</head>
<body class="min-h-screen flex flex-col text-right">

    <!-- Login Screen -->
    <div id="login-screen" class="fixed inset-0 z-50 flex items-center justify-center p-4" style="background-color: #0f172a;">
        <div class="bg-white p-8 rounded-2xl shadow-2xl w-full max-w-md border border-slate-200">
            <div class="text-center mb-8">
                <div class="inline-flex items-center justify-center w-16 h-16 bg-blue-100 text-blue-600 rounded-full mb-4 font-bold text-2xl">₪</div>
                <h2 class="text-2xl font-bold text-slate-800 font-black tracking-tighter">מערכת התחשבנות</h2>
                <p class="text-slate-500 mt-2 font-medium">ניהול מחזורים, הודעות ובונוסים</p>
            </div>
            <div class="space-y-4">
                <div>
                    <label class="block text-sm font-medium text-slate-700 mb-1 text-right">שם משתמש</label>
                    <input type="text" id="username" class="w-full p-3 border border-slate-300 rounded-lg focus:ring-2 focus:ring-blue-500 outline-none text-right" placeholder="הזן שם משתמש">
                </div>
                <div>
                    <label class="block text-sm font-medium text-slate-700 mb-1 text-right">סיסמה</label>
                    <input type="password" id="password" class="w-full p-3 border border-slate-300 rounded-lg focus:ring-2 focus:ring-blue-500 outline-none text-right" placeholder="••••••••">
                </div>
                <div id="login-error" class="text-red-500 text-sm hidden text-center font-bold">פרטי התחברות שגויים</div>
                <button id="login-btn" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 rounded-lg transition-colors mt-4 shadow-lg shadow-blue-200 uppercase tracking-widest">התחבר</button>
            </div>
        </div>
    </div>

    <!-- Player Details Modal -->
    <div id="player-modal" class="fixed inset-0 z-[60] hidden flex items-center justify-center p-4 modal-overlay">
        <div class="bg-white rounded-2xl shadow-2xl w-full max-w-2xl max-h-[90vh] overflow-hidden flex flex-col">
            <div class="p-6 border-b border-slate-100 flex justify-between items-center bg-slate-50">
                <div>
                    <h3 id="modal-player-name" class="text-2xl font-bold text-slate-800 font-black tracking-tighter text-right">שם שחקן</h3>
                    <p class="text-sm text-slate-500 mt-1 font-medium italic text-right">פירוט סשנים וחישוב תוצאה</p>
                </div>
                <button id="close-modal" class="text-slate-400 hover:text-slate-600 p-2 text-3xl font-light">&times;</button>
            </div>
            <div class="p-6 overflow-y-auto flex-grow text-right">
                <div id="modal-creds-section" class="mb-6 p-5 bg-emerald-50 border border-emerald-100 rounded-2xl hidden shadow-inner">
                    <h4 class="text-[11px] font-black text-emerald-700 uppercase tracking-widest mb-4 flex items-center gap-2">🔑 פרטי גישה לשחקן</h4>
                    <div class="flex flex-col sm:flex-row gap-4">
                        <div class="flex-1 bg-white p-3 rounded-xl border border-emerald-200">
                            <span class="text-[9px] text-emerald-500 font-bold block uppercase mb-1">שם משתמש:</span>
                            <code id="modal-display-username" class="text-lg font-black text-slate-800 tracking-tight block">---</code>
                        </div>
                        <div class="flex-1 bg-white p-3 rounded-xl border border-emerald-200">
                            <span class="text-[9px] text-emerald-500 font-bold block uppercase mb-1">סיסמה:</span>
                            <code id="modal-display-password" class="text-lg font-black text-slate-800 tracking-tight block">---</code>
                        </div>
                    </div>
                </div>
                <div class="grid grid-cols-2 gap-4 mb-6">
                    <div class="bg-blue-50 p-4 rounded-xl border border-blue-100 shadow-sm">
                        <span class="text-[10px] text-blue-600 font-bold uppercase tracking-widest block mb-1">סה"כ P&L</span>
                        <div id="modal-total-pnl" class="text-2xl font-black tracking-tighter">0.00</div>
                    </div>
                    <div id="modal-fee-card" class="bg-slate-50 p-4 rounded-xl border border-slate-100 shadow-sm">
                        <span class="text-[10px] text-slate-500 font-bold uppercase tracking-widest block mb-1">עמלה נטו</span>
                        <div id="modal-total-fee" class="text-2xl font-black tracking-tighter">0.00</div>
                    </div>
                </div>
                <h4 class="font-black text-slate-700 mb-3 text-xs uppercase tracking-widest border-r-4 border-slate-200 pr-2">היסטוריית משחקים וטורנירים:</h4>
                <div class="overflow-x-auto">
                    <table class="w-full border-collapse">
                        <thead>
                            <tr class="text-[10px] font-black text-slate-400 uppercase border-b">
                                <th class="pb-3 px-2 text-right">סוג/תאריך</th>
                                <th class="pb-3 px-2 text-right">משחק</th>
                                <th class="pb-3 px-2 text-left">תוצאה</th>
                                <th class="pb-3 px-2 text-left fee-col">עמלה</th>
                            </tr>
                        </thead>
                        <tbody id="modal-games-body" class="divide-y text-slate-700"></tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <!-- Application Content -->
    <div id="app-content" class="hidden flex flex-col min-h-screen">
        <header class="bg-white shadow-sm border-b border-slate-200 py-4 sticky top-0 z-40">
            <div class="container mx-auto px-4 lg:px-8 max-w-7xl text-right">
                <div class="flex flex-col md:flex-row justify-between items-center gap-4">
                    <div class="flex items-center gap-3">
                        <h1 class="text-2xl font-black text-slate-800 tracking-tighter uppercase">💼 התחשבנות</h1>
                        <span id="user-badge" class="bg-blue-50 text-blue-700 px-3 py-1 rounded-full text-[10px] font-black border border-blue-100 uppercase tracking-widest"></span>
                    </div>
                    
                    <div class="flex items-center gap-2 bg-slate-100 p-1 rounded-xl border border-slate-200">
                        <label class="text-[9px] font-black text-slate-400 uppercase px-2">מחזור:</label>
                        <select id="cycle-selector" class="bg-white border-0 text-slate-800 text-xs font-bold rounded-lg px-3 py-1.5 focus:ring-2 focus:ring-blue-500 outline-none shadow-sm cursor-pointer"></select>
                    </div>

                    <nav class="flex gap-2 items-center">
                        <div id="main-nav" class="flex gap-2 mr-4">
                            <button id="nav-dashboard" class="nav-btn active px-4 py-2 rounded-lg border border-slate-200 text-slate-700 font-bold text-sm whitespace-nowrap">📊 דאשבורד</button>
                            <button id="nav-agents" class="nav-btn px-4 py-2 rounded-lg border border-slate-200 text-slate-700 font-bold text-sm whitespace-nowrap">👥 דוחות</button>
                            <button id="nav-mtt" class="nav-btn px-4 py-2 rounded-lg border border-slate-200 text-slate-700 font-bold text-sm whitespace-nowrap">🏆 טורנירים</button>
                        </div>
                        <button id="logout-btn" class="bg-slate-50 text-slate-500 hover:text-red-600 px-4 py-2 rounded-lg border border-slate-200 font-black text-xs transition-all">🚪 יציאה</button>
                    </nav>
                </div>
            </div>
        </header>

        <main class="flex-grow container mx-auto px-4 lg:px-8 max-w-7xl py-8">
            
            <div id="view-dashboard" class="view-section block">
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                    <!-- Admin Left Column: Stats & Charts -->
                    <div class="lg:col-span-2">
                        <div class="mb-8 text-right">
                            <h2 class="text-3xl font-black text-slate-800 mb-2 tracking-tight">תמונת מצב מועדון</h2>
                            <p class="text-slate-600 font-medium">סיכום התחשבנות גלובלי למחזור הנבחר.</p>
                        </div>
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8" id="dash-stats"></div>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                            <div class="bg-white rounded-3xl shadow-sm border border-slate-200 p-6">
                                <h3 class="text-lg font-black text-slate-800 mb-4 border-b pb-2 uppercase tracking-tighter text-right">מאזן סופי לפי סוכן</h3>
                                <div class="chart-container"><canvas id="settlementChart"></canvas></div>
                            </div>
                            <div class="bg-white rounded-3xl shadow-sm border border-slate-200 p-6">
                                <h3 class="text-lg font-black text-slate-800 mb-4 border-b pb-2 uppercase tracking-tighter text-right">פילוח עמלות</h3>
                                <div class="chart-container"><canvas id="feeDistributionChart"></canvas></div>
                            </div>
                        </div>
                    </div>

                    <!-- Admin Right Column: Message Management -->
                    <div class="lg:col-span-1">
                        <div class="bg-slate-800 text-white rounded-[2rem] p-6 shadow-xl sticky top-24">
                            <h3 class="text-xl font-black mb-4 flex items-center gap-2 justify-end">📢 שלח הודעה לשחקנים <span class="text-blue-400">●</span></h3>
                            <div class="space-y-4">
                                <input type="text" id="admin-msg-title" class="w-full bg-slate-700 border-0 rounded-xl p-3 text-white placeholder-slate-400 text-sm font-bold text-right" placeholder="כותרת ההודעה...">
                                <textarea id="admin-msg-content" rows="4" class="w-full bg-slate-700 border-0 rounded-xl p-3 text-white placeholder-slate-400 text-sm font-medium text-right" placeholder="תוכן ההודעה או הבונוס..."></textarea>
                                <div class="flex gap-2">
                                    <button id="admin-send-bonus" class="flex-1 bg-emerald-600 hover:bg-emerald-500 py-3 rounded-xl font-black text-xs uppercase tracking-widest transition-colors">🎁 בונוס</button>
                                    <button id="admin-send-msg" class="flex-1 bg-blue-600 hover:bg-blue-500 py-3 rounded-xl font-black text-xs uppercase tracking-widest transition-colors">🚀 הודעה כללית</button>
                                </div>
                            </div>
                            <div class="mt-8 border-t border-slate-700 pt-6">
                                <h4 class="text-xs font-black text-slate-400 uppercase tracking-widest mb-4 text-right">הודעות אחרונות</h4>
                                <div id="admin-recent-msgs" class="space-y-3"></div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div id="view-agents" class="view-section hidden text-right">
                <div id="agent-selector-container" class="bg-white rounded-3xl shadow-sm border border-slate-200 p-6 mb-8">
                    <label class="block text-[11px] font-black text-slate-500 mb-2 uppercase tracking-widest">בחר סוכן להצגה:</label>
                    <select id="agent-selector" class="w-full md:w-1/3 bg-slate-50 border border-slate-300 text-slate-900 text-lg font-bold rounded-xl p-3 outline-none shadow-inner text-right"></select>
                </div>

                <div id="agent-details-container">
                    <div class="grid grid-cols-2 md:grid-cols-5 gap-4 mb-8" id="agent-metrics"></div>
                    <div class="bg-white rounded-3xl shadow-sm border border-slate-200 p-6">
                        <h3 class="text-lg font-black text-slate-800 mb-2 border-b border-slate-100 pb-2 uppercase tracking-tighter">פירוט שחקנים</h3>
                        <p class="text-xs text-slate-400 mb-4 font-medium italic tracking-tight uppercase tracking-tighter">* לחץ על שם שחקן לצפייה בפרטי גישה ובפירוט המשחקים.</p>
                        <div class="overflow-x-auto">
                            <table class="w-full border-collapse">
                                <thead class="bg-slate-50 text-slate-400 text-[10px] font-black uppercase tracking-widest">
                                    <tr class="text-right">
                                        <th class="p-4 border-b">שם שחקן</th>
                                        <th class="p-4 border-b text-left">תוצאה (P&L)</th>
                                        <th class="p-4 border-b text-left fee-col">עמלה</th>
                                    </tr>
                                </thead>
                                <tbody id="agent-players-table" class="divide-y text-slate-700 font-medium"></tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>

            <div id="view-player-single" class="view-section hidden text-right">
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-8 max-w-7xl mx-auto">
                    <!-- Left: Player Stats -->
                    <div class="lg:col-span-2">
                        <div class="bg-white rounded-[2rem] shadow-xl border border-slate-200 overflow-hidden mb-8">
                            <div class="bg-slate-900 text-white p-8 sm:p-12">
                                <h2 class="text-4xl font-black mb-2 tracking-tighter uppercase" id="player-view-name">שלום</h2>
                                <p class="opacity-50 font-bold uppercase tracking-widest text-xs tracking-widest">סיכום ביצועים אישי מבוקר</p>
                            </div>
                            <div class="p-8 sm:p-12">
                                <div class="bg-slate-50 p-8 rounded-3xl border border-slate-100 flex justify-between items-center mb-10 shadow-inner">
                                    <div class="text-right">
                                        <span class="text-[10px] font-black text-slate-400 uppercase tracking-widest block mb-1">תוצאה סופית (P&L)</span>
                                        <div id="player-view-pnl" class="text-5xl font-black tracking-tighter">0.00</div>
                                    </div>
                                    <div class="bg-white w-20 h-20 rounded-3xl shadow-sm flex items-center justify-center text-4xl">💹</div>
                                </div>
                                <h3 class="text-xl font-black text-slate-800 mb-6 border-b pb-2 uppercase tracking-tighter font-black">פירוט משחקים</h3>
                                <div class="overflow-x-auto">
                                    <table class="w-full">
                                        <thead>
                                            <tr class="text-[10px] font-black text-slate-400 uppercase border-b text-right">
                                                <th class="pb-3 px-2">תאריך</th>
                                                <th class="pb-3 px-2">סוג משחק</th>
                                                <th class="pb-3 px-2 text-left">תוצאה</th>
                                            </tr>
                                        </thead>
                                        <tbody id="player-view-table" class="divide-y text-slate-700 font-medium"></tbody>
                                    </table>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Right: Messages & Bonuses -->
                    <div class="lg:col-span-1">
                        <h3 class="text-xl font-black text-slate-800 mb-6 flex items-center gap-2 justify-end">הודעות ובונוסים <span class="text-blue-600">●</span></h3>
                        <div id="player-messages-list" class="space-y-4"></div>
                    </div>
                </div>
            </div>

            <div id="view-mtt" class="view-section hidden text-right">
                <div class="mb-8"><h2 class="text-3xl font-black text-slate-800 font-black tracking-tighter uppercase">ניתוח טורנירים (MTT)</h2></div>
                <div id="mtt-summary" class="grid grid-cols-1 md:grid-cols-2 gap-8"></div>
            </div>
        </main>
    </div>

    <script>
        const users = {
            'admin': { pass: 'admin123', role: 'admin', name: 'מנהל מערכת' },
            'חיים': { pass: 'חיים177', role: 'agent', name: 'חיים', agentIndex: 0 },
            'איתי': { pass: 'איתי2024', role: 'agent', name: 'איתי', agentIndex: 1 },
            'אבי': { pass: 'אבי2026', role: 'agent', name: 'אבי', agentIndex: 2 },
            'עוז': { pass: 'עוז999', role: 'agent', name: 'עוז', agentIndex: 3 },
            'אלחנן': { pass: 'אלחנן86', role: 'agent', name: 'אלחנן', agentIndex: 4 },
            'יוני': { pass: 'יוני25', role: 'agent', name: 'יוני', agentIndex: 5 },
            'בליינדרס': { pass: 'blinders7', role: 'agent', name: 'בליינדרס', agentIndex: 6 },
            'avim24': { pass: 'avim123', role: 'player', name: 'avim24' },
            'Bathens': { pass: 'bat123', role: 'player', name: 'Bathens' },
            'מור קריטי': { pass: 'מור123', role: 'player', name: 'מור קריטי' },
            'Black Rain82': { pass: 'black123', role: 'player', name: 'Black Rain82' },
            'OTC 1': { pass: 'otc123', role: 'player', name: 'OTC 1' },
            'IzMaR': { pass: 'izmar123', role: 'player', name: 'IzMaR' },
            'dipstay': { pass: 'dip123', role: 'player', name: 'dipstay' },
            'in2024': { pass: 'in2024in', role: 'player', name: 'in2024' },
            'Aviad1111': { pass: 'avi1111', role: 'player', name: 'Aviad1111' }
        };

        // Announcements state (Mock persistent storage)
        const announcements = [
            { type: 'msg', title: 'ברוכים הבאים למחזור החדש!', content: 'התחלנו מחזור חדש (05/05). בהצלחה לכולם בשולחנות!', date: '05/05/2026' },
            { type: 'bonus', title: 'בונוס הצטרפות', content: 'חולקו בונוסים לכל השחקנים החדשים שהצטרפו השבוע.', date: '04/05/2026' }
        ];

        const cycles = [
            {
                id: "current",
                label: "מחזור נוכחי (05/05 - 12/05)",
                agents: [
                    { name: "חיים", pastBalance: 0.00, players: [] },
                    { name: "איתי", pastBalance: 0.00, players: [] },
                    { name: "אבי", pastBalance: 0.00, players: [] },
                    { name: "עוז", pastBalance: 0.00, players: [] },
                    { name: "אלחנן", pastBalance: 0.00, players: [] },
                    { name: "יוני", pastBalance: 0.00, players: [] },
                    { name: "בליינדרס", pastBalance: 0.00, players: [] }
                ],
                mtt: { overlay: 0, internalLoss: 0 }
            },
            {
                id: "prev_cycle_1",
                label: "מחזור קודם (28/04 - 05/05)",
                mtt: { overlay: -1920.00, internalLoss: -236.03 },
                agents: [
                    {
                        name: "חיים", pastBalance: 0.00,
                        players: [
                            { name: "avim24", pnl: -1055.78, fee: 1008.05, games: [{d: '29/04', t: 'NLH 5/10', r: -1055.78, f: 1008.05}] },
                            { name: "Bathens", pnl: -5536.39, fee: 1145.82, games: [{d: '28/04', t: 'PLO 25/50', r: -3000, f: 600.50}, {d: '01/05', t: 'PLO 25/50', r: -2536.39, f: 545.32}] },
                            { name: "dipstay", pnl: -1240.00, fee: 121.49, games: [{d: '30/04', t: 'PLO 10/20', r: -1440.00, f: 121.49}, {d: '05/05', t: 'בונוס מתנה', r: 200.00, f: 0.00}] }
                        ]
                    },
                    {
                        name: "איתי", pastBalance: 0.00,
                        players: [
                            { name: "OTC 1", pnl: 126.70, fee: 173.00, games: [{d: '01/05', t: 'NLH 5/10', r: -53.34, f: 173.00}, {d: 'MTT', t: 'רווח טורניר', r: 180.04, f: 0.00}] },
                            { name: "Aviad1111", pnl: -3632.81, fee: 936.15, games: [{d: '28/04', t: 'PLO 25/50', r: -2000, f: 500}, {d: '30/04', t: 'NLH 10/20', r: -1632.81, f: 436.15}] }
                        ]
                    },
                    {
                        name: "עוז", pastBalance: -2550.00,
                        players: [
                            { name: "adirmezin12", pnl: -697.36, fee: 415.74, games: [{d: '30/04', t: 'PLO 5/10', r: -697.36, f: 415.74}] }
                        ]
                    },
                    {
                        name: "אלחנן", pastBalance: -2000.00,
                        players: [
                            { name: "IzMaR", pnl: 713.31, fee: 622.27, games: [{d: '05/05', t: 'חוב עבר', r: -2300.00, f: 0.00}, {d: '05/05', t: 'בונוס מתנה', r: 200.00, f: 0.00}, {d: '02/05', t: 'NLH 10/20', r: 1313.31, f: 322.27}] }
                        ]
                    }
                ]
            }
        ];

        let currentUser = null;
        let selectedCycleIndex = 0;
        let settlementChart = null, feeChart = null;
        const format = (v) => new Intl.NumberFormat('he-IL', { minimumFractionDigits: 2 }).format(v);
        const getAgentPersonalRate = (n) => (n === "אבי" ? 0.8 : (n === "עוז" ? 0.4 : 0.6));

        function calculateAgentSummary(agent, cycleAgents) {
            const sumPnl = agent.players.reduce((s, p) => s + p.pnl, 0);
            const rawFees = agent.players.reduce((s, p) => s + p.fee, 0);
            const personalRate = getAgentPersonalRate(agent.name);
            let agentCut = rawFees * personalRate;
            let referralCut = 0;
            if (agent.name === "חיים") {
                const oz = cycleAgents.find(a => a.name === "עוז");
                if (oz) referralCut = oz.players.reduce((s, p) => s + p.fee, 0) * 0.3;
            }
            return { sumPnl, agentCut, referralCut, final: sumPnl + agentCut + referralCut + agent.pastBalance, rawFees };
        }

        function handleLogin() {
            const u = document.getElementById('username').value.trim();
            const p = document.getElementById('password').value;
            if (users[u] && users[u].pass === p) {
                currentUser = users[u];
                document.getElementById('login-screen').classList.add('hidden');
                document.getElementById('app-content').classList.remove('hidden');
                renderCycleSelector();
                setupView();
            } else {
                document.getElementById('login-error').classList.remove('hidden');
            }
        }

        function renderCycleSelector() {
            const selector = document.getElementById('cycle-selector');
            selector.innerHTML = cycles.map((c, i) => `<option value="${i}">${c.label}</option>`).join('');
            selector.onchange = (e) => { selectedCycleIndex = Number(e.target.value); setupView(); };
        }

        function setupView() {
            document.getElementById('user-badge').textContent = currentUser.name;
            const currentCycle = cycles[selectedCycleIndex];
            if (currentUser.role === 'admin') {
                document.getElementById('main-nav').classList.remove('hidden');
                switchTab('nav-dashboard', 'view-dashboard');
                renderAdminAnnouncements();
            } else if (currentUser.role === 'agent') {
                document.getElementById('main-nav').classList.remove('hidden');
                document.getElementById('nav-dashboard').classList.add('hidden');
                document.getElementById('nav-mtt').classList.add('hidden');
                document.getElementById('agent-selector-container').classList.add('hidden');
                switchTab('nav-agents', 'view-agents');
                renderAgentDetails(currentCycle.agents.find(a => a.name === currentUser.name));
            } else if (currentUser.role === 'player') {
                document.getElementById('main-nav').classList.add('hidden');
                switchTab(null, 'view-player-single');
                renderPlayerSingleView(currentUser.name);
                renderPlayerAnnouncements();
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
            cycles[selectedCycleIndex].agents.forEach((a, i) => selector.innerHTML += `<option value="${i}">${a.name}</option>`);
            selector.onchange = (e) => e.target.value !== "" && renderAgentDetails(cycles[selectedCycleIndex].agents[e.target.value]);
        }

        function switchTab(activeBtnId, viewId) {
            document.querySelectorAll('.view-section').forEach(v => v.classList.add('hidden'));
            document.getElementById(viewId).classList.remove('hidden');
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active', 'bg-blue-600', 'text-white'));
            if(activeBtnId) document.getElementById(activeBtnId).classList.add('active', 'bg-blue-600', 'text-white');
            if(viewId === 'view-dashboard') renderDashboard();
            if(viewId === 'view-mtt') renderMTT();
        }

        function renderDashboard() {
            const summaries = cycles[selectedCycleIndex].agents.map(a => calculateAgentSummary(a, cycles[selectedCycleIndex].agents));
            document.getElementById('dash-stats').innerHTML = `
                <div class="bg-white p-6 rounded-3xl border shadow-sm text-right"><span class="text-[10px] text-slate-400 font-black uppercase tracking-widest block mb-1">סוכנים פעילים</span><div class="text-3xl font-black text-slate-800">${cycles[selectedCycleIndex].agents.length}</div></div>
                <div class="bg-white p-6 rounded-3xl border shadow-sm text-right"><span class="text-[10px] text-slate-400 font-black uppercase tracking-widest block mb-1">סה"כ עמלות (ברוטו)</span><div class="text-3xl font-black text-blue-600">${format(summaries.reduce((s, x) => s + x.rawFees, 0))}</div></div>
                <div class="bg-white p-6 rounded-3xl border shadow-sm text-right"><span class="text-[10px] text-slate-400 font-black uppercase tracking-widest block mb-1 uppercase">מאזן מועדון</span><div class="text-3xl font-black ${summaries.reduce((s, x) => s + x.final, 0) >=0 ? 'text-emerald-600' : 'text-red-600'}">${format(summaries.reduce((s, x) => s + x.final, 0))}</div></div>`;
            
            if(settlementChart) settlementChart.destroy();
            settlementChart = new Chart(document.getElementById('settlementChart'), {
                type: 'bar',
                data: { labels: cycles[selectedCycleIndex].agents.map(a => a.name), datasets: [{ data: summaries.map(x => x.final), backgroundColor: summaries.map(x => x.final >= 0 ? '#10b981' : '#f43f5e'), borderRadius: 6 }] },
                options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } } }
            });
        }

        // Announcement Logic
        function renderAdminAnnouncements() {
            document.getElementById('admin-recent-msgs').innerHTML = announcements.slice(-3).reverse().map(a => `
                <div class="bg-slate-700/50 p-3 rounded-xl border border-slate-600 text-right">
                    <div class="flex justify-between items-center mb-1">
                        <span class="text-[10px] text-slate-400 font-bold">${a.date}</span>
                        <span class="text-[9px] ${a.type==='bonus' ? 'text-emerald-400' : 'text-blue-400'} font-black uppercase uppercase tracking-tighter">${a.type==='bonus' ? '🎁 בונוס' : '📢 הודעה'}</span>
                    </div>
                    <div class="text-xs font-black text-white">${a.title}</div>
                </div>`).join('');
        }

        function addAnnouncement(type) {
            const title = document.getElementById('admin-msg-title').value;
            const content = document.getElementById('admin-msg-content').value;
            if(!title || !content) return;
            announcements.push({ type, title, content, date: new Date().toLocaleDateString('he-IL') });
            document.getElementById('admin-msg-title').value = '';
            document.getElementById('admin-msg-content').value = '';
            renderAdminAnnouncements();
        }

        function renderPlayerAnnouncements() {
            document.getElementById('player-messages-list').innerHTML = announcements.slice().reverse().map(a => `
                <div class="bg-white p-5 rounded-2xl shadow-sm border border-slate-200 text-right announcement-card">
                    <div class="flex justify-between items-center mb-2">
                        <span class="text-[10px] text-slate-400 font-bold">${a.date}</span>
                        <span class="px-2 py-0.5 rounded-full text-[9px] font-black uppercase ${a.type==='bonus' ? 'bg-emerald-100 text-emerald-700' : 'bg-blue-100 text-blue-700'}">${a.type==='bonus' ? 'בונוס' : 'הודעה'}</span>
                    </div>
                    <h4 class="text-sm font-black text-slate-800 mb-1">${a.title}</h4>
                    <p class="text-xs text-slate-600 font-medium leading-relaxed">${a.content}</p>
                </div>`).join('');
        }

        function renderAgentDetails(agent) {
            const summary = calculateAgentSummary(agent, cycles[selectedCycleIndex].agents);
            const rate = getAgentPersonalRate(agent.name);
            document.getElementById('agent-metrics').innerHTML = `
                <div class="bg-white p-4 rounded-xl border shadow-sm text-right"><span class="text-[9px] font-black text-slate-400 block mb-1">P&L שחקנים</span><div class="text-xl font-black ${summary.sumPnl >= 0 ? 'text-emerald-600' : 'text-red-600'}">${format(summary.sumPnl)}</div></div>
                <div class="bg-white p-4 rounded-xl border shadow-sm text-right"><span class="text-[9px] font-black text-slate-400 block mb-1">עמלת סוכן</span><div class="text-xl font-black text-blue-600">${format(summary.agentCut)}</div></div>
                ${agent.name === "חיים" ? `<div class="bg-blue-600 p-4 rounded-xl border border-blue-500 text-white text-right"><span class="text-[9px] font-black block mb-1 opacity-80 uppercase tracking-widest">עמלת רשת (עוז)</span><div class="text-xl font-black">${format(summary.referralCut)}</div></div>` : ''}
                <div class="bg-white p-4 rounded-xl border shadow-sm text-right"><span class="text-[9px] font-black text-slate-400 block mb-1">יתרת עבר</span><div class="text-xl font-black text-slate-700">${format(agent.pastBalance)}</div></div>
                <div class="${summary.final >= 0 ? 'bg-emerald-50 border-emerald-100' : 'bg-red-50 border-red-100'} p-4 rounded-xl border shadow-sm text-right"><span class="text-[9px] font-black uppercase block mb-1">שורה תחתונה</span><div class="text-2xl font-black ${summary.final >= 0 ? 'text-emerald-600' : 'text-red-600'}">${format(summary.final)}</div></div>`;
            
            const tbody = document.getElementById('agent-players-table');
            if (agent.players.length === 0) tbody.innerHTML = '<tr><td colspan="3" class="p-12 text-center text-slate-300 italic font-bold">אין פעילות במחזור זה</td></tr>';
            else tbody.innerHTML = [...agent.players].sort((a,b) => Math.abs(b.pnl) - Math.abs(a.pnl)).map(p => `
                <tr class="player-row cursor-pointer hover:bg-slate-50 border-b border-slate-100 transition-colors text-right" onclick="openPlayerDetails('${agent.name}', '${p.name}')">
                    <td class="p-4 font-bold text-slate-800 player-name text-sm">${p.name}</td>
                    <td class="p-4 font-black text-left ${p.pnl > 0 ? 'text-emerald-600' : p.pnl < 0 ? 'text-red-600' : 'text-slate-400'} text-sm" dir="ltr">${format(p.pnl)}</td>
                    <td class="p-4 text-left text-slate-600 fee-col font-bold text-sm" dir="ltr">${format(p.fee * rate)}</td>
                </tr>`).join('');
        }

        function renderPlayerSingleView(playerName) {
            let player = null;
            cycles[selectedCycleIndex].agents.forEach(a => { const found = a.players.find(p => p.name === playerName); if(found) player = found; });
            document.getElementById('player-view-name').textContent = `שלום, ${playerName}`;
            const pnlVal = document.getElementById('player-view-pnl');
            if(!player || player.games.length === 0) {
                pnlVal.textContent = "0.00"; pnlVal.className = "text-5xl font-black text-slate-300 tracking-tighter";
                document.getElementById('player-view-table').innerHTML = '<tr><td colspan="3" class="py-12 text-center text-slate-300 italic font-bold uppercase tracking-widest">אין פעילות רשומה</td></tr>';
            } else {
                pnlVal.textContent = format(player.pnl); pnlVal.className = `text-5xl font-black ${player.pnl >= 0 ? 'text-emerald-600' : 'text-red-600'} tracking-tighter`;
                document.getElementById('player-view-table').innerHTML = player.games.map(g => `
                    <tr class="hover:bg-slate-50 transition-colors text-right">
                        <td class="py-4 px-2 text-sm font-bold text-slate-400 uppercase tracking-tighter">${g.d}</td>
                        <td class="py-4 px-2 text-sm text-slate-700 font-black tracking-tight">${g.t}</td>
                        <td class="py-4 px-2 text-sm font-black text-left ${g.r >= 0 ? 'text-emerald-600' : 'text-red-600'}" dir="ltr">${format(g.r)}</td>
                    </tr>`).join('');
            }
        }

        function openPlayerDetails(agentName, playerName) {
            const agent = cycles[selectedCycleIndex].agents.find(a => a.name === agentName);
            const player = agent.players.find(p => p.name === playerName);
            const rate = getAgentPersonalRate(agent.name);
            document.getElementById('modal-player-name').textContent = playerName;
            document.getElementById('modal-total-pnl').textContent = format(player?.pnl || 0);
            document.getElementById('modal-total-pnl').className = `text-2xl font-black ${(player?.pnl || 0) >= 0 ? 'text-emerald-600' : 'text-red-600'}`;
            document.getElementById('modal-total-fee').textContent = format((player?.fee || 0) * rate);
            document.getElementById('modal-games-body').innerHTML = player?.games.map(g => `
                <tr class="hover:bg-slate-50 border-b border-slate-50 last:border-0 transition-colors text-right">
                    <td class="py-4 px-2 text-[11px] font-bold text-slate-400 uppercase">${g.d}</td>
                    <td class="py-4 px-2 text-sm font-black text-slate-700">${g.t}</td>
                    <td class="py-4 px-2 text-sm font-black text-left ${g.r >= 0 ? 'text-emerald-600' : 'text-red-600'}" dir="ltr">${format(g.r)}</td>
                    <td class="py-4 px-2 text-sm text-left font-bold text-slate-400 fee-col" dir="ltr">${format(g.f * rate)}</td>
                </tr>`).join('') || '<tr><td colspan="4" class="py-12 text-center text-slate-300">אין מידע זמין</td></tr>';
            
            const credsSection = document.getElementById('modal-creds-section');
            if(currentUser.role !== 'player' && users[playerName]) {
                credsSection.classList.remove('hidden');
                document.getElementById('modal-display-username').textContent = playerName;
                document.getElementById('modal-display-password').textContent = users[playerName].pass;
            } else credsSection.classList.add('hidden');
            document.getElementById('player-modal').classList.remove('hidden');
            document.body.style.overflow = 'hidden';
        }

        function closeModal() { document.getElementById('player-modal').classList.add('hidden'); document.body.style.overflow = 'auto'; }

        function renderMTT() {
            const mtt = cycles[selectedCycleIndex].mtt;
            document.getElementById('mtt-summary').innerHTML = `
                <div class="bg-white p-8 rounded-[2rem] border border-red-100 border-t-4 border-t-red-500 shadow-sm text-right">
                    <h3 class="text-xl font-black text-slate-800 mb-6 text-center uppercase tracking-tighter">זליגת כספים בטורנירים</h3>
                    <div class="space-y-4 font-bold">
                        <div class="flex justify-between p-3 bg-slate-50 rounded-2xl"><span>Overlay</span><span class="text-red-600">${format(mtt.overlay)}</span></div>
                        <div class="flex justify-between p-3 bg-slate-50 rounded-2xl"><span>הפסד שחקני בית</span><span class="text-red-600">${format(mtt.internalLoss)}</span></div>
                        <div class="flex justify-between p-5 bg-red-600 text-white rounded-[1.5rem] shadow mt-6 font-black uppercase tracking-widest"><span>סה"כ עלות מועדון</span><span class="text-2xl">${format(mtt.overlay + mtt.internalLoss)}</span></div>
                    </div>
                </div>`;
        }

        // Action Buttons
        document.getElementById('admin-send-msg').onclick = () => { addAnnouncement('msg'); renderPlayerAnnouncements(); };
        document.getElementById('admin-send-bonus').onclick = () => { addAnnouncement('bonus'); renderPlayerAnnouncements(); };
        document.getElementById('login-btn').onclick = handleLogin;
        document.getElementById('password').onkeypress = (e) => e.key === 'Enter' && handleLogin();
    </script>
</body>
</html>
