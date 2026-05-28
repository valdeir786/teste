<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Auqmia - PetCare Manager Pro</title>
    <link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;600;700;800;900&family=DM+Serif+Display:ital@0;1&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary: #5aaa9e;
            --primary-dark: #1e8374;
            --primary-light: #d4f0ec;
            --bg: #eef7f5;
            --card: #fffdf6;
            --yellow: #ffcb42;
            --yellow-dark: #c99a00;
            --purple: #a478d3;
            --text: #2a3a38;
            --text-muted: #7a9a97;
            --danger: #ff5252;
            --success: #3dcb7a;
            --border: #e2ede9;
            --sidebar-w: 250px;
            /* categorias */
            --cat-alimento: #e8793c;
            --cat-higiene: #5aaa9e;
            --cat-acessorio: #a478d3;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { font-family: 'Nunito', sans-serif; display: flex; height: 100vh; background: var(--bg); color: var(--text); overflow: hidden; }

        /* ───── SIDEBAR ───── */
        nav {
            width: var(--sidebar-w);
            background: linear-gradient(180deg, #1e8374 0%, #146b5e 100%);
            display: flex; flex-direction: column;
            box-shadow: 3px 0 20px rgba(0,0,0,0.15);
            flex-shrink: 0; overflow-y: auto;
        }
        .logo {
            padding: 22px 20px 18px;
            text-align: center;
            cursor: pointer;
            border-bottom: 1px solid rgba(255,255,255,0.12);
        }
        .logo h1 { font-family: 'DM Serif Display', serif; font-size: 1.6rem; color: #fff; letter-spacing: 1px; }
        .logo p { font-size: 0.72rem; color: rgba(255,255,255,0.65); font-style: italic; margin-top: 3px; }

        .profile-switcher { padding: 12px 16px; border-bottom: 1px solid rgba(255,255,255,0.12); }
        .profile-switcher select {
            background: rgba(0,0,0,0.25); color: white; border: 1px solid rgba(255,255,255,0.2);
            padding: 8px 10px; border-radius: 8px; width: 100%; cursor: pointer; font-family: inherit; font-size: 0.85rem;
        }
        .profile-switcher select option { background: #1e8374; }

        .nav-section { padding: 10px 16px 4px; font-size: 0.65rem; font-weight: 800; color: rgba(255,255,255,0.4); text-transform: uppercase; letter-spacing: 1.5px; }

        .nav-item {
            padding: 11px 20px; color: rgba(255,255,255,0.8); cursor: pointer;
            display: flex; align-items: center; gap: 10px;
            font-size: 0.88rem; font-weight: 700; transition: all 0.2s;
            border-left: 3px solid transparent; margin: 1px 0;
        }
        .nav-item:hover { background: rgba(255,255,255,0.1); color: #fff; }
        .nav-item.active { background: rgba(255,255,255,0.15); border-left-color: var(--yellow); color: #fff; }
        .nav-item .nav-icon { font-size: 1.1rem; width: 22px; text-align: center; }

        .nav-bottom { margin-top: auto; border-top: 1px solid rgba(255,255,255,0.12); padding: 8px 0; }

        /* ───── MAIN ───── */
        main { flex: 1; overflow-y: auto; background: var(--bg); }

        .page { display: none; padding: 28px; animation: fadeIn 0.3s ease; }
        .page.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(6px); } to { opacity: 1; transform: none; } }

        /* ───── COMPONENTES ───── */
        .page-header {
            display: flex; justify-content: space-between; align-items: flex-start;
            background: var(--card); padding: 20px 24px; border-radius: 16px;
            margin-bottom: 22px; box-shadow: 0 2px 12px rgba(0,0,0,0.05);
        }
        .page-header h2 { font-family: 'DM Serif Display', serif; font-size: 1.6rem; color: var(--primary-dark); }
        .page-header p { font-size: 0.82rem; color: var(--text-muted); margin-top: 3px; }

        .panel { background: var(--card); padding: 20px 22px; border-radius: 14px; box-shadow: 0 2px 10px rgba(0,0,0,0.04); margin-bottom: 18px; }
        .panel h3 { font-size: 0.95rem; color: var(--primary-dark); margin-bottom: 14px; font-weight: 800; }

        /* ───── DASHBOARD ───── */
        .kpi-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 16px; margin-bottom: 20px; }
        .kpi-card {
            background: var(--card); border-radius: 14px; padding: 18px 20px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.04);
            display: flex; flex-direction: column; gap: 6px;
        }
        .kpi-card .kpi-label { font-size: 0.72rem; font-weight: 800; color: var(--text-muted); text-transform: uppercase; letter-spacing: 1px; }
        .kpi-card .kpi-value { font-family: 'DM Serif Display', serif; font-size: 1.9rem; color: var(--primary-dark); }
        .kpi-card .kpi-sub { font-size: 0.75rem; color: var(--text-muted); }
        .kpi-card.accent-yellow .kpi-value { color: var(--yellow-dark); }
        .kpi-card.accent-purple .kpi-value { color: var(--purple); }
        .kpi-card.accent-red .kpi-value { color: var(--danger); }

        .dash-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 18px; }
        .log-item { display: flex; align-items: center; gap: 10px; padding: 9px 0; border-bottom: 1px solid var(--border); font-size: 0.82rem; }
        .log-item:last-child { border-bottom: none; }
        .log-dot { width: 8px; height: 8px; border-radius: 50%; background: var(--primary); flex-shrink: 0; }
        .log-dot.venda { background: var(--success); }
        .log-dot.agenda { background: var(--yellow); }
        .log-dot.estoque { background: var(--purple); }

        /* ───── ALERTAS DE ESTOQUE ───── */
        .alert-item { display: flex; justify-content: space-between; align-items: center; padding: 9px 12px; border-radius: 8px; margin-bottom: 6px; font-size: 0.82rem; }
        .alert-item.danger { background: #fff0f0; border-left: 3px solid var(--danger); }
        .alert-item.warning { background: #fffbea; border-left: 3px solid var(--yellow); }

        /* ───── FRENTE DE VENDAS ───── */
        .vendas-layout { display: grid; grid-template-columns: 1fr 340px; gap: 20px; }

        /* Tabs de categoria */
        .cat-tabs { display: flex; gap: 8px; margin-bottom: 16px; flex-wrap: wrap; }
        .cat-tab {
            padding: 8px 18px; border-radius: 30px; border: 2px solid transparent;
            font-size: 0.82rem; font-weight: 800; cursor: pointer; transition: all 0.2s;
            display: flex; align-items: center; gap: 6px; background: var(--bg);
            color: var(--text-muted);
        }
        .cat-tab:hover { transform: translateY(-1px); }
        .cat-tab.all.active   { background: var(--primary-dark); color: #fff; border-color: var(--primary-dark); }
        .cat-tab.alimento.active { background: var(--cat-alimento); color: #fff; border-color: var(--cat-alimento); }
        .cat-tab.higiene.active  { background: var(--cat-higiene);  color: #fff; border-color: var(--cat-higiene); }
        .cat-tab.acessorio.active{ background: var(--cat-acessorio);color: #fff; border-color: var(--cat-acessorio); }

        /* Barra de pesquisa */
        .search-bar { display: flex; align-items: center; background: var(--bg); border: 1.5px solid var(--border); border-radius: 10px; padding: 0 14px; margin-bottom: 16px; }
        .search-bar input { border: none; background: transparent; padding: 10px 8px; flex: 1; font-family: inherit; font-size: 0.88rem; outline: none; color: var(--text); }
        .search-bar span { color: var(--text-muted); font-size: 1rem; }

        /* Seção de categoria na vitrine */
        .cat-section { margin-bottom: 24px; }
        .cat-section-title {
            font-size: 0.72rem; font-weight: 900; text-transform: uppercase;
            letter-spacing: 2px; margin-bottom: 10px; padding: 4px 10px;
            border-radius: 6px; display: inline-block;
        }
        .cat-section-title.alimento { background: #fdeee6; color: var(--cat-alimento); }
        .cat-section-title.higiene  { background: #dff0ee; color: var(--cat-higiene); }
        .cat-section-title.acessorio{ background: #f0e8f7; color: var(--cat-acessorio); }

        .product-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(130px, 1fr)); gap: 12px; }
        .product-card {
            background: #f9f9f5; padding: 14px 10px; border-radius: 14px;
            text-align: center; cursor: pointer; border: 2px solid transparent;
            transition: all 0.2s; position: relative; overflow: hidden;
        }
        .product-card:hover { transform: translateY(-2px); box-shadow: 0 6px 18px rgba(0,0,0,0.09); }
        .product-card.alimento:hover { border-color: var(--cat-alimento); }
        .product-card.higiene:hover  { border-color: var(--cat-higiene); }
        .product-card.acessorio:hover{ border-color: var(--cat-acessorio); }
        .product-card.sem-estoque { opacity: 0.45; cursor: not-allowed; }
        .product-card .prod-icon { font-size: 2rem; margin-bottom: 6px; }
        .product-card .prod-name { font-size: 0.78rem; font-weight: 700; margin-bottom: 4px; color: var(--text); }
        .product-card .prod-preco { font-size: 0.85rem; font-weight: 800; color: var(--primary-dark); }
        .product-card .prod-estoque { font-size: 0.68rem; color: var(--text-muted); margin-top: 2px; }
        .badge-cat {
            position: absolute; top: 6px; right: 6px;
            width: 8px; height: 8px; border-radius: 50%;
        }
        .badge-cat.alimento { background: var(--cat-alimento); }
        .badge-cat.higiene  { background: var(--cat-higiene); }
        .badge-cat.acessorio{ background: var(--cat-acessorio); }

        /* Carrinho */
        .carrinho-item {
            display: flex; justify-content: space-between; align-items: center;
            padding: 8px 0; border-bottom: 1px solid var(--border); font-size: 0.82rem;
        }
        .carrinho-item button { background: none; border: none; color: var(--danger); cursor: pointer; font-size: 1rem; padding: 2px 6px; border-radius: 4px; }
        .carrinho-item button:hover { background: #fff0f0; }
        .total-row { display: flex; justify-content: space-between; padding: 14px 0 0; font-size: 1.05rem; font-weight: 800; }
        .total-row span:last-child { color: var(--primary-dark); font-size: 1.3rem; }

        /* ───── ESTOQUE ───── */
        .estoque-filtros { display: flex; gap: 10px; margin-bottom: 16px; flex-wrap: wrap; align-items: center; }
        .estoque-filtros select, .estoque-filtros input {
            border: 1.5px solid var(--border); border-radius: 8px;
            padding: 8px 12px; font-family: inherit; font-size: 0.83rem;
            background: var(--card); color: var(--text); outline: none; width: auto;
        }

        table { width: 100%; border-collapse: collapse; font-size: 0.85rem; }
        th { padding: 10px 14px; text-align: left; border-bottom: 2px solid var(--border); font-size: 0.72rem; font-weight: 900; text-transform: uppercase; letter-spacing: 1px; color: var(--text-muted); }
        td { padding: 12px 14px; border-bottom: 1px solid var(--border); }
        tr:hover td { background: #f7faf9; }
        .status-pill { padding: 4px 10px; border-radius: 20px; font-size: 0.7rem; font-weight: 800; }
        .status-cheio   { background: #d6f5e5; color: #1a7a45; }
        .status-medio   { background: #fff5d0; color: #7a5a00; }
        .status-baixo   { background: #ffe0e0; color: #a00; }

        /* ───── AGENDA ───── */
        .agenda-status { padding: 4px 10px; border-radius: 20px; font-size: 0.72rem; font-weight: 800; }
        .st-ocorrendo { background: #fff4e0; color: #c07000; }
        .st-certo     { background: #d6f5e5; color: #1a7a45; }
        .st-errado    { background: #ffe0e0; color: #a00; }
        .agenda-actions button { font-size: 0.72rem; padding: 4px 9px; border-radius: 5px; border: none; cursor: pointer; margin-right: 4px; font-weight: 700; }
        .btn-ok  { background: #d6f5e5; color: #1a7a45; }
        .btn-err { background: #ffe0e0; color: #a00; }
        .btn-del { background: #eee; color: #666; }

        /* ───── CLIENTES ───── */
        .cliente-card {
            display: flex; justify-content: space-between; align-items: center;
            padding: 14px 16px; border-radius: 10px; background: var(--bg);
            margin-bottom: 8px; border: 1px solid var(--border);
        }
        .cliente-card .c-name { font-weight: 800; font-size: 0.9rem; }
        .cliente-card .c-info { font-size: 0.77rem; color: var(--text-muted); margin-top: 2px; }
        .cliente-card .c-badge { font-size: 0.7rem; font-weight: 800; background: var(--primary-light); color: var(--primary-dark); padding: 4px 10px; border-radius: 20px; }

        /* ───── ÁREA DO CLIENTE ───── */
        .historico-item { border-left: 3px solid var(--primary); padding: 10px 14px; margin-bottom: 10px; background: var(--bg); border-radius: 0 8px 8px 0; font-size: 0.83rem; }
        .historico-item strong { color: var(--primary-dark); }

        /* ───── BOTÕES ───── */
        .btn { border: none; padding: 12px 20px; border-radius: 10px; color: white; font-weight: 800; cursor: pointer; font-family: inherit; font-size: 0.9rem; transition: all 0.2s; display: inline-flex; align-items: center; gap: 6px; }
        .btn:hover { filter: brightness(1.08); transform: translateY(-1px); }
        .btn-block { display: flex; width: 100%; justify-content: center; margin-top: 10px; }
        .btn-primary { background: var(--primary-dark); }
        .btn-success { background: var(--success); }
        .btn-danger  { background: var(--danger); }
        .btn-gray    { background: #b0b0b0; color: white; }

        /* ───── MODAL ───── */
        .overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.55); z-index: 98; backdrop-filter: blur(5px); }
        .modal {
            display: none; position: fixed; top: 50%; left: 50%;
            transform: translate(-50%, -50%);
            width: 460px; max-width: 95vw; max-height: 90vh; overflow-y: auto;
            background: white; border-radius: 20px; z-index: 100;
            padding: 30px; box-shadow: 0 16px 50px rgba(0,0,0,0.2);
        }
        .modal h3 { font-family: 'DM Serif Display', serif; font-size: 1.3rem; color: var(--primary-dark); margin-bottom: 18px; }
        .form-group { margin-bottom: 12px; }
        .form-group label { font-size: 0.75rem; font-weight: 800; color: var(--text-muted); text-transform: uppercase; letter-spacing: 1px; display: block; margin-bottom: 5px; }
        .form-group input, .form-group select {
            width: 100%; padding: 10px 14px; border: 1.5px solid var(--border);
            border-radius: 9px; font-family: inherit; font-size: 0.88rem;
            color: var(--text); outline: none; transition: border-color 0.2s; background: var(--bg);
        }
        .form-group input:focus, .form-group select:focus { border-color: var(--primary); background: white; }
        .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
        .modal-close { float: right; background: none; border: none; font-size: 1.3rem; cursor: pointer; color: var(--text-muted); margin-top: -8px; }

        /* QR Code */
        .qr-wrap { background: #f5f5f5; border-radius: 16px; padding: 20px; text-align: center; margin: 16px 0; }
        .qr-mock { display: inline-block; background: white; padding: 10px; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); }

        /* Método pag */
        .pag-metodos { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin-bottom: 10px; }
        .pag-btn { padding: 16px 10px; border-radius: 12px; border: 2px solid var(--border); background: var(--bg); cursor: pointer; text-align: center; font-weight: 800; font-size: 0.85rem; transition: all 0.2s; font-family: inherit; color: var(--text); }
        .pag-btn:hover, .pag-btn.sel { border-color: var(--primary); background: var(--primary-light); color: var(--primary-dark); }
        .pag-btn .pag-icon { font-size: 1.5rem; display: block; margin-bottom: 6px; }

        /* Scrollbar */
        ::-webkit-scrollbar { width: 5px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border); border-radius: 10px; }

        /* Relatórios */
        .relatorio-row { display: flex; justify-content: space-between; padding: 11px 0; border-bottom: 1px solid var(--border); font-size: 0.85rem; }
        .relatorio-row:last-child { border: none; }
        .relatorio-row .r-label { color: var(--text-muted); }
        .relatorio-row .r-val { font-weight: 800; }

        @media (max-width: 900px) {
            nav { width: 200px; }
            .kpi-grid { grid-template-columns: 1fr 1fr; }
            .vendas-layout { grid-template-columns: 1fr; }
        }
    </style>
</head>
<body>

<div class="overlay" id="overlay" onclick="closeModals()"></div>

<!-- ═══════════════ SIDEBAR ═══════════════ -->
<nav id="sidebar">
    <div class="logo" onclick="switchPage('dashboard', 'nav-dash')">
        <h1>🐾 Auqmia</h1>
        <p>"Luxo para quem late!"</p>
    </div>

    <div class="profile-switcher">
        <select id="user-profile" onchange="changeProfile()">
            <option value="dono">👑 Admin / Dono</option>
            <option value="estoquista">📦 Estoquista</option>
            <option value="recepcionista">📞 Recepcionista</option>
        </select>
    </div>

    <div class="nav-section">Operações</div>
    <div class="nav-item active" id="nav-dash"       onclick="switchPage('dashboard','nav-dash')">       <span class="nav-icon">🏠</span> Dashboard</div>
    <div class="nav-item"        id="nav-vendas"     onclick="switchPage('vendas','nav-vendas')">         <span class="nav-icon">💰</span> Frente de Vendas</div>
    <div class="nav-item"        id="nav-estoque"    onclick="switchPage('estoque','nav-estoque')">       <span class="nav-icon">📦</span> Estoque Central</div>

    <div class="nav-section">Gestão</div>
    <div class="nav-item"        id="nav-agenda"     onclick="switchPage('agenda','nav-agenda')">         <span class="nav-icon">📅</span> Agenda Master</div>
    <div class="nav-item"        id="nav-clientes"   onclick="switchPage('clientes-base','nav-clientes')"><span class="nav-icon">🐕</span> Base de Clientes</div>
    <div class="nav-item"        id="nav-relatorios" onclick="switchPage('relatorios','nav-relatorios')"> <span class="nav-icon">📊</span> Relatórios</div>

    <div class="nav-bottom">
        <div class="nav-item" id="nav-cliente-area" onclick="switchPage('login-cliente','nav-cliente-area')"><span class="nav-icon">👤</span> Área do Cliente</div>
    </div>
</nav>

<!-- ═══════════════ MAIN ═══════════════ -->
<main>

<!-- ── DASHBOARD ── -->
<section id="dashboard" class="page active">
    <div class="page-header">
        <div>
            <h2>Painel Principal</h2>
            <p>Bem-vindo(a)! Aqui está o resumo do dia — <span id="data-hoje"></span></p>
        </div>
        <div style="display:flex; align-items:center; gap:10px;">
            <span style="font-size:0.8rem; background:var(--primary-light); color:var(--primary-dark); padding:6px 14px; border-radius:20px; font-weight:800;" id="perfil-badge">👑 Dono</span>
        </div>
    </div>

    <div class="kpi-grid">
        <div class="kpi-card">
            <span class="kpi-label">Faturamento Total</span>
            <span class="kpi-value" id="kpi-faturamento">R$ 0</span>
            <span class="kpi-sub">Todas as vendas registradas</span>
        </div>
        <div class="kpi-card accent-yellow">
            <span class="kpi-label">Vendas Hoje</span>
            <span class="kpi-value" id="kpi-vendas-hoje">0</span>
            <span class="kpi-sub">Transações concluídas</span>
        </div>
        <div class="kpi-card accent-purple">
            <span class="kpi-label">Agendamentos</span>
            <span class="kpi-value" id="kpi-agendamentos">0</span>
            <span class="kpi-sub">Total cadastrados</span>
        </div>
        <div class="kpi-card accent-red">
            <span class="kpi-label">Alertas de Estoque</span>
            <span class="kpi-value" id="kpi-alertas">0</span>
            <span class="kpi-sub">Itens abaixo do mínimo</span>
        </div>
    </div>

    <div class="dash-grid">
        <div class="panel">
            <h3>📋 Movimentação Recente</h3>
            <div id="log-movimentacao"><p style="color:var(--text-muted);font-size:0.82rem;">Nenhuma movimentação ainda.</p></div>
        </div>
        <div>
            <div class="panel">
                <h3>⚠️ Alertas de Estoque Baixo</h3>
                <div id="alerta-estoque-painel"><p style="color:var(--text-muted);font-size:0.82rem;">Tudo em ordem.</p></div>
            </div>
            <div class="panel">
                <h3>📅 Próximos Agendamentos</h3>
                <div id="proximos-agendamentos"><p style="color:var(--text-muted);font-size:0.82rem;">Nenhum agendamento.</p></div>
            </div>
        </div>
    </div>
</section>

<!-- ── VENDAS ── -->
<section id="vendas" class="page">
    <div class="page-header">
        <div><h2>Frente de Caixa</h2><p>Selecione produtos e finalize a venda</p></div>
    </div>

    <div class="vendas-layout">
        <!-- VITRINE -->
        <div>
            <div class="panel" style="padding-bottom: 8px;">
                <!-- Barra de busca -->
                <div class="search-bar">
                    <span>🔍</span>
                    <input type="text" id="busca-produto" placeholder="Buscar produto..." oninput="renderGallery()">
                </div>

                <!-- Tabs de categoria -->
                <div class="cat-tabs">
                    <button class="cat-tab all active"       onclick="setCategoria('todas',this)">🛒 Todos</button>
                    <button class="cat-tab alimento"         onclick="setCategoria('alimento',this)">🦴 Alimentação</button>
                    <button class="cat-tab higiene"          onclick="setCategoria('higiene',this)">🧼 Higiene & Saúde</button>
                    <button class="cat-tab acessorio"        onclick="setCategoria('acessorio',this)">🎀 Acessórios</button>
                </div>
            </div>

            <!-- Galeria -->
            <div id="vitrine-container"></div>
        </div>

        <!-- CARRINHO -->
        <div class="panel" style="position: sticky; top: 0; max-height: calc(100vh - 80px); display:flex; flex-direction:column;">
            <h3>🛒 Carrinho</h3>
            <div id="carrinho-itens" style="flex:1; overflow-y:auto; min-height:120px;"></div>
            <div id="carrinho-vazio" style="color:var(--text-muted); font-size:0.82rem; padding: 20px 0; text-align:center;">Nenhum item adicionado.</div>
            <hr style="border:none; border-top:1px solid var(--border); margin: 12px 0;">
            <div class="total-row"><span>Total</span><span id="total-venda">R$ 0,00</span></div>
            <button class="btn btn-success btn-block" onclick="abrirPagamento()" style="margin-top:14px;">Finalizar Venda →</button>
            <button class="btn btn-block" style="background:#f0f0f0; color:var(--text-muted); margin-top:6px;" onclick="limparCarrinho()">Limpar Carrinho</button>
        </div>
    </div>
</section>

<!-- ── ESTOQUE ── -->
<section id="estoque" class="page">
    <div class="page-header">
        <div><h2>Inventário</h2><p>Gerencie produtos e quantidades em tempo real</p></div>
        <button class="btn btn-primary" onclick="openModal('modal-novo-item')">+ Novo Produto</button>
    </div>
    <div class="panel">
        <div class="estoque-filtros">
            <input type="text" id="busca-estoque" placeholder="🔍  Buscar produto..." oninput="renderEstoque()" style="width:220px;">
            <select id="filtro-cat-estoque" onchange="renderEstoque()">
                <option value="">Todas as categorias</option>
                <option value="alimento">🦴 Alimentação</option>
                <option value="higiene">🧼 Higiene & Saúde</option>
                <option value="acessorio">🎀 Acessórios</option>
            </select>
            <select id="filtro-status-estoque" onchange="renderEstoque()">
                <option value="">Todos os status</option>
                <option value="cheio">✅ Cheio</option>
                <option value="medio">⚠️ Médio</option>
                <option value="baixo">🔴 Baixo</option>
            </select>
        </div>
        <table>
            <thead>
                <tr>
                    <th>Produto</th><th>Categoria</th><th>Qtd</th><th>Mín.</th><th>Status</th><th>Preço</th><th>Ações</th>
                </tr>
            </thead>
            <tbody id="estoque-body"></tbody>
        </table>
    </div>
</section>

<!-- ── AGENDA ── -->
<section id="agenda" class="page">
    <div class="page-header">
        <div><h2>Agenda Master</h2><p>Gerencie todos os atendimentos e consultas</p></div>
        <button class="btn btn-primary" onclick="openModal('modal-agenda')">+ Novo Agendamento</button>
    </div>
    <div class="panel">
        <div class="estoque-filtros" style="margin-bottom:14px;">
            <select id="filtro-status-agenda" onchange="renderAgenda()">
                <option value="">Todos os status</option>
                <option value="Ocorrendo">🟡 Ocorrendo</option>
                <option value="Deu Certo">🟢 Deu Certo</option>
                <option value="Errado">🔴 Errado</option>
            </select>
            <select id="filtro-prof-agenda" onchange="renderAgenda()">
                <option value="">Todos os profissionais</option>
                <option>Dr. Roberto (Veterinário)</option>
                <option>Dra. Juliana (Vacinas)</option>
                <option>Lucas (Esteticista/Tosa)</option>
                <option>Mariana (Banhista)</option>
            </select>
        </div>
        <table>
            <thead><tr><th>Data/Hora</th><th>Tutor</th><th>Pet</th><th>Serviço</th><th>Profissional</th><th>Status</th><th>Ações</th></tr></thead>
            <tbody id="agenda-body"></tbody>
        </table>
    </div>
</section>

<!-- ── BASE DE CLIENTES ── -->
<section id="clientes-base" class="page">
    <div class="page-header">
        <div><h2>Base de Clientes</h2><p>Tutores e pets cadastrados automaticamente via agendamento</p></div>
    </div>
    <div class="panel">
        <div class="search-bar" style="margin-bottom:16px; max-width:400px;">
            <span>🔍</span>
            <input type="text" id="busca-cliente" placeholder="Buscar por nome ou CPF..." oninput="renderClientesBase()">
        </div>
        <div id="lista-clientes-geral"></div>
    </div>
</section>

<!-- ── RELATÓRIOS ── -->
<section id="relatorios" class="page">
    <div class="page-header">
        <div><h2>Relatórios</h2><p>Visão geral do desempenho financeiro e operacional</p></div>
    </div>
    <div style="display:grid; grid-template-columns: 1fr 1fr; gap:18px;">
        <div class="panel">
            <h3>💰 Financeiro</h3>
            <div id="rel-financeiro"></div>
        </div>
        <div class="panel">
            <h3>📦 Estoque</h3>
            <div id="rel-estoque"></div>
        </div>
        <div class="panel">
            <h3>📅 Agendamentos por Profissional</h3>
            <div id="rel-profissional"></div>
        </div>
        <div class="panel">
            <h3>🐾 Serviços mais Agendados</h3>
            <div id="rel-servicos"></div>
        </div>
    </div>
</section>

<!-- ── ÁREA DO CLIENTE ── -->
<section id="login-cliente" class="page">
    <div style="max-width:420px; margin: 40px auto;">
        <div class="panel" id="tutor-login-box" style="text-align:center;">
            <div style="font-size:3rem; margin-bottom:10px;">🐾</div>
            <h2 style="font-family:'DM Serif Display',serif; color:var(--primary-dark); margin-bottom:6px;">Portal do Tutor</h2>
            <p style="font-size:0.82rem; color:var(--text-muted); margin-bottom:20px;">Acesse seu histórico e agendamentos</p>
            <div class="form-group">
                <label>CPF (somente números)</label>
                <input type="text" id="login-cpf" placeholder="00000000000" maxlength="11">
            </div>
            <button class="btn btn-primary btn-block" onclick="loginCliente()">Entrar no Portal →</button>
        </div>
        <div id="perfil-cliente-view" style="display:none;"></div>
    </div>
</section>

</main>

<!-- ═══════════════ MODAIS ═══════════════ -->

<!-- MODAL: NOVO PRODUTO (ESTOQUE) -->
<div id="modal-novo-item" class="modal">
    <button class="modal-close" onclick="closeModals()">✕</button>
    <h3>Cadastrar Produto</h3>
    <div class="form-row">
        <div class="form-group">
            <label>Nome do Produto</label>
            <input type="text" id="est-nome" placeholder="Ex: Ração Golden">
        </div>
        <div class="form-group">
            <label>Categoria</label>
            <select id="est-categoria">
                <option value="alimento">🦴 Alimentação</option>
                <option value="higiene">🧼 Higiene & Saúde</option>
                <option value="acessorio">🎀 Acessórios</option>
            </select>
        </div>
    </div>
    <div class="form-row">
        <div class="form-group">
            <label>Quantidade Inicial</label>
            <input type="number" id="est-qtd" placeholder="0" min="0">
        </div>
        <div class="form-group">
            <label>Qtd. Mínima (alerta)</label>
            <input type="number" id="est-min" placeholder="5" min="1">
        </div>
    </div>
    <div class="form-row">
        <div class="form-group">
            <label>Preço de Venda (R$)</label>
            <input type="number" id="est-preco" placeholder="0,00" step="0.01">
        </div>
        <div class="form-group">
            <label>Ícone (emoji)</label>
            <input type="text" id="est-icon" placeholder="🦴">
        </div>
    </div>
    <button class="btn btn-primary btn-block" onclick="salvarEstoque()">Cadastrar Produto</button>
</div>

<!-- MODAL: EDITAR QUANTIDADE -->
<div id="modal-editar-item" class="modal">
    <button class="modal-close" onclick="closeModals()">✕</button>
    <h3>Atualizar Estoque</h3>
    <input type="hidden" id="edit-id">
    <div class="form-group">
        <label>Nome do Produto</label>
        <input type="text" id="edit-nome" disabled style="background:#f5f5f5; color:#888;">
    </div>
    <div class="form-row">
        <div class="form-group">
            <label>Nova Quantidade</label>
            <input type="number" id="edit-qtd" min="0">
        </div>
        <div class="form-group">
            <label>Novo Preço (R$)</label>
            <input type="number" id="edit-preco" step="0.01">
        </div>
    </div>
    <button class="btn btn-primary btn-block" onclick="salvarEdicaoEstoque()">Salvar Alterações</button>
</div>

<!-- MODAL: PAGAMENTO -->
<div id="modal-pagamento" class="modal">
    <button class="modal-close" onclick="closeModals()">✕</button>
    <h3>Finalizar Pagamento</h3>
    <div style="text-align:center; margin-bottom:18px;">
        <span style="font-family:'DM Serif Display',serif; font-size:2rem; color:var(--primary-dark);" id="pag-valor-txt"></span>
    </div>

    <div id="pag-opcoes">
        <p style="font-size:0.8rem; color:var(--text-muted); margin-bottom:10px; font-weight:700; text-transform:uppercase; letter-spacing:1px;">Selecione a forma de pagamento</p>
        <div class="pag-metodos">
            <button class="pag-btn" onclick="telaMetodo('PIX')"><span class="pag-icon">💠</span>PIX</button>
            <button class="pag-btn" onclick="telaMetodo('Cartão')"><span class="pag-icon">💳</span>Cartão</button>
            <button class="pag-btn" onclick="telaMetodo('Dinheiro')"><span class="pag-icon">💵</span>Dinheiro</button>
        </div>
    </div>

    <div id="metodo-pix" style="display:none; text-align:center;">
        <div class="qr-wrap">
            <div class="qr-mock">
                <svg width="160" height="160" viewBox="0 0 29 29" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M0 0h7v7H0zm2 2v3h3V2zm1 1h1v1H3zm4-3h1v1H7zm1 1h2v1H8zm1 1h1v3H9zm-1 2h1v1H8zm-8 5h1v1H0zm2 0h2v1H2zm3 0h1v2H5zm2 0h1v1H7zm4 0h1v1h-1zm2 0h3v1h-3zm4 0h2v1h-2zm3 0h2v2h-2zm-12 1h1v2h-1zm4 0h1v1h-1zm1 0h1v1h-1zm2 0h1v1h-1zm1 0h2v1h-2zm2 0h1v1h-1zm-13 1h1v1H1zm2 0h1v1H3zm2 0h1v1H5zm6 0h2v1h-2zm3 0h1v1h-1zm4 0h2v1h-2zm3 0h1v1h-1zm-19 1h1v1H0zm4 0h2v1H4zm5 0h1v1H9zm1 0h1v2h-1zm2 0h1v1h-1zm3 0h1v1h-1zm1 0h2v1h-2zm3 0h2v1h-2zm-15 1h1v1H2zm2 0h1v1H4zm7 0h1v1h-1zm5 0h2v1h-2zm4 0h1v2h-1zm2 0h1v1h-1zm-21 1h7v7H0zm2 2v3h3V2zm1 1h1v1H3zm5-2h1v1H8zm2 0h1v1h-1zm2 0h1v1h-1zm1 0h2v1h-2zm3 0h1v1h-1zm2 0h1v1h-1zm2 0h3v1h-3zm-13 1h1v1h-1zm2 0h2v1h-2zm3 0h1v2h-1zm4 0h1v1h-1zm1 0h1v1h-1zm2 0h2v1h-2zm1 0h1v1h-1zm-14 1h1v1h-1zm5 0h1v1H9zm4 0h1v1h-1zm2 0h1v1h-1zm5 0h1v1h-1zm1 0h1v1h-1zm2 0h1v1h-1zm-18 1h1v1H4zm3 0h1v1H7zm1 0h1v1H8zm3 0h1v1h-1zm1 0h2v1h-2zm4 0h1v1h-1zm1 0h1v1h-1zm3 0h2v1h-2zm-17 1h1v1H5zm4 0h1v1H9zm5 0h1v1h-1zm2 0h1v1h-1zm1 0h1v1h-1zm2 0h1v1h-1zm3 0h1v1h-1zm-16 1h2v1H6zm3 0h1v1H9zm2 0h2v1h-2zm3 0h1v1h-1zm2 0h2v1h-2zm4 0h1v1h-1z" fill="#000"/>
                </svg>
            </div>
            <p style="font-size:0.82rem; color:var(--text-muted); margin-top:10px;">Escaneie o código para pagar</p>
        </div>
        <div style="font-size:0.8rem; background: var(--primary-light); padding:10px 14px; border-radius:8px; color:var(--primary-dark); margin-bottom:12px;">
            Chave PIX: <strong>auqmia@petcare.com.br</strong>
        </div>
        <button class="btn btn-success btn-block" onclick="concluirVenda('PIX')">✅ Confirmar Recebimento</button>
    </div>

    <div id="metodo-cartao" style="display:none;">
        <div class="form-group">
            <label>Número de Parcelas</label>
            <select id="cartao-parcelas"></select>
        </div>
        <div class="form-group">
            <label>Bandeira</label>
            <select id="cartao-bandeira">
                <option>Visa</option><option>Mastercard</option><option>Elo</option><option>Amex</option>
            </select>
        </div>
        <button class="btn btn-success btn-block" onclick="concluirVenda('Cartão')">✅ Confirmar Cartão</button>
    </div>

    <div id="metodo-dinheiro" style="display:none;">
        <div class="form-group">
            <label>Valor Entregue pelo Cliente (R$)</label>
            <input type="number" id="dinheiro-pago" placeholder="0,00" oninput="calcularTroco()">
        </div>
        <div style="display:flex; justify-content:space-between; background:var(--bg); padding:12px 16px; border-radius:10px; margin:8px 0;">
            <span style="font-weight:700;">Troco:</span>
            <span id="troco-txt" style="font-weight:900; color:var(--primary-dark);">R$ 0,00</span>
        </div>
        <button class="btn btn-success btn-block" onclick="concluirVenda('Dinheiro')">✅ Confirmar Pagamento</button>
    </div>
</div>

<!-- MODAL: AGENDA -->
<div id="modal-agenda" class="modal">
    <button class="modal-close" onclick="closeModals()">✕</button>
    <h3>Novo Agendamento</h3>
    <div class="form-row">
        <div class="form-group">
            <label>Nome do Tutor</label>
            <input type="text" id="ag-tutor" placeholder="Nome completo">
        </div>
        <div class="form-group">
            <label>CPF (11 dígitos)</label>
            <input type="text" id="ag-cpf" placeholder="00000000000" maxlength="11">
        </div>
    </div>
    <div class="form-row">
        <div class="form-group">
            <label>Nome do Pet</label>
            <input type="text" id="ag-pet" placeholder="Nome do animal">
        </div>
        <div class="form-group">
            <label>Raça</label>
            <input type="text" id="ag-raca" placeholder="Raça do animal">
        </div>
    </div>
    <div class="form-row">
        <div class="form-group">
            <label>Serviço</label>
            <select id="ag-servico">
                <option>Banho e Tosa</option>
                <option>Consulta Médica</option>
                <option>Vacina</option>
                <option>Castração</option>
                <option>Retorno</option>
            </select>
        </div>
        <div class="form-group">
            <label>Profissional</label>
            <select id="ag-profissional">
                <option value="">Selecione...</option>
                <option>Dr. Roberto (Veterinário)</option>
                <option>Dra. Juliana (Vacinas)</option>
                <option>Lucas (Esteticista/Tosa)</option>
                <option>Mariana (Banhista)</option>
            </select>
        </div>
    </div>
    <div class="form-group">
        <label>Data e Hora</label>
        <input type="datetime-local" id="ag-data">
    </div>
    <div class="form-group">
        <label>Observações</label>
        <input type="text" id="ag-obs" placeholder="Alergia a algum produto, temperamento do pet...">
    </div>
    <button class="btn btn-primary btn-block" onclick="salvarAgendamento()">Confirmar Agendamento</button>
</div>

<!-- MODAL: SUCESSO VENDA -->
<div id="modal-sucesso" class="modal" style="text-align:center;">
    <div style="font-size:3.5rem; margin-bottom:10px;">✅</div>
    <h3 style="font-size:1.5rem;">Venda Concluída!</h3>
    <p id="sucesso-detalhe" style="color:var(--text-muted); font-size:0.85rem; margin:10px 0 20px;"></p>
    <button class="btn btn-primary btn-block" onclick="closeModals()">Fechar</button>
</div>

<script>
    /* ══════════════ BANCO DE DADOS ══════════════ */
    const PALAVRAS_BANIDAS = ["palavrao1", "palavrao2"];

    let db = {
        estoque: [
            { id: 1, nome: "Ração Golden",        qtd: 20, min: 8,  preco: 150.00, icon: "🦴", max: 40, categoria: "alimento" },
            { id: 2, nome: "Ração Premium Gatos",  qtd: 3,  min: 5,  preco: 85.00,  icon: "🐟", max: 20, categoria: "alimento" },
            { id: 3, nome: "Petisco Ossinho",       qtd: 35, min: 10, preco: 12.00,  icon: "🥩", max: 50, categoria: "alimento" },
            { id: 4, nome: "Shampoo Pet Neutro",    qtd: 5,  min: 6,  preco: 28.00,  icon: "🧴", max: 20, categoria: "higiene"  },
            { id: 5, nome: "Perfume Dog Colônia",   qtd: 12, min: 4,  preco: 35.00,  icon: "🌸", max: 25, categoria: "higiene"  },
            { id: 6, nome: "Condicionador Brilho",  qtd: 8,  min: 4,  preco: 22.00,  icon: "✨", max: 20, categoria: "higiene"  },
            { id: 7, nome: "Coleira Ajustável",     qtd: 10, min: 3,  preco: 45.00,  icon: "🏷️", max: 20, categoria: "acessorio"},
            { id: 8, nome: "Brinquedo Mordedor",    qtd: 2,  min: 4,  preco: 18.00,  icon: "🦷", max: 15, categoria: "acessorio"},
            { id: 9, nome: "Roupinha Inverno",      qtd: 7,  min: 3,  preco: 75.00,  icon: "🧥", max: 15, categoria: "acessorio"},
        ],
        agenda: [],
        clientes: [],
        vendasTotais: 0,
        vendasHoje: 0,
        historicoVendas: [],
        logs: []
    };

    let carrinho = [];
    let catAtiva = 'todas';

    /* ══════════════ UTILIDADES ══════════════ */
    function limparTexto(t) {
        let s = t.trim();
        PALAVRAS_BANIDAS.forEach(p => s = s.toLowerCase().replace(p, "****"));
        return s;
    }

    function fmt(v) { return `R$ ${parseFloat(v).toFixed(2).replace('.', ',')}` }

    function getStatusEstoque(p) {
        const pct = (p.qtd / Math.max(p.max, 1)) * 100;
        if (p.qtd <= p.min) return 'baixo';
        if (pct > 60)        return 'cheio';
        return 'medio';
    }

    function getCatLabel(c) {
        const map = { alimento:'🦴 Alimentação', higiene:'🧼 Higiene & Saúde', acessorio:'🎀 Acessórios' };
        return map[c] || c;
    }

    /* ══════════════ NAVEGAÇÃO ══════════════ */
    function switchPage(id, navId) {
        document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
        document.getElementById(id).classList.add('active');
        document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
        if(navId) {
            const el = document.getElementById(navId);
            if(el) el.classList.add('active');
        }
        if(id === 'vendas')       { carrinho = []; renderCarrinho(); renderGallery(); }
        if(id === 'estoque')      renderEstoque();
        if(id === 'clientes-base') renderClientesBase();
        if(id === 'agenda')        renderAgenda();
        if(id === 'relatorios')    renderRelatorios();
        if(id === 'login-cliente') sairAreaCliente();
    }

    /* ══════════════ PERFIL ══════════════ */
    function changeProfile() {
        const perfil = document.getElementById('user-profile').value;
        const labels = { dono:'👑 Dono', estoquista:'📦 Estoquista', recepcionista:'📞 Recepcionista' };
        document.getElementById('perfil-badge').textContent = labels[perfil];

        const map = {
            dono:         ['nav-dash','nav-vendas','nav-estoque','nav-agenda','nav-clientes','nav-relatorios'],
            estoquista:   ['nav-dash','nav-estoque'],
            recepcionista:['nav-dash','nav-vendas','nav-agenda','nav-clientes']
        };

        document.querySelectorAll('.nav-item[id]').forEach(nav => {
            nav.style.display = map[perfil].includes(nav.id) ? 'flex' : 'none';
        });
        switchPage('dashboard', 'nav-dash');
    }

    /* ══════════════ DASHBOARD ══════════════ */
    function updateDash() {
        document.getElementById('kpi-faturamento').textContent = fmt(db.vendasTotais);
        document.getElementById('kpi-vendas-hoje').textContent = db.vendasHoje;
        document.getElementById('kpi-agendamentos').textContent = db.agenda.length;

        const alertas = db.estoque.filter(p => p.qtd <= p.min);
        document.getElementById('kpi-alertas').textContent = alertas.length;

        // Logs
        const logEl = document.getElementById('log-movimentacao');
        if(!db.logs.length) {
            logEl.innerHTML = '<p style="color:var(--text-muted);font-size:0.82rem;">Nenhuma movimentação ainda.</p>';
        } else {
            logEl.innerHTML = db.logs.slice(0,6).map(l => {
                let tipo = l.includes('VENDA') ? 'venda' : l.includes('AGENDADO') ? 'agenda' : 'estoque';
                return `<div class="log-item"><div class="log-dot ${tipo}"></div><span>${l}</span></div>`;
            }).join('');
        }

        // Alertas estoque
        const alertEl = document.getElementById('alerta-estoque-painel');
        if(!alertas.length) {
            alertEl.innerHTML = '<p style="color:var(--text-muted);font-size:0.82rem;">Tudo em ordem. ✅</p>';
        } else {
            alertEl.innerHTML = alertas.map(p => `
                <div class="alert-item ${p.qtd === 0 ? 'danger' : 'warning'}">
                    <span>${p.icon} ${p.nome}</span>
                    <span style="font-weight:800;">${p.qtd} un.</span>
                </div>`).join('');
        }

        // Próximos agendamentos
        const proxEl = document.getElementById('proximos-agendamentos');
        const prox = db.agenda.filter(a => a.status === 'Ocorrendo').slice(0,4);
        if(!prox.length) {
            proxEl.innerHTML = '<p style="color:var(--text-muted);font-size:0.82rem;">Nenhum agendamento pendente.</p>';
        } else {
            proxEl.innerHTML = prox.map(a => `
                <div class="log-item">
                    <div class="log-dot agenda"></div>
                    <span>${a.pet} — ${a.servico} <span style="color:var(--text-muted)">${a.profissional.split(' ')[0]}</span></span>
                </div>`).join('');
        }
    }

    function addLog(t) {
        db.logs.unshift(`${new Date().toLocaleTimeString('pt-br')} — ${t}`);
        updateDash();
    }

    /* ══════════════ GALERIA DE PRODUTOS (VENDAS) ══════════════ */
    function setCategoria(cat, el) {
        catAtiva = cat;
        document.querySelectorAll('.cat-tab').forEach(t => t.classList.remove('active'));
        el.classList.add('active');
        renderGallery();
    }

    function renderGallery() {
        const busca = (document.getElementById('busca-produto')?.value || '').toLowerCase();
        const container = document.getElementById('vitrine-container');

        const cats = catAtiva === 'todas' ? ['alimento','higiene','acessorio'] : [catAtiva];
        const catMeta = {
            alimento:  { label: '🦴 Alimentação',    cls: 'alimento' },
            higiene:   { label: '🧼 Higiene & Saúde', cls: 'higiene' },
            acessorio: { label: '🎀 Acessórios',      cls: 'acessorio' },
        };

        let html = '';
        cats.forEach(cat => {
            const prods = db.estoque.filter(p => p.categoria === cat && (!busca || p.nome.toLowerCase().includes(busca)));
            if(!prods.length) return;

            const m = catMeta[cat];
            html += `<div class="cat-section"><div class="panel">`;
            html += `<div class="cat-section-title ${m.cls}">${m.label}</div>`;
            html += `<div class="product-grid">`;
            prods.forEach(p => {
                const noCarrinho = carrinho.filter(i => i.id === p.id).length;
                const disponivel = p.qtd - noCarrinho;
                const semEst = disponivel <= 0;
                html += `
                <div class="product-card ${cat} ${semEst ? 'sem-estoque' : ''}" onclick="${semEst ? '' : `addToCarrinho(${p.id})`}">
                    <div class="badge-cat ${cat}"></div>
                    <div class="prod-icon">${p.icon}</div>
                    <div class="prod-name">${p.nome}</div>
                    <div class="prod-preco">${fmt(p.preco)}</div>
                    <div class="prod-estoque">${semEst ? '⚠️ Sem estoque' : `${disponivel} disponíveis`}</div>
                </div>`;
            });
            html += `</div></div></div>`;
        });

        if(!html) html = `<div class="panel" style="text-align:center; color:var(--text-muted); padding:40px;">Nenhum produto encontrado.</div>`;
        container.innerHTML = html;
    }

    function addToCarrinho(id) {
        const p = db.estoque.find(i => i.id === id);
        const noCarrinho = carrinho.filter(i => i.id === id).length;
        if(noCarrinho >= p.qtd) {
            alert(`Atenção: Estoque esgotado para "${p.nome}"!`);
            return;
        }
        carrinho.push({ ...p });
        renderCarrinho();
        renderGallery();
    }

    function removerDoCarrinho(idx) {
        carrinho.splice(idx, 1);
        renderCarrinho();
        renderGallery();
    }

    function limparCarrinho() {
        carrinho = [];
        renderCarrinho();
        renderGallery();
    }

    function renderCarrinho() {
        const itemsEl = document.getElementById('carrinho-itens');
        const vazioEl = document.getElementById('carrinho-vazio');
        const total = carrinho.reduce((a,b) => a + b.preco, 0);
        document.getElementById('total-venda').textContent = fmt(total);

        if(!carrinho.length) {
            itemsEl.innerHTML = '';
            if(vazioEl) vazioEl.style.display = 'block';
            return;
        }
        if(vazioEl) vazioEl.style.display = 'none';

        // Agrupa por produto para exibir quantidade
        const agrupado = {};
        carrinho.forEach((item, idx) => {
            if(!agrupado[item.id]) agrupado[item.id] = { ...item, indices: [] };
            agrupado[item.id].indices.push(idx);
        });

        itemsEl.innerHTML = Object.values(agrupado).map(g => {
            const sub = g.preco * g.indices.length;
            return `<div class="carrinho-item">
                <span>${g.icon} ${g.nome} <span style="color:var(--text-muted);">×${g.indices.length}</span></span>
                <span style="display:flex;align-items:center;gap:8px;">
                    <span style="font-weight:800;">${fmt(sub)}</span>
                    <button onclick="removerDoCarrinho(${g.indices[g.indices.length-1]})">✕</button>
                </span>
            </div>`;
        }).join('');
    }

    /* ══════════════ PAGAMENTO ══════════════ */
    function abrirPagamento() {
        if(!carrinho.length) { alert('Adicione produtos ao carrinho antes!'); return; }
        const total = carrinho.reduce((a,b) => a + b.preco, 0);
        document.getElementById('pag-valor-txt').textContent = fmt(total);
        ['pix','cartao','dinheiro'].forEach(m => document.getElementById('metodo-'+m).style.display = 'none');
        document.getElementById('pag-opcoes').style.display = 'block';
        openModal('modal-pagamento');
    }

    function telaMetodo(m) {
        document.getElementById('pag-opcoes').style.display = 'none';
        ['pix','cartao','dinheiro'].forEach(x => document.getElementById('metodo-'+x).style.display = 'none');
        if(m === 'PIX')     document.getElementById('metodo-pix').style.display = 'block';
        if(m === 'Dinheiro') document.getElementById('metodo-dinheiro').style.display = 'block';
        if(m === 'Cartão') {
            const total = carrinho.reduce((a,b) => a + b.preco, 0);
            const sel = document.getElementById('cartao-parcelas');
            sel.innerHTML = '';
            for(let i = 1; i <= 12; i++) {
                const v = total / i;
                sel.innerHTML += `<option value="${i}">${i}× de ${fmt(v)}${i===1 ? ' (à vista)' : ''}</option>`;
            }
            document.getElementById('metodo-cartao').style.display = 'block';
        }
    }

    function calcularTroco() {
        const total = carrinho.reduce((a,b) => a + b.preco, 0);
        const pago  = parseFloat(document.getElementById('dinheiro-pago').value) || 0;
        const troco = pago - total;
        document.getElementById('troco-txt').textContent = troco > 0 ? fmt(troco) : 'R$ 0,00';
    }

    function concluirVenda(metodo) {
        const total = carrinho.reduce((a,b) => a + b.preco, 0);

        // Validação final de estoque
        const contagem = {};
        carrinho.forEach(i => contagem[i.id] = (contagem[i.id]||0)+1);
        for(let id in contagem) {
            const p = db.estoque.find(x => x.id == id);
            if(!p || p.qtd < contagem[id]) { alert('Erro de estoque! Verifique os itens.'); return; }
        }

        carrinho.forEach(item => {
            const p = db.estoque.find(i => i.id === item.id);
            if(p) p.qtd--;
        });

        let label = metodo;
        if(metodo === 'Cartão') {
            const parc = document.getElementById('cartao-parcelas').value;
            const band = document.getElementById('cartao-bandeira').value;
            label = `${band} ${parc}×`;
        }

        db.vendasTotais += total;
        db.vendasHoje++;
        db.historicoVendas.push({ data: new Date(), total, metodo: label });
        addLog(`VENDA (${label}): ${fmt(total)}`);
        carrinho = [];

        closeModals();
        renderCarrinho();
        renderEstoque();
        renderGallery();

        // Modal sucesso
        document.getElementById('sucesso-detalhe').textContent = `${fmt(total)} via ${label} — ${new Date().toLocaleTimeString('pt-br')}`;
        openModal('modal-sucesso');
    }

    /* ══════════════ ESTOQUE ══════════════ */
    function salvarEstoque() {
        const nome = limparTexto(document.getElementById('est-nome').value);
        const qtd  = parseInt(document.getElementById('est-qtd').value) || 0;
        const min  = parseInt(document.getElementById('est-min').value) || 5;
        const preco = parseFloat(document.getElementById('est-preco').value) || 0;
        const icon = document.getElementById('est-icon').value || '📦';
        const cat  = document.getElementById('est-categoria').value;

        if(!nome) { alert('Informe o nome do produto!'); return; }

        db.estoque.push({ id: Date.now(), nome, qtd, min, preco, icon, max: qtd*2||20, categoria: cat });
        addLog(`ESTOQUE: Produto "${nome}" cadastrado`);
        renderEstoque();
        renderGallery();
        closeModals();

        // Limpa campos
        ['est-nome','est-qtd','est-min','est-preco','est-icon'].forEach(id => document.getElementById(id).value='');
    }

    function abrirEdicao(id) {
        const p = db.estoque.find(i => i.id === id);
        if(!p) return;
        document.getElementById('edit-id').value    = id;
        document.getElementById('edit-nome').value  = p.nome;
        document.getElementById('edit-qtd').value   = p.qtd;
        document.getElementById('edit-preco').value = p.preco;
        openModal('modal-editar-item');
    }

    function salvarEdicaoEstoque() {
        const id   = parseInt(document.getElementById('edit-id').value);
        const qtd  = parseInt(document.getElementById('edit-qtd').value);
        const preco = parseFloat(document.getElementById('edit-preco').value);
        const p = db.estoque.find(i => i.id === id);
        if(p) { p.qtd = qtd; p.preco = preco; p.max = Math.max(p.max, qtd); }
        addLog(`ESTOQUE: "${p.nome}" atualizado → ${qtd} un.`);
        renderEstoque();
        renderGallery();
        closeModals();
    }

    function renderEstoque() {
        const busca  = (document.getElementById('busca-estoque')?.value||'').toLowerCase();
        const catF   = document.getElementById('filtro-cat-estoque')?.value  || '';
        const statF  = document.getElementById('filtro-status-estoque')?.value || '';

        const body = document.getElementById('estoque-body');
        const prods = db.estoque.filter(p => {
            if(busca && !p.nome.toLowerCase().includes(busca)) return false;
            if(catF  && p.categoria !== catF)                  return false;
            if(statF && getStatusEstoque(p) !== statF)         return false;
            return true;
        });

        body.innerHTML = prods.map(p => {
            const st  = getStatusEstoque(p);
            const stLabel = { cheio:'✅ OK', medio:'⚠️ Médio', baixo:'🔴 Baixo' }[st];
            return `<tr>
                <td>${p.icon} <strong>${p.nome}</strong></td>
                <td><span style="font-size:0.75rem;">${getCatLabel(p.categoria)}</span></td>
                <td><strong>${p.qtd}</strong></td>
                <td style="color:var(--text-muted);">${p.min}</td>
                <td><span class="status-pill status-${st}">${stLabel}</span></td>
                <td>${fmt(p.preco)}</td>
                <td>
                    <button onclick="abrirEdicao(${p.id})" style="border:none; background:var(--primary-light); color:var(--primary-dark); padding:5px 10px; border-radius:6px; cursor:pointer; font-weight:700; font-size:0.75rem;">✏️ Editar</button>
                </td>
            </tr>`;
        }).join('') || `<tr><td colspan="7" style="text-align:center;color:var(--text-muted);padding:20px;">Nenhum produto encontrado.</td></tr>`;
    }

    /* ══════════════ AGENDA ══════════════ */
    function salvarAgendamento() {
        const tutor = limparTexto(document.getElementById('ag-tutor').value);
        const cpf   = document.getElementById('ag-cpf').value.trim();
        const pet   = limparTexto(document.getElementById('ag-pet').value);
        const prof  = document.getElementById('ag-profissional').value;

        if(cpf.length !== 11)  { alert('CPF deve ter exatamente 11 dígitos!'); return; }
        if(!tutor)             { alert('Informe o nome do tutor!'); return; }
        if(!pet)               { alert('Informe o nome do pet!'); return; }
        if(!prof)              { alert('Selecione um profissional!'); return; }

        const novo = {
            id: Date.now(), tutor, cpf, pet,
            raca:        document.getElementById('ag-raca').value,
            servico:     document.getElementById('ag-servico').value,
            profissional:prof,
            data:        document.getElementById('ag-data').value,
            obs:         document.getElementById('ag-obs').value,
            status:      'Ocorrendo'
        };
        db.agenda.push(novo);

        let cli = db.clientes.find(c => c.cpf === cpf);
        if(!cli) { db.clientes.push({ nome: tutor, cpf, pets: [pet] }); }
        else if(!cli.pets.includes(pet)) { cli.pets.push(pet); }

        addLog(`AGENDADO: ${pet} (${novo.servico}) com ${prof.split(' ')[0]}`);
        renderAgenda();
        renderClientesBase();
        closeModals();
        ['ag-tutor','ag-cpf','ag-pet','ag-raca','ag-obs'].forEach(id => document.getElementById(id).value='');
    }

    function alterarStatusAgenda(id, status) {
        const ag = db.agenda.find(a => a.id === id);
        if(ag) { ag.status = status; addLog(`STATUS: ${ag.pet} → ${status}`); renderAgenda(); }
    }

    function excluirAgendamento(id) {
        if(!confirm('Deseja excluir este agendamento?')) return;
        db.agenda = db.agenda.filter(a => a.id !== id);
        renderAgenda();
        updateDash();
    }

    function renderAgenda() {
        const statF = document.getElementById('filtro-status-agenda')?.value || '';
        const profF = document.getElementById('filtro-prof-agenda')?.value   || '';

        const itens = db.agenda.filter(a => {
            if(statF && a.status !== statF)        return false;
            if(profF && a.profissional !== profF)  return false;
            return true;
        });

        const stCls = { 'Ocorrendo':'st-ocorrendo', 'Deu Certo':'st-certo', 'Errado':'st-errado' };

        document.getElementById('agenda-body').innerHTML = itens.map(a => `
            <tr>
                <td>${a.data ? new Date(a.data).toLocaleString('pt-br') : '—'}</td>
                <td>${a.tutor}</td>
                <td><strong>${a.pet}</strong> <span style="color:var(--text-muted);font-size:0.75rem;">${a.raca||''}</span></td>
                <td>${a.servico}</td>
                <td><strong>${a.profissional}</strong></td>
                <td><span class="agenda-status ${stCls[a.status]||''}">${a.status}</span></td>
                <td class="agenda-actions">
                    <button class="btn-ok"  onclick="alterarStatusAgenda(${a.id},'Deu Certo')">✅</button>
                    <button class="btn-err" onclick="alterarStatusAgenda(${a.id},'Errado')">❌</button>
                    <button class="btn-del" onclick="excluirAgendamento(${a.id})">🗑️</button>
                </td>
            </tr>`).join('') || `<tr><td colspan="7" style="text-align:center;color:var(--text-muted);padding:20px;">Nenhum agendamento encontrado.</td></tr>`;
    }

    /* ══════════════ CLIENTES ══════════════ */
    function renderClientesBase() {
        const busca = (document.getElementById('busca-cliente')?.value||'').toLowerCase();
        const lista = db.clientes.filter(c =>
            !busca || c.nome.toLowerCase().includes(busca) || c.cpf.includes(busca)
        );
        const el = document.getElementById('lista-clientes-geral');

        if(!lista.length) { el.innerHTML = '<p style="color:var(--text-muted);">Nenhum cliente cadastrado.</p>'; return; }

        el.innerHTML = lista.map(c => {
            const ags = db.agenda.filter(a => a.cpf === c.cpf).length;
            return `<div class="cliente-card">
                <div>
                    <div class="c-name">👤 ${c.nome}</div>
                    <div class="c-info">CPF: ${c.cpf} · Pets: ${c.pets.join(', ')}</div>
                </div>
                <span class="c-badge">${ags} serviço${ags!==1?'s':''}</span>
            </div>`;
        }).join('');
    }

    /* ══════════════ RELATÓRIOS ══════════════ */
    function renderRelatorios() {
        // Financeiro
        const vendPix    = db.historicoVendas.filter(v => v.metodo === 'PIX').reduce((a,v)=>a+v.total,0);
        const vendDin    = db.historicoVendas.filter(v => v.metodo === 'Dinheiro').reduce((a,v)=>a+v.total,0);
        const vendCard   = db.historicoVendas.filter(v => v.metodo.includes('×')).reduce((a,v)=>a+v.total,0);
        document.getElementById('rel-financeiro').innerHTML = [
            ['Faturamento Total',    fmt(db.vendasTotais)],
            ['Transações Hoje',      db.vendasHoje + ' vendas'],
            ['Receita via PIX',      fmt(vendPix)],
            ['Receita via Cartão',   fmt(vendCard)],
            ['Receita via Dinheiro', fmt(vendDin)],
        ].map(([l,v]) => `<div class="relatorio-row"><span class="r-label">${l}</span><span class="r-val">${v}</span></div>`).join('');

        // Estoque
        const baixos  = db.estoque.filter(p => getStatusEstoque(p) === 'baixo').length;
        const valTotal = db.estoque.reduce((a,p)=>a+p.qtd*p.preco,0);
        document.getElementById('rel-estoque').innerHTML = [
            ['Total de SKUs',          db.estoque.length + ' produtos'],
            ['Valor em Estoque',        fmt(valTotal)],
            ['Itens com Estoque Baixo', baixos + ' produto(s)'],
            ['Itens sem Estoque',       db.estoque.filter(p=>p.qtd===0).length + ' produto(s)'],
        ].map(([l,v]) => `<div class="relatorio-row"><span class="r-label">${l}</span><span class="r-val">${v}</span></div>`).join('');

        // Por profissional
        const profCont = {};
        db.agenda.forEach(a => profCont[a.profissional] = (profCont[a.profissional]||0)+1);
        const profHtml = Object.entries(profCont).sort((a,b)=>b[1]-a[1]).map(([n,c]) =>
            `<div class="relatorio-row"><span class="r-label">${n.split('(')[0].trim()}</span><span class="r-val">${c} atend.</span></div>`
        ).join('') || '<p style="color:var(--text-muted);font-size:0.82rem;">Sem dados.</p>';
        document.getElementById('rel-profissional').innerHTML = profHtml;

        // Por serviço
        const servCont = {};
        db.agenda.forEach(a => servCont[a.servico] = (servCont[a.servico]||0)+1);
        const servHtml = Object.entries(servCont).sort((a,b)=>b[1]-a[1]).map(([s,c]) =>
            `<div class="relatorio-row"><span class="r-label">${s}</span><span class="r-val">${c} vez(es)</span></div>`
        ).join('') || '<p style="color:var(--text-muted);font-size:0.82rem;">Sem dados.</p>';
        document.getElementById('rel-servicos').innerHTML = servHtml;
    }

    /* ══════════════ ÁREA DO CLIENTE ══════════════ */
    function loginCliente() {
        const cpf = document.getElementById('login-cpf').value.trim();
        if(cpf.length !== 11) { alert('CPF deve ter 11 dígitos!'); return; }
        const cli = db.clientes.find(c => c.cpf === cpf);
        const view = document.getElementById('perfil-cliente-view');

        if(!cli) { alert('Tutor não encontrado! O cadastro é feito automaticamente no primeiro agendamento.'); return; }

        const ags = db.agenda.filter(a => a.cpf === cpf);
        const stCls = { 'Ocorrendo':'color:#c07000', 'Deu Certo':'color:#1a7a45', 'Errado':'color:#a00' };

        const histHtml = ags.length ? ags.map(a => `
            <div class="historico-item">
                <strong>${a.data ? new Date(a.data).toLocaleDateString('pt-br') : '—'}</strong> · ${a.pet} (${a.raca||'—'})<br>
                ${a.servico} com <em>${a.profissional}</em>
                · <span style="${stCls[a.status]||''}; font-weight:800;">${a.status}</span>
                ${a.obs ? `<br><span style="color:var(--text-muted);font-size:0.77rem;">Obs: ${a.obs}</span>` : ''}
            </div>`) .join('') : '<p style="color:var(--text-muted);font-size:0.82rem;">Nenhum serviço registrado.</p>';

        document.getElementById('tutor-login-box').style.display = 'none';
        view.style.display = 'block';
        view.innerHTML = `
            <div class="panel">
                <div style="display:flex; justify-content:space-between; align-items:start; margin-bottom:16px;">
                    <div>
                        <h2 style="font-family:'DM Serif Display',serif; color:var(--primary-dark);">Olá, ${cli.nome}! 🐾</h2>
                        <p style="color:var(--text-muted); font-size:0.82rem;">CPF: ${cli.cpf} · Pets: ${cli.pets.join(', ')}</p>
                    </div>
                    <button onclick="sairAreaCliente()" class="btn btn-gray" style="padding:8px 14px; font-size:0.78rem;">Sair 🚪</button>
                </div>
                <hr style="border:none; border-top:1px solid var(--border); margin-bottom:16px;">
                <h3 style="font-size:0.95rem; margin-bottom:12px;">📅 Seus Agendamentos</h3>
                ${histHtml}
            </div>`;
    }

    function sairAreaCliente() {
        document.getElementById('perfil-cliente-view').style.display = 'none';
        document.getElementById('tutor-login-box').style.display = 'block';
        document.getElementById('login-cpf').value = '';
    }

    /* ══════════════ MODAL UTILS ══════════════ */
    function openModal(id) {
        document.getElementById('overlay').style.display = 'block';
        document.getElementById(id).style.display = 'block';
    }
    function closeModals() {
        document.getElementById('overlay').style.display = 'none';
        document.querySelectorAll('.modal').forEach(m => m.style.display = 'none');
    }

    /* ══════════════ INICIALIZAÇÃO ══════════════ */
    document.getElementById('data-hoje').textContent = new Date().toLocaleDateString('pt-br', { weekday:'long', day:'2-digit', month:'long', year:'numeric' });
    renderGallery();
    renderEstoque();
    updateDash();
</script>
</body>
</html>
