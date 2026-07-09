<!DOCTYPE html>
<html>
<body>
<h3>Отладка камер OnePlus 15</h3>
<pre id="result">Загрузка...</pre>
<script>
async function debugCameras() {
    let output = '';
    
    // 1. Список всех камер
    const devices = await navigator.mediaDevices.enumerateDevices();
    const videoDevices = devices.filter(d => d.kind === 'videoinput');
    output += `Всего камер: ${videoDevices.length}\n\n`;
    
    // 2. Пробуем открыть КАЖДУЮ камеру по deviceId
    for (let i = 0; i < videoDevices.length; i++) {
        try {
            const stream = await navigator.mediaDevices.getUserMedia({
                video: { deviceId: { exact: videoDevices[i].deviceId } },
                audio: false
            });
            const track = stream.getVideoTracks()[0];
            const label = track.label;
            const settings = track.getSettings();
            output += `Камера ${i}: ${label}\n`;
            output += `  deviceId: ${videoDevices[i].deviceId.substring(0, 30)}...\n`;
            output += `  Разрешение: ${settings.width}x${settings.height}\n`;
            output += `  facingMode: ${settings.facingMode || 'нет'}\n\n`;
            track.stop();
        } catch (e) {
            output += `Камера ${i}: ОШИБКА - ${e.message}\n\n`;
        }
    }
    
    // 3. Пробуем через facingMode
    try {
        const stream = await navigator.mediaDevices.getUserMedia({
            video: { facingMode: 'environment' },
            audio: false
        });
        const track = stream.getVideoTracks()[0];
        output += `facingMode 'environment':\n`;
        output += `  Название: ${track.label}\n`;
        output += `  deviceId: ${track.getSettings().deviceId?.substring(0, 30)}...\n`;
        track.stop();
    } catch (e) {
        output += `facingMode 'environment': ОШИБКА\n`;
    }
    
    document.getElementById('result').textContent = output;
}

debugCameras();
</script>
</body>
</html>