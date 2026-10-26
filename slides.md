---
theme: default
title: 'Metropolis: Yalnızlığın Geometrisi'
info: 'Mekânın bölünmesi, kent yaşamı ve özyineleme üzerine 10 dakikalık konuşma.'
lang: tr
colorSchema: dark
duration: 10min
transition: fade
layout: blank
---

<!--
Kapak için ayrılan slayt. Kapağı buraya ekleyebilirsiniz.
Süre: 0:00–0:15. Başlığı ve kendinizi tanıtın, ardından ikinci slayta geçin.
-->

---
layout: default
---

<div class="slide-grid">
  <div>
    <div class="slide-kicker">01 / MEKÂN VE SINIR</div>
    <h1>1. Bir Bölme Eylemi Olarak Kent</h1>
    <p>Kent, sokaklardan ve cephelerden önce <strong>kesintisiz bir yüzeydir</strong>. Biz o yüzeye çizgiler çeker; toprağı bölgelere, bölgeleri yapı adalarına, yapıları odalara, odaları ise pencere kenarındaki tek bir köşeye kadar böleriz.</p>
    <p>Her yeni kesik, mekâna yeni bir kimlik atar; <strong>bize ait olanla başkasına ait olanı</strong> birbirinden ayırır. Milyonlarca insanın ortaklaşa ürettiği metropol, yalnızca küçük ve kişisel parçalar hâlinde deneyimlenir.</p>
    <div class="question">İçinde yaşayacak yerler mi inşa ediyoruz, yoksa birbirimizden uzaklaşacak sınırlar mı?</div>
  </div>
  <div style="display:flex; justify-content:center;">
    <MetropolisCanvas mode="divide9" :seed="101" :width="340" :height="410" />
  </div>
</div>

<!--
Süre: 0:15–1:15 · 60 saniye

Bir kenti düşünmeden önce yalnızca kesintisiz bir boşluk düşünelim. Henüz sokaklar, mülkiyet sınırları ya da duvarlar yok. Sonra bu yüzeye bir çizgi çekiyoruz; Metropolis IX algoritmasında olduğu gibi iki iç nokta (P1 ve P2) seçtiğimiz anda, tek bir bütün dokuz ayrı bölgeye parçalanıyor.

Toprağı ilçelere, mahallelere, binalara ve odalara ayırıyoruz; odanın içinde bile kendimize bir masa ya da pencere önü seçiyoruz. Her bölünme yeni bir mekânsal kimlik üretiyor. Metropolis tam da bu çelişkinin üzerinde yükselir: Kolektif bir insan eyleminin ürünüdür ama sayısız bireysel yalnızlık alanı üzerinden deneyimlenir.
-->

---
layout: default
---

<div class="slide-grid">
  <div>
    <div class="slide-kicker">02 / YAKINLIK VE MESAFE</div>
    <h1>2. Metropol Yaşamının Paradoksu</h1>
    <p>Modern metropol, insanı olağanüstü bir hareket, kalabalık ve duyusal uyaran yoğunluğuna maruz bırakır. Kentli birey, bu baş döndürücü yoğunlukta ayakta kalabilmek için <strong>zihinsel bir mesafe ve içe çekilme</strong> geliştirir.</p>
    <p>Yüzlerce insanla aramızda yalnızca <strong>birkaç santimetrelik beton</strong> vardır. Uzaktan bakıldığında tek bir geometrik yüzey gibi görünen her cephe, içeride birbirine kapalı yüzlerce ayrı dünyayı, rutini ve kaygıyı barındırır.</p>
    <div class="question">Kent bizi yoğunlukla bir araya getirirken, sınırlarla birbirimizden ayrı tutar.</div>
  </div>
  <div style="display:flex; justify-content:center;">
    <MetropolisCanvas mode="artwork" :level="3" :seed="8420" :night="true" :paletteIndex="3" :width="340" :height="410" :interactive="true" />
  </div>
</div>

<!--
Süre: 1:15–2:20 · 65 saniye

