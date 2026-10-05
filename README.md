# gorod-po-tu-storonu<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Город по ту сторону</title>
<style>
* {
box-sizing: border-box;
-webkit-tap-highlight-color: transparent;
}
html, body {
margin: 0;
padding: 0;
width: 100%;
height: 100%;
overflow: hidden;
background: #05070c;
font-family: Arial, sans-serif;
color: white;
touch-action: none;
}
#game {
position: fixed;
inset: 0;
overflow: hidden;
}
canvas {
position: absolute;
inset: 0;
width: 100%;
height: 100%;
}
#intro {
position: fixed;
inset: 0;
z-index: 100;
display: flex;
align-items: center;
justify-content: center;
text-align: center;
background:
radial-gradient(circle at 50% 35%, rgba(110,20,45,.35), transparent 35%),
linear-gradient(#06070d, #10131c 55%, #030408);
transition: opacity 1s;
}
.intro-box {
width: min(700px, 90%);
}
.intro-small {
font-size: 11px;
letter-spacing: 5px;
color: #9296a0;
margin-bottom: 20px;
}
h1 {
margin: 0;
font-size: clamp(42px, 10vw, 82px);
line-height: .9;
letter-spacing: 5px;
text-shadow:
0 0 12px rgba(255,50,70,.7),
0 0 45px rgba(255,30,50,.3);
}
.intro-text {
margin: 28px auto;
max-width: 560px;
color: #c0c3ca;
font-size: 14px;
line-height: 1.7;
}
#start {
border: 1px solid rgba(255,100,110,.5);
border-radius: 9px;
background: #701c2d;
color: white;
padding: 15px 38px;
font-weight: bold;
letter-spacing: 2px;
font-size: 14px;
}
/* верхняя панель */
#hud {
position: fixed;
top: 15px;
left: 15px;
z-index: 20;
width: 320px;
padding: 14px;
border-radius: 14px;
background: rgba(5,7,13,.85);
border: 1px solid rgba(255,255,255,.13);
backdrop-filter: blur(10px);
}
.title {
font-size: 14px;
font-weight: bold;
letter-spacing: 2px;
margin-bottom: 10px;
}
.caption {
color: #888e99;
font-size: 10px;
text-transform: uppercase;
}
.bar {
height: 7px;
margin: 4px 0 9px;
border-radius: 10px;
overflow: hidden;
background: #222630;
}
#health {
height: 100%;
width: 100%;
background: #a9384c;
}
#energy {
height: 100%;
width: 100%;
background: #497ea9;
}
#time {
color: #969ba5;
font-size: 10px;
}
#mission {
margin-top: 10px;
padding-top: 10px;
border-top: 1px solid rgba(255,255,255,.1);
font-size: 12px;
line-height: 1.5;
}
/* инвентарь */
#inventory {
position: fixed;
top: 15px;
right: 15px;
z-index: 20;
padding: 12px 14px;
min-width: 150px;
border-radius: 14px;
background: rgba(5,7,13,.85);
border: 1px solid rgba(255,255,255,.13);
backdrop-filter: blur(10px);
font-size: 12px;
line-height: 1.7;
}
/* сообщение */
#message {
position: fixed;
z-index: 50;
left: 50%;
bottom: 145px;
transform: translateX(-50%);
width: min(600px, calc(100% - 30px));
padding: 13px 16px;
border-radius: 12px;
background: rgba(4,6,11,.94);
border: 1px solid rgba(255,255,255,.15);
text-align: center;
font-size: 13px;
line-height: 1.5;
opacity: 0;
pointer-events: none;
transition: .25s;
}
#message.show {
opacity: 1;
}
/* диалог */
#dialog {
display: none;
position: fixed;
left: 50%;
bottom: 18px;
transform: translateX(-50%);
z-index: 60;
width: min(650px, calc(100% - 24px));
padding: 18px;
border-radius: 17px;
background: rgba(4,6,11,.97);
border: 1px solid rgba(255,255,255,.18);
}
#dialog-name {
font-size: 17px;
font-weight: bold;
margin-bottom: 8px;
}
#dialog-text {
color: #d6d8de;
font-size: 14px;
line-height: 1.55;
}
#dialog-button {
width: 100%;
margin-top: 14px;
padding: 11px;
border: 0;
border-radius: 9px;
background: #e0e1e5;
color: #11131a;
font-weight: bold;
}
/* управление */
#joystick {
position: fixed;
left: 18px;
bottom: 18px;
z-index: 30;
width: 155px;
height: 155px;
}
.move {
position: absolute;
width: 48px;
height: 48px;
display: flex;
align-items: center;
justify-content: center;
border-radius: 13px;
background: rgba(8,11,18,.82);
border: 1px solid rgba(255,255,255,.17);
color: white;
font-size: 20px;
}
#up {
left: 53px;
top: 0;
}
#down {
left: 53px;
bottom: 0;
}
#left {
left: 0;
top: 53px;
}
#right {
right: 0;
top: 53px;
}
#action {
position: fixed;
right: 24px;
bottom: 35px;
z-index: 30;
width: 78px;
height: 78px;
border-radius: 50%;
border: 1px solid rgba(255,100,110,.4);
background: rgba(105,25,39,.88);
color: white;
font-size: 10px;
font-weight: bold;
}
#controls-info {
position: fixed;
right: 20px;
bottom: 125px;
z-index: 20;
color: #9a9ea7;
font-size: 10px;
text-align: right;
line-height: 1.5;
}
max-width:600px {
#hud {
width: 245px;
}
#inventory {
top: 115px;
}
#controls-info {
display: none;
}
}
</style>
</head>
<body>
<div id="game">
<canvas id="canvas"></canvas>
<!— ЗАСТАВКА —>
<div id="intro">
<div class="intro-box">
<div class="intro-small">
НЕИЗВЕСТНОЕ МЕСТО · 03:17
</div>
<h1>
ГОРОД

