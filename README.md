
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>YKS Net ve Puan Hesaplama | TYT AYT 2026</title>
<meta name="description" content="YKS 2026 TYT ve AYT net hesaplama aracı. Doğru yanlış girerek netini ve tahmini puanını hemen öğren.">
<meta name="keywords" content="YKS puan hesaplama, TYT net, AYT net, YKS 2026">

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdn.tailwindcss.com"></script>
</head>

<body class="bg-gray-100 p-4">

<div class="max-w-2xl mx-auto bg-white p-6 rounded-xl shadow">

<h1 class="text-2xl font-bold text-center mb-6">
YKS Net & Puan Hesaplama
</h1>

<img src="images/banner.jpg"
     alt="YKS Net ve Puan Hesaplama"
     class="w-full h-[70vh] sm:h-auto object-cover rounded-xl mb-6">


<!-- HATA MESAJI -->
<div id="hata" class="hidden bg-red-100 text-red-700 p-3 rounded mb-4 text-center font-semibold"></div>

<!-- TYT -->
<h2 class="text-xl font-semibold mb-2">TYT</h2>
<div class="grid grid-cols-3 gap-2 mb-4 text-sm sm:text-base">
  <span>Ders</span><span>Doğru</span><span>Yanlış</span>

  <span>Türkçe</span>
  <input id="t_d" type="number" min="0" class="border p-1">
  <input id="t_y" type="number" min="0" class="border p-1">

  <span>Matematik</span>
  <input id="m_d" type="number" min="0" class="border p-1">
  <input id="m_y" type="number" min="0" class="border p-1">

  <span>Sosyal</span>
  <input id="s_d" type="number" min="0" class="border p-1">
  <input id="s_y" type="number" min="0" class="border p-1">

  <span>Fen</span>
  <input id="f_d" type="number" min="0" class="border p-1">
  <input id="f_y" type="number" min="0" class="border p-1">
</div>

<!-- AYT -->
<h2 class="text-xl font-semibold mb-2">AYT</h2>
<div class="grid grid-cols-3 gap-2 mb-4 text-sm sm:text-base">

  <span>Matematik</span>
  <input id="ayt_m_d" type="number" min="0" class="border p-1">
  <input id="ayt_m_y" type="number" min="0" class="border p-1">

  <span>Edebiyat</span>
  <input id="ayt_e_d" type="number" min="0" class="border p-1">
  <input id="ayt_e_y" type="number" min="0" class="border p-1">

  <span>Sosyal</span>
  <input id="ayt_s_d" type="number" min="0" class="border p-1">
  <input id="ayt_s_y" type="number" min="0" class="border p-1">

  <span>Fen</span>
  <input id="ayt_f_d" type="number" min="0" class="border p-1">
  <input id="ayt_f_y" type="number" min="0" class="border p-1">
</div>

<label class="block mb-4 font-semibold">
Diploma Notu (0 – 100)
<input id="diploma" type="number" min="0" max="100" class="border p-2 w-full mt-1">
</label>

<button onclick="hesapla()" class="w-full bg-blue-600 text-white p-3 rounded font-bold">
Hesapla
</button>

<div id="sonuc" class="mt-4 font-semibold text-center"></div>
<canvas id="grafik" class="mt-6"></canvas>

</div>

<footer class="text-center text-sm mt-6">
<a href="hakkimizda.html">Hakkımızda</a> |
<a href="gizlilik.html">Gizlilik Politikası</a> |
<a href="iletisim.html">İletişim</a>
</footer>

<script>
function net(d, y) {
  return d - (y / 4);
}

function v(id) {
  return Number(document.getElementById(id).value) || 0;
}

function kontrol(d, y, max, ad) {
  if (d < 0 || y < 0) return ad + ": Negatif değer girilemez";
  if (d + y > max) return ad + ": Toplam soru sayısı aşıldı";
  return null;
}

function hesapla() {
  const hata = document.getElementById("hata");
  hata.classList.add("hidden");
  hata.innerHTML = "";

  const diploma = v("diploma");
  if (diploma < 0 || diploma > 100) {
    hata.innerText = "❌ Diploma notu 0 ile 100 arasında olmalıdır.";
    hata.classList.remove("hidden");
    return;
  }

  const kontroller = [
    kontrol(v("t_d"), v("t_y"), 40, "TYT Türkçe"),
    kontrol(v("m_d"), v("m_y"), 40, "TYT Matematik"),
    kontrol(v("s_d"), v("s_y"), 20, "TYT Sosyal"),
    kontrol(v("f_d"), v("f_y"), 20, "TYT Fen"),

    kontrol(v("ayt_m_d"), v("ayt_m_y"), 40, "AYT Matematik"),
    kontrol(v("ayt_e_d"), v("ayt_e_y"), 24, "AYT Edebiyat"),
    kontrol(v("ayt_s_d"), v("ayt_s_y"), 20, "AYT Sosyal"),
    kontrol(v("ayt_f_d"), v("ayt_f_y"), 20, "AYT Fen")
  ];

  const hataMesaji = kontroller.find(x => x);
  if (hataMesaji) {
    hata.innerText = "❌ " + hataMesaji;
    hata.classList.remove("hidden");
    return;
  }

  const tytNet =
    net(v("t_d"), v("t_y")) +
    net(v("m_d"), v("m_y")) +
    net(v("s_d"), v("s_y")) +
    net(v("f_d"), v("f_y"));

  const aytNet =
    net(v("ayt_m_d"), v("ayt_m_y")) +
    net(v("ayt_e_d"), v("ayt_e_y")) +
    net(v("ayt_s_d"), v("ayt_s_y")) +
    net(v("ayt_f_d"), v("ayt_f_y"));

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