Georg Simmel, metropol yaşamının zihinsel koşullarını incelerken temel bir paradoksa işaret eder: Fiziksel yakınlık, duygusal yakınlık üretmez. Kalabalığın ve hızın ortasında birey, kendini korumak için görünmez bir zırh ve psikolojik mesafe geliştirir.

Bir apartman bloğuna uzaktan baktığımızda tekrarlanan dikdörtgenlerden ve kat çizgilerinden oluşan soyut bir geometri görürüz. Oysa her dikdörtgenin arkasında bambaşka bir insan deneyimi gizlidir. Metropolis IX'un gece kompozisyonlarında beliren o yatay kat çizgileri ve ışıklı noktalar, yan yana süren ama birbirine değmeyen bu iç dünyaların görsel karşılığıdır.
-->

---
layout: default
---

<div class="slide-grid">
  <div>
    <div class="slide-kicker">03 / MEKÂNIN ÜRETİMİ</div>
    <h1>3. Toplumsal Bir Süreç Olarak Bölünme</h1>
    <p>Mekân, insan eylemlerini içine alan <strong>tarafsız ve boş bir kap değildir</strong>; mülkiyet, güç ilişkileri ve gündelik pratiklerle toplumsal olarak üretilir. Bir alanı bölmek yalnızca matematiksel bir işlem değil, politik ve sosyal bir karardır.</p>
    <p>Bir duvar içeriyi ve dışarıyı tanımlar; bir kapı erişimi, bir pencere ise mesafeyi koruyarak izlemeyi belirler. Ancak aynı sınırlar yalnızca dışlamaz; <strong>mahremiyet, güvenlik, özerklik ve kimlik</strong> için gerekli zemini de kurar.</p>
    <div class="question">Bölmek tanımlamaktır; tanımlamak ise hem içeriyi hem dışarıyı yaratmaktır.</div>
  </div>
  <div style="display:flex; justify-content:center;">
    <MetropolisCanvas mode="artwork" :level="3" :seed="31415" :showBorders="true" :paletteIndex="2" :width="340" :height="410" :interactive="true" />
  </div>
</div>

<!--
Süre: 2:20–3:25 · 65 saniye

Henri Lefebvre'nin vurguladığı gibi mekân asla nötr değildir; toplumun kendi ilişkilerini örgütleme biçiminin fiziksel bir yansımasıdır. Bir mülkiyet çizgisi sahipliği, bir duvar dahil olma ile dışlanma arasındaki sınırı kurar.

Fakat bu sınırları yalnızca baskıcı birer ayrım aracı olarak okuyamayız. Bizi birbirimizden ayıran duvarlar, aynı zamanda kendi kimliğimizi kurabildiğimiz, kapımızı kapatıp özgürleşebildiğimiz mahremiyet alanlarını var eder. Metropolis, bireysel özerklik ile kolektif aidiyet arasındaki bu kesintisiz müzakereyi araştırır.
-->

---
layout: default
---

<div class="slide-grid">
  <div>
    <div class="slide-kicker">04 / ALGORİTMİK MİMARİ</div>
    <h1>4. Özyineleme: Kentin Algoritmik Yorumu</h1>
    <p><strong>Özyineleme (recursion)</strong>, bir fonksiyonun kendi ürettiği daha küçük parçalara aynı kuralı tekrar tekrar uygulamasıdır. <em>Metropolis IX</em>'da tek bir ana hücre (<code>Cell</code>), içinden seçilen iki nokta ile <strong>merkez, 4 köşe ve 4 kenar köprüsü</strong> olmak üzere 9 alt hücreye ayrılır.</p>
    <p>Kentler de tek bir ölçekte kurulmaz; iç içe geçmiş kararlarla büyür. Algoritma, kentin tarihsel karmaşıklığını birebir kopyalamak yerine temel bir eğilimi soyutlar: <strong>ortak alanın kesintisiz biçimde bireysel bölgelere dönüşmesi</strong>.</p>
    <div class="code-pill">
      <span class="kw">subDivide</span>(level = 5, threshX = 0.1, threshY = 0.7)<br/>
      <span class="cm">// 1 Hücre → 9 → 81 → 729 → 6.561 → 59.049 mikro-bölge</span>
    </div>
  </div>
  <div style="display:flex; justify-content:center;">
    <MetropolisCanvas mode="recursive" :level="2" :seed="2022" :paletteIndex="0" :width="340" :height="410" :interactive="true" />
  </div>
