<?php
// Lista de pastas remotas (API GitHub)
$paths = [
    'agent'      => 'https://api.github.com/repos/chicoalff/ktc-revisor-a33/contents/agent',
    'diagnostic' => 'https://api.github.com/repos/chicoalff/ktc-revisor-a33/contents/workflow/diagnostic',
    'knowledge'  => 'https://api.github.com/repos/chicoalff/ktc-revisor-a33/contents/knowledge',
];

// Pasta de destino local
$destDir = __DIR__ . '/public_html/revisor/';

// Extensões que serão copiadas
$allowed = ['json','yaml','yml','pdf','md'];

function fetchJson($url) {
    $ch = curl_init($url);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_USERAGENT, "ktc-revisor-script");
    $out = curl_exec($ch);
    curl_close($ch);
    return json_decode($out, true);
}

function saveFile($url, $path) {
    $data = file_get_contents($url);
    file_put_contents($path, $data);
}

function backupIfExists($file) {
    if (!file_exists($file)) {
        return;
    }
    $oldDir = dirname($file) . '/old';
    if (!is_dir($oldDir)) {
        mkdir($oldDir, 0777, true);
    }
    $info      = pathinfo($file);
    $ts        = date('Ymd_His');
    $newName   = $info['filename'] . '_' . $ts . '.' . $info['extension'];
    rename($file, "$oldDir/$newName");
}

// Garante existência da pasta de destino
if (!is_dir($destDir)) {
    mkdir($destDir, 0777, true);
}

foreach ($paths as $label => $apiUrl) {
    $files = fetchJson($apiUrl);
    if (!is_array($files)) {
        echo "Falha ao listar $label\n";
        continue;
    }
    foreach ($files as $f) {
        if ($f['type'] === 'file') {
            $ext = strtolower(pathinfo($f['name'], PATHINFO_EXTENSION));
            if (in_array($ext, $allowed)) {
                $localPath = $destDir . $f['name'];
                // Se já existir, faz backup
                backupIfExists($localPath);
                // Faz download (cópia) do novo arquivo
                saveFile($f['download_url'], $localPath);
                echo "Copiado: {$f['name']}\n";
            }
        }
    }
}

echo "Sincronização concluída.\n";