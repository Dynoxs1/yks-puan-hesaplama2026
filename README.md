<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>YKS Net ve Puan Hesaplama</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdn.tailwindcss.com"></script>
</head>

<body class="bg-gray-100 p-6">
<div class="max-w-2xl mx-auto bg-white p-6 rounded-xl shadow">

<h1 class="text-2xl font-bold text-center mb-6">YKS Net & Puan Hesaplama</h1>

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
<footer style="text-align:center; margin-top:20px;">
  <a href="hakkimizda.html">Hakkımızda</a> |📄 HAKKIMIZDA

YKS Puan Hesaplama, üniversite sınavına hazırlanan adayların TYT ve AYT netlerini hızlı ve pratik şekilde hesaplayabilmesi için oluşturulmuş ücretsiz bir web sitesidir.

Amacımız; öğrencilerin doğru–yanlış sayılarını girerek netlerini öğrenmelerini, tahmini puanlarını görmelerini ve sınav sürecini daha bilinçli şekilde planlamalarını sağlamaktır.

Sitemizde yer alan hesaplamalar bilgilendirme amaçlıdır. ÖSYM tarafından açıklanan resmi sonuçlar ile birebir aynı olmayabilir. Hesaplamalar, genel kabul görmüş katsayılara ve geçmiş yıllardaki sınav sistemine dayalı tahmini verilerdir.

YKS sürecinde adaylara faydalı, sade ve erişilebilir bir araç sunmayı hedefliyoruz.
  <a href="gizlilik.html">Gizlilik Politikası</a> |🔒 GİZLİLİK POLİTİKASI (ÇOK ÖNEMLİ – ADSENSE)

Bu gizlilik politikası, YKS Puan Hesaplama web sitesini ziyaret eden kullanıcılar için geçerlidir.

Toplanan Bilgiler

Sitemizi ziyaret ettiğinizde:

Kişisel bilgi (ad, soyad, TC kimlik vb.) toplanmaz

Girilen doğru–yanlış bilgileri kaydedilmez

Hesaplamalar yalnızca tarayıcı üzerinde yapılır

Çerezler (Cookies)

Google AdSense ve benzeri üçüncü taraf reklam sağlayıcıları, kullanıcılara ilgi alanlarına göre reklam göstermek için çerezler kullanabilir.

Google’ın çerez kullanımı hakkında daha fazla bilgi için:
https://policies.google.com/technologies/ads

Kullanıcılar, tarayıcı ayarlarından çerezleri devre dışı bırakabilir.

Üçüncü Taraf Bağlantılar

Sitemizde üçüncü taraf web sitelerine yönlendiren bağlantılar bulunabilir. Bu sitelerin gizlilik uygulamalarından sitemiz sorumlu değildir.
  <a href="iletisim.html">İletişim</a>📞 İLETİŞİM

Bizimle iletişime geçmek için aşağıdaki e-posta adresini kullanabilirsiniz:

📧 furkanok61@gmail.com

Öneri, geri bildirim ve hata bildirimlerinizi memnuniyetle değerlendiriyoruz.
</footer>
⚠️ YASAL UYARI / SORUMLULUK REDDİ

Bu sitede yer alan TYT ve AYT puan hesaplamaları tahmini sonuçlar sunar.

ÖSYM tarafından açıklanan resmi sınav sonuçları esas alınmalıdır.
Sitemizde yer alan bilgiler doğrultusunda alınan kararlardan doğabilecek sonuçlardan site yönetimi sorumlu tutulamaz.