ПО ТУ СТОРОНУ
</h1>
<div class="intro-text">
Ты просыпаешься посреди незнакомого города.
Телефон не ловит сеть.
Вокруг почти никого нет.
Ты не помнишь, как сюда попал.
Тебе нужно найти еду, людей и дорогу домой.
</div>
<button id="start">
НАЧАТЬ ИГРУ
</button>
</div>
</div>
<!— ИГРОВАЯ ИНФОРМАЦИЯ —>
<div id="hud">
<div class="title">
ГОРОД ПО ТУ СТОРОНУ
</div>
<div class="caption">Здоровье</div>
<div class="bar">
<div id="health"></div>
</div>
<div class="caption">Энергия</div>
<div class="bar">
<div id="energy"></div>
</div>
<div id="time">
03:17
</div>
<div id="mission">
<b>Первая миссия</b>

Найди еду.
</div>
</div>
<!— ИНВЕНТАРЬ —>
<div id="inventory">
🎒 Инвентарь<
🍞 Еда: <span id="food">0</span><b
💧 Вода: <span id="water">0</span><br
🔑 Ключ: <span id="key">нет</span>
</div>
<div id="message"></div>
<!— ДИАЛОГ —>
<div id="dialog">
<div id="dialog-name">
Незнакомец
</div>
<div id="dialog-text"></div>
<button id="dialog-button">
Продолжить
</button>
</div>
<!— КНОПКИ —>
<div id="joystick">
<div class="move" id="up">▲</div>
<div class="move" id="down">▼</div>
<div class="move" id="left">◀</div>
<div class="move" id="right">▶</div>
</div>
<button id="action">
ДЕЙСТВИЕ
</button>
<div id="controls-info">
ПК: WASD / стрелки

