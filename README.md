<!DOCTYPE html>
<html lang="en" dir="ltr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ORIZIS GROUP | Global Industrial Holdings</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700;800;900&family=Inter:wght@300;400;600;700&display=swap" rel="stylesheet">
    <style>
        /* משתני עיצוב מרכזיים */
        :root {
            --primary-blue: #00C2FF;
            --accent-purple: #A377FF;
            --bg-light: #ffffff;
            --bg-dark: #020617;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-light);
            color: #0f172a;
            scroll-behavior: smooth;
            transition: background-color 0.4s;
        }

        .dark-mode {
            background-color: var(--bg-dark);
            color: #f1f5f9;
        }

        .montserrat { font-family: 'Montserrat', sans-serif; }

        /* ניווט שקוף */
        .glass-nav {
            background: rgba(255, 255, 255, 0.98);
            backdrop-filter: blur(12px);
            border-bottom: 1px solid rgba(0, 0, 0, 0.05);
        }
        .dark-mode .glass-nav {
            background: rgba(2, 6, 23, 0.98);
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        /* אזור Hero בעל ניגודיות גבוהה */
        .hero-section {
            background-color: #020617;
            color: #ffffff;
            padding: 140px 24px;
            position: relative;
        }
        .hero-title {
            font-size: clamp(3rem, 10vw, 7rem);
            line-height: 0.95;
            font-weight: 900;
            text-transform: uppercase;
            letter-spacing: -0.04em;
        }

        /* כרטיסיות פורטפוליו */
        .division-card {
            background: #ffffff;
            border: 1px solid #f1f5f9;
            padding: 60px 30px;
            border-radius: 3rem;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            text-align: center;
            cursor: pointer;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.02);
        }
        .dark-mode .division-card {
            background: rgba(255, 255, 255, 0.02);
            border-color: rgba(255, 255, 255, 0.05);
        }
        .division-card:hover {
            transform: translateY(-12px);
            box-shadow: 0 40px 80px -20px rgba(0, 0, 0, 0.12);
        }

        /* אלמנט המותג המשולב - מודגש וגדול יותר */
        .brand-unit { display: flex; flex-direction: column; align-items: center; }
        .brand-prefix {
            font-family: 'Montserrat', sans-serif;
            font-weight: 400;
            font-size: 12px;
            letter-spacing: 0.6em;
            color: #94a3b8;
            text-transform: uppercase;
            margin-bottom: 0px;
        }
        .brand-main {
            font-family: 'Montserrat', sans-serif;
            font-weight: 900; /* עובי מקסימלי */
            font-size: 40px;  /* הגדלה משמעותית */
            text-transform: uppercase;
            line-height: 0.9;
            letter-spacing: -0.05em; /* ריווח אותיות צפוף ויוקרתי */
        }

        .lang-rtl { direction: rtl; }
        .section-hidden { display: none; }

        /* קונטיינר למדיה */
        .video-container {
            background: #0f172a;
            border-radius: 3.5rem;
            min-height: 500px;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
            overflow: hidden;
            box-shadow: 0 30px 60px -15px rgba(0,0,0,0.3);
        }
        .video-container::after {
            content: '▶';
            font-size: 80px;
            color: white;
            opacity: 0.2;
        }

        .location-dot {
            width: 10px; height: 10px; background: var(--primary-blue); border-radius: 50%;
            position: relative; margin: 0 10px;
        }
        .location-dot::after {
            content: ''; position: absolute; inset: 0; border-radius: 50%;
            background: inherit; animation: pulse 2s infinite;
        }
        @keyframes pulse { 0% { transform: scale(1); opacity: 0.6; } 100% { transform: scale(3); opacity: 0; } }

        .lang-btn { font-size: 10px; font-weight: 900; padding: 6px 10px; border-radius: 6px; border: 1px solid transparent; }
        .lang-btn.active { border-color: var(--primary-blue); color: var(--primary-blue) !important; }

        .high-contrast-text { color: #0f172a; }
        .dark-mode .high-contrast-text { color: #ffffff; }
    </style>
</head>
<body id="bodyTag">

    <!-- סרגל ניווט -->
    <nav class="glass-nav fixed top-0 w-full z-[100] px-6 py-4">
        <div class="max-w-7xl mx-auto flex justify-between items-center">
            <div class="flex items-center gap-3 cursor-pointer" onclick="showHome()">
                <div class="w-9 h-9">
                    <svg viewBox="0 0 100 100" fill="none"><rect x="10" y="10" width="60" height="60" stroke="#00C2FF" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#A377FF" stroke-width="12"/></svg>
                </div>
                <span class="montserrat font-black text-2xl tracking-tighter uppercase italic high-contrast-text">ORIZIS <span class="font-light text-slate-400 not-italic text-sm">GROUP</span></span>
            </div>

            <div class="hidden lg:flex gap-6 items-center">
                <button onclick="showDivision('group')" class="text-[10px] font-black uppercase tracking-widest hover:text-blue-500 transition" data-i18n="nav_group">Management</button>
                <a href="#divisions" class="text-[10px] font-black uppercase tracking-widest hover:text-blue-500 transition" data-i18n="nav_divisions">Portfolio</a>
                
                <div class="flex items-center gap-3 border-l pl-6 border-slate-200">
                    <div class="flex flex-wrap gap-1 max-w-[200px] justify-end">
                        <button onclick="setLanguage('en')" id="btn-en" class="lang-btn active">EN</button>
                        <button onclick="setLanguage('he')" id="btn-he" class="lang-btn">HE</button>
                        <button onclick="setLanguage('fr')" id="btn-fr" class="lang-btn">FR</button>
                        <button onclick="setLanguage('az')" id="btn-az" class="lang-btn">AZ</button>
                        <button onclick="setLanguage('zh')" id="btn-zh" class="lang-btn">ZH</button>
                        <button onclick="setLanguage('ru')" id="btn-ru" class="lang-btn">RU</button>
                        <button onclick="setLanguage('ar')" id="btn-ar" class="lang-btn">AR</button>
                    </div>
                    <button onclick="toggleTheme()" class="ml-4 text-xl">🌓</button>
                </div>
            </div>
        </div>
    </nav>

    <div id="contentWrapper" class="pt-20">
        <!-- דף הבית -->
        <div id="homePage">
            <!-- אזור Hero -->
            <section class="hero-section">
                <div class="max-w-6xl mx-auto text-center">
                    <h1 class="hero-title montserrat mb-8" data-i18n="hero_title">Global Industrial <br><span class="text-blue-400">Authority</span></h1>
                    <p class="text-xl md:text-2xl font-light text-slate-300 mb-12 max-w-3xl mx-auto leading-relaxed" data-i18n="hero_desc">
                        Directing a powerhouse portfolio of 12 market-leading industrial subsidiaries across Africa, Europe, and Asia.
                    </p>
                    <div class="flex flex-col md:flex-row gap-6 justify-center">
                        <a href="#divisions" class="px-16 py-6 bg-white text-slate-900 font-black rounded-full hover:bg-blue-500 hover:text-white transition uppercase text-xs tracking-widest shadow-2xl">Subsidiary Verticals</a>
                        <button onclick="showDivision('group')" class="px-16 py-6 border-2 border-white text-white font-black rounded-full hover:bg-white hover:text-slate-900 transition uppercase text-xs tracking-widest">Strategy & Governance</button>
                    </div>
                </div>
            </section>

            <!-- גריד חטיבות -->
            <section id="divisions" class="py-32 px-6 bg-white dark:bg-slate-900">
                <div class="max-w-7xl mx-auto">
                    <div class="text-center mb-28">
                        <h2 class="text-4xl font-black montserrat uppercase tracking-tighter mb-4 high-contrast-text" data-i18n="div_title">Subsidiary Ecosystem</h2>
                        <p class="text-slate-400 font-bold uppercase tracking-[0.3em] text-[10px]" data-i18n="div_subtitle">Proven Market Leadership</p>
                        <div class="w-24 h-2 bg-blue-500 mx-auto rounded-full mt-8"></div>
                    </div>
                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-12" id="divisionsContainer"></div>
                </div>
            </section>

            <!-- נוכחות גלובלית -->
            <section id="global" class="py-32 px-6 bg-slate-50 dark:bg-slate-950">
                <div class="max-w-7xl mx-auto grid lg:grid-cols-2 gap-24 items-center">
                    <div>
                        <h2 class="text-5xl font-black montserrat uppercase tracking-tighter mb-8 high-contrast-text" data-i18n="global_title">Global Reach</h2>
                        <p class="text-slate-500 mb-12 leading-relaxed text-xl" data-i18n="global_desc">
                            Operational hubs and physical assets managed with cross-continental precision in 11 key regions.
                        </p>
                        <div class="grid grid-cols-2 gap-6" id="locationsList"></div>
                    </div>
                    <div class="bg-white dark:bg-slate-900 h-[500px] rounded-[4rem] flex items-center justify-center border border-slate-200 dark:border-white/5 shadow-2xl">
                         <div class="text-center">
                             <div class="text-[140px] font-black montserrat text-blue-500/10 leading-none">11</div>
                             <p class="uppercase tracking-[0.5em] text-sm font-black text-slate-400">Jurisdictions</p>
                         </div>
                    </div>
                </div>
            </section>
        </div>

        <!-- דף פירוט חטיבה -->
        <div id="divisionPage" class="section-hidden pb-40 bg-white dark:bg-slate-950">
            <header class="py-32 bg-slate-50 dark:bg-slate-900/50 border-b border-slate-200 dark:border-white/10">
                <div class="max-w-7xl mx-auto px-6 flex flex-col items-center text-center">
                    <div id="divLogoBox" class="mb-14 scale-150"></div>
                    <h1 id="divTitle" class="text-6xl md:text-9xl font-black montserrat uppercase tracking-tighter mb-10 high-contrast-text"></h1>
                    <div class="w-24 h-2 bg-blue-500 mb-12 rounded-full shadow-sm"></div>
                    <p id="divVision" class="max-w-4xl mx-auto text-2xl md:text-4xl text-slate-500 font-extralight leading-relaxed italic"></p>
                </div>
            </header>

            <main class="max-w-7xl mx-auto px-6 mt-32">
                <div class="grid lg:grid-cols-12 gap-24">
                    <div class="lg:col-span-8 space-y-40">
                        <section>
                            <h2 class="text-xs font-black uppercase tracking-[0.5em] text-blue-500 mb-12">Operational Overview</h2>
                            <p id="divAbout" class="text-3xl text-slate-600 dark:text-slate-400 leading-relaxed font-light"></p>
                        </section>
                        <section>
                            <h2 class="text-xs font-black uppercase tracking-[0.5em] text-blue-500 mb-12">Industrial Media</h2>
                            <div class="video-container">
                                <span class="text-white/20 text-[10px] font-black uppercase tracking-[1.5em]">Operational Reel</span>
                            </div>
                        </section>
                        <section>
                            <h2 class="text-xs font-black uppercase tracking-[0.5em] text-blue-500 mb-16">Strategic Capabilities</h2>
                            <div id="divCaps" class="grid sm:grid-cols-2 gap-10"></div>
                        </section>
                        <section>
                            <h2 class="text-xs font-black uppercase tracking-[0.5em] text-blue-500 mb-16">Selected Field Projects</h2>
                            <div id="divProjects" class="space-y-16"></div>
                        </section>
                    </div>
                    <div class="lg:col-span-4">
                        <div class="bg-slate-50 dark:bg-white/5 p-12 rounded-[4rem] border border-slate-200 dark:border-white/10 sticky top-32 shadow-2xl">
                            <div class="mb-16">
                                <h3 class="text-xs font-black uppercase tracking-[0.3em] text-slate-400 mb-10">Executive Leadership</h3>
                                <div id="divTeam" class="space-y-10"></div>
                            </div>
                            <div class="mb-16">
                                <h3 class="text-xs font-black uppercase tracking-[0.3em] text-slate-400 mb-10">Field Presence</h3>
                                <div id="divLocations" class="space-y-5"></div>
                            </div>
                            <button onclick="showHome()" class="w-full py-6 bg-slate-900 text-white rounded-full text-[10px] font-black uppercase tracking-widest hover:bg-blue-600 shadow-xl transition">Return to Ecosystem</button>
                        </div>
                    </div>
                </div>
            </main>
        </div>
    </div>

    <!-- כותרת תחתונה -->
    <footer class="py-24 border-t border-slate-100 dark:border-white/5 bg-white dark:bg-slate-950 px-6">
        <div class="max-w-7xl mx-auto flex flex-col md:flex-row justify-between items-center gap-12">
            <div class="text-center md:text-left">
                <div class="flex items-center justify-center md:justify-start gap-3 mb-6">
                    <div class="w-10 h-10"><svg viewBox="0 0 100 100" fill="none"><rect x="10" y="10" width="60" height="60" stroke="#00C2FF" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#A377FF" stroke-width="12"/></svg></div>
                    <span class="montserrat font-black text-2xl tracking-tighter uppercase italic high-contrast-text">ORIZIS GROUP</span>
                </div>
                <p class="text-xs text-slate-400 uppercase tracking-[0.4em] font-bold">12 Subsidiaries. 11 Nations. Global standard.</p>
            </div>
            <p class="text-[10px] font-black uppercase tracking-[0.3em] text-slate-400">© 2026 ORIZIS GROUP HOLDINGS LTD.</p>
        </div>
    </footer>

    <script>
        const translations = {
            en: {
                nav_group: "Management", nav_divisions: "Portfolio", nav_presence: "Operations",
                hero_title: "Global Industrial Authority",
                hero_desc: "Orizis Group leads strategic development and large-scale industrial execution across 12 high-impact sectors worldwide.",
                div_title: "Subsidiary Ecosystem", div_subtitle: "12 Verticals of Excellence",
                global_title: "Global Reach", global_desc: "Physical operations across 11 key regions, managing complex industrial chains with precision."
            },
            he: {
                nav_group: "ניהול", nav_divisions: "פורטפוליו", nav_presence: "פעילות",
                hero_title: "סמכות תעשייתית גלובלית",
                hero_desc: "אוריזיס גרופ מובילה פיתוח אסטרטגי וביצוע תעשייתי רחב היקף ב-12 סקטורים בעלי אימפקט גבוה ברחבי העולם.",
                div_title: "המערכת העסקית", div_subtitle: "12 תחומי מצוינות",
                global_title: "פריסה עולמית", global_desc: "פעילות פיזית ב-11 אזורי מפתח, המנוהלת בדייקנות ומומחיות בשרשראות תעשייה מורכבות."
            }
        };

        const locations = ["Zambia", "Botswana", "Tanzania", "Georgia", "Azerbaijan", "Bulgaria", "Israel", "Congo DRC", "Liberia", "Kenya", "Nigeria"];

        const divisions = [
            { 
                id: 'group', title: 'Management', color: '#00C2FF', textCol: '#0f172a', tagline: 'Strategic Nerve Center', 
                icon: '<rect x="10" y="10" width="60" height="60" stroke="#00C2FF" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#A377FF" stroke-width="12"/>',
                vision: 'Leading global strategy and sovereign-level investment.',
                about: 'The Management unit acts as the strategic and financial brain of the global conglomerate, managing cross-border capital flow and governmental advisory.',
                caps: ['Governmental Strategic Advisory', 'Global M&A & Integration', 'Capital Market Structuring', 'National Economic Policy Consulting'],
                leads: [{n:'Yehonathan Friedman', r:'CEO & Chairman'}],
                locations: ['Israel (Management Center)'],
                projects: [{t:'Global Roadmap 2030', d:'Strategic expansion planning for infrastructure corridors.'}]
            },
            { 
                id: 're', title: 'Real Estate', color: '#1E3A8A', textCol: '#1e3a8a', tagline: '360° Property Solutions', 
                icon: '<rect x="10" y="10" width="60" height="60" stroke="#1E3A8A" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#3B82F6" stroke-width="12"/>',
                vision: 'Redefining city skylines and investment portfolios in Azerbaijan.',
                about: 'Providing end-to-end real estate excellence in Baku. We integrate luxury construction with expert brokerage and legal advisory.',
                caps: ['Real Estate Marketing & Sales', 'Luxury Brokerage & Agency', 'Legal & Regulatory Advisory', 'Comprehensive Property Management'],
                leads: [{n:'RE Director', r:'Baku Division'}],
                locations: ['Azerbaijan', 'Georgia'],
                projects: [{t:'Baku Premium Towers', d:'Flagship luxury complex with full management and commercial suites.'}]
            },
            { 
                id: 'tech', title: 'Technologies', color: '#22d3ee', textCol: '#0891b2', tagline: 'Industrial R&D', 
                icon: '<rect x="10" y="10" width="60" height="60" stroke="#22D3EE" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#8B5CF6" stroke-width="12"/>',
                vision: 'Securing industrial grids with advanced cybersecurity.',
                about: 'Based in Sofia, Bulgaria, our Tech hub develops proprietary digital grids for group operations.',
                caps: ['National Cybersecurity Advisory', 'Industrial IoT System Design', 'Enterprise Digital Transformation', 'Proprietary Software R&D'],
                leads: [{n:'Chief Tech Officer', r:'Sofia Hub'}],
                locations: ['Bulgaria', 'Israel'],
                projects: [{t:'The Orizis Data Shield', d:'Proprietary data network connecting global subsidiaries.'}]
            },
            { id: 'hr', title: 'Human Capital', color: '#64748b', textCol: '#334155', tagline: 'Industrial Support', icon: '<rect x="10" y="10" width="60" height="60" stroke="#1e293b" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#94a3b8" stroke-width="12"/>', vision: 'Workforce excellence.', about: 'Providing workforce and safety protocols for demanding sites.', caps: ['National Manpower Policy', 'Security Audits', 'Industrial Facility Management', 'Strategic Staffing'], leads: [{n:'Head of HR', r:'Sofia'}], locations: ['Bulgaria', 'Zambia'], projects: [{t:'Mining Safety Grid', d:'Security management for major mines.'}] },
            { id: 'agro', title: 'Agriculture', color: '#10b981', textCol: '#065f46', tagline: 'Food Security', icon: '<rect x="10" y="10" width="60" height="60" stroke="#059669" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#10B981" stroke-width="12"/>', vision: 'Sustainable farming.', about: 'Leading precision agriculture in East Africa.', caps: ['Food Security Advisory', 'Smart Irrigation', 'Genetics R&D', 'Agri-Trade Logistics'], leads: [{n:'Agro Director', r:'Nairobi Hub'}], locations: ['Kenya'], projects: [{t:'Rift Valley Project', d:'Automated high-yield farming.'}] },
            { id: 'meridian', title: 'Meridian', color: '#dc2626', textCol: '#991b1b', tagline: 'National Infra', icon: '<rect x="10" y="10" width="60" height="60" stroke="#991B1B" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#DC2626" stroke-width="12"/>', vision: 'Continental connectivity.', about: 'Civil engineering in Sub-Saharan Africa.', caps: ['PPP Structuring', 'Sovereign Transport Plans', 'Major Civil Engineering', 'Energy Strategy'], leads: [{n:'Chief Engineer', r:'Africa Hub'}], locations: ['Congo DRC', 'Zambia'], projects: [{t:'Southern Power corridor', d:'Modernization of regional energy grids.'}] },
            { id: 'trade', title: 'Trade', color: '#b45309', textCol: '#78350f', tagline: 'Global Commerce', icon: '<rect x="10" y="10" width="60" height="60" stroke="#7C2D12" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#EA580C" stroke-width="12"/>', vision: 'Linking resources.', about: 'Global mineral brokerage.', caps: ['Trade Policy Advisory', 'Commodity Sourcing', 'Customs Optimization', 'Risk Advisory'], leads: [{n:'Trading Head', r:'Lagos'}], locations: ['Zambia', 'Nigeria'], projects: [{t:'Copper Export Corridor', d:'Managed export logistics for minerals.'}] },
            { id: 'finance', title: 'Capital', color: '#334155', textCol: '#0f172a', tagline: 'Strategic Funding', icon: '<rect x="10" y="10" width="60" height="60" stroke="#0F172A" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#475569" stroke-width="12"/>', vision: 'Industrial expansion capital.', about: 'Complex financial engineering for group projects.', caps: ['Sovereign Wealth Advisory', 'Asset Management', 'Project Finance', 'Venture Capital'], leads: [{n:'Finance Head', r:'Tel Aviv'}], locations: ['Israel'], projects: [{t:'Industrial Growth Fund', d:'$500M vehicle for agro/infra.'}] },
            { id: 'energy', title: 'Energy', color: '#eab308', textCol: '#854d0e', tagline: 'Power Solutions', icon: '<rect x="10" y="10" width="60" height="60" stroke="#EAB308" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#FDE047" stroke-width="12"/>', vision: 'Sustainable power.', about: 'Renewable grids for African nations.', caps: ['National Grid Stability', 'Solar Operations', 'Microgrid Design', 'Carbon Auditing'], leads: [{n:'Energy Lead', r:'East Africa'}], locations: ['Kenya', 'Zambia'], projects: [{t:'Kenya Solar Park', d:'50MW renewable energy project.'}] },
            { id: 'chem', title: 'Chemicals', color: '#f59e0b', textCol: '#92400e', tagline: 'Materials', icon: '<rect x="10" y="10" width="60" height="60" stroke="#D97706" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#F59E0B" stroke-width="12"/>', vision: 'Molecular foundation.', about: 'Supplying reagents and fertilizers.', caps: ['Chemical Safety Advisory', 'Industrial Waste Design', 'Fertilizer R&D', 'Mining Logistics'], leads: [{n:'Lead Chemist', r:'West Africa'}], locations: ['Nigeria', 'Azerbaijan'], projects: [{t:'Industrial Supply Hub', d:'Chemical chain for African mining.'}] },
            { id: 'logistics', title: 'Logistics', color: '#6366f1', textCol: '#3730a3', tagline: 'Transit Backbone', icon: '<rect x="10" y="10" width="60" height="60" stroke="#4338CA" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#818CF8" stroke-width="12"/>', vision: 'The trade engine.', about: 'Port and multimodal transit operations.', caps: ['National Port Consulting', 'Network Strategy', 'Freight Management', 'Logistics Hub Design'], leads: [{n:'Logistics Dir', r:'Monrovia'}], locations: ['Liberia'], projects: [{t:'Liberia Port Ops', d:'Cargo management for trade corridors.'}] },
            { id: 'media', title: 'Media', color: '#db2777', textCol: '#831843', tagline: 'Brand Narrative', icon: '<rect x="10" y="10" width="60" height="60" stroke="#DB2777" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#F472B6" stroke-width="12"/>', vision: 'Global storytelling.', about: 'Nation branding and corporate strategy.', caps: ['Nation Branding Strategy', 'Crisis Comms', 'Digital Media Design', 'Creative Narrative'], leads: [{n:'CCO', r:'Tel Aviv Hub'}], locations: ['Israel', 'Bulgaria'], projects: [{t:'Global Brand Sync', d:'Unified rebranding across all nations.'}] },
            { id: 'foundation', title: 'Foundation', color: '#9333ea', textCol: '#581c87', tagline: 'Social Impact', icon: '<rect x="10" y="10" width="60" height="60" stroke="#7E22CE" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#A855F7" stroke-width="12"/>', vision: 'Investing in communities.', about: 'Health and educational philanthropy.', caps: ['Impact Assessment', 'CSR Strategy', 'Educational Grants', 'Health Planning'], leads: [{n:'Foundation Head', r:'Global'}], locations: ['Global'], projects: [{t:'STEM Scholarships', d:'Supporting engineering students in Georgia.'}] }
        ];

        function setLanguage(lang) {
            document.documentElement.lang = lang;
            const isRTL = (lang === 'he' || lang === 'ar');
            document.documentElement.dir = isRTL ? 'rtl' : 'ltr';
            document.body.classList.toggle('lang-rtl', isRTL);
            document.querySelectorAll('[data-i18n]').forEach(el => {
                const key = el.getAttribute('data-i18n');
                if (translations[lang] && translations[lang][key]) el.innerText = translations[lang][key];
            });
            ['en', 'he', 'fr', 'az', 'zh', 'ru', 'ar'].forEach(l => {
                const btn = document.getElementById('btn-' + l);
                if (btn) btn.className = l === lang ? 'lang-btn active' : 'lang-btn text-slate-400';
            });
            initGrid();
        }

        function toggleTheme() { document.getElementById('bodyTag').classList.toggle('dark-mode'); }

        function initGrid() {
            const container = document.getElementById('divisionsContainer');
            container.innerHTML = '';
            divisions.filter(d => d.id !== 'group').forEach(div => {
                const card = document.createElement('div');
                card.className = 'division-card group';
                card.onclick = () => showDivision(div.id);
                card.innerHTML = `
                    <div class="brand-unit">
                        <div class="w-16 h-16 mb-10"><svg viewBox="0 0 100 100" fill="none">${div.icon}</svg></div>
                        <div class="brand-name-prefix">Orizis</div>
                        <div class="brand-name-main" style="color: ${div.textCol}">${div.title}</div>
                        <p class="text-[10px] tracking-widest uppercase font-black text-slate-400 mt-10 group-hover:text-blue-500 transition">${div.tagline}</p>
                    </div>
                `;
                container.appendChild(card);
            });

            const locList = document.getElementById('locationsList');
            locList.innerHTML = '';
            locations.forEach(loc => {
                const item = document.createElement('div');
                item.className = 'flex items-center text-[12px] font-black text-slate-500 uppercase tracking-widest';
                const marginClass = document.documentElement.dir === 'rtl' ? 'ml-3' : 'mr-3';
                item.innerHTML = `<span class="location-dot ${marginClass}"></span> ${loc}`;
                locList.appendChild(item);
            });
        }

        function showDivision(id) {
            const div = divisions.find(d => d.id === id);
            document.getElementById('homePage').classList.add('section-hidden');
            document.getElementById('divisionPage').classList.remove('section-hidden');
            window.scrollTo({top: 0, behavior: 'instant'});

            document.getElementById('divLogoBox').innerHTML = `
                <div class="brand-unit scale-150">
                    <div class="w-20 h-20 mb-6"><svg viewBox="0 0 100 100" fill="none">${div.icon}</svg></div>
                    <div class="brand-name-prefix text-lg">Orizis</div>
                    <div class="brand-name-main" style="color: ${div.textCol}">${div.title}</div>
                </div>
            `;
            document.getElementById('divTitle').innerText = div.title; 
            document.getElementById('divVision').innerText = `"${div.vision}"`;
            document.getElementById('divAbout').innerText = div.about;

            document.getElementById('divCaps').innerHTML = div.caps.map(c => `
                <div class="p-8 bg-white dark:bg-white/5 rounded-3xl border border-slate-100 dark:border-white/10 shadow-sm transition-all hover:shadow-md">
                    <div class="w-10 h-1.5 rounded-full mb-6" style="background:${div.color}"></div>
                    <span class="text-sm font-black uppercase tracking-widest text-slate-800 dark:text-slate-200 leading-relaxed">${c}</span>
                </div>
            `).join('');

            document.getElementById('divTeam').innerHTML = div.leads.map(l => `
                <div class="pb-8 border-b border-slate-100 dark:border-white/10 text-left">
                    <div class="font-black text-2xl uppercase montserrat high-contrast-text">${l.n}</div>
                    <div class="text-[11px] text-blue-500 uppercase tracking-widest font-black mt-2">${l.r}</div>
                </div>
            `).join('');

            document.getElementById('divLocations').innerHTML = div.locations.map(loc => `
                <div class="flex items-center text-[13px] font-bold text-slate-500 uppercase">
                    <span class="location-dot"></span> ${loc}
                </div>
            `).join('');

            document.getElementById('divProjects').innerHTML = div.projects.map(p => `
                <div class="bg-slate-50 dark:bg-white/5 p-14 rounded-[3rem] border-0 shadow-xl">
                    <h4 class="font-black text-4xl mb-10 uppercase montserrat tracking-widest high-contrast-text">${p.t}</h4>
                    <p class="text-slate-500 text-2xl leading-relaxed font-light mb-12">${p.d}</p>
                    <div class="pt-10 border-t border-slate-200 dark:border-white/10 flex justify-between items-center text-[10px] font-black uppercase tracking-widest text-blue-500">
                        <span>Corporate Analysis</span>
                        <span>[→]</span>
                    </div>
                </div>
            `).join('');
        }

        function showHome() {
            document.getElementById('homePage').classList.remove('section-hidden');
            document.getElementById('divisionPage').classList.add('section-hidden');
            window.scrollTo({top: 0, behavior: 'smooth'});
        }

        initGrid();
        setLanguage('en');
    </script>
</body>
</html>
