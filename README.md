<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>YKS Net ve Puan Hesaplama | TYT AYT 2026</title>
  <meta name="description" content="YKS 2026 TYT ve AYT net hesaplama aracı. Doğru yanlış girerek netini ve tahmini puanını hemen öğren. Ücretsiz YKS puan hesaplama sitesi.">
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body class="bg-gray-100 p-0 sm:p-6">

<div class="w-full sm:max-w-2xl mx-auto">

  <!-- Başlık ve Banner -->
  <div class="bg-white rounded-xl shadow-md p-6 mb-6">
    <h1 class="text-3xl sm:text-4xl font-bold text-center mb-4">YKS Net & Puan Hesaplama</h1>
    <img src="images/banner.jpg" alt="YKS Net ve Puan Hesaplama" class="w-full h-auto rounded-xl mb-4 object-cover">
  </div>

  <!-- Banner altı animasyonlu kutular -->
  <div class="flex flex-wrap justify-center items-center mt-6 mb-6 gap-4">
    <div class="flex-1 min-w-[150px] max-w-[200px] flex flex-col items-center p-4 bg-blue-100 rounded-xl shadow-lg hover:shadow-xl transition transform hover:-translate-y-1">
      <span class="font-semibold text-center text-lg animate-bounce hover:text-blue-700 transition">Kolay Net Girişi</span>
    </div>
    <div class="flex-1 min-w-[150px] max-w-[200px] flex flex-col items-center p-4 bg-green-100 rounded-xl shadow-lg hover:shadow-xl transition transform hover:-translate-y-1">
      <span class="font-semibold text-center text-lg animate-bounce hover:text-green-700 transition">Hızlı Puan Hesaplama</span>
    </div>
    <div class="flex-1 min-w-[150px] max-w-[200px] flex flex-col items-center p-4 bg-purple-100 rounded-xl shadow-lg hover:shadow-xl transition transform hover:-translate-y-1">
      <span class="font-semibold text-center text-lg animate-bounce hover:text-purple-700 transition">Mobil Uyumlu </span>
    </div>
  </div>

  <!-- TYT Tablosu -->
  <div class="bg-white rounded-xl shadow-md p-4 mb-6 overflow-x-auto">
    <h2 class="text-2xl sm:text-3xl font-semibold mb-2">TYT</h2>
    <table class="w-full border border-gray-300 rounded-lg text-lg">
      <thead class="bg-gray-100">
        <tr><th class="p-3 border">Ders</th><th class="p-3 border">Doğru</th><th class="p-3 border">Yanlış</th></tr>
      </thead>
      <tbody>
        <tr class="hover:bg-gray-50">
          <td class="p-2 sm:p-3 border">Türkçe</td>
          <td class="p-2 sm:p-3 border"><input id="t_dogru" type="number" class="w-full border p-2 sm:p-3 rounded text-lg"></td>
          <td class="p-2 sm:p-3 border"><input id="t_yanlis" type="number" class="w-full border p-2 sm:p-3 rounded text-lg"></td>
        </tr>
        <tr class="hover:bg-gray-50">
          <td class="p-2 sm:p-3 border">Matematik</td>
          <td class="p-2 sm:p-3 border"><input id="m_dogru" type="number" class="w-full border p-2 sm:p-3 rounded text-lg"></td>
          <td class="p-2 sm:p-3 border"><input id="m_yanlis" type="number" class="w-full border p-2 sm:p-3 rounded text-lg"></td>
        </tr>
        <tr class="hover:bg-gray-50">
          <td class="p-2 sm:p-3 border">Sosyal</td>
          <td class="p-2 sm:p-3 border"><input id="s_dogru" type="number" class="w-full border p-2 sm:p-3 rounded text-lg"></td>
          <td class="p-2 sm:p-3 border"><input id="s_yanlis" type="number" class="w-full border p-2 sm:p-3 rounded text-lg"></td>
        </tr>
        <tr class="hover:bg-gray-50">
          <td class="p-2 sm:p-3 border">Fen</td>
          <td class="p-2 sm:p-3 border"><input id="f_dogru" type="number" class="w-full border p-2 sm:p-3 rounded text-lg"></td>
          <td class="p-2 sm:p-3 border"><input id="f_yanlis" type="number" class="w-full border p-2 sm:p-3 rounded text-lg"></td>
        </tr>
      </tbody>
    </table>
  </div>

  <!-- AYT Tablosu -->
  <div class="bg-white rounded-xl shadow-md p-4 mb-6 overflow-x-auto">
    <h2 class="text-2xl sm:text-3xl font-semibold mb-2">AYT</h2>
    <table class="w-full border border-gray-300 rounded-lg text-lg">
      <thead class="bg-gray-100">
        <tr><th class="p-3 border">Ders</th><th class="p-3 border">Doğru</th><th class="p-3 border">Yanlış</th></tr>
      </thead>
      <tbody>
        <tr class="hover:bg-gray-50">
          <td class="p-2 sm:p-3 border">Matematik</td>
          <td class="p-2 sm:p-3 border"><input id="ayt_m_d" type="number" class="w-full border p-2 sm:p-3 rounded text-lg"></td>
          <td class="p-2 sm:p-3 border"><input id="ayt_m_y" type="number" class="w-full border p-2 sm:p-3 rounded text-lg"></td>
        </tr>
        <tr class="hover:bg-gray-50">
          <td class="p-2 sm:p-3 border">Edebiyat</td>
          <td class="p-2 sm:p-3 border"><input id="ayt_e_d" type="number" class="w-full border p-2 sm:p-3 rounded text-lg"></td>
          <td class="p-2 sm:p-3 border"><input id="ayt_e_y" type="number" class="w-full border p-2 sm:p-3 rounded text-lg"></td>
        </tr>
        <tr class="hover:bg-gray-50">
          <td class="p-2 sm:p-3 border">Sosyal</td>
          <td class="p-2 sm:p-3 border"><input id="ayt_s_d" type="number" class="w-full border p-2 sm:p-3 rounded text-lg"></td>
          <td class="p-2 sm:p-3 border"><input id="ayt_s_y" type="number" class="w-full border p-2 sm:p-3 rounded text-lg"></td>
        </tr>
        <tr class="hover:bg-gray-50">
          <td class="p-2 sm:p-3 border">Fen</td>
          <td class="p-2 sm:p-3 border"><input id="ayt_f_d" type="number" class="w-full border p-2 sm:p-3 rounded text-lg"></td>
          <td class="p-2 sm:p-3 border"><input id="ayt_f_y" type="number" class="w-full border p-2 sm:p-3 rounded text-lg"></td>
        </tr>
      </tbody>
    </table>
  </div>

  <!-- Diploma ve Hesapla -->
  <div class="bg-white rounded-xl shadow-md p-4 mb-6">
    <label class="block mb-3 text-lg">Diploma Notu:
      <input id="diploma" type="number" class="border p-3 w-full rounded text-lg">
    </label>
    <button onclick="hesapla()" class="w-full bg-blue-600 text-white p-3 sm:p-4 rounded hover:bg-blue-700 transition text-lg sm:text-xl">
      Hesapla
    </button>
    <div id="sonuc" class="mt-4 font-semibold text-center text-lg"></div>
    <canvas id="grafik" class="mt-6 w-full"></canvas>
  </div>

  <hr>
  <footer class="text-center mt-6 mb-6">
    <a href="hakkimizda.html">Hakkımızda</a> |
    <a href="gizlilik.html">Gizlilik Politikası</a> |
    <a href="iletisim.html">İletişim</a>
  </footer>
