<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Управление cen.txt</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:Arial,sans-serif;background:#f0f2f5;padding:15px;color:#333;max-width:600px;margin:0 auto}
h1{font-size:20px;margin:15px 0;text-align:center;color:#1a73e8}
.card{background:#fff;border-radius:10px;padding:15px;margin-bottom:15px;box-shadow:0 1px 3px rgba(0,0,0,0.1)}
.card h3{font-size:15px;margin-bottom:10px;color:#555}
input[type=text]{width:100%;padding:12px;font-size:16px;border:1px solid #ddd;border-radius:8px;margin-bottom:10px}
button{padding:10px 16px;border:none;border-radius:8px;cursor:pointer;font-weight:bold;font-size:14px}
.btn-send{background:#1a73e8;color:#fff;width:100%}
.btn-refresh{background:#34a853;color:#fff;width:100%}
.btn-clear{background:#ea4335;color:#fff;width:100%;margin-top:10px}
.btn-del{background:#ff4444;color:#fff;padding:6px 12px;font-size:13px;min-width:32px}
.line-row{display:flex;align-items:center;justify-content:space-between;padding:10px 12px;margin:4px 0;background:#f8f9fa;border-radius:8px;gap:10px}
.line-num{font-weight:bold;color:#1a73e8;min-width:28px;font-size:13px}
.line-text{flex:1;font-size:13px;word-break:break-all}
.empty{text-align:center;color:#999;padding:25px;font-size:14px}
.counter{text-align:center;font-size:13px;color:#666;margin-bottom:8px}
#status{text-align:center;font-size:14px;margin-top:8px;min-height:20px}
.ok{color:#34a853}
.err{color:#ea4335}
</style>
</head>
<body>

<h1>📋 Управление cen.txt</h1>

<div class="card">
    <h3>📤 Добавить запись</h3>
    <input type="text" id="data" value='{"action":"test","phone":"79161234567"}' placeholder='Введите JSON или текст'>
    <button class="btn-send" onclick="addLine()">Отправить</button>
    <div id="status"></div>
</div>

<div class="card">
    <h3>📄 Содержимое файла</h3>
    <div class="counter" id="counter">Загрузка...</div>
    <div id="lines-list"></div>
    <button class="btn-refresh" onclick="loadLines()">🔄 Обновить</button>
    <button class="btn-clear" onclick="deleteAll()">🗑 Очистить всё</button>
</div>

<script>
const API = 'http://192.168.1.23:8080';

async function addLine(){
    const s = document.getElementById('status');
    const input = document.getElementById('data');
    const text = input.value.trim();
    if(!text){ s.textContent='Введите данные'; s.className='err'; return; }
    
    s.textContent='Отправка...'; s.className='';
    try{
        const r = await fetch(API, {method:'POST', headers:{'Content-Type':'text/plain'}, body:text});
        const j = await r.json();
        if(j.status==='ok'){
            s.textContent='✅ Добавлено!';
            s.className='ok';
            input.value='';
            loadLines(); // обновить список
        }else{
            s.textContent='❌ '+j.message;
            s.className='err';
        }
    }catch(e){
        s.textContent='❌ Ошибка: '+e.message;
        s.className='err';
    }
}

async function loadLines(){
    const counter = document.getElementById('counter');
    const list = document.getElementById('lines-list');
    
    try{
        const r = await fetch(API+'/lines');
        const lines = await r.json();
        
        if(lines.length===0){
            counter.textContent='Строк: 0';
            list.innerHTML='<div class="empty">Файл пуст</div>';
        }else{
            counter.textContent='Строк: '+lines.length;
            let html='';
            lines.forEach((line, i)=>{
                html += `
                <div class="line-row">
                    <span class="line-num">#${i+1}</span>
                    <span class="line-text">${escapeHtml(line)}</span>
                    <button class="btn-del" onclick="deleteLine(${i})">✕</button>
                </div>`;
            });
            list.innerHTML=html;
        }
    }catch(e){
        counter.textContent='Ошибка загрузки';
        list.innerHTML='<div class="empty">'+e.message+'</div>';
    }
}

async function deleteLine(index){
    if(!confirm('Удалить строку #'+(index+1)+'?')) return;
    try{
        const r = await fetch(API+'/line?id='+index, {method:'DELETE'});
        const j = await r.json();
        if(j.status==='ok'){
            loadLines(); // обновить список
        }else{
            alert('Ошибка: '+j.message);
        }
    }catch(e){
        alert('Ошибка: '+e.message);
    }
}

async function deleteAll(){
    if(!confirm('Удалить ВСЕ строки безвозвратно?')) return;
    try{
        const r = await fetch(API+'/all', {method:'DELETE'});
        const j = await r.json();
        if(j.status==='ok'){
            loadLines(); // обновить список
        }else{
            alert('Ошибка: '+j.message);
        }
    }catch(e){
        alert('Ошибка: '+e.message);
    }
}

function escapeHtml(text){
    const d = document.createElement('div');
    d.textContent = text;
    return d.innerHTML;
}

// Загрузить список при открытии
loadLines();
</script>
</body>
</html>
