<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">

<!-- 🔥 إصلاح الجوال النهائي -->
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">

<title>سُدرة للأذكار</title>

<style>

/* =========================
   🔥 أساسيات عامة
========================= */
html, body{
margin:0;
padding:0;
width:100%;
overflow-x:hidden;
font-family:tahoma;
color:white;
background:linear-gradient(-45deg,#05070d,#0b0f1a,#111827,#05070d);
background-size:400% 400%;
animation:bg 12s ease infinite;
-webkit-text-size-adjust:100%;
}

*{
box-sizing:border-box;
}

@keyframes bg{
0%{background-position:0% 50%;}
50%{background-position:100% 50%;}
100%{background-position:0% 50%;}
}

/* =========================
   🔥 الهيدر
========================= */
header{
padding:16px;
font-size:22px;
font-weight:bold;
background:#00000055;
}

/* =========================
   🔥 الحاوية (مهم للجوال)
========================= */
.container{
width:100%;
max-width:600px;
margin:auto;
padding:10px;
}

/* =========================
   🔥 الكاردات
========================= */
.card{
background:rgba(255,255,255,0.06);
margin:10px 0;
padding:14px;
border-radius:14px;
box-shadow:0 0 15px #00000055;
backdrop-filter:blur(8px);
}

/* =========================
   🔥 النصوص
========================= */
h2{
color:#ffd700;
font-size:18px;
margin:8px 0;
}

p{
margin:8px 0;
font-size:14px;
line-height:1.6;
word-break:break-word;
}

/* =========================
   🔥 الأزرار (موبايل أولاً)
========================= */
button{
width:100%;
padding:12px;
margin-top:8px;
border:none;
border-radius:10px;
background:linear-gradient(45deg,#d4af37,#ffd700,#fff2a8);
color:#111;
font-weight:bold;
font-size:14px;
cursor:pointer;
}

/* =========================
   🔥 التسبيح
========================= */
.counter{
font-size:38px;
color:#00e5ff;
margin:10px 0;
}

/* =========================
   🕌 الصلاة
========================= */
.prayer{
font-size:13px;
line-height:1.8;
color:#9fd6ff;
}

/* =========================
   📱 موبايل صغير جداً
========================= */
@media (max-width:480px){

header{
font-size:18px;
padding:12px;
}

.container{
padding:8px;
}

.card{
padding:12px;
border-radius:12px;
}

h2{
font-size:16px;
}

button{
font-size:13px;
padding:11px;
}

.counter{
font-size:32px;
}
}

/* =========================
   🔻 footer
========================= */
footer{
margin-top:20px;
padding:15px;
opacity:0.8;
font-size:13px;
text-align:center;
}

</style>
</head>

<body>

<header>🌙 سُدرة للأذكار</header>

<div class="container">

<!-- الدعاء -->
<div class="card">
<h2>💫 دعاء</h2>
<p id="dua">اللهم اجعل لي من كل هم فرجًا</p>
<button onclick="newDua()">تغيير الدعاء</button>
</div>

<!-- الأذكار -->
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

<!-- الصلاة -->
<div class="card">
<h2>🕌 أوقات الصلاة</h2>
<div class="prayer" id="prayer">جاري التحميل...</div>
</div>

<!-- القرآن -->
<div class="card">
<h2>🎧 القرآن الكريم</h2>
<audio id="quran" controls></audio>
<button onclick="nextSurah()">السورة التالية</button>
</div>

<!-- مشاركة -->
<div class="card">
<h2>📢 نشر الموقع</h2>
<button onclick="shareSite()">نسخ الرابط</button>
<p id="msg"></p>
</div>

</div>

<footer>👤 أحمد الدوسري</footer>

<script>

let count = 0;

/* =========================
   💫 دعاء وأذكار
========================= */
const duas = [
"اللهم ارزقني راحة البال",
"اللهم اغفر لي ولوالدي",
"اللهم اهدني ووفقني",
"اللهم نور قلبي بالإيمان",
"اللهم اجعلني من الصالحين"
];

const azkar = [
"سبحان الله",
"الحمد لله",
"الله أكبر",
"لا إله إلا الله",
"أستغفر الله",
"سبحان الله وبحمده",
"لا حول ولا قوة إلا بالله"
];

function newDua(){
document.getElementById("dua").innerText =
duas[Math.floor(Math.random()*duas.length)];
}

function newZekr(){
document.getElementById("zekr").innerText =
azkar[Math.floor(Math.random()*azkar.length)];
}

/* =========================
   🔢 التسبيح
========================= */
function add(){
count++;
document.getElementById("count").innerText = count;
}

function reset(){
count = 0;
document.getElementById("count").innerText = count;
}

/* =========================
   📢 مشاركة
========================= */
function shareSite(){
navigator.clipboard.writeText(window.location.href);
document.getElementById("msg").innerText = "تم نسخ الرابط ✔️";
setTimeout(()=>{document.getElementById("msg").innerText="";},2000);
}

/* =========================
   🕌 أوقات الصلاة
========================= */
function loadPrayer(){
navigator.geolocation.getCurrentPosition(async(pos)=>{
let lat = pos.coords.latitude;
let lon = pos.coords.longitude;

let url = `https://api.aladhan.com/v1/timings?latitude=${lat}&longitude=${lon}&method=4`;

let res = await fetch(url);
let data = await res.json();
let t = data.data.timings;

document.getElementById("prayer").innerHTML =
`الفجر: ${t.Fajr}<br>
الظهر: ${t.Dhuhr}<br>
العصر: ${t.Asr}<br>
المغرب: ${t.Maghrib}<br>
العشاء: ${t.Isha}`;
});
}

/* =========================
   🎧 القرآن
========================= */
const surahs = [
"https://cdn.islamic.network/quran/audio-surah/128/ar.alafasy/1.mp3",
"https://cdn.islamic.network/quran/audio-surah/128/ar.alafasy/36.mp3",
"https://cdn.islamic.network/quran/audio-surah/128/ar.alafasy/55.mp3",
"https://cdn.islamic.network/quran/audio-surah/128/ar.alafasy/67.mp3"
];

let index = 0;
let audio = document.getElementById("quran");
audio.src = surahs[index];

function nextSurah(){
index = (index + 1) % surahs.length;
audio.src = surahs[index];
audio.play();
}

loadPrayer();

</script>

</body>
</html>