</div>

<script>
function net(d, y) { return d - (y / 4); }
function hesapla() {
  const tytNet = net(t_dogru.value, t_yanlis.value)+net(m_dogru.value, m_yanlis.value)+net(s_dogru.value, s_yanlis.value)+net(f_dogru.value, f_yanlis.value);
  const aytNet = net(ayt_m_d.value, ayt_m_y.value)+net(ayt_e_d.value, ayt_e_y.value)+net(ayt_s_d.value, ayt_s_y.value)+net(ayt_f_d.value, ayt_f_y.value);
  const diploma = Number(document.getElementById("diploma").value)||0;
  const tytPuan = 100 + tytNet * 4 + diploma * 0.12;
  const aytPuan = 100 + aytNet * 5 + diploma * 0.12;
  document.getElementById("sonuc").innerHTML = `TYT Net: ${tytNet.toFixed(2)} <br>AYT Net: ${aytNet.toFixed(2)} <br><br>TYT Puan: ${tytPuan.toFixed(2)} <br>AYT Puan: ${aytPuan.toFixed(2)}`;
  const ctx = document.getElementById("grafik");
  if(window.chart) window.chart.destroy();
  window.chart = new Chart(ctx,{type:"bar",data:{labels:["TYT","AYT"],datasets:[{label:"Puan",data:[tytPuan,aytPuan]}]}});
}
</script>

</body>
</html>
