
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <!-- SEO -->
  <title>YKS Net ve Puan Hesaplama | TYT AYT 2026</title>
  <meta name="description" content="YKS 2026 TYT ve AYT net ve puan hesaplama aracı. Doğru yanlış gir, netini ve tahmini puanını hemen öğren.">
  <meta name="keywords" content="YKS puan hesaplama, TYT net, AYT net, YKS 2026">

  <!-- Kütüphaneler -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
</head>

<body class="bg-gray-100 p-4">

<div class="max-w-3xl mx-auto bg-white p-6 rounded-xl shadow">

  <h1 class="text-3xl font-bold text-center mb-4">
    YKS Net & Puan Hesaplama
  </h1>

  <!-- Banner -->
  <img src="images/banner.jpg"
       alt="YKS Net ve Puan Hesaplama"
       class="w-full rounded-xl mb-6">

  <!-- TYT -->
  <h2 class="text-xl font-semibold mb-2">TYT</h2>
  <div class="grid grid-cols-3 gap-2 mb-4 text-sm sm:text-base">
    <span>Ders</span><span>Doğru</span><span>Yanlış</span>

    <span>Türkçe</span>
    <input id="t_d" type="number" min="0" max="40" class="border p-2">
    <input id="t_y" type="number" min="0" max="40" class="border p-2">

    <span>Matematik</span>
    <input id="m_d" type="number" min="0" max="40" class="border p-2">
    <input id="m_y" type="number" min="0" max="40" class="border p-2">

    <span>Sosyal</span>
    <input id="s_d" type="number" min="0" max="20" class="border p-2">
    <input id="s_y" type="number" min="0" max="20" class="border p-2">

    <span>Fen</span>
    <input id="f_d" type="number" min="0" max="20" class="border p-2">
    <input id="f_y" type="number" min="0" max="20" class="border p-2">
  </div>

  <!-- AYT -->
  <h2 class="text-xl font-semibold mb-2">AYT</h2>
  <div class="grid grid-cols-3 gap-2 mb-4 text-sm sm:text-base">
    <span>Matematik</span>
    <input id="ayt_m_d" type="number" min="0" max="40" class="border p-2">
    <input id="ayt_m_y" type="number" min="0" max="40" class="border p-2">

    <span>Edebiyat</span>
    <input id="ayt_e_d" type="number" min="0" max="24" class="border p-2">
    <input id="ayt_e_y" type="number" min="0" max="24" class="border p-2">

    <span>Sosyal</span>
    <input id="ayt_s_d" type="number" min="0" max="20" class="border p-2">
    <input id="ayt_s_y" type="number" min="0" max="20" class="border p-2">

    <span>Fen</span>
    <input id="ayt_f_d" type="number" min="0" max="20" class="border p-2">
    <input id="ayt_f_y" type="number" min="0" max="20" class="border p-2">
  </div>

  <!-- Diploma -->
  <label class="block mb-3 font-medium">
    Diploma Notu (0 - 100):
    <input id="diploma" type="number" min="0" max="100"
           class="border p-2 w-full mt-1">
  </label>

  <!-- Hata -->
  <div id="hata"
       class="hidden bg-red-100 text-red-700 p-2 rounded mb-3 text-center">
  </div>

  <button onclick="hesapla()"
          class="w-full bg-blue-600 text-white p-3 rounded text-lg">
    Hesapla
  </button>

  <!-- Sonuç -->
  <div id="sonuc" class="mt-4 font-semibold text-center text-lg"></div>

  <canvas id="grafik" class="mt-6"></canvas>

</div>

<!-- Footer -->
<footer class="text-center text-sm mt-6">
  <a href="hakkimizda.html">Hakkımızda</a> |
  <a href="gizlilik.html">Gizlilik Politikası</a> |
  <a href="iletisim.html">İletişim</a>
</footer>

<script>
function net(d, y) {
  d = Number(d) || 0;
  y = Number(y) || 0;
  return d - (y / 4);
}

function val(id) {
  return document.getElementById(id).value;
}

function hesapla() {
  const hata = document.getElementById("hata");
  hata.classList.add("hidden");

  const diploma = Number(val("diploma"));
  if (isNaN(diploma) || diploma < 0 || diploma > 100) {
    hata.innerText = "❌ Diploma notu 0 ile 100 arasında olmalıdır.";
    hata.classList.remove("hidden");
    return;
  }

  const tytNet =
    net(val("t_d"), val("t_y")) +
    net(val("m_d"), val("m_y")) +
    net(val("s_d"), val("s_y")) +
    net(val("f_d"), val("f_y"));

  const aytNet =
    net(val("ayt_m_d"), val("ayt_m_y")) +
    net(val("ayt_e_d"), val("ayt_e_y")) +
    net(val("ayt_s_d"), val("ayt_s_y")) +
    net(val("ayt_f_d"), val("ayt_f_y"));

  const tytPuan = 100 + tytNet * 4 + diploma * 0.12;
  const aytPuan = 100 + aytNet * 5 + diploma * 0.12;

  document.getElementById("sonuc").innerHTML = `
    TYT Net: ${tytNet.toFixed(2)}<br>
    AYT Net: ${aytNet.toFixed(2)}<br><br>
    TYT Puan: ${tytPuan.toFixed(2)}<br>
    AYT Puan: ${aytPuan.toFixed(2)}
  `;

  const ctx = document.getElementById("grafik");
  if (window.chart) window.chart.destroy();

  window.chart = new Chart(ctx, {
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