Е — действие
</div>
</div>
<script>
const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");
let width = window.innerWidth;
let height = window.innerHeight;
function resize() {
width = window.innerWidth;
height = window.innerHeight;
canvas.width = width * devicePixelRatio;
canvas.height = height * devicePixelRatio;
canvas.style.width = width + "px";
canvas.style.height = height + "px";
ctx.setTransform(
devicePixelRatio,
0,
0,
devicePixelRatio,
0,
0
);
}
resize();
window.addEventListener("resize", resize);
/* МИР */
const WORLD_WIDTH = 3200;
const WORLD_HEIGHT = 2400;
const player = {
x: 1600,
y: 1200,
speed: 3,
health: 100,
energy: 100
};
const camera = {
x: 0,
y: 0
};
const inventory = {
food: 0,
water: 0,
key: false
};
const keys = {};
let gameStarted = false;
let dialogOpen = false;
let mission = 0;
let gameTime = 0;
/* ДОМА */
const buildings = [];
function building(x,y,w,h,type) {
buildings.push({x,y,w,h,type});
}
building(100,100,390,270,0);
building(610,110,300,290,1);
building(1040,100,390,280,2);
building(1540,110,350,290,0);
building(2010,100,410,280,1);
building(2550,120,430,270,2);
building(110,620,350,300,2);
building(570,600,350,300,0);
building(1020,620,340,300,1);
building(1460,600,390,320,2);
building(1940,610,350,300,0);
building(2410,600,420,300,1);
building(120,1130,350,320,1);
building(580,1150,310,310,2);
building(1010,1140,360,330,0);
building(1480,1160,380,300,1);
building(1960,1130,390,340,2);
building(2440,1150,390,320,0);
building(120,1670,370,320,0);
building(610,1690,320,300,1);
building(1080,1660,370,330,2);
building(1550,1680,390,310,0);
building(2040,1660,390,330,1);
building(2530,1690,400,300,2);
/* ДОРОГИ */
const roads = [
{x:0,y:440,w:WORLD_WIDTH,h:125},
{x:0,y:950,w:WORLD_WIDTH,h:125},
{x:0,y:1515,w:WORLD_WIDTH,h:125},
{x:0,y:2025,w:WORLD_WIDTH,h:120},
{x:470,y:0,w:120,h:WORLD_HEIGHT},
{x:920,y:0,w:120,h:WORLD_HEIGHT},
{x:1430,y:0,w:120,h:WORLD_HEIGHT},
{x:1890,y:0,w:120,h:WORLD_HEIGHT},
{x:2420,y:0,w:120,h:WORLD_HEIGHT}
];
/* ДЕРЕВЬЯ */
const trees = [];
for(let i=0;i<230;i++) {
trees.push({
x: 30 + (i * 173) % 3140,
y: 30 + (i * 257) % 2340,
scale: .7 + (i % 5) * .1
});
}
/* ФОНАРИ */
const lights = [];
for(let x=150;x<WORLD_WIDTH;x+=145) {
for(let y=500;y<WORLD_HEIGHT;y+=300) {
lights.push({
x:x + ((y/300)%2)*50,
y:y
});
}
}
/* ПРЕДМЕТЫ */
const foodItem = {
x:790,
y:1080,
collected:false
};
const waterItem = {
x:1700,
y:510,
collected:false
};
/* NPC */
const npcs = [
{
x:1150,
y:500,
name:"Незнакомка",
color:"#7e526a",
text:[
"Ты тоже здесь впервые?",
"Я проснулась три дня назад.",
"Никто не знает, где находится этот город.",
"Телефоны здесь не работают.",
"Если хочешь выбраться — ищи старую автобусную станцию."
]
},
{
x:1600,
y:1030,
name:"Мужчина у магазина",
color:"#526b83",
text:[
"Не помню, когда последний раз видел нормальную дорогу.",
"Город постоянно меняется.",
"Старая станция находится на севере.",
"Но ночью туда лучше не ходить."
]
},
{
x:2160,
y:520,
name:"Пожилая женщина",
color:"#776750",
text:[
"Если услышишь радио — не отвечай.",
"Оно иногда называет имена людей.",
"Тех, кто отвечает, потом никто не видит.",
"И ещё... не приближайся к красным фонарям."
]
}
];
/* ДОЖДЬ */
const rain = [];
for(let i=0;i<280;i++) {
rain.push({
x:Math.random()@id322860606 (*width),
y:Math.random()*height,
speed:8+Math.random()*10,
length:8+Math.random()*15
});
}
/* КЛАВИАТУРА */
window.addEventListener("keydown", function(e) {
keys[e.key.toLowerCase()] = true;
if(e.key.toLowerCase() === "e") {
interact();
}
});
window.addEventListener("keyup", function(e) {
keys[e.key.toLowerCase()] = false;
});
/* МОБИЛЬНЫЕ КНОПКИ */
function control(id,key) {
const button = document.getElementById(id);
button.addEventListener("pointerdown",function(e) {
e.preventDefault();
keys[key] = true;
});
button.addEventListener("pointerup",function(e) {
e.preventDefault();
keys[key] = false;
});
button.addEventListener("pointercancel",function() {
keys[key] = false;
});
button.addEventListener("pointerleave",function() {
keys[key] = false;
});
}
control("up","arrowup");
control("down","arrowdown");
control("left","arrowleft");
control("right","arrowright");
document.getElementById("action").addEventListener(
"pointerdown",
function(e) {
e.preventDefault();
interact();
}
);
/* ПРОВЕРКА СТОЛКНОВЕНИЙ */
function collision(x,y) {
const r = 18;
for(const b of buildings) {
if(
x+r > b.x &&
x-r < b.x+b.w &&
y+r > b.y &&
y-r < b.y+b.h
) {
return true;
}
}
return false;
}
/* РАССТОЯНИЕ */
function distance(a,b) {
return Math.hypot(
a.x-b.x,
a.y-b.y
);
}
/* ДВИЖЕНИЕ */
function movePlayer(delta) {
if(dialogOpen) return;
let dx = 0;
let dy = 0;
if(keys["w"] || keys["arrowup"]) dy--;
if(keys["s"] || keys["arrowdown"]) dy++;
if(keys["a"] || keys["arrowleft"]) dx--;
if(keys["d"] || keys["arrowright"]) dx++;
if(dx || dy) {
const length = Math.hypot(dx,dy);
dx /= length;
dy /= length;
const speed =
player.speed *
(player.energy > 0 ? 1 : .5);
const nx =
player.x + dx * speed * delta;
const ny =
player.y + dy * speed * delta;
if(!collision(nx,player.y)) {
player.x = nx;
}
if(!collision(player.x,ny)) {
player.y = ny;
}
player.energy -= .025 * delta;
if(player.energy < 0) {
player.energy = 0;
}
} else {
player.energy += .008 * delta;
if(player.energy > 100) {
player.energy = 100;
}
}
player.x = Math.max(
20,
Math.min(WORLD_WIDTH-20,player.x)
);
player.y = Math.max(
20,
Math.min(WORLD_HEIGHT-20,player.y)
);
}
/* КАМЕРА */
function updateCamera() {
camera.x = player.x - width/2;
camera.y = player.y - height/2;
camera.x = Math.max(
0,
Math.min(WORLD_WIDTH-width,camera.x)
);
camera.y = Math.max(
0,
Math.min(WORLD_HEIGHT-height,camera.y)
);
}
/* НЕБО */
function drawSky() {
const gradient =
ctx.createLinearGradient(
0,
0,
0,
WORLD_HEIGHT
);
gradient.addColorStop(0,"#160813");
gradient.addColorStop(.25,"#260e1b");
gradient.addColorStop(.5,"#111722");
gradient.addColorStop(1,"#060b11");
ctx.fillStyle = gradient;
ctx.fillRect(
0,
0,
WORLD_WIDTH,
WORLD_HEIGHT
);
}
/* ДОРОГИ */
function drawRoads() {
for(const road of roads) {
ctx.fillStyle = "#292e38";
ctx.fillRect(
road.x,
road.y,
road.w,
road.h
);
ctx.strokeStyle =
"rgba(160,165,175,.35)";
ctx.lineWidth = 2;
ctx.setLineDash([30,30]);
ctx.beginPath();
if(road.w > road.h) {
ctx.moveTo(
road.x,
road.y+road.h/2
);
ctx.lineTo(
road.x+road.w,
road.y+road.h/2
);
} else {
ctx.moveTo(
road.x+road.w/2,
road.y
);
ctx.lineTo(
road.x+road.w/2,
road.y+road.h
);
}
ctx.stroke();
ctx.setLineDash([]);
}
}
/* ДЕРЕВЬЯ */
function drawTrees() {
for(const tree of trees) {
if(
tree.x < camera.x-100 ||
tree.x > camera.x+width+100 ||
tree.y < camera.y-100 ||
tree.y > camera.y+height+100
) continue;
ctx.save();
ctx.translate(
tree.x,
tree.y
);
ctx.scale(
tree.scale,
tree.scale
);
ctx.fillStyle="#17100f";
ctx.fillRect(
-4,
3,
8,
28
);
ctx.fillStyle="#10241e";
ctx.beginPath();
ctx.arc(
0,
-8,
21,
0,
Math.PI*2
);
ctx.fill();
ctx.beginPath();
ctx.arc(
-11,
3,
16,
0,
Math.PI*2
);
ctx.arc(
11,
3,
16,
0,
Math.PI*2
);
ctx.fill();
ctx.restore();
}
}
/* ДОМА */
function drawBuildings() {
buildings.forEach(function(b,index) {
ctx.fillStyle="#090b10";
ctx.fillRect(
b.x+8,
b.y+10,
b.w,
b.h
);
const colors = [
"#1c2029",
"#20232d",
"#24232c"
];
ctx.fillStyle =
colors[b.type];
ctx.fillRect(
b.x,
b.y,
b.w,
b.h
);
ctx.strokeStyle="#3c424e";
ctx.lineWidth=3;
ctx.strokeRect(
b.x,
b.y,
b.w,
b.h
);
ctx.fillStyle="#11141b";
ctx.fillRect(
b.x,
b.y,
b.w,
30
);
const columns =
Math.max(
2,
Math.floor(b.w/65)
);
const rows =
Math.max(
2,
Math.floor(b.h/70)
);
for(let x=0;x<columns;x++) {
for(let y=0;y<rows;y++) {
const wx =
b.x+18+
x*(b.w-36)/columns;
const wy =
b.y+47+
y*(b.h-65)/rows;
const lit =
(x+y+index)%5===0;
ctx.fillStyle =
lit
? "rgba(208,112,68,.72)"
: "#2b3440";
ctx.fillRect(
wx,
wy,
21,
27
);
}
}
});
}
/* ФОНАРИ */
function drawLights() {
for(const light of lights) {
if(
light.x < camera.x-100 ||
light.x > camera.x+width+100 ||
light.y < camera.y-100 ||
light.y > camera.y+height+100
) continue;
ctx.strokeStyle="#171a20";
ctx.lineWidth=5;
ctx.beginPath();
ctx.moveTo(
light.x,
light.y
);
ctx.lineTo(
light.x,
light.y-52
);
ctx.stroke();
const glow =
ctx.createRadialGradient(
light.x,
light.y-52,
2,
light.x,
light.y-52,
75
);
glow.addColorStop(
0,
"rgba(255,177,91,.35)"
);
glow.addColorStop(
1,
"rgba(255,100,50,0)"
);
ctx.fillStyle = glow;
ctx.beginPath();
ctx.arc(
light.x,
light.y-52,
75,
0,
Math.PI*2
);
ctx.fill();
ctx.fillStyle="#e2a266";
ctx.beginPath();
ctx.arc(
light.x,
light.y-52,
3,
0,
Math.PI*2
);
ctx.fill();
}
}
/* ЕДА */
function drawFood() {
if(foodItem.collected) return;
ctx.save();
ctx.translate(
foodItem.x,
foodItem.y
);
ctx.fillStyle="rgba(0,0,0,.45)";
ctx.beginPath();
ctx.ellipse(
0,
13,
21,
8,
0,
0,
Math.PI*2
);
ctx.fill();
ctx.fillStyle="#d29d4a";
ctx.fillRect(
-16,
-10,
32,
22
);
ctx.fillStyle="#f0d08a";
ctx.fillRect(
-10,
-16,
20,
7
);
ctx.fillStyle="#fff";
ctx.font="bold 11px Arial";
ctx.textAlign="center";
ctx.fillText(
"ЕДА",
0,
-25
);
ctx.restore();
}
/* ВОДА */
function drawWater() {
if(waterItem.collected) return;
ctx.save();
ctx.translate(
waterItem.x,
waterItem.y
);
ctx.fillStyle="rgba(0,0,0,.45)";
ctx.beginPath();
ctx.ellipse(
0,
13,
16,
6,
0,
0,
Math.PI*2
);
ctx.fill();
ctx.fillStyle="#4c8fc4";
ctx.fillRect(
-9,
-18,
18,
32
);
ctx.fillStyle="#b8def5";
ctx.fillRect(
-7,
-23,
14,
6
);
ctx.fillStyle="#fff";
ctx.font="bold 10px Arial";
ctx.textAlign="center";
ctx.fillText(
"ВОДА",
0,
-31
);
ctx.restore();
}
/* ЛЮДИ */
function drawNPC(npc) {
ctx.save();
ctx.translate(
npc.x,
npc.y
);
ctx.fillStyle="rgba(0,0,0,.5)";
ctx.beginPath();
ctx.ellipse(
0,
20,
19,
8,
0,
0,
Math.PI*2
);
ctx.fill();
ctx.fillStyle=npc.color;
ctx.fillRect(
-13,
-2,
26,
32
);
ctx.fillStyle="#dcae8b";
ctx.beginPath();
ctx.arc(
0,
-15,
12,
0,
Math.PI*2
);
ctx.fill();
ctx.fillStyle="#211e22";
ctx.beginPath();
ctx.arc(
0,
-19,
12,
Math.PI,
Math.PI*2
);
ctx.fill();
ctx.restore();
}
/* ИГРОК */
function drawPlayer() {
ctx.save();
ctx.translate(
player.x,
player.y
);
ctx.fillStyle="rgba(0,0,0,.55)";
ctx.beginPath();
ctx.ellipse(
0,
21,
20,
8,
0,
0,
Math.PI*2
);
ctx.fill();
ctx.fillStyle="#252e3b";
ctx.fillRect(
-14,
-2,
28,
33
);
ctx.fillStyle="#dcae8b";
ctx.beginPath();
ctx.arc(
0,
-15,
12,
0,
Math.PI*2
);
ctx.fill();
ctx.fillStyle="#17181c";
ctx.beginPath();
ctx.arc(
0,
-19,
12,
Math.PI,
Math.PI*2
);
ctx.fill();
ctx.restore();
}
/* КРАСНОЕ СВЕЧЕНИЕ */
function redGlow() {
const gradient =
ctx.createRadialGradient(
width*.5,
height*.08,
20,
width*.5,
height*.08,
height*.8
);
gradient.addColorStop(
0,
"rgba(150,20,45,.15)"
);
gradient.addColorStop(
.55,
"rgba(90,15,35,.04)"
);
gradient.addColorStop(
1,
"rgba(0,0,0,0)"
);
ctx.fillStyle=gradient;
ctx.fillRect(
0,
0,
width,
height
);
}
/* ТУМАН */
function fog() {
const gradient =
ctx.createRadialGradient(
width/2,
height/2,
80,
width/2,
height/2,
Math.max(width,height)*.75
);
gradient.addColorStop(
0,
"rgba(140,145,165,0)"
);
gradient.addColorStop(
.55,
"rgba(80,90,110,.06)"
);
gradient.addColorStop(
1,
"rgba(0,0,0,.55)"
);
ctx.fillStyle=gradient;
ctx.fillRect(
0,
0,
width,
height
);
}
/* ДОЖДЬ */
function drawRain() {
ctx.strokeStyle =
"rgba(180,195,215,.17)";
ctx.lineWidth=1;
for(const drop of rain) {
drop.y += drop.speed;
if(drop.y > height+20) {
drop.y=-20;
drop.x=Math.random()@id322860606 (*width);
}
ctx.beginPath();
ctx.moveTo(
drop.x,
drop.y
);
ctx.lineTo(
drop.x-3,
drop.y+drop.length
);
ctx.stroke();
}
}
/* ПОДСКАЗКА */
function interactionHint() {
let nearest = 9999;
let target = null;
if(!foodItem.collected) {
const d =
distance(player,foodItem);
if(d<nearest) {
nearest=d;
target="food";
}
}
if(!waterItem.collected) {
const d =
distance(player,waterItem);
if(d<nearest) {
nearest=d;
target="water";
}
}
for(const npc of npcs) {
const d =
distance(player,npc);
if(d<nearest) {
nearest=d;
target="npc";
}
}
if(nearest < 90) {
const x =
player.x-camera.x;
const y =
player.y-camera.y-55;
ctx.fillStyle =
"rgba(0,0,0,.82)";
ctx.beginPath();
ctx.roundRect(
x-60,
y-16,
120,
32,
8
);
ctx.fill();
ctx.fillStyle="#fff";
ctx.font="bold 11px Arial";
ctx.textAlign="center";
if(target==="food") {
ctx.fillText(
"Е — взять еду",
x,
y+4
);
} else if(target==="water") {
ctx.fillText(
"Е — взять воду",
x,
y+4
);
} else {
ctx.fillText(
"Е — поговорить",
x,
y+4
);
}
}
}
/* ОТРИСОВКА */
function render() {
ctx.clearRect(
0,
0,
width,
height
);
ctx.save();
ctx.translate(
-camera.x,
-camera.y
);
drawSky();
drawTrees();
drawRoads();
drawBuildings();
drawLights();
drawFood();
drawWater();
for(const npc of npcs) {
drawNPC(npc);
}
drawPlayer();
ctx.restore();
redGlow();
fog();
drawRain();
interactionHint();
}
/* ВЗАИМОДЕЙСТВИЕ */
function interact() {
if(!gameStarted || dialogOpen) {
return;
}
/* ЕДА */
if(
!foodItem.collected &&
distance(player,foodItem)<90
) {
foodItem.collected=true;
inventory.food++;
player.energy =
Math.min(
100,
player.energy+30
);
updateInventory();
mission=1;
document.getElementById("mission").innerHTML =
"<b>Первая миссия</b>

"Найди человека у дороги.";
showMessage(
"Ты нашёл еду. Теперь нужно найти человека."
);
return;
}
/* ВОДА */
if(
!waterItem.collected &&
distance(player,waterItem)<90
) {
waterItem.collecte


