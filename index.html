<?php
error_reporting(E_ALL);
ini_set('display_errors', 1);
ini_set('display_startup_errors', 1);
session_start();



// Verifica e cria pasta /data se necessário
$data_dir = __DIR__ . '/data';
if (!is_dir($data_dir)) {
    mkdir($data_dir, 0777, true);
}

// Caminhos dos arquivos JSON
define('USUARIOS_FILE', "$data_dir/usuarios.json");
define('DEMANDAS_FILE', "$data_dir/demandas.json");
define('AVISOS_FILE', "$data_dir/avisos.json");
define('SYNC_FILE', "$data_dir/sync.json");

// ADICIONAR ESTE BLOCO DE DEBUG:
if (isset($_GET['debug_sync'])) {
    header('Content-Type: text/plain');
    echo "=== DEBUG SYNC ===\n";
    echo "SYNC_FILE: " . SYNC_FILE . "\n";
    echo "Arquivo existe: " . (file_exists(SYNC_FILE) ? 'SIM' : 'NÃO') . "\n";
    if (file_exists(SYNC_FILE)) {
        echo "Conteúdo: " . file_get_contents(SYNC_FILE) . "\n";
        echo "Timestamp atual: " . time() . "\n";
        $sync_data = ler_json(SYNC_FILE);
        echo "Timestamp no arquivo: " . $sync_data['timestamp'] . "\n";
        echo "Diferença: " . (time() - $sync_data['timestamp']) . " segundos\n";
    }
    echo "Permissões da pasta data: " . substr(sprintf('%o', fileperms($data_dir)), -4) . "\n";
    exit;
}

if (isset($_GET['check_sync'])) {
    header('Content-Type: application/json');
    header('Cache-Control: no-cache, no-store, must-revalidate');
    header('Pragma: no-cache');
    header('Expires: 0');
    header('Last-Modified: ' . gmdate('D, d M Y H:i:s') . ' GMT');
    
    // Força limpeza completa do cache
    clearstatcache(true, SYNC_FILE);
    if (function_exists('opcache_invalidate')) {
        opcache_invalidate(SYNC_FILE, true);
    }
    
    // Lê com lock
    $handle = fopen(SYNC_FILE, 'r');
    if ($handle) {
        flock($handle, LOCK_SH);
        $sync_content = fread($handle, filesize(SYNC_FILE));
        flock($handle, LOCK_UN);
        fclose($handle);
    } else {
        $sync_content = file_get_contents(SYNC_FILE);
    }
    
    $sync_data = json_decode($sync_content, true);
    
    // Adiciona timestamp atual para debug
    $sync_data['check_time'] = microtime(true);
    
    echo json_encode($sync_data);
    exit;
}
// Garante existência dos arquivos
foreach ([USUARIOS_FILE, DEMANDAS_FILE, AVISOS_FILE, SYNC_FILE] as $arquivo) {
    if (!file_exists($arquivo)) {
        if ($arquivo === SYNC_FILE) {
        file_put_contents($arquivo, json_encode(['timestamp' => microtime(true)]));
        } else {
            file_put_contents($arquivo, '[]');
        }
    }
}

// Utilitários JSON
function ler_json($arquivo) {
    if (!file_exists($arquivo)) return [];
    
    // Limpa completamente o cache antes de ler
    clearstatcache(true, $arquivo);
    if (function_exists('opcache_invalidate')) {
        opcache_invalidate($arquivo, true);
    }
    
    // Lê com lock exclusivo
    $conteudo = file_get_contents($arquivo);
    if ($conteudo === false) {
        return [];
    }
    
    $dados = json_decode($conteudo, true);
    return $dados ?: [];
}

