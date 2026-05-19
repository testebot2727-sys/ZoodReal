<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FinFlow - Controle Financeiro Inteligente</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&family=Space+Grotesk:wght@500;600;700&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        bg: '#f8fafb',
                        card: '#ffffff',
                        border: '#e8ecf0',
                        fg: '#0f172a',
                        muted: '#64748b',
                        accent: '#0d9488',
                        accentLight: '#14b8a6',
                        accentDark: '#0f766e',
                        success: '#10b981',
                        warning: '#f59e0b',
                        danger: '#ef4444',
                        income: '#10b981',
                        expense: '#f43f5e'
                    },
                    fontFamily: {
                        display: ['Space Grotesk', 'sans-serif'],
                        body: ['Plus Jakarta Sans', 'sans-serif']
                    }
                }
            }
        }
    </script>
    <style>
        * { font-family: 'Plus Jakarta Sans', sans-serif; }
        .font-display { font-family: 'Space Grotesk', sans-serif; }
        
        :root {
            --bg: #f8fafb;
            --card: #ffffff;
            --border: #e8ecf0;
            --fg: #0f172a;
            --muted: #64748b;
            --accent: #0d9488;
            --accent-light: #14b8a6;
        }
        
        body {
            background: var(--bg);
            background-image: 
                radial-gradient(ellipse at 0% 0%, rgba(13, 148, 136, 0.06) 0%, transparent 50%),
                radial-gradient(ellipse at 100% 100%, rgba(20, 184, 166, 0.04) 0%, transparent 50%);
            min-height: 100vh;
        }
        
        .glass-card {
            background: rgba(255, 255, 255, 0.85);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(232, 236, 240, 0.8);
        }
        
        .sidebar-item {
            transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .sidebar-item:hover, .sidebar-item.active {
            background: linear-gradient(135deg, rgba(13, 148, 136, 0.1), rgba(20, 184, 166, 0.05));
            color: var(--accent);
        }
        
        .sidebar-item.active {
            border-left: 3px solid var(--accent);
        }
        
        .card-hover {
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .card-hover:hover {
            transform: translateY(-2px);
            box-shadow: 0 12px 40px -12px rgba(13, 148, 136, 0.15);
        }
        
        .btn-primary {
            background: linear-gradient(135deg, #0d9488 0%, #14b8a6 100%);
            transition: all 0.25s ease;
        }
        
        .btn-primary:hover {
            transform: translateY(-1px);
            box-shadow: 0 8px 24px -8px rgba(13, 148, 136, 0.5);
        }
        
        .btn-primary:active {
            transform: translateY(0);
        }
        
        .progress-bar {
            background: linear-gradient(90deg, #0d9488, #14b8a6);
            transition: width 0.6s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .input-field {
            transition: all 0.2s ease;
            border: 1.5px solid var(--border);
        }
        
        .input-field:focus {
            border-color: var(--accent);
            box-shadow: 0 0 0 3px rgba(13, 148, 136, 0.1);
            outline: none;
        }
        
        @keyframes slideUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        
        @keyframes scaleIn {
            from { opacity: 0; transform: scale(0.95); }
            to { opacity: 1; transform: scale(1); }
        }
        
        @keyframes confetti {
            0% { transform: translateY(0) rotate(0deg); opacity: 1; }
            100% { transform: translateY(-100px) rotate(720deg); opacity: 0; }
        }
        
        .animate-slide-up {
            animation: slideUp 0.5s ease forwards;
        }
        
        .animate-fade-in {
            animation: fadeIn 0.3s ease forwards;
        }
        
        .animate-scale-in {
            animation: scaleIn 0.3s ease forwards;
        }
        
        .stagger-1 { animation-delay: 0.05s; }
        .stagger-2 { animation-delay: 0.1s; }
        .stagger-3 { animation-delay: 0.15s; }
        .stagger-4 { animation-delay: 0.2s; }
        .stagger-5 { animation-delay: 0.25s; }
        
        .modal-overlay {
            background: rgba(15, 23, 42, 0.5);
            backdrop-filter: blur(4px);
        }
        
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border); border-radius: 3px; }
        ::-webkit-scrollbar-thumb:hover { background: var(--muted); }
        
        .balance-glow {
            position: relative;
        }
        
        .balance-glow::before {
            content: '';
            position: absolute;
            inset: -2px;
            background: linear-gradient(135deg, #0d9488, #14b8a6, #0d9488);
            border-radius: inherit;
            z-index: -1;
            opacity: 0.2;
            filter: blur(10px);
        }
        
        .status-paid { background: linear-gradient(135deg, #d1fae5, #a7f3d0); color: #065f46; }
        .status-pending { background: linear-gradient(135deg, #fef3c7, #fde68a); color: #92400e; }
        .status-overdue { background: linear-gradient(135deg, #fee2e2, #fecaca); color: #991b1b; }
        
        @media (prefers-reduced-motion: reduce) {
            *, *::before, *::after {
                animation-duration: 0.01ms !important;
                transition-duration: 0.01ms !important;
            }
        }

        .mobile-nav {
            box-shadow: 0 -4px 30px -5px rgba(0, 0, 0, 0.08);
        }
    </style>
</head>
<body class="text-fg antialiased">
    <!-- Auth Screen -->
    <div id="authScreen" class="min-h-screen flex items-center justify-center p-4">
        <div class="w-full max-w-md animate-scale-in">
            <!-- Logo -->
            <div class="text-center mb-8">
                <div class="inline-flex items-center justify-center w-16 h-16 rounded-2xl bg-gradient-to-br from-accent to-accentLight mb-4 shadow-lg shadow-accent/20">
                    <svg class="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/>
                    </svg>
                </div>
                <h1 class="font-display text-3xl font-bold text-fg">FinFlow</h1>
                <p class="text-muted mt-1">Controle financeiro inteligente</p>
            </div>

            <!-- Auth Card -->
            <div class="bg-card rounded-3xl p-8 shadow-xl shadow-black/5 border border-border">
                <!-- Tabs -->
                <div class="flex mb-6 bg-bg rounded-xl p-1">
                    <button onclick="switchAuthTab('login')" id="loginTab" class="flex-1 py-2.5 px-4 rounded-lg text-sm font-semibold transition-all bg-card shadow-sm text-fg">Entrar</button>
                    <button onclick="switchAuthTab('register')" id="registerTab" class="flex-1 py-2.5 px-4 rounded-lg text-sm font-semibold transition-all text-muted">Criar conta</button>
                </div>

                <!-- Login Form -->
                <form id="loginForm" onsubmit="handleLogin(event)" class="space-y-4">
                    <div>
                        <label class="block text-sm font-medium text-fg mb-1.5">Email</label>
                        <input type="email" required placeholder="seu@email.com" class="input-field w-full px-4 py-3 rounded-xl bg-bg text-fg placeholder:text-muted/50">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-fg mb-1.5">Senha</label>
                        <input type="password" required placeholder="Sua senha" class="input-field w-full px-4 py-3 rounded-xl bg-bg text-fg placeholder:text-muted/50">
                    </div>
                    <button type="submit" class="btn-primary w-full py-3.5 rounded-xl text-white font-semibold text-sm">Entrar na conta</button>
                    
                    <button type="button" onclick="showForgotPassword()" class="w-full text-sm text-muted hover:text-accent transition-colors">Esqueceu sua senha?</button>
                </form>

                <!-- Register Form -->
                <form id="registerForm" onsubmit="handleRegister(event)" class="space-y-4 hidden">
                    <div>
                        <label class="block text-sm font-medium text-fg mb-1.5">Nome completo</label>
                        <input type="text" required placeholder="Seu nome" class="input-field w-full px-4 py-3 rounded-xl bg-bg text-fg placeholder:text-muted/50">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-fg mb-1.5">Email</label>
                        <input type="email" required placeholder="seu@email.com" class="input-field w-full px-4 py-3 rounded-xl bg-bg text-fg placeholder:text-muted/50">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-fg mb-1.5">Senha</label>
                        <input type="password" required placeholder="Mínimo 6 caracteres" class="input-field w-full px-4 py-3 rounded-xl bg-bg text-fg placeholder:text-muted/50">
                    </div>
                    <button type="submit" class="btn-primary w-full py-3.5 rounded-xl text-white font-semibold text-sm">Criar minha conta</button>
                </form>

                <!-- Divider -->
                <div class="relative my-6">
                    <div class="absolute inset-0 flex items-center"><div class="w-full border-t border-border"></div></div>
                    <div class="relative flex justify-center"><span class="px-4 bg-card text-xs text-muted uppercase tracking-wider">ou continue com</span></div>
                </div>

                <!-- Google Login -->
                <button onclick="handleGoogleLogin()" class="w-full flex items-center justify-center gap-3 py-3 px-4 rounded-xl border border-border hover:bg-bg transition-colors group">
                    <svg class="w-5 h-5" viewBox="0 0 24 24"><path fill="#4285F4" d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z"/><path fill="#34A853" d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z"/><path fill="#FBBC05" d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z"/><path fill="#EA4335" d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z"/></svg>
                    <span class="text-sm font-medium text-fg group-hover:text-accent transition-colors">Google</span>
                </button>
            </div>
            
            <p class="text-center text-xs text-muted mt-6">Ao continuar, você concorda com nossos Termos de Uso</p>
        </div>
    </div>

    <!-- Main App -->
    <div id="mainApp" class="hidden min-h-screen flex">
        <!-- Sidebar Desktop -->
        <aside id="sidebarDesktop" class="hidden lg:flex flex-col w-64 bg-card border-r border-border fixed h-screen z-40">
            <div class="p-6 border-b border-border">
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 rounded-xl bg-gradient-to-br from-accent to-accentLight flex items-center justify-center shadow-lg shadow-accent/20">
                        <svg class="w-5 h-5 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/>
                        </svg>
                    </div>
                    <span class="font-display text-xl font-bold">FinFlow</span>
                </div>
            </div>
            
            <nav class="flex-1 p-4 space-y-1">
                <button onclick="showSection('dashboard')" data-section="dashboard" class="sidebar-item active w-full flex items-center gap-3 px-4 py-3 rounded-xl text-left text-sm font-medium">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 5a1 1 0 011-1h14a1 1 0 011 1v2a1 1 0 01-1 1H5a1 1 0 01-1-1V5zM4 13a1 1 0 011-1h6a1 1 0 011 1v6a1 1 0 01-1 1H5a1 1 0 01-1-1v-6zM16 13a1 1 0 011-1h2a1 1 0 011 1v6a1 1 0 01-1 1h-2a1 1 0 01-1-1v-6z"/></svg>
                    Dashboard
                </button>
                <button onclick="showSection('transactions')" data-section="transactions" class="sidebar-item w-full flex items-center gap-3 px-4 py-3 rounded-xl text-left text-sm font-medium text-muted">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 16V4m0 0L3 8m4-4l4 4m6 0v12m0 0l4-4m-4 4l-4-4"/></svg>
                    Transacoes
                </button>
                <button onclick="showSection('bills')" data-section="bills" class="sidebar-item w-full flex items-center gap-3 px-4 py-3 rounded-xl text-left text-sm font-medium text-muted">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-6 9l2 2 4-4"/></svg>
                    Contas
                </button>
                <button onclick="showSection('debts')" data-section="debts" class="sidebar-item w-full flex items-center gap-3 px-4 py-3 rounded-xl text-left text-sm font-medium text-muted">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10"/></svg>
                    Dividas
                </button>
                <button onclick="showSection('budget')" data-section="budget" class="sidebar-item w-full flex items-center gap-3 px-4 py-3 rounded-xl text-left text-sm font-medium text-muted">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z"/></svg>
                    Orcamento
                </button>
                <button onclick="showSection('reports')" data-section="reports" class="sidebar-item w-full flex items-center gap-3 px-4 py-3 rounded-xl text-left text-sm font-medium text-muted">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 17v-2m3 2v-4m3 4v-6m2 10H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"/></svg>
                    Relatorios
                </button>
            </nav>
            
            <div class="p-4 border-t border-border">
                <button onclick="handleLogout()" class="w-full flex items-center gap-3 px-4 py-3 rounded-xl text-left text-sm font-medium text-muted hover:text-danger hover:bg-danger/5 transition-colors">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1"/></svg>
                    Sair
                </button>
            </div>
        </aside>

        <!-- Main Content -->
        <main class="flex-1 lg:ml-64 pb-24 lg:pb-8">
            <!-- Header Mobile -->
            <header class="lg:hidden sticky top-0 z-30 bg-card/90 backdrop-blur-lg border-b border-border px-4 py-3">
                <div class="flex items-center justify-between">
                    <div class="flex items-center gap-2">
                        <div class="w-8 h-8 rounded-lg bg-gradient-to-br from-accent to-accentLight flex items-center justify-center">
                            <svg class="w-4 h-4 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/>
                            </svg>
                        </div>
                        <span class="font-display font-bold">FinFlow</span>
                    </div>
                    <button onclick="toggleMobileMenu()" class="p-2 rounded-lg hover:bg-bg transition-colors">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"/></svg>
                    </button>
                </div>
            </header>

            <!-- Dashboard Section -->
            <section id="section-dashboard" class="tab-content active p-4 lg:p-8 space-y-6">
                <!-- Welcome & Balance -->
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <!-- Balance Card -->
                    <div class="lg:col-span-1 balance-glow bg-gradient-to-br from-accent via-accentDark to-teal-800 rounded-3xl p-6 text-white animate-slide-up opacity-0 stagger-1">
                        <p class="text-white/70 text-sm font-medium mb-1">Saldo atual</p>
                        <h2 class="font-display text-4xl font-bold mb-4" id="currentBalance">R$ 12.450,00</h2>
                        <div class="flex gap-4 text-sm">
                            <div>
                                <p class="text-white/60">Receitas</p>
                                <p class="font-semibold text-emerald-200" id="totalIncome">+R$ 8.900,00</p>
                            </div>
                            <div>
                                <p class="text-white/60">Despesas</p>
                                <p class="font-semibold text-red-200" id="totalExpense">-R$ 5.450,00</p>
                            </div>
                        </div>
                    </div>

                    <!-- Monthly Summary -->
                    <div class="lg:col-span-2 grid grid-cols-2 sm:grid-cols-4 gap-4">
                        <div class="bg-card rounded-2xl p-5 border border-border card-hover animate-slide-up opacity-0 stagger-2">
                            <div class="w-10 h-10 rounded-xl bg-income/10 flex items-center justify-center mb-3">
                                <svg class="w-5 h-5 text-income" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 11l5-5m0 0l5 5m-5-5v12"/></svg>
                            </div>
                            <p class="text-xs text-muted font-medium mb-1">Este mes</p>
                            <p class="font-display text-xl font-bold text-income" id="monthIncome">R$ 8.900</p>
                            <p class="text-xs text-success mt-1">+12% vs anterior</p>
                        </div>
                        
                        <div class="bg-card rounded-2xl p-5 border border-border card-hover animate-slide-up opacity-0 stagger-3">
                            <div class="w-10 h-10 rounded-xl bg-expense/10 flex items-center justify-center mb-3">
                                <svg class="w-5 h-5 text-expense" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 13l-5 5m0 0l-5-5m5 5V6"/></svg>
                            </div>
                            <p class="text-xs text-muted font-medium mb-1">Gastos</p>
                            <p class="font-display text-xl font-bold text-expense" id="monthExpense">R$ 5.450</p>
                            <p class="text-xs text-danger mt-1">+5% vs anterior</p>
                        </div>
                        
                        <div class="bg-card rounded-2xl p-5 border border-border card-hover animate-slide-up opacity-0 stagger-4">
                            <div class="w-10 h-10 rounded-xl bg-accent/10 flex items-center justify-center mb-3">
                                <svg class="w-5 h-5 text-accent" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 7h6m0 10v-3m-3 3h.01M9 17h.01M9 14h.01M12 14h.01M15 11h.01M12 11h.01M9 11h.01M7 21h10a2 2 0 002-2V5a2 2 0 00-2-2H7a2 2 0 00-2 2v14a2 2 0 002 2z"/></svg>
                            </div>
                            <p class="text-xs text-muted font-medium mb-1">Contas pendentes</p>
                            <p class="font-display text-xl font-bold text-warning" id="pendingBills">4</p>
                            <p class="text-xs text-muted mt-1">R$ 2.340 total</p>
                        </div>
                        
                        <div class="bg-card rounded-2xl p-5 border border-border card-hover animate-slide-up opacity-0 stagger-5">
                            <div class="w-10 h-10 rounded-xl bg-danger/10 flex items-center justify-center mb-3">
                                <svg class="w-5 h-5 text-danger" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/></svg>
                            </div>
                            <p class="text-xs text-muted font-medium mb-1">Dividas ativas</p>
                            <p class="font-display text-xl font-bold text-danger" id="activeDebts">3</p>
                            <p class="text-xs text-muted mt-1">R$ 15.800 total</p>
                        </div>
                    </div>
                </div>

                <!-- Charts Row -->
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                    <!-- Category Pie Chart -->
                    <div class="bg-card rounded-2xl p-6 border border-border animate-slide-up opacity-0" style="animation-delay: 0.3s;">
                        <h3 class="font-display font-semibold text-lg mb-4">Gastos por categoria</h3>
                        <div class="relative h-64">
                            <canvas id="categoryChart"></canvas>
                        </div>
                    </div>
                    
                    <!-- Evolution Line Chart -->
                    <div class="bg-card rounded-2xl p-6 border border-border animate-slide-up opacity-0" style="animation-delay: 0.35s;">
                        <h3 class="font-display font-semibold text-lg mb-4">Evolucao mensal</h3>
                        <div class="relative h-64">
                            <canvas id="evolutionChart"></canvas>
                        </div>
                    </div>
                </div>

                <!-- Upcoming Bills -->
                <div class="bg-card rounded-2xl p-6 border border-border animate-slide-up opacity-0" style="animation-delay: 0.4s;">
                    <div class="flex items-center justify-between mb-4">
                        <h3 class="font-display font-semibold text-lg">Proximas contas</h3>
                        <button onclick="showSection('bills')" class="text-sm text-accent hover:text-accentDark font-medium transition-colors">Ver todas</button>
                    </div>
                    <div class="space-y-3" id="upcomingBillsList">
                        <!-- Filled by JS -->
                    </div>
                </div>

                <!-- Recent Transactions -->
                <div class="bg-card rounded-2xl p-6 border border-border animate-slide-up opacity-0" style="animation-delay: 0.45s;">
                    <div class="flex items-center justify-between mb-4">
                        <h3 class="font-display font-semibold text-lg">Transacoes recentes</h3>
                        <button onclick="showSection('transactions')" class="text-sm text-accent hover:text-accentDark font-medium transition-colors">Ver todas</button>
                    </div>
                    <div class="space-y-3" id="recentTransactionsList">
                        <!-- Filled by JS -->
                    </div>
                </div>
            </section>

            <!-- Transactions Section -->
            <section id="section-transactions" class="tab-content p-4 lg:p-8 space-y-6">
                <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4">
                    <div>
                        <h2 class="font-display text-2xl font-bold">Transacoes</h2>
                        <p class="text-muted text-sm">Gerencie suas receitas e despesas</p>
                    </div>
                    <button onclick="openTransactionModal()" class="btn-primary px-5 py-2.5 rounded-xl text-white font-semibold text-sm inline-flex items-center gap-2">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"/></svg>
                        Nova transacao
                    </button>
                </div>

                <!-- Filters -->
                <div class="bg-card rounded-2xl p-4 border border-border">
                    <div class="grid grid-cols-1 sm:grid-cols-4 gap-4">
                        <select onchange="filterTransactions()" id="filterType" class="input-field px-4 py-2.5 rounded-xl bg-bg text-sm">
                            <option value="">Todos os tipos</option>
                            <option value="income">Receitas</option>
                            <option value="expense">Despesas</option>
                        </select>
                        <select onchange="filterTransactions()" id="filterCategory" class="input-field px-4 py-2.5 rounded-xl bg-bg text-sm">
                            <option value="">Todas categorias</option>
                            <option value="moradia">Moradia</option>
                            <option value="alimentacao">Alimentacao</option>
                            <option value="transporte">Transporte</option>
                            <option value="lazer">Lazer</option>
                            <option value="saude">Saude</option>
                            <option value="educacao">Educacao</option>
                            <option value="outros">Outros</option>
                        </select>
                        <input type="month" onchange="filterTransactions()" id="filterMonth" class="input-field px-4 py-2.5 rounded-xl bg-bg text-sm">
                        <button onclick="resetFilters()" class="px-4 py-2.5 rounded-xl border border-border hover:bg-bg transition-colors text-sm font-medium">Limpar filtros</button>
                    </div>
                </div>

                <!-- Transactions List -->
                <div class="bg-card rounded-2xl border border-border overflow-hidden">
                    <div class="divide-y divide-border" id="transactionsList">
                        <!-- Filled by JS -->
                    </div>
                </div>
            </section>

            <!-- Bills Section -->
            <section id="section-bills" class="tab-content p-4 lg:p-8 space-y-6">
                <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4">
                    <div>
                        <h2 class="font-display text-2xl font-bold">Contas a Pagar</h2>
                        <p class="text-muted text-sm">Controle suas contas e vencimentos</p>
                    </div>
                    <button onclick="openBillModal()" class="btn-primary px-5 py-2.5 rounded-xl text-white font-semibold text-sm inline-flex items-center gap-2">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"/></svg>
                        Nova conta
                    </button>
                </div>

                <!-- Bills Stats -->
                <div class="grid grid-cols-3 gap-4">
                    <div class="bg-card rounded-2xl p-4 border border-border text-center">
                        <p class="text-2xl font-display font-bold text-success" id="paidBillsCount">8</p>
                        <p class="text-xs text-muted">Pagas</p>
                    </div>
                    <div class="bg-card rounded-2xl p-4 border border-border text-center">
                        <p class="text-2xl font-display font-bold text-warning" id="pendingBillsCount">4</p>
                        <p class="text-xs text-muted">Pendentes</p>
                    </div>
                    <div class="bg-card rounded-2xl p-4 border border-border text-center">
                        <p class="text-2xl font-display font-bold text-danger" id="overdueBillsCount">1</p>
                        <p class="text-xs text-muted">Atrasadas</p>
                    </div>
                </div>

                <!-- Bills List -->
                <div class="space-y-3" id="billsList">
                    <!-- Filled by JS -->
                </div>
            </section>

            <!-- Debts Section -->
            <section id="section-debts" class="tab-content p-4 lg:p-8 space-y-6">
                <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4">
                    <div>
                        <h2 class="font-display text-2xl font-bold">Dividas</h2>
                        <p class="text-muted text-sm">Controle e elimine suas dividas</p>
                    </div>
                    <button onclick="openDebtModal()" class="btn-primary px-5 py-2.5 rounded-xl text-white font-semibold text-sm inline-flex items-center gap-2">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"/></svg>
                        Nova divida
                    </button>
                </div>

                <!-- Strategy Selector -->
                <div class="bg-card rounded-2xl p-6 border border-border">
                    <h3 class="font-display font-semibold mb-4">Estrategia de pagamento</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <label class="cursor-pointer">
                            <input type="radio" name="strategy" value="snowball" checked class="peer hidden" onchange="updateStrategy()">
                            <div class="p-4 rounded-xl border-2 border-border peer-checked:border-accent peer-checked:bg-accent/5 transition-all">
                                <div class="flex items-start gap-3">
                                    <div class="w-10 h-10 rounded-lg bg-accent/10 flex items-center justify-center shrink-0">
                                        <svg class="w-5 h-5 text-accent" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"/></svg>
                                    </div>
                                    <div>
                                        <p class="font-semibold">Bola de neve</p>
                                        <p class="text-sm text-muted">Pague primeiro as menores dividas para ganhar motivacao</p>
                                    </div>
                                </div>
                            </div>
                        </label>
                        <label class="cursor-pointer">
                            <input type="radio" name="strategy" value="avalanche" class="peer hidden" onchange="updateStrategy()">
                            <div class="p-4 rounded-xl border-2 border-border peer-checked:border-accent peer-checked:bg-accent/5 transition-all">
                                <div class="flex items-start gap-3">
                                    <div class="w-10 h-10 rounded-lg bg-accent/10 flex items-center justify-center shrink-0">
                                        <svg class="w-5 h-5 text-accent" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6"/></svg>
                                    </div>
                                    <div>
                                        <p class="font-semibold">Avalanche</p>
                                        <p class="text-sm text-muted">Pague primeiro as dividas com maiores juros</p>
                                    </div>
                                </div>
                            </div>
                        </label>
                    </div>
                </div>

                <!-- Debts List -->
                <div class="space-y-4" id="debtsList">
                    <!-- Filled by JS -->
                </div>
            </section>

            <!-- Budget Section -->
            <section id="section-budget" class="tab-content p-4 lg:p-8 space-y-6">
                <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4">
                    <div>
                        <h2 class="font-display text-2xl font-bold">Orcamento Mensal</h2>
                        <p class="text-muted text-sm">Defina limites por categoria</p>
                    </div>
                    <button onclick="openBudgetModal()" class="btn-primary px-5 py-2.5 rounded-xl text-white font-semibold text-sm inline-flex items-center gap-2">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"/></svg>
                        Definir limite
                    </button>
                </div>

                <!-- Budget Cards -->
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4" id="budgetList">
                    <!-- Filled by JS -->
                </div>
            </section>

            <!-- Reports Section -->
            <section id="section-reports" class="tab-content p-4 lg:p-8 space-y-6">
                <div>
                    <h2 class="font-display text-2xl font-bold">Relatorios</h2>
                    <p class="text-muted text-sm">Analise seu desempenho financeiro</p>
                </div>

                <!-- Month Comparison -->
                <div class="bg-card rounded-2xl p-6 border border-border">
                    <h3 class="font-display font-semibold text-lg mb-4">Comparativo mensal</h3>
                    <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-6">
                        <div class="text-center p-4 bg-bg rounded-xl">
                            <p class="text-sm text-muted">Receitas</p>
                            <p class="text-xl font-display font-bold text-income">R$ 8.900</p>
                            <p class="text-xs text-success">+12%</p>
                        </div>
                        <div class="text-center p-4 bg-bg rounded-xl">
                            <p class="text-sm text-muted">Despesas</p>
                            <p class="text-xl font-display font-bold text-expense">R$ 5.450</p>
                            <p class="text-xs text-danger">+5%</p>
                        </div>
                        <div class="text-center p-4 bg-bg rounded-xl">
                            <p class="text-sm text-muted">Saldo</p>
                            <p class="text-xl font-display font-bold text-accent">R$ 3.450</p>
                            <p class="text-xs text-success">+28%</p>
                        </div>
                        <div class="text-center p-4 bg-bg rounded-xl">
                            <p class="text-sm text-muted">Economia</p>
                            <p class="text-xl font-display font-bold text-success">38.7%</p>
                            <p class="text-xs text-success">+4.2pp</p>
                        </div>
                    </div>
                    <div class="h-72">
                        <canvas id="reportChart"></canvas>
                    </div>
                </div>

                <!-- Top Categories Growth -->
                <div class="bg-card rounded-2xl p-6 border border-border">
                    <h3 class="font-display font-semibold text-lg mb-4">Categorias em destaque</h3>
                    <div class="space-y-4" id="topCategoriesList">
                        <!-- Filled by JS -->
                    </div>
                </div>
            </section>
        </main>

        <!-- Mobile Bottom Navigation -->
        <nav class="lg:hidden mobile-nav fixed bottom-0 left-0 right-0 bg-card/95 backdrop-blur-lg border-t border-border z-50">
            <div class="flex justify-around py-2">
                <button onclick="showSection('dashboard')" data-mobile-nav="dashboard" class="mobile-nav-btn active flex flex-col items-center p-2 text-accent">
                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 5a1 1 0 011-1h14a1 1 0 011 1v2a1 1 0 01-1 1H5a1 1 0 01-1-1V5zM4 13a1 1 0 011-1h6a1 1 0 011 1v6a1 1 0 01-1 1H5a1 1 0 01-1-1v-6zM16 13a1 1 0 011-1h2a1 1 0 011 1v6a1 1 0 01-1 1h-2a1 1 0 01-1-1v-6z"/></svg>
                    <span class="text-[10px] font-medium mt-0.5">Home</span>
                </button>
                <button onclick="showSection('transactions')" data-mobile-nav="transactions" class="mobile-nav-btn flex flex-col items-center p-2 text-muted">
                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 16V4m0 0L3 8m4-4l4 4m6 0v12m0 0l4-4m-4 4l-4-4"/></svg>
                    <span class="text-[10px] font-medium mt-0.5">Mov.</span>
                </button>
                <button onclick="openTransactionModal()" class="flex flex-col items-center p-2 -mt-4">
                    <div class="w-12 h-12 rounded-full btn-primary flex items-center justify-center shadow-lg shadow-accent/30">
                        <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"/></svg>
                    </div>
                </button>
                <button onclick="showSection('bills')" data-mobile-nav="bills" class="mobile-nav-btn flex flex-col items-center p-2 text-muted">
                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-6 9l2 2 4-4"/></svg>
                    <span class="text-[10px] font-medium mt-0.5">Contas</span>
                </button>
                <button onclick="showSection('more')" data-mobile-nav="more" class="mobile-nav-btn flex flex-col items-center p-2 text-muted">
                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h7"/></svg>
                    <span class="text-[10px] font-medium mt-0.5">Mais</span>
                </button>
            </div>
        </nav>
    </div>

    <!-- Transaction Modal -->
    <div id="transactionModal" class="fixed inset-0 z-50 hidden">
        <div class="modal-overlay absolute inset-0" onclick="closeTransactionModal()"></div>
        <div class="absolute inset-x-4 top-[10%] md:inset-x-auto md:left-1/2 md:-translate-x-1/2 md:w-full md:max-w-lg">
            <div class="bg-card rounded-3xl shadow-2xl max-h-[80vh] overflow-y-auto animate-scale-in">
                <div class="p-6 border-b border-border flex items-center justify-between">
                    <h3 class="font-display text-xl font-bold" id="transactionModalTitle">Nova transacao</h3>
                    <button onclick="closeTransactionModal()" class="p-2 rounded-lg hover:bg-bg transition-colors">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/></svg>
                    </button>
                </div>
                <form onsubmit="saveTransaction(event)" class="p-6 space-y-4">
                    <input type="hidden" id="transactionId">
                    
                    <!-- Type Toggle -->
                    <div class="flex gap-2 p-1 bg-bg rounded-xl">
                        <button type="button" onclick="setTransactionType('expense')" id="typeExpenseBtn" class="flex-1 py-2.5 rounded-lg text-sm font-semibold transition-all bg-expense text-white">Despesa</button>
                        <button type="button" onclick="setTransactionType('income')" id="typeIncomeBtn" class="flex-1 py-2.5 rounded-lg text-sm font-semibold transition-all text-muted">Receita</button>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-fg mb-1.5">Valor</label>
                        <div class="relative">
                            <span class="absolute left-4 top-1/2 -translate-y-1/2 text-muted">R$</span>
                            <input type="number" step="0.01" required id="transactionAmount" placeholder="0,00" class="input-field w-full pl-10 pr-4 py-3 rounded-xl bg-bg text-fg">
                        </div>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-fg mb-1.5">Descricao</label>
                        <input type="text" required id="transactionDescription" placeholder="Ex: Supermercado" class="input-field w-full px-4 py-3 rounded-xl bg-bg text-fg">
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-fg mb-1.5">Categoria</label>
                        <select required id="transactionCategory" class="input-field w-full px-4 py-3 rounded-xl bg-bg text-fg">
                            <option value="">Selecione...</option>
                            <option value="moradia">Moradia</option>
                            <option value="alimentacao">Alimentacao</option>
                            <option value="transporte">Transporte</option>
                            <option value="lazer">Lazer</option>
                            <option value="saude">Saude</option>
                            <option value="educacao">Educacao</option>
                            <option value="salario">Salario</option>
                            <option value="freelance">Freelance</option>
                            <option value="investimentos">Investimentos</option>
                            <option value="outros">Outros</option>
                        </select>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-fg mb-1.5">Data</label>
                        <input type="date" required id="transactionDate" class="input-field w-full px-4 py-3 rounded-xl bg-bg text-fg">
                    </div>
                    
                    <button type="submit" class="btn-primary w-full py-3.5 rounded-xl text-white font-semibold text-sm mt-2">Salvar transacao</button>
                </form>
            </div>
        </div>
    </div>

    <!-- Bill Modal -->
    <div id="billModal" class="fixed inset-0 z-50 hidden">
        <div class="modal-overlay absolute inset-0" onclick="closeBillModal()"></div>
        <div class="absolute inset-x-4 top-[10%] md:inset-x-auto md:left-1/2 md:-translate-x-1/2 md:w-full md:max-w-lg">
            <div class="bg-card rounded-3xl shadow-2xl max-h-[80vh] overflow-y-auto animate-scale-in">
                <div class="p-6 border-b border-border flex items-center justify-between">
                    <h3 class="font-display text-xl font-bold" id="billModalTitle">Nova conta</h3>
                    <button onclick="closeBillModal()" class="p-2 rounded-lg hover:bg-bg transition-colors">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/></svg>
                    </button>
                </div>
                <form onsubmit="saveBill(event)" class="p-6 space-y-4">
                    <input type="hidden" id="billId">
                    
                    <div>
                        <label class="block text-sm font-medium text-fg mb-1.5">Nome da conta</label>
                        <input type="text" required id="billName" placeholder="Ex: Aluguel" class="input-field w-full px-4 py-3 rounded-xl bg-bg text-fg">
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-fg mb-1.5">Valor</label>
                        <div class="relative">
                            <span class="absolute left-4 top-1/2 -translate-y-1/2 text-muted">R$</span>
                            <input type="number" step="0.01" required id="billAmount" placeholder="0,00" class="input-field w-full pl-10 pr-4 py-3 rounded-xl bg-bg text-fg">
                        </div>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-fg mb-1.5">Data de vencimento</label>
                        <input type="date" required id="billDueDate" class="input-field w-full px-4 py-3 rounded-xl bg-bg text-fg">
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-fg mb-1.5">Recorrencia</label>
                        <select id="billRecurrence" class="input-field w-full px-4 py-3 rounded-xl bg-bg text-fg">
                            <option value="unique">Unica</option>
                            <option value="monthly">Mensal</option>
                        </select>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-fg mb-1.5">Categoria</label>
                        <select id="billCategory" class="input-field w-full px-4 py-3 rounded-xl bg-bg text-fg">
                            <option value="moradia">Moradia</option>
                            <option value="alimentacao">Alimentacao</option>
                            <option value="transporte">Transporte</option>
                            <option value="lazer">Lazer</option>
                            <option value="saude">Saude</option>
                            <option value="educacao">Educacao</option>
                            <option value="outros">Outros</option>
                        </select>
                    </div>
                    
                    <button type="submit" class="btn-primary w-full py-3.5 rounded-xl text-white font-semibold text-sm mt-2">Salvar conta</button>
                </form>
            </div>
        </div>
    </div>

    <!-- Debt Modal -->
    <div id="debtModal" class="fixed inset-0 z-50 hidden">
        <div class="modal-overlay absolute inset-0" onclick="closeDebtModal()"></div>
        <div class="absolute inset-x-4 top-[5%] md:inset-x-auto md:left-1/2 md:-translate-x-1/2 md:w-full md:max-w-lg">
            <div class="bg-card rounded-3xl shadow-2xl max-h-[85vh] overflow-y-auto animate-scale-in">
                <div class="p-6 border-b border-border flex items-center justify-between">
                    <h3 class="font-display text-xl font-bold" id="debtModalTitle">Nova divida</h3>
                    <button onclick="closeDebtModal()" class="p-2 rounded-lg hover:bg-bg transition-colors">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/></svg>
                    </button>
                </div>
                <form onsubmit="saveDebt(event)" class="p-6 space-y-4">
                    <input type="hidden" id="debtId">
                    
                    <div>
                        <label class="block text-sm font-medium text-fg mb-1.5">Credor / Nome</label>
                        <input type="text" required id="debtCreditor" placeholder="Ex: Banco Itau - Cartao" class="input-field w-full px-4 py-3 rounded-xl bg-bg text-fg">
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-fg mb-1.5">Valor total</label>
                        <div class="relative">
                            <span class="absolute left-4 top-1/2 -translate-y-1/2 text-muted">R$</span>
                            <input type="number" step="0.01" required id="debtTotal" placeholder="0,00" class="input-field w-full pl-10 pr-4 py-3 rounded-xl bg-bg text-fg">
                        </div>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-fg mb-1.5">Juros mensal (%)</label>
                        <input type="number" step="0.1" id="debtInterest" placeholder="2.5" class="input-field w-full px-4 py-3 rounded-xl bg-bg text-fg">
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-fg mb-1.5">Parcelas restantes</label>
                        <input type="number" required id="debtInstallments" placeholder="12" class="input-field w-full px-4 py-3 rounded-xl bg-bg text-fg">
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-fg mb-1.5">Valor da parcela</label>
                        <div class="relative">
                            <span class="absolute left-4 top-1/2 -translate-y-1/2 text-muted">R$</span>
                            <input type="number" step="0.01" required id="debtInstallmentValue" placeholder="0,00" class="input-field w-full pl-10 pr-4 py-3 rounded-xl bg-bg text-fg">
                        </div>
                    </div>
                    
                    <button type="submit" class="btn-primary w-full py-3.5 rounded-xl text-white font-semibold text-sm mt-2">Salvar divida</button>
                </form>
            </div>
        </div>
    </div>

    <!-- Budget Modal -->
    <div id="budgetModal" class="fixed inset-0 z-50 hidden">
        <div class="modal-overlay absolute inset-0" onclick="closeBudgetModal()"></div>
        <div class="absolute inset-x-4 top-[20%] md:inset-x-auto md:left-1/2 md:-translate-x-1/2 md:w-full md:max-w-md">
            <div class="bg-card rounded-3xl shadow-2xl animate-scale-in">
                <div class="p-6 border-b border-border flex items-center justify-between">
                    <h3 class="font-display text-xl font-bold">Definir limite</h3>
                    <button onclick="closeBudgetModal()" class="p-2 rounded-lg hover:bg-bg transition-colors">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/></svg>
                    </button>
                </div>
                <form onsubmit="saveBudget(event)" class="p-6 space-y-4">
                    <div>
                        <label class="block text-sm font-medium text-fg mb-1.5">Categoria</label>
                        <select required id="budgetCategory" class="input-field w-full px-4 py-3 rounded-xl bg-bg text-fg">
                            <option value="moradia">Moradia</option>
                            <option value="alimentacao">Alimentacao</option>
                            <option value="transporte">Transporte</option>
                            <option value="lazer">Lazer</option>
                            <option value="saude">Saude</option>
                            <option value="educacao">Educacao</option>
                            <option value="outros">Outros</option>
                        </select>
                    </div>
                    
                    <div>
                        <label class="block text-sm font-medium text-fg mb-1.5">Limite mensal</label>
                        <div class="relative">
                            <span class="absolute left-4 top-1/2 -translate-y-1/2 text-muted">R$</span>
                            <input type="number" step="0.01" required id="budgetLimit" placeholder="0,00" class="input-field w-full pl-10 pr-4 py-3 rounded-xl bg-bg text-fg">
                        </div>
                    </div>
                    
                    <button type="submit" class="btn-primary w-full py-3.5 rounded-xl text-white font-semibold text-sm mt-2">Salvar limite</button>
                </form>
            </div>
        </div>
    </div>

    <!-- Toast Notification -->
    <div id="toast" class="fixed bottom-24 lg:bottom-8 left-1/2 -translate-x-1/2 z-50 hidden">
        <div class="bg-fg text-white px-6 py-3 rounded-xl shadow-xl flex items-center gap-3 animate-slide-up">
            <svg id="toastIcon" class="w-5 h-5 text-success" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/></svg>
            <span id="toastMessage" class="font-medium text-sm">Operacao realizada com sucesso!</span>
        </div>
    </div>

    <!-- Celebration Overlay -->
    <div id="celebrationOverlay" class="fixed inset-0 z-[60] pointer-events-none hidden">
        <div class="absolute inset-0 flex items-center justify-center">
            <div class="text-center animate-scale-in">
                <div class="w-24 h-24 mx-auto mb-4 rounded-full bg-gradient-to-br from-success to-emerald-400 flex items-center justify-center shadow-2xl shadow-success/30">
                    <svg class="w-12 h-12 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/></svg>
                </div>
                <h3 class="font-display text-2xl font-bold text-fg">Parabens!</h3>
                <p id="celebrationMessage" class="text-muted mt-1">Voce esta no caminho certo!</p>
            </div>
        </div>
    </div>

    <script>
        // ==================== DATA ====================
        let appState = {
            isAuthenticated: false,
            currentSection: 'dashboard',
            transactionType: 'expense',
            strategy: 'snowball'
        };

        // Mock Data
        let transactions = [
            { id: 1, type: 'income', amount: 7500, description: 'Salario', category: 'salario', date: '2024-01-05' },
            { id: 2, type: 'income', amount: 1400, description: 'Freelance Design', category: 'freelance', date: '2024-01-10' },
            { id: 3, type: 'expense', amount: 1800, description: 'Aluguel', category: 'moradia', date: '2024-01-03' },
            { id: 4, type: 'expense', amount: 850, description: 'Supermercado', category: 'alimentacao', date: '2024-01-08' },
            { id: 5, type: 'expense', amount: 320, description: 'Combustivel', category: 'transporte', date: '2024-01-12' },
            { id: 6, type: 'expense', amount: 150, description: 'Netflix + Spotify', category: 'lazer', date: '2024-01-02' },
            { id: 7, type: 'expense', amount: 450, description: 'Academia', category: 'saude', date: '2024-01-01' },
            { id: 8, type: 'expense', amount: 280, description: 'Curso Online', category: 'educacao', date: '2024-01-15' },
            { id: 9, type: 'expense', amount: 1200, description: 'Cartao Credito', category: 'outros', date: '2024-01-18' },
            { id: 10, type: 'expense', amount: 400, description: 'Restaurante', category: 'alimentacao', date: '2024-01-20' },
            { id: 11, type: 'income', amount: 350, description: 'Cashback', category: 'investimentos', date: '2024-01-22' },
            { id: 12, type: 'expense', amount: 550, description: 'Farmacia', category: 'saude', date: '2024-01-25' }
        ];

        let bills = [
            { id: 1, name: 'Aluguel', amount: 1800, dueDate: '2024-02-05', recurrence: 'monthly', status: 'pending', category: 'moradia' },
            { id: 2, name: 'Internet', amount: 120, dueDate: '2024-02-10', recurrence: 'monthly', status: 'pending', category: 'moradia' },
            { id: 3, name: 'Cartao Credito', amount: 2350, dueDate: '2024-02-15', recurrence: 'monthly', status: 'overdue', category: 'outros' },
            { id: 4, name: 'Streaming', amount: 150, dueDate: '2024-02-20', recurrence: 'monthly', status: 'pending', category: 'lazer' },
            { id: 5, name: 'Condominio', amount: 450, dueDate: '2024-02-08', recurrence: 'monthly', status: 'paid', category: 'moradia' },
            { id: 6, name: 'Energia', amount: 220, dueDate: '2024-01-28', recurrence: 'monthly', status: 'paid', category: 'moradia' },
            { id: 7, name: 'Agua', amount: 85, dueDate: '2024-01-25', recurrence: 'monthly', status: 'paid', category: 'moradia' },
            { id: 8, name: 'Celular', amount: 89, dueDate: '2024-01-20', recurrence: 'monthly', status: 'paid', category: 'transporte' },
            { id: 9, name: 'Seguro Carro', amount: 180, dueDate: '2024-02-01', recurrence: 'monthly', status: 'pending', category: 'transporte' },
            { id: 10, name: 'Academia', amount: 150, dueDate: '2024-01-30', recurrence: 'monthly', status: 'paid', category: 'saude' },
            { id: 11, name: 'Plano Saude', amount: 380, dueDate: '2024-02-12', recurrence: 'monthly', status: 'pending', category: 'saude' },
            { id: 12, name: 'Escola Filhos', amount: 1200, dueDate: '2024-02-18', recurrence: 'monthly', status: 'pending', category: 'educacao' },
            { id: 13, name: 'Manutencao Carro', amount: 650, dueDate: '2024-01-15', recurrence: 'unique', status: 'paid', category: 'transporte' }
        ];

        let debts = [
            { id: 1, creditor: 'Cartao Nubank', total: 8500, interest: 3.5, installments: 12, installmentValue: 850, paidInstallments: 4 },
            { id: 2, creditor: 'Emprestimo Pessoal', total: 4800, interest: 2.8, installments: 24, installmentValue: 280, paidInstallments: 8 },
            { id: 3, creditor: 'Financiamento Carro', total: 25000, interest: 1.2, installments: 48, installmentValue: 720, paidInstallments: 18 }
        ];

        let budgets = [
            { category: 'moradia', limit: 3000, spent: 2555 },
            { category: 'alimentacao', limit: 1500, spent: 1250 },
            { category: 'transporte', limit: 800, spent: 500 },
            { category: 'lazer', limit: 500, spent: 300 },
            { category: 'saude', limit: 600, spent: 1030 },
            { category: 'educacao', limit: 400, spent: 280 },
            { category: 'outros', limit: 1000, spent: 1200 }
        ];

        const categories = {
            moradia: { name: 'Moradia', icon: '🏠', color: '#6366f1' },
            alimentacao: { name: 'Alimentacao', icon: '🍽️', color: '#f59e0b' },
            transporte: { name: 'Transporte', icon: '🚗', color: '#3b82f6' },
            lazer: { name: 'Lazer', icon: '🎮', color: '#ec4899' },
            saude: { name: 'Saude', icon: '💊', color: '#10b981' },
            educacao: { name: 'Educacao', icon: '📚', color: '#8b5cf6' },
            salario: { name: 'Salario', icon: '💰', color: '#10b981' },
            freelance: { name: 'Freelance', icon: '💻', color: '#06b6d4' },
            investimentos: { name: 'Investimentos', icon: '📈', color: '#14b8a6' },
            outros: { name: 'Outros', icon: '📦', color: '#64748b' }
        };

        // ==================== AUTH ====================
        function switchAuthTab(tab) {
            const loginTab = document.getElementById('loginTab');
            const registerTab = document.getElementById('registerTab');
            const loginForm = document.getElementById('loginForm');
            const registerForm = document.getElementById('registerForm');

            if (tab === 'login') {
                loginTab.classList.add('bg-card', 'shadow-sm', 'text-fg');
                loginTab.classList.remove('text-muted');
                registerTab.classList.remove('bg-card', 'shadow-sm', 'text-fg');
                registerTab.classList.add('text-muted');
                loginForm.classList.remove('hidden');
                registerForm.classList.add('hidden');
            } else {
                registerTab.classList.add('bg-card', 'shadow-sm', 'text-fg');
                registerTab.classList.remove('text-muted');
                loginTab.classList.remove('bg-card', 'shadow-sm', 'text-fg');
                loginTab.classList.add('text-muted');
                registerForm.classList.remove('hidden');
                loginForm.classList.add('hidden');
            }
        }

        function handleLogin(e) {
            e.preventDefault();
            appState.isAuthenticated = true;
            showMainApp();
            showToast('Bem-vindo de volta!', 'success');
        }

        function handleRegister(e) {
            e.preventDefault();
            appState.isAuthenticated = true;
            showMainApp();
            showToast('Conta criada com sucesso!', 'success');
            showCelebration('Sua jornada financeira comeca agora!');
        }

        function handleGoogleLogin() {
            appState.isAuthenticated = true;
            showMainApp();
            showToast('Login com Google realizado!', 'success');
        }

        function handleLogout() {
            appState.isAuthenticated = false;
            document.getElementById('authScreen').classList.remove('hidden');
            document.getElementById('mainApp').classList.add('hidden');
            showToast('Ate logo!', 'info');
        }

        function showForgotPassword() {
            showToast('Link de recuperacao enviado para seu email!', 'info');
        }

        function showMainApp() {
            document.getElementById('authScreen').classList.add('hidden');
            document.getElementById('mainApp').classList.remove('hidden');
            initializeCharts();
            renderAll();
        }

        // ==================== NAVIGATION ====================
        function showSection(section) {
            appState.currentSection = section;

            // Hide all sections
            document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
            // Show target section
            const targetSection = document.getElementById(`section-${section}`);
            if (targetSection) {
                targetSection.classList.add('active');
            }

            // Update sidebar
            document.querySelectorAll('.sidebar-item').forEach(el => {
                el.classList.remove('active');
                el.classList.add('text-muted');
            });
            const sidebarItem = document.querySelector(`[data-section="${section}"]`);
            if (sidebarItem) {
                sidebarItem.classList.add('active');
                sidebarItem.classList.remove('text-muted');
            }

            // Update mobile nav
            document.querySelectorAll('.mobile-nav-btn').forEach(el => {
                el.classList.remove('text-accent');
                el.classList.add('text-muted');
            });
            const mobileNav = document.querySelector(`[data-mobile-nav="${section}"]`);
            if (mobileNav) {
                mobileNav.classList.add('text-accent');
                mobileNav.classList.remove('text-muted');
            }

            // Handle "More" section
            if (section === 'more') {
                showSection('debts');
            }

            // Close mobile menu if open
            closeMobileMenu();

            // Reinitialize charts when needed
            if (section === 'reports') {
                setTimeout(() => initializeReportChart(), 100);
            }
        }

        function toggleMobileMenu() {
            const sidebar = document.getElementById('sidebarDesktop');
            sidebar.classList.toggle('hidden');
            sidebar.classList.toggle('fixed');
            sidebar.classList.toggle('inset-0');
            sidebar.classList.toggle('z-50');
            sidebar.classList.toggle('w-full');
        }

        function closeMobileMenu() {
            const sidebar = document.getElementById('sidebarDesktop');
            if (window.innerWidth < 1024) {
                sidebar.classList.add('hidden');
                sidebar.classList.remove('fixed', 'inset-y-0', 'left-0', 'z-50', 'w-full');
            }
        }

        // ==================== TRANSACTIONS ====================
        function openTransactionModal(id = null) {
            document.getElementById('transactionModal').classList.remove('hidden');
            document.getElementById('transactionModalTitle').textContent = id ? 'Editar transacao' : 'Nova transacao';
            document.getElementById('transactionId').value = id || '';
            document.getElementById('transactionDate').valueAsDate = new Date();

            if (id) {
                const transaction = transactions.find(t => t.id === id);
                if (transaction) {
                    setTransactionType(transaction.type);
                    document.getElementById('transactionAmount').value = transaction.amount;
                    document.getElementById('transactionDescription').value = transaction.description;
                    document.getElementById('transactionCategory').value = transaction.category;
                    document.getElementById('transactionDate').value = transaction.date;
                }
            } else {
                setTransactionType('expense');
                document.getElementById('transactionAmount').value = '';
                document.getElementById('transactionDescription').value = '';
                document.getElementById('transactionCategory').value = '';
            }
        }

        function closeTransactionModal() {
            document.getElementById('transactionModal').classList.add('hidden');
        }

        function setTransactionType(type) {
            appState.transactionType = type;
            const expenseBtn = document.getElementById('typeExpenseBtn');
            const incomeBtn = document.getElementById('typeIncomeBtn');

            if (type === 'expense') {
                expenseBtn.classList.add('bg-expense', 'text-white');
                expenseBtn.classList.remove('text-muted');
                incomeBtn.classList.remove('bg-income', 'text-white');
                incomeBtn.classList.add('text-muted');
            } else {
                incomeBtn.classList.add('bg-income', 'text-white');
                incomeBtn.classList.remove('text-muted');
                expenseBtn.classList.remove('bg-expense', 'text-white');
                expenseBtn.classList.add('text-muted');
            }
        }

        function saveTransaction(e) {
            e.preventDefault();
            const id = document.getElementById('transactionId').value;
            const transaction = {
                id: id ? parseInt(id) : Date.now(),
                type: appState.transactionType,
                amount: parseFloat(document.getElementById('transactionAmount').value),
                description: document.getElementById('transactionDescription').value,
                category: document.getElementById('transactionCategory').value,
                date: document.getElementById('transactionDate').value
            };

            if (id) {
                const index = transactions.findIndex(t => t.id === parseInt(id));
                transactions[index] = transaction;
                showToast('Transacao atualizada!', 'success');
            } else {
                transactions.unshift(transaction);
                showToast('Transacao adicionada!', 'success');
            }

            closeTransactionModal();
            renderAll();
        }

        function deleteTransaction(id) {
            if (confirm('Deseja excluir esta transacao?')) {
                transactions = transactions.filter(t => t.id !== id);
                showToast('Transacao removida!', 'info');
                renderAll();
            }
        }

        function filterTransactions() {
            renderTransactionsList();
        }

        function resetFilters() {
            document.getElementById('filterType').value = '';
            document.getElementById('filterCategory').value = '';
            document.getElementById('filterMonth').value = '';
            renderTransactionsList();
        }

        // ==================== BILLS ====================
        function openBillModal(id = null) {
            document.getElementById('billModal').classList.remove('hidden');
            document.getElementById('billModalTitle').textContent = id ? 'Editar conta' : 'Nova conta';
            document.getElementById('billId').value = id || '';

            if (id) {
                const bill = bills.find(b => b.id === id);
                if (bill) {
                    document.getElementById('billName').value = bill.name;
                    document.getElementById('billAmount').value = bill.amount;
                    document.getElementById('billDueDate').value = bill.dueDate;
                    document.getElementById('billRecurrence').value = bill.recurrence;
                    document.getElementById('billCategory').value = bill.category;
                }
            } else {
                document.getElementById('billName').value = '';
                document.getElementById('billAmount').value = '';
                document.getElementById('billDueDate').value = '';
                document.getElementById('billRecurrence').value = 'unique';
                document.getElementById('billCategory').value = 'outros';
            }
        }

        function closeBillModal() {
            document.getElementById('billModal').classList.add('hidden');
        }

        function saveBill(e) {
            e.preventDefault();
            const id = document.getElementById('billId').value;
            const bill = {
                id: id ? parseInt(id) : Date.now(),
                name: document.getElementById('billName').value,
                amount: parseFloat(document.getElementById('billAmount').value),
                dueDate: document.getElementById('billDueDate').value,
                recurrence: document.getElementById('billRecurrence').value,
                category: document.getElementById('billCategory').value,
                status: 'pending'
            };

            if (id) {
                const index = bills.findIndex(b => b.id === parseInt(id));
                bill.status = bills[index].status;
                bills[index] = bill;
                showToast('Conta atualizada!', 'success');
            } else {
                bills.push(bill);
                showToast('Conta adicionada!', 'success');
            }

            closeBillModal();
            renderAll();
        }

        function toggleBillStatus(id) {
            const bill = bills.find(b => b.id === id);
            if (bill) {
                if (bill.status === 'paid') {
                    bill.status = 'pending';
                } else {
                    bill.status = 'paid';
                    showToast('Conta marcada como paga!', 'success');
                    if (Math.random() > 0.5) {
                        showCelebration('Uma conta a menos! Continue assim!');
                    }
                }
                renderAll();
            }
        }

        function deleteBill(id) {
            if (confirm('Deseja excluir esta conta?')) {
                bills = bills.filter(b => b.id !== id);
                showToast('Conta removida!', 'info');
                renderAll();
            }
        }

        // ==================== DEBTS ====================
        function openDebtModal(id = null) {
            document.getElementById('debtModal').classList.remove('hidden');
            document.getElementById('debtModalTitle').textContent = id ? 'Editar divida' : 'Nova divida';
            document.getElementById('debtId').value = id || '';

            if (id) {
                const debt = debts.find(d => d.id === id);
                if (debt) {
                    document.getElementById('debtCreditor').value = debt.creditor;
                    document.getElementById('debtTotal').value = debt.total;
                    document.getElementById('debtInterest').value = debt.interest;
                    document.getElementById('debtInstallments').value = debt.installments;
                    document.getElementById('debtInstallmentValue').value = debt.installmentValue;
                }
            } else {
                document.getElementById('debtCreditor').value = '';
                document.getElementById('debtTotal').value = '';
                document.getElementById('debtInterest').value = '';
                document.getElementById('debtInstallments').value = '';
                document.getElementById('debtInstallmentValue').value = '';
            }
        }

        function closeDebtModal() {
            document.getElementById('debtModal').classList.add('hidden');
        }

        function saveDebt(e) {
            e.preventDefault();
            const id = document.getElementById('debtId').value;
            const debt = {
                id: id ? parseInt(id) : Date.now(),
                creditor: document.getElementById('debtCreditor').value,
                total: parseFloat(document.getElementById('debtTotal').value),
                interest: parseFloat(document.getElementById('debtInterest').value) || 0,
                installments: parseInt(document.getElementById('debtInstallments').value),
                installmentValue: parseFloat(document.getElementById('debtInstallmentValue').value),
                paidInstallments: id ? debts.find(d => d.id === parseInt(id))?.paidInstallments || 0 : 0
            };

            if (id) {
                const index = debts.findIndex(d => d.id === parseInt(id));
                debts[index] = debt;
                showToast('Divida atualizada!', 'success');
            } else {
                debts.push(debt);
                showToast('Divida registrada!', 'success');
            }

            closeDebtModal();
            renderAll();
        }

        function payDebtInstallment(id) {
            const debt = debts.find(d => d.id === id);
            if (debt && debt.paidInstallments < debt.installments) {
                debt.paidInstallments++;
                
                if (debt.paidInstallments >= debt.installments) {
                    showToast('Parabens! Divida quitada!', 'success');
                    showCelebration('Voce eliminou uma divida! Isso e incrivel!');
                } else {
                    showToast('Parcela paga!', 'success');
                }
                
                renderAll();
            }
        }

        function updateStrategy() {
            appState.strategy = document.querySelector('input[name="strategy"]:checked').value;
            renderDebtsList();
        }

        function calculatePayoffSavings(debt) {
            const remaining = debt.installments - debt.paidInstallments;
            const monthlyRate = debt.interest / 100;
            const remainingBalance = debt.total * Math.pow(1 + monthlyRate, debt.paidInstallments) - 
                                   (debt.installmentValue * ((Math.pow(1 + monthlyRate, debt.paidInstallments) - 1) / monthlyRate));
            
            const totalWithInterest = debt.installmentValue * remaining;
            const savings = totalWithInterest - Math.max(0, remainingBalance);
            
            return Math.max(0, savings).toFixed(2);
        }

        function deleteDebt(id) {
            if (confirm('Deseja excluir esta divida?')) {
                debts = debts.filter(d => d.id !== id);
                showToast('Divida removida!', 'info');
                renderAll();
            }
        }

        // ==================== BUDGET ====================
        function openBudgetModal() {
            document.getElementById('budgetModal').classList.remove('hidden');
            document.getElementById('budgetCategory').value = 'alimentacao';
            document.getElementById('budgetLimit').value = '';
        }

        function closeBudgetModal() {
            document.getElementById('budgetModal').classList.add('hidden');
        }

        function saveBudget(e) {
            e.preventDefault();
            const category = document.getElementById('budgetCategory').value;
            const limit = parseFloat(document.getElementById('budgetLimit').value);

            const existingIndex = budgets.findIndex(b => b.category === category);
            if (existingIndex >= 0) {
                budgets[existingIndex].limit = limit;
            } else {
                budgets.push({ category, limit, spent: 0 });
            }

            showToast('Limite definido!', 'success');
            closeBudgetModal();
            renderBudgetList();
        }

        // ==================== RENDER FUNCTIONS ====================
        function renderAll() {
            renderDashboard();
            renderTransactionsList();
            renderBillsList();
            renderDebtsList();
            renderBudgetList();
            updateCharts();
        }

        function renderDashboard() {
            // Calculate totals
            const income = transactions.filter(t => t.type === 'income').reduce((sum, t) => sum + t.amount, 0);
            const expense = transactions.filter(t => t.type === 'expense').reduce((sum, t) => sum + t.amount, 0);
            const balance = income - expense;

            document.getElementById('currentBalance').textContent = formatCurrency(balance);
            document.getElementById('totalIncome').textContent = `+${formatCurrency(income)}`;
            document.getElementById('totalExpense').textContent = `-${formatCurrency(expense)}`;
            document.getElementById('monthIncome').textContent = formatCurrency(income);
            document.getElementById('monthExpense').textContent = formatCurrency(expense);

            // Bills stats
            const pendingCount = bills.filter(b => b.status === 'pending').length;
            const overdueCount = bills.filter(b => b.status === 'overdue').length;
            const pendingTotal = bills.filter(b => b.status === 'pending' || b.status === 'overdue').reduce((sum, b) => sum + b.amount, 0);

            document.getElementById('pendingBills').textContent = pendingCount + overdueCount;
            document.querySelector('#pendingBills + p').textContent = `${formatCurrency(pendingTotal)} total`;
            document.getElementById('activeDebts').textContent = debts.length;
            document.querySelector('#activeDebts + p').textContent = `${formatCurrency(debts.reduce((sum, d) => sum + d.total, 0))} total`;

            // Upcoming bills
            const upcomingBillsContainer = document.getElementById('upcomingBillsList');
            const upcomingBills = [...bills]
                .filter(b => b.status !== 'paid')
                .sort((a, b) => new Date(a.dueDate) - new Date(b.dueDate))
                .slice(0, 4);

            upcomingBillsContainer.innerHTML = upcomingBills.map(bill => `
                <div class="flex items-center justify-between p-4 bg-bg rounded-xl hover:bg-border/30 transition-colors cursor-pointer" onclick="toggleBillStatus(${bill.id})">
                    <div class="flex items-center gap-3">
                        <div class="w-10 h-10 rounded-xl ${getStatusClass(bill.status)} flex items-center justify-center text-sm font-bold">
                            ${bill.status === 'paid' ? '✓' : getDaysUntilDue(bill.dueDate)}
                        </div>
                        <div>
                            <p class="font-medium text-sm">${bill.name}</p>
                            <p class="text-xs text-muted">${formatDate(bill.dueDate)}</p>
                        </div>
                    </div>
                    <div class="text-right">
                        <p class="font-semibold text-sm">${formatCurrency(bill.amount)}</p>
                        <span class="text-xs px-2 py-0.5 rounded-full ${getStatusBadgeClass(bill.status)}">${getStatusLabel(bill.status)}</span>
                    </div>
                </div>
            `).join('');

            // Recent transactions
            const recentContainer = document.getElementById('recentTransactionsList');
            const recentTransactions = [...transactions].slice(0, 5);

            recentContainer.innerHTML = recentTransactions.map(t => `
                <div class="flex items-center justify-between p-4 bg-bg rounded-xl hover:bg-border/30 transition-colors">
                    <div class="flex items-center gap-3">
                        <div class="w-10 h-10 rounded-xl flex items-center justify-center" style="background: ${categories[t.category]?.color}15; color: ${categories[t.category]?.color}">
                            ${categories[t.category]?.icon || '📦'}
                        </div>
                        <div>
                            <p class="font-medium text-sm">${t.description}</p>
                            <p class="text-xs text-muted">${categories[t.category]?.name || t.category}</p>
                        </div>
                    </div>
                    <p class="font-semibold text-sm ${t.type === 'income' ? 'text-income' : 'text-expense'}">
                        ${t.type === 'income' ? '+' : '-'}${formatCurrency(t.amount)}
                    </p>
                </div>
            `).join('');
        }

        function renderTransactionsList() {
            const container = document.getElementById('transactionsList');
            const typeFilter = document.getElementById('filterType').value;
            const categoryFilter = document.getElementById('filterCategory').value;
            const monthFilter = document.getElementById('filterMonth').value;

            let filtered = [...transactions];

            if (typeFilter) filtered = filtered.filter(t => t.type === typeFilter);
            if (categoryFilter) filtered = filtered.filter(t => t.category === categoryFilter);
            if (monthFilter) filtered = filtered.filter(t => t.date.startsWith(monthFilter));

            filtered.sort((a, b) => new Date(b.date) - new Date(a.date));

            if (filtered.length === 0) {
                container.innerHTML = `
                    <div class="p-12 text-center">
                        <div class="w-16 h-16 mx-auto mb-4 rounded-full bg-bg flex items-center justify-center">
                            <svg class="w-8 h-8 text-muted" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2"/></svg>
                        </div>
                        <p class="text-muted">Nenhuma transacao encontrada</p>
                    </div>
                `;
                return;
            }

            container.innerHTML = filtered.map(t => `
                <div class="flex items-center justify-between p-4 hover:bg-bg/50 transition-colors">
                    <div class="flex items-center gap-4">
                        <div class="w-12 h-12 rounded-xl flex items-center justify-center" style="background: ${categories[t.category]?.color}15; color: ${categories[t.category]?.color}">
                            ${categories[t.category]?.icon || '📦'}
                        </div>
                        <div>
                            <p class="font-medium">${t.description}</p>
                            <p class="text-sm text-muted">${categories[t.category]?.name} • ${formatDate(t.date)}</p>
                        </div>
                    </div>
                    <div class="flex items-center gap-4">
                        <p class="font-semibold ${t.type === 'income' ? 'text-income' : 'text-expense'}">
                            ${t.type === 'income' ? '+' : '-'}${formatCurrency(t.amount)}
                        </p>
                        <div class="flex gap-1">
                            <button onclick="openTransactionModal(${t.id})" class="p-2 rounded-lg hover:bg-bg transition-colors text-muted hover:text-accent">
                                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z"/></svg>
                            </button>
                            <button onclick="deleteTransaction(${t.id})" class="p-2 rounded-lg hover:bg-bg transition-colors text-muted hover:text-danger">
                                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"/></svg>
                            </button>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        function renderBillsList() {
            const container = document.getElementById('billsList');
            
            // Update counts
            document.getElementById('paidBillsCount').textContent = bills.filter(b => b.status === 'paid').length;
            document.getElementById('pendingBillsCount').textContent = bills.filter(b => b.status === 'pending').length;
            document.getElementById('overdueBillsCount').textContent = bills.filter(b => b.status === 'overdue').length;

            const sortedBills = [...bills].sort((a, b) => {
                const statusOrder = { overdue: 0, pending: 1, paid: 2 };
                return statusOrder[a.status] - statusOrder[b.status];
            });

            container.innerHTML = sortedBills.map(bill => `
                <div class="bg-card rounded-2xl p-5 border border-border card-hover">
                    <div class="flex items-start justify-between">
                        <div class="flex items-start gap-4">
                            <button onclick="toggleBillStatus(${bill.id})" class="w-12 h-12 rounded-xl ${getStatusClass(bill.status)} flex items-center justify-center text-lg font-bold transition-transform hover:scale-110">
                                ${bill.status === 'paid' ? '✓' : '○'}
                            </button>
                            <div>
                                <p class="font-semibold text-lg">${bill.name}</p>
                                <p class="text-sm text-muted">${categories[bill.category]?.name || bill.category} • ${bill.recurrence === 'monthly' ? 'Mensal' : 'Unica'}</p>
                                <p class="text-sm text-muted mt-1">Vence: ${formatDate(bill.dueDate)}</p>
                            </div>
                        </div>
                        <div class="text-right">
                            <p class="font-display text-xl font-bold">${formatCurrency(bill.amount)}</p>
                            <span class="inline-block mt-2 text-xs px-3 py-1 rounded-full ${getStatusBadgeClass(bill.status)}">${getStatusLabel(bill.status)}</span>
                        </div>
                    </div>
                    <div class="flex justify-end gap-2 mt-4 pt-4 border-t border-border">
                        <button onclick="openBillModal(${bill.id})" class="px-4 py-2 rounded-lg text-sm font-medium text-muted hover:text-accent hover:bg-accent/5 transition-colors">Editar</button>
                        <button onclick="deleteBill(${bill.id})" class="px-4 py-2 rounded-lg text-sm font-medium text-muted hover:text-danger hover:bg-danger/5 transition-colors">Excluir</button>
                    </div>
                </div>
            `).join('');
        }

        function renderDebtsList() {
            const container = document.getElementById('debtsList');
            
            let sortedDebts = [...debts];
            if (appState.strategy === 'snowball') {
                sortedDebts.sort((a, b) => (a.total - (a.paidInstallments * a.installmentValue)) - (b.total - (b.paidInstallments * b.installmentValue)));
            } else {
                sortedDebts.sort((a, b) => b.interest - a.interest);
            }

            container.innerHTML = sortedDebts.map((debt, index) => {
                const progress = (debt.paidInstallments / debt.installments) * 100;
                const remaining = debt.installments - debt.paidInstallments;
                const savings = calculatePayoffSavings(debt);
                const remainingBalance = debt.total - (debt.paidInstallments * debt.installmentValue);

                return `
                <div class="bg-card rounded-2xl p-6 border border-border card-hover ${index === 0 ? 'ring-2 ring-accent ring-offset-2' : ''}">
                    ${index === 0 ? '<div class="absolute -top-3 left-6 bg-accent text-white text-xs font-bold px-3 py-1 rounded-full">PRIORIDADE</div>' : ''}
                    <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-4">
                        <div>
                            <p class="font-semibold text-lg">${debt.creditor}</p>
                            <p class="text-sm text-muted">${debt.interest}% ao mes • ${remaining} parcelas restantes</p>
                        </div>
                        <div class="text-right">
                            <p class="text-sm text-muted">Saldo devedor</p>
                            <p class="font-display text-2xl font-bold text-danger">${formatCurrency(Math.max(0, remainingBalance))}</p>
                        </div>
                    </div>
                    
                    <div class="mb-4">
                        <div class="flex justify-between text-sm mb-2">
                            <span class="text-muted">Progresso</span>
                            <span class="font-medium">${debt.paidInstallments}/${debt.installments} parcelas</span>
                        </div>
                        <div class="h-3 bg-bg rounded-full overflow-hidden">
                            <div class="progress-bar h-full rounded-full" style="width: ${progress}%"></div>
                        </div>
                    </div>

                    <div class="grid grid-cols-2 gap-4 p-4 bg-bg rounded-xl mb-4">
                        <div>
                            <p class="text-xs text-muted">Valor da parcela</p>
                            <p class="font-semibold">${formatCurrency(debt.installmentValue)}</p>
                        </div>
                        <div>
                            <p class="text-xs text-muted">Economia pagando agora</p>
                            <p class="font-semibold text-success">R$ ${savings}</p>
                        </div>
                    </div>

                    <div class="flex gap-3">
                        <button onclick="payDebtInstallment(${debt.id})" class="flex-1 btn-primary py-3 rounded-xl text-white font-semibold text-sm" ${debt.paidInstallments >= debt.installments ? 'disabled opacity-50' : ''}>
                            ${debt.paidInstallments >= debt.installments ? 'Quitada' : 'Pagar parcela'}
                        </button>
                        <button onclick="openDebtModal(${debt.id})" class="px-4 py-3 rounded-xl border border-border hover:bg-bg transition-colors">
                            <svg class="w-5 h-5 text-muted" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z"/></svg>
                        </button>
                        <button onclick="deleteDebt(${debt.id})" class="px-4 py-3 rounded-xl border border-border hover:bg-danger/5 hover:border-danger/20 transition-colors">
                            <svg class="w-5 h-5 text-muted hover:text-danger" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"/></svg>
                        </button>
                    </div>
                </div>
            `}).join('');
        }

        function renderBudgetList() {
            const container = document.getElementById('budgetList');

            container.innerHTML = budgets.map(budget => {
                const percentage = Math.min(100, (budget.spent / budget.limit) * 100);
                const isOverBudget = budget.spent > budget.limit;
                const isNearLimit = percentage >= 80 && !isOverBudget;

                return `
                <div class="bg-card rounded-2xl p-5 border border-border card-hover">
                    <div class="flex items-center justify-between mb-3">
                        <div class="flex items-center gap-3">
                            <div class="w-10 h-10 rounded-xl flex items-center justify-center" style="background: ${categories[budget.category]?.color}15; color: ${categories[budget.category]?.color}">
                                ${categories[budget.category]?.icon || '📦'}
                            </div>
                            <div>
                                <p class="font-semibold">${categories[budget.category]?.name}</p>
                                <p class="text-xs text-muted">Limite: ${formatCurrency(budget.limit)}</p>
                            </div>
                        </div>
                        <div class="text-right">
                            <p class="font-semibold ${isOverBudget ? 'text-danger' : isNearLimit ? 'text-warning' : 'text-fg'}">${formatCurrency(budget.spent)}</p>
                            <p class="text-xs ${isOverBudget ? 'text-danger' : isNearLimit ? 'text-warning' : 'text-success'}">${isOverBudget ? `Excedido em ${formatCurrency(budget.spent - budget.limit)}` : `Faltam ${formatCurrency(budget.limit - budget.spent)}`}</p>
                        </div>
                    </div>
                    <div class="h-2.5 bg-bg rounded-full overflow-hidden">
                        <div class="h-full rounded-full transition-all duration-500 ${isOverBudget ? 'bg-danger' : isNearLimit ? 'bg-warning' : 'progress-bar'}" style="width: ${percentage}%"></div>
                    </div>
                    <div class="flex justify-between mt-2 text-xs text-muted">
                        <span>${percentage.toFixed(0)}% utilizado</span>
                        ${isOverBudget ? '<span class="text-danger font-medium">Atencao!</span>' : isNearLimit ? '<span class="text-warning font-medium">Proximo do limite</span>' : '<span class="text-success">OK</span>'}
                    </div>
                </div>
            `}).join('');

            // Top categories for reports
            const topCategoriesContainer = document.getElementById('topCategoriesList');
            const sortedByGrowth = [...budgets].sort((a, b) => (b.spent / b.limit) - (a.spent / a.limit));

            topCategoriesContainer.innerHTML = sortedByGrowth.slice(0, 4).map((budget, i) => `
                <div class="flex items-center gap-4">
                    <span class="w-8 h-8 rounded-full bg-bg flex items-center justify-center font-bold text-sm ${i === 0 ? 'text-danger' : i === 1 ? 'text-warning' : 'text-muted'}">${i + 1}</span>
                    <div class="flex-1">
                        <div class="flex justify-between mb-1">
                            <span class="font-medium text-sm">${categories[budget.category]?.name}</span>
                            <span class="text-sm ${budget.spent > budget.limit ? 'text-danger' : ''}">${(budget.spent / budget.limit * 100).toFixed(0)}%</span>
                        </div>
                        <div class="h-2 bg-bg rounded-full overflow-hidden">
                            <div class="h-full rounded-full ${budget.spent > budget.limit ? 'bg-danger' : i === 0 ? 'bg-warning' : 'bg-accent'}" style="width: ${Math.min(100, budget.spent / budget.limit * 100)}%"></div>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        // ==================== CHARTS ====================
        let categoryChart = null;
        let evolutionChart = null;
        let reportChart = null;

        function initializeCharts() {
            initCategoryChart();
            initEvolutionChart();
        }

        function initCategoryChart() {
            const ctx = document.getElementById('categoryChart').getContext('2d');
            
            const expensesByCategory = {};
            transactions.filter(t => t.type === 'expense').forEach(t => {
                expensesByCategory[t.category] = (expensesByCategory[t.category] || 0) + t.amount;
            });

            const labels = Object.keys(expensesByCategory).map(c => categories[c]?.name || c);
            const data = Object.values(expensesByCategory);
            const colors = Object.keys(expensesByCategory).map(c => categories[c]?.color || '#64748b');

            if (categoryChart) categoryChart.destroy();
            
            categoryChart = new Chart(ctx, {
                type: 'doughnut',
                data: {
                    labels,
                    datasets: [{
                        data,
                        backgroundColor: colors,
                        borderWidth: 0,
                        hoverOffset: 8
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    cutout: '68%',
                    plugins: {
                        legend: {
                            position: 'right',
                            labels: {
                                padding: 16,
                                usePointStyle: true,
                                pointStyleWidth: 10,
                                font: { size: 12, family: 'Plus Jakarta Sans' }
                            }
                        }
                    }
                }
            });
        }

        function initEvolutionChart() {
            const ctx = document.getElementById('evolutionChart').getContext('2d');
            
            const months = ['Set', 'Out', 'Nov', 'Dez', 'Jan', 'Fev'];
            const incomeData = [7200, 8100, 7800, 8500, 8900, 8900];
            const expenseData = [5200, 5800, 5100, 6200, 5450, 5450];

            if (evolutionChart) evolutionChart.destroy();
            
            evolutionChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: months,
                    datasets: [
                        {
                            label: 'Receitas',
                            data: incomeData,
                            borderColor: '#10b981',
                            backgroundColor: 'rgba(16, 185, 129, 0.1)',
                            fill: true,
                            tension: 0.4,
                            pointRadius: 4,
                            pointHoverRadius: 6,
                            pointBackgroundColor: '#10b981'
                        },
                        {
                            label: 'Despesas',
                            data: expenseData,
                            borderColor: '#f43f5e',
                            backgroundColor: 'rgba(244, 63, 94, 0.1)',
                            fill: true,
                            tension: 0.4,
                            pointRadius: 4,
                            pointHoverRadius: 6,
                            pointBackgroundColor: '#f43f5e'
                        }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    interaction: {
                        intersect: false,
                        mode: 'index'
                    },
                    plugins: {
                        legend: {
                            position: 'top',
                            align: 'end',
                            labels: {
                                usePointStyle: true,
                                padding: 16,
                                font: { size: 12, family: 'Plus Jakarta Sans' }
                            }
                        }
                    },
                    scales: {
                        x: {
                            grid: { display: false },
                            ticks: { font: { size: 11 } }
                        },
                        y: {
                            grid: { color: 'rgba(0,0,0,0.04)' },
                            ticks: {
                                callback: value => `R$ ${(value / 1000).toFixed(0)}k`,
                                font: { size: 11 }
                            }
                        }
                    }
                }
            });
        }

        function initializeReportChart() {
            const ctx = document.getElementById('reportChart').getContext('2d');
            
            const months = ['Set/23', 'Out/23', 'Nov/23', 'Dez/23', 'Jan/24', 'Fev/24'];
            const balanceData = [2000, 2300, 2700, 2300, 3450, 3450];

            if (reportChart) reportChart.destroy();
            
            reportChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: months,
                    datasets: [{
                        label: 'Saldo mensal',
                        data: balanceData,
                        backgroundColor: months.map((_, i) => i === months.length - 1 ? '#0d9488' : 'rgba(13, 148, 136, 0.3)'),
                        borderRadius: 8,
                        barThickness: 40
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { display: false }
                    },
                    scales: {
                        x: {
                            grid: { display: false }
                        },
                        y: {
                            grid: { color: 'rgba(0,0,0,0.04)' },
                            ticks: {
                                callback: value => `R$ ${value.toLocaleString()}`
                            }
                        }
                    }
                }
            });
        }

        function updateCharts() {
            if (categoryChart) initCategoryChart();
        }

        // ==================== UTILITIES ====================
        function formatCurrency(value) {
            return new Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL' }).format(value);
        }

        function formatDate(dateStr) {
            return new Date(dateStr + 'T00:00:00').toLocaleDateString('pt-BR', { day: '2-digit', month: 'short' });
        }

        function getDaysUntilDue(dateStr) {
            const today = new Date();
            const due = new Date(dateStr + 'T00:00:00');
            const diff = Math.ceil((due - today) / (1000 * 60 * 60 * 24));
            if (diff < 0) return Math.abs(diff);
            return diff;
        }

        function getStatusClass(status) {
            switch (status) {
                case 'paid': return 'status-paid';
                case 'pending': return 'status-pending';
                case 'overdue': return 'status-overdue';
                default: return 'bg-gray-100 text-gray-600';
            }
        }

        function getStatusBadgeClass(status) {
            switch (status) {
                case 'paid': return 'bg-success/10 text-success';
                case 'pending': return 'bg-warning/10 text-warning';
                case 'overdue': return 'bg-danger/10 text-danger';
                default: return 'bg-gray-100 text-gray-600';
            }
        }

        function getStatusLabel(status) {
            switch (status) {
                case 'paid': return 'Pago';
                case 'pending': return 'Pendente';
                case 'overdue': return 'Atrasado';
                default: return status;
            }
        }

        function showToast(message, type = 'success') {
            const toast = document.getElementById('toast');
            const toastIcon = document.getElementById('toastIcon');
            const toastMessage = document.getElementById('toastMessage');

            toastMessage.textContent = message;
            
            if (type === 'success') {
                toastIcon.innerHTML = '<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/>';
                toastIcon.className = 'w-5 h-5 text-success';
            } else if (type === 'error') {
                toastIcon.innerHTML = '<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>';
                toastIcon.className = 'w-5 h-5 text-danger';
            } else {
                toastIcon.innerHTML = '<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/>';
                toastIcon.className = 'w-5 h-5 text-accent';
            }

            toast.classList.remove('hidden');
            setTimeout(() => toast.classList.add('hidden'), 3000);
        }

        function showCelebration(message) {
            const overlay = document.getElementById('celebrationOverlay');
            document.getElementById('celebrationMessage').textContent = message;
            overlay.classList.remove('hidden');
            setTimeout(() => overlay.classList.add('hidden'), 2500);
        }

        // Initialize
        document.addEventListener('DOMContentLoaded', () => {
            // Set default date for filters
            const now = new Date();
            document.getElementById('filterMonth').value = `${now.getFullYear()}-${String(now.getMonth() + 1).padStart(2, '0')}`;
        });
    </script>
</body>
</html>
