<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Отладка камер OnePlus 15</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
            background: #1a1a1a;
            color: #fff;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
        }
        
        .header {
            background: #2d2d2d;
            padding: 20px;
            text-align: center;
            border-bottom: 2px solid #4CAF50;
        }
        
        .header h1 {
            font-size: 24px;
            margin-bottom: 10px;
            color: #4CAF50;
        }
        
        .header p {
            font-size: 14px;
            color: #aaa;
        }
        
        .current-camera {
            background: #333;
            padding: 15px;
            margin: 15px;
            border-radius: 10px;
            border-left: 4px solid #2196F3;
        }
        
        .current-camera h3 {
            color: #2196F3;
            margin-bottom: 10px;
            font-size: 16px;
        }
        
        .current-camera .info {
            font-size: 18px;
            font-weight: bold;
            word-break: break-word;
            color: #fff;
        }
        
        .current-camera .details {
            font-size: 14px;
            color: #aaa;
            margin-top: 5px;
        }
        
        .camera-list {
            background: #333;
            padding: 15px;
            margin: 15px;
            border-radius: 10px;
        }
        
        .camera-list h3 {
            color: #4CAF50;
            margin-bottom: 15px;
            font-size: 16px;
        }
        
        .camera-item {
            background: #3d3d3d;
            margin-bottom: 10px;
            padding: 15px;
            border-radius: 8px;
            border: 2px solid transparent;
            transition: all 0.3s;
        }
        
        .camera-item.selected {
            border-color: #4CAF50;
            background: #2d4a2d;
        }
        
        .camera-item button {
            width: 100%;
            background: none;
            border: none;
            color: inherit;
            text-align: left;
            cursor: pointer;
        }
        
        .camera-index {
            font-size: 12px;
            color: #888;
            margin-bottom: 5px;
        }
        
        .camera-label {
            font-size: 16px;
            font-weight: bold;
            margin-bottom: 8px;
            color: #4CAF50;
            word-break: break-word;
        }
        
        .camera-id {
            font-size: 12px;
            color: #888;
            word-break: break-word;
            margin-bottom: 8px;
            font-family: monospace;
        }
        
        .camera-badge {
            display: inline-block;
            padding: 4px 8px;
            border-radius: 4px;
            font-size: 12px;
            font-weight: bold;
            margin-right: 5px;
            margin-bottom: 5px;
        }
        
        .badge-wide {
            background: #4CAF50;
            color: white;
        }
        
        .badge-ultrawide {
            background: #2196F3;
            color: white;
        }
        
        .badge-macro {
            background: #f44336;
            color: white;
        }
        
        .badge-tele {
            background: #FF9800;
            color: white;
        }
        
        .badge-main {
            background: #9C27B0;
            color: white;
        }
        
        .badge-back {
            background: #00BCD4;
            color: white;
        }
        
        .preview-section {
            background: #333;
            padding: 15px;
            margin: 15px;
            border-radius: 10px;
        }
        
        .preview-section h3 {
            color: #FF9800;
            margin-bottom: 15px;
            font-size: 16px;
        }
        
        .video-container {
            position: relative;
            width: 100%;
            max-width: 400px;
            margin: 0 auto;
            background: #000;
            border-radius: 10px;
            overflow: hidden;
        }
        
        #previewVideo {
            width: 100%;
            height: auto;
            display: block;
        }
        
        .scan-frame {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 80%;
            height: 150px;
            border: 3px solid #4CAF50;
            border-radius: 10px;
            pointer-events: none;
        }
        
        .controls {
            display: flex;
            gap: 10px;
            margin: 15px;
            flex-wrap: wrap;
        }
        
        .btn {
            flex: 1;
            min-width: 120px;
            padding: 15px;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .btn-primary {
            background: #4CAF50;
            color: white;
        }
        
        .btn-secondary {
            background: #2196F3;
            color: white;
        }
        
        .btn-danger {
            background: #f44336;
            color: white;
        }
        
        .btn:active {
            transform: scale(0.95);
        }
        
        .log-panel {
            background: #222;
            padding: 15px;
            margin: 15px;
            border-radius: 10px;
            max-height: 200px;
            overflow-y: auto;
            font-family: monospace;
            font-size: 12px;
        }
        
        .log-entry {
            padding: 3px 0;
            border-bottom: 1px solid #333;
            color: #0f0;
        }
        
        .log-error {
            color: #f44336;
        }
        
        .log-warning {
            color: #FF9800;
        }
        
        .status-bar {
            background: #2d2d2d;
            padding: 10px 15px;
            margin: 0 15px 15px;
            border-radius: 5px;
            font-size: 14px;
            border-left: 4px solid #4CAF50;
        }
        
        .recommendation {
            background: #1e3a1e;
            padding: 15px;
            margin: 15px;
            border-radius: 10px;
            border: 2px solid #4CAF50;
        }
        
        .recommendation h4 {
            color: #4CAF50;
            margin-bottom: 10px;
        }
        
        .recommendation .camera-id {
            color: #fff;
            background: #2d2d2d;
            padding: 10px;
            border-radius: 5px;
            font-family: monospace;
            word-break: break-all;
            margin: 10px 0;
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>📷 Отладка камер OnePlus 15</h1>
        <p>Нажмите на камеру для теста, найдите широкоугольную</p>
    </div>

    <div class="current-camera" id="currentCamera">
        <h3>Текущая камера:</h3>
        <div class="info" id="currentCameraLabel">Не выбрана</div>
        <div class="details" id="currentCameraId"></div>
        <div class="details" id="currentCameraSettings"></div>
    </div>

    <div class="status-bar" id="statusBar">
        ⏳ Загрузка списка камер...
    </div>

    <div class="camera-list" id="cameraList">
        <h3>Доступные камеры:</h3>
        <div id="camerasContainer">
            <p style="color: #666; text-align: center;">Загрузка...</p>
        </div>
    </div>

    <div class="preview-section" id="previewSection" style="display: none;">
        <h3>Предпросмотр:</h3>
        <div class="video-container">
            <video id="previewVideo" playsinline autoplay></video>
            <div class="scan-frame"></div>
        </div>
        <div style="display: flex; gap: 10px; margin-top: 15px;">
            <button class="btn btn-secondary" id="testBarcodeBtn">🧪 Тест штрихкода</button>
            <button class="btn btn-danger" id="stopPreviewBtn">⏹️ Остановить</button>
        </div>
    </div>

    <div class="controls">
        <button class="btn btn-primary" id="refreshCamerasBtn">🔄 Обновить список</button>
        <button class="btn btn-secondary" id="recommendBtn">🔍 Найти лучшую камеру</button>
        <button class="btn btn-danger" id="clearLogBtn">🗑️ Очистить лог</button>
    </div>

    <div class="log-panel" id="logPanel">
        <div class="log-entry">=== Отладчик камер запущен ===</div>
    </div>

    <div class="recommendation" id="recommendation" style="display: none;">
        <h4>✅ Рекомендуемая камера для OnePlus 15:</h4>
        <p id="recommendText"></p>
        <div class="camera-id" id="recommendDeviceId"></div>
        <button class="btn btn-primary" id="copyDeviceIdBtn" style="width: 100%; margin-top: 10px;">📋 Скопировать ID камеры</button>
    </div>

    <script>
        let currentStream = null;
        let testBarcodeDetector = null;
        let currentDeviceId = null;
        let cameras = [];

        // Функция для добавления лога
        function addLog(message, type = 'info') {
            const logPanel = document.getElementById('logPanel');
            const logEntry = document.createElement('div');
            logEntry.className = 'log-entry';
            if (type === 'error') logEntry.classList.add('log-error');
            if (type === 'warning') logEntry.classList.add('log-warning');
            
            const time = new Date().toLocaleTimeString('ru-RU', { hour12: false });
            logEntry.textContent = `[${time}] ${message}`;
            
            logPanel.appendChild(logEntry);
            logPanel.scrollTop = logPanel.scrollHeight;
        }

        // Обновление статуса
        function setStatus(message, type = 'info') {
            const statusBar = document.getElementById('statusBar');
            statusBar.textContent = message;
            statusBar.style.borderLeftColor = type === 'error' ? '#f44336' : 
                                            type === 'warning' ? '#FF9800' : '#4CAF50';
            addLog(message, type);
        }

        // Остановка текущего стрима
        function stopCurrentStream() {
            if (currentStream) {
                currentStream.getTracks().forEach(track => track.stop());
                currentStream = null;
            }
            document.getElementById('previewSection').style.display = 'none';
            document.getElementById('previewVideo').srcObject = null;
        }

        // Определение типа камеры по названию
        function getCameraType(label) {
            const types = [];
            const l = label.toLowerCase();
            
            if (l.includes('ultrawide') || l.includes('ultra wide') || l.includes('сверхширок') || l.includes('0.6')) {
                types.push({ type: 'ultrawide', text: 'СВЕРХШИРОКАЯ' });
            }
            if (l.includes('wide') || l.includes('широко')) {
                types.push({ type: 'wide', text: 'ШИРОКАЯ' });
            }
            if (l.includes('macro') || l.includes('макро')) {
                types.push({ type: 'macro', text: 'МАКРО' });
            }
            if (l.includes('tele') || l.includes('теле')) {
                types.push({ type: 'tele', text: 'ТЕЛЕФОТО' });
            }
            if (l.includes('back') || l.includes('задняя')) {
                types.push({ type: 'back', text: 'ЗАДНЯЯ' });
            }
            if (l.includes('front') || l.includes('фронт') || l.includes('передняя')) {
                types.push({ type: 'front', text: 'ФРОНТАЛЬНАЯ' });
            }
            if (l.includes('main') || l.includes('основная')) {
                types.push({ type: 'main', text: 'ОСНОВНАЯ' });
            }
            
            return types;
        }

        // Получение бейджей для камеры
        function getCameraBadges(label) {
            const types = getCameraType(label);
            return types.map(t => 
                `<span class="camera-badge badge-${t.type}">${t.text}</span>`
            ).join('');
        }

        // Загрузка списка камер
        async function loadCameras() {
            try {
                setStatus('🔄 Запрашиваю разрешение на камеру...');
                
                // Сначала запрашиваем разрешение
                const tempStream = await navigator.mediaDevices.getUserMedia({ video: true });
                tempStream.getTracks().forEach(track => track.stop());
                
                setStatus('✅ Разрешение получено, загружаю список камер...');
                
                const devices = await navigator.mediaDevices.enumerateDevices();
                cameras = devices.filter(device => device.kind === 'videoinput');
                
                const container = document.getElementById('camerasContainer');
                container.innerHTML = '';
                
                if (cameras.length === 0) {
                    container.innerHTML = '<p style="color: #f44336;">❌ Камеры не найдены</p>';
                    setStatus('❌ Камеры не найдены', 'error');
                    return;
                }
                
                setStatus(`✅ Найдено камер: ${cameras.length}`);
                
                cameras.forEach((camera, index) => {
                    const div = document.createElement('div');
                    div.className = 'camera-item';
                    div.id = `camera-${index}`;
                    
                    const badges = getCameraBadges(camera.label);
                    
                    div.innerHTML = `
                        <button onclick="selectCamera('${camera.deviceId}', ${index})">
                            <div class="camera-index">Камера ${index}</div>
                            <div class="camera-label">${camera.label || 'Без названия'}</div>
                            <div class="camera-id">ID: ${camera.deviceId.substring(0, 50)}...</div>
                            <div>${badges || '<span style="color: #888;">Тип не определен</span>'}</div>
                        </button>
                    `;
                    
                    container.appendChild(div);
                });
                
                addLog('📸 Список камер загружен');
                
            } catch (error) {
                setStatus(`❌ Ошибка: ${error.message}`, 'error');
                addLog(`Ошибка загрузки камер: ${error.message}`, 'error');
            }
        }

        // Выбор камеры для теста
        async function selectCamera(deviceId, index) {
            try {
                stopCurrentStream();
                
                // Подсветка выбранной камеры
                document.querySelectorAll('.camera-item').forEach(item => {
                    item.classList.remove('selected');
                });
                document.getElementById(`camera-${index}`).classList.add('selected');
                
                currentDeviceId = deviceId;
                
                const camera = cameras.find(c => c.deviceId === deviceId);
                
                document.getElementById('currentCameraLabel').textContent = camera.label || 'Без названия';
                document.getElementById('currentCameraId').textContent = `ID: ${deviceId}`;
                
                setStatus(`🎥 Тестирую камеру ${index}: ${camera.label || 'без названия'}`);
                
                // Запускаем предпросмотр
                const stream = await navigator.mediaDevices.getUserMedia({
                    video: {
                        deviceId: { exact: deviceId },
                        width: { ideal: 1280 },
                        height: { ideal: 720 }
                    }
                });
                
                currentStream = stream;
                
                const video = document.getElementById('previewVideo');
                video.srcObject = stream;
                
                document.getElementById('previewSection').style.display = 'block';
                
                // Получаем информацию о треке
                const track = stream.getVideoTracks()[0];
                const settings = track.getSettings();
                const capabilities = track.getCapabilities ? track.getCapabilities() : {};
                
                let settingsText = '';
                settingsText += `📏 Разрешение: ${settings.width}x${settings.height}`;
                if (settings.focalLength) {
                    settingsText += ` | 📐 Фокусное: ${settings.focalLength}mm`;
                    if (settings.focalLength < 5) {
                        settingsText += ' ✅ ШИРОКИЙ УГОЛ';
                    }
                }
                if (capabilities.zoom) {
                    settingsText += ` | 🔍 Зум: от ${capabilities.zoom.min} до ${capabilities.zoom.max}`;
                }
                
                document.getElementById('currentCameraSettings').textContent = settingsText;
                
                addLog(`📷 Тест камеры ${index}: ${settingsText}`);
                
                // Проверяем, похожа ли на широкоугольную
                const label = camera.label.toLowerCase();
                if (label.includes('ultrawide') || label.includes('сверхширок') || 
                    label.includes('0.6') || (settings.focalLength && settings.focalLength < 5)) {
                    addLog(`✅ ЭТО ПОХОЖЕ НА ШИРОКОУГОЛЬНУЮ КАМЕРУ!`, 'warning');
                }
                
            } catch (error) {
                setStatus(`❌ Ошибка при тесте камеры: ${error.message}`, 'error');
                addLog(`Ошибка: ${error.message}`, 'error');
            }
        }

        // Тест штрихкода
        async function testBarcode() {
            if (!currentStream) {
                alert('Сначала выберите камеру');
                return;
            }
            
            try {
                addLog('🔍 Инициализация детектора штрихкодов...');
                
                if (!('BarcodeDetector' in window)) {
                    addLog('❌ BarcodeDetector не поддерживается', 'error');
                    alert('BarcodeDetector не поддерживается в этом браузере');
                    return;
                }
                
                const formats = await BarcodeDetector.getSupportedFormats();
                testBarcodeDetector = new BarcodeDetector({ 
                    formats: formats.filter(f => 
                        ['ean_13', 'ean_8', 'code_128', 'code_39'].includes(f)
                    )
                });
                
                addLog(`✅ Детектор создан, форматы: ${formats.join(', ')}`);
                
                const video = document.getElementById('previewVideo');
                const canvas = document.createElement('canvas');
                const context = canvas.getContext('2d');
                
                let detected = false;
                
                const checkBarcode = setInterval(async () => {
                    if (video.readyState === video.HAVE_ENOUGH_DATA && !detected) {
                        canvas.width = video.videoWidth;
                        canvas.height = video.videoHeight;
                        
                        context.drawImage(video, 0, 0, canvas.width, canvas.height);
                        
                        try {
                            const barcodes = await testBarcodeDetector.detect(canvas);
                            
                            if (barcodes.length > 0) {
                                detected = true;
                                clearInterval(checkBarcode);
                                addLog(`✅ ШТРИХКОД НАЙДЕН: ${barcodes[0].rawValue}`, 'warning');
                            }
                        } catch (e) {
                            console.log('Ошибка детекции:', e);
                        }
                    }
                }, 500);
                
                setTimeout(() => {
                    if (!detected) {
                        clearInterval(checkBarcode);
                        addLog('⏰ Штрихкод не найден за 10 секунд', 'warning');
                    }
                }, 10000);
                
            } catch (error) {
                addLog(`❌ Ошибка теста штрихкода: ${error.message}`, 'error');
            }
        }

        // Поиск лучшей камеры
        function recommendCamera() {
            addLog('🔍 Поиск лучшей камеры для OnePlus 15...');
            
            let bestCamera = null;
            let bestScore = -1;
            
            cameras.forEach(camera => {
                const label = camera.label.toLowerCase();
                let score = 0;
                
                // Широкоугольная - наилучший выбор
                if (label.includes('ultrawide') || label.includes('сверхширок') || label.includes('0.6')) {
                    score += 100;
                }
                if (label.includes('wide') || label.includes('широко')) {
                    score += 80;
                }
                // Основная задняя
                if (label.includes('back') || label.includes('задняя')) {
                    score += 50;
                }
                if (label.includes('main') || label.includes('основная')) {
                    score += 40;
                }
                
                // Штрафы
                if (label.includes('macro') || label.includes('макро')) {
                    score -= 100;
                }
                if (label.includes('tele') || label.includes('теле')) {
                    score -= 50;
                }
                if (label.includes('front') || label.includes('фронт')) {
                    score -= 200;
                }
                
                if (score > bestScore) {
                    bestScore = score;
                    bestCamera = camera;
                }
            });
            
            if (bestCamera) {
                const recDiv = document.getElementById('recommendation');
                const recText = document.getElementById('recommendText');
                const recId = document.getElementById('recommendDeviceId');
                
                const badges = getCameraBadges(bestCamera.label);
                
                recText.innerHTML = `
                    <strong>${bestCamera.label || 'Без названия'}</strong><br>
                    ${badges}<br>
                    Оценка: ${bestScore}
                `;
                recId.textContent = bestCamera.deviceId;
                recDiv.style.display = 'block';
                
                addLog(`✅ Рекомендуемая камера найдена с оценкой ${bestScore}`, 'warning');
            } else {
                addLog('❌ Не удалось определить лучшую камеру', 'error');
            }
        }

        // Инициализация
        async function init() {
            addLog('🚀 Отладчик камер запущен');
            
            // Загружаем камеры
            await loadCameras();
            
            // Проверяем поддержку BarcodeDetector
            if (!('BarcodeDetector' in window)) {
                addLog('⚠️ BarcodeDetector не поддерживается, тест штрихкодов недоступен', 'warning');
            }
        }

        // Обработчики событий
        document.getElementById('refreshCamerasBtn').addEventListener('click', () => {
            stopCurrentStream();
            loadCameras();
        });

        document.getElementById('testBarcodeBtn').addEventListener('click', testBarcode);

        document.getElementById('stopPreviewBtn').addEventListener('click', () => {
            stopCurrentStream();
            addLog('⏹️ Предпросмотр остановлен');
        });

        document.getElementById('clearLogBtn').addEventListener('click', () => {
            document.getElementById('logPanel').innerHTML = '<div class="log-entry">=== Лог очищен ===</div>';
        });

        document.getElementById('recommendBtn').addEventListener('click', recommendCamera);

        document.getElementById('copyDeviceIdBtn').addEventListener('click', () => {
            const deviceId = document.getElementById('recommendDeviceId').textContent;
            navigator.clipboard.writeText(deviceId).then(() => {
                addLog('📋 ID камеры скопирован');
                alert('ID камеры скопирован в буфер обмена');
            }).catch(() => {
                alert('Не удалось скопировать');
            });
        });

        // Запуск
        init();
    </script>
</body>
</html>