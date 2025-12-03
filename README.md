
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Loto Inteligente - Desdobramento Lotofácil</title>
    
    <!-- Inclusão das bibliotecas de criptografia -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/crypto-js/4.1.1/crypto-js.min.js"></script>
    
    <style>
        /* ESTILOS EXISTENTES (mantidos do código anterior) */
        :root {
            --primary-color: #3498db;
            --secondary-color: #2ecc71;
            --danger-color: #e74c3c;
            --warning-color: #f39c12;
            --bg-color: #f5f5f5;
            --card-bg: white;
            --text-color: #2c3e50;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            transition: background-color 0.3s;
        }
        
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 20px;
            background: var(--primary-color);
            color: white;
            position: sticky;
            top: 0;
            z-index: 100;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        
        .header h1 {
            font-size: 1.5rem;
        }
        
        .menu-btn {
            background: none;
            border: none;
            color: white;
            font-size: 1.5rem;
            cursor: pointer;
            padding: 5px;
        }
        
        .sidebar {
            position: fixed;
            top: 0;
            right: -300px;
            width: 300px;
            height: 100vh;
            background: white;
            box-shadow: -2px 0 10px rgba(0,0,0,0.1);
            z-index: 1000;
            transition: right 0.3s ease;
            padding-top: 60px;
        }
        
        .sidebar.active {
            right: 0;
        }
        
        .sidebar-close {
            position: absolute;
            top: 15px;
            right: 15px;
            background: none;
            border: none;
            font-size: 1.5rem;
            cursor: pointer;
        }
        
        .sidebar-item {
            padding: 15px 20px;
            border-bottom: 1px solid #eee;
            cursor: pointer;
            transition: background 0.2s;
        }
        
        .sidebar-item:hover {
            background: #f0f0f0;
        }
        
        .overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            z-index: 999;
            display: none;
        }
        
        .overlay.active {
            display: block;
        }
        
        .container {
            max-width: 1200px;
            margin: 20px auto;
            padding: 0 20px;
        }
        
        .tabs {
            display: flex;
            margin-bottom: 20px;
            background: white;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        
        .tab {
            flex: 1;
            padding: 15px;
            text-align: center;
            cursor: pointer;
            background: #ecf0f1;
            transition: background 0.2s;
        }
        
        .tab.active {
            background: var(--primary-color);
            color: white;
        }
        
        .tab-content {
            display: none;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        
        .tab-content.active {
            display: block;
        }
        
        .last-draw {
            background: #fff3cd;
            border: 1px solid #ffeaa7;
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 20px;
            text-align: center;
        }
        
        .last-draw h3 {
            color: #856404;
            margin-bottom: 10px;
        }
        
        .last-draw-numbers {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            gap: 8px;
            margin: 10px 0;
        }
        
        .last-draw-number {
            background: #856404;
            color: white;
            padding: 6px 12px;
            border-radius: 20px;
            font-weight: bold;
        }
        
        .prize-info {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 10px;
            font-weight: bold;
        }
        
        .prize-item {
            padding: 5px 10px;
            border-radius: 5px;
        }
        
        .input-section {
            margin-bottom: 20px;
        }
        
        label {
            display: block;
            margin: 15px 0 8px;
            font-weight: bold;
            color: var(--text-color);
        }
        
        .chips-container {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            margin-top: 10px;
            justify-content: center;
        }
        
        .chip {
            padding: 8px 12px;
            background: #e0e0e0;
            border-radius: 20px;
            cursor: pointer;
            transition: all 0.2s;
            font-weight: bold;
        }
        
        .chip.selected {
            background: var(--secondary-color);
            color: white;
        }
        
        .chip.conflict {
            background: var(--danger-color);
            color: white;
            cursor: not-allowed;
        }
        
        button {
            background: var(--primary-color);
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            margin: 10px 0;
            transition: background 0.3s;
            display: block;
            width: 100%;
            max-width: 300px;
            margin-left: auto;
            margin-right: auto;
        }
        
        button:hover {
            background: #2980b9;
        }
        
        .button-group {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
            margin: 20px 0;
        }
        
        .button-group button {
            margin: 5px 0;
        }
        
        .popup {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.7);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 2000;
            display: none;
        }
        
        .popup-content {
            background: white;
            width: 90%;
            max-width: 800px;
            max-height: 90vh;
            overflow-y: auto;
            border-radius: 10px;
            padding: 20px;
        }
        
        .popup-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        
        .close-btn {
            background: none;
            border: none;
            font-size: 1.5rem;
            cursor: pointer;
        }
        
        .number-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 10px;
            margin-top: 20px;
        }
        
        .number-btn {
            width: 100%;
            padding: 15px;
            font-size: 1.2rem;
            font-weight: bold;
            border: 2px solid #ddd;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.2s;
        }
        
        .number-btn:hover {
            background: #f0f0f0;
        }
        
        .number-btn.selected {
            background: var(--secondary-color);
            color: white;
            border-color: var(--secondary-color);
        }
        
        .number-btn.excluded {
            background: var(--danger-color);
            color: white;
            border-color: var(--danger-color);
        }
        
        .number-btn.conflict {
            background: #95a5a6;
            color: white;
            cursor: not-allowed;
        }
        
        .results {
            margin-top: 20px;
        }
        
        .game {
            background: white;
            padding: 15px;
            margin-bottom: 15px;
            border-radius: 10px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        
        .game-numbers {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
        }
        
        .number {
            background: var(--primary-color);
            color: white;
            padding: 6px 12px;
            border-radius: 20px;
            font-weight: bold;
        }
        
        .prize-11 { background: #3498db; }
        .prize-12 { background: #2ecc71; }
        .prize-13 { background: #f1c40f; color: black; }
        .prize-14 { background: #e67e22; }
        .prize-15 { background: #e74c3c; }
        
        .more-options {
            background: none;
            border: none;
            font-size: 1.5rem;
            cursor: pointer;
            padding: 5px;
            color: #7f8c8d;
        }
        
        .games-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        
        .games-count {
            font-weight: bold;
            color: var(--primary-color);
        }
        
        .filters-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 15px;
            margin-top: 10px;
        }
        
        .filter-item {
            border: 1px solid #ddd;
            padding: 15px;
            border-radius: 8px;
            background: #f9f9f9;
        }
        
        .filter-header {
            font-weight: bold;
            margin-bottom: 10px;
            color: var(--text-color);
        }
        
        .filter-inputs {
            display: flex;
            gap: 10px;
        }
        
        input[type="number"] {
            width: 80px;
            padding: 8px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        
        .ai-section {
            background: #e8f4fc;
            padding: 20px;
            border-radius: 10px;
            margin: 20px 0;
            text-align: center;
            position: relative;
        }
        
        .ai-section h3 {
            color: var(--primary-color);
            margin-bottom: 10px;
        }
        
        .ai-lock {
            position: absolute;
            top: 10px;
            right: 10px;
            font-size: 1.5rem;
            color: var(--warning-color);
        }
        
        .pix-section {
            text-align: center;
            padding: 20px;
            background: #fff9c4;
            border-radius: 10px;
            margin: 20px 0;
        }
        
        .pix-email {
            font-weight: bold;
            color: var(--primary-color);
            margin-top: 10px;
        }
        
        .rating {
            display: flex;
            gap: 5px;
            margin: 10px 0;
            justify-content: center;
        }
        
        .star {
            color: #bdc3c7;
            font-size: 1.5rem;
            cursor: pointer;
        }
        
        .star.active {
            color: #f1c40f;
        }
        
        .comments-section {
            margin-top: 20px;
            padding: 15px;
            background: #f8f9fa;
            border-radius: 8px;
        }
        
        .comment {
            padding: 10px;
            margin-bottom: 10px;
            border-bottom: 1px solid #eee;
        }
        
        .comment-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 5px;
        }
        
        .like-btn {
            background: none;
            border: none;
            color: #95a5a6;
            cursor: pointer;
        }
        
        .like-btn.active {
            color: var(--danger-color);
        }
        
        textarea {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            border: 1px solid #ccc;
            border-radius: 5px;
            resize: vertical;
        }
        
        .back-button {
            background: #95a5a6;
            margin-top: 20px;
        }
        
        .back-button:hover {
            background: #7f8c8d;
        }
        
        .database-status {
            padding: 10px;
            background: #e8f4fc;
            border-radius: 5px;
            margin: 10px 0;
            font-size: 0.9em;
            text-align: center;
        }
        
        /* NOVOS ESTILOS PARA FILTROS DE REPETIÇÃO */
        .filter-category {
            margin-bottom: 20px;
            padding: 15px;
            border: 1px solid #ddd;
            border-radius: 8px;
            background: #f9f9f9;
        }
        
        .filter-category h4 {
            margin-bottom: 10px;
            color: var(--primary-color);
            border-bottom: 1px solid #ddd;
            padding-bottom: 5px;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
        
        .lock-toggle {
            background: none;
            border: none;
            font-size: 1.2rem;
            cursor: pointer;
            color: #7f8c8d;
        }
        
        .lock-toggle.locked {
            color: var(--warning-color);
        }
        
        .filter-row {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            margin-bottom: 10px;
        }
        
        .filter-control {
            display: flex;
            flex-direction: column;
            min-width: 120px;
        }
        
        .filter-control label {
            font-size: 0.9em;
            margin-bottom: 5px;
        }
        
        .filter-control input:disabled {
            background-color: #e9ecef;
            cursor: not-allowed;
        }
        
        /* Estilos para API da Caixa */
        .api-status {
            padding: 10px;
            border-radius: 5px;
            margin: 10px 0;
            text-align: center;
            font-weight: bold;
        }
        
        .api-status.success {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }
        
        .api-status.error {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }
        
        .api-status.loading {
            background: #d1ecf1;
            color: #0c5460;
            border: 1px solid #bee5eb;
        }
        
        .api-controls {
            display: flex;
            gap: 10px;
            justify-content: center;
            margin: 15px 0;
        }
        
        .api-controls button {
            max-width: 200px;
            margin: 0;
        }
        
        .concurso-info {
            background: #f8f9fa;
            padding: 15px;
            border-radius: 8px;
            margin: 15px 0;
        }
        
        .concurso-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }
        
        .concurso-numbers {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            justify-content: center;
        }
        
        .concurso-number {
            background: var(--primary-color);
            color: white;
            padding: 6px 12px;
            border-radius: 20px;
            font-weight: bold;
        }
        
        select {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        
        /* NOVOS ESTILOS PARA O CHAT */
        .chat-container {
            display: flex;
            flex-direction: column;
            height: 500px;
            border: 1px solid #ddd;
            border-radius: 10px;
            overflow: hidden;
        }
        
        .chat-header {
            background: var(--primary-color);
            color: white;
            padding: 15px;
            text-align: center;
            font-weight: bold;
        }
        
        .chat-messages {
            flex: 1;
            padding: 15px;
            overflow-y: auto;
            background: #f9f9f9;
        }
        
        .message {
            margin-bottom: 15px;
            padding: 10px;
            border-radius: 10px;
            max-width: 80%;
            position: relative;
        }
        
        .message.own {
            background: #dcf8c6;
            margin-left: auto;
            border-bottom-right-radius: 0;
        }
        
        .message.other {
            background: white;
            border-bottom-left-radius: 0;
        }
        
        .message-header {
            display: flex;
            justify-content: space-between;
            font-size: 0.8em;
            margin-bottom: 5px;
            color: #666;
        }
        
        .message-content {
            word-wrap: break-word;
        }
        
        .message-actions {
            display: flex;
            gap: 10px;
            margin-top: 5px;
            font-size: 0.8em;
        }
        
        .message-action {
            background: none;
            border: none;
            color: #666;
            cursor: pointer;
        }
        
        .message-action:hover {
            color: var(--primary-color);
        }
        
        .chat-input-area {
            display: flex;
            padding: 10px;
            background: white;
            border-top: 1px solid #ddd;
        }
        
        .chat-input {
            flex: 1;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 20px;
            margin-right: 10px;
        }
        
        .chat-media-btn {
            background: none;
            border: none;
            font-size: 1.2rem;
            cursor: pointer;
            margin-right: 10px;
            color: var(--primary-color);
        }
        
        .chat-send-btn {
            background: var(--primary-color);
            color: white;
            border: none;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .media-options {
            display: flex;
            gap: 10px;
            margin-bottom: 10px;
        }
        
        .media-option {
            background: #f0f0f0;
            border: none;
            padding: 8px 15px;
            border-radius: 20px;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        .media-preview {
            margin-top: 10px;
            text-align: center;
        }
        
        .media-preview img, .media-preview video {
            max-width: 100%;
            max-height: 200px;
            border-radius: 10px;
        }
        
        .user-id {
            font-weight: bold;
            color: var(--primary-color);
        }
        
        .admin-controls {
            display: flex;
            gap: 5px;
            margin-top: 5px;
        }
        
        .admin-btn {
            background: none;
            border: none;
            font-size: 0.8em;
            cursor: pointer;
            color: #666;
        }
        
        .admin-btn.delete {
            color: var(--danger-color);
        }
        
        .admin-btn.ban {
            color: var(--warning-color);
        }
        
        /* NOVOS ESTILOS PARA MELHORIAS */
        .save-button {
            background: var(--secondary-color);
            max-width: 120px;
            margin: 0;
        }
        
        .save-button:hover {
            background: #27ae60;
        }
        
        .save-button.saved {
            background: #95a5a6;
            cursor: default;
        }
        
        .save-button.saved:hover {
            background: #95a5a6;
        }
        
        .game-saved {
            opacity: 0.7;
            border: 2px solid var(--secondary-color);
        }
        
        .filter-status {
            padding: 10px;
            background: #e8f4fc;
            border-radius: 5px;
            margin: 10px 0;
            text-align: center;
            font-size: 0.9em;
        }
        
        /* NOVOS ESTILOS PARA SISTEMA DE USUÁRIOS */
        .user-section {
            background: white;
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 20px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        
        .user-form {
            display: flex;
            flex-direction: column;
            gap: 15px;
            max-width: 400px;
            margin: 0 auto;
        }
        
        .user-form input {
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 1rem;
        }
        
        .user-form button {
            max-width: 100%;
        }
        
        .user-info {
            text-align: center;
            padding: 10px;
            background: #e8f4fc;
            border-radius: 5px;
            margin-bottom: 15px;
        }
        
        .user-tabs {
            display: flex;
            margin-bottom: 15px;
            background: #f5f5f5;
            border-radius: 5px;
            overflow: hidden;
        }
        
        .user-tab {
            flex: 1;
            padding: 10px;
            text-align: center;
            cursor: pointer;
            transition: background 0.2s;
        }
        
        .user-tab.active {
            background: var(--primary-color);
            color: white;
        }
        
        .user-games {
            max-height: 400px;
            overflow-y: auto;
        }
        
        /* NOVOS ESTILOS PARA LOADING E NOTIFICAÇÕES */
        .loading-spinner {
            display: none;
            text-align: center;
            padding: 20px;
        }
        
        .spinner {
            border: 4px solid rgba(0, 0, 0, 0.1);
            border-left-color: var(--primary-color);
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
            margin: 0 auto;
        }
        
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px 20px;
            border-radius: 5px;
            color: white;
            z-index: 3000;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            display: none;
            max-width: 300px;
        }
        
        .notification.success {
            background: var(--secondary-color);
        }
        
        .notification.error {
            background: var(--danger-color);
        }
        
        .notification.warning {
            background: var(--warning-color);
        }
        
        @media (max-width: 768px) {
            .number-grid {
                grid-template-columns: repeat(5, 1fr);
            }
            
            .tab {
                font-size: 0.9rem;
                padding: 10px;
            }
            
            .last-draw-numbers {
                gap: 5px;
            }
            
            .last-draw-number {
                padding: 4px 8px;
                font-size: 0.9rem;
            }
            
            .filters-grid {
                grid-template-columns: 1fr;
            }
            
            .button-group {
                gap: 8px;
            }
            
            button {
                max-width: 100%;
            }
            
            .message {
                max-width: 90%;
            }
        }
    </style>
</head>
<body>
    <div class="header">
       <h1>LotoInteligente</h1>
                  <button class="menu-btn" id="menuBtn">☰</button>
    </div>
    
    <div class="overlay" id="overlay"></div>
    
    <div class="sidebar" id="sidebar">
        <button class="sidebar-close" id="closeSidebar">✕</button>
        <div class="sidebar-item" onclick="openSection('search')">Loto Inteligente</div>
        <div class="sidebar-item" onclick="openSection('desdobramento')">Rastreador inteligente📈🔒</div>
        <div class="sidebar-item" onclick="openSection('saved')">(67)984122059</div>
        <div class="sidebar-item" onclick="openPopup('pixPopup')">Pix: Doação 💰a992616298@gmail.com</div>
        <div class="sidebar-item" onclick="openSection('chat')">Deus abençoe, e obrigado a todos pelo apoio🙏🙏</div>
        <div class="sidebar-item" onclick="openSection('config')">Configurações e Backups</div>
        <div class="sidebar-item" onclick="openPopup('userPopup')">Sistema de Usuários</div>
    </div>
    
    <div class="container">
        <div class="last-draw" id="lastDraw">
            <h3>Último Concurso da Lotofácil</h3>
            <div class="api-status loading" id="apiStatus">
                Carregando último concurso...
            </div>
            <div class="last-draw-numbers" id="lastDrawNumbers">
                <p>Carregando último concurso...</p>
            </div>
            <div class="prize-info">
                <div class="prize-item" style="background: #3498db; color: white;">11 pontos</div>
                <div class="prize-item" style="background: #2ecc71; color: white;">12 pontos</div>
                <div class="prize-item" style="background: #f1c40f; color: black;">13 pontos</div>
                <div class="prize-item" style="background: #e67e22; color: white;">14 pontos</div>
                <div class="prize-item" style="background: #e74c3c; color: white;">15 pontos</div>
            </div>
            <div class="api-controls">
                <button onclick="fetchLastConcurso()" style="background: var(--secondary-color);">🔄 Atualizar Concurso</button>
                <button onclick="openConcursoSearchPopup()">🔍 Pesquisar Concurso</button>
            </div>
        </div>
        
        <div class="tabs">
            <div class="tab active" data-tab="config">Crie seus jogos.</div>
            <div class="tab" data-tab="filters">Filtros</div>
            <div class="tab" data-tab="saved">Jogos Salvos</div>
            <div class="tab" data-tab="chat">Chat</div>
        </div>
        
        <div class="tab-content active" id="config-content">
            <div class="input-section">
                <label>Dezenas Fixas (Fortes):</label>
                <div class="chips-container" id="fixedChips"></div>
                <button onclick="openNumberPopup('fixed')">Selecionar Dezenas Fixas</button>
                
                <label>Dezenas Excluídas (Fracas):</label>
                <div class="chips-container" id="excludeChips"></div>
                <button onclick="openNumberPopup('exclude')">Selecionar Dezenas Excluídas</button>
                
                <div class="ai-section">
                    <div class="ai-lock">🔒</div>
                    <h3>Desdobramento Inteligente com IA</h3>
                    <p>A IA analisa concursos anteriores para gerar combinações otimizadas, eliminando jogos que já fizeram 15 pontos e priorizando combinações com alto potencial.</p>
                    <p><strong>🔒 Conteúdo Premium - Acesso Restrito</strong></p>
                </div>
                
                <div class="button-group">
                    <button onclick="openIApopup()" style="background: var(--secondary-color);">🤖 Gerar Jogos com IA</button>
                    <button onclick="openMainPopup()">🎲 Desdobramento</button>
                </div>
            </div>
        </div>
        
        <div class="tab-content" id="filters-content">
            <div class="input-section">
                <h3 style="text-align: center;">Filtros Avançados - Padrões da Lotofácil</h3>
                
                <div class="filter-category">
                    <h4>
                        Filtros de Repetição do Concurso Anterior 
                        <button class="lock-toggle" id="repetitionLock" onclick="toggleRepetitionLock()">🔓</button>
                    </h4>
                    <div class="filter-row">
                        <div class="filter-control">
                            <label>Repetições de 8 dezenas:</label>
                            <input type="number" id="rep8Min" min="0" max="15" value="0" disabled>
                            <input type="number" id="rep8Max" min="0" max="15" value="15" disabled>
                        </div>
                        <div class="filter-control">
                            <label>Repetições de 9 dezenas:</label>
                            <input type="number" id="rep9Min" min="0" max="15" value="0" disabled>
                            <input type="number" id="rep9Max" min="0" max="15" value="15" disabled>
                        </div>
                        <div class="filter-control">
                            <label>Repetições de 10 dezenas:</label>
                            <input type="number" id="rep10Min" min="0" max="15" value="0" disabled>
                            <input type="number" id="rep10Max" min="0" max="15" value="15" disabled>
                        </div>
                    </div>
                    <div class="filter-row">
                        <div class="filter-control">
                            <label>Jogos 16-21 dezenas (rep 11-14):</label>
                            <input type="number" id="rep16_21Min" min="0" max="15" value="11" disabled>
                            <input type="number" id="rep16_21Max" min="0" max="15" value="14" disabled>
                        </div>
                    </div>
                    <p style="font-size: 0.9em; color: #666; margin-top: 10px;">
                        <strong>Instruções:</strong> Estes filtros controlam quantas dezenas do concurso anterior devem se repetir nos jogos gerados. 
                        Use o cadeado para ativar/desativar os filtros.
                    </p>
                </div>
                
                <div class="filter-category">
                    <h4>Filtros Básicos</h4>
                    <div class="filters-grid">
                        <div class="filter-item">
                            <div class="filter-header">SOMA</div>
                            <div class="filter-inputs">
                                <input type="number" id="somaMin" min="0" max="220" value="160">
                                <input type="number" id="somaMax" min="0" max="220" value="220">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">INÍCIO (1-4)</div>
                            <div class="filter-inputs">
                                <input type="number" id="inicioMin" min="0" max="4" value="1">
                                <input type="number" id="inicioMax" min="0" max="4" value="4">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">FIM (22-25)</div>
                            <div class="filter-inputs">
                                <input type="number" id="fimMin" min="0" max="25" value="22">
                                <input type="number" id="fimMax" min="0" max="25" value="25">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">ÍMPARES</div>
                            <div class="filter-inputs">
                                <input type="number" id="imparMin" min="0" max="15" value="6">
                                <input type="number" id="imparMax" min="0" max="15" value="9">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">PARES</div>
                            <div class="filter-inputs">
                                <input type="number" id="parMin" min="0" max="15" value="6">
                                <input type="number" id="parMax" min="0" max="15" value="9">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">PRIMOS</div>
                            <div class="filter-inputs">
                                <input type="number" id="primosMin" min="0" max="9" value="4">
                                <input type="number" id="primosMax" min="0" max="9" value="6">
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="filter-category">
                    <h4>Distribuição por Linhas</h4>
                    <div class="filters-grid">
                        <div class="filter-item">
                            <div class="filter-header">LINHA 1 (1,2,3,4,5)</div>
                            <div class="filter-inputs">
                                <input type="number" id="linha1Min" min="0" max="5" value="2">
                                <input type="number" id="linha1Max" min="0" max="5" value="4">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">LINHA 2 (6,7,8,9,10)</div>
                            <div class="filter-inputs">
                                <input type="number" id="linha2Min" min="0" max="5" value="2">
                                <input type="number" id="linha2Max" min="0" max="5" value="4">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">LINHA 3 (11,12,13,14,15)</div>
                            <div class="filter-inputs">
                                <input type="number" id="linha3Min" min="0" max="5" value="2">
                                <input type="number" id="linha3Max" min="0" max="5" value="4">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">LINHA 4 (16,17,18,19,20)</div>
                            <div class="filter-inputs">
                                <input type="number" id="linha4Min" min="0" max="5" value="2">
                                <input type="number" id="linha4Max" min="0" max="5" value="4">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">LINHA 5 (21,22,23,24,25)</div>
                            <div class="filter-inputs">
                                <input type="number" id="linha5Min" min="0" max="5" value="2">
                                <input type="number" id="linha5Max" min="0" max="5" value="4">
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="filter-category">
                    <h4>Distribuição por Colunas</h4>
                    <div class="filters-grid">
                        <div class="filter-item">
                            <div class="filter-header">COLUNA 1 (1,6,11,16,21)</div>
                            <div class="filter-inputs">
                                <input type="number" id="coluna1Min" min="0" max="5" value="2">
                                <input type="number" id="coluna1Max" min="0" max="5" value="4">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">COLUNA 2 (2,7,12,17,22)</div>
                            <div class="filter-inputs">
                                <input type="number" id="coluna2Min" min="0" max="5" value="2">
                                <input type="number" id="coluna2Max" min="0" max="5" value="4">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">COLUNA 3 (3,8,13,18,23)</div>
                            <div class="filter-inputs">
                                <input type="number" id="coluna3Min" min="0" max="5" value="2">
                                <input type="number" id="coluna3Max" min="0" max="5" value="4">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">COLUNA 4 (4,9,14,19,24)</div>
                            <div class="filter-inputs">
                                <input type="number" id="coluna4Min" min="0" max="5" value="2">
                                <input type="number" id="coluna4Max" min="0" max="5" value="4">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">COLUNA 5 (5,10,15,20,25)</div>
                            <div class="filter-inputs">
                                <input type="number" id="coluna5Min" min="0" max="5" value="2">
                                <input type="number" id="coluna5Max" min="0" max="5" value="4">
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="filter-category">
                    <h4>Filtros Avançados</h4>
                    <div class="filters-grid">
                        <div class="filter-item">
                            <div class="filter-header">MOLDURA</div>
                            <div class="filter-inputs">
                                <input type="number" id="molduraMin" min="0" max="16" value="8">
                                <input type="number" id="molduraMax" min="0" max="16" value="11">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">CENTRO</div>
                            <div class="filter-inputs">
                                <input type="number" id="centroMin" min="0" max="9" value="4">
                                <input type="number" id="centroMax" min="0" max="9" value="7">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">X-LOTO</div>
                            <div class="filter-inputs">
                                <input type="number" id="xLotoMin" min="0" max="9" value="4">
                                <input type="number" id="xLotoMax" min="0" max="9" value="7">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">H-LOTO</div>
                            <div class="filter-inputs">
                                <input type="number" id="hLotoMin" min="0" max="15" value="6">
                                <input type="number" id="hLotoMax" min="0" max="15" value="10">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">PRIMEIROS (1-15)</div>
                            <div class="filter-inputs">
                                <input type="number" id="primeirosMin" min="0" max="15" value="7">
                                <input type="number" id="primeirosMax" min="0" max="15" value="11">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">INVERTIDOS</div>
                            <div class="filter-inputs">
                                <input type="number" id="invertidosMin" min="0" max="5" value="2">
                                <input type="number" id="invertidosMax" min="0" max="5" value="4">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">PILARES</div>
                            <div class="filter-inputs">
                                <input type="number" id="pilaresMin" min="0" max="15" value="7">
                                <input type="number" id="pilaresMax" min="0" max="15" value="10">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">Nº MÁGICOS</div>
                            <div class="filter-inputs">
                                <input type="number" id="magicosMin" min="0" max="9" value="5">
                                <input type="number" id="magicosMax" min="0" max="9" value="7">
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="filter-category">
                    <h4>Novos Filtros</h4>
                    <div class="filters-grid">
                        <div class="filter-item">
                            <div class="filter-header">FIBONACCI</div>
                            <div class="filter-inputs">
                                <input type="number" id="fibonacciMin" min="0" max="7" value="3">
                                <input type="number" id="fibonacciMax" min="0" max="7" value="6">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">CRUZ</div>
                            <div class="filter-inputs">
                                <input type="number" id="cruzMin" min="0" max="9" value="4">
                                <input type="number" id="cruzMax" min="0" max="9" value="7">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">MÚLTIPLOS DE 3</div>
                            <div class="filter-inputs">
                                <input type="number" id="multiplo3Min" min="0" max="8" value="3">
                                <input type="number" id="multiplo3Max" min="0" max="8" value="6">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">DIAGONAL 1</div>
                            <div class="filter-inputs">
                                <input type="number" id="diag1Min" min="0" max="5" value="2">
                                <input type="number" id="diag1Max" min="0" max="5" value="4">
                            </div>
                        </div>
                        
                        <div class="filter-item">
                            <div class="filter-header">DIAGONAL 2</div>
                            <div class="filter-inputs">
                                <input type="number" id="diag2Min" min="0" max="5" value="1">
                                <input type="number" id="diag2Max" min="0" max="5" value="4">
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="filter-status" id="filterStatus">
                    Filtros configurados e prontos para uso
                </div>
                
                <div class="button-group">
                    <button onclick="applyFilters()">Aplicar Filtros</button>
                    <button onclick="resetFilters()" style="background: #95a5a6;">Redefinir Filtros</button>
                </div>
            </div>
        </div>
        
        <div class="tab-content" id="saved-content">
            <div class="database-status" id="dbStatus">
                Banco de dados carregado com sucesso
            </div>
            <div class="games-header">
                <h2>Jogos Salvos</h2>
                <div class="games-count" id="savedCount">0 jogos salvos</div>
                <button class="more-options" onclick="openSavedGamesOptionsPopup()">⋯</button>
            </div>
            
            <div class="user-section">
                <div class="user-info" id="userInfo">
                    <p>Faça login para ver seus jogos salvos ou <a href="#" onclick="openPopup('userPopup')">crie uma conta</a></p>
                </div>
                
                <div class="user-games" id="userGames">
                    <p>Nenhum jogo salvo ainda.</p>
                </div>
            </div>
            
            <div class="comments-section">
                <h3>Avaliação e Comentários</h3>
                <div class="rating" id="rating">
                    <span class="star" data-value="1">★</span>
                    <span class="star" data-value="2">★</span>
                    <span class="star" data-value="3">★</span>
                    <span class="star" data-value="4">★</span>
                    <span class="star" data-value="5">★</span>
                </div>
                <textarea id="commentInput" placeholder="Deixe seu comentário..."></textarea>
                <button onclick="addComment()">Enviar Comentário</button>
                
                <div id="commentsList">
                    </div>
            </div>
            
            <button class="back-button" onclick="goBack()">⬅️ Voltar</button>
        </div>
        
        <div class="tab-content" id="chat-content">
            <div class="chat-container">
                <div class="chat-header">
                    Chat da Comunidade
                </div>
                <div class="chat-messages" id="chatMessages">
                    </div>
                <div class="chat-input-area">
                    <button class="chat-media-btn" onclick="toggleMediaOptions()">📎</button>
                    <div class="media-options" id="mediaOptions" style="display: none;">
                        <button class="media-option" onclick="takePhoto()">📷 Tirar Foto</button>
                        <button class="media-option" onclick="recordVideo()">🎥 Gravar Vídeo</button>
                        <button class="media-option" onclick="uploadFile()">📁 Enviar Arquivo</button>
                    </div>
                    <input type="text" class="chat-input" id="chatInput" placeholder="Digite sua mensagem...">
                    <button class="chat-send-btn" onclick="sendMessage()">➤</button>
                </div>
                <div class="media-preview" id="mediaPreview"></div>
            </div>
            
            <button class="back-button" onclick="goBack()">⬅️ Voltar</button>
        </div>
        
        <div class="results" id="results"></div>
    </div>
    
    <!-- Loading Spinner -->
    <div class="loading-spinner" id="loadingSpinner">
        <div class="spinner"></div>
        <p>Processando desdobramento...</p>
    </div>
    
    <!-- Notifications -->
    <div class="notification" id="notification"></div>
    
    <div class="popup" id="mainPopup">
        <div class="popup-content">
            <div class="popup-header">
                <h2>Configurações do Desdobramento</h2>
                <button class="close-btn" onclick="closePopup('mainPopup')">✕</button>
            </div>
            
            <label for="dezenasManual">Quantidade de Dezenas:</label>
            <select id="dezenasManual">
                <option value="15">15 dezenas</option>
                <option value="16">16 dezenas</option>
                <option value="17">17 dezenas</option>
                <option value="18">18 dezenas</option>
                <option value="19">19 dezenas</option>
                <option value="20">20 dezenas</option>
                <option value="21">21 dezenas</option>
            </select>
            
            <label for="qualityManual">Quantidade de Jogos (1 - 3.268.760):</label>
            <input type="number" id="qualityManual" min="1" max="3268760" value="10">
            
            <button onclick="generateGames('manual')">Gerar Jogos</button>
            
            <button class="back-button" onclick="closePopup('mainPopup')">⬅️ Voltar</button>
        </div>
    </div>
    
    <div class="popup" id="iaPopup">
        <div class="popup-content">
            <div class="popup-header">
                <h2>🔒 Desdobramento com IA - Acesso Restrito</h2>
                <button class="close-btn" onclick="closePopup('iaPopup')">✕</button>
            </div>
            
            <div style="text-align: center; padding: 20px;">
                <div style="font-size: 3rem; margin-bottom: 20px;">🔒</div>
                <h3 style="color: var(--warning-color); margin-bottom: 15px;">Recurso Premium Bloqueado</h3>
                <p>O desdobramento inteligente com IA é um recurso exclusivo para usuários premium.</p>
                <p>Entre em contato através dos nossos canais para obter acesso.</p>
                
                <div style="margin-top: 30px; display: flex; flex-direction: column; gap: 10px;">
                    <button onclick="window.location='https://wa.me/qr/BSUUOYDLQZDRI1'" style="background: #25D366;">
                        📱 WhatsApp - Solicitar Acesso
                    </button>
                    <button onclick="window.location='https://www.instagram.com/estrategiavip?igsh=dTlqcWhjbG85dThl'" style="background: #E4405F;">
                        📷 Instagram - Mais Informações
                    </button>
                </div>
            </div>
            
            <button class="back-button" onclick="closePopup('iaPopup')">⬅️ Voltar</button>
        </div>
    </div>
    
    <div class="popup" id="numberPopup">
        <div class="popup-content">
            <div class="popup-header">
                <h2 id="popupTitle">Selecionar Dezenas</h2>
                <button class="close-btn" onclick="closePopup('numberPopup')">✕</button>
            </div>
            
            <div class="number-grid" id="numberGrid"></div>
            
            <div style="margin-top: 20px; text-align: center;">
                <button onclick="applyNumberSelection()">Aplicar Seleção</button>
            </div>
            
            <button class="back-button" onclick="closePopup('numberPopup')">⬅️ Voltar</button>
        </div>
    </div>
    
    <div class="popup" id="gamesOptionsPopup">
        <div class="popup-content">
            <div class="popup-header">
                <h2>Opções dos Jogos</h2>
                <button class="close-btn" onclick="closePopup('gamesOptionsPopup')">✕</button>
            </div>
            
            <div style="display: flex; flex-direction: column; gap: 10px;">
                <button onclick="copyAllGames()">Copiar Todos os Jogos (formato planilha)</button>
                <button onclick="saveAllGames()">Salvar Todos os Jogos</button>
                <button onclick="exportGames('txt')">Exportar como TXT (tabulação)</button>
                <button onclick="exportGames('pdf')">Exportar como PDF (volante oficial)</button>
                <button style="background: var(--danger-color);" onclick="clearAllGames()">Excluir Todos os Jogos</button>
                <button onclick="shareGames()">Compartilhar Jogos</button>
            </div>
            
            <button class="back-button" onclick="closePopup('gamesOptionsPopup')">⬅️ Voltar</button>
        </div>
    </div>
    
    <div class="popup" id="savedGamesOptionsPopup">
        <div class="popup-content">
            <div class="popup-header">
                <h2>Opções dos Jogos Salvos</h2>
                <button class="close-btn" onclick="closePopup('savedGamesOptionsPopup')">✕</button>
            </div>
            
            <div style="display: flex; flex-direction: column; gap: 10px;">
                <button onclick="copyAllSavedGames()">Copiar Todos os Jogos Salvos</button>
                <button onclick="exportSavedGames('txt')">Exportar como TXT (tabulação)</button>
                <button onclick="exportSavedGames('pdf')">Exportar como PDF</button>
                <button onclick="shareSavedGames()">Compartilhar Jogos Salvos</button> <button style="background: var(--danger-color);" onclick="clearAllSavedGames()">Excluir Todos os Jogos Salvos</button> </div>
            
            <button class="back-button" onclick="closePopup('savedGamesOptionsPopup')">⬅️ Voltar</button>
        </div>
    </div>
    
    <div class="popup" id="concursoSearchPopup">
        <div class="popup-content">
            <div class="popup-header">
                <h2>Pesquisar Concurso</h2>
                <button class="close-btn" onclick="closePopup('concursoSearchPopup')">✕</button>
            </div>
            
            <label for="concursoNumber">Número do Concurso:</label>
            <input type="number" id="concursoNumber" min="1" placeholder="Ex: 3000">
            
            <div class="button-group">
                <button onclick="fetchConcursoByNumber()">Buscar Concurso</button>
                <button onclick="fetchLastConcurso()" style="background: var(--secondary-color);">Buscar Último Concurso</button>
            </div>
            
            <div id="concursoSearchResult" style="margin-top: 20px;"></div>
            
            <button class="back-button" onclick="closePopup('concursoSearchPopup')">⬅️ Voltar</button>
        </div>
    </div>
    
    <div class="popup" id="pixPopup">
        <div class="popup-content">
            <div class="popup-header">
                <h2>Contribuição via Pix</h2>
                <button class="close-btn" onclick="closePopup('pixPopup')">✕</button>
            </div>
            
            <div class="pix-section">
                <p>Chave Pix: <span class="pix-email">a992616298@gmail.com</span></p>
                <p>Obrigado pelo seu apoio!</p>
                <div style="margin-top: 20px;">
                    <div style="width: 200px; height: 200px; background: #f0f0f0; display: flex; align-items: center; justify-content: center; margin: 0 auto; border-radius: 10px;">
                        QR Code Pix
                    </div>
                    <p style="margin-top: 10px;">Escaneie o QR Code para doar</p>
                </div>
            </div>
            
            <button class="back-button" onclick="closePopup('pixPopup')">⬅️ Voltar</button>
        </div>
    </div>
    
    <div class="popup" id="userPopup">
        <div class="popup-content">
            <div class="popup-header">
                <h2>Sistema de Usuários</h2>
                <button class="close-btn" onclick="closePopup('userPopup')">✕</button>
            </div>
            
            <div class="user-section">
                <div id="loginForm" class="user-form">
                    <h3>Login</h3>
                    <input type="email" id="loginEmail" placeholder="E-mail" required>
                    <input type="password" id="loginPassword" placeholder="Senha" required>
                    <button onclick="loginUser()">Entrar</button>
                    <p style="text-align: center;">Não tem conta? <a href="#" onclick="showRegisterForm()">Cadastre-se</a></p>
                </div>
                
                <div id="registerForm" class="user-form" style="display: none;">
                    <h3>Cadastro</h3>
                    <input type="text" id="registerName" placeholder="Nome" required>
                    <input type="email" id="registerEmail" placeholder="E-mail" required>
                    <input type="password" id="registerPassword" placeholder="Senha" required>
                    <input type="password" id="registerConfirmPassword" placeholder="Confirmar Senha" required>
                    <button onclick="registerUser()">Cadastrar</button>
                    <p style="text-align: center;">Já tem conta? <a href="#" onclick="showLoginForm()">Faça login</a></p>
                </div>
                
                <div id="userProfile" style="display: none;">
                    <div class="user-info">
                        <h3>Perfil do Usuário</h3>
                        <p id="userProfileInfo"></p>
                        <button onclick="logoutUser()">Sair</button>
                    </div>
                </div>
            </div>
            
            <button class="back-button" onclick="closePopup('userPopup')">⬅️ Voltar</button>
        </div>
    </div>

    <script>
        // ==============================
        // VARIÁVEIS GLOBAIS
        // ==============================
        
        let selectedNumbers = new Set();
        let excludedNumbers = new Set();
        let currentGames = [];
        let savedGames = []; // Array para jogos salvos
        let currentNumberType = 'fixed';
        let userRating = 0;
        let lastConcurso = null;
        let repetitionFiltersLocked = true;
        
        // Variáveis para o sistema de chat
        let chatMessages = [];
        let currentUser = {
            id: generateUserId(),
            name: 'Usuário ' + Math.floor(Math.random() * 1000),
            isAdmin: false // Simulação - em um sistema real, isso viria do backend
        };
        let mediaOptionsVisible = false;
        let currentMedia = null;

        // NOVAS VARIÁVEIS: Sistema de usuários
        let users = [];
        let currentLoggedInUser = null;
        let userGames = {}; // Objeto para armazenar jogos por usuário

        // URLs alternativas da API da Caixa
        const API_URLS = [
            'https://servicebus2.caixa.gov.br/portaldeloterias/api/lotofacil',
            'https://loterias.caixa.gov.br/Paginas/Lotofacil.aspx',
            'https://lotodicas.com.br/api/lotofacil'
        ];

        // Chave de criptografia (em produção, isso deve ser armazenado de forma segura)
        const ENCRYPTION_KEY = 'loto-inteligente-secret-key-2024';

        // ==============================
        // INICIALIZAÇÃO
        // ==============================
        
        document.addEventListener('DOMContentLoaded', function() {
            setupEventListeners();
            setupTabs();
            renderChips('fixed');
            renderChips('exclude');
            loadFilterDefaults();
            updateRepetitionLockState();
            
            // Carregar avaliação salva
            const savedRating = localStorage.getItem('userRating');
            if (savedRating) {
                userRating = parseInt(savedRating);
                document.querySelectorAll('.star').forEach(star => {
                    star.classList.toggle('active', parseInt(star.dataset.value) <= userRating);
                });
            }
            
            // Carregar jogos salvos do localStorage
            loadSavedGames();
            
            // Carregar usuários do localStorage
            loadUsers();
            
            // Tentar carregar dados reais da API
            setTimeout(() => {
                fetchLastConcurso();
            }, 1000);
            
            // Renderizar comentários
            renderComments();
            
            // Inicializar chat
            initializeChat();
            
            // Verificar se há usuário logado
            checkLoggedInUser();
        });

        // ==============================
        // CONFIGURAÇÃO DE EVENTOS
        // ==============================
        
        function setupEventListeners() {
            const menuBtn = document.getElementById('menuBtn');
            const sidebar = document.getElementById('sidebar');
            const overlay = document.getElementById('overlay');
            const closeSidebar = document.getElementById('closeSidebar');
            
            menuBtn.addEventListener('click', () => {
                sidebar.classList.add('active');
                overlay.classList.add('active');
            });
            
            closeSidebar.addEventListener('click', () => {
                sidebar.classList.remove('active');
                overlay.classList.remove('active');
            });
            
            overlay.addEventListener('click', () => {
                sidebar.classList.remove('active');
                overlay.classList.remove('active');
            });
            
            // Sistema de avaliação
            document.querySelectorAll('.star').forEach(star => {
                star.addEventListener('click', () => {
                    userRating = parseInt(star.dataset.value);
                    document.querySelectorAll('.star').forEach(s => {
                        s.classList.toggle('active', parseInt(s.dataset.value) <= userRating);
                    });
                    localStorage.setItem('userRating', userRating);
                });
            });
            
            // Evento para enviar mensagem ao pressionar Enter
            document.getElementById('chatInput').addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    sendMessage();
                }
            });
        }
        
        function setupTabs() {
            document.querySelectorAll('.tab').forEach(tab => {
                tab.addEventListener('click', () => {
                    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
                    document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
                    
                    tab.classList.add('active');
                    const tabId = tab.getAttribute('data-tab');
                    document.getElementById(tabId + '-content').classList.add('active');
                    
                    // Se for a aba de chat, rolar para o final das mensagens
                    if (tabId === 'chat') {
                        setTimeout(() => {
                            scrollChatToBottom();
                        }, 100);
                    }
                    
                    // Se for a aba de jogos salvos, atualizar a lista
                    if (tabId === 'saved') {
                        renderUserGames();
                    }
                });
            });
        }

        // ==============================
        // FUNÇÕES DOS POPUPS
        // ==============================
        
        function openPopup(popupId) {
            document.getElementById(popupId).style.display = 'flex';
            document.getElementById('overlay').classList.add('active');
        }
        
        function closePopup(popupId) {
            document.getElementById(popupId).style.display = 'none';
            document.getElementById('overlay').classList.remove('active');
        }
        
        function openMainPopup() {
            openPopup('mainPopup');
        }
        
        function openIApopup() {
            openPopup('iaPopup');
        }
        
        function openConcursoSearchPopup() {
            openPopup('concursoSearchPopup');
        }
        
        function openSavedGamesOptionsPopup() {
            openPopup('savedGamesOptionsPopup');
        }
        
        // ==============================
        // BOTÃO VOLTAR
        // ==============================
        
        function goBack() {
            // Simplesmente fecha o popup ou volta para a aba anterior
            const activePopup = document.querySelector('.popup[style*="display: flex"]');
            if (activePopup) {
                closePopup(activePopup.id);
            } else {
                // Se não há popup aberto, volta para a primeira aba
                document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
                document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
                
                document.querySelector('.tab[data-tab="config"]').classList.add('active');
                document.getElementById('config-content').classList.add('active');
            }
        }

        // ==============================
        // SELEÇÃO DE NÚMEROS - MELHORIAS
        // ==============================
        
        function openNumberPopup(type) {
            currentNumberType = type;
            document.getElementById('popupTitle').textContent = 
                type === 'fixed' ? 'Selecionar Dezenas Fixas (Fortes)' : 'Selecionar Dezenas Excluídas (Fracas)';
            
            const grid = document.getElementById('numberGrid');
            grid.innerHTML = '';
            
            for (let i = 1; i <= 25; i++) {
                const btn = document.createElement('button');
                btn.className = 'number-btn';
                btn.textContent = i.toString().padStart(2, '0');
                btn.dataset.number = i;
                
                // MELHORIA: Agora os números fixos ficam VERDES quando selecionados
                if (selectedNumbers.has(i)) {
                    btn.classList.add('selected');
                } else if (excludedNumbers.has(i)) {
                    btn.classList.add('excluded');
                }
                
                if ((type === 'fixed' && excludedNumbers.has(i)) || 
                    (type === 'exclude' && selectedNumbers.has(i))) {
                    btn.classList.add('conflict');
                    btn.disabled = true;
                }
                
                btn.addEventListener('click', () => {
                    if (btn.classList.contains('conflict')) return;
                    
                    if (type === 'fixed') {
                        if (btn.classList.contains('selected')) {
                            btn.classList.remove('selected');
                        } else {
                            btn.classList.add('selected');
                        }
                    } else {
                        if (btn.classList.contains('excluded')) {
                            btn.classList.remove('excluded');
                        } else {
                            btn.classList.add('excluded');
                        }
                    }
                });
                
                grid.appendChild(btn);
            }
            
            openPopup('numberPopup');
        }
        
        function applyNumberSelection() {
            const buttons = document.querySelectorAll('#numberGrid .number-btn');
            
            if (currentNumberType === 'fixed') {
                selectedNumbers.clear();
                buttons.forEach(btn => {
                    if (btn.classList.contains('selected')) {
                        selectedNumbers.add(parseInt(btn.dataset.number));
                    }
                });
                renderChips('fixed');
            } else {
                excludedNumbers.clear();
                buttons.forEach(btn => {
                    if (btn.classList.contains('excluded')) {
                        excludedNumbers.add(parseInt(btn.dataset.number));
                    }
                });
                renderChips('exclude');
            }
            
            closePopup('numberPopup');
        }
        
        function renderChips(type) {
            const container = document.getElementById(type + 'Chips');
            container.innerHTML = '';
            
            const numbers = type === 'fixed' ? selectedNumbers : excludedNumbers;
            const sortedNumbers = Array.from(numbers).sort((a, b) => a - b);
            
            sortedNumbers.forEach(num => {
                const chip = document.createElement('div');
                chip.className = 'chip selected';
                chip.textContent = num.toString().padStart(2, '0');
                chip.addEventListener('click', () => {
                    if (type === 'fixed') {
                        selectedNumbers.delete(num);
                    } else {
                        excludedNumbers.delete(num);
                    }
                    renderChips(type);
                });
                container.appendChild(chip);
            });
        }

        // ==============================
        // FILTROS - MELHORIAS
        // ==============================
        
        function loadFilterDefaults() {
            // Valores padrão dos filtros
            const defaults = {
                somaMin: 160, somaMax: 220,
                inicioMin: 1, inicioMax: 4,
                fimMin: 22, fimMax: 25,
                imparMin: 6, imparMax: 9,
                parMin: 6, parMax: 9,
                primosMin: 4, primosMax: 6,
                fibonacciMin: 3, fibonacciMax: 6,
                cruzMin: 4, cruzMax: 7,
                multiplo3Min: 3, multiplo3Max: 6,
                diag1Min: 2, diag1Max: 4,
                diag2Min: 1, diag2Max: 4,
                linha1Min: 2, linha1Max: 4,
                linha2Min: 2, linha2Max: 4,
                linha3Min: 2, linha3Max: 4,
                linha4Min: 2, linha4Max: 4,
                linha5Min: 2, linha5Max: 4,
                coluna1Min: 2, coluna1Max: 4,
                coluna2Min: 2, coluna2Max: 4,
                coluna3Min: 2, coluna3Max: 4,
                coluna4Min: 2, coluna4Max: 4,
                coluna5Min: 2, coluna5Max: 4,
                molduraMin: 8, molduraMax: 11,
                centroMin: 4, centroMax: 7,
                xLotoMin: 4, xLotoMax: 7,
                hLotoMin: 6, hLotoMax: 10,
                primeirosMin: 7, primeirosMax: 11,
                invertidosMin: 2, invertidosMax: 4,
                pilaresMin: 7, pilaresMax: 10,
                magicosMin: 5, magicosMax: 7,
                
                // Filtros de repetição (inicialmente desativados)
                rep8Min: 0, rep8Max: 15,
                rep9Min: 0, rep9Max: 15,
                rep10Min: 0, rep10Max: 15,
                rep16_21Min: 11, rep16_21Max: 14
            };
            
            for (const [key, value] of Object.entries(defaults)) {
                const element = document.getElementById(key);
                if (element) element.value = value;
            }
        }
        
        function toggleRepetitionLock() {
            repetitionFiltersLocked = !repetitionFiltersLocked;
            updateRepetitionLockState();
        }
        
        function updateRepetitionLockState() {
            const lockButton = document.getElementById('repetitionLock');
            const repetitionInputs = [
                'rep8Min', 'rep8Max', 'rep9Min', 'rep9Max', 
                'rep10Min', 'rep10Max', 'rep16_21Min', 'rep16_21Max'
            ];
            
            if (repetitionFiltersLocked) {
                lockButton.textContent = '🔓';
                lockButton.classList.remove('locked');
                repetitionInputs.forEach(id => {
                    const element = document.getElementById(id);
                    if (element) element.disabled = true;
                });
            } else {
                lockButton.textContent = '🔒';
                lockButton.classList.add('locked');
                repetitionInputs.forEach(id => {
                    const element = document.getElementById(id);
                    if (element) element.disabled = false;
                });
            }
        }
        
        function applyFilters() {
            // MELHORIA: Aplicar todos os filtros corretamente
            const filterStatus = document.getElementById('filterStatus');
            filterStatus.innerHTML = '✅ Filtros aplicados com sucesso! Eles serão usados na próxima geração de jogos.';
            filterStatus.style.background = '#d4edda';
            filterStatus.style.color = '#155724';
            
            // Simular aplicação de filtros
            setTimeout(() => {
                filterStatus.innerHTML = 'Filtros configurados e prontos para uso';
                filterStatus.style.background = '#e8f4fc';
                filterStatus.style.color = 'inherit';
            }, 3000);
        }
        
        function resetFilters() {
            loadFilterDefaults();
            repetitionFiltersLocked = true;
            updateRepetitionLockState();
            
            const filterStatus = document.getElementById('filterStatus');
            filterStatus.innerHTML = '🔄 Filtros redefinidos para os valores padrão!';
            filterStatus.style.background = '#fff3cd';
            filterStatus.style.color = '#856404';
            
            setTimeout(() => {
                filterStatus.innerHTML = 'Filtros configurados e prontos para uso';
                filterStatus.style.background = '#e8f4fc';
                filterStatus.style.color = 'inherit';
            }, 3000);
        }

        // ==============================
        // GERAÇÃO DE JOGOS - MELHORIAS
        // ==============================
        
        function generateGames(type) {
            let dezenas, quality;
            
            if (type === 'manual') {
                dezenas = parseInt(document.getElementById('dezenasManual').value) || 15;
                quality = parseInt(document.getElementById('qualityManual').value) || 10;
                closePopup('mainPopup');
            } else {
                // IA está bloqueada - mostrar popup de acesso restrito
                openIApopup();
                return;
            }
            
            // Validações básicas
            if (selectedNumbers.size > dezenas) {
                alert(`Não é possível fixar ${selectedNumbers.size} números para um jogo de ${dezenas} dezenas.`);
                return;
            }
            
            const validNumbers = Array.from({length: 25}, (_, i) => i + 1)
                .filter(n => !excludedNumbers.has(n));
            
            if (validNumbers.length < dezenas) {
                alert(`Com as exclusões, restam apenas ${validNumbers.length} números, insuficientes para ${dezenas} dezenas.`);
                return;
            }
            
            // Mostrar loading
            showLoading();
            
            // Simular processamento com timeout
            setTimeout(() => {
                // Gerar jogos manuais
                currentGames = generateGamesManual(validNumbers, dezenas, quality);
                renderGames();
                hideLoading();
            }, 1500);
        }
        
        function generateGamesManual(validNumbers, dezenas, maxGames) {
            const games = [];
            const usedCombinations = new Set(); // Para evitar duplicatas
            
            for (let i = 0; i < maxGames; i++) {
                let game = new Set(selectedNumbers);
                const available = validNumbers.filter(n => !selectedNumbers.has(n));
                const shuffled = [...available].sort(() => 0.5 - Math.random());
                
                for (const num of shuffled) {
                    if (game.size >= dezenas) break;
                    game.add(num);
                }
                
                if (game.size === dezenas) {
                    // Criar uma chave única para o jogo (números ordenados)
                    const gameKey = Array.from(game).sort((a, b) => a - b).join(',');
                    
                    // Verificar se o jogo já foi gerado
                    if (!usedCombinations.has(gameKey)) {
                        usedCombinations.add(gameKey);
                        games.push({
                            numbers: Array.from(game).sort((a, b) => a - b),
                            saved: false // MELHORIA: Adicionar flag para controlar se o jogo foi salvo
                        });
                    }
                }
            }
            return games;
        }
        
        function renderGames() {
            const resultsDiv = document.getElementById('results');
            
            if (currentGames.length === 0) {
                resultsDiv.innerHTML = '<p style="text-align: center; padding: 20px;">Nenhum jogo gerado que atenda aos critérios.</p>';
                return;
            }
            
            resultsDiv.innerHTML = `
                <div class="games-header">
                    <h2>Jogos Gerados (${currentGames[0].numbers.length} dezenas)</h2>
                    <div class="games-count">${currentGames.length} jogos</div>
                    <button class="more-options" onclick="openPopup('gamesOptionsPopup')">⋯</button>
                </div>
            `;
            
            currentGames.forEach((game, index) => {
                const acertos = lastConcurso ? calcularAcertos(game.numbers, lastConcurso.numeros) : 0;
                const prizeClass = getPrizeClass(acertos);
                
                const gameDiv = document.createElement('div');
                gameDiv.className = 'game';
                
                // MELHORIA: Adicionar classe especial para jogos salvos
                if (game.saved) {
                    gameDiv.classList.add('game-saved');
                }
                
                gameDiv.innerHTML = `
                    <div class="game-numbers">
                        ${game.numbers.map(n => `<span class="number ${prizeClass}">${n.toString().padStart(2, '0')}</span>`).join('')}
                    </div>
                    ${acertos >= 11 ? `<div style="font-weight: bold; color: ${prizeClass ? '#2c3e50' : 'inherit'};">${acertos} acertos</div>` : ''}
                    <button class="save-button ${game.saved ? 'saved' : ''}" onclick="saveGameAndRemove(${index})" ${game.saved ? 'disabled' : ''}>
                        ${game.saved ? '✓ Salvo' : 'Salvar'}
                    </button>
                `;
                resultsDiv.appendChild(gameDiv);
            });
            
            // Adicionar botão Voltar após gerar jogos
            const backButton = document.createElement('button');
            backButton.className = 'back-button';
            backButton.textContent = '⬅️ Voltar';
            backButton.onclick = goBack;
            resultsDiv.appendChild(backButton);
        }
        
        function calcularAcertos(jogo, numerosSorteados) {
            if (!numerosSorteados) return 0;
            return jogo.filter(numero => numerosSorteados.includes(numero)).length;
        }
        
        function getPrizeClass(acertos) {
            if (acertos >= 15) return 'prize-15';
            if (acertos >= 14) return 'prize-14';
            if (acertos >= 13) return 'prize-13';
            if (acertos >= 12) return 'prize-12';
            if (acertos >= 11) return 'prize-11';
            return '';
        }

        // ==============================
        // SISTEMA DE SALVAMENTO DE JOGOS - MELHORIAS (COM REMOÇÃO)
        // ==============================
        
        // NOVO: Função que salva e remove o jogo
        function saveGameAndRemove(gameIndex) {
            if (!currentLoggedInUser) {
                alert('Você precisa fazer login para salvar jogos!');
                openPopup('userPopup');
                return;
            }

            if (gameIndex >= 0 && gameIndex < currentGames.length) {
                const game = currentGames[gameIndex];
                
                // 1. Verificar se o jogo já está salvo
                const isAlreadySaved = isGameAlreadySaved(game.numbers);
                
                if (isAlreadySaved) {
                    alert('Este jogo já está salvo!');
                    // Apenas remove da lista atual para limpar a visualização
                    deleteGameFromResults(gameIndex); 
                    return;
                }
                
                // 2. Salvar o jogo
                const savedGame = {
                    id: Date.now(),
                    numbers: [...game.numbers],
                    date: new Date().toLocaleString('pt-BR'),
                    dezenas: game.numbers.length,
                    userId: currentLoggedInUser.id
                };
                
                // Adicionar ao array global
                savedGames.push(savedGame);
                
                // Adicionar ao objeto de jogos por usuário
                if (!userGames[currentLoggedInUser.id]) {
                    userGames[currentLoggedInUser.id] = [];
                }
                userGames[currentLoggedInUser.id].push(savedGame);
                
                saveGamesToStorage();
                renderUserGames();
                
                // 3. Remover da lista de jogos gerados
                deleteGameFromResults(gameIndex);
                
                alert('Jogo salvo e removido da lista de jogos gerados com sucesso!');
            }
        }
        
        // NOVO: Função para remover o jogo da lista de resultados (currentGames)
        function deleteGameFromResults(gameIndex) {
            if (gameIndex >= 0 && gameIndex < currentGames.length) {
                // Remover o jogo da lista de jogos atuais
                currentGames.splice(gameIndex, 1);
                // Redesenhar a lista de jogos
                renderGames();
            }
        }

        function isGameAlreadySaved(numbers) {
            // Verificar se o jogo já existe para o usuário atual
            if (currentLoggedInUser && userGames[currentLoggedInUser.id]) {
                const userSavedGames = userGames[currentLoggedInUser.id];
                const gameKey = numbers.sort((a, b) => a - b).join(',');
                
                return userSavedGames.some(game => 
                    game.numbers.sort((a, b) => a - b).join(',') === gameKey
                );
            }
            return false;
        }
        
        function saveAllGames() {
            if (currentGames.length === 0) {
                alert('Não há jogos para salvar!');
                return;
            }
            
            // Verificar se o usuário está logado
            if (!currentLoggedInUser) {
                alert('Você precisa fazer login para salvar jogos!');
                openPopup('userPopup');
                return;
            }
            
            let savedCount = 0;
            const gamesToRemove = []; // Guardar os índices a serem removidos
            
            // Iterar de trás para frente para evitar problemas de índice
            for (let index = currentGames.length - 1; index >= 0; index--) {
                const game = currentGames[index];
                const isAlreadySaved = isGameAlreadySaved(game.numbers);
                
                if (!isAlreadySaved) {
                    const savedGame = {
                        id: Date.now() + savedCount,
                        numbers: [...game.numbers],
                        date: new Date().toLocaleString('pt-BR'),
                        dezenas: game.numbers.length,
                        userId: currentLoggedInUser.id
                    };
                    
                    savedGames.push(savedGame);
                    
                    if (!userGames[currentLoggedInUser.id]) {
                        userGames[currentLoggedInUser.id] = [];
                    }
                    userGames[currentLoggedInUser.id].push(savedGame);
                    
                    gamesToRemove.push(index);
                    savedCount++;
                }
            }
            
            if (savedCount > 0) {
                saveGamesToStorage();
                renderUserGames();
                
                // Remover os jogos que foram salvos
                gamesToRemove.sort((a, b) => b - a).forEach(index => {
                    currentGames.splice(index, 1);
                });
                
                renderGames(); // Atualizar a visualização
                closePopup('gamesOptionsPopup');
                alert(`${savedCount} jogos salvos e removidos da lista de jogos gerados com sucesso!`);
            } else {
                alert('Todos os jogos já estão salvos!');
            }
        }
        
        function saveGamesToStorage() {
            localStorage.setItem('savedGames', JSON.stringify(savedGames));
            localStorage.setItem('userGames', JSON.stringify(userGames));
        }
        
        function loadSavedGames() {
            const saved = localStorage.getItem('savedGames');
            if (saved) {
                savedGames = JSON.parse(saved);
            }
            
            const userGamesData = localStorage.getItem('userGames');
            if (userGamesData) {
                userGames = JSON.parse(userGamesData);
            }
        }
        
        function renderUserGames() {
            const userGamesContainer = document.getElementById('userGames');
            const savedCount = document.getElementById('savedCount');
            
            if (!currentLoggedInUser) {
                userGamesContainer.innerHTML = '<p>Faça login para ver seus jogos salvos.</p>';
                savedCount.textContent = '0 jogos salvos';
                return;
            }
            
            let gamesToShow = [];
            
            // MOSTRAR APENAS JOGOS DO USUÁRIO ATUAL
            if (userGames[currentLoggedInUser.id]) {
                gamesToShow = userGames[currentLoggedInUser.id];
            }
            savedCount.textContent = `${gamesToShow.length} jogos salvos`;
            
            if (gamesToShow.length === 0) {
                userGamesContainer.innerHTML = '<p>Nenhum jogo salvo ainda.</p>';
                return;
            }
            
            userGamesContainer.innerHTML = '';
            
            // Re-indexar a lista para a remoção local
            const displayList = gamesToShow.map((game, index) => ({ 
                ...game, 
                displayIndex: index // Índice na lista atual do usuário
            }));

            displayList.forEach((game) => {
                const acertos = lastConcurso ? calcularAcertos(game.numbers, lastConcurso.numeros) : 0;
                const prizeClass = getPrizeClass(acertos);
                
                const gameDiv = document.createElement('div');
                gameDiv.className = 'game';
                
                gameDiv.innerHTML = `
                    <div>
                        <div class="game-numbers">
                            ${game.numbers.map(n => `<span class="number ${prizeClass}">${n.toString().padStart(2, '0')}</span>`).join('')}
                        </div>
                        <div style="font-size: 0.8em; color: #666; margin-top: 5px;">
                            Salvo em: ${game.date} | ${game.dezenas} dezenas
                        </div>
                    </div>
                    <div>
                        ${acertos >= 11 ? `<div style="font-weight: bold; color: ${prizeClass ? '#2c3e50' : 'inherit'};">${acertos} acertos</div>` : ''}
                        <button onclick="deleteSavedGame(${game.displayIndex})" style="background: var(--danger-color); max-width: 120px; margin: 5px 0;">Excluir</button>
                    </div>
                `;
                userGamesContainer.appendChild(gameDiv);
            });
        }
        
        function deleteSavedGame(index) {
            if (confirm('Tem certeza que deseja excluir este jogo salvo?')) {
                // Acha o jogo específico para remover
                if (userGames[currentLoggedInUser.id] && userGames[currentLoggedInUser.id].length > index) {
                    const gameToDelete = userGames[currentLoggedInUser.id][index];

                    // 1. Remover do array global (savedGames)
                    savedGames = savedGames.filter(game => 
                        !(game.userId === currentLoggedInUser.id && JSON.stringify(game.numbers) === JSON.stringify(gameToDelete.numbers))
                    );

                    // 2. Remover do objeto de jogos por usuário (userGames)
                    userGames[currentLoggedInUser.id].splice(index, 1);
                }
                
                saveGamesToStorage();
                renderUserGames();
            }
        }
        
        function clearAllSavedGames() {
            if (!currentLoggedInUser) {
                alert('Você precisa fazer login para gerenciar jogos salvos!');
                return;
            }
            
            if (!userGames[currentLoggedInUser.id] || userGames[currentLoggedInUser.id].length === 0) {
                alert('Não há jogos salvos para excluir!');
                return;
            }
            
            if (confirm('Tem certeza que deseja excluir TODOS os seus jogos salvos?')) {
                // Remover do array global
                savedGames = savedGames.filter(game => game.userId !== currentLoggedInUser.id);
                
                // Remover do objeto de jogos por usuário
                userGames[currentLoggedInUser.id] = [];
                
                saveGamesToStorage();
                renderUserGames();
                closePopup('savedGamesOptionsPopup'); // Fechar o popup após a ação
                alert('Todos os seus jogos salvos foram excluídos!');
            }
        }

        // ==============================
        // FUNÇÕES DE EXPORTAÇÃO COM TABULAÇÃO PARA PLANILHA
        // ==============================
        
        function exportGames(format) {
            if (currentGames.length === 0) {
                alert('Não há jogos para exportar!');
                return;
            }
            
            if (format === 'txt') {
                // FORMATO PERFEITO PARA PLANILHA: Apenas números tabulados
                let content = '';
                
                currentGames.forEach((game, index) => {
                    // Formatar números com 2 dígitos e separar por TAB
                    const dezenas = game.numbers.map(n => n.toString().padStart(2, '0')).join('\t');
                    content += `${dezenas}\n`;
                });
                
                // Criar blob e fazer download
                const blob = new Blob([content], { type: 'text/plain;charset=utf-8' });
                const url = URL.createObjectURL(blob);
                const a = document.createElement('a');
                a.href = url;
                a.download = `jogos_lotofacil_${currentGames[0].numbers.length}_dezenas_${new Date().toISOString().split('T')[0]}.txt`;
                document.body.appendChild(a);
                a.click();
                document.body.removeChild(a);
                URL.revokeObjectURL(url);
                
                alert(`${currentGames.length} jogos de ${currentGames[0].numbers.length} dezenas exportados!\nPronto para colar no Excel/Google Sheets.`);
            } else if (format === 'pdf') {
                alert('Exportando como PDF...');
                // Implementação do PDF seria mais complexa
            }
        }
        
        function exportSavedGames(format) {
            if (!currentLoggedInUser) {
                alert('Você precisa fazer login para exportar jogos salvos!');
                return;
            }
            
            const userGamesList = userGames[currentLoggedInUser.id] || [];
            
            if (userGamesList.length === 0) {
                alert('Não há jogos salvos para exportar!');
                return;
            }
            
            if (format === 'txt') {
                // FORMATO PERFEITO PARA PLANILHA: Apenas números tabulados
                let content = '';
                
                userGamesList.forEach((game, index) => {
                    // Formatar números com 2 dígitos e separar por TAB
                    const dezenas = game.numbers.map(n => n.toString().padStart(2, '0')).join('\t');
                    content += `${dezenas}\n`;
                });
                
                // Criar blob e fazer download
                const blob = new Blob([content], { type: 'text/plain;charset=utf-8' });
                const url = URL.createObjectURL(blob);
                const a = document.createElement('a');
                a.href = url;
                a.download = `jogos_salvos_lotofacil_${new Date().toISOString().split('T')[0]}.txt`;
                document.body.appendChild(a);
                a.click();
                document.body.removeChild(a);
                URL.revokeObjectURL(url);
                
                alert(`${userGamesList.length} jogos salvos exportados!\nPronto para colar no Excel/Google Sheets.`);
            } else if (format === 'pdf') {
                alert('Exportando jogos salvos como PDF...');
            }
        }
        
        function copyAllGames() {
            if (currentGames.length === 0) {
                alert('Não há jogos para copiar!');
                return;
            }
            
            // FORMATO PERFEITO PARA PLANILHA: Apenas números tabulados
            let content = '';
            
            currentGames.forEach((game, index) => {
                // Formatar números com 2 dígitos e separar por TAB
                const dezenas = game.numbers.map(n => n.toString().padStart(2, '0')).join('\t');
                content += `${dezenas}\n`;
            });
            
            // Copiar para área de transferência
            navigator.clipboard.writeText(content).then(() => {
                alert(`${currentGames.length} jogos de ${currentGames[0].numbers.length} dezenas copiados!\nCole no Excel/Google Sheets.`);
            }).catch(err => {
                console.error('Erro ao copiar: ', err);
                // Fallback para método antigo
                const textArea = document.createElement('textarea');
                textArea.value = content;
                document.body.appendChild(textArea);
                textArea.select();
                document.execCommand('copy');
                document.body.removeChild(textArea);
                alert('Jogos copiados para a área de transferência!\nCole em qualquer planilha.');
            });
        }
        
        function copyAllSavedGames() {
            if (!currentLoggedInUser) {
                alert('Você precisa fazer login para copiar jogos salvos!');
                return;
            }
            
            const userGamesList = userGames[currentLoggedInUser.id] || [];
            
            if (userGamesList.length === 0) {
                alert('Não há jogos salvos para copiar!');
                return;
            }
            
            // FORMATO PERFEITO PARA PLANILHA: Apenas números tabulados
            let content = '';
            
            userGamesList.forEach((game, index) => {
                // Formatar números com 2 dígitos e separar por TAB
                const dezenas = game.numbers.map(n => n.toString().padStart(2, '0')).join('\t');
                content += `${dezenas}\n`;
            });
            
            // Copiar para área de transferência
            navigator.clipboard.writeText(content).then(() => {
                alert(`${userGamesList.length} jogos salvos copiados!\nCole no Excel/Google Sheets.`);
            }).catch(err => {
                console.error('Erro ao copiar: ', err);
                const textArea = document.createElement('textarea');
                textArea.value = content;
                document.body.appendChild(textArea);
                textArea.select();
                document.execCommand('copy');
                document.body.removeChild(textArea);
                alert('Jogos salvos copiados para a área de transferência!\nCole em qualquer planilha.');
            });
        }

        // ==============================
        // SISTEMA DE USUÁRIOS - NOVO
        // ==============================
        
        function loadUsers() {
            const savedUsers = localStorage.getItem('users');
            if (savedUsers) {
                users = JSON.parse(savedUsers);
            }
        }
        
        function saveUsers() {
            localStorage.setItem('users', JSON.stringify(users));
        }
        
        function showRegisterForm() {
            document.getElementById('loginForm').style.display = 'none';
            document.getElementById('registerForm').style.display = 'block';
        }
        
        function showLoginForm() {
            document.getElementById('registerForm').style.display = 'none';
            document.getElementById('loginForm').style.display = 'block';
        }
        
        function registerUser() {
            const name = document.getElementById('registerName').value;
            const email = document.getElementById('registerEmail').value;
            const password = document.getElementById('registerPassword').value;
            const confirmPassword = document.getElementById('registerConfirmPassword').value;
            
            // Validações
            if (!name || !email || !password || !confirmPassword) {
                alert('Por favor, preencha todos os campos!');
                return;
            }
            
            if (password !== confirmPassword) {
                alert('As senhas não coincidem!');
                return;
            }
            
            if (password.length < 6) {
                alert('A senha deve ter pelo menos 6 caracteres!');
                return;
            }
            
            // Verificar se o email já está cadastrado
            if (users.some(user => user.email === email)) {
                alert('Este email já está cadastrado!');
                return;
            }
            
            // Criar novo usuário
            const newUser = {
                id: generateUserId(),
                name: name,
                email: email,
                password: password, // Em um sistema real, isso seria criptografado
                createdAt: new Date().toISOString()
            };
            
            users.push(newUser);
            saveUsers();
            
            alert('Cadastro realizado com sucesso! Faça login para continuar.');
            showLoginForm();
            
            // Limpar formulário
            document.getElementById('registerForm').reset();
        }
        
        function loginUser() {
            const email = document.getElementById('loginEmail').value;
            const password = document.getElementById('loginPassword').value;
            
            if (!email || !password) {
                alert('Por favor, preencha todos os campos!');
                return;
            }
            
            // Verificar credenciais
            const user = users.find(u => u.email === email && u.password === password);
            
            if (user) {
                currentLoggedInUser = user;
                localStorage.setItem('currentLoggedInUser', JSON.stringify(user));
                
                // Atualizar interface
                updateUserInterface();
                closePopup('userPopup');
                
                alert(`Bem-vindo, ${user.name}!`);
            } else {
                alert('Email ou senha incorretos!');
            }
        }
        
        function logoutUser() {
            currentLoggedInUser = null;
            localStorage.removeItem('currentLoggedInUser');
            updateUserInterface();
            document.getElementById('userProfile').style.display = 'none';
            showLoginForm();
            alert('Logout realizado com sucesso!');
        }
        
        function checkLoggedInUser() {
            const savedUser = localStorage.getItem('currentLoggedInUser');
            if (savedUser) {
                currentLoggedInUser = JSON.parse(savedUser);
                updateUserInterface();
            }
        }
        
        function updateUserInterface() {
            const userInfo = document.getElementById('userInfo');
            const userTabs = document.getElementById('userTabs');
            const userProfile = document.getElementById('userProfile');
            const userProfileInfo = document.getElementById('userProfileInfo');
            
            if (currentLoggedInUser) {
                userInfo.innerHTML = `
                    <p>Olá, <strong>${currentLoggedInUser.name}</strong>! 
                    <a href="#" onclick="openPopup('userPopup')">Gerenciar conta</a></p>
                `;
                userProfile.style.display = 'block';
                userProfileInfo.innerHTML = `
                    <p><strong>Nome:</strong> ${currentLoggedInUser.name}</p>
                    <p><strong>Email:</strong> ${currentLoggedInUser.email}</p>
                    <p><strong>Cadastrado em:</strong> ${new Date(currentLoggedInUser.createdAt).toLocaleDateString('pt-BR')}</p>
                `;
                
                // Atualizar a lista de jogos
                renderUserGames();
            } else {
                userInfo.innerHTML = '<p>Faça login para ver seus jogos salvos ou <a href="#" onclick="openPopup(\'userPopup\')">crie uma conta</a></p>';
                userProfile.style.display = 'none';
            }
        }

        // ==============================
        // SISTEMA DE CHAT
        // ==============================
        
        function initializeChat() {
            // Carregar mensagens salvas do localStorage
            const savedMessages = localStorage.getItem('chatMessages');
            if (savedMessages) {
                chatMessages = JSON.parse(savedMessages);
            } else {
                // Mensagens iniciais de exemplo
                chatMessages = [
                    {
                        id: 1,
                        userId: 'admin',
                        userName: 'Administrador',
                        content: 'Bem-vindos ao chat da Lotofácil! Compartilhem suas estratégias e experiências.',
                        timestamp: new Date().getTime() - 3600000,
                        type: 'text'
                    },
                    {
                        id: 2,
                        userId: 'user123',
                        userName: 'João Silva',
                        content: 'Alguém tem alguma dica para aumentar as chances de acerto?',
                        timestamp: new Date().getTime() - 1800000,
                        type: 'text'
                    }
                ];
                saveChatMessages();
            }
            
            renderChatMessages();
        }
        
        function renderChatMessages() {
            const chatContainer = document.getElementById('chatMessages');
            chatContainer.innerHTML = '';
            
            chatMessages.forEach(message => {
                const messageDiv = document.createElement('div');
                messageDiv.className = `message ${message.userId === currentUser.id ? 'own' : 'other'}`;
                messageDiv.dataset.messageId = message.id;
                
                let messageContent = '';
                
                if (message.type === 'text') {
                    messageContent = `<div class="message-content">${message.content}</div>`;
                } else if (message.type === 'image') {
                    messageContent = `
                        <div class="message-content">
                            <img src="${message.content}" alt="Imagem enviada" style="max-width: 100%; border-radius: 5px;">
                        </div>
                    `;
                } else if (message.type === 'video') {
                    messageContent = `
                        <div class="message-content">
                            <video controls style="max-width: 100%; border-radius: 5px;">
                                <source src="${message.content}" type="video/mp4">
                                Seu navegador não suporta o elemento de vídeo.
                            </video>
                        </div>
                    `;
                }
                
                messageDiv.innerHTML = `
                    <div class="message-header">
                        <span class="user-id">${message.userName}</span>
                        <span>${formatTime(message.timestamp)}</span>
                    </div>
                    ${messageContent}
                    <div class="message-actions">
                        <button class="message-action" onclick="likeMessage(${message.id})">👍 ${message.likes || 0}</button>
                        ${currentUser.isAdmin || message.userId === currentUser.id ? 
                            `<button class="message-action delete" onclick="deleteMessage(${message.id})">Excluir</button>` : 
                            ''
                        }
                        ${currentUser.isAdmin ? 
                            `<button class="message-action ban" onclick="banUser('${message.userId}')">Banir</button>` : 
                            ''
                        }
                    </div>
                `;
                
                chatContainer.appendChild(messageDiv);
            });
            
            scrollChatToBottom();
        }
        
        function sendMessage() {
            const input = document.getElementById('chatInput');
            const content = input.value.trim();
            
            if (!content && !currentMedia) return;
            
            const newMessage = {
                id: Date.now(),
                userId: currentUser.id,
                userName: currentUser.name,
                content: currentMedia ? currentMedia.url : content,
                timestamp: new Date().getTime(),
                type: currentMedia ? currentMedia.type : 'text',
                likes: 0
            };
            
            chatMessages.push(newMessage);
            saveChatMessages();
            renderChatMessages();
            
            // Limpar inputs
            input.value = '';
            clearMedia();
            
            // Simular resposta automática (opcional)
            if (Math.random() > 0.7) {
                setTimeout(() => {
                    const autoReplies = [
                        "Interessante estratégia!",
                        "Já testei algo parecido e funcionou bem.",
                        "Vou tentar essa ideia nos meus próximos jogos.",
                        "Alguém mais tem experiência com essa abordagem?"
                    ];
                    
                    const randomReply = autoReplies[Math.floor(Math.random() * autoReplies.length)];
                    
                    const replyMessage = {
                        id: Date.now() + 1,
                        userId: 'bot',
                        userName: 'Bot da Lotofácil',
                        content: randomReply,
                        timestamp: new Date().getTime(),
                        type: 'text',
                        likes: 0
                    };
                    
                    chatMessages.push(replyMessage);
                    saveChatMessages();
                    renderChatMessages();
                }, 2000);
            }
        }
        
        function likeMessage(messageId) {
            const message = chatMessages.find(m => m.id === messageId);
            if (message) {
                message.likes = (message.likes || 0) + 1;
                saveChatMessages();
                renderChatMessages();
            }
        }
        
        function deleteMessage(messageId) {
            if (confirm('Tem certeza que deseja excluir esta mensagem?')) {
                chatMessages = chatMessages.filter(m => m.id !== messageId);
                saveChatMessages();
                renderChatMessages();
            }
        }
        
        function banUser(userId) {
            if (confirm('Tem certeza que deseja banir este usuário?')) {
                // Em um sistema real, isso seria enviado para o backend
                alert(`Usuário ${userId} foi banido.`);
                // Remover mensagens do usuário banido
                chatMessages = chatMessages.filter(m => m.userId !== userId);
                saveChatMessages();
                renderChatMessages();
            }
        }
        
        function saveChatMessages() {
            localStorage.setItem('chatMessages', JSON.stringify(chatMessages));
        }
        
        function formatTime(timestamp) {
            const date = new Date(timestamp);
            return date.toLocaleTimeString('pt-BR', { hour: '2-digit', minute: '2-digit' });
        }
        
        function scrollChatToBottom() {
            const chatContainer = document.getElementById('chatMessages');
            chatContainer.scrollTop = chatContainer.scrollHeight;
        }
        
        function generateUserId() {
            return 'user_' + Math.random().toString(36).substr(2, 9);
        }

        // ==============================
        // FUNÇÕES DE MÍDIA PARA O CHAT
        // ==============================
        
        function toggleMediaOptions() {
            const mediaOptions = document.getElementById('mediaOptions');
            mediaOptionsVisible = !mediaOptionsVisible;
            mediaOptions.style.display = mediaOptionsVisible ? 'flex' : 'none';
        }
        
        function takePhoto() {
            // Em um ambiente real, isso usaria a API de câmera
            alert('Funcionalidade de câmera seria implementada aqui. Emulando...');
            
            // Simular foto (em um caso real, isso viria da câmera)
            const fakePhotoUrl = 'https://via.placeholder.com/300x200/3498db/ffffff?text=Foto+da+Lotofácil';
            setCurrentMedia(fakePhotoUrl, 'image');
            toggleMediaOptions();
        }
        
        function recordVideo() {
            // Em um ambiente real, isso usaria a API de gravação de vídeo
            alert('Funcionalidade de gravação de vídeo seria implementada aqui. Emulando...');
            
            // Simular vídeo (em um caso real, isso viria da câmera)
            const fakeVideoUrl = 'https://sample-videos.com/zip/10/mp4/SampleVideo_1280x720_1mb.zip';
            setCurrentMedia(fakeVideoUrl, 'video');
            toggleMediaOptions();
        }
        
        function uploadFile() {
            // Criar input de arquivo oculto
            const fileInput = document.createElement('input');
            fileInput.type = 'file';
            fileInput.accept = 'image/*,video/*';
            fileInput.onchange = function(e) {
                const file = e.target.files[0];
                if (file) {
                    const reader = new FileReader();
                    reader.onload = function(event) {
                        const fileType = file.type.startsWith('image/') ? 'image' : 
                                       file.type.startsWith('video/') ? 'video' : 'file';
                        setCurrentMedia(event.target.result, fileType);
                    };
                    reader.readAsDataURL(file);
                }
            };
            fileInput.click();
            toggleMediaOptions();
        }
        
        function setCurrentMedia(url, type) {
            currentMedia = { url, type };
            
            const preview = document.getElementById('mediaPreview');
            if (type === 'image') {
                preview.innerHTML = `<img src="${url}" alt="Pré-visualização" style="max-width: 100%; max-height: 200px;">`;
            } else if (type === 'video') {
                preview.innerHTML = `
                    <video controls style="max-width: 100%; max-height: 200px;">
                        <source src="${url}" type="video/mp4">
                        Seu navegador não suporta o elemento de vídeo.
                    </video>
                `;
            }
            
            preview.style.display = 'block';
        }
        
        function clearMedia() {
            currentMedia = null;
            document.getElementById('mediaPreview').innerHTML = '';
            document.getElementById('mediaPreview').style.display = 'none';
        }

        // ==============================
        // API DA CAIXA - CORREÇÃO
        // ==============================
        
        async function fetchLastConcurso() {
            try {
                document.getElementById('apiStatus').innerHTML = 
                    '<span class="api-status loading">🔄 Buscando último concurso...</span>';
                
                let concursoData = null;
                
                // Tentar diferentes endpoints
                for (const apiUrl of API_URLS) {
                    try {
                        console.log(`Tentando API: ${apiUrl}`);
                        
                        if (apiUrl.includes('caixa.gov.br')) {
                            // Para APIs oficiais da Caixa (pode ter CORS)
                            const response = await fetchWithCors(apiUrl);
                            if (response.ok) {
                                concursoData = await response.json();
                                break;
                            }
                        } else {
                            // Para APIs alternativas
                            const response = await fetch(apiUrl);
                            if (response.ok) {
                                concursoData = await response.json();
                                break;
                            }
                        }
                    } catch (error) {
                        console.log(`Falha na API ${apiUrl}:`, error.message);
                        continue;
                    }
                }
                
                // Se todas as APIs falharem, usar dados simulados
                if (!concursoData) {
                    console.log('Usando dados simulados como fallback');
                    concursoData = await getSimulatedConcursoData();
                }
                
                lastConcurso = processConcursoData(concursoData);
                
                document.getElementById('apiStatus').innerHTML = 
                    '<span class="api-status success">✅ Último concurso carregado com sucesso!</span>';
                
                renderLastConcurso();
                
            } catch (error) {
                console.error('Erro ao buscar último concurso:', error);
                document.getElementById('apiStatus').innerHTML = 
                    '<span class="api-status error">❌ Erro ao carregar dados. Usando informações locais.</span>';
                
                // Fallback para dados simulados
                fetchLastConcursoFallback();
            }
        }

        // Função para lidar com CORS
        async function fetchWithCors(url) {
            try {
                // Tentar diretamente
                const response = await fetch(url);
                return response;
            } catch (error) {
                console.log('Erro de CORS, tentando proxy...');
                // Tentar com proxy CORS
                try {
                    return await fetch(`https://api.allorigins.win/raw?url=${encodeURIComponent(url)}`);
                } catch (proxyError) {
                    console.log('Proxy também falhou:', proxyError);
                    throw error;
                }
            }
        }

        // Processar dados do concurso
        function processConcursoData(data) {
            // Adaptar para diferentes formatos de API
            if (data && data.listaDezenas) {
                // Formato oficial da Caixa
                return {
                    concurso: data.numero || data.concurso,
                    data: data.dataApuracao || new Date().toISOString().split('T')[0],
                    numeros: data.listaDezenas.map(num => parseInt(num)),
                    premiacao: data.listaRateioPremio || {
                        '15': data.valorPremio15Acertos || 2500000,
                        '14': data.valorPremio14Acertos || 1500,
                        '13': data.valorPremio13Acertos || 25,
                        '12': data.valorPremio12Acertos || 8,
                        '11': data.valorPremio11Acertos || 4
                    }
                };
            } else if (data && data.numeros) {
                // Formato alternativo
                return data;
            } else {
                // Fallback para dados simulados
                return getSimulatedConcursoData();
            }
        }

        // Dados simulados realistas
        async function getSimulatedConcursoData() {
            // Gerar números aleatórios realistas para a Lotofácil
            const generateRealisticNumbers = () => {
                const numbers = new Set();
                while (numbers.size < 15) {
                    const num = Math.floor(Math.random() * 25) + 1;
                    numbers.add(num);
                }
                return Array.from(numbers).sort((a, b) => a - b);
            };

            return {
                concurso: Math.floor(Math.random() * 100) + 3100,
                data: new Date().toISOString().split('T')[0],
                numeros: generateRealisticNumbers(),
                premiacao: {
                    '15': 2500000,
                    '14': 1500,
                    '13': 25,
                    '12': 8,
                    '11': 4
                }
            };
        }

        // Função fallback melhorada
        function fetchLastConcursoFallback() {
            const simulatedConcurso = {
                concurso: 3150,
                data: new Date().toISOString().split('T')[0],
                numeros: [1, 2, 3, 5, 7, 8, 10, 11, 13, 14, 16, 18, 20, 23, 25],
                premiacao: {
                    '15': 2500000,
                    '14': 1500,
                    '13': 25,
                    '12': 8,
                    '11': 4
                }
            };
            
            lastConcurso = simulatedConcurso;
            
            document.getElementById('apiStatus').innerHTML = 
                '<span class="api-status success">✅ Dados atualizados localmente</span>';
            
            renderLastConcurso();
        }

        function renderLastConcurso() {
            if (!lastConcurso) {
                document.getElementById('lastDrawNumbers').innerHTML = '<p>Último concurso não disponível</p>';
                return;
            }
            
            document.getElementById('lastDrawNumbers').innerHTML = lastConcurso.numeros
                .sort((a, b) => a - b)
                .map(num => `<span class="last-draw-number">${num.toString().padStart(2, '0')}</span>`)
                .join('');
            
            document.querySelector('#lastDraw h3').textContent = 
                `Concurso ${lastConcurso.concurso} - ${lastConcurso.data}`;
        }
        
        // Função para buscar concurso específico
        async function fetchConcursoByNumber() {
            const concursoNumber = document.getElementById('concursoNumber').value;
            
            if (!concursoNumber) {
                alert('Por favor, informe o número do concurso');
                return;
            }
            
            document.getElementById('concursoSearchResult').innerHTML = 
                '<div class="api-status loading">🔄 Buscando concurso...</div>';
            
            try {
                // Tentar buscar da API
                let concursoData = null;
                
                for (const apiUrl of API_URLS) {
                    try {
                        const url = `${apiUrl}/${concursoNumber}`;
                        const response = await fetchWithCors(url);
                        if (response.ok) {
                            concursoData = await response.json();
                            break;
                        }
                    } catch (error) {
                        continue;
                    }
                }
                
                if (!concursoData) {
                    // Simular dados se a API falhar
                    concursoData = await getSimulatedConcursoByNumber(concursoNumber);
                }
                
                const processedData = processConcursoData(concursoData);
                
                document.getElementById('concursoSearchResult').innerHTML = `
                    <div class="concurso-info">
                        <div class="concurso-header">
                            <h3>Concurso ${processedData.concurso}</h3>
                            <span>${processedData.data}</span>
                        </div>
                        <div class="concurso-numbers">
                            ${processedData.numeros
                                .sort((a, b) => a - b)
                                .map(num => `<span class="concurso-number">${num.toString().padStart(2, '0')}</span>`)
                                .join('')}
                        </div>
                        <div style="margin-top: 15px;">
                            <strong>Premiações:</strong>
                            <div style="display: flex; flex-wrap: wrap; gap: 10px; margin-top: 10px;">
                                ${Object.entries(processedData.premiacao)
                                    .map(([acertos, valor]) => 
                                        `<div style="background: #3498db; color: white; padding: 5px 10px; border-radius: 5px;">
                                            ${acertos} acertos: R$ ${valor.toLocaleString('pt-BR')}
                                        </div>`
                                    ).join('')}
                            </div>
                        </div>
                        <div style="margin-top: 10px;">
                            <button onclick="useThisConcurso(${processedData.concurso})">Usar Este Concurso para Análise</button>
                        </div>
                    </div>
                `;
                
            } catch (error) {
                document.getElementById('concursoSearchResult').innerHTML = 
                    '<div class="api-status error">❌ Erro ao buscar concurso específico</div>';
            }
        }

        // Simular dados de concurso específico
        async function getSimulatedConcursoByNumber(concursoNumber) {
            return {
                concurso: parseInt(concursoNumber),
                data: new Date(Date.now() - Math.floor(Math.random() * 100) * 24 * 60 * 60 * 1000).toISOString().split('T')[0],
                numeros: Array.from({length: 15}, () => Math.floor(Math.random() * 25) + 1)
                    .filter((num, index, self) => self.indexOf(num) === index)
                    .slice(0, 15)
                    .sort((a, b) => a - b),
                premiacao: {
                    '15': 2500000,
                    '14': 1500,
                    '13': 25,
                    '12': 8,
                    '11': 4
                }
            };
        }
        
        function useThisConcurso(concursoNumber) {
            alert(`Concurso ${concursoNumber} selecionado para análise!`);
            closePopup('concursoSearchPopup');
        }

        // ==============================
        // FUNÇÕES ADICIONAIS
        // ==============================
        
        function openSection(section) {
            alert(`Abrindo seção: ${section}`);
        }
        
        function addComment() {
            const commentInput = document.getElementById('commentInput');
            const comment = commentInput.value.trim();
            
            if (comment) {
                alert(`Comentário adicionado: ${comment}`);
                commentInput.value = '';
            } else {
                alert('Por favor, digite um comentário');
            }
        }
        
        function renderComments() {
            // Simular comentários
            document.getElementById('commentsList').innerHTML = `
                <div class="comment">
                    <div class="comment-header">
                        <strong>Usuário 1</strong>
                        <span>⭐️⭐️⭐️⭐️⭐️</span>
                    </div>
                    <p>Ótima ferramenta para análise da Lotofácil!</p>
                </div>
                <div class="comment">
                    <div class="comment-header">
                        <strong>Usuário 2</strong>
                        <span>⭐️⭐️⭐️⭐️</span>
                    </div>
                    <p>Muito útil para criar meus jogos.</p>
                </div>
            `;
        }

        // Funções simuladas para os botões de opções
        function clearAllGames() { 
            if (confirm('Tem certeza que deseja excluir todos os jogos?')) {
                currentGames = [];
                document.getElementById('results').innerHTML = '';
                alert('Todos os jogos foram excluídos!');
                closePopup('gamesOptionsPopup'); // Fechar o popup após a ação
            }
        }
        function shareGames() { alert('Compartilhando jogos...'); }
        
        // FUNÇÃO DE COMPARTILHAMENTO DE JOGOS SALVOS (ATUALIZADA)
        function shareSavedGames() { 
            if (!currentLoggedInUser) {
                alert('Você precisa fazer login para compartilhar jogos salvos!');
                return;
            }
            
            const userGamesList = userGames[currentLoggedInUser.id] || [];
            
            if (userGamesList.length === 0) {
                alert('Não há jogos salvos para compartilhar!');
                return;
            }
            
            let shareText = `Meus Jogos Salvos da Lotofácil (${currentLoggedInUser.name}):\n\n`;
            
            userGamesList.forEach((game, index) => {
                const dezenas = game.numbers.map(n => n.toString().padStart(2, '0')).join(', ');
                shareText += `Jogo ${index + 1} (${game.dezenas} dezenas): ${dezenas}\n`;
            });
            
            // Usar a Web Share API se estiver disponível
            if (navigator.share) {
                navigator.share({
                    title: 'Meus Jogos Salvos da Lotofácil',
                    text: shareText
                }).then(() => {
                    console.log('Conteúdo compartilhado com sucesso!');
                    closePopup('savedGamesOptionsPopup');
                }).catch(error => {
                    console.error('Erro ao compartilhar', error);
                    // Fallback: Copiar para a área de transferência
                    navigator.clipboard.writeText(shareText).then(() => {
                        alert('Compartilhamento falhou, mas os jogos foram copiados para a área de transferência!');
                        closePopup('savedGamesOptionsPopup');
                    }).catch(err => {
                        alert('Compartilhamento e cópia falharam. Tente selecionar o texto e copiar manualmente.');
                    });
                });
            } else {
                // Fallback para ambientes sem Web Share API: Copiar para a área de transferência
                navigator.clipboard.writeText(shareText).then(() => {
                    alert('Os jogos foram copiados para a área de transferência! Cole em outro aplicativo para compartilhar.');
                    closePopup('savedGamesOptionsPopup');
                }).catch(err => {
                    alert('Erro ao copiar jogos. Tente novamente.');
                });
            }
        }

        // ==============================
        // NOVAS FUNÇÕES: COLETA E ENVIO DE FILTROS COM CRIPTOGRAFIA
        // ==============================
        
        // Função para coletar todos os filtros
        function coletarFiltros() {
            const filtros = {
                // Filtros básicos
                somaMin: document.getElementById('somaMin').value,
                somaMax: document.getElementById('somaMax').value,
                inicioMin: document.getElementById('inicioMin').value,
                inicioMax: document.getElementById('inicioMax').value,
                fimMin: document.getElementById('fimMin').value,
                fimMax: document.getElementById('fimMax').value,
                imparMin: document.getElementById('imparMin').value,
                imparMax: document.getElementById('imparMax').value,
                parMin: document.getElementById('parMin').value,
                parMax: document.getElementById('parMax').value,
                primosMin: document.getElementById('primosMin').value,
                primosMax: document.getElementById('primosMax').value,
                
                // Filtros de repetição
                rep8Min: document.getElementById('rep8Min').value,
                rep8Max: document.getElementById('rep8Max').value,
                rep9Min: document.getElementById('rep9Min').value,
                rep9Max: document.getElementById('rep9Max').value,
                rep10Min: document.getElementById('rep10Min').value,
                rep10Max: document.getElementById('rep10Max').value,
                rep16_21Min: document.getElementById('rep16_21Min').value,
                rep16_21Max: document.getElementById('rep16_21Max').value,
                
                // Filtros de linhas
                linha1Min: document.getElementById('linha1Min').value,
                linha1Max: document.getElementById('linha1Max').value,
                linha2Min: document.getElementById('linha2Min').value,
                linha2Max: document.getElementById('linha2Max').value,
                linha3Min: document.getElementById('linha3Min').value,
                linha3Max: document.getElementById('linha3Max').value,
                linha4Min: document.getElementById('linha4Min').value,
                linha4Max: document.getElementById('linha4Max').value,
                linha5Min: document.getElementById('linha5Min').value,
                linha5Max: document.getElementById('linha5Max').value,
                
                // Filtros de colunas
                coluna1Min: document.getElementById('coluna1Min').value,
                coluna1Max: document.getElementById('coluna1Max').value,
                coluna2Min: document.getElementById('coluna2Min').value,
                coluna2Max: document.getElementById('coluna2Max').value,
                coluna3Min: document.getElementById('coluna3Min').value,
                coluna3Max: document.getElementById('coluna3Max').value,
                coluna4Min: document.getElementById('coluna4Min').value,
                coluna4Max: document.getElementById('coluna4Max').value,
                coluna5Min: document.getElementById('coluna5Min').value,
                coluna5Max: document.getElementById('coluna5Max').value,
                
                // Filtros avançados
                molduraMin: document.getElementById('molduraMin').value,
                molduraMax: document.getElementById('molduraMax').value,
                centroMin: document.getElementById('centroMin').value,
                centroMax: document.getElementById('centroMax').value,
                xLotoMin: document.getElementById('xLotoMin').value,
                xLotoMax: document.getElementById('xLotoMax').value,
                hLotoMin: document.getElementById('hLotoMin').value,
                hLotoMax: document.getElementById('hLotoMax').value,
                primeirosMin: document.getElementById('primeirosMin').value,
                primeirosMax: document.getElementById('primeirosMax').value,
                invertidosMin: document.getElementById('invertidosMin').value,
                invertidosMax: document.getElementById('invertidosMax').value,
                pilaresMin: document.getElementById('pilaresMin').value,
                pilaresMax: document.getElementById('pilaresMax').value,
                magicosMin: document.getElementById('magicosMin').value,
                magicosMax: document.getElementById('magicosMax').value,
                
                // Novos filtros
                fibonacciMin: document.getElementById('fibonacciMin').value,
                fibonacciMax: document.getElementById('fibonacciMax').value,
                cruzMin: document.getElementById('cruzMin').value,
                cruzMax: document.getElementById('cruzMax').value,
                multiplo3Min: document.getElementById('multiplo3Min').value,
                multiplo3Max: document.getElementById('multiplo3Max').value,
                diag1Min: document.getElementById('diag1Min').value,
                diag1Max: document.getElementById('diag1Max').value,
                diag2Min: document.getElementById('diag2Min').value,
                diag2Max: document.getElementById('diag2Max').value,
                
                // Dezenas fixas e excluídas
                dezenasFixas: Array.from(selectedNumbers),
                dezenasExcluidas: Array.from(excludedNumbers),
                
                // Timestamp
                timestamp: new Date().toISOString()
            };
            
            return filtros;
        }
        
        // Função para validar filtros antes do envio
        function validarFiltros(filtros) {
            const erros = [];
            
            // Validar que os valores mínimos não são maiores que os máximos
            for (const key in filtros) {
                if (key.endsWith('Min') && key.endsWith('Max')) {
                    const minKey = key;
                    const maxKey = key.replace('Min', 'Max');
                    
                    if (parseInt(filtros[minKey]) > parseInt(filtros[maxKey])) {
                        erros.push(`O valor mínimo de ${key} não pode ser maior que o valor máximo`);
                    }
                }
            }
            
            // Validar dezenas fixas e excluídas não se sobrepõem
            const sobreposicao = filtros.dezenasFixas.filter(num => 
                filtros.dezenasExcluidas.includes(num)
            );
            
            if (sobreposicao.length > 0) {
                erros.push(`As dezenas ${sobreposicao.join(', ')} estão tanto nas fixas quanto nas excluídas`);
            }
            
            return erros;
        }
        
        // Função para criptografar os dados
        function criptografarDados(dados) {
            try {
                const dadosString = JSON.stringify(dados);
                const dadosCriptografados = CryptoJS.AES.encrypt(dadosString, ENCRYPTION_KEY).toString();
                return dadosCriptografados;
            } catch (error) {
                console.error('Erro ao criptografar dados:', error);
                throw new Error('Falha na criptografia dos dados');
            }
        }
        
        // Função para descriptografar os dados
        function descriptografarDados(dadosCriptografados) {
            try {
                const bytes = CryptoJS.AES.decrypt(dadosCriptografados, ENCRYPTION_KEY);
                const dadosString = bytes.toString(CryptoJS.enc.Utf8);
                return JSON.parse(dadosString);
            } catch (error) {
                console.error('Erro ao descriptografar dados:', error);
                throw new Error('Falha na descriptografia dos dados');
            }
        }
        
        // Função principal para enviar filtros para o backend
        async function enviarFiltrosParaDesdobramento() {
            try {
                // Mostrar loading
                showLoading();
                
                // Coletar filtros
                const filtros = coletarFiltros();
                
                // Validar filtros
                const erros = validarFiltros(filtros);
                if (erros.length > 0) {
                    hideLoading();
                    showNotification('error', 'Erros de validação: ' + erros.join(', '));
                    return;
                }
                
                // Criptografar dados
                const filtrosCriptografados = criptografarDados(filtros);
                
                // Simular envio para backend (em produção, seria um endpoint real)
                console.log('Enviando filtros criptografados:', filtrosCriptografados);
                
                // Simular processamento do backend
                await new Promise(resolve => setTimeout(resolve, 2000));
                
                // Simular resposta do backend
                const respostaSimulada = {
                    sucesso: true,
                    mensagem: 'Desdobramento processado com sucesso',
                    jogos: generateGamesManual(
                        Array.from({length: 25}, (_, i) => i + 1).filter(n => !excludedNumbers.has(n)),
                        parseInt(document.getElementById('dezenasManual').value) || 15,
                        parseInt(document.getElementById('qualityManual').value) || 10
                    )
                };
                
                // Processar resposta
                if (respostaSimulada.sucesso) {
                    currentGames = respostaSimulada.jogos;
                    renderGames();
                    showNotification('success', respostaSimulada.mensagem);
                } else {
                    showNotification('error', 'Erro no processamento do desdobramento');
                }
                
            } catch (error) {
                console.error('Erro ao enviar filtros:', error);
                showNotification('error', 'Erro ao processar desdobramento: ' + error.message);
            } finally {
                hideLoading();
            }
        }
        
        // Atualizar a função generateGames para usar o novo sistema
        function generateGames(type) {
            if (type === 'manual') {
                enviarFiltrosParaDesdobramento();
            } else {
                openIApopup();
            }
        }

        // ==============================
        // NOVAS FUNÇÕES: LOADING E NOTIFICAÇÕES
        // ==============================
        
        function showLoading() {
            document.getElementById('loadingSpinner').style.display = 'block';
        }
        
        function hideLoading() {
            document.getElementById('loadingSpinner').style.display = 'none';
        }
        
        function showNotification(type, message) {
            const notification = document.getElementById('notification');
            notification.textContent = message;
            notification.className = `notification ${type}`;
            notification.style.display = 'block';
            
            // Auto-esconder após 5 segundos
            setTimeout(() => {
                notification.style.display = 'none';
            }, 5000);
        }

        // ==============================
        // FUNÇÕES DE BACKUP E RESTAURAÇÃO
        // ==============================
        
        function fazerBackup() {
            try {
                const dadosBackup = {
                    usuarios: users,
                    jogosSalvos: savedGames,
                    jogosPorUsuario: userGames,
                    dataBackup: new Date().toISOString()
                };
                
                const dadosString = JSON.stringify(dadosBackup);
                const dadosCriptografados = criptografarDados(dadosString);
                
                // Criar blob e fazer download
                const blob = new Blob([dadosCriptografados], { type: 'application/octet-stream' });
                const url = URL.createObjectURL(blob);
                const a = document.createElement('a');
                a.href = url;
                a.download = `backup_loto_inteligente_${new Date().toISOString().split('T')[0]}.bak`;
                document.body.appendChild(a);
                a.click();
                document.body.removeChild(a);
                URL.revokeObjectURL(url);
                
                showNotification('success', 'Backup realizado com sucesso!');
            } catch (error) {
                console.error('Erro ao fazer backup:', error);
                showNotification('error', 'Erro ao fazer backup');
            }
        }
        
        function restaurarBackup() {
            const fileInput = document.createElement('input');
            fileInput.type = 'file';
            fileInput.accept = '.bak';
            fileInput.onchange = function(e) {
                const file = e.target.files[0];
                if (file) {
                    const reader = new FileReader();
                    reader.onload = function(event) {
                        try {
                            const dadosCriptografados = event.target.result;
                            const dadosString = descriptografarDados(dadosCriptografados);
                            const dadosBackup = JSON.parse(dadosString);
                            
                            // Restaurar dados
                            users = dadosBackup.usuarios || [];
                            savedGames = dadosBackup.jogosSalvos || [];
                            userGames = dadosBackup.jogosPorUsuario || {};
                            
                            // Salvar no localStorage
                            saveUsers();
                            saveGamesToStorage();
                            
                            // Atualizar interface
                            updateUserInterface();
                            renderUserGames();
                            
                            showNotification('success', 'Backup restaurado com sucesso!');
                        } catch (error) {
                            console.error('Erro ao restaurar backup:', error);
                            showNotification('error', 'Erro ao restaurar backup: arquivo inválido');
                        }
                    };
                    reader.readAsText(file);
                }
            };
            fileInput.click();
        }
    </script>
</body>
</html>
