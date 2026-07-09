<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Отладка камер</title>
    <style>
        body { font-family: Arial; padding: 10px; background: #000; color: #fff; margin: 0; }
        h3 { text-align: center; margin: 10px 0; }
        .camera-test { margin-bottom: 20px; border: 2px solid #333; border-radius: 10px; padding: 10px; }
        .camera-test h4 { margin: 5px 0; }
        video { width: 100%; border-radius: 5px; background: #111; }
        button { padding: 10px 20px; margin: 5px; border: none; border-radius: 5px; cursor: pointer; font-size: 14px; }
        .btn-open { background: #4CAF50; color: #fff; }
        .btn-stop { background: #f44336; color: #fff; }
        .info { font-size: 12px; color: #aaa; margin: 5px 0; }
    </style>
</head>
<body>
    <h3>Тест камер OnePlus 15</h3>
    <div id="cameras"></div>

    <script>
        async function testCameras() {
            const devices = await navigator.mediaDevices.enumerateDevices();
            const videoDevices = devices.filter(d => d.kind === 'videoinput');
            const container = document.getElementById('cameras');

            for (let i = 0; i < videoDevices.length; i++) {
                const device = videoDevices[i];
                
                const div = document.createElement('div');
                div.className = 'camera-test';
                div.innerHTML = `
                    <h4>Камера ${i}</h4>
                    <div class="info">deviceId: ${device.deviceId.substring(0, 30)}...</div>
                    <div class="info" id="label-${i}">label: загрузка...</div>
                    <div class="info" id="settings-${i}"></div>
                    <video id="video-${i}" playsinline autoplay muted></video>
                    <button class="btn-open" onclick="openCam(${i}, '${device.deviceId}')">Открыть</button>
                    <button class="btn-stop" onclick="stopCam(${i})">Остановить</button>
                `;
                container.appendChild(div);
            }
        }

        const streams = {};

        async function openCam(index, deviceId) {
            // Останавливаем все остальные камеры
            for (const key in streams) {
                if (streams[key]) {
                    streams[key].getTracks().forEach(t => t.stop());
                    delete streams[key];
                }
            }

            try {
                const stream = await navigator.mediaDevices.getUserMedia({
                    video: { deviceId: { exact: deviceId }, width: { ideal: 1280 }, height: { ideal: 720 } },
                    audio: false
                });

                const video = document.getElementById(`video-${index}`);
                video.srcObject = stream;
                streams[index] = stream;

                const track = stream.getVideoTracks()[0];
                const settings = track.getSettings();
                
                document.getElementById(`label-${index}`).textContent = `label: ${track.label}`;
                document.getElementById(`settings-${index}`).textContent = 
                    `Разрешение: ${settings.width}x${settings.height} | facingMode: ${settings.facingMode || 'нет'}`;
            } catch (e) {
                document.getElementById(`label-${index}`).textContent = `ОШИБКА: ${e.message}`;
            }
        }

        function stopCam(index) {
            if (streams[index]) {
                streams[index].getTracks().forEach(t => t.stop());
                delete streams[index];
                const video = document.getElementById(`video-${index}`);
                video.srcObject = null;
            }
        }

        testCameras();
    </script>
</body>
</html>
