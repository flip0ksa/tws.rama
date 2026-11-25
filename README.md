<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Twisted Labs</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;700;800&display=swap" rel="stylesheet">
<style>
:root{--bg:#070707;--card:#1f1f1f;--line:#333;--pink:#ff4d6d;--txt:#fff;--mute:#a9a9a9}
*{box-sizing:border-box;margin:0}
html,body{background:var(--bg);color:var(--txt);font-family:Tajawal,system-ui,sans-serif}
body{min-height:100vh;padding:calc(64px + env(safe-area-inset-top,0px)) 16px calc(84px + env(safe-area-inset-bottom,0px))}
button{font:inherit;color:inherit;background:none;border:0;cursor:pointer}
button:focus-visible{outline:2px solid var(--pink);outline-offset:2px}
svg{width:24px;height:24px;fill:none;stroke:currentColor;stroke-width:1.8;stroke-linecap:round;stroke-linejoin:round}

.top{position:fixed;inset:0 0 auto;z-index:5;display:flex;align-items:center;justify-content:space-between;padding:calc(10px + env(safe-area-inset-top,0px)) 16px 10px;background:rgba(7,7,7,.92);backdrop-filter:blur(8px);border-bottom:1px solid var(--line)}
.brand{font-weight:800;font-size:18px;letter-spacing:.04em;color:var(--pink);direction:ltr}

.page{display:none;max-width:640px;margin:0 auto}.page.on{display:block}
h1{font-size:44px;line-height:1.05;font-weight:800;direction:ltr;text-align:right;margin-top:12px}
.sub{color:var(--mute);margin:4px 0 20px}

.card{display:block;width:100%;text-align:right;background:var(--card);border:1px solid var(--line);border-radius:16px;padding:16px;margin-bottom:12px}
.row{display:flex;justify-content:space-between;align-items:flex-start;gap:12px}
.card h3{font-size:22px;font-weight:800;direction:ltr;text-align:right}
.ar{color:var(--mute);font-size:14px}
.price{color:var(--pink);font-weight:800;font-size:17px;white-space:nowrap}
.meta{color:var(--mute);font-size:14px;margin-top:10px;display:flex;flex-wrap:wrap;gap:6px 14px}
.desc{display:none;margin-top:12px;padding-top:12px;border-top:1px solid var(--line);line-height:1.7;font-size:15px}
.card.open{border-color:var(--pink)}.card.open .desc{display:block}

.empty{background:var(--card);border:1px dashed var(--line);border-radius:16px;padding:32px 16px;text-align:center;color:var(--mute);line-height:1.8}

.nav{position:fixed;inset:auto 0 0;z-index:5;display:flex;background:rgba(7,7,7,.96);border-top:1px solid var(--line);padding-bottom:env(safe-area-inset-bottom,0px)}
.nav button{flex:1;display:flex;flex-direction:column;align-items:center;gap:4px;padding:10px 0;color:var(--mute);font-size:13px}
.nav button.on{color:var(--pink)}

#scrim{position:fixed;inset:0;z-index:8;background:rgba(0,0,0,.6);opacity:0;pointer-events:none;transition:opacity .2s}
#drawer{position:fixed;inset:0 0 0 auto;z-index:9;width:min(78%,300px);background:#111;border-left:1px solid var(--line);padding:calc(20px + env(safe-area-inset-top,0px)) 16px 20px;transform:translateX(100%);transition:transform .25s}
body.menu #scrim{opacity:1;pointer-events:auto}body.menu #drawer{transform:none}
#drawer h2{font-size:15px;color:var(--mute);font-weight:400;margin-bottom:8px}
#drawer button{display:block;width:100%;text-align:right;padding:14px 4px;font-size:18px;border-bottom:1px solid var(--line)}
#toast{position:fixed;left:50%;bottom:calc(90px + env(safe-area-inset-bottom,0px));z-index:12;transform:translateX(-50%);background:var(--pink);color:#000;font-weight:700;padding:10px 18px;border-radius:99px;opacity:0;transition:opacity .2s;pointer-events:none}
#toast.show{opacity:1}
@media (prefers-reduced-motion:reduce){*{transition:none!important}}
</style>
</head>
<body>

<header class="top">
  <button id="menuBtn" aria-label="القائمة"><svg viewBox="0 0 24 24"><path d="M4 7h16M4 12h16M4 17h16"/></svg></button>
  <span class="brand">TWISTED LABS</span>
</header>

<main>
  <section id="home" class="page on">
    <h1>GAMES</h1>
    <p class="sub">الألعاب والأسعار، اضغط على اللعبة لتفاصيلها</p>
    <div id="grid"></div>
  </section>

  <section id="board" class="page">
    <h1>LEADERBOARD</h1>
    <p class="sub">ترتيب اللاعبين</p>
    <div class="empty">الترتيب يطلع هنا بعد ما يبدأ اللاعبين يجمعون النقاط.</div>
  </section>
</main>

<nav class="nav">
  <button data-p="home" class="on"><svg viewBox="0 0 24 24"><path d="M3 11l9-8 9 8M5 10v10h14V10M10 20v-6h4v6"/></svg>الألعاب</button>
  <button data-p="board"><svg viewBox="0 0 24 24"><rect x="5" y="3" width="14" height="18" rx="2"/><path d="M9 3v18M12 8h4M12 12h4"/></svg>الليدر بورد</button>
</nav>

<div id="scrim"></div>
<aside id="drawer" aria-label="القائمة">
  <h2>المزيد</h2>
  <button>المجتمع</button>
  <button>التصويت</button>
  <button>الفريق</button>
  <button>المتجر</button>
  <button>حسابي</button>
</aside>
<div id="toast">قريبًا</div>

<script>
// [الاسم، العربي، السعر، [معلومات]، الوصف]
const GAMES=[
["The Doors","7 غرف مترابطة","49 ريال للشخص",["15 دقيقة","1 إلى 6 لاعبين"],"أسئلة بفئات يختارها الفريق قبل الدخول. الفوز بإنهاء الغرف السبع قبل نهاية الوقت."],
["Escape Room","3 غرف مترابطة","49 ريال للشخص",["20 دقيقة","1 إلى 5 لاعبين"],"ثيم مرعب: تشبيك أسلاك، كودين، وأزرار. معكم لاسلكي لطلب تلميح."],
["Console Area","أريا الكونسول","20 ريال للساعة",["حجز ساعة كحد أدنى"],"أجهزة PlayStation وجهاز ألعاب الطيبين القديمة. العب على أي جهاز متاح، ويقدر أكثر من شخص يلعبون على نفس الجهاز."],
["Nintendo","نينتندو","من 20 ريال",["ربع ساعة 20 ريال","نص ساعة 30 ريال"],"اختر ربع ساعة أو نص ساعة."],
["PC","غرفة الجيمينج سنتر","10 ريال للساعة",["10 أجهزة","حجز ساعة كحد أدنى"],"جهاز لكل شخص، وألعاب كثيرة وحسابات جاهزة من Twisted Minds. تقدر تاخذ يد سوني."],
["Champion Test","3 ألعاب","39 ريال للشخص",["من عمر 10 سنوات","كل لعبة مرة واحدة"],"Reaction Sticks وMind Racer وTwall."],
["Drift Simulator","محاكاة درفت","39 ريال لنص ساعة",["من عمر 7 سنوات","4 أجهزة"],"لعب حر، والجهاز يشيل شخص واحد. حجز نص ساعة كحد أدنى."],
["Formula Simulator","محاكاة فورمولا","55 ريال",["من عمر 11 سنة","حوالي 20 دقيقة"],"3 أجهزة مربوطة في سباق واحد، كل جهاز لاعب. 4 جولات والفوز لأسرع لفة."]
];
const $=s=>document.querySelector(s);
$("#grid").innerHTML=GAMES.map(g=>`<button class="card"><div class="row"><div><h3>${g[0]}</h3><div class="ar">${g[1]}</div></div><div class="price">${g[2]}</div></div><div class="meta">${g[3].map(m=>`<span>${m}</span>`).join("")}</div><p class="desc">${g[4]}</p></button>`).join("");
$("#grid").addEventListener("click",e=>{const c=e.target.closest(".card");if(c)c.classList.toggle("open")});

document.querySelectorAll(".nav button").forEach(b=>b.onclick=()=>{
  document.querySelectorAll(".nav button,.page").forEach(x=>x.classList.remove("on"));
  b.classList.add("on");$("#"+b.dataset.p).classList.add("on");scrollTo(0,0);
});

const toggle=()=>document.body.classList.toggle("menu");
$("#menuBtn").onclick=toggle;$("#scrim").onclick=toggle;
let t;document.querySelectorAll("#drawer button").forEach(b=>b.onclick=()=>{
  toggle();const el=$("#toast");el.classList.add("show");clearTimeout(t);t=setTimeout(()=>el.classList.remove("show"),1500);
});
</script>
</body>
</html>
