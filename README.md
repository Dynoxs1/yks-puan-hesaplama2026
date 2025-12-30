
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>YKS Net ve Puan Hesaplama | TYT AYT 2026</title>
<meta name="description" content="YKS TYT ve AYT net hesaplama aracı. Doğru yanlış girerek netini ve tahmini puanını hemen öğren. Ücretsiz.">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdn.tailwindcss.com"></script>
</head>

<body class="bg-gray-100">

<!-- Başlık -->
<header style="padding:20px;text-align:center;font-size:24px;font-weight:600;background:linear-gradient(135deg,#ffecd2,#fcb69f);box-shadow:0 4px 8px rgba(0,0,0,0.1);">
🎯 YKS Destek Sitesi
</header>

<!-- Başlık Altı Butonlar -->
<div style="max-width:1200px;margin:20px auto;padding:0 20px;">
  <button class="btn" onclick="openPanel('puan')" style="margin-right:10px;">Puan İpuçları</button>
  <button class="btn" onclick="openPanel('tavsiye')">Sınav Tavsiyeleri</button>

  <!-- Açılır Panel -->
  <div id="infoPanel" class="panel hidden" style="background:white;border-radius:12px;padding:15px;box-shadow:0 6px 12px rgba(0,0,0,0.15);margin-top:10px;position:relative;">
    <button onclick="closePanel()" style="position:absolute;top:5px;right:5px;border:none;background:transparent;font-size:18px;cursor:pointer;">×</button>
    <h4 id="panelTitle" style="color:#ff7e5f;margin-bottom:10px;"></h4>
    <p id="panelText" style="font-size:14px;color:#555;"></p>
  </div>
</div>

<!-- 🔵 Kaç Net Yapmalıyım Widget Başlangıç -->
<div id="floatingNetButtonWrapper" style="position: fixed; bottom: 90px; right: 10px; z-index: 9999; display: flex; flex-direction: column; align-items: flex-end; gap:4px;">
  <div style="background: rgba(255,255,255,0.9); padding: 3px 8px; border-radius: 6px; font-size: 12px; box-shadow: 0 2px 5px rgba(0,0,0,0.15); color: #333;">Hedef puanınızı hesaplayın</div>
  <button id="floatingNetButton" onclick="toggleFloatingWidget()" style="width:50px; height:50px; border-radius:50%; border:none; background:#ff7e5f; color:white; font-size:24px; cursor:pointer; box-shadow:0 4px 8px rgba(0,0,0,0.2); transition:0.3s; position: relative;" onmouseover="this.style.background='#feb47b'" onmouseout="this.style.background='#ff7e5f'">📝</button>
</div>

<div id="floatingNetWidget" style="position: fixed; bottom: 80px; right: 10px; width: 180px; background: #ffecd2; border-radius: 12px; padding: 10px; font-family: 'Poppins', sans-serif; font-size: 13px; box-shadow: 0 4px 10px rgba(0,0,0,0.15); display:none; transform: scale(0); opacity: 0; transition: transform 0.2s, opacity 0.2s; z-index: 9999;">
  <button onclick="toggleFloatingWidget()" style="position: absolute; top: -5px; right: -5px; border: none; background: transparent; font-size: 16px; font-weight: bold; cursor: pointer; color: #333;">×</button>

  <div style="display:flex; gap:2px; align-items:center;">
    <input type="number" id="floatingHedefPuan" placeholder="Hedef" style="width: 90px; padding:5px; border-radius:5px; border:1px solid #ccc; font-size:13px; transition: border 0.2s;" onfocus="this.style.borderColor='#ff7e5f'" onblur="this.style.borderColor='#ccc'">
    <button onclick="hesaplaFloatingNet()" style="padding:5px 8px; font-size:13px; border:none; border-radius:5px; background:#ff7e5f; color:white; cursor:pointer; transition:0.3s;" onmouseover="this.style.background='#feb47b'" onmouseout="this.style.background='#ff7e5f'">Hesapla</button>
  </div>

  <div id="floatingNetSonuc" style="margin-top:8px; display:flex; justify-content:space-between;">
    <div>📘 TYT: <span id="floatingTytNet">0</span></div>
    <div>📗 AYT: <span id="floatingAytNet">0</span></div>
  </div>
