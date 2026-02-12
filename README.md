<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>iOS Печать - Xprinter XP-P323B</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            max-width: 600px;
            margin: 0 auto;
            padding: 20px;
            background: #f5f5f5;
        }
        
        .container {
            background: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        
        h1 {
            font-size: 24px;
            color: #333;
            margin-bottom: 20px;
            text-align: center;
        }
        
        .printer-card {
            background: #f8f9fa;
            border-radius: 12px;
            padding: 20px;
            margin-bottom: 20px;
            border-left: 4px solid #4CAF50;
        }
        
        .printer-name {
            font-size: 18px;
            font-weight: bold;
            color: #333;
            margin-bottom: 5px;
        }
        
        .printer-status {
            font-size: 14px;
            color: #666;
            margin-bottom: 10px;
        }
        
        .connected {
            color: #4CAF50;
            font-weight: bold;
        }
        
        .disconnected {
            color: #f44336;
            font-weight: bold;
        }
        
        .btn {
            background: #4CAF50;
            color: white;
            border: none;
            padding: 15px 25px;
            border-radius: 10px;
            font-size: 16px;
            font-weight: 600;
            width: 100%;
            margin-bottom: 10px;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .btn:active {
            transform: scale(0.98);
        }
        
        .btn.blue {
            background: #2196F3;
        }
        
        .btn:disabled {
            background: #ccc;
            cursor: not-allowed;
        }
        
        .instructions {
            background: #e3f2fd;
            padding: 15px;
            border-radius: 10px;
            margin-bottom: 20px;
            font-size: 14px;
            color: #0c5460;
        }
        
        .product-info {
            background: white;
            border: 1px solid #ddd;
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 20px;
        }
        
        .product-title {
            font-size: 16px;
            font-weight: bold;
            color: #333;
            margin-bottom: 10px;
        }
        
        .product-detail {
            display: flex;
            justify-content: space-between;
            padding: 5px 0;
            border-bottom: 1px solid #eee;
        }
        
        .price {
            color: #e74c3c;
            font-weight: bold;
            font-size: 18px;
        }
        
        .debug-log {
            background: #1e1e1e;
            color: #00ff00;
            font-family: monospace;
            padding: 15px;
            border-radius: 10px;
            font-size: 12px;
            max-height: 200px;
            overflow-y: auto;
            margin-top: 20px;
            display: none;
        }
        
        .debug-line {
            margin: 2px 0;
            word-break: break-all;
        }
        
        .warning {
            background: #fff3cd;
            border-left: 4px solid #ffc107;
            padding: 15px;
            border-radius: 5px;
            margin-bottom: 20px;
            font-size: 14px;
        }
        
        @media (prefers-color-scheme: dark) {
            body { background: #1a1a1a; }
            .container { background: #2d2d2d; }
            h1 { color: #fff; }
            .printer-card { background: #333; }
            .printer-name { color: #fff; }
            .product-info { background: #333; border-color: #444; }
            .product-title { color: #fff; }
            .product-detail { color: #ddd; border-bottom-color: #444; }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🖨️ iOS Печать Xprinter</h1>
        
        <div class="warning">
            <strong>⚠️ Важно для iOS:</strong><br>
            1. Убедитесь, что Bluetooth включен в настройках iPhone<br>
            2. Принтер Xprinter XP-P323B должен быть включен<br>
            3. Нажмите "Подключить принтер" и выберите устройство из списка<br>
            4. После подключения нажмите "Печать"
        </div>
        
        <div class="instructions" id="bluetoothStatus">
            🔵 Bluetooth: Проверка...
        </div>
        
        <div class="printer-card">
            <div class="printer-name">Xprinter XP-P323B</div>
            <div class="printer-status" id="printerStatus">
                <span class="disconnected">❌ Не подключен</span>
            </div>
            <div id="printerInfo" style="display: none; margin-top: 10px; padding: 10px; background: #e8f5e9; border-radius: 8px;">
                <span style="color: #2e7d32;">✅ Подключено к: <span id="deviceName"></span></span>
            </div>
        </div>
        
        <button class="btn blue" id="connectBtn" onclick="connectPrinter()">
            🔍 Найти и подключить принтер
        </button>
        
        <div id="productSection" style="display: none;">
            <div class="product-info">
                <div class="product-title" id="productArticle">Артикул: 620-107K</div>
                <div id="productName" style="margin-bottom: 10px;">Портмоне + зажим "SOMUCH" мат, цв: черный</div>
                <div class="product-detail">
                    <span>Оптовая:</span>
                    <span class="price" id="wholesalePrice">570 руб.</span>
                </div>
                <div class="product-detail">
                    <span>Розничная:</span>
                    <span class="price" id="retailPrice">750 руб.</span>
                </div>
            </div>
            
            <button class="btn" id="printBtn" onclick="printLabel()" disabled>
                🖨️ Печать ценника
            </button>
        </div>
        
        <button class="btn" id="testBtn" onclick="testConnection()" style="background: #9C27B0; margin-top: 10px;">
            🧪 Тест подключения
        </button>
        
        <button class="btn" id="debugBtn" onclick="toggleDebug()" style="background: #607D8B;">
            📋 Показать отладку
        </button>
        
        <div class="debug-log" id="debugLog"></div>
    </div>

    <script>
        // Переменные для Bluetooth
        let bluetoothDevice = null;
        let bluetoothCharacteristic = null;
        let isConnected = false;
        let debugEnabled = false;
        
        // UUID для Xprinter
        const PRINTER_UUIDS = {
            services: [
                '0000ff00-0000-1000-8000-00805f9b34fb',
                '000018f0-0000-1000-8000-00805f9b34fb',
                '6e400001-b5a3-f393-e0a9-e50e24dcca9e',
                '0000ae30-0000-1000-8000-00805f9b34fb'
            ],
            writeChars: [
                '0000ff01-0000-1000-8000-00805f9b34fb',
                '6e400002-b5a3-f393-e0a9-e50e24dcca9e',
                '0000ae01-0000-1000-8000-00805f9b34fb'
            ]
        };

        // Товар для печати
        let currentProduct = {
            article: '620-107K',
            name: 'Портмоне + зажим "SOMUCH" мат, цв: черный',
            wholesalePrice: '570,00',
            retailPrice: '750,00'
        };

        // Функция для отладки
        function debug(message) {
            console.log(message);
            if (!debugEnabled) return;
            
            const log = document.getElementById('debugLog');
            const line = document.createElement('div');
            line.className = 'debug-line';
            line.textContent = `[${new Date().toLocaleTimeString()}] ${message}`;
            log.appendChild(line);
            log.scrollTop = log.scrollHeight;
        }

        function toggleDebug() {
            debugEnabled = !debugEnabled;
            const log = document.getElementById('debugLog');
            const btn = document.getElementById('debugBtn');
            
            if (debugEnabled) {
                log.style.display = 'block';
                btn.textContent = '📋 Скрыть отладку';
                debug('Отладка включена');
            } else {
                log.style.display = 'none';
                btn.textContent = '📋 Показать отладку';
            }
        }

        // Проверка поддержки Web Bluetooth
        function checkBluetoothSupport() {
            const status = document.getElementById('bluetoothStatus');
            
            if (navigator.bluetooth) {
                status.innerHTML = '🔵 Bluetooth: <span style="color: #4CAF50;">✓ Поддерживается</span>';
                debug('Web Bluetooth поддерживается');
                return true;
            } else {
                status.innerHTML = '🔵 Bluetooth: <span style="color: #f44336;">✗ НЕ поддерживается</span><br>' +
                                 'Используйте Safari на iOS с HTTPS';
                debug('Web Bluetooth НЕ поддерживается');
                return false;
            }
        }

        // Подключение к принтеру
        async function connectPrinter() {
            debug('Начинаем поиск принтера...');
            
            try {
                if (!navigator.bluetooth) {
                    throw new Error('Web Bluetooth не поддерживается');
                }

                // Показываем системное окно выбора устройств
                debug('Запрашиваем устройство...');
                
                const device = await navigator.bluetooth.requestDevice({
                    acceptAllDevices: true,
                    optionalServices: PRINTER_UUIDS.services
                });

                debug(`Выбрано устройство: ${device.name || 'Без имени'}`);
                debug(`ID устройства: ${device.id}`);

                bluetoothDevice = device;
                
                // Сохраняем в localStorage
                try {
                    localStorage.setItem('savedPrinterId', device.id);
                    localStorage.setItem('savedPrinterName', device.name || 'Xprinter');
                    debug('Устройство сохранено');
                } catch (e) {
                    debug('Ошибка сохранения: ' + e.message);
                }

                // Подключаемся
                await connectToDevice(device);

            } catch (error) {
                debug('Ошибка: ' + error.message);
                
                if (error.message.includes('User cancelled')) {
                    alert('Поиск отменен. Нажмите кнопку еще раз и выберите принтер из списка.');
                } else {
                    alert('Ошибка: ' + error.message);
                }
            }
        }

        // Подключение к устройству
        async function connectToDevice(device) {
            try {
                debug('Подключение к GATT серверу...');
                
                // Добавляем обработчик отключения
                device.addEventListener('gattserverdisconnected', onDisconnected);
                
                // Подключаемся
                const server = await device.gatt.connect();
                debug('GATT сервер подключен');
                
                // Ищем сервис
                let service = null;
                for (const uuid of PRINTER_UUIDS.services) {
                    try {
                        debug(`Пробуем сервис: ${uuid}`);
                        service = await server.getPrimaryService(uuid);
                        debug(`✓ Сервис найден: ${uuid}`);
                        break;
                    } catch (e) {
                        debug(`✗ Сервис не найден: ${uuid}`);
                    }
                }

                if (!service) {
                    throw new Error('Не найден сервис принтера');
                }

                // Ищем характеристику для записи
                let characteristic = null;
                for (const uuid of PRINTER_UUIDS.writeChars) {
                    try {
                        debug(`Пробуем характеристику: ${uuid}`);
                        characteristic = await service.getCharacteristic(uuid);
                        debug(`✓ Характеристика найдена: ${uuid}`);
                        break;
                    } catch (e) {
                        debug(`✗ Характеристика не найдена: ${uuid}`);
                    }
                }

                if (!characteristic) {
                    throw new Error('Не найдена характеристика для печати');
                }

                bluetoothCharacteristic = characteristic;
                isConnected = true;
                
                // Обновляем UI
                document.getElementById('printerStatus').innerHTML = 
                    '<span class="connected">✅ Подключен</span>';
                document.getElementById('printerInfo').style.display = 'block';
                document.getElementById('deviceName').textContent = device.name || 'Xprinter XP-P323B';
                document.getElementById('productSection').style.display = 'block';
                document.getElementById('printBtn').disabled = false;
                
                debug('✓ Принтер успешно подключен!');

            } catch (error) {
                debug('Ошибка подключения: ' + error.message);
                throw error;
            }
        }

        // Обработчик отключения
        function onDisconnected(event) {
            debug('Принтер отключен');
            isConnected = false;
            bluetoothCharacteristic = null;
            
            document.getElementById('printerStatus').innerHTML = 
                '<span class="disconnected">❌ Отключен</span>';
            document.getElementById('printerInfo').style.display = 'none';
            document.getElementById('printBtn').disabled = true;
        }

        // Тест подключения
        async function testConnection() {
            debug('=== ТЕСТ ПОДКЛЮЧЕНИЯ ===');
            
            if (!navigator.bluetooth) {
                debug('✗ Web Bluetooth НЕ поддерживается');
                alert('Web Bluetooth не поддерживается. Используйте Safari.');
                return;
            }
            
            debug('✓ Web Bluetooth поддерживается');
            
            try {
                const devices = await navigator.bluetooth.getDevices();
                debug(`Сохраненных устройств: ${devices.length}`);
                
                devices.forEach((d, i) => {
                    debug(`${i + 1}. ${d.name || 'Без имени'} (${d.id})`);
                });
                
                // Проверяем сохраненный принтер
                const savedId = localStorage.getItem('savedPrinterId');
                if (savedId) {
                    debug(`Сохраненный принтер ID: ${savedId}`);
                    const savedName = localStorage.getItem('savedPrinterName');
                    debug(`Имя: ${savedName}`);
                    
                    const savedDevice = devices.find(d => d.id === savedId);
                    if (savedDevice) {
                        debug('✓ Сохраненный принтер найден');
                    } else {
                        debug('✗ Сохраненный принтер не найден');
                    }
                } else {
                    debug('Нет сохраненных принтеров');
                }
                
            } catch (e) {
                debug('Ошибка: ' + e.message);
            }
            
            debug('=== КОНЕЦ ТЕСТА ===');
        }

        // Формирование команды печати
        function createPrintCommand() {
            debug('Формирование команды печати...');
            
            // ESC/POS команды
            const ESC = 0x1B;
            const GS = 0x1D;
            
            // Инициализация
            const init = new Uint8Array([ESC, 0x40]);
            
            // Текст
            const textEncoder = new TextEncoder();
            
            // Собираем команду
            let parts = [];
            
            // Инициализация
            parts.push(init);
            
            // Центрирование
            parts.push(new Uint8Array([ESC, 0x61, 0x01]));
            
            // Артикул - жирный
            parts.push(new Uint8Array([ESC, 0x45, 0x01])); // Жирный вкл
            parts.push(textEncoder.encode(currentProduct.article + '\n'));
            parts.push(new Uint8Array([ESC, 0x45, 0x00])); // Жирный выкл
            
            // Разделитель
            parts.push(textEncoder.encode('================================\n'));
            
            // Название товара
            parts.push(textEncoder.encode(currentProduct.name + '\n'));
            
            // Разделитель
            parts.push(textEncoder.encode('================================\n'));
            
            // Цены
            parts.push(textEncoder.encode('РОЗНИЦА: ' + currentProduct.retailPrice + ' руб.\n'));
            parts.push(textEncoder.encode('ОПТ: ' + currentProduct.wholesalePrice + ' руб.\n'));
            
            // Разделитель
            parts.push(textEncoder.encode('================================\n'));
            
            // Дата
            const now = new Date();
            const dateStr = now.toLocaleDateString('ru-RU');
            parts.push(textEncoder.encode(dateStr + '\n'));
            
            // Разделитель
            parts.push(textEncoder.encode('================================\n'));
            
            // Название магазина
            parts.push(textEncoder.encode('ИП Мааруф Р.\n'));
            
            // Отрезаем бумагу
            parts.push(new Uint8Array([GS, 0x56, 0x00]));
            
            // Объединяем все части
            const totalLength = parts.reduce((sum, p) => sum + p.length, 0);
            const result = new Uint8Array(totalLength);
            
            let offset = 0;
            parts.forEach(p => {
                result.set(p, offset);
                offset += p.length;
            });
            
            debug(`Команда сформирована, размер: ${result.length} байт`);
            return result;
        }

        // Печать
        async function printLabel() {
            if (!isConnected || !bluetoothCharacteristic) {
                alert('Сначала подключите принтер');
                return;
            }
            
            debug('Начинаем печать...');
            
            try {
                const command = createPrintCommand();
                
                // Отправляем команду
                await bluetoothCharacteristic.writeValue(command);
                
                debug('✓ Команда отправлена');
                showNotification('✅ Ценник отправлен на печать!', 'success');
                
            } catch (error) {
                debug('Ошибка печати: ' + error.message);
                showNotification('❌ Ошибка печати: ' + error.message, 'error');
            }
        }

        // Уведомление
        function showNotification(message, type) {
            const notification = document.createElement('div');
            notification.style.cssText = `
                position: fixed;
                top: 20px;
                left: 50%;
                transform: translateX(-50%);
                background: ${type === 'success' ? '#4CAF50' : '#f44336'};
                color: white;
                padding: 15px 25px;
                border-radius: 10px;
                font-weight: bold;
                z-index: 1000;
                box-shadow: 0 4px 15px rgba(0,0,0,0.2);
                animation: slideDown 0.3s ease-out;
            `;
            notification.textContent = message;
            
            document.body.appendChild(notification);
            
            setTimeout(() => {
                notification.style.animation = 'slideUp 0.3s ease-out';
                setTimeout(() => notification.remove(), 300);
            }, 3000);
        }

        // Добавляем стили для анимации
        const style = document.createElement('style');
        style.textContent = `
            @keyframes slideDown {
                from { transform: translateX(-50%) translateY(-100%); opacity: 0; }
                to { transform: translateX(-50%) translateY(0); opacity: 1; }
            }
            @keyframes slideUp {
                from { transform: translateX(-50%) translateY(0); opacity: 1; }
                to { transform: translateX(-50%) translateY(-100%); opacity: 0; }
            }
        `;
        document.head.appendChild(style);

        // Инициализация при загрузке
        window.onload = function() {
            debugEnabled = true;
            toggleDebug();
            
            // Проверяем поддержку Bluetooth
            checkBluetoothSupport();
            
            // Показываем товар
            document.getElementById('productArticle').textContent = `Артикул: ${currentProduct.article}`;
            document.getElementById('productName').textContent = currentProduct.name;
            document.getElementById('wholesalePrice').textContent = `${currentProduct.wholesalePrice} руб.`;
            document.getElementById('retailPrice').textContent = `${currentProduct.retailPrice} руб.`;
            
            // Проверяем сохраненный принтер
            try {
                const savedId = localStorage.getItem('savedPrinterId');
                if (savedId) {
                    debug('Найден сохраненный принтер: ' + savedId);
                }
            } catch (e) {}
            
            debug('Страница загружена');
        };
    </script>
</body>
</html>