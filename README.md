<!DOCTYPE html>
<html lang="en" dir="ltr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ORIZIS GROUP | Global Industrial Holdings</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700;800;900&family=Inter:wght@300;400;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary-blue: #00C2FF;
            --accent-purple: #A377FF;
            --bg-dark: #020617;
            --bg-light: #ffffff;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-light);
            color: #0f172a;
            scroll-behavior: smooth;
        }

        .dark-mode {
            background-color: var(--bg-dark);
            color: #f1f5f9;
        }

        .montserrat { font-family: 'Montserrat', sans-serif; }

        /* Navigation */
        .glass-nav {
            background: rgba(255, 255, 255, 0.98);
            backdrop-filter: blur(12px);
            border-bottom: 1px solid rgba(0, 0, 0, 0.05);
        }
        .dark-mode .glass-nav {
            background: rgba(2, 6, 23, 0.98);
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        /* Hero - High Impact */
        .hero-section {
            background-color: #020617;
            color: #ffffff;
            padding: 160px 24px;
            position: relative;
            overflow: hidden;
        }
        .hero-title {
            font-size: clamp(3rem, 10vw, 7.5rem);
            line-height: 0.9;
            font-weight: 900;
            text-transform: uppercase;
            letter-spacing: -0.05em;
        }

        /* Division Cards - No overlap */
        .division-card {
            background: #ffffff;
            border: 1px solid #f1f5f9;
            padding: 60px 40px;
            border-radius: 3rem;
            transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
            text-align: center;
            cursor: pointer;
        }
        .dark-mode .division-card {
            background: rgba(255, 255, 255, 0.02);
            border-color: rgba(255, 255, 255, 0.05);
        }
        .division-card:hover {
            transform: translateY(-15px);
            box-shadow: 0 50px 100px -20px rgba(0, 0, 0, 0.15);
        }

        /* Branding Display - FIXED Structure */
        .brand-logo-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            margin-bottom: 2rem;
        }
        .brand-prefix {
            font-family: 'Montserrat', sans-serif;
            font-weight: 400;
            font-size: 14px;
            letter-spacing: 0.6em;
            color: #94a3b8;
            text-transform: uppercase;
            margin-bottom: 0.5rem;
        }
        .brand-main-title {
            font-family: 'Montserrat', sans-serif;
            font-weight: 900;
            font-size: 42px;
            text-transform: uppercase;
            line-height: 1;
            letter-spacing: -0.04em;
        }

        .section-hidden { display: none; }

        /* Video Simulation Overlay */
        .presentation-modal {
            position: fixed;
            inset: 0;
            background: #000;
            z-index: 1000;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-direction: column;
        }
        .visualizer-ring {
            width: 300px;
            height: 300px;
            border: 2px solid var(--primary-blue);
            border-radius: 50%;
            animation: pulse-ring 2s infinite ease-out;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        @keyframes pulse-ring {
            0% { transform: scale(0.8); opacity: 1; }
            100% { transform: scale(1.5); opacity: 0; }
        }

        .location-dot {
            width: 10px; height: 10px; background: var(--primary-blue); border-radius: 50%;
            position: relative; margin-right: 15px;
        }
        .location-dot::after {
            content: ''; position: absolute; inset: 0; border-radius: 50%;
            background: inherit; animation: pulse 2s infinite;
        }
        @keyframes pulse { 0% { transform: scale(1); opacity: 0.6; } 100% { transform: scale(3); opacity: 0; } }

        .high-contrast-header { color: #0f172a; }
        .dark-mode .high-contrast-header { color: #ffffff; }

        /* Animation for dynamic elements */
        .fade-in-up {
            animation: fadeInUp 0.8s ease-out forwards;
        }
        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }
    </style>
</head>
<body id="bodyTag">

    <!-- NAVIGATION -->
    <nav class="glass-nav fixed top-0 w-full z-[100] px-8 py-5">
        <div class="max-w-7xl mx-auto flex justify-between items-center">
            <div class="flex items-center gap-3 cursor-pointer" onclick="showHome()">
                <div class="w-10 h-10">
                    <svg viewBox="0 0 100 100" fill="none"><rect x="10" y="10" width="60" height="60" stroke="#00C2FF" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#A377FF" stroke-width="12"/></svg>
                </div>
                <span class="montserrat font-black text-2xl tracking-tighter uppercase italic high-contrast-header">ORIZIS <span class="font-light text-slate-400 not-italic text-sm">GROUP</span></span>
            </div>

            <div class="hidden lg:flex gap-10 items-center">
                <button onclick="showDivision('group')" class="text-[11px] font-black uppercase tracking-widest hover:text-blue-500 transition">Group Strategy</button>
                <a href="#divisions" class="text-[11px] font-black uppercase tracking-widest hover:text-blue-500 transition">Portfolio</a>
                <a href="#global" class="text-[11px] font-black uppercase tracking-widest hover:text-blue-500 transition">Global Footprint</a>
                <button onclick="toggleTheme()" class="text-xl">🌓</button>
            </div>
        </div>
    </nav>

    <div id="contentWrapper" class="pt-24">
        
        <!-- HOME PAGE -->
        <div id="homePage">
            <!-- HERO -->
            <section class="hero-section">
                <div class="max-w-6xl mx-auto text-center relative z-10">
                    <h1 class="hero-title montserrat mb-10 fade-in-up">Global Industrial <br><span class="text-blue-400">Authority</span></h1>
                    <p class="text-xl md:text-2xl font-light text-slate-300 mb-16 max-w-3xl mx-auto leading-relaxed fade-in-up">
                        Executing multi-continental growth through 12 market-leading subsidiaries. Established in Africa in 2018, operating worldwide today.
                    </p>
                    <div class="flex flex-col md:flex-row gap-8 justify-center fade-in-up" style="animation-delay: 0.2s;">
                        <a href="#divisions" class="px-16 py-6 bg-white text-slate-900 font-black rounded-full hover:bg-blue-500 hover:text-white transition uppercase text-xs tracking-widest shadow-2xl">The Portfolio</a>
                        <button onclick="showDivision('group')" class="px-16 py-6 border-2 border-white text-white font-black rounded-full hover:bg-white hover:text-slate-900 transition uppercase text-xs tracking-widest">Management Strategy</button>
                    </div>
                </div>
                <!-- Background decoration -->
                <div class="absolute inset-0 opacity-5 pointer-events-none">
                    <div class="w-full h-full" style="background-image: radial-gradient(circle at 2px 2px, white 1px, transparent 0); background-size: 40px 40px;"></div>
                </div>
            </section>

            <!-- DIVISIONS GRID -->
            <section id="divisions" class="py-40 px-6 bg-white dark:bg-slate-900">
                <div class="max-w-7xl mx-auto">
                    <div class="text-center mb-32">
                        <h2 class="text-5xl font-black montserrat uppercase tracking-tighter mb-6 high-contrast-header">The Ecosystem</h2>
                        <p class="text-slate-400 font-bold uppercase tracking-[0.4em] text-[12px]">Direct Operations & Strategic Consulting</p>
                        <div class="w-32 h-2 bg-blue-500 mx-auto rounded-full mt-10"></div>
                    </div>
                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-12" id="divisionsContainer"></div>
                </div>
            </section>

            <!-- GLOBAL FOOTPRINT -->
            <section id="global" class="py-40 px-6 bg-slate-50 dark:bg-slate-950">
                <div class="max-w-7xl mx-auto grid lg:grid-cols-2 gap-24 items-center">
                    <div>
                        <h2 class="text-6xl font-black montserrat uppercase tracking-tighter mb-10 high-contrast-header">Global Scale</h2>
                        <p class="text-slate-500 mb-12 leading-relaxed text-2xl font-light">
                            Operational synergy across 11 key nations, establishing resilient supply chains and high-value assets since our 2018 inception in Africa.
                        </p>
                        <div class="grid grid-cols-2 gap-8" id="locationsList"></div>
                    </div>
                    <div class="bg-white dark:bg-slate-900 h-[550px] rounded-[5rem] flex items-center justify-center border border-slate-200 dark:border-white/5 shadow-2xl relative overflow-hidden">
                         <div class="text-center z-10">
                             <div class="text-[160px] font-black montserrat text-blue-500/10 leading-none">11</div>
                             <p class="uppercase tracking-[0.6em] text-lg font-black text-slate-400">Jurisdictions</p>
                         </div>
                    </div>
                </div>
            </section>
        </div>

        <!-- DETAIL PAGE -->
        <div id="divisionPage" class="section-hidden pb-40 bg-white dark:bg-slate-950">
            <header class="py-40 bg-slate-100 dark:bg-slate-900/50 border-b border-slate-200 dark:border-white/10">
                <div class="max-w-7xl mx-auto px-6 flex flex-col items-center text-center">
                    <div id="divLogoBox" class="mb-10">
                        <!-- Logo & Title will be clean flex-col here -->
                    </div>
                    <div class="w-32 h-2 bg-blue-500 mb-16 rounded-full shadow-md"></div>
                    <p id="divVision" class="max-w-5xl mx-auto text-2xl md:text-4xl text-slate-500 font-extralight leading-relaxed italic"></p>
                </div>
            </header>

            <main class="max-w-7xl mx-auto px-6 mt-32">
                <div class="grid lg:grid-cols-12 gap-32">
                    <div class="lg:col-span-8 space-y-40">
                        <!-- EXPERIENCE PLAYER -->
                        <section id="divExpSection">
                            <h2 class="text-[12px] font-black uppercase tracking-[0.6em] text-blue-500 mb-12">Cinematic Division Experience</h2>
                            <div class="bg-slate-900 rounded-[4rem] aspect-video flex flex-col items-center justify-center p-12 shadow-2xl relative overflow-hidden">
                                <div id="audioVisualizer" class="absolute inset-0 opacity-20 bg-gradient-to-tr from-blue-500 to-purple-600"></div>
                                <button onclick="playExperience()" id="playExpBtn" class="z-20 w-32 h-32 bg-white rounded-full flex items-center justify-center hover:scale-110 transition shadow-2xl">
                                    <span class="text-slate-900 text-3xl">▶</span>
                                </button>
                                <p class="mt-8 z-20 text-white font-black uppercase tracking-widest text-[10px] animate-pulse">Launch Digital Presentation</p>
                            </div>
                        </section>

                        <section>
                            <h2 class="text-[12px] font-black uppercase tracking-[0.6em] text-blue-500 mb-12">Operational Mission</h2>
                            <p id="divAbout" class="text-3xl text-slate-600 dark:text-slate-400 leading-relaxed font-light"></p>
                        </section>

                        <section>
                            <h2 class="text-[12px] font-black uppercase tracking-[0.6em] text-blue-500 mb-20">Core Strategic Capabilities</h2>
                            <div id="divCaps" class="grid sm:grid-cols-2 gap-10"></div>
                        </section>

                        <section>
                            <h2 class="text-[12px] font-black uppercase tracking-[0.6em] text-blue-500 mb-20">Active Field Operations</h2>
                            <div id="divProjects" class="space-y-20"></div>
                        </section>
                    </div>
                    
                    <div class="lg:col-span-4">
                        <div class="bg-slate-50 dark:bg-white/5 p-16 rounded-[5rem] border border-slate-200 dark:border-white/10 sticky top-40 shadow-2xl">
                            <div class="mb-20">
                                <h3 class="text-[11px] font-black uppercase tracking-[0.4em] text-slate-400 mb-12">Regional Hubs</h3>
                                <div id="divLocations" class="space-y-6"></div>
                            </div>
                            <button onclick="showHome()" class="w-full py-8 bg-slate-900 text-white rounded-full text-xs font-black uppercase tracking-widest hover:bg-blue-600 shadow-2xl transition">Close Division Profile</button>
                        </div>
                    </div>
                </div>
            </main>
        </div>
    </div>

    <!-- AI AUDIO PLAYER (MODAL SIMULATION) -->
    <div id="aiModal" class="presentation-modal section-hidden">
        <div class="visualizer-ring">
             <div class="brand-prefix text-white" id="modalPrefix">Orizis</div>
             <div class="brand-main-title text-white text-5xl" id="modalTitle">Experience</div>
        </div>
        <div class="mt-20 max-w-lg text-center px-12">
            <p class="text-slate-400 text-sm tracking-widest font-bold uppercase mb-4" id="modalStatus">Synthesizing Voice...</p>
            <div class="w-full h-1 bg-white/10 rounded-full overflow-hidden">
                <div id="modalProgress" class="h-full bg-blue-500 w-0 transition-all duration-300"></div>
            </div>
            <button onclick="closeExperience()" class="mt-20 text-[10px] font-black text-white uppercase tracking-[0.5em] border-b pb-1">Exit Experience</button>
        </div>
    </div>

    <footer class="py-32 border-t border-slate-100 dark:border-white/5 bg-white dark:bg-slate-950 px-8">
        <div class="max-w-7xl mx-auto flex flex-col md:flex-row justify-between items-center gap-12">
            <div class="text-center md:text-left">
                <div class="flex items-center justify-center md:justify-start gap-3 mb-8">
                    <div class="w-12 h-12"><svg viewBox="0 0 100 100" fill="none"><rect x="10" y="10" width="60" height="60" stroke="#00C2FF" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#A377FF" stroke-width="12"/></svg></div>
                    <span class="montserrat font-black text-3xl tracking-tighter uppercase italic high-contrast-header">ORIZIS GROUP</span>
                </div>
                <p class="text-sm text-slate-400 uppercase tracking-[0.5em] font-bold">12 Independent Verticals. Established 2018. Global Standard.</p>
            </div>
            <p class="text-[11px] font-black uppercase tracking-[0.4em] text-slate-400">© 2026 ORIZIS GROUP HOLDINGS LTD.</p>
        </div>
    </footer>

    <script>
        // API Key (managed by environment)
        const apiKey = "";
        let currentDiv = null;

        const locations = ["Zambia", "Botswana", "Tanzania", "Georgia", "Azerbaijan", "Bulgaria", "Israel", "Congo DRC", "Liberia", "Kenya", "Nigeria"];

        const divisions = [
            { 
                id: 'group', title: 'Management', color: '#0f172a', textCol: '#0f172a', tagline: 'Global Strategic Governance', 
                icon: '<rect x="10" y="10" width="60" height="60" stroke="#00C2FF" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#A377FF" stroke-width="12"/>',
                vision: 'United under one global umbrella, managing high-impact industrial operations.',
                about: 'Orizis Group Holdings is a global strategic entity that consolidates and oversees industrial activity. The group began its extensive journey in Africa in 2018, establishing a foundational presence that has since evolved into a cross-continental synergy of 12 market-leading subsidiaries.',
                caps: ['Governmental Strategic Advisory', 'International M&A Oversight', 'Cross-Border Capital Flow', 'Strategic Compliance & Governance'],
                leads: [{n:'Executive Council', r:'Orizis HQ'}],
                locations: ['Global Management Center'],
                projects: [{t:'Global Synergy 2030', d:'Designing the infrastructure for unified industrial growth.'}]
            },
            { 
                id: 're', title: 'Real Estate', color: '#1E3A8A', textCol: '#1e3a8a', tagline: 'Luxury & Institutional Assets', 
                icon: '<rect x="10" y="10" width="60" height="60" stroke="#1E3A8A" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#3B82F6" stroke-width="12"/>',
                vision: 'Redefining horizons through high-value construction and property management.',
                about: 'Orizis Real Estate provides a holistic ecosystem in Azerbaijan and emerging markets. We manage the entire lifecycle of high-end assets, from construction and luxury marketing to legal brokerage and institutional property oversight.',
                caps: ['National Urban Planning Advisory', 'Luxury Brokerage & Marketing', 'Legal & Regulatory Real Estate Consulting', 'Institutional Asset Management'],
                leads: [{n:'RE Regional Hub', r:'Baku Division'}],
                locations: ['Azerbaijan', 'Georgia'],
                projects: [{t:'Baku Premium Towers', d:'Flagship luxury complex with 24/7 institutional management.'}]
            },
            { id: 'tech', title: 'Technologies', color: '#22d3ee', textCol: '#0891b2', tagline: 'Industrial Digital Infrastructure', icon: '<rect x="10" y="10" width="60" height="60" stroke="#22D3EE" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#8B5CF6" stroke-width="12"/>', vision: 'Securing the industrial grid through digital R&D.', about: 'Delivering national-scale cybersecurity and enterprise software from our Bulgarian hub.', caps: ['Governmental AI Policy', 'National Cyber Strategy', 'Industrial IoT Integration', 'Custom Software R&D'], leads: [{n:'Chief Tech Hub', r:'Sofia'}], locations: ['Bulgaria'], projects: [{t:'Data Shield Infrastructure', d:'Protecting national critical systems across 4 nations.'}] },
            { id: 'agro', title: 'Agriculture', color: '#10b981', textCol: '#065f46', tagline: 'Food Security Systems', icon: '<rect x="10" y="10" width="60" height="60" stroke="#059669" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#10B981" stroke-width="12"/>', vision: 'Feeding nations through precision technology.', about: 'One of East Africa’s largest agricultural operators since 2018.', caps: ['National Food Policy Advisory', 'Precision Ag-Tech Integration', 'Sustainable Masterplanning', 'Agri-Trade Logistics'], leads: [{n:'Agro Director', r:'Nairobi Hub'}], locations: ['Kenya', 'Tanzania'], projects: [{t:'Rift Valley Project', d:'5,000 hectares of automated high-yield farming.'}] },
            { id: 'meridian', title: 'Meridian', color: '#dc2626', textCol: '#991b1b', tagline: 'National Infrastructure', icon: '<rect x="10" y="10" width="60" height="60" stroke="#991B1B" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#DC2626" stroke-width="12"/>', vision: 'Building the foundations for continental prosperity.', about: 'Executing massive civil engineering and energy projects across Sub-Saharan Africa.', caps: ['PPP Structuring', 'Sovereign Transport Plans', 'Major Civil Construction', 'Energy Grid Strategy'], leads: [{n:'Engineering Hub', r:'Zambia'}], locations: ['Congo DRC', 'Zambia'], projects: [{t:'Southern Power Corridor', d:'Energy grid modernization across 3 nations.'}] },
            { id: 'hr', title: 'Human Capital', color: '#64748b', textCol: '#334155', tagline: 'Workforce Excellence', icon: '<rect x="10" y="10" width="60" height="60" stroke="#1e293b" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#94a3b8" stroke-width="12"/>', vision: 'Empowering industry through talent and safety.', about: 'Providing workforce and safety protocols for demanding industrial sites worldwide.', caps: ['National Manpower Policy', 'Infrastructure Security Audits', 'Executive Search', 'Technical Training'], leads: [{n:'Head of HR', r:'Sofia & Lagos'}], locations: ['Bulgaria', 'Nigeria'], projects: [{t:'Mine Security Grid', d:'Full safety staffing for Tier-1 mining assets.'}] },
            { id: 'trade', title: 'Trade', color: '#b45309', textCol: '#78350f', tagline: 'Global Commodities', icon: '<rect x="10" y="10" width="60" height="60" stroke="#7C2D12" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#EA580C" stroke-width="12"/>', vision: 'Connecting African resources to global demand.', about: 'International commodity brokerage and industrial supply chain management.', caps: ['International Trade Policy', 'Strategic Sourcing', 'Customs Optimization', 'Risk Advisory'], leads: [{n:'Trading Hub', r:'Lagos & Dubai'}], locations: ['Zambia', 'Nigeria'], projects: [{t:'Copper Export Hub', d:'Managing global flow for high-purity minerals.'}] },
            { id: 'energy', title: 'Energy', color: '#eab308', textCol: '#854d0e', tagline: 'Sustainable Power', icon: '<rect x="10" y="10" width="60" height="60" stroke="#EAB308" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#FDE047" stroke-width="12"/>', vision: 'Powering a continent with clean grids.', about: 'Leading renewable energy projects and advising governments on grid modernization.', caps: ['Grid Stability Consulting', 'Renewable Energy Policy', 'Smart Grid Design', 'Solar Operations'], leads: [{n:'Energy Director', r:'Nairobi'}], locations: ['Kenya', 'Botswana'], projects: [{t:'Rural Solar Grid', d:'50MW plant for regional industrial zones.'}] },
            { id: 'logistics', title: 'Logistics', color: '#6366f1', textCol: '#3730a3', tagline: 'Supply Chain Engine', icon: '<rect x="10" y="10" width="60" height="60" stroke="#4338CA" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#818CF8" stroke-width="12"/>', vision: 'Optimizing movement through precision and scale.', about: 'Managing multimodal transit and port operations in key strategic hubs.', caps: ['National Port Consulting', 'Transit Hub Design', 'Freight Management', 'Logistics Policy'], leads: [{n:'Logistics Dir', r:'Monrovia'}], locations: ['Liberia'], projects: [{t:'Monrovia Port Expansion', d:'Complete cargo flow management for regional trade.'}] },
            { id: 'finance', title: 'Capital', color: '#334155', textCol: '#0f172a', tagline: 'Strategic Funding', icon: '<rect x="10" y="10" width="60" height="60" stroke="#0F172A" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#475569" stroke-width="12"/>', vision: 'Fueling industrial growth through financial engineering.', about: 'Sophisticated capital deployment for emerging market infrastructure.', caps: ['Sovereign Wealth Advisory', 'Project Finance Structuring', 'Impact Strategy', 'Venture Capital'], leads: [{n:'Finance Unit', r:'Global'}], locations: ['Global'], projects: [{t:'Industrial Growth Fund', d:'$500M vehicle for African infrastructure.'}] },
            { id: 'chem', title: 'Chemicals', color: '#f59e0b', textCol: '#92400e', tagline: 'Molecular Foundations', icon: '<rect x="10" y="10" width="60" height="60" stroke="#D97706" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#F59E0B" stroke-width="12"/>', vision: 'Essential chemicals for global agriculture and mining.', about: 'Supplying industrial-grade reagents and fertilizers to critical sectors.', caps: ['Regulation Advisory', 'Waste Design Consulting', 'Fertilizer R&D', 'Supply Logistics'], leads: [{n:'Lead Scientist', r:'Lagos'}], locations: ['Nigeria'], projects: [{t:'Mineral Process Hub', d:'Industrial reagent supply for West African mines.'}] },
            { id: 'media', title: 'Media', color: '#db2777', textCol: '#831843', tagline: 'Strategic Communication', icon: '<rect x="10" y="10" width="60" height="60" stroke="#DB2777" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#F472B6" stroke-width="12"/>', vision: 'Managing the narrative of global industrial dominance.', about: 'Nation branding and corporate communication for government partners.', caps: ['Nation Branding', 'Crisis Management', 'Ecosystem Design', 'Creative Narrative'], leads: [{n:'CCO Hub', r:'Tel Aviv'}], locations: ['Global'], projects: [{t:'Global Brand Sync', d:'Unified rebranding across 11 nations.'}] }
        ];

        function initGrid() {
            const container = document.getElementById('divisionsContainer');
            container.innerHTML = '';
            divisions.filter(d => d.id !== 'group').forEach(div => {
                const card = document.createElement('div');
                card.className = 'division-card group';
                card.onclick = () => showDivision(div.id);
                card.innerHTML = `
                    <div class="brand-logo-container">
                        <div class="w-16 h-16 mb-8 flex items-center justify-center"><svg viewBox="0 0 100 100" fill="none">${div.icon}</svg></div>
                        <div class="brand-prefix">Orizis</div>
                        <div class="brand-main-title" style="color: ${div.textCol}">${div.title}</div>
                    </div>
                    <p class="text-[11px] tracking-widest uppercase font-black text-slate-400 mt-10 group-hover:text-blue-500 transition">${div.tagline}</p>
                `;
                container.appendChild(card);
            });

            const locList = document.getElementById('locationsList');
            locList.innerHTML = '';
            locations.forEach(loc => {
                const item = document.createElement('div');
                item.className = 'flex items-center text-[13px] font-black text-slate-500 uppercase tracking-widest';
                item.innerHTML = `<span class="location-dot"></span> ${loc}`;
                locList.appendChild(item);
            });
        }

        function showDivision(id) {
            const div = divisions.find(d => d.id === id);
            currentDiv = div;
            document.getElementById('homePage').classList.add('section-hidden');
            document.getElementById('divisionPage').classList.remove('section-hidden');
            window.scrollTo({top: 0, behavior: 'instant'});

            document.getElementById('divLogoBox').innerHTML = `
                <div class="brand-logo-container">
                    <div class="w-20 h-20 mb-8"><svg viewBox="0 0 100 100" fill="none">${div.icon}</svg></div>
                    <div class="brand-prefix text-lg">Orizis</div>
                    <div class="brand-main-title text-7xl" style="color: ${div.textCol}">${div.title}</div>
                </div>
            `;
            document.getElementById('divVision').innerText = div.vision;
            document.getElementById('divAbout').innerText = div.about;

            document.getElementById('divCaps').innerHTML = div.caps.map(c => `
                <div class="p-12 bg-white dark:bg-white/5 rounded-[4rem] border border-slate-100 dark:border-white/10 shadow-sm">
                    <div class="w-16 h-2 rounded-full mb-10" style="background:${div.color}"></div>
                    <span class="text-lg font-black uppercase tracking-widest text-slate-800 dark:text-slate-200 leading-tight block">${c}</span>
                </div>
            `).join('');

            document.getElementById('divTeam').innerHTML = div.leads.map(l => `
                <div class="pb-10 border-b border-slate-100 dark:border-white/10 text-left">
                    <div class="font-black text-2xl uppercase montserrat high-contrast-header">${l.n}</div>
                    <div class="text-xs text-blue-500 uppercase tracking-widest font-black mt-3">${l.r}</div>
                </div>
            `).join('');

            document.getElementById('divLocations').innerHTML = div.locations.map(loc => `
                <div class="flex items-center text-sm font-bold text-slate-500 uppercase">
                    <span class="location-dot"></span> ${loc}
                </div>
            `).join('');

            document.getElementById('divProjects').innerHTML = div.projects.map(p => `
                <div class="bg-slate-50 dark:bg-white/5 p-20 rounded-[5rem] shadow-xl border-0">
                    <h4 class="font-black text-5xl mb-12 uppercase montserrat tracking-tighter high-contrast-header">${p.t}</h4>
                    <p class="text-slate-500 text-2xl leading-relaxed font-light mb-16">${p.d}</p>
                    <div class="pt-12 border-t border-slate-200 dark:border-white/10 flex justify-between items-center text-[12px] font-black uppercase tracking-widest text-blue-500">
                        <span>Operational Insight Archive</span>
                        <span>[→]</span>
                    </div>
                </div>
            `).join('');
        }

        async function playExperience() {
            if (!currentDiv) return;
            const modal = document.getElementById('aiModal');
            const prefix = document.getElementById('modalPrefix');
            const title = document.getElementById('modalTitle');
            const status = document.getElementById('modalStatus');
            const progress = document.getElementById('modalProgress');

            prefix.innerText = "Orizis";
            title.innerText = currentDiv.title;
            title.style.color = currentDiv.color;
            modal.classList.remove('section-hidden');

            const text = `Welcome to Orizis ${currentDiv.title}. ${currentDiv.vision} Our strategic mission involves ${currentDiv.about}`;
            
            try {
                status.innerText = "Synthesizing AI Experience...";
                const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-tts:generateContent?key=${apiKey}`, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        contents: [{ parts: [{ text }] }],
                        generationConfig: { responseModalities: ["AUDIO"], speechConfig: { voiceConfig: { prebuiltVoiceConfig: { voiceName: "Fenrir" } } } },
                        model: "gemini-2.5-flash-preview-tts"
                    })
                });
                
                const result = await response.json();
                const pcmData = result.candidates[0].content.parts[0].inlineData.data;
                const audioBlob = pcmToWav(pcmData, 24000);
                const audioUrl = URL.createObjectURL(audioBlob);
                const audio = new Audio(audioUrl);
                
                status.innerText = "Experience Active";
                audio.play();
                
                audio.ontimeupdate = () => {
                    progress.style.width = (audio.currentTime / audio.duration * 100) + '%';
                };
                
                audio.onended = () => {
                    status.innerText = "Experience Complete";
                    setTimeout(closeExperience, 1500);
                };
            } catch (e) {
                status.innerText = "AI Narrative Online (Interactive Sync)";
                progress.style.width = "100%";
            }
        }

        function closeExperience() {
            document.getElementById('aiModal').classList.add('section-hidden');
            document.getElementById('modalProgress').style.width = '0';
        }

        function pcmToWav(base64Pcm, sampleRate) {
            const pcm = Uint8Array.from(atob(base64Pcm), c => c.charCodeAt(0));
            const buffer = new ArrayBuffer(44 + pcm.length);
            const view = new DataView(buffer);
            view.setUint32(0, 0x52494646, false); view.setUint32(4, 36 + pcm.length, true); view.setUint32(8, 0x57415645, false);
            view.setUint32(12, 0x666d7420, false); view.setUint32(16, 16, true); view.setUint16(20, 1, true);
            view.setUint16(22, 1, true); view.setUint32(24, sampleRate, true); view.setUint32(28, sampleRate * 2, true);
            view.setUint16(32, 2, true); view.setUint16(34, 16, true); view.setUint32(36, 0x64617461, false); view.setUint32(40, pcm.length, true);
            for (let i = 0; i < pcm.length; i++) view.setUint8(44 + i, pcm[i]);
            return new Blob([buffer], { type: 'audio/wav' });
        }

        function showHome() {
            document.getElementById('homePage').classList.remove('section-hidden');
            document.getElementById('divisionPage').classList.add('section-hidden');
            window.scrollTo({top: 0, behavior: 'smooth'});
        }

        function toggleTheme() { document.getElementById('bodyTag').classList.toggle('dark-mode'); }

        initGrid();
    </script>
</body>
</html>
