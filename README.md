
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>YKS Net ve Puan Hesaplama | TYT AYT 2026</title>
<meta name="description" content="YKS TYT ve AYT net hesaplama aracı. Doğru yanlış girerek netini ve tahmini puanını hemen öğren. Ücretsiz.">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdn.tailwindcss.com"></script>
<style>
/* Genel stiller */
body{background:#f7f7f7;color:#333;font-family:'Poppins',sans-serif;}
header{width:100%;padding:20px;text-align:center;font-size:24px;font-weight:600;background:linear-gradient(135deg,#ffecd2,#fcb69f);box-shadow:0 4px 8px rgba(0,0,0,0.1);}
.btn{display:inline-block;padding:8px 14px;border:none;border-radius:8px;background:#ff7e5f;color:white;cursor:pointer;font-size:14px;transition:0.3s;text-decoration:none;text-align:center;margin-right:10px;}
.btn:hover{background:#feb47b;}
.hidden{display:none;}
.panel{background:white;border-radius:12px;padding:15px;box-shadow:0 6px 12px rgba(0,0,0,0.15);margin-top:10px;position:relative;}
/* Widget */
#floatingNetButtonWrapper{position:fixed;bottom:90px;right:10px;z-index:9999;display:flex;flex-direction:column;align-items:flex-end;gap:4px;}
#floatingNetWidget{position:fixed;bottom:80px;right:10px;width:180px;background:#ffecd2;border-radius:12px;padding:10px;font-size:13px;box-shadow:0 4px 10px rgba(0,0,0,0.15);display:none;transform:scale(0);opacity:0;transition: transform 0.2s, opacity 0.2s;z-index:9999;}
@media (max-width:768px){
    #floatingNetWidget{width:160px;right:5px;}
    #floatingNetButtonWrapper{right:5px;align-items:flex-end;}
    #floatingHedefPuan{width:70px !important;}
}
</style>
</head>
<!-- İlk Giriş İpucu Balonu -->
<div id="firstVisitTip" style="
    position: fixed;
    bottom: 170px;
    right: 16px;
    background: #ffffff;
    padding: 10px 12px;
    border-radius: 10px;
    box-shadow: 0 6px 20px rgba(0,0,0,0.25);
    font-size: 13px;
    max-width: 220px;
    z-index: 2147483647;
">
  <strong>💡 İpucu</strong><br>
  TYT ve AYT doğru–yanlışlarını girerek netini hemen öğrenebilirsin.
</div>

<script>
window.addEventListener("load", function () {
    setTimeout(function () {
        var tip = document.getElementById("firstVisitTip");
        if (tip) {
            tip.style.display = "none";
        }
    }, 2500);
});
</script>

<body>

<header>🎯 YKS Destek Sitesi</header>

<div style="max-width:1200px;margin:20px auto;padding:0 20px;">
  <button class="btn" onclick="openPanel('puan')">Puan İpuçları</button>
  <button class="btn" onclick="openPanel('tavsiye')">Sınav Tavsiyeleri</button>

<!-- Floating Widget -->
<div id="floatingNetButtonWrapper" style="position:fixed;bottom:90px;right:10px;z-index:9999;display:flex;flex-direction:column;align-items:flex-end;gap:4px;">
  <button id="floatingNetButton" onclick="toggleFloatingWidget()" style="width:50px;height:50px;border-radius:50%;border:none;background:#ff7e5f;color:white;font-size:24px;cursor:pointer;box-shadow:0 4px 8px rgba(0,0,0,0.2);transition:0.3s;" onmouseover="this.style.background='#feb47b'" onmouseout="this.style.background='#ff7e5f'">📝</button>
  <div style="background: rgba(255,255,255,0.9); padding: 3px 8px; border-radius:6px; font-size:12px; box-shadow:0 2px 5px rgba(0,0,0,0.15); color:#333;">Hedef puanınızı hesaplayın</div>
</div>

<div id="floatingNetWidget" style="position:fixed;bottom:80px;right:10px;width:180px;background:#ffecd2;border-radius:12px;padding:10px;font-family:'Poppins',sans-serif;font-size:13px;box-shadow:0 4px 10px rgba(0,0,0,0.15);transform:scale(0);opacity:0;transition:0.2s;z-index:10001;">
  <button onclick="toggleFloatingWidget()" style="position:absolute;top:-5px;right:-5px;border:none;background:transparent;font-size:16px;font-weight:bold;cursor:pointer;color:#333;z-index:10002;">×</button>
  <div style="display:flex; gap:2px; align-items:center;">
    <input type="number" id="floatingHedefPuan" placeholder="Hedef" style="width:90px;padding:5px;border-radius:5px;border:1px solid #ccc;font-size:13px;transition:border 0.2s;" onfocus="this.style.borderColor='#ff7e5f'" onblur="this.style.borderColor='#ccc'">
    <button onclick="hesaplaFloatingNet()" style="padding:5px 8px;font-size:13px;border:none;border-radius:5px;background:#ff7e5f;color:white;cursor:pointer;transition:0.3s;" onmouseover="this.style.background='#feb47b'" onmouseout="this.style.background='#ff7e5f'">Hesapla</button>
  </div>
  <div id="floatingNetSonuc" style="margin-top:8px; display:flex; justify-content:space-between;">
    <div>📘 TYT: <span id="floatingTytNet">0</span></div>
    <div>📗 AYT: <span id="floatingAytNet">0</span></div>
  </div>
</div>

<script>
// Panel içerik
const panelContent = {
  puan: { 
    title:"📊 Puan İpuçları",
    text:`<ul style="padding-left:18px;margin:0;">
<li>📝 <strong>Ders Planı Oluştur:</strong> Günlük ve haftalık hedefler belirle, konu dağılımını dengeli yap.</li>
<li>⏱ <strong>Zamanı Verimli Kullan:</strong> Her konu için süre belirle, denemeleri süreli çöz.</li>
<li>🔍 <strong>Yanlış Analizi:</strong> Yanlış yaptığın soruları mutlaka tekrar et, eksiklerini not al.</li>
<li>💡 <strong>Küçük Hedefler:</strong> Günlük 5-10 soruluk mini hedefler motivasyonu artırır.</li>
<li>📊 <strong>Denemelerle Ölçüm:</strong> Haftalık veya aylık denemelerle ilerlemeyi takip et.</li>
</ul>`},
  tavsiye: {
    title:"🎯 Sınav Tavsiyeleri",
    text:`<ul style="padding-left:18px;margin:0;">
<li>🧘‍♂️ <strong>Stres Yönetimi:</strong> Sınav öncesi nefes egzersizleri yap, rahatlamaya çalış.</li>
<li>🕒 <strong>Zaman Yönetimi:</strong> Soruları hızlı okuyup süreyi dengeli kullan.</li>
<li>📌 <strong>Soru Tiplerini Önceden Çalış:</strong> Test stratejilerini bilmek zamandan kazanmanı sağlar.</li>
<li>☕ <strong>Kısa Molalar:</strong> Çalışma sırasında 5 dakikalık kısa molalar konsantrasyonu artırır.</li>
<li>🌙 <strong>Uyku ve Beslenme:</strong> Sınav öncesi iyi uyumak ve hafif beslenmek performansı artırır.</li>
</ul>`}
};

function openPanel(key){
  const panel=document.getElementById('infoPanel');
  document.getElementById('panelTitle').innerHTML = panelContent[key].title;
  document.getElementById('panelText').innerHTML = panelContent[key].text;
  panel.style.display='block';
}

function closePanel(){
  document.getElementById('infoPanel').style.display='none';
}

// Widget hesaplama
function toggleFloatingWidget(){
  const w=document.getElementById('floatingNetWidget');
  if(w.style.transform==='scale(1)'){
    w.style.transform='scale(0)';
    w.style.opacity='0';
    setTimeout(()=>{w.style.display='none';},200);
  }else{
    w.style.display='block';
    setTimeout(()=>{w.style.transform='scale(1)'; w.style.opacity='1';},10);
  }
}

function hesaplaFloatingNet(){
  let hedef=parseFloat(document.getElementById('floatingHedefPuan').value)||0;
  if(hedef<0) hedef=0;
  if(hedef>500) hedef=500;
  document.getElementById('floatingHedefPuan').value=hedef;
  document.getElementById('floatingTytNet').innerText=Math.round((hedef*0.4)/4);
  document.getElementById('floatingAytNet').innerText=Math.round((hedef*0.6)/5);
}
</script>

</body>
</html>

<style>
/* Responsive: Mobilde input daralıyor, widget sağ alt köşede */
@media (max-width: 768px) {
    #floatingNetWidget {
        width: 160px;
        right: 5px;
    }
    #floatingNetButtonWrapper {
        right: 5px;
        align-items: flex-end;
    }
    #floatingHedefPuan {
        width: 70px !important; /* mobilde input daralıyor */
    }
}
</style>

<!-- 🔵 BANNER -->
<div class="w-full bg-white">
  <img src="images/banner.jpg"
       alt="YKS Net ve Puan Hesaplama"
       class="w-full h-[45vh] sm:h-[55vh] object-contain">
</div>

<!-- Banner altı animasyonlu kutular -->
<div class="grid grid-cols-3 gap-4 my-8">

  <div class="bg-white rounded-2xl shadow flex flex-col items-center justify-center
              aspect-square text-center transition-transform duration-300 hover:-translate-y-2">
    <div class="text-4xl mb-2">✍️</div>
    <div class="font-semibold text-base md:text-lg leading-tight">
      Kolay Net Girişi
    </div>
  </div>

  <div class="bg-white rounded-2xl shadow flex flex-col items-center justify-center
              aspect-square text-center transition-transform duration-300 hover:-translate-y-2">
    <div class="text-4xl mb-2">⚡</div>
    <div class="font-semibold text-base md:text-lg leading-tight">
      Hızlı Puan Hesaplama
    </div>
  </div>

  <div class="bg-white rounded-2xl shadow flex flex-col items-center justify-center
              aspect-square text-center transition-transform duration-300 hover:-translate-y-2">
    <div class="text-4xl mb-2">📱</div>
    <div class="font-semibold text-base md:text-lg leading-tight">
      Mobil Uyumlu
    </div>
  </div>

</div>
<!-- Bilgi Rozetleri -->
<div class="flex flex-wrap gap-2 mb-4">
  <span class="px-3 py-1 text-xs rounded-full bg-blue-100 text-blue-700">
    📊 Anlık Net Hesaplama
  </span>
  <span class="px-3 py-1 text-xs rounded-full bg-green-100 text-green-700">
    🎯 2026 Güncel YKS Sistemi
  </span>
  <span class="px-3 py-1 text-xs rounded-full bg-purple-100 text-purple-700">
    ⚡ Ücretsiz & Hızlı
  </span>
</div>

<!-- 🔵 HESAPLAMA ALANI -->
<div class="max-w-2xl mx-auto bg-white p-6 rounded-2xl shadow-lg">

<h1 class="text-3xl font-extrabold text-center mb-2 text-gray-800">
YKS Net & Puan Hesaplama
</h1>

<!-- TYT -->
<h2 class="text-xl font-semibold mb-2">TYT</h2>
<div class="grid grid-cols-3 gap-2 mb-4 text-sm sm:text-base">
  <span>Ders</span><span>Doğru</span><span>Yanlış</span>

  <span>Türkçe</span>
  <input id="t_d" type="number" min="0" max="40" class="border p-1">
  <input id="t_y" type="number" min="0" max="40" class="border p-1">

  <span>Matematik</span>
  <input id="m_d" type="number" min="0" max="40" class="border p-1">
  <input id="m_y" type="number" min="0" max="40" class="border p-1">

  <span>Sosyal</span>
  <input id="s_d" type="number" min="0" max="20" class="border p-1">
  <input id="s_y" type="number" min="0" max="20" class="border p-1">

  <span>Fen</span>
  <input id="f_d" type="number" min="0" max="20" class="border p-1">
  <input id="f_y" type="number" min="0" max="20" class="border p-1">
</div>

<!-- AYT -->
<h2 class="text-xl font-semibold mb-2">AYT</h2>
<div class="grid grid-cols-3 gap-2 mb-4 text-sm sm:text-base">
  <span>Matematik</span>
  <input id="am_d" type="number" min="0" max="40" class="border p-1">
  <input id="am_y" type="number" min="0" max="40" class="border p-1">

  <span>Edebiyat</span>
  <input id="ae_d" type="number" min="0" max="40" class="border p-1">
  <input id="ae_y" type="number" min="0" max="40" class="border p-1">

  <span>Sosyal</span>
  <input id="as_d" type="number" min="0" max="40" class="border p-1">
  <input id="as_y" type="number" min="0" max="40" class="border p-1">

  <span>Fen</span>
  <input id="af_d" type="number" min="0" max="40" class="border p-1">
  <input id="af_y" type="number" min="0" max="40" class="border p-1">
</div>

<!-- DİPLOMA -->
<label class="block mb-3 font-semibold">
Diploma Notu (0 – 100)
<input id="diploma" type="number" min="0" max="100"
       class="border p-2 w-full mt-1">
</label>

<button onclick="hesapla()"
class="w-full bg-blue-600 text-white p-2 rounded hover:bg-blue-700 transition">
Hesapla
</button>

<div id="sonuc" class="mt-4 text-center font-semibold"></div>
<canvas id="grafik" class="mt-6"></canvas>

</div>

<!-- FOOTER -->
<footer class="text-center text-sm mt-8 mb-4">
  <a href="hakkimizda.html">Hakkımızda</a> |
  <a href="gizlilik.html">Gizlilik Politikası</a> |
  <a href="iletisim.html">İletişim</a>
</footer>

<script>
function net(d, y, max) {
  d = Number(d); y = Number(y);
  if (d < 0 || y < 0 || d + y > max) return null;
  return d - (y / 4);
}

function hesapla() {
  const n = [
    net(t_d.value, t_y.value, 40),
    net(m_d.value, m_y.value, 40),
    net(s_d.value, s_y.value, 20),
    net(f_d.value, f_y.value, 20),
    net(am_d.value, am_y.value, 40),
    net(ae_d.value, ae_y.value, 40),
    net(as_d.value, as_y.value, 40),
    net(af_d.value, af_y.value, 40)
  ];

  if (n.includes(null)) {
    alert("❌ Doğru–yanlış sayıları geçerli aralıklarda olmalı!");
    return;
  }

  const diploma = Number(document.getElementById("diploma").value);
  if (diploma < 0 || diploma > 100) {
    alert("❌ Diploma notu 0–100 arasında olmalı!");
    return;
  }

  const tytNet = n.slice(0,4).reduce((a,b)=>a+b,0);
  const aytNet = n.slice(4).reduce((a,b)=>a+b,0);

  const tytPuan = 100 + tytNet * 4 + diploma * 0.12;
  const aytPuan = 100 + aytNet * 5 + diploma * 0.12;

  document.getElementById("sonuc").innerHTML = `
  TYT Net: ${tytNet.toFixed(2)}<br>
  AYT Net: ${aytNet.toFixed(2)}<br><br>
  TYT Puan: ${tytPuan.toFixed(2)}<br>
  AYT Puan: ${aytPuan.toFixed(2)}
  `;

  if (window.chart) window.chart.destroy();
  window.chart = new Chart(document.getElementById("grafik"), {
    type: "bar",
    data: {
      labels: ["TYT", "AYT"],
      datasets: [{
        label: "Puan",
        data: [tytPuan, aytPuan]
      }]
    }
  });
}
</script>
