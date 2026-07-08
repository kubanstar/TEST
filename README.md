<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Тест отправки</title>
    <style>
        body { font-family: Arial; padding: 20px; }
        input, button { font-size: 18px; padding: 10px; margin: 5px; width: 90%; }
        #status { margin-top: 20px; font-weight: bold; }
        .ok { color: green; }
        .err { color: red; }
    </style>
</head>
<body>
    <h2>📤 Отправка данных на ПК (192.168.1.23:8080)</h2>
    
    <label>Данные (JSON или текст):</label>
    <input type="text" id="data" value='{"action":"test","phone":"79161234567","sum":1500}' placeholder='{"key":"value"}'>
    
    <button onclick="send()">🚀 ОТПРАВИТЬ</button>
    
    <div id="status"></div>
    <div id="log"></div>

    <script>
        async function send() {
            const status = document.getElementById('status');
            const log = document.getElementById('log');
            const data = document.getElementById('data').value;
            
            status.textContent = 'Отправка...';
            status.className = '';
            
            // Пробуем два варианта URL (иногда localhost не резолвится с телефона)
            const urls = [
                'http://192.168.1.23:8080/',
                'http://192.168.1.23:8080'
            ];
            
            let success = false;
            
            for (const url of urls) {
                try {
                    const response = await fetch(url, {
                        method: 'POST',
                        mode: 'cors',
                        headers: { 'Content-Type': 'text/plain' },
                        body: data
                    });
                    
                    if (response.ok) {
                        const text = await response.text();
                        status.textContent = `✅ Успешно! Ответ сервера: ${text}`;
                        status.className = 'ok';
                        log.innerHTML += `<br>[${new Date().toLocaleTimeString()}] Отправлено: ${data}`;
                        success = true;
                        break;
                    }
                } catch (err) {
                    // Пробуем следующий URL
                    console.log('Не удалось с ' + url + ': ' + err.message);
                }
            }
            
            if (!success) {
                status.textContent = `❌ Не удалось отправить. Открой http://192.168.1.23:8080/ в браузере и убедись, что видишь "OK".`;
                status.className = 'err';
            }
        }
    </script>
</body>
</html>
