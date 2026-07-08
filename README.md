<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Тест API cen.txt</title>
<style>
body{font-family:Arial;padding:20px;background:#f0f2f5}
.card{background:#fff;padding:15px;border-radius:10px;margin-bottom:15px;box-shadow:0 1px 3px rgba(0,0,0,0.1)}
button{padding:10px 16px;margin:5px;border:none;border-radius:6px;cursor:pointer;font-weight:bold}
.btn-send{background:#1a73e8;color:#fff}
.btn-get{background:#34a853;color:#fff}
.btn-del{background:#ea4335;color:#fff}
input{padding:10px;width:70%;border:1px solid #ddd;border-radius:6px}
pre{background:#f5f5f5;padding:10px;border-radius:6px;overflow-x:auto}
</style>
</head>
<body>
<h2>🧪 Тест API (192.168.1.23:8080)</h2>

<div class="card">
<h3>POST / — добавить строку</h3>
<input id="data" value='{"action":"test","phone":"79161234567"}'>
<button class="btn-send" onclick="postData()">Отправить</button>
</div>

<div class="card">
<h3>GET /lines — получить строки</h3>
<button class="btn-get" onclick="getLines()">Загрузить</button>
<pre id="lines-result">Нажми "Загрузить"</pre>
</div>

<div class="card">
<h3>DELETE /line?id=N — удалить строку</h3>
<input id="lineId" value="0" style="width:60px">
<button class="btn-del" onclick="deleteLine()">Удалить</button>
</div>

<div class="card">
<h3>DELETE /all — очистить всё</h3>
<button class="btn-del" onclick="deleteAll()">Очистить файл</button>
</div>

<script>
const API = 'http://192.168.1.23:8080';

async function postData(){
const d=document.getElementById('data').value;
const r=await fetch(API,{method:'POST',headers:{'Content-Type':'text/plain'},body:d});
const j=await r.json();
alert(JSON.stringify(j));
}

async function getLines(){
const r=await fetch(API+'/lines');
const j=await r.json();
document.getElementById('lines-result').textContent=JSON.stringify(j,null,2);
}

async function deleteLine(){
const id=document.getElementById('lineId').value;
const r=await fetch(API+'/line?id='+id,{method:'DELETE'});
const j=await r.json();
alert(JSON.stringify(j));
getLines();
}

async function deleteAll(){
if(!confirm('Точно удалить всё?'))return;
const r=await fetch(API+'/all',{method:'DELETE'});
const j=await r.json();
alert(JSON.stringify(j));
getLines();
}
</script>
</body>
</html>