</div>

<script>
function toggleFloatingWidget() {
    const widget = document.getElementById('floatingNetWidget');
    if(widget.style.display === "none" || widget.style.display === "") {
        widget.style.display = "block";
        setTimeout(()=>{
            widget.style.transform = "scale(1)";
            widget.style.opacity = "1";
        },10);
    } else {
        widget.style.transform = "scale(0)";
        widget.style.opacity = "0";
        setTimeout(()=>{ widget.style.display="none"; },200);
    }
}

function hesaplaFloatingNet() {
    let hedef = parseFloat(document.getElementById('floatingHedefPuan').value) || 0;
    if (hedef < 0) hedef = 0;
    if (hedef > 500) hedef = 500;
    document.getElementById('floatingHedefPuan').value = hedef;

    const tytNet = Math.round((hedef * 0.4) / 4);
    const aytNet = Math.round((hedef * 0.6) / 5);

    document.getElementById('floatingTytNet').innerText = tytNet;
    document.getElementById('floatingAytNet').innerText = aytNet;

    let tytEl = document.getElementById('floatingTytNet');
    let aytEl = document.getElementById('floatingAytNet');

    tytEl.style.transform = "scale(1.2)";
    aytEl.style.transform = "scale(1.2)";
    setTimeout(()=>{ tytEl.style.transform="scale(1)"; aytEl.style.transform="scale(1)"; },250);
}

// Puan İpuçları ve Sınav Tavsiyeleri Paneli
const panelContent = {
    puan: { title: "Puan İpuçları", text: "Ders planı yapın, deneme çözün ve yanlışlarınızı analiz edin. Günlük küçük hedefler motivasyonu artırır." },
    tavsiye: { title: "Sınav Tavsiyeleri", text: "Sınav günü stres yönetimi önemlidir. Zamanı verimli kullanın, soru tiplerini önceden çalışın ve kısa molalar verin." }
};

function openPanel(key){
    const panel = document.getElementById('infoPanel');
    document.getElementById('panelTitle').innerText = panelContent[key].title;
    document.getElementById('panelText').innerText = panelContent[key].text;
    panel.classList.remove('hidden');
}

function closePanel(){
    document.getElementById('infoPanel').classList.add('hidden');
}
</script>