</div>

<!--
Süre: 3:25–4:40 · 75 saniye

Sanatsal pratiğimde bu kentsel bölünmeyi özyinelemeli (recursive) bir algoritma üzerinden modelliyorum. Metropolis IX'un kaynak kodundaki `Cell.createChildren()` fonksiyonu, klasik ikili bölünmenin ötesine geçer: Bir dikdörtgenin içinde rastlantısal ama belirli eşiklerle kısıtlanmış iki nokta seçer ve alanı 9 yeni dikdörtgene ayırır.

Bu işlem 5 derinlik seviyesi boyunca (`divLevel = 5`) kendi kendini çağırdığında, tek bir yüzeyden on binlerce mikro-hücre doğar. Sağdaki tuvalde derinlik sürgüsünü değiştirdiğinizde, basit bir matematiksel kuralın nasıl iç içe geçmiş bir metropol dokusuna dönüştüğünü canlı olarak görebilirsiniz.
-->

---
layout: default
---

<div class="slide-grid">
  <div>
    <div class="slide-kicker">05 / SOYUTLAMADAN YAŞAMA</div>
    <h1>5. Matematiksel Yapıdan İnsan Deneyimine</h1>
    <p>Bir algoritma yalnızlığı, aidiyeti ya da özlemi bilmez; yalnızca koordinatları ve oranları hesaplar. Ancak dikey ve yatay bölünmeler yan yana geldiğinde, zihnimiz bu soyut geometride <strong>tanıdık bir kent hafızası</strong> yakalar: cepheler, pencereler ve sıkışık mahalleler.</p>
    <p>Sanatçı her ayrıntıyı tek tek çizmez; içinden yaşam ihtimallerinin doğacağı <strong>kurallar ve kısıtlamalar sistemini</strong> kurar. Tek bir dikdörtgen kenti anlatamaz; anlamı üreten şey parçaların birikimi ve yan yanalığıdır.</p>
    <div class="feature-tags">
      <div class="feature-tag"><span>Dikdörtgen</span> → Oda</div>
      <div class="feature-tag"><span>Oda</span> → Ev</div>
      <div class="feature-tag"><span>Ev</span> → İnsan</div>
      <div class="feature-tag"><span>İnsan</span> → Anılar ve İhtimaller Evreni</div>
    </div>
  </div>
  <div style="display:flex; justify-content:center;">
    <MetropolisCanvas mode="anatomy" :level="3" :seed="777" :paletteIndex="1" :width="340" :height="410" :interactive="true" />
  </div>
</div>

<!--
Süre: 4:40–5:45 · 65 saniye

Algoritma bir dikdörtgeni bölerken yalnızlığı ya da mahremiyeti bilmez; yalnızca matematiksel bir talimatı yerine getirir. Buna rağmen ortaya çıkan kompozisyona baktığımızda tanıdık bir şey görürüz: Yukarıdan bakılan bir şehir, apartman cepheleri, pencereler ve yoğun mahalleler. Geometri anlam kazanır, çünkü onu kendi yapılı çevre deneyimimiz üzerinden okuruz.

Üretken sanatta sanatçı her pencereyi tek tek belirlemek yerine, biçimlerin doğacağı kurallar ve ilişkiler sistemini tasarlar; eser, niyet ile hesaplama arasındaki bir müzakereye dönüşür. Soyut bir dikdörtgen zihnimizde önce bir odaya, oda bir eve, ev bir insana, insan ise başlı başına bir anılar ve ihtimaller evrenine dönüşür.
-->

---
layout: default
---

