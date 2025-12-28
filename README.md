<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
     
<meta name="google-site-verification" content="8px4ptCkSDsTG0QElNpXCS4McjaAd4RbFuTWkZKxn4Y" />
     
<title>YKS Net ve Puan Hesaplama</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdn.tailwindcss.com"></script>
</head>
     


<body class="bg-gray-100 p-6">
<div class="max-w-2xl mx-auto bg-white p-6 rounded-xl shadow">

<h1 class="text-2xl font-bold text-center mb-6">YKS Net & Puan Hesaplama</h1>
<img src="images/banner.jpg"
     alt="YKS Net ve Puan Hesaplama"
     class="w-full rounded-xl mb-6">


<!-- TYT -->
<h2 class="text-xl font-semibold mb-2">TYT</h2>
<div class="grid grid-cols-3 gap-2 mb-4">
  <span>Ders</span><span>Doğru</span><span>Yanlış</span>

  <span>Türkçe</span>
  <input id="t_dogru" type="number" class="border p-1">
  <input id="t_yanlis" type="number" class="border p-1">

  <span>Matematik</span>
  <input id="m_dogru" type="number" class="border p-1">
  <input id="m_yanlis" type="number" class="border p-1">

  <span>Sosyal</span>
  <input id="s_dogru" type="number" class="border p-1">
  <input id="s_yanlis" type="number" class="border p-1">

  <span>Fen</span>
  <input id="f_dogru" type="number" class="border p-1">
  <input id="f_yanlis" type="number" class="border p-1">
</div>

<!-- AYT -->
<h2 class="text-xl font-semibold mb-2">AYT</h2>
<div class="grid grid-cols-3 gap-2 mb-4">
  <span>Matematik</span>
  <input id="ayt_m_d" type="number" class="border p-1">
  <input id="ayt_m_y" type="number" class="border p-1">

  <span>Edebiyat</span>
  <input id="ayt_e_d" type="number" class="border p-1">
  <input id="ayt_e_y" type="number" class="border p-1">

  <span>Sosyal</span>
  <input id="ayt_s_d" type="number" class="border p-1">
  <input id="ayt_s_y" type="number" class="border p-1">

  <span>Fen</span>
  <input id="ayt_f_d" type="number" class="border p-1">
  <input id="ayt_f_y" type="number" class="border p-1">
</div>

<label class="block mb-3">Diploma Notu:
  <input id="diploma" type="number" class="border p-1 w-full">
</label>

<button onclick="hesapla()" class="w-full bg-blue-600 text-white p-2 rounded">
Hesapla
</button>

<div id="sonuc" class="mt-4 font-semibold text-center"></div>
<canvas id="grafik" class="mt-6"></canvas>

</div>

<script>
function net(d, y) {
  return d - (y / 4);
}

function hesapla() {
  // TYT netleri
  const tytNet =
    net(t_dogru.value, t_yanlis.value) +
    net(m_dogru.value, m_yanlis.value) +
    net(s_dogru.value, s_yanlis.value) +
    net(f_dogru.value, f_yanlis.value);

  // AYT netleri
  const aytNet =
    net(ayt_m_d.value, ayt_m_y.value) +
    net(ayt_e_d.value, ayt_e_y.value) +
    net(ayt_s_d.value, ayt_s_y.value) +
    net(ayt_f_d.value, ayt_f_y.value);

  const diploma = Number(document.getElementById("diploma").value) || 0;

  // Tahmini puan
  const tytPuan = 100 + tytNet * 4 + diploma * 0.12;
  const aytPuan = 100 + aytNet * 5 + diploma * 0.12;

  document.getElementById("sonuc").innerHTML = `
    TYT Net: ${tytNet.toFixed(2)} <br>
    AYT Net: ${aytNet.toFixed(2)} <br><br>
    TYT Puan: ${tytPuan.toFixed(2)} <br>
    AYT Puan: ${aytPuan.toFixed(2)}
  `;

  // Grafik
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
<hr>
<footer style="text-align:center; margin:20px 0;">
  <a href="hakkimizda.html">Hakkımızda</a> |
  <a href="gizlilik.html">Gizlilik Politikası</a> |
  <a href="iletisim.html">İletişim</a>
</footer>


