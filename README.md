<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AgencyFlow – Simple CRM & Agency Management</title>
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    <style>
        :root {
            --primary: #2B6CB0;
            --primary-light: #ebf4ff;
            --success: #38a169;
            --warning: #d69e2e;
            --danger: #e53e3e;
            --gray-50: #f7fafc;
            --gray-100: #edf2f7;
            --gray-200: #e2e8f0;
            --gray-300: #cbd5e0;
            --gray-600: #4a5568;
            --gray-700: #2d3748;
            --gray-800: #1a202c;
            --sidebar-width: 250px;
            --topbar-height: 60px;
            --bottom-nav-height: 60px;
            --card-shadow: 0 1px 3px rgba(0,0,0,0.1);
            --border-radius: 12px;
            --transition: all 0.2s ease;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: var(--gray-50);
            color: var(--gray-700);
            display: flex;
            height: 100vh;
            overflow: hidden;
            transition: background 0.3s, color 0.3s;
        }

        /* Dark mode */
        body.dark {
            --gray-50: #1a202c;
            --gray-100: #2d3748;
            --gray-200: #4a5568;
            --gray-300: #718096;
            --gray-600: #cbd5e0;
            --gray-700: #e2e8f0;
            --gray-800: #f7fafc;
            --primary-light: #2b4a6f;
            --card-shadow: 0 1px 3px rgba(255,255,255,0.05);
            background: #1a202c;
        }

        /* Sidebar */
        .sidebar {
            width: var(--sidebar-width);
            background: white;
            border-right: 1px solid var(--gray-200);
            display: flex;
            flex-direction: column;
            transition: transform 0.3s, width 0.3s, background 0.3s;
            z-index: 100;
        }
        body.dark .sidebar { background: #171923; }

        .sidebar-header {
            padding: 1rem;
            display: flex;
            align-items: center;
            gap: 0.75rem;
            border-bottom: 1px solid var(--gray-200);
        }
        .sidebar-header .logo {
            font-size: 1.5rem;
            color: var(--primary);
        }
        .sidebar-header h1 {
            font-size: 1.25rem;
            font-weight: 700;
            color: var(--gray-800);
        }
        body.dark .sidebar-header h1 { color: white; }

        .sidebar-nav {
            flex: 1;
            overflow-y: auto;
            padding: 0.5rem 0;
        }
        .sidebar-nav a {
            display: flex;
            align-items: center;
            gap: 0.75rem;
            padding: 0.6rem 1.25rem;
            color: var(--gray-600);
            text-decoration: none;
            font-size: 0.9rem;
            transition: var(--transition);
            border-left: 3px solid transparent;
        }
        .sidebar-nav a:hover {
            background: var(--gray-100);
            color: var(--gray-800);
        }
        .sidebar-nav a.active {
            background: var(--primary-light);
            color: var(--primary);
            border-left-color: var(--primary);
            font-weight: 500;
        }
        .sidebar-nav a i {
            width: 20px;
            text-align: center;
        }

        /* Main content */
        .main {
            flex: 1;
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        /* Topbar */
        .topbar {
            height: var(--topbar-height);
            background: white;
            border-bottom: 1px solid var(--gray-200);
            display: flex;
            align-items: center;
            padding: 0 1.5rem;
            gap: 1rem;
            transition: background 0.3s;
        }
        body.dark .topbar { background: #171923; }

        .topbar .search {
            flex: 1;
            position: relative;
            max-width: 400px;
        }
        .topbar .search input {
            width: 100%;
            padding: 0.5rem 1rem 0.5rem 2.25rem;
            border: 1px solid var(--gray-200);
            border-radius: 9999px;
            background: var(--gray-50);
            color: var(--gray-700);
            font-size: 0.9rem;
        }
        .topbar .search i {
            position: absolute;
            left: 0.9rem;
            top: 50%;
            transform: translateY(-50%);
            color: var(--gray-600);
        }
        .topbar .actions {
            display: flex;
            align-items: center;
            gap: 1rem;
        }
        .topbar .actions .icon-btn {
            background: none;
            border: none;
            font-size: 1.2rem;
            color: var(--gray-600);
            cursor: pointer;
            position: relative;
        }
        .topbar .actions .icon-btn .badge {
            position: absolute;
            top: -8px;
            right: -8px;
            background: var(--danger);
            color: white;
            border-radius: 50%;
            width: 18px;
            height: 18px;
            font-size: 0.7rem;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .topbar .actions .avatar {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            background: var(--primary);
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            cursor: pointer;
        }

        /* Content area */
        .content {
            flex: 1;
            overflow-y: auto;
            padding: 1.5rem;
        }

        /* KPI Cards */
        .kpi-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 1rem;
            margin-bottom: 1.5rem;
        }
        .kpi-card {
            background: white;
            border-radius: var(--border-radius);
            box-shadow: var(--card-shadow);
            padding: 1.25rem;
            display: flex;
            align-items: center;
            gap: 1rem;
            transition: transform 0.2s;
        }
        .kpi-card:hover { transform: translateY(-2px); }
        .kpi-card .icon {
            width: 48px;
            height: 48px;
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
            color: white;
            flex-shrink: 0;
        }
        .kpi-card .icon.blue { background: #3182ce; }
        .kpi-card .icon.green { background: #38a169; }
        .kpi-card .icon.orange { background: #dd6b20; }
        .kpi-card .icon.purple { background: #805ad5; }
        .kpi-card .info h3 {
            font-size: 1.5rem;
            font-weight: 700;
            color: var(--gray-800);
            margin-bottom: 0.25rem;
        }
        .kpi-card .info p {
            font-size: 0.85rem;
            color: var(--gray-600);
        }

        /* Section headers */
        .section-title {
            font-size: 1.2rem;
            font-weight: 600;
            color: var(--gray-800);
            margin: 1.5rem 0 0.75rem;
        }
        body.dark .section-title { color: white; }

        /* Widget grid */
        .widget-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 1rem;
        }
        .widget {
            background: white;
            border-radius: var(--border-radius);
            box-shadow: var(--card-shadow);
            padding: 1.25rem;
        }
        .widget h4 {
            font-size: 0.95rem;
            font-weight: 600;
            color: var(--gray-600);
            margin-bottom: 0.75rem;
        }
        .widget .chart-container {
            position: relative;
            height: 200px;
        }

        /* Quick actions */
        .quick-actions {
            display: flex;
            flex-wrap: wrap;
            gap: 0.5rem;
            margin-bottom: 1.5rem;
        }
        .quick-actions button {
            background: white;
            border: 1px solid var(--gray-200);
            border-radius: 9999px;
            padding: 0.5rem 1rem;
            font-size: 0.85rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 0.5rem;
            transition: var(--transition);
        }
        .quick-actions button:hover {
            background: var(--primary-light);
            border-color: var(--primary);
            color: var(--primary);
        }

        /* Table (for leads, clients) */
        .data-table {
            width: 100%;
            border-collapse: collapse;
            background: white;
            border-radius: var(--border-radius);
            overflow: hidden;
            box-shadow: var(--card-shadow);
        }
        .data-table th,
        .data-table td {
            padding: 0.75rem 1rem;
            text-align: left;
            font-size: 0.9rem;
            border-bottom: 1px solid var(--gray-200);
        }
        .data-table th {
            background: var(--gray-100);
            color: var(--gray-600);
            font-weight: 600;
        }
        .status-badge {
            padding: 0.25rem 0.75rem;
            border-radius: 9999px;
            font-size: 0.75rem;
            font-weight: 500;
        }
        .status-new { background: #bee3f8; color: #2b6cb0; }
        .status-contacted { background: #fefcbf; color: #975a16; }
        .status-won { background: #c6f6d5; color: #22543d; }
        .status-lost { background: #fed7d7; color: #822727; }

        /* Kanban - simplified */
        .kanban {
            display: flex;
            gap: 1rem;
            overflow-x: auto;
            padding-bottom: 1rem;
        }
        .kanban-column {
            min-width: 250px;
            background: var(--gray-100);
            border-radius: var(--border-radius);
            padding: 0.75rem;
        }
        .kanban-column h5 {
            font-size: 0.9rem;
            margin-bottom: 0.75rem;
            color: var(--gray-600);
        }
        .kanban-card {
            background: white;
            border-radius: 8px;
            padding: 0.75rem;
            margin-bottom: 0.5rem;
            box-shadow: 0 1px 2px rgba(0,0,0,0.05);
            cursor: grab;
        }
        .kanban-card .deal-value { font-weight: 600; color: var(--gray-800); }

        /* Mobile bottom nav */
        .bottom-nav {
            display: none;
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: white;
            border-top: 1px solid var(--gray-200);
            height: var(--bottom-nav-height);
            justify-content: space-around;
            align-items: center;
            z-index: 200;
            padding-bottom: env(safe-area-inset-bottom);
        }
        .bottom-nav a {
            display: flex;
            flex-direction: column;
            align-items: center;
            font-size: 0.75rem;
            color: var(--gray-600);
            text-decoration: none;
            gap: 2px;
        }
        .bottom-nav a.active {
            color: var(--primary);
            font-weight: 500;
        }

        /* Responsive */
        @media (max-width: 768px) {
            .sidebar {
                position: fixed;
                left: 0;
                top: 0;
                bottom: 0;
                transform: translateX(-100%);
                width: 80%;
                max-width: 300px;
            }
            .sidebar.open { transform: translateX(0); }
            .main { width: 100%; }
            .bottom-nav { display: flex; }
            .topbar { padding: 0 0.75rem; }
            .content { padding: 1rem; padding-bottom: calc(var(--bottom-nav-height) + 1rem); }
            .topbar .search { max-width: 100%; }
        }

        /* Scrollbar */
        ::-webkit-scrollbar { width: 8px; height: 8px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--gray-300); border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: var(--gray-600); }
    </style>
</head>
<body>
    <!-- Sidebar -->
    <div class="sidebar" id="sidebar">
        <div class="sidebar-header">
            <i class="fas fa-bolt logo"></i>
            <h1>AgencyFlow</h1>
        </div>
        <nav class="sidebar-nav">
            <a href="#" class="active" data-page="dashboard"><i class="fas fa-home"></i> Dashboard</a>
            <a href="#" data-page="leads"><i class="fas fa-filter"></i> Leads</a>
            <a href="#" data-page="clients"><i class="fas fa-building"></i> Clients</a>
            <a href="#" data-page="sales"><i class="fas fa-chart-line"></i> Sales</a>
            <a href="#" data-page="projects"><i class="fas fa-folder-open"></i> Projects</a>
            <a href="#" data-page="tasks"><i class="fas fa-check-square"></i> Tasks</a>
            <a href="#" data-page="content"><i class="fas fa-calendar-alt"></i> Content</a>
            <a href="#" data-page="campaigns"><i class="fas fa-bullhorn"></i> Campaigns</a>
            <a href="#" data-page="reports"><i class="fas fa-file-alt"></i> Reports</a>
            <a href="#" data-page="invoices"><i class="fas fa-file-invoice"></i> Invoices</a>
            <a href="#" data-page="messages"><i class="fas fa-comments"></i> Messages</a>
            <a href="#" data-page="calendar"><i class="fas fa-calendar"></i> Calendar</a>
            <a href="#" data-page="files"><i class="fas fa-folder"></i> Files</a>
            <a href="#" data-page="settings"><i class="fas fa-cog"></i> Settings</a>
        </nav>
    </div>

    <!-- Main Area -->
    <div class="main">
        <!-- Topbar -->
        <div class="topbar">
            <button class="icon-btn" id="sidebarToggle" style="background:none; border:none; font-size:1.2rem; cursor:pointer;">
                <i class="fas fa-bars"></i>
            </button>
            <div class="search">
                <i class="fas fa-search"></i>
                <input type="text" placeholder="Search... (Ctrl+K)" id="globalSearch">
            </div>
            <div class="actions">
                <button class="icon-btn" id="themeToggle" title="Toggle dark mode"><i class="fas fa-moon"></i></button>
                <button class="icon-btn"><i class="fas fa-bell"></i><span class="badge">3</span></button>
                <div class="avatar">A</div>
            </div>
        </div>

        <!-- Content -->
        <div class="content" id="content">
            <!-- Dynamically loaded content goes here -->
        </div>
    </div>

    <!-- Mobile Bottom Navigation -->
    <div class="bottom-nav" id="bottomNav">
        <a href="#" class="active" data-page="dashboard"><i class="fas fa-home"></i> Dashboard</a>
        <a href="#" data-page="leads"><i class="fas fa-filter"></i> Leads</a>
        <a href="#" data-page="clients"><i class="fas fa-building"></i> Clients</a>
        <a href="#" data-page="tasks"><i class="fas fa-check-square"></i> Tasks</a>
        <a href="#" data-page="more" id="moreLink"><i class="fas fa-ellipsis-h"></i> More</a>
    </div>

    <!-- JavaScript -->
    <script>
        // Sample data for demo
        const sampleData = {
            leads: [
                { name: 'Rahul Sharma', company: 'Sharma Textiles', phone: '+91 98765 43210', email: 'rahul@sharmatextiles.com', service: 'SEO', source: 'Website', budget: '₹30,000', status: 'New', assignee: 'Amit', followup: '2025-02-10' },
                { name: 'Priya Patel', company: 'Spice Villa Restaurant', phone: '+91 91234 56789', email: 'priya@spicevilla.in', service: 'Social Media', source: 'Instagram', budget: '₹25,000', status: 'Contacted', assignee: 'Sneha', followup: '2025-02-11' },
                { name: 'Vikram Singh', company: 'BuildRight Constructions', phone: '+91 99887 77665', email: 'vikram@buildright.in', service: 'Google Ads', source: 'Google Ads', budget: '₹50,000', status: 'Qualified', assignee: 'Amit', followup: '2025-02-10' },
                { name: 'Anita Desai', company: 'Desai Jewels', phone: '+91 98111 22334', email: 'anita@desaijewels.com', service: 'SEO + Social', source: 'Referral', budget: '₹45,000', status: 'Proposal Sent', assignee: 'Raj', followup: '2025-02-12' },
                { name: 'Kiran Kumar', company: 'Kumar Fitness', phone: '+91 90555 66778', email: 'kiran@kumarfitness.in', service: 'Content Marketing', source: 'WhatsApp', budget: '₹20,000', status: 'Won', assignee: 'Sneha', followup: '2025-02-15' },
            ],
            clients: [
                { company: 'Kumar Fitness', contact: 'Kiran Kumar', service: 'Content Marketing', status: 'Active', monthlyFee: '₹20,000', manager: 'Sneha' },
                { company: 'Sharma Textiles', contact: 'Rahul Sharma', service: 'SEO', status: 'Active', monthlyFee: '₹30,000', manager: 'Amit' },
                { company: 'Spice Villa', contact: 'Priya Patel', service: 'Social Media', status: 'Active', monthlyFee: '₹25,000', manager: 'Sneha' },
                { company: 'BuildRight', contact: 'Vikram Singh', service: 'Google Ads', status: 'On Hold', monthlyFee: '₹50,000', manager: 'Amit' },
            ],
            tasks: [
                { name: 'Create social media calendar for Spice Villa', client: 'Spice Villa', due: '2025-02-10', priority: 'High', status: 'In Progress', assignee: 'Sneha' },
                { name: 'Write meta descriptions for Sharma Textiles', client: 'Sharma Textiles', due: '2025-02-11', priority: 'Medium', status: 'To Do', assignee: 'Raj' },
                { name: 'Set up Google Ads campaign for BuildRight', client: 'BuildRight', due: '2025-02-12', priority: 'Urgent', status: 'To Do', assignee: 'Amit' },
                { name: 'Design logo for Kumar Fitness', client: 'Kumar Fitness', due: '2025-02-14', priority: 'High', status: 'Review', assignee: 'Priya' },
            ]
        };

        // Navigation
        let currentPage = 'dashboard';
        const contentDiv = document.getElementById('content');
        const sidebarLinks = document.querySelectorAll('.sidebar-nav a');
        const bottomNavLinks = document.querySelectorAll('.bottom-nav a');
        const sidebar = document.getElementById('sidebar');
        const sidebarToggle = document.getElementById('sidebarToggle');

        function renderDashboard() {
            const kpiCards = [
                { icon: 'fa-user-plus', color: 'blue', value: '3', label: 'New Leads' },
                { icon: 'fa-clock', color: 'orange', value: '5', label: 'Follow-ups Today' },
                { icon: 'fa-building', color: 'green', value: '4', label: 'Active Clients' },
                { icon: 'fa-folder-open', color: 'purple', value: '6', label: 'Active Projects' },
                { icon: 'fa-check-square', color: 'blue', value: '8', label: 'Tasks Due Today' },
                { icon: 'fa-hourglass-half', color: 'orange', value: '2', label: 'Pending Approvals' },
                { icon: 'fa-rupee-sign', color: 'green', value: '₹1,25,000', label: 'Monthly Revenue' },
                { icon: 'fa-exclamation-circle', color: 'red', value: '₹45,000', label: 'Outstanding Payments' },
                { icon: 'fa-bullhorn', color: 'purple', value: '3', label: 'Campaigns Running' },
            ];

            return `
                <div class="quick-actions">
                    <button><i class="fas fa-user-plus"></i> Add Lead</button>
                    <button><i class="fas fa-building"></i> Add Client</button>
                    <button><i class="fas fa-chart-line"></i> Create Deal</button>
                    <button><i class="fas fa-folder-open"></i> Create Project</button>
                    <button><i class="fas fa-check-square"></i> Add Task</button>
                    <button><i class="fas fa-calendar-alt"></i> Add Content</button>
                    <button><i class="fas fa-file-invoice"></i> Create Invoice</button>
                    <button><i class="fas fa-file-alt"></i> Create Report</button>
                </div>

                <div class="kpi-grid">
                    ${kpiCards.map(card => `
                        <div class="kpi-card">
                            <div class="icon ${card.color}"><i class="fas ${card.icon}"></i></div>
                            <div class="info">
                                <h3>${card.value}</h3>
                                <p>${card.label}</p>
                            </div>
                        </div>
                    `).join('')}
                </div>

                <h2 class="section-title">Sales Overview</h2>
                <div class="widget-grid">
                    <div class="widget">
                        <h4>Pipeline Value & Won Revenue</h4>
                        <div class="chart-container"><canvas id="salesChart"></canvas></div>
                    </div>
                    <div class="widget">
                        <h4>Lead Conversion Funnel</h4>
                        <div class="chart-container"><canvas id="funnelChart"></canvas></div>
                    </div>
                    <div class="widget">
                        <h4>Task Completion</h4>
                        <div class="chart-container"><canvas id="taskChart"></canvas></div>
                    </div>
                </div>

                <h2 class="section-title">Today's Follow-ups</h2>
                <div class="widget" style="margin-bottom:1rem;">
                    <table class="data-table">
                        <thead><tr><th>Lead/Client</th><th>Type</th><th>Time</th><th>Assigned To</th></tr></thead>
                        <tbody>
                            <tr><td>Rahul Sharma</td><td>Call</td><td>10:30 AM</td><td>Amit</td></tr>
                            <tr><td>Priya Patel</td><td>WhatsApp</td><td>12:00 PM</td><td>Sneha</td></tr>
                            <tr><td>Vikram Singh</td><td>Email</td><td>3:30 PM</td><td>Amit</td></tr>
                        </tbody>
                    </table>
                </div>
            `;
        }

        function renderLeads() {
            const leads = sampleData.leads;
            return `
                <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:1rem;">
                    <h2 class="section-title" style="margin:0;">Leads</h2>
                    <button class="btn" style="background:var(--primary); color:white; padding:0.5rem 1rem; border:none; border-radius:8px; cursor:pointer;"><i class="fas fa-plus"></i> Add Lead</button>
                </div>
                <div class="widget" style="margin-bottom:1rem;">
                    <table class="data-table">
                        <thead><tr><th>Name</th><th>Company</th><th>Service</th><th>Source</th><th>Budget</th><th>Status</th><th>Next Follow-up</th></tr></thead>
                        <tbody>
                            ${leads.map(lead => `
                                <tr>
                                    <td>${lead.name}</td>
                                    <td>${lead.company}</td>
                                    <td>${lead.service}</td>
                                    <td>${lead.source}</td>
                                    <td>${lead.budget}</td>
                                    <td><span class="status-badge status-${lead.status.toLowerCase().replace(' ', '-')}">${lead.status}</span></td>
                                    <td>${lead.followup}</td>
                                </tr>
                            `).join('')}
                        </tbody>
                    </table>
                </div>
                <div class="kanban" style="margin-top:1rem;">
                    <div class="kanban-column"><h5>New (2)</h5>
                        <div class="kanban-card"><strong>Rahul Sharma</strong><br>SEO<br><span class="deal-value">₹30,000</span></div>
                    </div>
                    <div class="kanban-column"><h5>Contacted (1)</h5>
                        <div class="kanban-card"><strong>Priya Patel</strong><br>Social Media<br><span class="deal-value">₹25,000</span></div>
                    </div>
                    <div class="kanban-column"><h5>Qualified (1)</h5>
                        <div class="kanban-card"><strong>Vikram Singh</strong><br>Google Ads<br><span class="deal-value">₹50,000</span></div>
                    </div>
                    <div class="kanban-column"><h5>Proposal Sent (1)</h5>
                        <div class="kanban-card"><strong>Anita Desai</strong><br>SEO + Social<br><span class="deal-value">₹45,000</span></div>
                    </div>
                    <div class="kanban-column"><h5>Won (1)</h5>
                        <div class="kanban-card"><strong>Kiran Kumar</strong><br>Content Marketing<br><span class="deal-value">₹20,000</span></div>
                    </div>
                    <div class="kanban-column"><h5>Lost (0)</h5></div>
                </div>
            `;
        }

        function renderClients() {
            const clients = sampleData.clients;
            return `
                <h2 class="section-title">Clients</h2>
                <table class="data-table">
                    <thead><tr><th>Company</th><th>Contact Person</th><th>Service</th><th>Status</th><th>Monthly Fee</th><th>Manager</th></tr></thead>
                    <tbody>
                        ${clients.map(client => `
                            <tr>
                                <td>${client.company}</td>
                                <td>${client.contact}</td>
                                <td>${client.service}</td>
                                <td><span class="status-badge status-${client.status.toLowerCase().replace(' ', '-')}">${client.status}</span></td>
                                <td>${client.monthlyFee}</td>
                                <td>${client.manager}</td>
                            </tr>
                        `).join('')}
                    </tbody>
                </table>
            `;
        }

        function renderTasks() {
            const tasks = sampleData.tasks;
            return `
                <h2 class="section-title">Tasks</h2>
                <table class="data-table">
                    <thead><tr><th>Task</th><th>Client</th><th>Due Date</th><th>Priority</th><th>Status</th><th>Assignee</th></tr></thead>
                    <tbody>
                        ${tasks.map(task => `
                            <tr>
                                <td>${task.name}</td>
                                <td>${task.client}</td>
                                <td>${task.due}</td>
                                <td>${task.priority}</td>
                                <td>${task.status}</td>
                                <td>${task.assignee}</td>
                            </tr>
                        `).join('')}
                    </tbody>
                </table>
            `;
        }

        // Generic render for other pages (placeholder)
        function renderPlaceholder(pageName) {
            return `<h2 class="section-title">${pageName.charAt(0).toUpperCase() + pageName.slice(1)}</h2><p>This module is part of the full version. Prototype demonstrates dashboard, leads, clients, and tasks.</p>`;
        }

        function renderPage(page) {
            switch(page) {
                case 'dashboard': return renderDashboard();
                case 'leads': return renderLeads();
                case 'clients': return renderClients();
                case 'tasks': return renderTasks();
                default: return renderPlaceholder(page);
            }
        }

        function setActivePage(page) {
            currentPage = page;
            contentDiv.innerHTML = renderPage(page);
            // Update active states
            sidebarLinks.forEach(link => link.classList.remove('active'));
            bottomNavLinks.forEach(link => link.classList.remove('active'));
            document.querySelectorAll(`[data-page="${page}"]`).forEach(el => el.classList.add('active'));
            // Close sidebar on mobile
            sidebar.classList.remove('open');
            // Re-render charts if needed
            if (page === 'dashboard') {
                setTimeout(initCharts, 100);
            }
        }

        // Initial render
        setActivePage('dashboard');

        // Event listeners
        sidebarLinks.forEach(link => {
            link.addEventListener('click', (e) => {
                e.preventDefault();
                setActivePage(link.dataset.page);
            });
        });
        bottomNavLinks.forEach(link => {
            link.addEventListener('click', (e) => {
                e.preventDefault();
                if (link.id === 'moreLink') {
                    sidebar.classList.toggle('open');
                } else {
                    setActivePage(link.dataset.page);
                }
            });
        });
        sidebarToggle.addEventListener('click', () => {
            sidebar.classList.toggle('open');
        });
        document.getElementById('themeToggle').addEventListener('click', () => {
            document.body.classList.toggle('dark');
        });

        // Charts initialization
        function initCharts() {
            // Sales Chart
            const salesCtx = document.getElementById('salesChart');
            if (salesCtx) {
                new Chart(salesCtx, {
                    type: 'line',
                    data: {
                        labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
                        datasets: [
                            { label: 'Won Revenue', data: [80000, 95000, 110000, 105000, 125000, 130000], borderColor: '#38a169', backgroundColor: 'rgba(56,161,105,0.1)', tension: 0.3 },
                            { label: 'Pipeline Value', data: [120000, 140000, 160000, 180000, 170000, 190000], borderColor: '#2B6CB0', backgroundColor: 'rgba(43,108,176,0.1)', tension: 0.3 }
                        ]
                    },
                    options: { responsive: true, maintainAspectRatio: false }
                });
            }
            // Funnel Chart
            const funnelCtx = document.getElementById('funnelChart');
            if (funnelCtx) {
                new Chart(funnelCtx, {
                    type: 'bar',
                    data: {
                        labels: ['New', 'Contacted', 'Qualified', 'Proposal', 'Won'],
                        datasets: [{ label: 'Leads', data: [50, 35, 25, 15, 8], backgroundColor: ['#bee3f8', '#90cdf4', '#63b3ed', '#3182ce', '#2b6cb0'] }]
                    },
                    options: { indexAxis: 'y', responsive: true, maintainAspectRatio: false }
                });
            }
            // Task Chart
            const taskCtx = document.getElementById('taskChart');
            if (taskCtx) {
                new Chart(taskCtx, {
                    type: 'doughnut',
                    data: {
                        labels: ['Completed', 'In Progress', 'To Do', 'Overdue'],
                        datasets: [{ data: [12, 8, 15, 3], backgroundColor: ['#38a169', '#3182ce', '#ecc94b', '#e53e3e'] }]
                    },
                    options: { responsive: true, maintainAspectRatio: false }
                });
            }
        }

        // Global search (simple alert)
        document.getElementById('globalSearch').addEventListener('keydown', (e) => {
            if (e.key === 'Enter') {
                alert('Search functionality will be implemented in full version.');
            }
        });

        // Ctrl+K focus
        document.addEventListener('keydown', (e) => {
            if ((e.ctrlKey || e.metaKey) && e.key === 'k') {
                e.preventDefault();
                document.getElementById('globalSearch').focus();
            }
        });
    </script>
</body>
</html>