function salvar_json($arquivo, $dados) {
    file_put_contents($arquivo, json_encode($dados, JSON_PRETTY_PRINT));
}
function atualizar_sync() {
    $sync_data = [
        'timestamp' => time(), // Mudança aqui: usar time() em vez de microtime(true)
        'last_action' => date('Y-m-d H:i:s'),
        'random' => mt_rand(100000, 999999),
        'server_time' => time()
    ];
    
    // Força gravação imediata
    $json_content = json_encode($sync_data);
    file_put_contents(SYNC_FILE, $json_content, LOCK_EX);
    
    // Força limpeza do cache
    clearstatcache(true, SYNC_FILE);
    if (function_exists('opcache_invalidate')) {
        opcache_invalidate(SYNC_FILE, true);
    }
    
    // Força permissões
    chmod(SYNC_FILE, 0666);
}
// Usuários fixos
$usuarios = [
    ["id" => 1, "login" => "010420611", "senha" => "1234"],
    ["id" => 2, "login" => "010497737", "senha" => "123"],
    ["id" => 3, "login" => "jasbarbosa", "senha" => "123"],
    ["id" => 4, "login" => "maycon341753", "senha" => "123"]
];
salvar_json(USUARIOS_FILE, $usuarios);
// Adicionar usuário de suporte
// Adicionar usuário de suporte
if (isset($_GET['convidar'])) {
    $demanda_id = (int) $_GET['convidar'];
    $usuario_id = (int) $_GET['usuario'];
    $demandas = ler_json(DEMANDAS_FILE);
    foreach ($demandas as &$d) {
        if ($d['id'] == $demanda_id && $d['responsavel_id'] == $_SESSION['usuario']['id']) {
            $d['suporte_id'] = $usuario_id;
        }
    }
    salvar_json(DEMANDAS_FILE, $demandas);
    atualizar_sync(); // ADICIONAR ESTA LINHA
    header("Location: ?painel");
    exit;
}
// Excluir aviso (apenas Denilson)
if (isset($_GET['excluir_aviso']) && $_SESSION['usuario']['login'] == 'Denilson') {
    $aviso_id = (int) $_GET['excluir_aviso'];
    $avisos = ler_json(AVISOS_FILE);
    $avisos = array_filter($avisos, function($a) use ($aviso_id) {
        return $a['id'] != $aviso_id;
    });
    salvar_json(AVISOS_FILE, array_values($avisos));
    atualizar_sync();
    header("Location: ?painel");
    exit;
}

// Excluir demanda finalizada (apenas Denilson)
if (isset($_GET['excluir_demanda']) && $_SESSION['usuario']['login'] == 'Denilson') {
    $demanda_id = (int) $_GET['excluir_demanda'];
    $demandas = ler_json(DEMANDAS_FILE);
    $demandas = array_filter($demandas, function($d) use ($demanda_id) {
        return $d['id'] != $demanda_id || !$d['finalizada'];
    });
    salvar_json(DEMANDAS_FILE, array_values($demandas));
    atualizar_sync();
    header("Location: ?painel");
    exit;
}
// Processa login
if (isset($_POST['login'])) {
    $login = $_POST['login'];
    $senha = $_POST['senha'];
    foreach ($usuarios as $usuario) {
        if ($usuario['login'] === $login && $usuario['senha'] === $senha) {
            $_SESSION['usuario'] = $usuario;
            header("Location: ?painel");
            exit;
        }
    }
    // Limpar avisos antigos (mais de 48 horas)
$avisos = ler_json(AVISOS_FILE);
$avisos_filtrados = [];
foreach ($avisos as $aviso) {
    $data_aviso = strtotime($aviso['data']);
    $agora = time();
    $diferenca_horas = ($agora - $data_aviso) / 3600;
    if ($diferenca_horas <= 48) {
        $avisos_filtrados[] = $aviso;
    }
}
if (count($avisos_filtrados) != count($avisos)) {
    salvar_json(AVISOS_FILE, $avisos_filtrados);
}
    $erro = "Login ou senha inválidos.";
}

// Logout
if (isset($_GET['logout'])) {
    session_destroy();
    header("Location: ?");
    exit;
}

// Aceitar demanda
if (isset($_GET['aceitar'])) {
    $id = (int) $_GET['aceitar'];
    $demandas = ler_json(DEMANDAS_FILE);
    foreach ($demandas as &$d) {
        if ($d['id'] == $id && empty($d['responsavel_id'])) {
            $d['responsavel_id'] = $_SESSION['usuario']['id'];
        }
    }
    salvar_json(DEMANDAS_FILE, $demandas);
    atualizar_sync(); // ADICIONAR ESTA LINHA
    header("Location: ?painel");
    exit;
}

