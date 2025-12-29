
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>YKS Net ve Puan Hesaplama | TYT AYT 2026</title>
<meta name="description" content="YKS TYT ve AYT net hesaplama aracı. Doğru yanlış girerek netini ve tahmini puanını hemen öğren. Ücretsiz.">

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdn.tailwindcss.com"></script>
</head>
<style>
/* Genel arka plan */
body {
  background: linear-gradient(135deg, #f3f4f6, #e5e7eb);
}

/* Ana kart */
.main-card {
  animation: fadeIn 0.6s ease-in-out;
}

/* Input güzelleştirme */
input {
  border-radius: 0.5rem;
  transition: all 0.2s ease;
}

input:focus {
  outline: none;
  border-color: #2563eb;
  box-shadow: 0 0 0 2px rgba(37,99,235,0.2);
}

/* Hesapla butonu */
button {
  transition: all 0.25s ease;
}

button:hover {
  transform: translateY(-1px);
  box-shadow: 0 10px 20px rgba(37,99,235,0.25);
}

/* Sonuç alanı */
#sonuc {
  background: #f8fafc;
  border-radius: 0.75rem;
  padding: 1rem;
  margin-top: 1rem;
  animation: fadeIn 0.5s ease-in-out;
}

/* Banner görseli */
.banner-img {
  max-height: 260px;
  object-fit: cover;
}

/* Animasyon */
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>


<body class="bg-gray-100">

<!-- 🔵 BANNER -->
<div class="w-full bg-white">
  <img src="images/banner.jpg"
     alt="YKS Net ve Puan Hesaplama"
     class="banner-img w-full rounded-xl mb-6">

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
<div class="main-card max-w-2xl mx-auto bg-white p-6 rounded-xl shadow-xl">

<h1 class="text-2xl font-bold text-center mb-6">
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
<script>
function net(d, y) {
  return d - (y / 4);
}

function hesapla() {
  const tNet = net(t_dogru.value, t_yanlis.value);
  const mNet = net(m_dogru.value, m_yanlis.value);
  const sNet = net(s_dogru.value, s_yanlis.value);
  const fNet = net(f_dogru.value, f_yanlis.value);

  // Önceki grafik varsa sil
  if (window.netChart) {
    window.netChart.destroy();
  }

  const ctx = document.getElementById("netGrafik").getContext("2d");

  window.netChart = new Chart(ctx, {
    type: "bar",
    data: {
      labels: ["Türkçe", "Matematik", "Sosyal", "Fen"],
      datasets: [{
        label: "Netler",
        data: [tNet, mNet, sNet, fNet],
        backgroundColor: [
          "#3b82f6",
          "#22c55e",
          "#f59e0b",
          "#ef4444"
        ],
        borderRadius: 8
      }]
    },
    options: {
      responsive: true,
      scales: {
        y: {
          beginAtZero: true
        }
      }
    }
  });
}
</script>


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

</body>
</html>