<div class="slide-grid">
  <div>
    <div class="slide-kicker">06 / DETERMINİZM VE ENTROPİ</div>
    <h1>6. Düzen, Rastlantı ve Kontrol Yanılsaması</h1>
    <p>İlk bakışta ızgara (grid), mutlak bir rasyonelliğin ve kontrolün simgesi gibi görünür. Oysa başlangıçtaki bölme oranlarında yapılan küçücük bir sapma, kendisinden sonra gelen tüm alt bölünmelerin kaderini değiştirir.</p>
    <p>Kentler de imar planları, yönetmelikler ve altyapı ağlarıyla düzenlenmek istenir; ancak <strong>yaşanan kent her zaman planı aşar</strong>. Kodda <code>sprawl</code> (kentsel yayılma) aktif olduğunda devreye giren <code>addChaos()</code> fonksiyonu, köşeleri sarsarak bu organik taşmayı görünür kılar.</p>
    <div class="question">Tamamlanmış ve sabit görünen her kent, aslında devam eden bir sürecin geçici bir kesitidir.</div>
  </div>
  <div style="display:flex; justify-content:center;">
    <MetropolisCanvas mode="chaos" :level="3" :chaos="4.5" :seed="1923" :paletteIndex="4" :width="340" :height="410" :interactive="true" />
  </div>
</div>

<!--
Süre: 5:45–6:50 · 65 saniye

Izgara sistemleri bize tam bir kontrol yanılsaması sunar. Oysa Metropolis IX'da her 작품, `fxhash` diziliminden üretilen deterministik bir tohuma (`xmur3(fxhash)`) bağlıdır ve üst seviyedeki en ufak bir oran değişimi alt katmanlarda bambaşka bir kent doğurur.

Dahası, kodun %97 olasılıkla aktif ettiği `Urban Sprawl` (Kentsel Yayılma) özelliği, `decorator.addChaos()` fonksiyonunu çağırarak her hücrenin A, B, C, D köşelerini rastlantısal olarak sarsar. Sağdaki KAOS sürgüsünü hareket ettirdiğinizde görebileceğiniz gibi, katı geometrik plan insan yaşamının doğaçlaması ve düzensizliğiyle esner.
-->

---
layout: default
---

<div class="slide-grid">
  <div>
    <div class="slide-kicker">07 / AYRIM VE TEMAS</div>
    <h1>7. Yalnızlık Başkalarının Yokluğu Değildir</h1>
    <p><em>Metropolis</em>'teki yalnızlık, salt bir kopuş ya da dışlanma değildir; bireyin kendi içine çekilebildiği, düşünebildiği ve <strong>kendi ritmini kurabildiği yaratıcı bir özgürlük alanıdır</strong>. Üstelik hiçbir özel alan bütünüyle bağımsız değildir; duvarların içinden geçen görünmez altyapılar bizi birbirimize bağlar.</p>
    <p>Geometride <strong>iki dikdörtgeni birbirinden ayıran kenar, aynı zamanda onların birbirine değdiği ortak çizgidir</strong>. Sınır yalnızca ayırmaz; karşılaşmanın, kaldırımın ve temasın yerini de tanımlar.</p>
    <div class="question">Mesele aramızda kaç sınır olduğu değil, o sınırların yarattığı alanları nasıl yaşadığımızdır.</div>
  </div>
  <div style="display:flex; justify-content:center;">
    <MetropolisCanvas mode="shared-edge" :level="3" :seed="6161" :showBorders="true" :paletteIndex="0" :width="340" :height="410" :interactive="true" />
  </div>
</div>

<!--
Süre: 6:50–7:55 · 65 saniye

Yalnızlığı yalnızca olumsuz bir izolasyon olarak düşünmek zorunda değiliz. Kalabalıktan geri çekilebilmek, tefekkür ve bireysel özgürlük için vazgeçilmezdir. Üstelik en mahrem odamız bile elektrikten suya, iletişim ağlarından sokaklara kadar devasa bir ortak altyapının parçasıdır.

Jane Jacobs'ın kent yaşamında kaldırımlara ve gündelik karşılaşmalara atfettiği değer, bize sınırların öteki yüzünü hatırlatır: İki dikdörtgen bir kenarı paylaştığında, o çizgi onları hem ayırır hem de birbirine komşu kılar. Sınır, ayrılığın olduğu kadar temasın da geometrisidir.
-->

---
layout: default
---

