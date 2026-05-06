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
            --bg-light: #ffffff;
            --bg-dark: #020617;
            --text-main: #0f172a;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-light);
            color: var(--text-main);
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
            backdrop-filter: blur(15px);
            border-bottom: 1px solid rgba(0, 0, 0, 0.05);
        }
        .dark-mode .glass-nav {
            background: rgba(2, 6, 23, 0.98);
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        /* Hero - GUARANTEED WHITE TEXT ON DARK BACKGROUND */
        .hero-section {
            background-color: #020617 !important; /* Forces dark background */
            padding: clamp(140px, 20vh, 250px) 24px;
            position: relative;
            overflow: hidden;
            text-align: center;
        }
        .hero-title {
            color: #ffffff !important; /* Forces white text unconditionally */
            font-size: clamp(3rem, 8vw, 7rem);
            line-height: 0.95;
            font-weight: 900;
            text-transform: uppercase;
            letter-spacing: -0.02em;
        }
        .hero-desc {
            color: #cbd5e1 !important; /* Light slate text for readability */
        }

        /* Portfolio Grid Cards */
        .division-card {
            background: #ffffff;
            border: 1px solid #f1f5f9;
            padding: 50px 24px;
            border-radius: 2.5rem;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            text-align: center;
            cursor: pointer;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.02);
            width: 100%;
        }
        .dark-mode .division-card {
            background: rgba(255, 255, 255, 0.02);
            border-color: rgba(255, 255, 255, 0.05);
        }
        .division-card:hover {
            transform: translateY(-10px);
            box-shadow: 0 50px 80px -20px rgba(0, 0, 0, 0.15);
        }

        /* Branding Block - NO OVERLAP, PREVENT WORD BREAKING */
        .brand-block {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 1.5rem; /* Safe distance between icon and text */
            width: 100%;
        }
        .brand-identity {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 0.5rem;
            width: 100%;
        }
        .brand-pfx {
            font-family: 'Montserrat', sans-serif;
            font-weight: 600;
            font-size: 12px;
            letter-spacing: 0.4em;
            color: #94a3b8;
            text-transform: uppercase;
        }
        .brand-main {
            font-family: 'Montserrat', sans-serif;
            font-weight: 900; 
            font-size: clamp(18px, 1.8vw, 28px); /* Perfectly scaled to fit long words */
            text-transform: uppercase;
            line-height: 1.1;
            letter-spacing: -0.02em;
            width: 100%;
            /* The following ensures words like FOUNDATION stay intact */
            word-break: normal; 
            overflow-wrap: normal;
            hyphens: none;
        }

        .section-hidden { display: none; }

        .location-dot {
            width: 10px; height: 10px; background: var(--primary-blue); border-radius: 50%;
            position: relative; margin-right: 15px;
        }
        .location-dot::after {
            content: ''; position: absolute; inset: 0; border-radius: 50%;
            background: inherit; animation: pulse 2s infinite;
        }
        @keyframes pulse { 0% { transform: scale(1); opacity: 0.6; } 100% { transform: scale(3); opacity: 0; } }

        /* Media Placeholder */
        .media-placeholder {
            background: #f8fafc;
            border: 1px dashed #e2e8f0;
            border-radius: 3rem;
            min-height: 400px;
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
        }
        .dark-mode .media-placeholder {
            background: #0f172a;
            border-color: #1e293b;
        }

        /* High Contrast Support */
        .high-contrast-title { color: #0f172a; }
        .dark-mode .high-contrast-title { color: #ffffff; }
    </style>
</head>
<body id="bodyTag">

    <!-- NAVIGATION -->
    <nav class="glass-nav fixed top-0 w-full z-[100] px-6 md:px-10 py-5">
        <div class="max-w-7xl mx-auto flex justify-between items-center">
            <div class="flex items-center gap-3 cursor-pointer" onclick="showHome()">
                <div class="w-10 h-10">
                    <svg viewBox="0 0 100 100" fill="none"><rect x="10" y="10" width="60" height="60" stroke="#00C2FF" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#A377FF" stroke-width="12"/></svg>
                </div>
                <span class="montserrat font-black text-xl md:text-2xl tracking-tighter uppercase italic high-contrast-title">ORIZIS <span class="font-light text-slate-400 not-italic text-sm">GROUP</span></span>
            </div>

            <div class="hidden lg:flex gap-12 items-center">
                <button onclick="showDivision('group')" class="text-[11px] font-black uppercase tracking-widest hover:text-blue-500 transition">Group Strategy</button>
                <a href="#divisions" class="text-[11px] font-black uppercase tracking-widest hover:text-blue-500 transition">Portfolio</a>
                <button onclick="toggleTheme()" class="text-2xl ml-4">🌓</button>
            </div>
        </div>
    </nav>

    <div id="contentWrapper" class="pt-24">
        
        <!-- HOME PAGE -->
        <div id="homePage">
            <!-- HERO -->
            <section class="hero-section">
                <div class="max-w-6xl mx-auto">
                    <h1 class="hero-title montserrat mb-10">Global Industrial <br><span class="text-blue-500">Authority</span></h1>
                    <p class="hero-desc text-xl md:text-3xl font-light mb-16 max-w-4xl mx-auto leading-relaxed">
                        Established in Africa in 2018, Orizis Group governs 12 specialized subsidiaries across multiple continents, driving industrial progress through strategic synergy.
                    </p>
                    <div class="flex flex-col md:flex-row gap-6 justify-center">
                        <a href="#divisions" class="px-16 py-6 bg-white text-slate-900 font-black rounded-full hover:bg-blue-600 hover:text-white transition uppercase text-xs tracking-widest shadow-2xl">Subsidiary Verticals</a>
                        <button onclick="showDivision('group')" class="px-16 py-6 border-2 border-white text-white font-black rounded-full hover:bg-white hover:text-slate-900 transition uppercase text-xs tracking-widest">Global Governance</button>
                    </div>
                </div>
            </section>

            <!-- PORTFOLIO GRID -->
            <section id="divisions" class="py-40 px-6 bg-white dark:bg-slate-900">
                <div class="max-w-7xl mx-auto">
                    <div class="text-center mb-24">
                        <h2 class="text-5xl font-black montserrat uppercase tracking-tighter mb-6 high-contrast-title">The Ecosystem</h2>
                        <p class="text-slate-400 font-bold uppercase tracking-[0.5em] text-[12px]">12 Specialized Market Leaders</p>
                        <div class="w-32 h-2.5 bg-blue-500 mx-auto rounded-full mt-10"></div>
                    </div>
                    <!-- Exactly 12 Subsidiaries injected here via JS -->
                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8" id="divisionsContainer">
                    </div>
                </div>
            </section>

            <!-- FOOTPRINT -->
            <section id="global" class="py-40 px-6 bg-slate-50 dark:bg-slate-950">
                <div class="max-w-7xl mx-auto grid lg:grid-cols-2 gap-32 items-center">
                    <div>
                        <h2 class="text-6xl md:text-7xl font-black montserrat uppercase tracking-tighter mb-12 high-contrast-title">Global Reach</h2>
                        <p class="text-slate-500 mb-16 leading-relaxed text-xl md:text-2xl font-light">
                            Operational presence in 11 key nations, managing physical industrial assets and resilient supply chains established since 2018.
                        </p>
                        <div class="grid grid-cols-2 gap-8" id="locationsList">
                            <!-- LOCATIONS INJECTED BY JS -->
                        </div>
                    </div>
                    <div class="bg-white dark:bg-slate-900 h-[500px] md:h-[600px] rounded-[4rem] flex items-center justify-center border border-slate-200 dark:border-white/5 shadow-2xl">
                         <div class="text-center">
                             <div class="text-[140px] md:text-[180px] font-black montserrat text-blue-500/10 leading-none">11</div>
                             <p class="uppercase tracking-[0.7em] text-sm md:text-xl font-black text-slate-400">Jurisdictions</p>
                         </div>
                    </div>
                </div>
            </section>
        </div>

        <!-- DETAIL PAGE -->
        <div id="divisionPage" class="section-hidden pb-48 bg-white dark:bg-slate-950">
            <header class="py-40 bg-slate-50 dark:bg-slate-900/50 border-b border-slate-200 dark:border-white/10">
                <div class="max-w-7xl mx-auto px-6 flex flex-col items-center text-center" id="divHeaderBox"></div>
            </header>

            <main class="max-w-7xl mx-auto px-6 mt-40">
                <div class="grid lg:grid-cols-12 gap-32">
                    <div class="lg:col-span-8 space-y-40">
                        
                        <section>
                            <h2 class="text-[12px] font-black uppercase tracking-[0.8em] text-blue-500 mb-12">Operational Overview</h2>
                            <p id="divAbout" class="text-3xl md:text-4xl text-slate-600 dark:text-slate-400 leading-relaxed font-light"></p>
                        </section>

                        <!-- Clean Media Placeholder (No Video) -->
                        <section>
                            <h2 class="text-[12px] font-black uppercase tracking-[0.8em] text-blue-500 mb-16">Industrial Showcase</h2>
                            <div class="media-placeholder">
                                <span class="text-slate-300 font-black uppercase tracking-[1em] text-xs">[ Cinematic Division Reel Placeholder ]</span>
                            </div>
                        </section>

                        <section>
                            <h2 class="text-[12px] font-black uppercase tracking-[0.8em] text-blue-500 mb-16">Strategic Capabilities</h2>
                            <div id="divCaps" class="grid sm:grid-cols-2 gap-8"></div>
                        </section>

                        <section>
                            <h2 class="text-[12px] font-black uppercase tracking-[0.8em] text-blue-500 mb-16">Active Projects</h2>
                            <div id="divProjects" class="space-y-16"></div>
                        </section>
                    </div>
                    
                    <div class="lg:col-span-4">
                        <div class="bg-slate-50 dark:bg-white/5 p-12 md:p-16 rounded-[4rem] border border-slate-200 dark:border-white/10 sticky top-40 shadow-2xl">
                            <div class="mb-20">
                                <h3 class="text-[11px] font-black uppercase tracking-[0.5em] text-slate-400 mb-10">Leadership & Hubs</h3>
                                <div id="divTeam" class="space-y-8"></div>
                            </div>
                            <button onclick="showHome()" class="w-full py-8 bg-slate-900 text-white rounded-full text-[10px] font-black uppercase tracking-widest hover:bg-blue-600 shadow-xl transition">Close Division Profile</button>
                        </div>
                    </div>
                </div>
            </main>
        </div>
    </div>

    <footer class="py-32 border-t border-slate-100 dark:border-white/5 bg-white dark:bg-slate-950 px-8">
        <div class="max-w-7xl mx-auto flex flex-col md:flex-row justify-between items-center gap-12">
            <div class="text-center md:text-left">
                <div class="flex items-center justify-center md:justify-start gap-4 mb-8">
                    <div class="w-12 h-12"><svg viewBox="0 0 100 100" fill="none"><rect x="10" y="10" width="60" height="60" stroke="#00C2FF" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#A377FF" stroke-width="12"/></svg></div>
                    <span class="montserrat font-black text-3xl tracking-tighter uppercase italic high-contrast-title">ORIZIS GROUP</span>
                </div>
                <p class="text-xs text-slate-400 uppercase tracking-[0.6em] font-bold">12 Global Subsidiaries. Established Africa 2018. Industrial Power.</p>
            </div>
            <p class="text-[10px] font-black uppercase tracking-[0.4em] text-slate-400">© 2026 ORIZIS GROUP HOLDINGS LTD.</p>
        </div>
    </footer>

    <script>
        const locations = ["Zambia", "Botswana", "Tanzania", "Georgia", "Azerbaijan", "Bulgaria", "Israel", "Congo DRC", "Liberia", "Kenya", "Nigeria"];

        // EXACTLY 13 Entries: 1 Group (Management) + 12 Subsidiaries
        const divisions = [
            { 
                id: 'group', title: 'Management', color: '#0f172a', textCol: '#0f172a', tagline: 'Unified Strategic Umbrella', 
                icon: '<rect x="10" y="10" width="60" height="60" stroke="#00C2FF" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#A377FF" stroke-width="12"/>',
                vision: 'United under one global umbrella, managing high-impact international activities.',
                about: 'Orizis Group Holdings serves as the overarching international umbrella, coordinating and governing diversified industrial activity across multiple continents. Established in Africa in 2018, the group focuses on large-scale asset management, high-level governance, and creating synergy between its 12 market-leading subsidiaries. We act as the strategic bridge between emerging resources and global capital.',
                caps: ['Governmental Strategic Advisory', 'International Portfolio Governance', 'Cross-Border Capital Deployment', 'Regulatory Compliance & Strategy', 'Multi-National Execution Oversight'],
                leads: [{n:'Executive Council', r:'Orizis HQ Hub'}],
                projects: [{t:'Global Synergy Roadmap', d:'Designing the infrastructure for unified growth across 11 nations.'}]
            },
            { 
                id: 're', title: 'Real Estate', color: '#1E3A8A', textCol: '#1a2e5a', tagline: '360° Property Ecosystem', 
                icon: '<rect x="10" y="10" width="60" height="60" stroke="#1E3A8A" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#3B82F6" stroke-width="12"/>',
                vision: 'Transforming landscapes through luxury construction and asset management.',
                about: 'Orizis Real Estate provides an end-to-end value chain in Azerbaijan and Georgia. We manage high-end assets from construction and design to luxury marketing, legal brokerage, and institutional property management.',
                caps: ['Governmental Urban Planning', 'Luxury Asset Marketing Agency', 'Real Estate Legal Support', 'Institutional Property Management', 'Brokerage & Transaction Advisory'],
                leads: [{n:'RE Director', r:'Baku Division'}],
                projects: [{t:'Baku Premium Towers', d:'Luxury complex under 24/7 institutional Orizis management.'}]
            },
            { 
                id: 'tech', title: 'Technologies', color: '#22d3ee', textCol: '#0891b2', tagline: 'Industrial Digital Shield', 
                icon: '<rect x="10" y="10" width="60" height="60" stroke="#22D3EE" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#8B5CF6" stroke-width="12"/>', 
                vision: 'Digital R&D powering global operations.', 
                about: 'Deploying cybersecurity and industrial IoT from our Bulgarian hub to protect national-scale infrastructures and group-wide data synchronization.', 
                caps: ['National Cyber Strategy', 'Industrial IoT System Design', 'Governmental AI Policy', 'Enterprise Software R&D', 'Strategic IT Infrastructure'], 
                leads: [{n:'CTO Sofia', r:'R&D Division'}], 
                projects: [{t:'Data Shield Infrastructure', d:'Protecting national critical systems for partner governments.'}] 
            },
            { id: 'agro', title: 'Agriculture', color: '#10b981', textCol: '#064e3b', tagline: 'Food Security Systems', icon: '<rect x="10" y="10" width="60" height="60" stroke="#059669" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#10B981" stroke-width="12"/>', vision: 'Feeding nations through precision scale.', about: 'Operating major precision agriculture projects in East Africa since 2018.', caps: ['National Food Security Advisory', 'Precision Ag-Tech Integration', 'Sustainable Irrigation Plans', 'Agri-Trade Logistics', 'Crop Genetics Advisory'], leads: [{n:'MD Agriculture', r:'East Africa'}], projects: [{t:'Rift Valley Farm Hub', d:'Automated high-yield maize facility.'}] },
            { id: 'meridian', title: 'Meridian', color: '#dc2626', textCol: '#7f1d1d', tagline: 'Continental Transit Hubs', icon: '<rect x="10" y="10" width="60" height="60" stroke="#991B1B" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#DC2626" stroke-width="12"/>', vision: 'Building the foundations of economy.', about: 'Executing massive infrastructure and energy grid projects in Sub-Saharan Africa since 2018.', caps: ['PPP Infrastructure Design', 'Sovereign Transport Plans', 'Major Civil Engineering', 'Energy Grid Modernization', 'Urban Connectivity Strategy'], leads: [{n:'Chief Engineer', r:'Africa Hub'}], projects: [{t:'Regional Power Corridor', d:'Energy grid synchronization between regional economies.'}] },
            { id: 'hr', title: 'Human Capital', color: '#64748b', textCol: '#1e293b', tagline: 'Talent & Security Safety', icon: '<rect x="10" y="10" width="60" height="60" stroke="#1e293b" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#94a3b8" stroke-width="12"/>', vision: 'Professional excellence at scale.', about: 'Providing the critical workforce and safety protocols for the worlds toughest industrial environments.', caps: ['National Manpower Policy', 'Security Risk Audits', 'Strategic Executive Search', 'Technical Safety Training', 'Facility Standards Consulting'], leads: [{n:'Head of Talent', r:'Sofia & Lagos'}], projects: [{t:'Mine Security Grid', d:'Full safety staffing for Tier-1 mining assets in Zambia.'}] },
            { id: 'trade', title: 'Trade', color: '#b45309', textCol: '#78350f', tagline: 'Global Commodity Exchange', icon: '<rect x="10" y="10" width="60" height="60" stroke="#7C2D12" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#EA580C" stroke-width="12"/>', vision: 'Directing the flow of global demand.', about: 'International commodity brokerage and logistics management for essential minerals.', caps: ['Trade Policy Consulting', 'Strategic Mineral Sourcing', 'Customs Optimization', 'Risk Management', 'International Market Entry'], leads: [{n:'Head of Trade', r:'Dubai Hub'}], projects: [{t:'Copper Export Hub', d:'Managing trade flow for high-purity Zambian copper.'}] },
            { id: 'energy', title: 'Energy', color: '#eab308', textCol: '#713f12', tagline: 'Industrial Power Grids', icon: '<rect x="10" y="10" width="60" height="60" stroke="#EAB308" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#FDE047" stroke-width="12"/>', vision: 'Sustainable energy for industrial growth.', about: 'Developing renewable grids and advising governments on energy transition.', caps: ['Grid Stability Advisory', 'Renewable Energy Policy', 'Smart Grid Design', 'Solar Operations', 'Carbon Footprint Consulting'], leads: [{n:'Energy Director', r:'East Africa'}], projects: [{t:'Rural Solar Grid', d:'50MW plant for regional industrial zones.'}] },
            { id: 'logistics', title: 'Logistics', color: '#6366f1', textCol: '#312e81', tagline: 'Global Movement Engine', icon: '<rect x="10" y="10" width="60" height="60" stroke="#4338CA" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#818CF8" stroke-width="12"/>', vision: 'Optimizing global supply chains.', about: 'Managing multimodal transit and port ground operations in major hubs.', caps: ['National Port Consulting', 'Network Strategy Design', 'Industrial Freight Mgmt', 'Transit Hub Feasibility', 'Supply Chain Security'], leads: [{n:'Logistics Dir', r:'Monrovia Hub'}], projects: [{t:'Liberia Port Ground Ops', d:'Integrated cargo flow for West African corridors.'}] },
            { id: 'finance', title: 'Capital', color: '#334155', textCol: '#020617', tagline: 'Strategic Project Finance', icon: '<rect x="10" y="10" width="60" height="60" stroke="#0F172A" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#475569" stroke-width="12"/>', vision: 'Funding global industrialization.', about: 'Engineering the financial vehicles for group-wide infrastructure and energy projects.', caps: ['Sovereign Wealth Advisory', 'Asset Mgmt Strategy', 'Project Finance Structure', 'Venture Capital', 'Governance Strategy'], leads: [{n:'Finance Unit', r:'Global Hub'}], projects: [{t:'Industrial Growth Fund', d:'$500M vehicle for multi-national projects.'}] },
            { id: 'chem', title: 'Chemicals', color: '#f59e0b', textCol: '#78350f', tagline: 'Industrial Molecular Science', icon: '<rect x="10" y="10" width="60" height="60" stroke="#D97706" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#F59E0B" stroke-width="12"/>', vision: 'Molecular foundations for industry.', about: 'Supplying industrial-grade fertilizers and reagents to critical sectors.', caps: ['Chemical Regulation', 'Industrial Waste Design', 'Fertilizer R&D', 'Mining Supply Chain', 'Lab Standard Consulting'], leads: [{n:'Lead Scientist', r:'Lagos Hub'}], projects: [{t:'Mineral Process Hub', d:'Industrial reagent logistics for mining sites.'}] },
            { id: 'media', title: 'Media', color: '#db2777', textCol: '#701a75', tagline: 'Strategic Nation Branding', icon: '<rect x="10" y="10" width="60" height="60" stroke="#DB2777" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#F472B6" stroke-width="12"/>', vision: 'Managing the narrative of industry.', about: 'Nation branding and creative strategy for industrial conglomerates and governmental partners.', caps: ['Government Branding', 'Crisis Communication', 'Digital Media Design', 'Creative Narrative', 'Public Perception Policy'], leads: [{n:'CCO Hub', r:'Tel Aviv Hub'}], projects: [{t:'Global Brand Sync', d:'Unified rebranding across 11 nations.'}] },
            { id: 'foundation', title: 'Foundation', color: '#9333ea', textCol: '#581c87', tagline: 'Social Impact Philanthropy', icon: '<rect x="10" y="10" width="60" height="60" stroke="#7E22CE" stroke-width="12"/><rect x="30" y="30" width="60" height="60" stroke="#A855F7" stroke-width="12"/>', vision: 'Investing in human potential.', about: 'Focusing on STEM education and healthcare in our active regions.', caps: ['Impact Assessment', 'CSR Strategy Design', 'Educational Grants', 'Health Access Planning', 'Social Infrastructure Consulting'], leads: [{n:'Foundation Head', r:'Global'}], projects: [{t:'STEM Scholarships', d:'Supporting next-gen engineers in the Caucasus.'}] }
        ];

        function initGrid() {
            const container = document.getElementById('divisionsContainer');
            container.innerHTML = '';
            
            // Render exactly 12 Subsidiaries (filter out the 'group' object)
            divisions.filter(d => d.id !== 'group').forEach(div => {
                const card = document.createElement('div');
                card.className = 'division-card group';
                card.onclick = () => showDivision(div.id);
                card.innerHTML = `
                    <div class="brand-block">
                        <div class="w-16 h-16 md:w-20 md:h-20 flex items-center justify-center">
                            <svg viewBox="0 0 100 100" fill="none">${div.icon}</svg>
                        </div>
                        <div class="brand-identity">
                            <div class="brand-pfx">Orizis</div>
                            <div class="brand-main" style="color: ${div.textCol}">${div.title}</div>
                        </div>
                    </div>
                    <p class="text-[10px] md:text-[11px] tracking-widest uppercase font-black text-slate-400 mt-10 md:mt-12 group-hover:text-blue-500 transition">${div.tagline}</p>
                `;
                container.appendChild(card);
            });

            const locList = document.getElementById('locationsList');
            locList.innerHTML = '';
            locations.forEach(loc => {
                const item = document.createElement('div');
                item.className = 'flex items-center text-[12px] md:text-[14px] font-black text-slate-500 uppercase tracking-widest';
                item.innerHTML = `<span class="location-dot"></span> ${loc}`;
                locList.appendChild(item);
            });
        }

        function showDivision(id) {
            const div = divisions.find(d => d.id === id);
            document.getElementById('homePage').classList.add('section-hidden');
            document.getElementById('divisionPage').classList.remove('section-hidden');
            window.scrollTo({top: 0, behavior: 'instant'});

            document.getElementById('divHeaderBox').innerHTML = `
                <div class="brand-block scale-110 md:scale-150">
                    <div class="w-20 h-20 md:w-24 md:h-24 mb-4 flex items-center justify-center">
                        <svg viewBox="0 0 100 100" fill="none">${div.icon}</svg>
                    </div>
                    <div class="brand-identity">
                        <div class="brand-pfx text-sm md:text-lg">Orizis</div>
                        <div class="brand-main text-5xl md:text-8xl" style="color: ${div.textCol}; font-size: clamp(40px, 8vw, 90px);">${div.title}</div>
                    </div>
                </div>
                <div class="w-20 h-2 bg-blue-500 mt-12 mb-10 rounded-full"></div>
                <p class="max-w-4xl mx-auto text-xl md:text-3xl text-slate-500 font-extralight leading-relaxed italic">"${div.vision}"</p>
            `;
            
            document.getElementById('divAbout').innerText = div.about;

            document.getElementById('divCaps').innerHTML = div.caps.map(c => `
                <div class="p-10 md:p-14 bg-white dark:bg-white/5 rounded-[3rem] border border-slate-100 dark:border-white/10 shadow-sm hover:shadow-xl transition-all">
                    <div class="w-12 h-1.5 rounded-full mb-8" style="background:${div.color}"></div>
                    <span class="text-base md:text-xl font-black uppercase tracking-widest text-slate-800 dark:text-slate-200 leading-tight block">${c}</span>
                </div>
            `).join('');

            document.getElementById('divTeam').innerHTML = div.leads.map(l => `
                <div class="pb-8 border-b border-slate-100 dark:border-white/10 text-left">
                    <div class="font-black text-xl uppercase montserrat high-contrast-title">${l.n}</div>
                    <div class="text-[10px] text-blue-500 uppercase tracking-widest font-black mt-2">${l.r}</div>
                </div>
            `).join('');

            document.getElementById('divProjects').innerHTML = div.projects.map(p => `
                <div class="bg-slate-50 dark:bg-white/5 p-12 md:p-20 rounded-[4rem] shadow-xl border-0">
                    <h4 class="font-black text-3xl md:text-4xl mb-8 uppercase montserrat tracking-tighter high-contrast-title">${p.t}</h4>
                    <p class="text-slate-500 text-xl md:text-2xl leading-relaxed font-light mb-12">${p.d}</p>
                    <div class="pt-8 border-t border-slate-200 dark:border-white/10 flex justify-between items-center text-[10px] font-black uppercase tracking-widest text-blue-500">
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

        function toggleTheme() { document.getElementById('bodyTag').classList.toggle('dark-mode'); }

        initGrid();
    </script>
</body>
</html>
