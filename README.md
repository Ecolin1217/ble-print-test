<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BLE ESC/POS 測試 v2</title>
</head>
<body>
<h2>BLE ESC/POS 測試 v2</h2>

<button onclick="connect()">連線</button>
<button onclick="list()">列出 Service</button>

<pre id="log"></pre>

<script>
let device, server;

const optionalServices = [
'18f0','fff0','ffe0','ff00','ae30','ae3a'
];

function log(msg){
document.getElementById('log').textContent += msg + "\n";
}

async function connect(){
device = await navigator.bluetooth.requestDevice({
acceptAllDevices: true,
optionalServices
});
server = await device.gatt.connect();
log("已連線：" + device.name);
}

async function list(){
let services = await server.getPrimaryServices();
for(let s of services){
log("Service: " + s.uuid);
let chars = await s.getCharacteristics();
for(let c of chars){
let props = Object.keys(c.properties).filter(k=>c.properties[k]).join(",");
log("  Char: " + c.uuid + " | " + props);
}
}
}
</script>

</body>
</html>