<div class="slide-grid">
  <div>
    <div class="slide-kicker">08 / PARÇA VE BÜTÜN</div>
    <h1>8. Kolektif Bir Portre Olarak Metropolis</h1>
    <p><em>Metropolis IX</em>, belirli bir coğrafyayı ya da mimari projeyi haritalamaz; çağdaş kent deneyiminin <strong>soyut ve kolektif bir portresini</strong> çizer. Her alt bölünme kendine ait bir bölge kurar ama hiçbir hücre genel kompozisyondan bağımsız var olamaz.</p>
    <p>Burada görsel bir paradoks ortaya çıkar: <strong>Algoritma görüntüyü ayırma işlemiyle inşa eder; fakat birleşik ve bütünsel kompozisyonu mümkün kılan şey de tam olarak bu ayrılmadır.</strong></p>
    <div class="question">Bizi birbirimize bağlayan yapılarla birbirimizden ayrılırız.</div>
  </div>
  <div style="display:flex; justify-content:center;">
    <MetropolisCanvas mode="artwork" :level="4" :chaos="1.5" :seed="9090" :paletteIndex="3" :night="true" :width="340" :height="410" :interactive="true" />
  </div>
</div>

<!--
Süre: 7:55–8:55 · 60 saniye

Uzaktan bakıldığında Metropolis IX, parçalanmış bir kentin görüntüsü olmaktan çok, parçalanma yoluyla var olan kolektif bir organizmanın portresidir. Tekil bölgeler olmasa bu görsel derinlik doğmaz; sınırlar olmasa doku ortaya çıkmaz.

Bireysel olan ile kolektif olan birbirinin zıddı değildir; birbirlerini kesintisiz olarak üretirler. Metropol bize kendi hayatlarımızı kuracağımız çerçeveyi verirken, bizim o küçük hücrelerde biriktirdiğimiz yaşamlar da metropole anlamını kazandırır.
-->

---
layout: default
---

<div class="slide-grid">
  <div>
    <div class="slide-kicker">09 / SONUÇ</div>
    <h1>9. Sonuç: Yalnızlığın Geometrisi</h1>
    <p>Tek bir dikdörtgenle başladık; basit bir kuralı özyinelemeli olarak tekrarladık ve binlerce parçadan oluşan karmaşık bir kent dokusuna ulaştık. Ancak <em>Metropolis</em>'in asıl sorusu matematiksel değil, <strong>varoluşsaldır</strong>: Ayrı dünyalarda yaşarken bir arada olmak ne anlama gelir?</p>
    <p>Kent yalnızca beton ve camdan ibaret bir yapı yığını değildir; insanın <strong>mekân, kimlik, mahremiyet, özgürlük ve aidiyet</strong> arzularının üst üste binmiş geometrisidir.</p>
    <blockquote>
      Mekânları daha küçük mekânlara böleriz.<br/>
      Onları sahiplenir, sınırları içinde kendi yalnızlığımızı yaşarız.<br/>
      <strong>Buna Metropolis deriz.</strong>
    </blockquote>
  </div>
  <div style="display:flex; justify-content:center;">
    <MetropolisCanvas mode="interactive" :level="3" :chaos="1.5" :seed="2026" :paletteIndex="1" :width="340" :height="410" :interactive="true" />
  </div>
</div>

<!--
Süre: 8:55–10:00 · 65 saniye

Tek bir dikdörtgenden yola çıktık; onu böldük, her parçayı yeniden böldük ve basit bir algoritmik kuralın içinden devasa bir kentsel doku çıkardık. Ama Metropolis'in merkezindeki soru hiçbir zaman yalnızca matematiksel olmadı: Bireyselliğimizi koruyan sınırlar, başkalarıyla kurduğumuz bağları nasıl şekillendiriyor? Ve kişisel bölgelerin toplamı hangi noktada ortak bir yaşama dönüşüyor?

Metropolis bu sorulara kesin bir yanıt vermek yerine, kente yeniden bakabileceğimiz görsel bir dil öneriyor:

Mekânları daha küçük mekânlara böleriz. Onları sahiplenir, sınırları içinde kendi yalnızlığımızı yaşarız. Ve yan yana gelen bu sayısız tekil dünyaya... Metropolis deriz. Teşekkür ederim.
-->