<style>
@media (max-width: 768px) {
    #floatingNetWidget { width: 160px; right: 5px; }
    #floatingNetButtonWrapper { right: 5px; align-items: flex-end; }
    #floatingHedefPuan { width: 70px !important; }
}
.btn{display:inline-block;padding:8px 14px;border:none;border-radius:8px;background:#ff7e5f;color:white;cursor:pointer;font-size:14px;transition:0.3s;text-decoration:none;text-align:center;}
.btn:hover{background:#feb47b;}
.hidden{display:none;}
.panel{position:relative;}
</style>

</body>
</html>

<!-- Floating Mini Widget Butonu + Açıklama -->
<div id="floatingNetButtonWrapper" style="position: fixed; bottom: 90px; right: 10px; z-index: 9999; display: flex; flex-direction: column; align-items: flex-end; gap:4px;">
  
  <!-- Açıklama -->
  <div style="
      background: rgba(255,255,255,0.9);
      padding: 3px 8px;
      border-radius: 6px;
      font-size: 12px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.15);
      color: #333;
  ">Hedef puanınızı hesaplayın</div>

  <!-- İkon -->
  <button id="floatingNetButton" onclick="toggleFloatingWidget()" style="
      width:50px;
      height:50px;
      border-radius:50%;
      border:none;
      background:#ff7e5f;
      color:white;
      font-size:24px;
      cursor:pointer;
      box-shadow:0 4px 8px rgba(0,0,0,0.2);
      transition:0.3s;
      position: relative;
  " onmouseover="this.style.background='#feb47b'" onmouseout="this.style.background='#ff7e5f'">📝</button>
</div>

<!-- Floating Widget -->
<div id="floatingNetWidget" style="
    position: fixed;
    bottom: 80px;
    right: 10px;
    width: 180px;
    background: #ffecd2;
    border-radius: 12px;
    padding: 10px;
    font-family: 'Poppins', sans-serif;
    font-size: 13px;
    box-shadow: 0 4px 10px rgba(0,0,0,0.15);
    display:none;
    transform: scale(0);
    opacity: 0;
    transition: transform 0.2s, opacity 0.2s;
    z-index: 9999;
">
  <!-- Kapatma Butonu -->
  <button onclick="toggleFloatingWidget()" style="
      position: absolute;
      top: -5px;
      right: -5px;
      border: none;
      background: transparent;
      font-size: 16px;
      font-weight: bold;
      cursor: pointer;
      color: #333;
  ">×</button>

  <!-- Hedef ve Hesapla -->
  <div style="display:flex; gap:2px; align-items:center;">
    <input type="number" id="floatingHedefPuan" placeholder="Hedef" style="
        width: 90px;  /* desktop genişliği */
        padding:5px;
        border-radius:5px;
        border:1px solid #ccc;
        font-size:13px;
        transition: border 0.2s;
    " onfocus="this.style.borderColor='#ff7e5f'" onblur="this.style.borderColor='#ccc'">
    
    <button onclick="hesaplaFloatingNet()" style="
        padding:5px 8px;
        font-size:13px;
        border:none;
        border-radius:5px;
        background:#ff7e5f;
        color:white;
        cursor:pointer;
        transition:0.3s;
    " onmouseover="this.style.background='#feb47b'" onmouseout="this.style.background='#ff7e5f'">Hesapla</button>
  </div>

  <!-- Net Sonuçları -->
  <div id="floatingNetSonuc" style="margin-top:8px; display:flex; justify-content:space-between;">
    <div>📘 TYT: <span id="floatingTytNet">0</span></div>
    <div>📗 AYT: <span id="floatingAytNet">0</span></div>
  </div>
</div>

<script>
function toggleFloatingWidget() {
    const widget = document.getElementById('floatingNetWidget');
    if(widget.style.display === "none" || widget.style.display === "") {
        widget.style.display = "block";
        setTimeout(()=>{
            widget.style.transform = "scale(1)";
            widget.style.opacity = "1";
        },10);
    } else {
        widget.style.transform = "scale(0)";
        widget.style.opacity = "0";
        setTimeout(()=>{ widget.style.display="none"; },200);
    }
}

function hesaplaFloatingNet() {
    let hedef = parseFloat(document.getElementById('floatingHedefPuan').value) || 0;
    if (hedef < 0) hedef = 0;
    if (hedef > 500) hedef = 500;

    document.getElementById('floatingHedefPuan').value = hedef;

    const tytNet = Math.round((hedef * 0.4) / 4);
    const aytNet = Math.round((hedef * 0.6) / 5);

    document.getElementById('floatingTytNet').innerText = tytNet;
    document.getElementById('floatingAytNet').innerText = aytNet;

    let tytEl = document.getElementById('floatingTytNet');
    let aytEl = document.getElementById('floatingAytNet');

    tytEl.style.transform = "scale(1.2)";
    aytEl.style.transform = "scale(1.2)";
    setTimeout(()=>{ tytEl.style.transform="scale(1)"; aytEl.style.transform="scale(1)"; },250);
}
</script>

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

</body><body class="bg-gradient-to-br from-blue-50 to-gray-100 min-h-screen p-4">
