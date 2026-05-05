<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>מערכת התחשבנות - גרסה מתוקנת ומלאה</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    
    <style>
        body { font-family: system-ui, -apple-system, sans-serif; background-color: #f8fafc; color: #1e293b; }
        .chart-container { position: relative; width: 100%; max-width: 800px; margin: auto; height: 350px; }
        .nav-btn.active { background-color: #2563eb; color: white; border-color: #2563eb; }
        .nav-btn { transition: all 0.2s ease; cursor: pointer; }
        .modal-overlay { background-color: rgba(15, 23, 42, 0.7); backdrop-filter: blur(4px); }
        .player-row:hover .player-name { color: #2563eb; text-decoration: underline; }
        input, select { direction: rtl; }
    </style>
</head>
<body class="min-h-screen flex flex-col text-right">

    <!-- Login Screen -->
    <div id="login-screen" class="fixed inset-0 z-50 flex items-center justify-center p-4" style="background-color: #0f172a;">
        <div class="bg-white p-8 rounded-2xl shadow-2xl w-full max-w-md border border-slate-200">
            <div class="text-center mb-8">
                <div class="inline-flex items-center justify-center w-16 h-16 bg-blue-100 text-blue-600 rounded-full mb-4 font-bold text-2xl">₪</div>
                <h2 class="text-2xl font-bold text-slate-800 font-black tracking-tighter uppercase">כניסת מערכת</h2>
                <p class="text-slate-500 mt-2 font-medium">התחשבנות מבוקרת - גישה מאובטחת</p>
            </div>
            <div class="space-y-4 text-right">
                <div>
                    <label class="block text-sm font-bold text-slate-700 mb-1">שם משתמש</label>
                    <input type="text" id="username" class="w-full p-3 border border-slate-300 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none text-right" placeholder="הזן שם משתמש">
                </div>
                <div>
                    <label class="block text-sm font-bold text-slate-700 mb-1">סיסמה</label>
                    <input type="password" id="password" class="w-full p-3 border border-slate-300 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none text-right" placeholder="••••••••">
                </div>
                <div id="login-error" class="text-red-600 text-sm hidden text-center font-black bg-red-50 p-2 rounded-lg border border-red-100 italic">פרטי התחברות שגויים</div>
                <button id="login-btn" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-black py-4 rounded-xl transition-all shadow-lg shadow-blue-200 uppercase tracking-widest">התחבר</button>
            </div>
        </div>
    </div>

    <!-- Player Details Modal -->
    <div id="player-modal" class="fixed inset-0 z-[60] hidden flex items-center justify-center p-4 modal-overlay">
        <div class="bg-white rounded-3xl shadow-2xl w-full max-w-2xl max-h-[90vh] overflow-hidden flex flex-col">
            <div class="p-6 border-b border-slate-100 flex justify-between items-center bg-slate-50">
                <button id="close-modal" class="text-slate-400 hover:text-slate-600 p-2 text-3xl font-light">&times;</button>
                <div class="text-right">
                    <h3 id="modal-player-name" class="text-2xl font-bold text-slate-800 font-black tracking-tighter">שם שחקן</h3>
                    <p class="text-sm text-slate-500 mt-1 font-medium italic">פירוט סשנים וחישוב תוצאה</p>
                </div>
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
                    <div class="bg-blue-50 p-4 rounded-2xl border border-blue-100 text-right shadow-sm">
                        <span class="text-[10px] text-blue-600 font-black uppercase tracking-widest block mb-1">סה"כ P&L</span>
                        <div id="modal-total-pnl" class="text-2xl font-black tracking-tighter">0.00</div>
                    </div>
                    <div id="modal-fee-card" class="bg-slate-50 p-4 rounded-2xl border border-slate-100 text-right shadow-sm">
                        <span class="text-[10px] text-slate-500 font-black uppercase tracking-widest block mb-1">עמלה נטו</span>
                        <div id="modal-total-fee" class="text-2xl font-black tracking-tighter">0.00</div>
                    </div>
                </div>
                <h4 class="font-black text-slate-700 mb-3 text-xs uppercase tracking-widest border-r-4 border-slate-200 pr-2">היסטוריית משחקים:</h4>
                <div class="overflow-x-auto">
                    <table class="w-full text-right border-collapse">
                        <thead>
                            <tr class="text-[10px] font-black text-slate-400 uppercase border-b">
                                <th class="pb-3 px-2">תאריך</th>
                                <th class="pb-3 px-2">משחק</th>
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
                    <div class="flex items-center gap-3 order-last md:order-first">
                        <h1 class="text-2xl font-black text-slate-800 tracking-tighter uppercase">💼 התחשבנות</h1>
                        <span id="user-badge" class="bg-blue-50 text-blue-700 px-3 py-1 rounded-full text-[10px] font-black border border-blue-100 uppercase tracking-tighter"></span>
                    </div>
                    <div class="flex items-center gap-2 bg-slate-100 p-1 rounded-2xl border border-slate-200">
                        <label class="text-[9px] font-black text-slate-400 uppercase px-2">מחזור:</label>
                        <select id="cycle-selector" class="bg-white border-0 text-slate-800 text-xs font-black rounded-xl px-4 py-2 focus:ring-2 focus:ring-blue-500 outline-none shadow-sm cursor-pointer"></select>
                    </div>
                    <nav class="flex gap-2 items-center">
                        <div id="main-nav" class="flex gap-2 mr-4">
                            <button id="nav-dashboard" class="nav-btn active px-4 py-2 rounded-xl border border-slate-200 text-slate-700 font-bold text-sm">📊 דאשבורד</button>
                            <button id="nav-agents" class="nav-btn px-4 py-2 rounded-xl border border-slate-200 text-slate-700 font-bold text-sm">👥 דוחות</button>
                            <button id="nav-mtt" class="nav-btn px-4 py-2 rounded-xl border border-slate-200 text-slate-700 font-bold text-sm">🏆 טורנירים</button>
                        </div>
                        <button id="logout-btn" class="bg-slate-50 text-slate-500 hover:text-red-600 px-4 py-2 rounded-xl border border-slate-200 font-black text-xs uppercase tracking-widest">🚪 יציאה</button>
                    </nav>
                </div>
            </div>
        </header>

        <main class="flex-grow container mx-auto px-4 lg:px-8 max-w-7xl py-8">
            <div id="view-dashboard" class="view-section block">
                <div class="mb-8 text-right"><h2 class="text-3xl font-black text-slate-800 mb-2 tracking-tight">תמונת מצב מועדון</h2></div>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8" id="dash-stats"></div>
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                    <div class="bg-white rounded-3xl shadow-sm border border-slate-200 p-6">
                        <h3 class="text-lg font-black text-slate-800 mb-4 border-b pb-2 uppercase tracking-tighter text-right">מאזן סופי לפי סוכן</h3>
                        <div class="chart-container"><canvas id="settlementChart"></canvas></div>
                    </div>
                    <div class="bg-white rounded-3xl shadow-sm border border-slate-200 p-6">
                        <h3 class="text-lg font-black text-slate-800 mb-4 border-b pb-2 uppercase tracking-tighter text-right font-black">פילוח עמלות נטו</h3>
                        <div class="chart-container"><canvas id="feeDistributionChart"></canvas></div>
                    </div>
                </div>
            </div>

            <div id="view-agents" class="view-section hidden text-right">
                <div id="agent-selector-container" class="bg-white rounded-3xl shadow-sm border border-slate-200 p-6 mb-8 text-right">
                    <label class="block text-[11px] font-black text-slate-500 mb-2 uppercase tracking-widest">בחר סוכן:</label>
                    <select id="agent-selector" class="w-full md:w-1/3 bg-slate-50 border border-slate-300 text-slate-900 text-lg font-bold rounded-xl p-3 outline-none shadow-inner"></select>
                </div>
                <div id="agent-details-container">
                    <div class="grid grid-cols-2 md:grid-cols-5 gap-4 mb-8" id="agent-metrics"></div>
                    <div class="bg-white rounded-3xl shadow-sm border border-slate-200 p-6">
                        <h3 class="text-lg font-black text-slate-800 mb-2 border-b border-slate-100 pb-2 uppercase tracking-tighter text-right">פירוט שחקנים</h3>
                        <div class="overflow-x-auto">
                            <table class="w-full text-right border-collapse font-medium">
                                <thead class="bg-slate-50 text-slate-400 text-[10px] font-black uppercase tracking-widest">
                                    <tr>
                                        <th class="p-4 border-b">שם שחקן</th>
                                        <th class="p-4 border-b text-left">תוצאה (P&L)</th>
                                        <th class="p-4 border-b text-left fee-col">עמלה</th>
                                    </tr>
                                </thead>
                                <tbody id="agent-players-table" class="divide-y text-slate-700"></tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>

            <div id="view-player-single" class="view-section hidden text-right">
                <div class="max-w-3xl mx-auto">
                    <div class="bg-white rounded-[2.5rem] shadow-xl border border-slate-200 overflow-hidden mb-8">
                        <div class="bg-slate-900 text-white p-8 sm:p-12 text-right">
                            <h2 class="text-4xl font-black mb-2 tracking-tighter uppercase" id="player-view-name">שלום</h2>
                            <p class="opacity-50 font-bold uppercase tracking-widest text-xs">סיכום ביצועים אישי</p>
                        </div>
                        <div class="p-8 sm:p-12">
                            <div class="bg-slate-50 p-8 rounded-[2rem] border border-slate-100 flex justify-between items-center mb-10 shadow-inner">
                                <div class="text-right">
                                    <span class="text-[10px] font-black text-slate-400 uppercase tracking-widest block mb-1">תוצאה סופית (P&L)</span>
                                    <div id="player-view-pnl" class="text-5xl font-black tracking-tighter">0.00</div>
                                </div>
                                <div class="bg-white w-20 h-20 rounded-3xl shadow-sm flex items-center justify-center text-4xl">💹</div>
                            </div>
                            <h3 class="text-xl font-black text-slate-800 mb-6 border-b pb-2 uppercase tracking-tighter text-right">פירוט משחקים</h3>
                            <div class="overflow-x-auto"><table class="w-full text-right font-medium"><tbody id="player-view-table" class="divide-y text-slate-700"></tbody></table></div>
                        </div>
                    </div>
                </div>
            </div>

            <div id="view-mtt" class="view-section hidden text-right">
                <div class="mb-8 font-black"><h2 class="text-3xl uppercase">ניתוח טורנירים (MTT)</h2></div>
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
            'בליינדרס': { pass: 'blinders7', role: 'agent', name: 'בליינדרס', agentIndex: 6 }
        };

        const cycles = [
            {
                id: "current",
                label: "מחזור נוכחי (05/05 - 12/05)",
                mtt: { overlay: 0, internalLoss: 0 },
                agents: [
                    { name: "חיים", pastBalance: 0.00, players: [] },
                    { name: "איתי", pastBalance: 0.00, players: [] },
                    { name: "אבי", pastBalance: 0.00, players: [] },
                    { name: "עוז", pastBalance: 0.00, players: [] },
                    { name: "אלחנן", pastBalance: 0.00, players: [] },
                    { name: "יוני", pastBalance: 0.00, players: [] },
                    { name: "בליינדרס", pastBalance: 0.00, players: [] }
                ]
            },
            {
                id: "historical",
                label: "מחזור שנסגר (28/04 - 05/05)",
                mtt: { overlay: -1920.00, internalLoss: -236.03 },
                agents: [
                    {
                        name: "חיים", pastBalance: 0.00,
                        players: [
                            { name: "Bathens", pnl: -5536.39, fee: 1145.82, games: [{d:'28/04',t:'PLO 25/50',r:-3000,f:600.50},{d:'01/05',t:'PLO 25/50',r:-2536.39,f:545.32}] },
                            { name: "avim24", pnl: -1055.78, fee: 1008.05, games: [{d:'29/04',t:'NLH 5/10',r:-1055.78,f:1008.05}] },
                            { name: "dipstay", pnl: -1240.00, fee: 121.49, games: [{d:'30/04',t:'PLO 10/20',r:-1440,f:121.49},{d:'05/05',t:'בונוס',r:200,f:0}] },
                            { name: "omar1989!", pnl: -500.00, fee: 621.35, games: [{d:'02/05',t:'NLH 5/10',r:-500,f:621.35}] },
                            { name: "Amirgu", pnl: -554.00, fee: 144.67, games: [{d:'29/04',t:'NLH 10/20',r:-554,f:144.67}] },
                            { name: "raz121212", pnl: 291.69, fee: 129.32, games: [{d:'30/04',t:'NLH 2/4',r:291.69,f:129.32}] },
                            { name: "Snow23", pnl: 206.23, fee: 30.00, games: [{d:'03/05',t:'NLH 5/10',r:206.23,f:30}] },
                            { name: "or75", pnl: 162.37, fee: 32.57, games: [{d:'02/05',t:'NLH 2/4',r:162.37,f:32.57}] },
                            { name: "Leopold2291", pnl: 116.64, fee: 30.00, games: [{d:'04/05',t:'NLH 5/10',r:116.64,f:30}] },
                            { name: "Kobisayer", pnl: 102.40, fee: 137.68, games: [{d:'03/05',t:'NLH 5/10',r:102.40,f:137.68}] },
                            { name: "Yana79", pnl: 70.85, fee: 80.00, games: [{d:'01/05',t:'NLH 5/10',r:70.85,f:80}] },
                            { name: "nigil", pnl: 145.41, fee: 0.00, games: [{d:'01/05',t:'NLH 2/4',r:145.41,f:0}] },
                            { name: "Gil", pnl: -453.00, fee: 38.91, games: [{d:'01/05',t:'NLH 5/10',r:-453,f:38.91}] },
                            { name: "Adi Rahimian", pnl: -126.11, fee: 30.90, games: [{d:'02/05',t:'NLH 5/10',r:-126.11,f:30.90}] },
                            { name: "Baraks1", pnl: -55.00, fee: 98.52, games: [{d:'04/05',t:'NLH 2/4',r:-55,f:98.52}] },
                            { name: "YoavAA", pnl: 23.54, fee: 15.00, games: [{d:'03/05',t:'NLH 5/10',r:23.54,f:15}] },
                            { name: "dasi7", pnl: 3.44, fee: 75.00, games: [{d:'02/05',t:'NLH 5/10',r:3.44,f:75}] },
                            { name: "Yona177", pnl: 0.00, fee: 237.65, games: [{d:'01/05',t:'NLH 5/10',r:0,f:237.65}] },
                            { name: "Yossi xXx", pnl: 0, fee: 0, games: [] },
                            { name: "Dadi@Max", pnl: 0, fee: 0, games: [] },
                            { name: "ilanzi", pnl: 0, fee: 0, games: [] },
                            { name: "addADDis", pnl: 0, fee: 0, games: [] },
                            { name: "Shekel@", pnl: 0, fee: 0, games: [] },
                            { name: "DysonV", pnl: 0, fee: 0, games: [] },
                            { name: "malibu pompom", pnl: 0, fee: 0, games: [] }
                        ]
                    },
                    {
                        name: "עוז", pastBalance: -2550.00,
                        players: [
                            { name: "adirmezin12", pnl: -697.36, fee: 415.74, games: [{d:'30/04',r:-697.36,f:415.74}] },
                            { name: "AM26", pnl: 107.00, fee: 439.00, games: [{d:'01/05',r:107,f:439}] },
                            { name: "BOOM", pnl: -600.00, fee: 289.00, games: [{d:'02/05',r:-600,f:289}] },
                            { name: "Ofir eliyahu198", pnl: -261.73, fee: 120.16, games: [{d:'03/05',r:-261.73,f:120.16}] },
                            { name: "yosi!!", pnl: -300.00, fee: 60.29, games: [{d:'04/05',r:-300,f:60.29}] },
                            { name: "ozozoz111", pnl: -300.00, fee: 30.19, games: [{d:'29/04',r:-300,f:30.19}] },
                            { name: "israel999", pnl: 0.00, fee: 0.00, games: [] }
                        ]
                    },
                    {
                        name: "איתי", pastBalance: 0.00,
                        players: [
                            { name: "OTC 1", pnl: 126.70, fee: 173.00, games: [{d:'01/05',t:'NLH 5/10',r:-53.34,f:173.00},{d:'MTT',t:'רווח',r:180.04,f:0}] },
                            { name: "Aviad1111", pnl: -3632.81, fee: 936.15, games: [{d:'28/04',r:-2000,f:500},{d:'30/04',r:-1632.81,f:436.15}] },
                            { name: "in2024", pnl: 775.41, fee: 257.16, games: [{d:'01/05',r:775.41,f:257.16}] },
                            { name: "Black Rain82", pnl: 862.65, fee: 232.56, games: [{d:'03/05',r:862.65,f:232.56}] },
                            { name: "dan13579", pnl: 102.56, fee: 25.77, games: [{d:'02/05',r:102.56,f:25.77}] },
                            { name: "heziza", pnl: -400.00, fee: 84.56, games: [{d:'04/05',r:-400,f:84.56}] }
                        ]
                    },
                    {
                        name: "אבי", pastBalance: 0.00,
                        players: [
                            { name: "מור קריטי", pnl: -5473.00, fee: 1777.00, games: [{d:'28/04',r:-5473,f:1777}] },
                            { name: "yoram3554", pnl: -4419.44, fee: 1924.24, games: [{d:'01/05',r:-4419,f:1924}] },
                            { name: "dani shovevani1", pnl: 2643.54, fee: 1054.53, games: [{d:'01/05',r:2871.02,f:1054.53},{d:'MTT',r:-227.48,f:0}] },
                            { name: "אלדד כהן", pnl: -1075.75, fee: 2675.64, games: [{d:'29/04',r:-1075,f:2675}] },
                            { name: "עופר וקנין", pnl: 791.86, fee: 2299.81, games: [{d:'30/04',r:791,f:2299}] },
                            { name: "Alof mlyda", pnl: -1388.74, fee: 1836.81, games: [{d:'04/05',r:-1144.46,f:1836.81},{d:'MTT',r:-244.28,f:0}] },
                            { name: "P2338-2447", pnl: -176.78, fee: 1913.70, games: [{d:'02/05',r:-365.66,f:1913.70},{d:'MTT',r:188.88,f:0}] },
                            { name: "דני", pnl: -900, fee: 467.11, games: [{d:'02/05',r:-900,f:467}] }
                        ]
                    },
                    {
                        name: "יוני", pastBalance: 0.00,
                        players: [
                            { name: "sepopo", pnl: 0, fee: 283.83, games: [{d:'02/05',r:0,f:283.83}] },
                            { name: "SHAYPI", pnl: 216.33, fee: 97.70, games: [{d:'02/05',r:216.33,f:97.70}] },
                            { name: "owl50", pnl: 0, fee: 0, games: [] },
                            { name: "YOUSUFTHEBEAR", pnl: 0, fee: 0, games: [] },
                            { name: "RAP MASTER", pnl: 0, fee: 0, games: [] },
                            { name: "ghost baba", pnl: -213.22, fee: 2.81, games: [{d:'02/05',r:-26.68,f:2.81},{d:'MTT',r:-186.54,f:0}] }
                        ]
                    },
                    {
                        name: "אלחנן", pastBalance: -2000,
                        players: [
                            { name: "IzMaR", pnl: 713.31, fee: 622.27, games: [{d:'05/05',t:'חוב/בונוס',r:-2100,f:0},{d:'02/05',r:2813.31,f:622.27}] }
                        ]
                    },
                    {
                        name: "בליינדרס", pastBalance: 0,
                        players: [
                            { name: "Hagaim", pnl: 700, fee: 160.45, games: [{d:'02/05',r:700,f:160.45}] },
                            { name: "Yoni250787", pnl: 67.76, fee: 0, games: [] },
                            { name: "BlindersT", pnl: 10.50, fee: 1.51, games: [] }
                        ]
                    }
                ]
            }
        ];

        let currentUser = null;
        let selectedCycleIndex = 1;
        let settlementChart = null;

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
            selector.innerHTML = cycles.map((c, i) => `<option value="${i}" ${i===selectedCycleIndex ? 'selected':''}>${c.label}</option>`).join('');
            selector.onchange = (e) => { selectedCycleIndex = Number(e.target.value); setupView(); };
        }

        function setupView() {
            document.getElementById('user-badge').textContent = currentUser.name;
            const cycle = cycles[selectedCycleIndex];
            if (currentUser.role === 'admin') {
                switchTab('nav-dashboard', 'view-dashboard');
            } else {
                document.querySelectorAll('.nav-btn').forEach(b => { if(b.id !== 'nav-agents') b.classList.add('hidden'); });
                switchTab('nav-agents', 'view-agents');
                renderAgentDetails(cycle.agents.find(a => a.name === currentUser.name));
            }
            initNavigation();
        }

        function initNavigation() {
            document.getElementById('logout-btn').onclick = () => window.location.reload();
            document.getElementById('close-modal').onclick = () => document.getElementById('player-modal').classList.add('hidden');
            const selector = document.getElementById('agent-selector');
            selector.innerHTML = '<option value="">בחר סוכן...</option>';
            cycles[selectedCycleIndex].agents.forEach((a, i) => selector.innerHTML += `<option value="${i}">${a.name}</option>`);
            selector.onchange = (e) => e.target.value !== "" && renderAgentDetails(cycles[selectedCycleIndex].agents[e.target.value]);
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
            const summaries = cycles[selectedCycleIndex].agents.map(a => calculateAgentSummary(a, cycles[selectedCycleIndex].agents));
            document.getElementById('dash-stats').innerHTML = `
                <div class="bg-white p-6 rounded-3xl border shadow-sm"><span class="text-[10px] text-slate-400 block mb-1">סוכנים</span><div class="text-3xl font-black">${summaries.length}</div></div>
                <div class="bg-white p-6 rounded-3xl border shadow-sm"><span class="text-[10px] text-slate-400 block mb-1">סה"כ עמלות</span><div class="text-3xl font-black text-blue-600">${format(summaries.reduce((s,x)=>s+x.rawFees,0))}</div></div>
                <div class="bg-white p-6 rounded-3xl border shadow-sm"><span class="text-[10px] text-slate-400 block mb-1">מאזן מועדון</span><div class="text-3xl font-black ${summaries.reduce((s,x)=>s+x.final,0)>=0?'text-emerald-600':'text-red-600'}">${format(summaries.reduce((s,x)=>s+x.final,0))}</div></div>`;
            if(settlementChart) settlementChart.destroy();
            settlementChart = new Chart(document.getElementById('settlementChart'), {
                type: 'bar',
                data: { labels: cycles[selectedCycleIndex].agents.map(a => a.name), datasets: [{ data: summaries.map(x => x.final), backgroundColor: summaries.map(x => x.final >= 0 ? '#10b981' : '#f43f5e'), borderRadius: 6 }] },
                options: { responsive: true, maintainAspectRatio: false }
            });
        }

        function renderAgentDetails(agent) {
            const summary = calculateAgentSummary(agent, cycles[selectedCycleIndex].agents);
            const rate = getAgentPersonalRate(agent.name);
            document.getElementById('agent-metrics').innerHTML = `
                <div class="bg-white p-4 rounded-2xl border shadow-sm"><span class="text-[9px] font-black text-slate-400 block mb-1 uppercase">P&L שחקנים</span><div class="text-xl font-black ${summary.sumPnl>=0?'text-emerald-600':'text-red-600'}">${format(summary.sumPnl)}</div></div>
                <div class="bg-white p-4 rounded-2xl border shadow-sm"><span class="text-[9px] font-black text-slate-400 block mb-1 uppercase">עמלת סוכן</span><div class="text-xl font-black text-blue-600">${format(summary.agentCut)}</div></div>
                ${agent.name==="חיים"?`<div class="bg-blue-600 p-4 rounded-2xl border border-blue-500 text-white text-right shadow-lg"><span class="text-[9px] font-black block mb-1 opacity-80">עמלת רשת (עוז)</span><div class="text-xl font-black">${format(summary.referralCut)}</div></div>`:''}
                <div class="bg-white p-4 rounded-2xl border shadow-sm"><span class="text-[9px] font-black text-slate-400 block mb-1 uppercase">יתרת עבר</span><div class="text-xl font-black text-slate-700">${format(agent.pastBalance)}</div></div>
                <div class="${summary.final>=0?'bg-emerald-50':'bg-red-50'} p-4 rounded-2xl border shadow-sm col-span-2 md:col-span-1"><span class="text-[9px] font-black block mb-1 uppercase">שורה תחתונה</span><div class="text-2xl font-black ${summary.final>=0?'text-emerald-600':'text-red-600'}">${format(summary.final)}</div></div>`;
            const tbody = document.getElementById('agent-players-table');
            tbody.innerHTML = agent.players.length===0?'<tr><td colspan="3" class="p-12 text-center text-slate-300 italic font-bold">אין פעילות</td></tr>':
                [...agent.players].sort((a,b)=>Math.abs(b.pnl)-Math.abs(a.pnl)).map(p => `
                <tr class="player-row cursor-pointer hover:bg-slate-50 border-b border-slate-100 transition-colors" onclick="openPlayerDetails('${agent.name}','${p.name}')">
                    <td class="p-4 font-bold text-slate-800 text-sm">${p.name}</td>
                    <td class="p-4 font-black text-left ${p.pnl>0?'text-emerald-600':p.pnl<0?'text-red-600':'text-slate-400'} text-sm" dir="ltr">${format(p.pnl)}</td>
                    <td class="p-4 text-left text-slate-600 font-black text-sm" dir="ltr">${format(p.fee*rate)}</td>
                </tr>`).join('');
        }

        function openPlayerDetails(agentName, playerName) {
            const cycle = cycles[selectedCycleIndex];
            const agent = cycle.agents.find(a => a.name === agentName);
            const player = agent.players.find(p => p.name === playerName);
            const rate = getAgentPersonalRate(agent.name);
            document.getElementById('modal-player-name').textContent = playerName;
            document.getElementById('modal-total-pnl').textContent = format(player?.pnl || 0);
            document.getElementById('modal-total-pnl').className = `text-2xl font-black ${(player?.pnl||0)>=0?'text-emerald-600':'text-red-600'}`;
            document.getElementById('modal-total-fee').textContent = format((player?.fee||0)*rate);
            document.getElementById('modal-games-body').innerHTML = player?.games.length>0? player.games.map(g => `
                <tr class="hover:bg-slate-50 border-b border-slate-50 last:border-0 transition-colors">
                    <td class="py-4 px-2 text-[11px] font-bold text-slate-400 uppercase">${g.d||'01/05'}</td>
                    <td class="py-4 px-2 text-sm font-black text-slate-700">${g.t||'סשן'}</td>
                    <td class="py-4 px-2 text-sm font-black text-left ${g.r>=0?'text-emerald-600':'text-red-600'}" dir="ltr">${format(g.r)}</td>
                    <td class="py-4 px-2 text-sm text-left font-bold text-slate-400 fee-col" dir="ltr">${format(g.f*rate)}</td>
                </tr>`).join('') : '<tr><td colspan="4" class="py-12 text-center text-slate-300 font-black italic">אין מידע זמין</td></tr>';
            document.getElementById('player-modal').classList.remove('hidden');
        }

        function renderMTT() {
            const mtt = cycles[selectedCycleIndex].mtt;
            document.getElementById('mtt-summary').innerHTML = `
                <div class="bg-white p-8 rounded-3xl border border-red-100 border-t-4 shadow-sm text-right">
                    <h3 class="text-xl font-black text-slate-800 mb-6 text-center uppercase tracking-tighter">זליגת כספים בטורנירים</h3>
                    <div class="space-y-4 font-bold">
                        <div class="flex justify-between p-3 bg-slate-50 rounded-2xl"><span>Overlay</span><span class="text-red-600 font-black">${format(mtt.overlay)}</span></div>
                        <div class="flex justify-between p-3 bg-slate-50 rounded-2xl"><span>הפסד שחקני בית</span><span class="text-red-600 font-black">${format(mtt.internalLoss)}</span></div>
                        <div class="flex justify-between p-5 bg-red-600 text-white rounded-3xl shadow mt-6 font-black uppercase"><span class="uppercase tracking-widest">סה"כ עלות מועדון</span><span class="text-2xl">${format(mtt.overlay + mtt.internalLoss)}</span></div>
                    </div>
                </div>`;
        }

        document.getElementById('login-btn').onclick = handleLogin;
        // Fix keyboard Enter logic
        window.addEventListener('keydown', (e) => {
            if (e.key === 'Enter' && !document.getElementById('login-screen').classList.contains('hidden')) {
                handleLogin();
            }
        });

        // Initialize dashboard navigation
        const tabs = ['nav-dashboard', 'nav-agents', 'nav-mtt'];
        tabs.forEach(tabId => document.getElementById(tabId).onclick = () => switchTab(tabId, 'view-' + tabId.split('-')[1]));
    </script>
</body>
</html>
