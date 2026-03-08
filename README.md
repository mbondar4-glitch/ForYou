# ForYou
Вне времени
<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Для тебя ❤️</title>

<style>

html,body{
margin:0;
padding:0;
background:black;
overflow:hidden;
font-family:Arial, sans-serif;
color:white;
}

canvas{
position:absolute;
top:0;
left:0;
}

#textContainer{
position:absolute;
top:50%;
left:50%;
transform:translate(-50%,-50%);
text-align:center;
z-index:10;
width:90%;
}

.line{
opacity:0;
font-weight:900;
text-shadow:0 0 20px #ff3d6e,0 0 40px #ff3d6e;
transition:opacity 2s;
}

.line.show{
opacity:1;
}

.line1{font-size:clamp(36px,7vw,70px);}
.line2{font-size:clamp(30px,6vw,60px);}
.line3{font-size:clamp(30px,6vw,60px);}
.line4{font-size:clamp(34px,7vw,65px);}

</style>
</head>

<body>

<canvas id="matrix"></canvas>
<canvas id="heartCanvas"></canvas>

<div id="textContainer">
<div class="line line1">С праздником</div>
<div class="line line2">Моя самая прекрасная</div>
<div class="line line3">Моя самая любимая</div>
<div class="line line4">Моё чудо ❤️‍🔥</div>
</div>

<script>

const canvas = document.getElementById("matrix");
const ctx = canvas.getContext("2d");

const heartCanvas = document.getElementById("heartCanvas");
const hctx = heartCanvas.getContext("2d");

function resize(){
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

heartCanvas.width = window.innerWidth;
heartCanvas.height = window.innerHeight;
}

resize();
window.onresize = resize;

const letters = "アイウエオカキクケコサシスセソ0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ";
const fontSize = 24;

let columns = canvas.width / fontSize;
let drops = [];

for(let i=0;i<columns;i++){
drops[i]=Math.random()*canvas.height;
}

function drawMatrix(){

ctx.fillStyle="rgba(0,0,0,0.08)";
ctx.fillRect(0,0,canvas.width,canvas.height);

ctx.font=fontSize+"px monospace";

for(let i=0;i<drops.length;i++){

let char;

if(Math.random()<0.025){

char="❤";
ctx.fillStyle="rgb(255,40,70)";
ctx.font="30px serif";

}else{

char=letters[Math.floor(Math.random()*letters.length)];

let shade=170+Math.random()*80;
ctx.fillStyle="rgb("+shade+",120,"+shade+")";
ctx.font=fontSize+"px monospace";

}

ctx.fillText(char,i*fontSize,drops[i]);

drops[i]+=fontSize;

if(drops[i]>canvas.height && Math.random()>0.975){
drops[i]=0;
}

}

}

setInterval(drawMatrix,33);

function showText(){

const lines=document.querySelectorAll(".line");

lines.forEach((line,i)=>{
setTimeout(()=>{
line.classList.add("show");
},i*2000);
});

}

setTimeout(showText,4000);



let particles=[];
let forming=false;

function heartShape(t){

let x=16*Math.pow(Math.sin(t),3);
let y=13*Math.cos(t)-5*Math.cos(2*t)-2*Math.cos(3*t)-Math.cos(4*t);

return {x,y};
}

function spawnParticles(){

particles=[];

for(let i=0;i<120;i++){

particles.push({

x:Math.random()*heartCanvas.width,
y:Math.random()*heartCanvas.height,
tx:0,
ty:0

});

}

}

spawnParticles();

function createHeartTargets(){

let centerX=heartCanvas.width/2;
let centerY=heartCanvas.height/2;

particles.forEach((p,i)=>{

let t=(i/particles.length)*Math.PI*2;

let pos=heartShape(t);

p.tx=centerX+pos.x*10;
p.ty=centerY-pos.y*10;

});

}

function animateHearts(){

hctx.clearRect(0,0,heartCanvas.width,heartCanvas.height);

particles.forEach(p=>{

if(forming){

p.x+=(p.tx-p.x)*0.05;
p.y+=(p.ty-p.y)*0.05;

}else{

p.y+=1;

if(p.y>heartCanvas.height){
p.y=0;
p.x=Math.random()*heartCanvas.width;
}

}

hctx.fillStyle="rgb(255,60,90)";
hctx.beginPath();
hctx.arc(p.x,p.y,4,0,Math.PI*2);
hctx.fill();

});

requestAnimationFrame(animateHearts);

}

animateHearts();

setTimeout(()=>{

forming=true;
createHeartTargets();

},12000);

</script>

</body>
</html>
