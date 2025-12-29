
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
<style>
<!-- Sabit Olmayan, Kutucuk Panel -->
<style>
#netCalculator {
    position: relative; /* artık sabit değil */
    width: 220px;
    background: linear-gradient(135deg, #ffecd2, #fcb69f);
    border-radius: 12px;
    padding: 15px;
    box-shadow: 0 6px 12px rgba(0,0,0,0.15);
    font-family: 'Poppins', sans-serif;
    font-size: 14px;
    margin: 20px; /* sayfa içinde biraz boşluk */
}
#netCalculator button {
    width:100%;
    padding:5px;
    border:none;
    background:#ff7e5f;
    color:white;
    border-radius:8px;
    font-size:13px;
    transition:0.3s;
}
#netCalculator button:hover {
    background: #feb47b;
}
#netCalculator input {
    width:60px;
    padding:4px;
    border-radius:6px;
    border:1px solid #ccc;
    margin-top:4px;
}
#netCalculator span {
    transition: all 0.3s ease;
}

/* Mobil uyumlu */
@media screen and (max-width: 500px) {
    #netCalculator {
        width: 150px;
        padding: 10px;
        font-size: 12px;
        margin: 10px;
    }
    #netCalculator button {
        padding: 4px;
        font-size: 12px;
    }
    #netCalculator input {
        width: 50px;
        padding: 3px;
    }
}
</style>

<div id="netCalculator">
  <h4 style="margin:0 0 10px 0; font-size:15px; text-align:center;">Kaç Net Yapmalıyım?</h4>
  
  <label>Hedef Puan:<br>
    <input type="number" id="hedefPuan" min="0" max="500">
  </label><br><br>
  
  <button onclick="hesaplaNet()">Hesapla</button>
  
  <div id="netSonuc" style="margin-top:10px;">
    <p>📘 TYT Net: <span id="tytNetSonuc">0</span></p>
    <p>📗 AYT Net: <span id="aytNetSonuc">0</span></p>
  </div>
</div>

<script>
function hesaplaNet() {
    let hedefInput = document.getElementById('hedefPuan');
    let hedef = parseFloat(hedefInput.value) || 0;

    if (hedef < 0) hedef = 0;
    if (hedef > 500) {
        hedef = 500;
        hedefInput.value = 500;
    }

    const tytNet = Math.round((hedef * 0.4) / 4); 
    const aytNet = Math.round((hedef * 0.6) / 5); 

    let tytEl = document.getElementById('tytNetSonuc');
    let aytEl = document.getElementById('aytNetSonuc');

    tytEl.innerText = tytNet;
    aytEl.innerText = aytNet;

    tytEl.style.transform = "scale(1.2)";
    aytEl.style.transform = "scale(1.2)";
    setTimeout(()=>{ tytEl.style.transform="scale(1)"; aytEl.style.transform="scale(1)"; },300);
}
</script>

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
