<!DOCTYPE html>
<html>
<body>
<h3>Камеры OnePlus 15:</h3>
<pre id="result">Загрузка...</pre>
<script>
async function getCameras() {
    let output = '';
    const modes = ['environment', 'user'];
    
    for (const mode of modes) {
        try {
            const stream = await navigator.mediaDevices.getUserMedia({
                video: { facingMode: mode },
                audio: false
            });
            
            const track = stream.getVideoTracks()[0];
            const settings = track.getSettings();
            const label = track.label || 'Без названия';
            
            output += `${mode}: ${label}\n`;
            output += `  width: ${settings.width}, height: ${settings.height}\n`;
            output += `  deviceId: ${settings.deviceId || 'нет'}\n`;
            output += `  facingMode: ${settings.facingMode || 'нет'}\n\n`;
            
            track.stop();
        } catch (e) {
            output += `${mode}: ОШИБКА — ${e.message}\n\n`;
        }
    }
    
    // Теперь попробуем получить все камеры через enumerateDevices
    const devices = await navigator.mediaDevices.enumerateDevices();
    const videoDevices = devices.filter(d => d.kind === 'videoinput');
    
    output += `Всего камер в системе: ${videoDevices.length}\n`;
    videoDevices.forEach((d, i) => {
        output += `  Камера ${i}: deviceId=${d.deviceId?.substring(0,20)}... groupId=${d.groupId?.substring(0,20)}...\n`;
    });
    
    document.getElementById('result').textContent = output;
}

getCameras();
</script>
</body>
</html>