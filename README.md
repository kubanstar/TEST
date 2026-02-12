<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
    <title>Xprinter iOS - Печать ценников</title>
    
    <!-- PWA Manifest -->
    <link rel="manifest" href="manifest.json">
    
    <!-- iOS specific meta tags -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Xprinter">
    <meta name="format-detection" content="telephone=no">
    
    <!-- iOS Icons -->
    <link rel="apple-touch-icon" href="icon-192.png">
    <link rel="apple-touch-startup-image" href="icon-512.png">
    
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            background: #f5f5f5;
            padding: 20px;
            padding-bottom: 40px;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        
        .container {
            max-width: 600px;
            width: 100%;
            background: white;
            border-radius: 20px;
            padding: 25px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.1);
        }
        
        h1 {
            font-size: 24px;
            font-weight: 600;
            color: #333;
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .install-prompt {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 20px;
            border-radius: 16px;
            margin-bottom: 25px;
            text-align: center;
            box-shadow: 0 10px 30px rgba(102,126,234,0.3);
            display: none;
        }
        
        .install-prompt.show {
            display: block;
            animation: slideIn 0.5s ease-out;
        }
        
        .install-prompt h3 {
            font-size: 20px;
            margin-bottom: 10px;
        }
        
        .install-prompt p {
            font-size: 16px;
            margin-bottom: 20px;
            opacity: 0.95;
        }
        
        .install-btn {
            background: white;
            color: #667eea;
            border: none;
            padding: 12px 30px;
            border-radius: 30px;
            font-size: 18px;
            font-weight: 600;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
            transition: transform 0.2s;
        }
        
        .install-btn:active {
            transform: scale(0.95);
        }
        
        .bluetooth-status {
            background: #f8f9fa;
            border-radius: 16px;
            padding: 20px;
            margin-bottom: 20px;
            border-left: 4px solid #4CAF50;
        }
        
        .status-title {
            font-size: 14px;
            color: #666;
            margin-bottom: 8px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        
        .status-value {
            font-size: 18px;
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .printer-card {
            background: #fff;
            border: 2px solid #e0e0e0;
            border-radius: 16px;
            padding: 20px;
            margin-bottom: 20px;
            transition: all 0.3s;
        }
        
        .printer-card.connected {
            border-color: #4CAF50;
            background: #f1f8e9;
        }
        
        .printer-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }
        
        .printer-name {
            font-size: 18px;
            font-weight: 600;
            color: #333;
        }
        
        .printer-status-badge {
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
        }
        
        .badge-disconnected {
            background: #ffebee;
            color: #f44336;
        }
        
        .badge-connected {
            background: #e8f5e9;
            color: #4CAF50;
        }
        
        .printer-details {
            font-size: 14px;
            color: #666;
            margin-top: 10px;
            padding-top: 10px;
            border-top: 1px solid #eee;
        }
        
        .btn {
            width: 100%;
            padding: 16px;
            border: none;
            border-radius: 12px;
            font-size: 18px;
            font-weight: 600;
            margin-bottom: 12px;
            cursor: pointer;
            transition: all 0.2s;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }
        
        .btn:active {
            transform: scale(0.98);
        }
        
        .btn-primary {
            background: #4CAF50;
            color: white;
            box-shadow: 0 4px 15px rgba(76,175,80,0.3);
        }
        
        .btn-secondary {
            background: #2196F3;
            color: white;
            box-shadow: 0 4px 15px rgba(33,150,243,0.3);
        }
        
        .btn-warning {
            background: #FF9800;
            color: white;
            box-shadow: 0 4px 15px rgba(255,152,0,0.3);
        }
        
        .btn:disabled {
            opacity: 0.5;
            transform: none;
            box-shadow: none;
            cursor: not-allowed;
        }
        
        .product-card {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border-radius: 20px;
            padding: 25px;
            margin-bottom: 20px;
            color: white;
        }
        
        .product-article {
            font-size: 14px;
            opacity: 0.9;
            margin-bottom: 8px;
        }
        
        .product-name {
            font-size: 20px;
            font-weight: 600;
            margin-bottom: 20px;
            line-height: 1.4;
        }
        
        .product-price-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 0;
            border-bottom: 1px solid rgba(255,255,255,0.2);
        }
        
        .product-price-row:last-child {
            border-bottom: none;
        }
        
        .price-label {
            font-size: 16px;
            opacity: 0.95;
        }
        
        .price-value {
            font-size: 24px;
            font-weight: 700;
        }
        
        .debug-panel {
            margin-top: 20px;
            padding: 15px;
            background: #1e1e1e;
            border-radius: 12px;
            display: none;
        }
        
        .debug-panel.show {
            display: block;
        }
        
        .debug-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
            color: #00ff00;
            font-family: monospace;
        }
        
        .debug-content {
            background: #2d2d2d;
            padding: 15px;
            border-radius: 8px;
            max-height: 300px;
            overflow-y: auto;
            font-family: monospace;
            font-size: 12px;
            color: #00ff00;
        }
        
        .debug-line {
            margin: 4px 0;
            word-break: break-all;
        }
        
        .toast {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%) translateY(-100%);
            background: #333;
            color: white;
            padding: 12px 24px;
            border-radius: 30px;
            font-weight: 500;
            box-shadow: 0 4px 15px rgba(0,0,0,0.3);
            z-index: 1000;
            transition: transform 0.3s;
            max-width: 90%;
            text-align: center;
        }
        
        .toast.show {
            transform: translateX(-50%) translateY(0);
        }
        
        .toast.success {
            background: #4CAF50;
        }
        
        .toast.error {
            background: #f44336;
        }
        
        @keyframes slideIn {
            from {
                transform: translateY(-20px);
                opacity: 0;
            }
            to {
                transform: translateY(0);
                opacity: 1;
            }
        }
        
        @media (prefers-color-scheme: dark) {
            body { background: #1a1a1a; }
            .container { background: #2d2d2d; }
            h1 { color: #fff; }
            .bluetooth-status { background: #333; }
            .status-title { color: #aaa; }
            .printer-card { background: #333; border-color: #444; }
            .printer-name { color: #fff; }
            .printer-details { color: #aaa; border-top-color: #444; }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>
            <span>🖨️ Xprinter XP-P323B</span>
        </h1>
        
        <!-- PWA Install Prompt -->
        <div class="install-prompt" id="installPrompt">
            <h3>📱 Добавьте на главный экран</h3>
            <p>Для работы Bluetooth на iOS необходимо добавить сайт на главный экран</p>
            <button class="install-btn" id="installPwaBtn">
                Нажмите: Поделиться → На экран «Домой»
            </button>
        </div>
        
        <!-- Bluetooth Status -->
        <div class="bluetooth-status">
            <div class="status-title">Состояние Bluetooth</div>
            <div class="status-value" id="btStatus">
                ⚡ Проверка...
            </div>
        </div>
        
        <!-- PWA Status -->
        <div class="bluetooth-status" style="border-left-color: #FF9800;" id="pwaStatus">
            <div class="status-title">Режим работы</div>
            <div class="status-value" id="pwaMode">
                🌐 Обычный режим
            </div>
        </div>
        
        <!-- Printer Card -->
        <div class="printer-card" id="printerCard">
            <div class="printer-header">
                <span class="printer-name">Xprinter XP-P323B</span>
                <span class="printer-status-badge badge-disconnected" id="printerBadge">
                    ❌ Не подключен
                </span>
            </div>
            <div class="printer-details" id="printerDetails">
                Статус: ожидание подключения
            </div>
        </div>
        
        <!-- Actions -->
        <button class="btn btn-primary" id="connectBtn">
            🔍 Найти и подключить принтер
        </button>
        
        <!-- Product Card -->
        <div class="product-card">
            <div class="product-article" id="productArticle">Артикул: 620-107K</div>
            <div class="product-name" id="productName">
                Портмоне + зажим "SOMUCH" мат, цв: черный
            </div>
            <div class="product-price-row">
                <span class="price-label">Розничная цена</span>
                <span class="price-value" id="retailPrice">750 ₽</span>
            </div>
            <div class="product-price-row">
                <span class="price-label">Оптовая цена</span>
                <span class="price-value" id="wholesalePrice">570 ₽</span>
            </div>
        </div>
        
        <!-- Print Button -->
        <button class="btn btn-secondary" id="printBtn" disabled>
            🖨️ Напечатать ценник
        </button>
        
        <!-- Test Button -->
        <button class="btn btn-warning" id="testBtn">
            🧪 Проверить Bluetooth
        </button>
        
        <!-- Debug Toggle -->
        <button class="btn" id="debugBtn" style="background: #607D8B; color: white;">
            📋 Показать отладку
        </button>
        
        <!-- Debug Panel -->
        <div class="debug-panel" id="debugPanel">
            <div class="debug-header">
                <span>📋 Отладочная информация</span>
                <span style="cursor: pointer;" onclick="clearDebug()">🗑️</span>
            </div>
            <div class="debug-content" id="debugContent"></div>
        </div>
    </div>
    
    <script>
        // ========== ГЛОБАЛЬНЫЕ ПЕРЕМЕННЫЕ ==========
        let bluetoothDevice = null;
        let bluetoothCharacteristic = null;
        let isConnected = false;
        let debugEnabled = false;
        let isStandalone = false;
        
        // UUID для Xprinter (точные как в нативном приложении)
        const PRINTER_SERVICE_UUID = '0000ff00-0000-1000-8000-00805f9b34fb';
        const PRINTER_CHAR_UUID = '0000ff01-0000-1000-8000-00805f9b34fb';
        
        // Альтернативные UUID
        const ALTERNATIVE_UUIDS = {
            services: [
                '0000ff00-0000-1000-8000-00805f9b34fb',
                '000018f0-0000-1000-8000-00805f9b34fb',
                '6e400001-b5a3-f393-e0a9-e50e24dcca9e',
                '0000ae30-0000-1000-8000-00805f9b34fb'
            ],
            characteristics: [
                '0000ff01-0000-1000-8000-00805f9b34fb',
                '6e400002-b5a3-f393-e0a9-e50e24dcca9e',
                '0000ae01-0000-1000-8000-00805f9b34fb'
            ]
        };
        
        // Тестовый товар
        const currentProduct = {
            article: '620-107K',
            name: 'Портмоне + зажим "SOMUCH" мат, цв: черный',
            wholesale: '570',
            retail: '750'
        };
        
        // ========== PWA / STANDALONE DETECTION ==========
        function checkIfStandalone() {
            isStandalone = window.navigator.standalone === true || 
                          window.matchMedia('(display-mode: standalone)').matches;
            
            const pwaModeEl = document.getElementById('pwaMode');
            const installPrompt = document.getElementById('installPrompt');
            
            if (isStandalone) {
                pwaModeEl.innerHTML = '📱 PWA режим (с главного экрана) ✅';
                pwaModeEl.style.color = '#4CAF50';
                installPrompt.classList.remove('show');
                debug('✅ PWA режим активирован - Bluetooth должен работать');
            } else {
                pwaModeEl.innerHTML = '🌐 Обычный браузер - ❗ НУЖНО ДОБАВИТЬ НА ГЛАВНЫЙ ЭКРАН';
                pwaModeEl.style.color = '#FF9800';
                installPrompt.classList.add('show');
                debug('⚠️ Не PWA режим - Bluetooth может не работать');
            }
        }
        
        // ========== ФУНКЦИИ ОТЛАДКИ ==========
        function debug(message) {
            console.log(message);
            if (!debugEnabled) return;
            
            const content = document.getElementById('debugContent');
            const line = document.createElement('div');
            line.className = 'debug-line';
            line.textContent = `[${new Date().toLocaleTimeString()}] ${message}`;
            content.appendChild(line);
            content.scrollTop = content.scrollHeight;
        }
        
        function clearDebug() {
            document.getElementById('debugContent').innerHTML = '';
        }
        
        // ========== УВЕДОМЛЕНИЯ ==========
        function showToast(message, type = 'info') {
            const toast = document.createElement('div');
            toast.className = `toast ${type}`;
            toast.textContent = message;
            document.body.appendChild(toast);
            
            setTimeout(() => toast.classList.add('show'), 10);
            setTimeout(() => {
                toast.classList.remove('show');
                setTimeout(() => toast.remove(), 300);
            }, 3000);
        }
        
        // ========== ПРОВЕРКА BLUETOOTH ==========
        async function checkBluetooth() {
            const btStatus = document.getElementById('btStatus');
            
            if (!navigator.bluetooth) {
                btStatus.innerHTML = '❌ Web Bluetooth НЕ ПОДДЕРЖИВАЕТСЯ';
                btStatus.style.color = '#f44336';
                debug('❌ Web Bluetooth не поддерживается');
                return false;
            }
            
            btStatus.innerHTML = '✅ Web Bluetooth поддерживается';
            btStatus.style.color = '#4CAF50';
            debug('✅ Web Bluetooth поддерживается');
            
            // Проверяем сохраненные устройства
            try {
                const devices = await navigator.bluetooth.getDevices();
                debug(`📱 Сохраненных устройств: ${devices.length}`);
                
                if (devices.length > 0) {
                    devices.forEach((d, i) => {
                        debug(`   ${i+1}. ${d.name || 'Без имени'} (${d.id})`);
                    });
                    
                    // Ищем сохраненный принтер
                    const savedId = localStorage.getItem('xprinter_device_id');
                    if (savedId) {
                        const savedDevice = devices.find(d => d.id === savedId);
                        if (savedDevice) {
                            debug(`✅ Найден сохраненный принтер: ${savedDevice.name}`);
                            bluetoothDevice = savedDevice;
                        }
                    }
                }
            } catch (e) {
                debug(`⚠️ Ошибка получения устройств: ${e.message}`);
            }
            
            return true;
        }
        
        // ========== ПОДКЛЮЧЕНИЕ К ПРИНТЕРУ ==========
        async function connectPrinter() {
            debug('🚀 Начинаем поиск принтера...');
            
            try {
                if (!navigator.bluetooth) {
                    throw new Error('Web Bluetooth не поддерживается');
                }
                
                if (!isStandalone) {
                    const confirm = window.confirm(
                        'Для работы Bluetooth необходимо добавить сайт на главный экран.\n\n' +
                        '1. Нажмите кнопку "Поделиться" (⎙)\n' +
                        '2. Выберите "На экран «Домой»"\n' +
                        '3. Запустите приложение с главного экрана\n\n' +
                        'Продолжить поиск в браузере?'
                    );
                    if (!confirm) return;
                }
                
                showToast('🔍 Ищем принтеры...', 'info');
                
                const device = await navigator.bluetooth.requestDevice({
                    acceptAllDevices: true,
                    optionalServices: ALTERNATIVE_UUIDS.services
                });
                
                debug(`✅ Выбрано устройство: ${device.name || 'Без имени'}`);
                debug(`   ID: ${device.id}`);
                
                bluetoothDevice = device;
                
                // Сохраняем устройство
                try {
                    localStorage.setItem('xprinter_device_id', device.id);
                    localStorage.setItem('xprinter_device_name', device.name || 'Xprinter');
                    debug('💾 Устройство сохранено');
                } catch (e) {
                    debug(`⚠️ Ошибка сохранения: ${e.message}`);
                }
                
                await connectToDevice(device);
                
            } catch (error) {
                debug(`❌ Ошибка: ${error.message}`);
                
                if (error.message.includes('User cancelled')) {
                    showToast('❌ Поиск отменен', 'error');
                } else {
                    showToast(`❌ ${error.message}`, 'error');
                }
            }
        }
        
        async function connectToDevice(device) {
            try {
                debug('🔄 Подключение к GATT серверу...');
                showToast('🔄 Подключение к принтеру...', 'info');
                
                device.addEventListener('gattserverdisconnected', onDisconnected);
                
                const server = await device.gatt.connect();
                debug('✅ GATT сервер подключен');
                
                // Ищем сервис
                let service = null;
                for (const uuid of ALTERNATIVE_UUIDS.services) {
                    try {
                        debug(`   Пробуем сервис: ${uuid}`);
                        service = await server.getPrimaryService(uuid);
                        debug(`   ✅ Найден сервис: ${uuid}`);
                        break;
                    } catch (e) {
                        debug(`   ❌ Сервис не найден: ${uuid}`);
                    }
                }
                
                if (!service) {
                    throw new Error('Не найден сервис принтера');
                }
                
                // Ищем характеристику
                let characteristic = null;
                for (const uuid of ALTERNATIVE_UUIDS.characteristics) {
                    try {
                        debug(`   Пробуем характеристику: ${uuid}`);
                        characteristic = await service.getCharacteristic(uuid);
                        debug(`   ✅ Найдена характеристика: ${uuid}`);
                        break;
                    } catch (e) {
                        debug(`   ❌ Характеристика не найдена: ${uuid}`);
                    }
                }
                
                if (!characteristic) {
                    throw new Error('Не найдена характеристика для печати');
                }
                
                bluetoothCharacteristic = characteristic;
                isConnected = true;
                
                // Обновляем UI
                document.getElementById('printerCard').classList.add('connected');
                document.getElementById('printerBadge').innerHTML = '✅ Подключен';
                document.getElementById('printerBadge').className = 'printer-status-badge badge-connected';
                document.getElementById('printerDetails').innerHTML = 
                    `Подключено: ${device.name || 'Xprinter XP-P323B'}<br>ID: ${device.id.substr(0, 8)}...`;
                document.getElementById('printBtn').disabled = false;
                
                showToast('✅ Принтер подключен!', 'success');
                debug('🎉 Принтер готов к работе!');
                
            } catch (error) {
                debug(`❌ Ошибка подключения: ${error.message}`);
                showToast(`❌ ${error.message}`, 'error');
                throw error;
            }
        }
        
        function onDisconnected(event) {
            debug('🔌 Принтер отключен');
            isConnected = false;
            bluetoothCharacteristic = null;
            
            document.getElementById('printerCard').classList.remove('connected');
            document.getElementById('printerBadge').innerHTML = '❌ Не подключен';
            document.getElementById('printerBadge').className = 'printer-status-badge badge-disconnected';
            document.getElementById('printerDetails').innerHTML = 'Статус: ожидание подключения';
            document.getElementById('printBtn').disabled = true;
            
            showToast('🔌 Принтер отключен', 'error');
        }
        
        // ========== ФОРМИРОВАНИЕ КОМАНДЫ ПЕЧАТИ ==========
        function createPrintCommand() {
            debug('📄 Формирование команды печати...');
            
            const ESC = 0x1B;
            const GS = 0x1D;
            
            const textEncoder = new TextEncoder();
            let command = new Uint8Array(0);
            
            function append(bytes) {
                const newArray = new Uint8Array(command.length + bytes.length);
                newArray.set(command, 0);
                newArray.set(bytes, command.length);
                command = newArray;
            }
            
            // Инициализация
            append(new Uint8Array([ESC, 0x40]));
            
            // Центрирование
            append(new Uint8Array([ESC, 0x61, 0x01]));
            
            // Жирный текст для артикула
            append(new Uint8Array([ESC, 0x45, 0x01]));
            append(textEncoder.encode(currentProduct.article + '\n'));
            append(new Uint8Array([ESC, 0x45, 0x00]));
            
            // Линия
            append(textEncoder.encode('='.repeat(32) + '\n'));
            
            // Название товара
            append(textEncoder.encode(currentProduct.name + '\n'));
            
            // Линия
            append(textEncoder.encode('='.repeat(32) + '\n'));
            
            // Цены
            append(textEncoder.encode('\n'));
            append(textEncoder.encode(`РОЗНИЦА: ${currentProduct.retail} руб.\n`));
            append(textEncoder.encode(`ОПТОВАЯ: ${currentProduct.wholesale} руб.\n`));
            append(textEncoder.encode('\n'));
            
            // Линия
            append(textEncoder.encode('='.repeat(32) + '\n'));
            
            // Дата
            const date = new Date();
            append(textEncoder.encode(date.toLocaleDateString('ru-RU') + '\n'));
            
            // Линия
            append(textEncoder.encode('='.repeat(32) + '\n'));
            
            // Магазин
            append(textEncoder.encode('ИП Мааруф Р.\n'));
            append(textEncoder.encode('\n\n'));
            
            // Отрезка
            append(new Uint8Array([GS, 0x56, 0x00]));
            
            debug(`📊 Размер команды: ${command.length} байт`);
            return command;
        }
        
        // ========== ПЕЧАТЬ ==========
        async function printLabel() {
            if (!isConnected || !bluetoothCharacteristic) {
                showToast('❌ Принтер не подключен', 'error');
                return;
            }
            
            try {
                debug('🖨️ Отправка команды печати...');
                showToast('🖨️ Печать...', 'info');
                
                const command = createPrintCommand();
                await bluetoothCharacteristic.writeValue(command);
                
                debug('✅ Команда отправлена');
                showToast('✅ Ценник отправлен на печать!', 'success');
                
            } catch (error) {
                debug(`❌ Ошибка печати: ${error.message}`);
                showToast(`❌ ${error.message}`, 'error');
            }
        }
        
        // ========== ТЕСТ BLUETOOTH ==========
        async function testBluetooth() {
            debug('🧪 ЗАПУСК ТЕСТА BLUETOOTH');
            
            // 1. Проверка поддержки
            debug(`1. Web Bluetooth: ${navigator.bluetooth ? '✅' : '❌'}`);
            
            // 2. Проверка PWA режима
            debug(`2. PWA режим: ${isStandalone ? '✅' : '❌'}`);
            
            // 3. Проверка сохраненных устройств
            try {
                const devices = await navigator.bluetooth.getDevices();
                debug(`3. Сохраненных устройств: ${devices.length}`);
                
                devices.forEach((d, i) => {
                    debug(`   ${i+1}. ${d.name || 'Без имени'} (${d.id})`);
                });
                
                if (devices.length === 0) {
                    debug('   ➡️ Нет сохраненных устройств. Нажмите "Найти принтер"');
                }
                
            } catch (e) {
                debug(`3. Ошибка: ${e.message}`);
            }
            
            // 4. Проверка текущего подключения
            debug(`4. Текущее подключение: ${isConnected ? '✅' : '❌'}`);
            
            // 5. Инструкция
            debug('\n📋 ИНСТРУКЦИЯ:');
            debug('   1. Убедитесь, что сайт добавлен на главный экран');
            debug('   2. Запустите с главного экрана');
            debug('   3. Включите Bluetooth на iPhone');
            debug('   4. Включите принтер');
            debug('   5. Нажмите "Найти принтер"');
            debug('   6. Выберите Xprinter из списка');
            
            showToast('🧪 Тест завершен, смотрите отладку', 'info');
        }
        
        // ========== ИНИЦИАЛИЗАЦИЯ ==========
        window.onload = async function() {
            // Включаем отладку
            debugEnabled = true;
            
            // Проверяем PWA режим
            checkIfStandalone();
            
            // Проверяем Bluetooth
            await checkBluetooth();
            
            // Загружаем данные товара
            document.getElementById('productArticle').innerHTML = `Артикул: ${currentProduct.article}`;
            document.getElementById('productName').innerHTML = currentProduct.name;
            document.getElementById('retailPrice').innerHTML = `${currentProduct.retail} ₽`;
            document.getElementById('wholesalePrice').innerHTML = `${currentProduct.wholesale} ₽`;
            
            debug('✅ Страница загружена');
            debug(`🌐 URL: ${window.location.href}`);
            debug(`📱 PWA: ${isStandalone ? 'Да' : 'Нет'}`);
        };
        
        // ========== ОБРАБОТЧИКИ СОБЫТИЙ ==========
        document.getElementById('connectBtn').addEventListener('click', connectPrinter);
        document.getElementById('printBtn').addEventListener('click', printLabel);
        document.getElementById('testBtn').addEventListener('click', testBluetooth);
        
        document.getElementById('debugBtn').addEventListener('click', function() {
            const panel = document.getElementById('debugPanel');
            const btn = document.getElementById('debugBtn');
            
            if (panel.classList.contains('show')) {
                panel.classList.remove('show');
                btn.innerHTML = '📋 Показать отладку';
                debugEnabled = false;
            } else {
                panel.classList.add('show');
                btn.innerHTML = '📋 Скрыть отладку';
                debugEnabled = true;
            }
        });
        
        document.getElementById('installPwaBtn').addEventListener('click', function() {
            showToast('1. Нажмите кнопку "Поделиться" (⎙)\n2. Выберите "На экран «Домой»"', 'info');
        });
        
        // Слушаем изменение режима отображения
        window.matchMedia('(display-mode: standalone)').addListener(checkIfStandalone);
    </script>
</body>
</html>