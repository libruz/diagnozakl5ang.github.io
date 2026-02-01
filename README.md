<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Odtwarzacz MP3 - diagnozakl5ang.github.io</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.6;
            color: #333;
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
        }
        
        .container {
            background: white;
            border-radius: 15px;
            padding: 30px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            margin-top: 20px;
        }
        
        header {
            text-align: center;
            margin-bottom: 40px;
            padding-bottom: 20px;
            border-bottom: 2px solid #f0f0f0;
        }
        
        h1 {
            color: #2c3e50;
            font-size: 2.5em;
            margin-bottom: 10px;
        }
        
        .subtitle {
            color: #7f8c8d;
            font-size: 1.1em;
        }
        
        .audio-player {
            background: linear-gradient(to right, #f8f9fa, #e9ecef);
            border-radius: 15px;
            padding: 30px;
            margin-bottom: 30px;
            border: 1px solid #dee2e6;
        }
        
        .player-container {
            max-width: 600px;
            margin: 0 auto;
        }
        
        audio {
            width: 100%;
            height: 50px;
            margin-bottom: 20px;
            border-radius: 25px;
        }
        
        .player-controls {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 20px;
            flex-wrap: wrap;
        }
        
        button {
            background: linear-gradient(to right, #667eea, #764ba2);
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: 600;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);
        }
        
        button:active {
            transform: translateY(0);
        }
        
        .file-info {
            background: #f8f9fa;
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 30px;
            border-left: 4px solid #667eea;
        }
        
        .info-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 15px;
            margin-top: 10px;
        }
        
        .info-item {
            display: flex;
            flex-direction: column;
        }
        
        .info-label {
            font-weight: bold;
            color: #667eea;
            margin-bottom: 5px;
        }
        
        .info-value {
            font-family: 'Courier New', monospace;
            background: #eef2f7;
            padding: 8px 12px;
            border-radius: 5px;
            word-break: break-all;
        }
        
        .url-section {
            background: #eef2f7;
            border-radius: 10px;
            padding: 25px;
            margin-top: 30px;
        }
        
        .url-section h3 {
            color: #2c3e50;
            margin-bottom: 15px;
        }
        
        .url-box {
            background: white;
            padding: 15px;
            border-radius: 8px;
            border: 1px solid #ddd;
            margin-bottom: 15px;
            overflow-x: auto;
        }
        
        .url-box code {
            color: #d63384;
            font-size: 14px;
        }
        
        .copy-btn {
            background: #6c757d;
            padding: 8px 16px;
            font-size: 14px;
        }
        
        .test-result {
            margin-top: 15px;
            padding: 15px;
            border-radius: 8px;
            display: none;
        }
        
        .test-result.success {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
            display: block;
        }
        
        .test-result.error {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
            display: block;
        }
        
        .audio-wave {
            height: 100px;
            background: linear-gradient(90deg, transparent 0%, #667eea 50%, transparent 100%);
            margin: 20px 0;
            border-radius: 10px;
            opacity: 0.3;
            animation: wave 2s infinite;
        }
        
        @keyframes wave {
            0% { opacity: 0.2; }
            50% { opacity: 0.5; }
            100% { opacity: 0.2; }
        }
        
        .status-indicator {
            display: inline-flex;
            align-items: center;
            gap: 8px;
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 14px;
            margin-bottom: 15px;
        }
        
        .status-dot {
            width: 10px;
            height: 10px;
            border-radius: 50%;
            animation: pulse 2s infinite;
        }
        
        .status-ready .status-dot {
            background: #28a745;
        }
        
        .status-loading .status-dot {
            background: #ffc107;
        }
        
        .status-error .status-dot {
            background: #dc3545;
        }
        
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.2); }
            100% { transform: scale(1); }
        }
        
        footer {
            text-align: center;
            margin-top: 40px;
            padding-top: 20px;
            border-top: 1px solid #eee;
            color: #7f8c8d;
            font-size: 0.9em;
        }
        
        @media (max-width: 768px) {
            .container {
                padding: 20px;
            }
            
            h1 {
                font-size: 2em;
            }
            
            .player-controls {
                flex-direction: column;
            }
            
            button {
                width: 100%;
                justify-content: center;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>🎵 Odtwarzacz MP3 - diagnozakl5ang</h1>
            <p class="subtitle">Odtwarzanie pliku audio z GitHub Pages</p>
        </header>
        
        <main>
            <div class="audio-player">
                <div class="player-container">
                    <!-- Status pliku -->
                    <div class="status-indicator status-ready" id="statusIndicator">
                        <span class="status-dot"></span>
                        <span id="statusText">Gotowy do odtwarzania</span>
                    </div>
                    
                    <!-- Wizualizacja audio -->
                    <div class="audio-wave" id="audioWave"></div>
                    
                    <!-- ODTWARZACZ AUDIO -->
                    <audio id="audioPlayer" controls>
                        <!-- GŁÓWNE ŹRÓDŁO: GitHub Pages -->
                        <source src="https://libruz.github.io/diagnozakl5ang.github.io/nagranie1.mp3" type="audio/mp3">
                        
                        <!-- ALTERNATYWNE ŹRÓDŁA -->
                        <source src="./nagranie1.mp3" type="audio/mp3">
                        <source src="https://cdn.jsdelivr.net/gh/libruz/diagnozakl5ang.github.io@main/nagranie1.mp3" type="audio/mp3">
                        <source src="https://raw.githubusercontent.com/libruz/diagnozakl5ang.github.io/main/nagranie1.mp3" type="audio/mp3">
                        
                        Twoja przeglądarka nie obsługuje odtwarzacza audio.
                    </audio>
                    
                    <!-- Informacje o audio -->
                    <div class="file-info">
                        <div class="info-grid">
                            <div class="info-item">
                                <span class="info-label">Nazwa pliku:</span>
                                <span class="info-value">nagranie1.mp3</span>
                            </div>
                            <div class="info-item">
                                <span class="info-label">Format:</span>
                                <span class="info-value">MP3 (audio/mpeg)</span>
                            </div>
                            <div class="info-item">
                                <span class="info-label">Źródło:</span>
                                <span class="info-value" id="currentSource">Ładowanie...</span>
                            </div>
                            <div class="info-item">
                                <span class="info-label">Dostępność:</span>
                                <span class="info-value" id="availabilityStatus">Sprawdzanie...</span>
                            </div>
                        </div>
                    </div>
                    
                    <!-- Kontrolki -->
                    <div class="player-controls">
                        <button onclick="playAudio()">
                            <span>▶️</span> Odtwórz
                        </button>
                        <button onclick="pauseAudio()">
                            <span>⏸️</span> Pauza
                        </button>
                        <button onclick="stopAudio()">
                            <span>⏹️</span> Stop
                        </button>
                        <button onclick="testAudioSources()">
                            <span>🔍</span> Testuj źródła
                        </button>
                        <button onclick="downloadAudio()">
                            <span>⬇️</span> Pobierz
                        </button>
                    </div>
                </div>
            </div>
            
            <!-- Sekcja z URL -->
            <div class="url-section">
                <h3>📎 Bezpośrednie linki do pliku MP3</h3>
                
                <div class="url-box">
                    <strong>GitHub Pages:</strong><br>
                    <code id="url1">https://libruz.github.io/diagnozakl5ang.github.io/nagranie1.mp3</code>
                    <button class="copy-btn" onclick="copyToClipboard('url1')">Kopiuj</button>
                </div>
                
                <div class="url-box">
                    <strong>Raw GitHub:</strong><br>
                    <code id="url2">https://raw.githubusercontent.com/libruz/diagnozakl5ang.github.io/main/nagranie1.mp3</code>
                    <button class="copy-btn" onclick="copyToClipboard('url2')">Kopiuj</button>
                </div>
                
                <div class="url-box">
                    <strong>jsDelivr CDN:</strong><br>
                    <code id="url3">https://cdn.jsdelivr.net/gh/libruz/diagnozakl5ang.github.io@main/nagranie1.mp3</code>
                    <button class="copy-btn" onclick="copyToClipboard('url3')">Kopiuj</button>
                </div>
                
                <div class="test-result" id="testResult"></div>
            </div>
            
            <!-- Instrukcje -->
            <div class="file-info">
                <h3>📋 Instrukcja użycia</h3>
                <ol style="margin-left: 20px; margin-top: 10px;">
                    <li>Plik MP3 musi być dostępny publicznie w repozytorium</li>
                    <li>GitHub Pages musi być włączony w ustawieniach repozytorium</li>
                    <li>Jeśli audio nie odtwarza się, użyj przycisku "Testuj źródła"</li>
                    <li>Możesz skorzystać z bezpośrednich linków do pobrania pliku</li>
                </ol>
            </div>
        </main>
        
        <footer>
            <p>© 2024 Odtwarzacz MP3 dla diagnozakl5ang.github.io</p>
            <p>Repozytorium: <a href="https://github.com/libruz/diagnozakl5ang.github.io" target="_blank">github.com/libruz/diagnozakl5ang.github.io</a></p>
        </footer>
    </div>

    <script>
        // Podstawowe zmienne
        const audio = document.getElementById('audioPlayer');
        const currentSourceElement = document.getElementById('currentSource');
        const availabilityElement = document.getElementById('availabilityStatus');
        const statusIndicator = document.getElementById('statusIndicator');
        const statusText = document.getElementById('statusText');
        const audioWave = document.getElementById('audioWave');
        const testResultElement = document.getElementById('testResult');
        
        // Lista źródeł do testowania
        const audioSources = [
            {
                name: 'GitHub Pages',
                url: 'https://libruz.github.io/diagnozakl5ang.github.io/nagranie1.mp3'
            },
            {
                name: 'Ścieżka względna',
                url: './nagranie1.mp3'
            },
            {
                name: 'jsDelivr CDN',
                url: 'https://cdn.jsdelivr.net/gh/libruz/diagnozakl5ang.github.io@main/nagranie1.mp3'
            },
            {
                name: 'Raw GitHub',
                url: 'https://raw.githubusercontent.com/libruz/diagnozakl5ang.github.io/main/nagranie1.mp3'
            }
        ];
        
        // Inicjalizacja
        document.addEventListener('DOMContentLoaded', function() {
            updateAudioInfo();
            checkAllSources();
            
            // Ukryj wizualizację na start
            audioWave.style.display = 'none';
        });
        
        // Aktualizuj informacje o audio
        function updateAudioInfo() {
            // Aktualne źródło
            if (audio.currentSrc) {
                currentSourceElement.textContent = audio.currentSrc;
                
                // Znajdź które źródło jest używane
                const currentSource = audioSources.find(source => 
                    audio.currentSrc.includes(source.url.split('/').pop())
                );
                
                if (currentSource) {
                    currentSourceElement.innerHTML = `
                        <strong>${currentSource.name}:</strong><br>
                        <small>${audio.currentSrc}</small>
                    `;
                }
            }
            
            // Status dostępności
            if (audio.error) {
                availabilityElement.innerHTML = `<span style="color: #dc3545;">❌ Błąd: ${audio.error.message}</span>`;
                updateStatus('error', 'Błąd odtwarzania');
            } else if (audio.readyState >= 2) {
                availabilityElement.innerHTML = '<span style="color: #28a745;">✅ Dostępny</span>';
                updateStatus('ready', 'Gotowy do odtwarzania');
            } else {
                availabilityElement.innerHTML = '<span style="color: #ffc107;">⏳ Ładowanie...</span>';
                updateStatus('loading', 'Ładowanie audio...');
            }
        }
        
        // Aktualizuj status wskaźnika
        function updateStatus(status, message) {
            statusIndicator.className = `status-indicator status-${status}`;
            statusText.textContent = message;
        }
        
        // Funkcje kontroli audio
        function playAudio() {
            audio.play().then(() => {
                updateStatus('ready', 'Odtwarzanie...');
                audioWave.style.display = 'block';
            }).catch(error => {
                updateStatus('error', 'Błąd odtwarzania');
                showTestResult(`❌ Nie można odtworzyć: ${error.message}`, 'error');
            });
        }
        
        function pauseAudio() {
            audio.pause();
            updateStatus('ready', 'Wstrzymane');
            audioWave.style.display = 'none';
        }
        
        function stopAudio() {
            audio.pause();
            audio.currentTime = 0;
            updateStatus('ready', 'Zatrzymano');
            audioWave.style.display = 'none';
        }
        
        // Testowanie wszystkich źródeł
        async function testAudioSources() {
            updateStatus('loading', 'Testowanie źródeł...');
            showTestResult('🔍 Testowanie dostępności pliku MP3...', 'info');
            
            let workingSource = null;
            
            for (const source of audioSources) {
                try {
                    const response = await fetch(source.url, { method: 'HEAD' });
                    
                    if (response.ok) {
                        const contentType = response.headers.get('content-type');
                        const size = response.headers.get('content-length');
                        
                        if (contentType && (contentType.includes('audio') || contentType.includes('octet-stream'))) {
                            showTestResult(
                                `✅ <strong>${source.name}:</strong> Działa!<br>
                                 📊 Rozmiar: ${formatBytes(size)}<br>
                                 📦 Typ: ${contentType}<br>
                                 <button onclick="switchToSource('${source.url}')" style="margin-top: 5px; padding: 5px 10px; font-size: 12px;">
                                     Użyj tego źródła
                                 </button>`,
                                'success'
                            );
                            
                            if (!workingSource) {
                                workingSource = source.url;
                            }
                        } else {
                            showTestResult(
                                `⚠️ <strong>${source.name}:</strong> Nieprawidłowy typ (${contentType})`,
                                'error'
                            );
                        }
                    } else {
                        showTestResult(
                            `❌ <strong>${source.name}:</strong> Status ${response.status}`,
                            'error'
                        );
                    }
                } catch (error) {
                    showTestResult(
                        `❌ <strong>${source.name}:</strong> ${error.message}`,
                        'error'
                    );
                }
            }
            
            if (workingSource) {
                updateStatus('ready', 'Znaleziono działające źródła');
            } else {
                updateStatus('error', 'Brak dostępnych źródeł');
            }
        }
        
        // Sprawdź wszystkie źródła przy ładowaniu
        async function checkAllSources() {
            for (const source of audioSources) {
                try {
                    const response = await fetch(source.url, { method: 'HEAD' });
                    if (response.ok) {
                        // Automatycznie przełącz na pierwsze działające źródło
                        if (audio.currentSrc === '' || audio.currentSrc.includes('blob:')) {
                            audio.src = source.url;
                            audio.load();
                            break;
                        }
                    }
                } catch (error) {
                    console.log(`Source ${source.name} failed:`, error.message);
                }
            }
        }
        
        // Przełącz na określone źródło
        function switchToSource(url) {
            audio.src = url;
            audio.load();
            audio.play().catch(() => {
                // Ignoruj błąd autoodtwarzania
            });
            showTestResult(`🔄 Przełączono źródło`, 'success');
            updateAudioInfo();
        }
        
        // Pobierz plik audio
        function downloadAudio() {
            const link = document.createElement('a');
            link.href = audio.src;
            link.download = 'nagranie1.mp3';
            link.click();
        }
        
        // Kopiuj do schowka
        function copyToClipboard(elementId) {
            const element = document.getElementById(elementId);
            const text = element.textContent;
            
            navigator.clipboard.writeText(text).then(() => {
                showTestResult('✅ Skopiowano do schowka!', 'success');
            }).catch(err => {
                showTestResult('❌ Błąd kopiowania: ' + err, 'error');
            });
        }
        
        // Pomocnicze funkcje
        function showTestResult(message, type) {
            testResultElement.innerHTML = message;
            testResultElement.className = `test-result ${type}`;
        }
        
        function formatBytes(bytes) {
            if (!bytes) return '0 Bytes';
            const k = 1024;
            const sizes = ['Bytes', 'KB', 'MB', 'GB'];
            const i = Math.floor(Math.log(bytes) / Math.log(k));
            return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
        }
        
        // Event listeners dla audio
        audio.addEventListener('loadeddata', updateAudioInfo);
        audio.addEventListener('error', updateAudioInfo);
        audio.addEventListener('play', () => {
            updateStatus('ready', 'Odtwarzanie...');
            audioWave.style.display = 'block';
        });
        audio.addEventListener('pause', () => {
            updateStatus('ready', 'Wstrzymane');
            audioWave.style.display = 'none';
        });
        audio.addEventListener('ended', () => {
            updateStatus('ready', 'Zakończono odtwarzanie');
            audioWave.style.display = 'none';
        });
        audio.addEventListener('waiting', () => {
            updateStatus('loading', 'Buforowanie...');
        });
        
        // Automatyczne odtwarzanie po załadowaniu (jeśli dostępne)
        audio.addEventListener('canplay', function() {
            updateStatus('ready', 'Gotowy do odtwarzania');
            
            // Automatyczne odtwarzanie - opcjonalne
            // audio.play().catch(e => console.log('Auto-play prevented:', e));
        });
    </script>
</body>
</html>
