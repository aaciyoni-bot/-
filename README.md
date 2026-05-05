<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>מערכת התחשבנות - ניהול מחזורים ושחקנים</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    
    <!-- Chosen Palette: Professional Slate & Blue (Financial Audit Focus) -->
    
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
<body class="min-h-screen flex flex-col text-right">

    <!-- Login Screen -->
    <div id="login-screen" class="fixed inset-0 z-50 flex items-center justify-center p-4" style="background-color: #0f172a;">
        <div class="bg-white p-8 rounded-2xl shadow-2xl w-full max-w-md border border-slate-200">
            <div class="text-center mb-8">
                <div class="inline-flex items-center justify-center w-16 h-16 bg-blue-100 text-blue-600 rounded-full mb-4 font-bold text-2xl">₪</div>
                <h2 class="text-2xl font-bold text-slate-800 font-black tracking-tighter uppercase">כניסת מערכת</h2>
                <p class="text-slate-500 mt-2 font-medium">ניהול התחשבנות וסשנים</p>
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
                    <h4 class="text-[11px] font-black text-emerald-700 uppercase tracking-widest mb-4 flex items-center gap-2">🔑 פרטי גישה לשחקן (לשליחה)</h4>
                    <div class="flex flex-col sm:flex-row gap-4">
                        <div class="flex-1 bg-white p-3 rounded-xl border border-emerald-200">
                            <span class="text-[9px] text-emerald-500 font-bold block uppercase mb-1">שם משתמש:</span>
                            <code id="modal-display-username" class="text-lg font-black text-slate-800 tracking-tight">---</code>
                        </div>
                        <div class="flex-1 bg-white p-3 rounded-xl border border-emerald-200">
                            <span class="text-[9px] text-emerald-500 font-bold block uppercase mb-1">סיסמה:</span>
                            <code id="modal-display-password" class="text-lg font-black text-slate-800 tracking-tight">---</code>
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
                
                <h4 class="font-black text-slate-700 mb-3 text-xs uppercase tracking-widest border-r-4 border-slate-200 pr-2">היסטוריית משחקים וטורנירים:</h4>
                <div class="overflow-x-auto">
                    <table class="w-full text-right border-collapse">
                        <thead>
                            <tr class="text-[10px] font-black text-slate-400 uppercase border-b">
                                <th class="pb-3 px-2">סוג/תאריך</th>
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
                    <div class="flex items-center gap-3 order-last md:order-first">
                        <h1 class="text-2xl font-black text-slate-800 tracking-tighter uppercase">💼 התחשבנות</h1>
                        <span id="user-badge" class="bg-blue-50 text-blue-700 px-3 py-1 rounded-full text-[10px] font-black border border-blue-100 uppercase tracking-widest tracking-tighter"></span>
                    </div>
                    
                    <div class="flex items-center gap-2 bg-slate-100 p-1 rounded-2xl border border-slate-200">
                        <label class="text-[9px] font-black text-slate-400 uppercase px-2">מחזור:</label>
                        <select id="cycle-selector" class="bg-white border-0 text-slate-800 text-xs font-black rounded-xl px-4 py-2 focus:ring-2 focus:ring-blue-500 outline-none shadow-sm cursor-pointer"></select>
                    </div>

                    <nav class="flex gap-2 items-center">
                        <div id="main-nav" class="flex gap-2 mr-4">
                            <button id="nav-dashboard" class="nav-btn active px-4 py-2 rounded-xl border border-slate-200 text-slate-700 font-bold text-sm whitespace-nowrap">📊 דאשבורד</button>
                            <button id="nav-agents" class="nav-btn px-4 py-2 rounded-xl border border-slate-200 text-slate-700 font-bold text-sm whitespace-nowrap">👥 דוחות</button>
                            <button id="nav-mtt" class="nav-btn px-4 py-2 rounded-xl border border-slate-200 text-slate-700 font-bold text-sm whitespace-nowrap">🏆 טורנירים</button>
                        </div>
                        <button id="logout-btn" class="bg-slate-50 text-slate-500 hover:text-red-600 px-4 py-2 rounded-xl border border-slate-200 font-black text-xs transition-all uppercase tracking-widest">🚪 יציאה</button>
                    </nav>
                </div>
            </div>
        </header>

        <main class="flex-grow container mx-auto px-4 lg:px-8 max-w-7xl py-8">
            
            <div id="view-dashboard" class="view-section block">
                <div class="mb-8 text-right">
                    <h2 class="text-3xl font-black text-slate-800 mb-2 tracking-tight">תמונת מצב מועדון</h2>
                    <p class="text-slate-600 font-medium">סיכום התחשבנות גלובלי למחזור הנבחר.</p>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8" id="dash-stats"></div>
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                    <div class="bg-white rounded-3xl shadow-sm border border-slate-200 p-6">
                        <h3 class="text-lg font-black text-slate-800 mb-4 border-b pb-2 uppercase tracking-tighter text-right font-black">מאזן סופי לפי סוכן</h3>
                        <div class="chart-container"><canvas id="settlementChart"></canvas></div>
                    </div>
                    <div class="bg-white rounded-3xl shadow-sm border border-slate-200 p-6">
                        <h3 class="text-lg font-black text-slate-800 mb-4 border-b pb-2 uppercase tracking-tighter text-right font-black">פילוח עמלות (נטו מועדון)</h3>
                        <div class="chart-container"><canvas id="feeDistributionChart"></canvas></div>
                    </div>
                </div>
            </div>

            <div id="view-agents" class="view-section hidden text-right">
                <div id="agent-selector-container" class="bg-white rounded-3xl shadow-sm border border-slate-200 p-6 mb-8 text-right">
                    <label class="block text-[11px] font-black text-slate-500 mb-2 uppercase tracking-widest">בחר סוכן להצגה:</label>
                    <select id="agent-selector" class="w-full md:w-1/3 bg-slate-50 border border-slate-300 text-slate-900 text-lg font-bold rounded-xl p-3 outline-none shadow-inner text-right"></select>
                </div>

                <div id="agent-details-container">
                    <div class="grid grid-cols-2 md:grid-cols-5 gap-4 mb-8" id="agent-metrics"></div>
                    <div class="bg-white rounded-3xl shadow-sm border border-slate-200 p-6">
                        <h3 class="text-lg font-black text-slate-800 mb-2 border-b border-slate-100 pb-2 uppercase tracking-tighter">פירוט שחקנים</h3>
                        <p class="text-xs text-slate-400 mb-4 font-medium italic tracking-tight">* לחץ על שם שחקן לצפייה בפרטי גישה ובפירוט המשחקים.</p>
                        <div class="overflow-x-auto">
                            <table class="w-full text-right border-collapse font-medium">
                                <thead class="bg-slate-50 text-slate-400 text-[10px] font-black uppercase tracking-widest">
                                    <tr class="text-right">
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
                            <p class="opacity-50 font-bold uppercase tracking-widest text-xs tracking-widest">סיכום ביצועים אישי מבוקר</p>
                        </div>
                        <div class="p-8 sm:p-12">
                            <div class="bg-slate-50 p-8 rounded-[2rem] border border-slate-100 flex justify-between items-center mb-10 shadow-inner">
                                <div class="text-right">
                                    <span class="text-[10px] font-black text-slate-400 uppercase tracking-widest block mb-1">תוצאה סופית (P&L)</span>
                                    <div id="player-view-pnl" class="text-5xl font-black tracking-tighter">0.00</div>
                                </div>
                                <div class="bg-white w-20 h-20 rounded-3xl shadow-sm flex items-center justify-center text-4xl">💹</div>
                            </div>
                            
                            <h3 class="text-xl font-black text-slate-800 mb-6 border-b pb-2 uppercase tracking-tighter tracking-tight font-black">פירוט משחקים</h3>
                            <div class="overflow-x-auto">
                                <table class="w-full text-right font-medium">
                                    <thead>
                                        <tr class="text-[10px] font-black text-slate-400 uppercase border-b">
                                            <th class="pb-3 px-2 text-right">תאריך</th>
                                            <th class="pb-3 px-2 text-right">סוג משחק</th>
                                            <th class="pb-3 px-2 text-left">תוצאה</th>
                                        </tr>
                                    </thead>
                                    <tbody id="player-view-table" class="divide-y text-slate-700"></tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div id="view-mtt" class="view-section hidden text-right font-medium">
                <div class="mb-8"><h2 class="text-3xl font-black text-slate-800 font-black tracking-tighter uppercase font-black">ניתוח טורנירים (MTT)</h2></div>
                <div id="mtt-summary" class="grid grid-cols-1 md:grid-cols-2 gap-8"></div>
            </div>
        </main>
    </div>

    <script>
        // MASTER LOGINS
        const users = {
            'admin': { pass: 'admin123', role: 'admin', name: 'מנהל מערכת' },
            'חיים': { pass: 'חיים177', role: 'agent', name: 'חיים', agentIndex: 0 },
            'איתי': { pass: 'איתי2024', role: 'agent', name: 'איתי', agentIndex: 1 },
            'אבי': { pass: 'אבי2026', role: 'agent', name: 'אבי', agentIndex: 2 },
            'עוז': { pass: 'עוז999', role: 'agent', name: 'עוז', agentIndex: 3 },
            'אלחנן': { pass: 'אלחנן86', role: 'agent', name: 'אלחנן', agentIndex: 4 },
            'יוני': { pass: 'יוני25', role: 'agent', name: 'יוני', agentIndex: 5 },
            'בליינדרס': { pass: 'blinders7', role: 'agent', name: 'בליינדרס', agentIndex: 6 },
            // Players
            'avim24': { pass: 'avim123', role: 'player', name: 'avim24' },
            'Bathens': { pass: 'bat123', role: 'player', name: 'Bathens' },
            'Yana79': { pass: 'yana79', role: 'player', name: 'Yana79' },
            'nigil': { pass: 'nigil123', role: 'player', name: 'nigil' },
            'sepopo': { pass: 'sepopo123', role: 'player', name: 'sepopo' },
            'owl50': { pass: 'owl123', role: 'player', name: 'owl50' },
            'YOUSUFTHEBEAR': { pass: 'bear123', role: 'player', name: 'YOUSUFTHEBEAR' },
            'RAP MASTER': { pass: 'rap123', role: 'player', name: 'RAP MASTER' },
            'SHAYPI': { pass: 'shay123', role: 'player', name: 'SHAYPI' },
            'Aviad1111': { pass: 'avi1111', role: 'player', name: 'Aviad1111' },
            'OTC 1': { pass: 'otc123', role: 'player', name: 'OTC 1' },
            'IzMaR': { pass: 'izmar123', role: 'player', name: 'IzMaR' }
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
                            { name: "Bathens", pnl: -5536.39, fee: 1145.82, games: [{d: '28/04', t: 'PLO 25/50', r: -3000, f: 600.50}, {d: '01/05', t: 'PLO 25/50', r: -2536.39, f: 545.32}] },
                            { name: "avim24", pnl: -1055.78, fee: 1008.05, games: [{d: '29/04', t: 'NLH 5/10', r: -1055.78, f: 1008.05}] },
                            { name: "nigil", pnl: 145.41, fee: 0.00, games: [{d: '01/05', t: 'NLH 2/4', r: 145.41, f: 0.00}] },
                            { name: "dipstay", pnl: -1240.00, fee: 121.49, games: [{d: '30/04', t: 'PLO 10/20', r: -1440.00, f: 121.49}, {d: '05/05', t: 'בונוס מתנה', r: 200.00, f: 0.00}] },
                            { name: "Yana79", pnl: 70.85, fee: 80.00, games: [{d: '01/05', t: 'NLH 5/10', r: 70.85, f: 80}] },
                            { name: "raz121212", pnl: 291.69, fee: 129.32, games: [{d: '30/04', t: 'NLH 2/4', r: 291.69, f: 129.32}] },
                            { name: "Adi Rahimian", pnl: -126.11, fee: 30.90, games: [{d: '02/05', t: 'NLH 5/10', r: -126.11, f: 30.90}] },
                            { name: "Gil", pnl: -453.00, fee: 38.91, games: [{d: '01/05', t: 'NLH 5/10', r: -453, f: 38.91}] }
                        ]
                    },
                    {
                        name: "איתי", pastBalance: 0.00,
                        players: [
                            { name: "Aviad1111", pnl: -3632.81, fee: 936.15, games: [{d: '28/04', t: 'PLO 25/50', r: -2000, f: 500}, {d: '30/04', t: 'NLH 10/20', r: -1632.81, f: 436.15}] },
                            { name: "OTC 1", pnl: 126.70, fee: 173.00, games: [{d: '01/05', t: 'NLH 5/10', r: -53.34, f: 173.00}, {d: 'MTT', t: 'רווח טורניר', r: 180.04, f: 0.00}] },
                            { name: "in2024", pnl: 775.41, fee: 257.16, games: [{d: '01/05', t: 'NLH 5/10', r: 775.41, f: 257.16}] },
                            { name: "Black Rain82", pnl: 862.65, fee: 232.56, games: [{d: '03/05', t: 'PLO 5/10', r: 862.65, f: 232.56}] }
                        ]
                    },
                    {
                        name: "אבי", pastBalance: 0.00,
                        players: [
                            { name: "מור קריטי", pnl: -5473.00, fee: 1777.00, games: [{d: '28/04', t: 'NLH 25/50', r: -5473, f: 1777}] },
                            { name: "yoram3554", pnl: -4419.44, fee: 1924.24, games: [{d: '01/05', t: 'PLO 25/50', r: -4419, f: 1924}] },
                            { name: "Alof mlyda", pnl: -1388.74, fee: 1836.81, games: [{d: '04/05', t: 'NLH 25/50', r: -1144.46, f: 1836.81}, {d: 'MTT', t: 'הפסד טורניר', r: -244.28, f: 0.00}] },
                            { name: "עופר וקנין", pnl: 791.86, fee: 2299.81, games: [{d: '30/04', t: 'PLO 25/50', r: 791, f: 2299}] },
                            { name: "dani shovevani1", pnl: 2643.54, fee: 1054.53, games: [{d: '01/05', t: 'NLH 10/20', r: 2871.02, f: 1054.53}, {d: 'MTT', t: 'הפסד טורניר', r: -227.48, f: 0.00}] }
                        ]
                    },
                    {
                        name: "עוז", pastBalance: -2550.00,
                        players: [
                            { name: "adirmezin12", pnl: -697.36, fee: 415.74, games: [{d: '30/04', t: 'PLO 5/10', r: -697.36, f: 415.74}] },
                            { name: "AM26", pnl: 107.00, fee: 439.00, games: [{d: '01/05', t: 'NLH 5/10', r: 107, f: 439}] },
                            { name: "BOOM", pnl: -600.00, fee: 289.00, games: [{d: '02/05', t: 'NLH 5/10', r: -600, f: 289}] }
                        ]
                    },
                    {
                        name: "אלחנן", pastBalance: -2000.00,
                        players: [
                            { name: "IzMaR", pnl: 713.31, fee: 622.27, games: [{d: '30/04', t: 'NLH 10/20', r: 1500, f: 300}, {d: '02/05', t: 'NLH 10/20', r: 1313.31, f: 322.27}, {d: '05/05', t: 'חוב עבר', r: -2300.00, f: 0.00}, {d: '05/05', t: 'בונוס מתנה', r: 200.00, f: 0.00}] }
                        ]
                    },
                    {
                        name: "יוני", pastBalance: 0.00,
                        players: [
                            { name: "SHAYPI", pnl: 216.33, fee: 97.70, games: [{d: '02/05', t: 'סשן משולב', r: 216.33, f: 97.70}] },
                            { name: "sepopo", pnl: 0.00, fee: 283.83, games: [{d: '02/05', t: 'סשן משולב', r: 0.00, f: 283.83}] },
                            { name: "ghost baba", pnl: -213.22, fee: 2.81, games: [{d: '02/05', t: 'NLH 2/4', r: -26.68, f: 2.81}, {d: 'MTT', t: 'הפסד טורניר', r: -186.54, f: 0.00}] },
                            { name: "owl50", pnl: 0.00, fee: 0.00, games: [] },
                            { name: "YOUSUFTHEBEAR", pnl: 0.00, fee: 0.00, games: [] },
                            { name: "RAP MASTER", pnl: 0.00, fee: 0.00, games: [] }
                        ]
                    },
                    {
                        name: "בליינדרס", pastBalance: 0.00,
                        players: [
                            { name: "Hagaim", pnl: 700.00, fee: 160.45, games: [{d: '02/05', t: 'NLH 5/10', r: 700, f: 160.45}] }
                        ]
                    }
                ]
            }
        ];

        let currentUser = null;
        let selectedCycleIndex = 1;
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
                if (oz) {
                    const ozRawFeesTotal = oz.players.reduce((s, p) => s + p.fee, 0);
                    referralCut = ozRawFeesTotal * 0.3;
                }
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
            const currentCycle = cycles[selectedCycleIndex];
            const nav = document.getElementById('main-nav');
            if (currentUser.role === 'admin') {
                nav.classList.remove('hidden');
                switchTab('nav-dashboard', 'view-dashboard');
            } else if (currentUser.role === 'agent') {
                nav.classList.remove('hidden');
                document.getElementById('nav-dashboard').classList.add('hidden');
                document.getElementById('nav-mtt').classList.add('hidden');
                document.getElementById('agent-selector-container').classList.add('hidden');
                switchTab('nav-agents', 'view-agents');
                const agentData = currentCycle.agents.find(a => a.name === currentUser.name);
                renderAgentDetails(agentData);
            } else if (currentUser.role === 'player') {
                nav.classList.add('hidden');
                switchTab(null, 'view-player-single');
                renderPlayerSingleView(currentUser.name);
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
            const currentCycle = cycles[selectedCycleIndex];
            selector.innerHTML = '<option value="">בחר סוכן...</option>';
            currentCycle.agents.forEach((a, i) => selector.innerHTML += `<option value="${i}">${a.name}</option>`);
            selector.onchange = (e) => e.target.value !== "" && renderAgentDetails(currentCycle.agents[e.target.value]);
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
            const currentCycle = cycles[selectedCycleIndex];
            const summaries = currentCycle.agents.map(a => calculateAgentSummary(a, currentCycle.agents));
            document.getElementById('dash-stats').innerHTML = `
                <div class="bg-white p-6 rounded-3xl border shadow-sm text-right"><span class="text-[10px] text-slate-400 font-black block mb-1">סוכנים פעילים</span><div class="text-3xl font-black text-slate-800 tracking-tighter">${currentCycle.agents.length}</div></div>
                <div class="bg-white p-6 rounded-3xl border shadow-sm text-right"><span class="text-[10px] text-slate-400 font-black block mb-1 uppercase">סה"כ עמלות (ברוטו)</span><div class="text-3xl font-black text-blue-600 tracking-tighter">${format(summaries.reduce((s,x)=>s+x.rawFees,0))}</div></div>
                <div class="bg-white p-6 rounded-3xl border shadow-sm text-right"><span class="text-[10px] text-slate-400 font-black block mb-1 uppercase">מאזן מועדון סופי</span><div class="text-3xl font-black ${summaries.reduce((s,x)=>s+x.final,0) >=0 ? 'text-emerald-600' : 'text-red-600'} tracking-tighter">${format(summaries.reduce((s,x)=>s+x.final,0))}</div></div>`;
            if(settlementChart) settlementChart.destroy();
            settlementChart = new Chart(document.getElementById('settlementChart'), {
                type: 'bar',
                data: { labels: currentCycle.agents.map(a => a.name), datasets: [{ data: summaries.map(x => x.final), backgroundColor: summaries.map(x => x.final >= 0 ? '#10b981' : '#f43f5e'), borderRadius: 6 }] },
                options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } } }
            });
        }

        function renderAgentDetails(agent) {
            const currentCycle = cycles[selectedCycleIndex];
            const summary = calculateAgentSummary(agent, currentCycle.agents);
            const rate = getAgentPersonalRate(agent.name);
            document.getElementById('agent-metrics').innerHTML = `
                <div class="bg-white p-4 rounded-2xl border shadow-sm text-right"><span class="text-[9px] font-black text-slate-400 block mb-1 uppercase">P&L שחקנים</span><div class="text-xl font-black ${summary.sumPnl >= 0 ? 'text-emerald-600' : 'text-red-600'} tracking-tight">${format(summary.sumPnl)}</div></div>
                <div class="bg-white p-4 rounded-2xl border shadow-sm text-right"><span class="text-[9px] font-black text-slate-400 block mb-1 uppercase">עמלת סוכן</span><div class="text-xl font-black text-blue-600 tracking-tight">${format(summary.agentCut)}</div></div>
                ${agent.name === "חיים" ? `<div class="bg-blue-600 p-4 rounded-2xl border border-blue-500 text-white text-right shadow-lg"><span class="text-[9px] font-black block mb-1 opacity-80 uppercase tracking-widest">עמלת רשת (עוז)</span><div class="text-xl font-black">${format(summary.referralCut)}</div></div>` : ''}
                <div class="bg-white p-4 rounded-2xl border shadow-sm text-right"><span class="text-[9px] font-black text-slate-400 block mb-1 uppercase">יתרת עבר</span><div class="text-xl font-black text-slate-700 tracking-tight">${format(agent.pastBalance)}</div></div>
                <div class="${summary.final >= 0 ? 'bg-emerald-50 border-emerald-100' : 'bg-red-50 border-red-100'} p-4 rounded-2xl border shadow-sm text-right col-span-2 md:col-span-1"><span class="text-[9px] font-black block mb-1 uppercase">שורה תחתונה</span><div class="text-2xl font-black ${summary.final >= 0 ? 'text-emerald-600' : 'text-red-600'} tracking-tighter">${format(summary.final)}</div></div>`;
            const tbody = document.getElementById('agent-players-table');
            if (!agent.players || agent.players.length === 0) tbody.innerHTML = '<tr><td colspan="3" class="p-12 text-center text-slate-300 italic font-bold">אין פעילות במחזור זה</td></tr>';
            else tbody.innerHTML = [...agent.players].sort((a,b) => Math.abs(b.pnl) - Math.abs(a.pnl)).map(p => `
                <tr class="player-row cursor-pointer hover:bg-slate-50 border-b border-slate-100 transition-colors text-right" onclick="openPlayerDetails('${agent.name}', '${p.name}')">
                    <td class="p-4 font-bold text-slate-800 player-name text-sm">${p.name}</td>
                    <td class="p-4 font-black text-left ${p.pnl > 0 ? 'text-emerald-600' : p.pnl < 0 ? 'text-red-600' : 'text-slate-400'} text-sm font-black" dir="ltr">${format(p.pnl)}</td>
                    <td class="p-4 text-left text-slate-600 fee-col font-black text-sm" dir="ltr">${format(p.fee * rate)}</td>
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
                pnlVal.textContent = format(player.pnl); pnlVal.className = `text-5xl font-black ${player.pnl >= 0 ? 'text-emerald-600' : 'text-red-600'} tracking-tighter font-black`;
                document.getElementById('player-view-table').innerHTML = player.games.map(g => `
                    <tr class="hover:bg-slate-50 transition-colors text-right font-medium">
                        <td class="py-4 px-2 text-sm font-bold text-slate-400 uppercase tracking-tighter tracking-tight">${g.d}</td>
                        <td class="py-4 px-2 text-sm text-slate-700 font-black tracking-tight">${g.t}</td>
                        <td class="py-4 px-2 text-sm font-black text-left ${g.r >= 0 ? 'text-emerald-600' : 'text-red-600'}" dir="ltr">${format(g.r)}</td>
                    </tr>`).join('');
            }
        }

        function openPlayerDetails(agentName, playerName) {
            const currentCycle = cycles[selectedCycleIndex];
            const agent = currentCycle.agents.find(a => a.name === agentName);
            const player = agent.players.find(p => p.name === playerName);
            const rate = getAgentPersonalRate(agent.name);
            document.getElementById('modal-player-name').textContent = playerName;
            const pnlVal = document.getElementById('modal-total-pnl');
            const feeVal = document.getElementById('modal-total-fee');
            if(!player) {
                pnlVal.textContent = "0.00"; pnlVal.className = "text-2xl font-black text-slate-300"; feeVal.textContent = "0.00";
                document.getElementById('modal-games-body').innerHTML = '<tr><td colspan="4" class="py-12 text-center text-slate-300 font-black italic">אין פעילות במחזור זה</td></tr>';
            } else {
                pnlVal.textContent = format(player.pnl); pnlVal.className = `text-2xl font-black ${player.pnl >= 0 ? 'text-emerald-600' : 'text-red-600'}`;
                feeVal.textContent = format(player.fee * rate);
                document.getElementById('modal-games-body').innerHTML = player.games.map(g => `
                    <tr class="hover:bg-slate-50 border-b border-slate-50 last:border-0 transition-colors text-right font-medium">
                        <td class="py-4 px-2 text-[11px] font-bold text-slate-400 uppercase tracking-tighter tracking-tight">${g.d}</td>
                        <td class="py-4 px-2 text-sm font-black text-slate-700 tracking-tight">${g.t}</td>
                        <td class="py-4 px-2 text-sm font-black text-left ${g.r >= 0 ? 'text-emerald-600' : 'text-red-600'}" dir="ltr">${format(g.r)}</td>
                        <td class="py-4 px-2 text-sm text-left font-bold text-slate-400 fee-col" dir="ltr">${format(g.f * rate)}</td>
                    </tr>`).join('');
            }
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
                <div class="bg-white p-8 rounded-[2.5rem] border border-red-100 border-t-4 border-t-red-500 shadow-sm text-right">
                    <h3 class="text-xl font-black text-slate-800 mb-6 text-center uppercase tracking-tighter">זליגת כספים בטורנירים</h3>
                    <div class="space-y-4 font-bold">
                        <div class="flex justify-between p-3 bg-slate-50 rounded-2xl font-black"><span>Overlay</span><span class="text-red-600 font-black">${format(mtt.overlay)}</span></div>
                        <div class="flex justify-between p-3 bg-slate-50 rounded-2xl font-black"><span>הפסד שחקני בית</span><span class="text-red-600 font-black">${format(mtt.internalLoss)}</span></div>
                        <div class="flex justify-between p-5 bg-red-600 text-white rounded-3xl shadow mt-6 font-black tracking-widest uppercase tracking-widest"><span class="uppercase">סה"כ עלות מועדון</span><span class="text-2xl">${format(mtt.overlay + mtt.internalLoss)}</span></div>
                    </div>
                </div>`;
        }

        document.getElementById('password').addEventListener('keydown', (e) => { if (e.key === 'Enter') handleLogin(); });
        document.getElementById('login-btn').onclick = handleLogin;
    </script>
</body>
</html>
