<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>سُدرة للأذكار</title>

<style>
body{
margin:0;
font-family:tahoma;
color:white;
text-align:center;
background:linear-gradient(-45deg,#05070d,#0b0f1a,#111827,#05070d);
background-size:400% 400%;
animation:bg 12s ease infinite;
}

@keyframes bg{
0%{background-position:0% 50%;}
50%{background-position:100% 50%;}
100%{background-position:0% 50%;}
}

header{
padding:20px;
font-size:26px;
font-weight:bold;
background:#00000055;
}

/* 🔥 تحسين الجوال */
.container{
max-width:850px;
margin:auto;
padding:10px;
}

@media(max-width:600px){
header{font-size:22px;}
.card{padding:15px;}
button{width:100%;}
}

.card{
background:rgba(255,255,255,0.06);
margin:12px 0;
padding:20px;
border-radius:18px;
box-shadow:0 0 20px #00000066;
backdrop-filter:blur(10px);
}

h2{color:#ffd700;}

button{
padding:12px 18px;
margin:6px;
border:none;
border-radius:12px;
background:linear-gradient(45deg,#d4af37,#ffd700,#fff2a8);
color:#111;
font-weight:bold;
cursor:pointer;
transition:0.3s;
}

button:hover{transform:scale(1.05);}

.counter{
font-size:50px;
color:#00e5ff;
margin:10px 0;
}

.prayer{
font-size:14px;
color:#9ad1ff;
line-height:1.8;
}

footer{
margin-top:20px;
padding:15px;
opacity:0.8;
font-size:14px;
}
</style>
</head>

<body>

<header>🌙 سُدرة للأذكار</header>

<div class="container">

<!-- الأذكار -->
<div class="card">
<h2>💫 دعاء</h2>
<p id="dua">اللهم اجعل لي من كل هم فرجًا</p>
<button onclick="newDua()">تغيير الدعاء</button>
</div>

<div class="card">
<h2>📿 الأذكار</h2>
<p id="zekr">سبحان الله</p>
<button onclick="newZekr()">ذكر جديد</button>
</div>

<!-- التسبيح -->
<div class="card">
<h2>🔢 التسبيح</h2>
<div class="counter" id="count">0</div>
<button onclick="add()">تسبيح</button>
<button onclick="reset()">إعادة</button>
</div>

<!-- أوقات الصلاة -->
<div class="card">
<h2>🕌 أوقات الصلاة</h2>
<div class="prayer" id="prayer">جاري التحميل...</div>
</div>

<!-- القرآن -->
<div class="card">
<h2>🎧 القرآن الكريم</h2>
<audio id="quran" controls></audio>
<br>
<button onclick="nextSurah()">السورة التالية</button>
</div>

<!-- نشر -->
<div class="card">
<h2>📢 نشر الموقع</h2>
<button onclick="shareSite()">نسخ رابط الموقع</button>
<p id="msg"></p>
</div>

</div>

<footer>👤 أحمد الدوسري</footer>

<script>

let count=0;

const duas=[
"اللهم ارزقني راحة البال",
"اللهم اغفر لي ولوالدي",
"اللهم اهدني ووفقني",
"اللهم نور قلبي بالإيمان",
"اللهم اجعلني من الصالحين"
];

const azkar=[
"سبحان الله",
"الحمد لله",
"الله أكبر",
"لا إله إلا الله",
"أستغفر الله",
"سبحان الله وبحمده",
"لا حول ولا قوة إلا بالله"
];

function newDua(){
document.getElementById("dua").innerText=
duas[Math.floor(Math.random()*duas.length)];
}

function newZekr(){
document.getElementById("zekr").innerText=
azkar[Math.floor(Math.random()*azkar.length)];
}

function add(){
count++;
document.getElementById("count").innerText=count;
}

function reset(){
count=0;
document.getElementById("count").innerText=count;
}

/* مشاركة */
function shareSite(){
navigator.clipboard.writeText(window.location.href);
document.getElementById("msg").innerText="تم نسخ الرابط ✔️";
setTimeout(()=>{document.getElementById("msg").innerText="";},2000);
}

/* 🕌 أوقات الصلاة حسب الموقع */
function loadPrayer(){
navigator.geolocation.getCurrentPosition(async(pos)=>{
let lat=pos.coords.latitude;
let lon=pos.coords.longitude;

let url=`https://api.aladhan.com/v1/timings?latitude=${lat}&longitude=${lon}&method=4`;

let res=await fetch(url);
let data=await res.json();
let t=data.data.timings;

document.getElementById("prayer").innerHTML=
`الفجر: ${t.Fajr}<br>
الظهر: ${t.Dhuhr}<br>
العصر: ${t.Asr}<br>
المغرب: ${t.Maghrib}<br>
العشاء: ${t.Isha}`;
});
}

/* 🎧 القرآن (سور متغيرة) */
const surahs=[
"https://cdn.islamic.network/quran/audio-surah/128/ar.alafasy/1.mp3",
"https://cdn.islamic.network/quran/audio-surah/128/ar.alafasy/36.mp3",
"https://cdn.islamic.network/quran/audio-surah/128/ar.alafasy/55.mp3",
"https://cdn.islamic.network/quran/audio-surah/128/ar.alafasy/67.mp3"
];

let index=0;
let audio=document.getElementById("quran");
audio.src=surahs[index];

function nextSurah(){
index=(index+1)%surahs.length;
audio.src=surahs[index];
audio.play();
}

loadPrayer();

</script>

</body>
</html>