// Finalizar demanda
if (isset($_GET['finalizar'])) {
    $id = (int) $_GET['finalizar'];
    $demandas = ler_json(DEMANDAS_FILE);
    foreach ($demandas as &$d) {
        if ($d['id'] == $id && $d['responsavel_id'] == $_SESSION['usuario']['id']) {
            $d['finalizada'] = true;
            $d['data_finalizacao'] = date('Y-m-d H:i:s');
        }
    }
    salvar_json(DEMANDAS_FILE, $demandas);
    atualizar_sync(); // ADICIONAR ESTA LINHA
    header("Location: ?painel");
    exit; // ADICIONAR ESTA LINHA
}

// Adicionar demanda
if (isset($_POST['nova_demanda'])) {
    $demandas = ler_json(DEMANDAS_FILE);
    $demandas[] = [
        'id' => time(),
        'titulo' => $_POST['titulo'],
        'descricao' => $_POST['descricao'],
        'autor_id' => $_SESSION['usuario']['id'],
        'responsavel_id' => null,
        'suporte_id' => null,
        'data' => date('Y-m-d H:i:s'),
        'finalizada' => false,
        'avaliada_por_denilson' => false
    ];
    salvar_json(DEMANDAS_FILE, $demandas);
    atualizar_sync(); // ADICIONAR ESTA LINHA
    header("Location: ?painel");
    exit; // ADICIONAR ESTA LINHA
}

// Adicionar aviso
if (isset($_POST['novo_aviso'])) {
    $avisos = ler_json(AVISOS_FILE);
    $avisos[] = [
        'id' => time(),
        'titulo' => $_POST['titulo'],
        'descricao' => $_POST['descricao'],
        'autor_id' => $_SESSION['usuario']['id'],
        'data' => date('Y-m-d H:i:s')
    ];
    salvar_json(AVISOS_FILE, $avisos);
    atualizar_sync(); // ADICIONAR ESTA LINHA
    header("Location: ?painel");
    exit; // ADICIONAR ESTA LINHA
}

