<!DOCTYPE html>
<html lang="en" dir="ltr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ORIZIS GROUP | Global Industrial Holdings</title>
    
    <!-- Favicon - הלוגו הגיאומטרי המקורי -->
    <link rel="icon" type="image/svg+xml" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'%3E%3Crect x='10' y='10' width='60' height='60' stroke='%2300C2FF' stroke-width='12' fill='none'/%3E%3Crect x='30' y='30' width='60' height='60' stroke='%23A377FF' stroke-width='12' fill='none'/%3E%3C/svg%3E">
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;700;800;900&family=Inter:wght@300;400;600;700&display=swap" rel="stylesheet">
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

        /* Hero - GUARANTEED HIGH CONTRAST */
        .hero-section {
            background-color: #020617 !important; 
            padding: clamp(140px, 20vh, 250px) 24px;
            position: relative;
            overflow: hidden;
            text-align: center;
        }
        .hero-title {
            color: #ffffff !important; 
            font-size: clamp(3rem, 8vw, 7rem);
            line-height: 0.95;
            font-weight: 900;
            text-transform: uppercase;
            letter-spacing: -0.02em;
        }
        .hero-desc {
            color: #cbd5e1 !important; 
        }

        /* Portfolio Grid Cards */
        .division-card {
            background: #ffffff;
            border: 1px solid #f1f5f9;
            padding: 50px 10px;
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
            gap: 1.5rem; 
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
            font-size: clamp(12px, 2.5vw, 16px); 
            text-transform: uppercase;
            line-height: 1.1;
            letter-spacing: -0.01em;
            width: 100%;
            white-space: nowrap; 
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

        /* Clients Marquee Section */
        .clients-marquee {
            overflow: hidden;
            white-space: nowrap;
            position: relative;
            width: 100%;
            mask-image: linear-gradient(to right, transparent, black 10%, black 90%, transparent);
            -webkit-mask-image: linear-gradient(to right, transparent, black 10%, black 90%, transparent);
        }
        .clients-track {
            display: inline-flex;
            align-items: center;
            gap: 6rem;
            /* מתאים את מהירות הגלילה (ניתן לשנות את ה-80s למהיר/איטי יותר) */
            animation: marquee 80s linear infinite; 
        }
        .clients-track:hover {
            animation-play-state: paused;
        }
        @keyframes marquee {
            0% { transform: translateX(0); }
            100% { transform: translateX(-50%); }
        }
        .client-logo {
            display: flex;
            align-items: center;
            gap: 1rem;
            opacity: 0.4;
            transition: all 0.4s ease;
            filter: grayscale(100%);
            cursor: pointer;
        }
        .client-logo:hover {
            opacity: 1;
            filter: grayscale(0%);
            transform: scale(1.05);
        }
        .client-icon { width: 44px; height: 44px; flex-shrink: 0; }
        .client-name {
            font-family: 'Montserrat', sans-serif;
            font-weight: 900;
            font-size: 1.25rem;
            text-transform: uppercase;
            letter-spacing: -0.02em;
            line-height: 1;
        }
        .client-industry {
            font-family: 'Inter', sans-serif;
            font-weight: 700;
            font-size: 0.6rem;
            letter-spacing: 0.25em;
            text-transform: uppercase;
            color: #94a3b8;
            margin-top: 0.2rem;
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
            
            <!-- ORIGINAL LOGO -->
            <div class="flex items-center gap-4 cursor-pointer" onclick="showHome()">
                <div class="w-10 h-10 md:w-12 md:h-12 flex items-center justify-center flex-shrink-0">
                    <svg viewBox="0 0 100 100" fill="none" class="w-full h-full">
                        <rect x="10" y="10" width="60" height="60" stroke="#00C2FF" stroke-width="12"/>
                        <rect x="30" y="30" width="60" height="60" stroke="#A377FF" stroke-width="12"/>
                    </svg>
                </div>
                <!-- Vertical Separator -->
                <div class="w-[2px] h-10 bg-slate-200 dark:bg-slate-700 hidden sm:block"></div>
                <!-- Text Mark -->
                <div class="flex flex-col justify-center text-[#00C2FF]">
                    <span class="montserrat font-black text-[20px] md:text-[24px] leading-[0.9] tracking-widest uppercase">ORIZIS</span>
                    <span class="montserrat font-light text-[20px] md:text-[24px] leading-[0.9] tracking-[0.25em] uppercase">GROUP</span>
                </div>
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
            <section class="hero-section relative">
                <div class="absolute inset-0 w-full h-full opacity-20 bg-slate-900 flex items-center justify-center text-slate-500 text-xl md:text-3xl font-bold uppercase tracking-widest text-center px-6">
                    
                </div>
                
                <div class="max-w-6xl mx-auto relative z-10">
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
                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8" id="divisionsContainer"></div>
                </div>
            </section>

            <!-- FOOTPRINT -->
            <section id="global" class="py-40 px-6 bg-slate-50 dark:bg-slate-950 relative overflow-hidden">
                <div class="max-w-7xl mx-auto grid lg:grid-cols-2 gap-32 items-center relative z-10">
                    <div>
                        <h2 class="text-6xl md:text-7xl font-black montserrat uppercase tracking-tighter mb-12 high-contrast-title">Global Reach</h2>
                        <p class="text-slate-500 mb-16 leading-relaxed text-xl md:text-2xl font-light">
                            Operational presence in 11 key nations, managing physical industrial assets and resilient supply chains established since 2018.
                        </p>
                        <div class="grid grid-cols-2 gap-8" id="locationsList"></div>
                    </div>
                    <div class="bg-white dark:bg-slate-900 h-[500px] md:h-[600px] rounded-[4rem] flex items-center justify-center border border-slate-200 dark:border-white/5 shadow-2xl relative overflow-hidden">
                         <div class="absolute inset-0 w-full h-full opacity-10 dark:opacity-20 bg-slate-800 flex items-center justify-center text-slate-500 text-sm font-bold uppercase tracking-widest text-center px-4">
                             
                         </div>
                         <div class="text-center relative z-10">
                             <div class="text-[140px] md:text-[180px] font-black montserrat text-blue-500/10 leading-none">11</div>
                             <p class="uppercase tracking-[0.7em] text-sm md:text-xl font-black text-slate-400">Jurisdictions</p>
                         </div>
                    </div>
                </div>
            </section>

            <!-- CLIENTS MARQUEE (TRUSTED BY) -->
            <section id="clients" class="py-32 bg-white dark:bg-slate-900 border-t border-slate-100 dark:border-white/5">
                <div class="max-w-7xl mx-auto px-6 mb-20 text-center">
                    <h2 class="text-4xl md:text-5xl font-black montserrat uppercase tracking-tighter mb-6 high-contrast-title">Trusted By Global Leaders</h2>
                    <p class="text-slate-400 font-bold uppercase tracking-[0.5em] text-[11px]">Strategic Partners & Institutional Clients</p>
                    <div class="w-24 h-2 bg-blue-500 mx-auto rounded-full mt-8"></div>
                </div>
                <div class="clients-marquee pb-10">
                    <div class="clients-track" id="clientsTrack">
                        <!-- Injected via JS -->
                    </div>
                </div>
            </section>
        </div>

        <!-- DETAIL PAGE -->
        <div id="divisionPage" class="section-hidden pb-48 bg-white dark:bg-slate-950">
            <!-- Dynamic Header populated via JS -->
            <header id="divHeaderArea" class="py-40 relative overflow-hidden text-center border-b border-slate-200 dark:border-white/10"></header>

            <main class="max-w-7xl mx-auto px-6 mt-40">
                <div class="grid lg:grid-cols-12 gap-32">
                    <div class="lg:col-span-8 space-y-40">
                        
                        <section>
                            <h2 class="text-[12px] font-black uppercase tracking-[0.8em] text-blue-500 mb-12">Operational Overview</h2>
                            <p id="divAbout" class="text-3xl md:text-4xl text-slate-600 dark:text-slate-400 leading-relaxed font-light"></p>
                        </section>

                        <section>
                            <h2 class="text-[12px] font-black uppercase tracking-[0.8em] text-blue-500 mb-16">Visual Showcase</h2>
                            <div class="rounded-[3rem] overflow-hidden shadow-2xl h-[300px] md:h-[500px]" id="divShowcaseImg"></div>
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
                <!-- ORIGINAL LOGO IN FOOTER -->
                <div class="flex items-center justify-center md:justify-start gap-4 mb-8">
                    <div class="w-12 h-12 flex items-center justify-center flex-shrink-0">
                        <svg viewBox="0 0 100 100" fill="none" class="w-full h-full">
                            <rect x="10" y="10" width="60" height="60" stroke="#00C2FF" stroke-width="12"/>
                            <rect x="30" y="30" width="60" height="60" stroke="#A377FF" stroke-width="12"/>
                        </svg>
                    </div>
                    <div class="w-[2px] h-10 bg-slate-200 dark:bg-slate-700 hidden sm:block"></div>
                    <div class="flex flex-col justify-center text-[#00C2FF]">
                        <span class="montserrat font-black text-[22px] leading-[0.9] tracking-widest uppercase">ORIZIS</span>
                        <span class="montserrat font-light text-[22px] leading-[0.9] tracking-[0.25em] uppercase">GROUP</span>
                    </div>
                </div>
                <p class="text-xs text-slate-400 uppercase tracking-[0.6em] font-bold">12 Global Subsidiaries. Established Africa 2018. Industrial Power.</p>
            </div>
            <p class="text-[10px] font-black uppercase tracking-[0.4em] text-slate-400">© 2026 ORIZIS GROUP HOLDINGS LTD.</p>
        </div>
    </footer>

    <script>
        const locations = ["Zambia", "Botswana", "Tanzania", "Georgia", "Azerbaijan", "Bulgaria", "Israel", "Congo DRC", "Liberia", "Kenya", "Nigeria"];

        const divisions = [
            { 
                id: 'group', title: 'Management', color: '#00C2FF', textCol: '#0f172a', tagline: 'Unified Strategic Umbrella', 
                heroImage: '', showcaseImage: '',
                vision: 'United under one global umbrella, managing high-impact international activities.',
                about: 'Orizis Group Holdings serves as the overarching international umbrella, coordinating and governing diversified industrial activity across multiple continents. Established in Africa in 2018, the group focuses on large-scale asset management, high-level governance, and creating synergy between its 12 market-leading subsidiaries. We act as the strategic bridge between emerging resources and global capital.',
                caps: ['Governmental Strategic Advisory', 'International Portfolio Governance', 'Cross-Border Capital Deployment', 'Regulatory Compliance & Strategy', 'Multi-National Execution Oversight'],
                leads: [{n:'Executive Council', r:'Orizis HQ Hub'}],
                projects: [{t:'Global Synergy Roadmap', d:'Designing the infrastructure for unified growth across 11 nations.', img: ''}]
            },
            { 
                id: 're', title: 'Real Estate', color: '#1E3A8A', textCol: '#1a2e5a', tagline: '360° Property Ecosystem', 
                heroImage: '', showcaseImage: '',
                vision: 'Transforming landscapes through luxury construction and asset management.',
                about: 'Orizis Real Estate provides an end-to-end value chain in Azerbaijan and Georgia. We manage high-end assets from construction and design to luxury marketing, legal brokerage, and institutional property management.',
                caps: ['Governmental Urban Planning', 'Luxury Asset Marketing Agency', 'Real Estate Legal Support', 'Institutional Property Management', 'Brokerage & Transaction Advisory'],
                leads: [{n:'RE Director', r:'Baku Division'}],
                projects: [{t:'Baku Premium Towers', d:'Luxury complex under 24/7 institutional Orizis management.', img: ''}]
            },
            { 
                id: 'tech', title: 'Technologies', color: '#22d3ee', textCol: '#0891b2', tagline: 'Industrial Digital Shield', 
                heroImage: '', showcaseImage: '',
                vision: 'Digital R&D powering global operations.', 
                about: 'Deploying cybersecurity and industrial IoT from our Bulgarian hub to protect national-scale infrastructures and group-wide data synchronization.', 
                caps: ['National Cyber Strategy', 'Industrial IoT System Design', 'Governmental AI Policy', 'Enterprise Software R&D', 'Strategic IT Infrastructure'], 
                leads: [{n:'CTO Sofia', r:'R&D Division'}], 
                projects: [{t:'Data Shield Infrastructure', d:'Protecting national critical systems for partner governments.', img: ''}] 
            },
            { id: 'agro', title: 'Agriculture', color: '#10b981', textCol: '#064e3b', tagline: 'Food Security Systems', heroImage: '', showcaseImage: '', vision: 'Feeding nations through precision scale.', about: 'Operating major precision agriculture projects in East Africa since 2018.', caps: ['National Food Security Advisory', 'Precision Ag-Tech Integration', 'Sustainable Irrigation Plans', 'Agri-Trade Logistics', 'Crop Genetics Advisory'], leads: [{n:'MD Agriculture', r:'East Africa'}], projects: [{t:'Rift Valley Farm Hub', d:'Automated high-yield maize facility.', img: ''}] },
            { id: 'meridian', title: 'Meridian', color: '#dc2626', textCol: '#7f1d1d', tagline: 'Continental Transit Hubs', heroImage: '', showcaseImage: '', vision: 'Building the foundations of economy.', about: 'Executing massive infrastructure and energy grid projects in Sub-Saharan Africa since 2018.', caps: ['PPP Infrastructure Design', 'Sovereign Transport Plans', 'Major Civil Engineering', 'Energy Grid Modernization', 'Urban Connectivity Strategy'], leads: [{n:'Chief Engineer', r:'Africa Hub'}], projects: [{t:'Regional Power Corridor', d:'Energy grid synchronization between regional economies.', img: ''}] },
            { id: 'hr', title: 'Human Capital', color: '#64748b', textCol: '#1e293b', tagline: 'Talent & Security Safety', heroImage: '', showcaseImage: '', vision: 'Professional excellence at scale.', about: 'Providing the critical workforce and safety protocols for the worlds toughest industrial environments.', caps: ['National Manpower Policy', 'Security Risk Audits', 'Strategic Executive Search', 'Technical Safety Training', 'Facility Standards Consulting'], leads: [{n:'Head of Talent', r:'Sofia & Lagos'}], projects: [{t:'Mine Security Grid', d:'Full safety staffing for Tier-1 mining assets in Zambia.', img: ''}] },
            { id: 'trade', title: 'Trade', color: '#b45309', textCol: '#78350f', tagline: 'Global Commodity Exchange', heroImage: '', showcaseImage: '', vision: 'Directing the flow of global demand.', about: 'International commodity brokerage and logistics management for essential minerals.', caps: ['Trade Policy Consulting', 'Strategic Mineral Sourcing', 'Customs Optimization', 'Risk Management', 'International Market Entry'], leads: [{n:'Head of Trade', r:'Dubai Hub'}], projects: [{t:'Copper Export Hub', d:'Managing trade flow for high-purity Zambian copper.', img: ''}] },
            { id: 'energy', title: 'Energy', color: '#eab308', textCol: '#713f12', tagline: 'Industrial Power Grids', heroImage: '', showcaseImage: '', vision: 'Sustainable energy for industrial growth.', about: 'Developing renewable grids and advising governments on energy transition.', caps: ['Grid Stability Advisory', 'Renewable Energy Policy', 'Smart Grid Design', 'Solar Operations', 'Carbon Footprint Consulting'], leads: [{n:'Energy Director', r:'East Africa'}], projects: [{t:'Rural Solar Grid', d:'50MW plant for regional industrial zones.', img: ''}] },
            { id: 'logistics', title: 'Logistics', color: '#6366f1', textCol: '#312e81', tagline: 'Global Movement Engine', heroImage: '', showcaseImage: '', vision: 'Optimizing global supply chains.', about: 'Managing multimodal transit and port ground operations in major hubs.', caps: ['National Port Consulting', 'Network Strategy Design', 'Industrial Freight Mgmt', 'Transit Hub Feasibility', 'Supply Chain Security'], leads: [{n:'Logistics Dir', r:'Monrovia Hub'}], projects: [{t:'Liberia Port Ground Ops', d:'Integrated cargo flow for West African corridors.', img: ''}] },
            { id: 'finance', title: 'Capital', color: '#334155', textCol: '#020617', tagline: 'Strategic Project Finance', heroImage: '', showcaseImage: '', vision: 'Funding global industrialization.', about: 'Engineering the financial vehicles for group-wide infrastructure and energy projects.', caps: ['Sovereign Wealth Advisory', 'Asset Mgmt Strategy', 'Project Finance Structure', 'Venture Capital', 'Governance Strategy'], leads: [{n:'Finance Unit', r:'Global Hub'}], projects: [{t:'Industrial Growth Fund', d:'$500M vehicle for multi-national projects.', img: ''}] },
            { id: 'chem', title: 'Chemicals', color: '#f59e0b', textCol: '#78350f', tagline: 'Industrial Molecular Science', heroImage: '', showcaseImage: '', vision: 'Molecular foundations for industry.', about: 'Supplying industrial-grade fertilizers and reagents to critical sectors.', caps: ['Chemical Regulation', 'Industrial Waste Design', 'Fertilizer R&D', 'Mining Supply Chain', 'Lab Standard Consulting'], leads: [{n:'Lead Scientist', r:'Lagos Hub'}], projects: [{t:'Mineral Process Hub', d:'Industrial reagent logistics for mining sites.', img: ''}] },
            { id: 'media', title: 'Media', color: '#db2777', textCol: '#701a75', tagline: 'Strategic Nation Branding', heroImage: '', showcaseImage: '', vision: 'Managing the narrative of industry.', about: 'Nation branding and creative strategy for industrial conglomerates and governmental partners.', caps: ['Government Branding', 'Crisis Communication', 'Digital Media Design', 'Creative Narrative', 'Public Perception Policy'], leads: [{n:'CCO Hub', r:'Tel Aviv Hub'}], projects: [{t:'Global Brand Sync', d:'Unified rebranding across 11 nations.', img: ''}] },
            { id: 'foundation', title: 'Foundation', color: '#9333ea', textCol: '#581c87', tagline: 'Social Impact Philanthropy', heroImage: '', showcaseImage: '', vision: 'Investing in human potential.', about: 'Focusing on STEM education and healthcare in our active regions.', caps: ['Impact Assessment', 'CSR Strategy Design', 'Educational Grants', 'Health Access Planning', 'Social Infrastructure Consulting'], leads: [{n:'Foundation Head', r:'Global'}], projects: [{t:'STEM Scholarships', d:'Supporting next-gen engineers in the Caucasus.', img: ''}] }
        ];

        // 30 Fictional High-End Corporate Clients
        const fictionalClients = [
            { name: "Vertex Capital", ind: "Investment Banking", icon: "bank", color: "#1E3A8A" },
            { name: "OmniTech", ind: "Enterprise Systems", icon: "tech", color: "#0284C7" },
            { name: "Atlas Minerals", ind: "Global Extraction", icon: "industry", color: "#B45309" },
            { name: "Solara", ind: "Renewable Energy", icon: "energy", color: "#EAB308" },
            { name: "Meridian Transit", ind: "Maritime Logistics", icon: "global", color: "#4338CA" },
            { name: "Pinnacle Dev", ind: "Commercial Real Estate", icon: "build", color: "#334155" },
            { name: "BioChem Global", ind: "Industrial Synthetics", icon: "tech", color: "#059669" },
            { name: "Vanguard Trust", ind: "Wealth Management", icon: "bank", color: "#1E293B" },
            { name: "Quantum Data", ind: "Data Infrastructure", icon: "tech", color: "#6366F1" },
            { name: "Titan Heavy", ind: "Heavy Machinery", icon: "industry", color: "#7C2D12" },
            { name: "Volt Power", ind: "National Grids", icon: "energy", color: "#D97706" },
            { name: "Swift Cargo", ind: "Global Freight", icon: "global", color: "#2563EB" },
            { name: "Skyline Holdings", ind: "Urban Development", icon: "build", color: "#475569" },
            { name: "GreenYield", ind: "Agro-Technology", icon: "tech", color: "#10B981" },
            { name: "Aethel Finance", ind: "Sovereign Funds", icon: "bank", color: "#7E22CE" },
            { name: "Sentient Systems", ind: "Cybersecurity", icon: "tech", color: "#0EA5E9" },
            { name: "GeoExtract", ind: "Mining Operations", icon: "industry", color: "#9A3412" },
            { name: "NexGen Nuclear", ind: "Power Facilities", icon: "energy", color: "#F59E0B" },
            { name: "Atlas Supply", ind: "Supply Chain", icon: "global", color: "#1D4ED8" },
            { name: "Foundation Build", ind: "Civil Engineering", icon: "build", color: "#1E293B" },
            { name: "SynthMaterials", ind: "Advanced Materials", icon: "tech", color: "#047857" },
            { name: "Equinox Bank", ind: "Corporate Banking", icon: "bank", color: "#3730A3" },
            { name: "CyberCore Defense", ind: "Defense Systems", icon: "tech", color: "#3B82F6" },
            { name: "IronClad Resources", ind: "Resource Holdings", icon: "industry", color: "#991B1B" },
            { name: "AeroDynamics", ind: "Aerospace", icon: "tech", color: "#64748B" },
            { name: "Global Ports", ind: "Port Operations", icon: "global", color: "#4F46E5" },
            { name: "Zenith Trust", ind: "Property Trust", icon: "build", color: "#0F172A" },
            { name: "AgriCorp Int", ind: "Global Farming", icon: "industry", color: "#16A34A" },
            { name: "Nexa Wealth", ind: "Private Equity", icon: "bank", color: "#6B21A8" },
            { name: "NovaNet Global", ind: "Telecommunications", icon: "global", color: "#0284C7" }
        ];

        // SVGs for the clients to make them look authentic
        const clientIcons = {
            bank: '<path d="M12 1L3 5v6c0 5.55 3.84 10.74 9 12 5.16-1.26 9-6.45 9-12V5l-9-4zm0 10.99h7c-.53 4.12-3.28 7.79-7 8.94V12H5V6.3l7-3.11v8.8z"/>',
            tech: '<path d="M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5"/>',
            industry: '<path d="M19.14,12.94c0.04-0.3,0.06-0.61,0.06-0.94c0-0.32-0.02-0.64-0.06-0.94l2.03-1.58c0.18-0.14,0.23-0.41,0.12-0.61 l-1.92-3.32c-0.12-0.22-0.37-0.29-0.59-0.22l-2.39,0.96c-0.5-0.38-1.03-0.7-1.62-0.94L14.4,2.81c-0.04-0.24-0.24-0.41-0.48-0.41 h-3.84c-0.24,0-0.43,0.17-0.47,0.41L9.25,5.35C8.66,5.59,8.12,5.92,7.63,6.29L5.24,5.33c-0.22-0.08-0.47,0-0.59,0.22L2.73,8.87 C2.62,9.08,2.66,9.34,2.86,9.48l2.03,1.58C4.84,11.36,4.8,11.69,4.8,12s0.02,0.64,0.06,0.94l-2.03,1.58 c-0.18,0.14-0.23,0.41-0.12,0.61l1.92,3.32c0.12,0.22,0.37,0.29,0.59,0.22l2.39-0.96c0.5,0.38,1.03,0.7,1.62,0.94l0.36,2.54 c0.05,0.24,0.24,0.41,0.48,0.41h3.84c0.24,0,0.43-0.17,0.47-0.41l0.36-2.54c0.59-0.24,1.13-0.56,1.62-0.94l2.39,0.96 c0.22,0.08,0.47,0,0.59-0.22l1.92-3.32c0.12-0.22,0.07-0.49-0.12-0.61L19.14,12.94z M12,15.6c-1.98,0-3.6-1.62-3.6-3.6 s1.62-3.6,3.6-3.6s3.6,1.62,3.6,3.6S13.98,15.6,12,15.6z"/>',
            energy: '<path d="M11 21h-1l1-7H7.5c-.58 0-.57-.32-.38-.66C7.67 12.24 9.02 9.68 11.23 6H12l-1 7h3.5c.49 0 .56.33.47.51l-4.57 7.49z"/>',
            global: '<path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-1 17.93c-3.95-.49-7-3.85-7-7.93 0-.62.08-1.21.21-1.79L9 15v1c0 1.1.9 2 2 2v1.93zm6.9-2.54c-.26-.81-1-1.39-1.9-1.39h-1v-3c0-.55-.45-1-1-1H8v-2h2c.55 0 1-.45 1-1V7h2c1.1 0 2-.9 2-2v-.41c2.93 1.19 5 4.06 5 7.41 0 2.08-.8 3.97-2.1 5.39z"/>',
            build: '<path d="M3 21h18v-2H3v2zm6-4h6V7H9v10zm-6 0h4V11H3v6zm14 0h4V11h-4v6z"/>'
        };

        // Original Clean SVG Generator
        function getLogoSVG(color1, color2) {
            return `
            <svg viewBox="0 0 100 100" fill="none" class="w-full h-full">
                <rect x="10" y="10" width="60" height="60" stroke="${color1}" stroke-width="12"/>
                <rect x="30" y="30" width="60" height="60" stroke="${color2}" stroke-width="12"/>
            </svg>
            `;
        }

        function initGrid() {
            const container = document.getElementById('divisionsContainer');
            container.innerHTML = '';
            
            divisions.filter(d => d.id !== 'group').forEach(div => {
                const card = document.createElement('div');
                card.className = 'division-card group';
                card.onclick = () => showDivision(div.id);
                card.innerHTML = `
                    <div class="brand-block">
                        <div class="w-16 h-16 md:w-20 md:h-20 flex items-center justify-center transition-transform group-hover:scale-105">
                            ${getLogoSVG(div.color, div.textCol)}
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

            document.getElementById('divHeaderArea').innerHTML = `
                <div class="absolute inset-0 z-0">
                    <div class="w-full h-full bg-slate-900 flex items-center justify-center text-slate-500 text-sm md:text-xl font-bold uppercase tracking-widest px-4 text-center">
                        ${div.heroImage}
                    </div>
                    <div class="image-overlay"></div>
                </div>
                <div class="max-w-7xl mx-auto px-6 flex flex-col items-center text-center relative z-10" id="divHeaderBox">
                    <div class="brand-block scale-110 md:scale-150">
                        <div class="w-20 h-20 md:w-24 md:h-24 mb-4 flex items-center justify-center">
                            ${getLogoSVG(div.color, div.textCol)}
                        </div>
                        <div class="brand-identity">
                            <div class="brand-pfx text-sm md:text-lg text-slate-300">Orizis</div>
                            <div class="brand-main text-white" style="font-size: clamp(30px, 6vw, 70px); white-space: normal;">${div.title}</div>
                        </div>
                    </div>
                    <div class="w-20 h-2 bg-blue-500 mt-12 mb-10 rounded-full"></div>
                    <p class="max-w-4xl mx-auto text-xl md:text-3xl text-slate-300 font-extralight leading-relaxed italic">"${div.vision}"</p>
                </div>
            `;
            
            document.getElementById('divAbout').innerText = div.about;

            document.getElementById('divShowcaseImg').innerHTML = `
                <div class="w-full h-full bg-slate-800 flex items-center justify-center text-slate-500 text-sm md:text-lg font-bold uppercase tracking-widest hover:scale-105 transition duration-700 px-4 text-center">
                    ${div.showcaseImage}
                </div>
            `;

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
                <div class="bg-slate-50 dark:bg-white/5 rounded-[4rem] shadow-xl border-0 overflow-hidden flex flex-col group">
                    <div class="h-64 md:h-96 w-full overflow-hidden bg-slate-800 flex items-center justify-center text-slate-500 text-xs md:text-sm font-bold uppercase tracking-widest px-4 text-center group-hover:scale-105 transition duration-700">
                        ${p.img}
                    </div>
                    <div class="p-12 md:p-20">
                        <h4 class="font-black text-3xl md:text-4xl mb-8 uppercase montserrat tracking-tighter high-contrast-title">${p.t}</h4>
                        <p class="text-slate-500 text-xl md:text-2xl leading-relaxed font-light mb-12">${p.d}</p>
                        <div class="pt-8 border-t border-slate-200 dark:border-white/10 flex justify-between items-center text-[10px] font-black uppercase tracking-widest text-blue-500">
                            <span>Corporate Analysis</span>
                            <span>[→]</span>
                        </div>
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

        function initClients() {
            const track = document.getElementById('clientsTrack');
            let html = '';
            fictionalClients.forEach(c => {
                html += `
                    <div class="client-logo">
                        <div class="client-icon" style="color: ${c.color}">
                            <svg viewBox="0 0 24 24" fill="currentColor" class="w-full h-full drop-shadow-md">${clientIcons[c.icon]}</svg>
                        </div>
                        <div class="flex flex-col text-left">
                            <span class="client-name" style="color: ${c.color}">${c.name}</span>
                            <span class="client-industry">${c.ind}</span>
                        </div>
                    </div>
                `;
            });
            // We duplicate the HTML string to create a seamless infinite loop effect
            track.innerHTML = html + html;
        }

        initGrid();
        initClients(); // Initialize the client marquee
    </script>
</body>
</html>