function nome_usuario($id) {
    foreach (ler_json(USUARIOS_FILE) as $u) {
        if ($u['id'] == $id) return $u['login'];
    }
    return "?";
}
?>
<?php
if (
    isset($_SESSION['usuario']) &&
    $_SESSION['usuario']['login'] == 'Denilson' &&
    $_SERVER['REQUEST_METHOD'] === 'POST'
) {
    $demandas = ler_json(DEMANDAS_FILE);
    $alterado = false;

    foreach ($demandas as &$d) {
        if (isset($_POST['avaliar_' . $d['id']])) {
            $d['avaliada_por_denilson'] = true;
            $alterado = true;
        }
    }

    if ($alterado) {
        salvar_json(DEMANDAS_FILE, $demandas);
        header("Location: ?painel");
        exit;
    }
}
?>
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Portal de Demandas</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            color: #333;
            line-height: 1.6;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        /* Login Styles */
        .login-container {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
        }

        .login-card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(20px);
            border-radius: 20px;
            padding: 40px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            width: 100%;
            max-width: 400px;
            text-align: center;
        }

        .login-card h2 {
            color: #2d3748;
            margin-bottom: 30px;
            font-size: 28px;
            font-weight: 700;
        }

        .login-card .logo {
            width: 80px;
            height: 80px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border-radius: 50%;
            margin: 0 auto 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 24px;
        }

        /* Header */
        .header {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(20px);
            border-radius: 20px;
            padding: 20px 30px;
            margin-bottom: 30px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
        }

        .header h2 {
            color: #2d3748;
            font-size: 24px;
            font-weight: 700;
        }

        .user-info {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .user-avatar {
            width: 40px;
            height: 40px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: 600;
        }

        /* Cards */
        .card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(20px);
            border-radius: 16px;
            padding: 25px;
            margin-bottom: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }

        .card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 4px;
            background: linear-gradient(90deg, #667eea 0%, #764ba2 100%);
        }

        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.15);
        }

        .section-title {
            color: #2d3748;
            font-size: 22px;
            font-weight: 700;
            margin: 40px 0 20px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .section-title::before {
            content: '';
            width: 4px;
            height: 24px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border-radius: 2px;
        }

        /* Forms */
        .form-group {
            margin-bottom: 20px;
        }

        .form-row {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-bottom: 30px;
        }

        input, textarea {
            width: 100%;
            padding: 15px 20px;
            border: 2px solid #e2e8f0;
            border-radius: 12px;
            font-size: 16px;
            font-family: inherit;
            transition: all 0.3s ease;
            background: rgba(255, 255, 255, 0.8);
        }

        input:focus, textarea:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
            background: white;
        }

        textarea {
            resize: vertical;
            min-height: 100px;
        }

        /* Buttons */
        .btn {
            padding: 12px 24px;
            border: none;
            border-radius: 10px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            text-decoration: none;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            font-family: inherit;
        }

        .btn-primary {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            box-shadow: 0 4px 15px rgba(102, 126, 234, 0.3);
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(102, 126, 234, 0.4);
        }

        .btn-success {
            background: linear-gradient(135deg, #48bb78 0%, #38a169 100%);
            color: white;
            box-shadow: 0 4px 15px rgba(72, 187, 120, 0.3);
        }

        .btn-success:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(72, 187, 120, 0.4);
        }

        .btn-warning {
            background: linear-gradient(135deg, #ed8936 0%, #dd6b20 100%);
            color: white;
            box-shadow: 0 4px 15px rgba(237, 137, 54, 0.3);
        }

        .btn-warning:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(237, 137, 54, 0.4);
        }

        .btn-danger {
            background: linear-gradient(135deg, #f56565 0%, #e53e3e 100%);
            color: white;
            box-shadow: 0 4px 15px rgba(245, 101, 101, 0.3);
        }

        .btn-danger:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(245, 101, 101, 0.4);
        }

        .btn-outline {
            background: transparent;
            border: 2px solid #667eea;
            color: #667eea;
        }

        .btn-outline:hover {
            background: #667eea;
            color: white;
        }

        /* Status badges */
        .status-badge {
            display: inline-flex;
            align-items: center;
            gap: 5px;
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .status-pending {
            background: rgba(237, 137, 54, 0.1);
            color: #dd6b20;
        }

        .status-completed {
            background: rgba(72, 187, 120, 0.1);
            color: #38a169;
        }

        .status-in-progress {
            background: rgba(102, 126, 234, 0.1);
            color: #667eea;
        }

        /* Meta info */
        .meta-info {
            display: flex;
            align-items: center;
            gap: 20px;
            margin-top: 15px;
            font-size: 14px;
            color: #718096;
            flex-wrap: wrap;
        }
       

        .meta-item {
            display: flex;
            align-items: center;
            gap: 5px;
        }

        /* Error messages */
        .error-message {
            background: rgba(245, 101, 101, 0.1);
            border: 1px solid rgba(245, 101, 101, 0.2);
            color: #e53e3e;
            padding: 15px 20px;
            border-radius: 12px;
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        /* Grid layouts */
        .grid-2 {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
        }

        /* Card content styling */
        .card-title {
            font-size: 18px;
            font-weight: 700;
            color: #2d3748;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .card-description {
            color: #4a5568;
            margin-bottom: 15px;
            line-height: 1.6;
        }

        .card-actions {
            display: flex;
            gap: 10px;
            margin-top: 20px;
            flex-wrap: wrap;
        }

        /* Responsive */
        @media (max-width: 768px) {
            .container {
                padding: 15px;
            }

            .header {
                flex-direction: column;
                gap: 15px;
                text-align: center;
            }

            .form-row {
                grid-template-columns: 1fr;
            }

            .card-actions {
                justify-content: center;
            }

            .meta-info {
                justify-content: center;
            }
        }

        /* Loading animation */
        .loading {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 2px solid #f3f3f3;
            border-top: 2px solid #667eea;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* Pulse animation for new items */
        @keyframes pulse {
            0% { box-shadow: 0 0 0 0 rgba(102, 126, 234, 0.4); }
            70% { box-shadow: 0 0 0 10px rgba(102, 126, 234, 0); }
            100% { box-shadow: 0 0 0 0 rgba(102, 126, 234, 0); }
        }

        .pulse {
            animation: pulse 2s infinite;
        }
    </style>
</head>
<body>
<?php if (!isset($_SESSION['usuario'])): ?>
    <div class="login-container">
        <div class="login-card">
            <div class="logo">
                <i class="fas fa-tasks"></i>
            </div>
            <h2>Portal de Demandas</h2>
            <?php if (isset($erro)): ?>
                <div class="error-message">
                    <i class="fas fa-exclamation-triangle"></i>
                    <?= $erro ?>
                </div>
            <?php endif; ?>
            <form method="post">
                <div class="form-group">
                    <input type="text" name="login" placeholder="Usuário" required>
                </div>
                <div class="form-group">
                    <input type="password" name="senha" placeholder="Senha" required>
                </div>
                <button class="btn btn-primary" style="width: 100%;">
                    <i class="fas fa-sign-in-alt"></i>
                    Entrar
                </button>
            </form>
        </div>
    </div>
<?php else: ?>
    <div class="container">
        <div class="header">
            <div style="display: flex; align-items: center; gap: 15px;">
                <div style="width: 50px; height: 50px; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); border-radius: 12px; display: flex; align-items: center; justify-content: center; color: white; font-size: 20px;">
                    <i class="fas fa-tasks"></i>
                </div>
                <div>
                    <h2>Portal de Demandas</h2>
                    <p style="color: #718096; margin: 0;">Sistema de Gerenciamento</p>
                </div>
            </div>
            <div class="user-info">
                <div class="user-avatar">
                    <?= strtoupper(substr($_SESSION['usuario']['login'], 0, 1)) ?>
                </div>
                <div>
                    <div style="font-weight: 600; color: #2d3748;">
                        <?= $_SESSION['usuario']['login'] ?>
                    </div>
                    <div style="font-size: 12px; color: #718096;">
                        Online
                    </div>
                </div>
                <a href="?logout" class="btn btn-outline">
                    <i class="fas fa-sign-out-alt"></i>
                    Sair
                </a>
            </div>
        </div>

        <div class="grid-2">
            <div class="card">
                <div class="card-title">
                    <i class="fas fa-plus-circle" style="color: #667eea;"></i>
                    Nova Demanda
                </div>
                <form method="post">
                    <div class="form-group">
                        <input name="titulo" placeholder="Título da demanda" required>
                    </div>
                    <div class="form-group">
                        <textarea name="descricao" placeholder="Descreva detalhadamente a demanda..." required></textarea>
                    </div>
                    <button class="btn btn-primary" name="nova_demanda">
                        <i class="fas fa-paper-plane"></i>
                        Criar Demanda
                    </button>
                </form>
            </div>

            <div class="card">
                <div class="card-title">
                    <i class="fas fa-bullhorn" style="color: #ed8936;"></i>
                    Novo Aviso
                </div>
                <form method="post">
                    <div class="form-group">
                        <input name="titulo" placeholder="Título do aviso" required>
                    </div>
                    <div class="form-group">
                        <textarea name="descricao" placeholder="Conteúdo do aviso..." required></textarea>
                    </div>
                    <button class="btn btn-warning" name="novo_aviso">
                        <i class="fas fa-broadcast-tower"></i>
                        Publicar Aviso
                    </button>
                </form>
            </div>
        </div>

       </div>

        <h3 class="section-title">
            <i class="fas fa-bullhorn"></i>
            Avisos Ativos
        </h3>
        <div class="grid-2">
            <?php foreach (ler_json(AVISOS_FILE) as $aviso): ?>
                <div class="card">
                    <div class="status-badge" style="background: rgba(237, 137, 54, 0.1); color: #dd6b20;">
                        <i class="fas fa-bullhorn"></i>
                        Aviso
                    </div>
                    <div class="card-title" style="margin-top: 15px;">
                        <?= htmlspecialchars($aviso['titulo']) ?>
                    </div>
                    <div class="card-description">
                        <?= htmlspecialchars($aviso['descricao']) ?>
                    </div>
                    <div class="meta-info">
                        <div class="meta-item">
                            <i class="fas fa-user"></i>
                            <?= nome_usuario($aviso['autor_id']) ?>
                        </div>
                        <div class="meta-item">
                            <i class="fas fa-calendar-alt"></i>
                            <?= date('d/m/Y H:i', strtotime($aviso['data'])) ?>
                        </div>
                        <div class="meta-item">
                            <i class="fas fa-clock"></i>
                            <?php
                            $horas_restantes = 48 - ((time() - strtotime($aviso['data'])) / 3600);
                            echo ceil($horas_restantes) . "h restantes";
                            ?>
                        </div>
                    </div>
                    <?php if ($_SESSION['usuario']['login'] == 'Denilson'): ?>
    <div class="card-actions">
        <a href="?excluir_aviso=<?= $aviso['id'] ?>" class="btn btn-danger" 
           onclick="return confirm('Tem certeza que deseja excluir este aviso?')">
            <i class="fas fa-trash"></i>
            Excluir
        </a>
    </div>
<?php endif; ?>
                </div>
                 
            <?php endforeach; ?>
            
           

        </div>

        <h3 class="section-title"> 

        <h3 class="section-title">
            <i class="fas fa-clock"></i>
            Demandas Pendentes
        </h3>
        <div class="grid-2">
            <?php foreach (ler_json(DEMANDAS_FILE) as $d): 
                if (empty($d['responsavel_id']) && !$d['finalizada']): ?>
                <div class="card pulse">
                    <div class="status-badge status-pending">
                        <i class="fas fa-hourglass-half"></i>
                        Pendente
                    </div>
                    <div class="card-title" style="margin-top: 15px;">
                        <?= htmlspecialchars($d['titulo']) ?>
                    </div>
                    <div class="card-description">
                        <?= htmlspecialchars($d['descricao']) ?>
                    </div>
                    <div class="meta-info">
                        <div class="meta-item">
                            <i class="fas fa-user"></i>
                            <?= nome_usuario($d['autor_id']) ?>
                        </div>
                        <div class="meta-item">
                            <i class="fas fa-calendar-alt"></i>
                            <?= date('d/m/Y H:i', strtotime($d['data'])) ?>
                        </div>
                    </div>
                    <div class="card-actions">
                        <a href="?aceitar=<?= $d['id'] ?>" class="btn btn-success">
                            <i class="fas fa-check"></i>
                            Aceitar Demanda
                        </a>
                    </div>
                </div>
            <?php endif; endforeach; ?>
        </div>

        <h3 class="section-title">
            <i class="fas fa-cogs"></i>
            Minhas Demandas em Andamento
        </h3>
        <div class="grid-2">
            <?php foreach (ler_json(DEMANDAS_FILE) as $d): 
                if ($d['responsavel_id'] == $_SESSION['usuario']['id'] && !$d['finalizada']): ?>
                <div class="card">
                    <div class="status-badge status-in-progress">
                        <i class="fas fa-spinner"></i>
                        Em Andamento
                    </div>
                    <div class="card-title" style="margin-top: 15px;">
                        <?= htmlspecialchars($d['titulo']) ?>
                    </div>
                    <div class="card-description">
                        <?= htmlspecialchars($d['descricao']) ?>
                    </div>
                    <div class="meta-info">
                        <div class="meta-item">
                            <i class="fas fa-user"></i>
                            Criado por <?= nome_usuario($d['autor_id']) ?>
                        </div>
                        <div class="meta-item">
                            <i class="fas fa-calendar-alt"></i>
                            <?= date('d/m/Y H:i', strtotime($d['data'])) ?>
                        </div>
                    </div>
                   <div class="card-actions">
                        <a href="?finalizar=<?= $d['id'] ?>" class="btn btn-success">
                            <i class="fas fa-check-circle"></i>
                            Finalizar
                        </a>
                        <?php if (empty($d['suporte_id'])): ?>
                            <button class="btn btn-primary" onclick="mostrarModalConvite(<?= $d['id'] ?>)">
                                <i class="fas fa-user-plus"></i>
                                Convidar Suporte
                            </button>
                        <?php else: ?>
                            <span class="status-badge" style="background: rgba(72, 187, 120, 0.1); color: #38a169;">
                                <i class="fas fa-users"></i>
                                Suporte: <?= nome_usuario($d['suporte_id']) ?>
                            </span>
                        <?php endif; ?>
                    </div>
                </div>
            <?php endif; endforeach; ?>
        </div>
        <h3 class="section-title">
            <i class="fas fa-hands-helping"></i>
            Demandas onde fui Convidado como Suporte
        </h3>
        <div class="grid-2">
            <?php foreach (ler_json(DEMANDAS_FILE) as $d): 
                if ($d['suporte_id'] == $_SESSION['usuario']['id'] && !$d['finalizada']): ?>
                <div class="card">
                    <div class="status-badge" style="background: rgba(102, 126, 234, 0.1); color: #667eea;">
                        <i class="fas fa-hands-helping"></i>
                        Apoiando
                    </div>
                    <div class="card-title" style="margin-top: 15px;">
                        <?= htmlspecialchars($d['titulo']) ?>
                    </div>
                    <div class="card-description">
                        <?= htmlspecialchars($d['descricao']) ?>
                    </div>
                    <div class="meta-info">
                        <div class="meta-item">
                            <i class="fas fa-user"></i>
                            Criado por <?= nome_usuario($d['autor_id']) ?>
                        </div>
                        <div class="meta-item">
                            <i class="fas fa-user-cog"></i>
                            Responsável: <?= nome_usuario($d['responsavel_id']) ?>
                        </div>
                        <div class="meta-item">
                            <i class="fas fa-calendar-alt"></i>
                            <?= date('d/m/Y H:i', strtotime($d['data'])) ?>
                        </div>
                    </div>
                    <div class="card-actions">
                        <span class="status-badge" style="background: rgba(72, 187, 120, 0.1); color: #38a169;">
                            <i class="fas fa-info-circle"></i>
                            Você está apoiando esta demanda
                        </span>
                    </div>
                </div>
            <?php endif; endforeach; ?>
        </div>
        <h3 class="section-title">
            <i class="fas fa-check-double"></i>
            Demandas Finalizadas
        </h3>
        <div class="grid-2">
            <?php foreach (ler_json(DEMANDAS_FILE) as $d): 
                if ($d['responsavel_id'] == $_SESSION['usuario']['id'] && $d['finalizada']): ?>
                <div class="card">
                    <div class="status-badge status-completed">
                        <i class="fas fa-check-circle"></i>
                        Finalizada
                    </div>
                    <div class="card-title" style="margin-top: 15px;">
                        <?= htmlspecialchars($d['titulo']) ?>
                    </div>
                    <div class="meta-info">
                        <div class="meta-item">
                            <i class="fas fa-calendar-check"></i>
                            Finalizada em <?= date('d/m/Y H:i', strtotime($d['data_finalizacao'])) ?>
                        </div>
                        <?php if ($d['avaliada_por_denilson']): ?>
                            <div class="meta-item" style="color: #38a169;">
                                <i class="fas fa-star"></i>
                                Avaliada
                            </div>
                        <?php endif; ?>
                    </div>
                    <?php if ($_SESSION['usuario']['login'] == 'Denilson'): ?>
    <div class="card-actions">
        <a href="?excluir_demanda=<?= $d['id'] ?>" class="btn btn-danger" 
           onclick="return confirm('Tem certeza que deseja excluir esta demanda?')">
            <i class="fas fa-trash"></i>
            Excluir
        </a>
    </div>
<?php endif; ?>
                </div>
            <?php endif; endforeach; ?>
            
        </div>

        <?php if ($_SESSION['usuario']['login'] == 'Denilson'): ?>
            <h3 class="section-title">
                <i class="fas fa-star"></i>
                Avaliação das Demandas Finalizadas
            </h3>
            <div class="grid-2">
                <?php foreach (ler_json(DEMANDAS_FILE) as $d):
                    if ($d['finalizada'] && !$d['avaliada_por_denilson']): ?>
                    <div class="card">
                        <div class="status-badge status-completed">
                            <i class="fas fa-check-circle"></i>
                            Aguardando Avaliação
                        </div>
                        <div class="card-title" style="margin-top: 15px;">
                            <?= htmlspecialchars($d['titulo']) ?>
                        </div>
                        <div class="meta-info">
                            <div class="meta-item">
                                <i class="fas fa-user"></i>
                                Responsável: <?= nome_usuario($d['responsavel_id']) ?>
                            </div>
                            <div class="meta-item">
                                <i class="fas fa-calendar-check"></i>
                                Finalizada em <?= date('d/m/Y H:i', strtotime($d['data_finalizacao'])) ?>
                            </div>
                        </div>
                        <div class="card-actions">
                            <form method="post" style="display: inline;">
                                <button class="btn btn-success" name="avaliar_<?= $d['id'] ?>">
                                    <i class="fas fa-thumbs-up"></i>
                                    Avaliar como OK
                                </button>
                                <a href="?excluir_demanda=<?= $d['id'] ?>" class="btn btn-danger" 
   onclick="return confirm('Tem certeza que deseja excluir esta demanda?')">
    <i class="fas fa-trash"></i>
    Excluir
</a>
                            </form>
                        </div>
                    </div>
                <?php endif; endforeach; ?>
            </div>
        <?php endif; ?>
    </div>
<?php endif; ?>

<script>
// Sistema de auto-refresh simplificado e eficaz
let intervalId;
let isChecking = false;
function startAutoRefresh() {
    if (intervalId) clearInterval(intervalId);
    
    intervalId = setInterval(() => {
        if (isChecking) return;
        isChecking = true;
        
        fetch(window.location.href + '&_check=' + Date.now(), {
            method: 'GET',
            cache: 'no-store',
            headers: {
                'Cache-Control': 'no-cache',
                'Pragma': 'no-cache'
            }
        })
        .then(response => response.text())
        .then(html => {
            // Compara se o conteúdo mudou
            const parser = new DOMParser();
            const newDoc = parser.parseFromString(html, 'text/html');
            const currentCards = document.querySelectorAll('.card').length;
            const newCards = newDoc.querySelectorAll('.card').length;
            
            if (currentCards !== newCards) {
                location.reload();
            }
        })
        .catch(error => {
            console.log('Erro na verificação:', error);
        })
        .finally(() => {
            isChecking = false;
        });
    }, 2000); // Verifica a cada 2 segundos
}
// Inicia o auto-refresh quando a página carrega
document.addEventListener('DOMContentLoaded', function() {
    startAutoRefresh();
    
    // Adiciona efeito de loading aos botões
    const buttons = document.querySelectorAll('.btn');
    buttons.forEach(button => {
        button.addEventListener('click', function(e) {
            if (this.type === 'submit' || this.tagName === 'A') {
                const icon = this.querySelector('i');
                if (icon && !icon.classList.contains('fa-spinner')) {
                    icon.className = 'fas fa-spinner fa-spin';
                }
            }
        });
    });
});
// Funções do modal de convite
let demandaAtual = null;
function mostrarModalConvite(demandaId) {
    demandaAtual = demandaId;
    const modal = document.getElementById('modalConvite');
    modal.style.display = 'flex';
}
function fecharModal() {
    const modal = document.getElementById('modalConvite');
    modal.style.display = 'none';
    demandaAtual = null;
}
function convidarUsuario(usuarioId) {
    if (demandaAtual) {
        window.location.href = `?convidar=${demandaAtual}&usuario=${usuarioId}`;
    }
}

// Fechar modal ao clicar fora
document.getElementById('modalConvite').addEventListener('click', function(e) {
    if (e.target === this) {
        fecharModal();
    }
});
</script>
<!-- Modal de Convite -->
<div id="modalConvite" style="display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); z-index: 1000; align-items: center; justify-content: center;">
    <div style="background: white; border-radius: 20px; padding: 30px; max-width: 400px; width: 90%; box-shadow: 0 20px 40px rgba(0,0,0,0.3);">
        <h3 style="margin-bottom: 20px; color: #2d3748;">
            <i class="fas fa-user-plus"></i>
            Convidar Suporte
        </h3>
        <p style="color: #4a5568; margin-bottom: 20px;">Selecione um usuário para ajudar nesta demanda:</p>
        <div id="listaUsuarios" style="margin-bottom: 20px;">
            <?php foreach (ler_json(USUARIOS_FILE) as $usuario): ?>
                <?php if ($usuario['id'] != $_SESSION['usuario']['id']): ?>
                    <div style="padding: 10px; border: 1px solid #e2e8f0; border-radius: 8px; margin-bottom: 10px; cursor: pointer; transition: all 0.3s ease;" 
                         onclick="convidarUsuario(<?= $usuario['id'] ?>)" 
                         onmouseover="this.style.background='#f7fafc'" 
                         onmouseout="this.style.background='white'">
                        <div style="display: flex; align-items: center; gap: 10px;">
                            <div style="width: 30px; height: 30px; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-weight: 600; font-size: 12px;">
                                <?= strtoupper(substr($usuario['login'], 0, 1)) ?>
                            </div>
                            <span style="font-weight: 600;"><?= $usuario['login'] ?></span>
                        </div>
                    </div>
                <?php endif; ?>
            <?php endforeach; ?>
        </div>
        <div style="display: flex; gap: 10px; justify-content: flex-end;">
            <button class="btn btn-outline" onclick="fecharModal()">
                <i class="fas fa-times"></i>
                Cancelar
            </button>
        </div>
    </div>
</div>
</body>
</html>
