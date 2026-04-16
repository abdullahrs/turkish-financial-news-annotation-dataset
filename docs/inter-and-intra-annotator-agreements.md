# Etiketleme Anlaşma Metrikleri Rehberi

Bu doküman, topladığınız veri örneklerini 5 kişiye bölerek etiketleme yaparken hangi **anlaşma (agreement)** metriklerini kullanabileceğinizi ve bunları nasıl yorumlayabileceğinizi özetler. Amaç, veri setinin güvenilirliğini akademik çalışmalarda kabul gören ölçülerle değerlendirmektir.

## 1. Temel Kavramlar

### Inter-Annotator Agreement (IAA)

Farklı etiketleyicilerin aynı veri örneğine ne kadar benzer karar verdiğini ölçer.

Örnek:

- 5 kişi aynı haber kaydını "olumlu", "olumsuz", "nötr" diye etiketliyorsa
- bu kişilerin birbirleriyle ne kadar tutarlı olduğu IAA ile ölçülür

### Intra-Annotator Agreement

Aynı kişinin, aynı veya benzer veriyi farklı zamanlarda ne kadar tutarlı etiketlediğini ölçer.

Örnek:

- Etiketleyici A bugün 100 örneği etiketler
- 2 hafta sonra aynı örneklerin bir kısmını tekrar etiketler
- iki tur arasındaki tutarlılık intra-annotator agreement olarak raporlanır

Akademik veri setlerinde genelde önce **inter-annotator agreement**, imkan varsa ayrıca **intra-annotator agreement** raporlanır.

## 2. 5 Kişilik Etiketleme Düzeni Nasıl Kurulmalı?

5 kişi arasında veriyi tamamen ayırıp herkes farklı örnekleri etiketlerse, gerçek anlamda agreement hesabı yapmak zorlaşır. Çünkü agreement için aynı örneğin birden fazla kişi tarafından etiketlenmiş olması gerekir.

Bu yüzden pratikte şu yapı önerilir:

1. Verinin bir kısmı ortak etiketleme için ayrılır.
2. Bu ortak kısım 5 kişinin tamamına veya en az 2-3 kişilik kesişen gruplara verilir.
3. Kalan büyük kısım kişilere paylaştırılır.
4. Agreement metrikleri ortak etiketlenen bölüm üzerinde hesaplanır.

### Önerilen oran

- `%10-%20` ortak etiketleme
- `%80-%90` kişilere bölünmüş ana veri

Bu yöntem hem iş yükünü dengeler hem de veri setinin güvenilirliğini ölçmenizi sağlar.

### Uygulama Notu (Bu Proje)

- `common 100` içindeki haberler inter-annotator agreement için kullanılacaktır.
- Her annotator kendi ayrı CSV dosyasını dolduracaktır (`AS`, `FC`, `MHC`, `HTI`, `SK`).
- Intra-annotator kontrolü için her annotator kendi ilk 5 anotasyonunda kullandığı `doc_id` değerlerini dosyanın sonunda tekrar etiketler.

## 3. En Sık Kullanılan Akademik Agreement Metrikleri

## 3.1. Ham Uyum Oranı (Percent Agreement)

En basit metriktir.

Formül:

`Uyum Oranı = Aynı etiket verilen örnek sayısı / Toplam ortak örnek sayısı`

Örnek:

- 100 ortak örneğin 82 tanesinde iki etiketleyici aynı kararı verdiyse
- percent agreement = `82 / 100 = 0.82`

### Avantajı

- Çok kolay anlaşılır.

### Dezavantajı

- Şansa bağlı uyumu dikkate almaz.
- Özellikle dengesiz sınıflarda yanıltıcı olabilir.

Bu yüzden akademik çalışmalarda tek başına yeterli görülmez; genelde kappa veya alpha ile birlikte verilir.

## 3.2. Cohen's Kappa

İki etiketleyici arasındaki uyumu, **şansa bağlı beklenen uyumu çıkararak** ölçer.

Formül:

`Kappa = (Gözlenen Uyum - Beklenen Uyum) / (1 - Beklenen Uyum)`

### Ne zaman kullanılır?

- Tam olarak **2 etiketleyici** varsa
- nominal kategoriler kullanılıyorsa

### 5 kişi için nasıl kullanılır?

5 kişi varsa Cohen's Kappa doğrudan tek sayı olarak tüm gruba uygulanmaz. Bunun yerine:

- her ikili annotator çifti için ayrı ayrı hesaplanır
- sonra ortalama kappa raporlanabilir

5 kişi için toplam çift sayısı:

`5 x 4 / 2 = 10`

Yani 10 adet ikili kappa değeri çıkar.

### Ne işe yarar?

- Hangi annotator çiftlerinin daha tutarlı olduğunu görürsünüz.
- Eğitim ihtiyacı olan annotator'leri tespit etmek kolaylaşır.

## 3.3. Fleiss' Kappa

Cohen's Kappa'nın, **ikiden fazla etiketleyici** için genelleştirilmiş halidir.

### Ne zaman kullanılır?

- Aynı örnekler birden fazla kişi tarafından etiketleniyorsa
- etiketler kategorikse
- bir örneğe düşen annotator sayısı sabitse

Sizin senaryonuzda, ortak bir alt küme 5 kişinin tamamı tarafından etiketlenirse, en doğal grup metriği genelde **Fleiss' Kappa** olur.

### Avantajı

- 5 kişi gibi çok annotator'lü yapılara uygundur.

### Sınırlaması

- Eksik etiket durumlarında esnek değildir.
- Ordinal veri için en güçlü seçenek değildir.

## 3.4. Krippendorff's Alpha

Akademik yayınlarda en çok tercih edilen ve en esnek metriklerden biridir.

### Ne zaman kullanılır?

- 2 veya daha fazla annotator varsa
- annotator sayısı örnekten örneğe değişebiliyorsa
- eksik etiketler varsa
- nominal, ordinal, interval veya ratio veri varsa

### Neden güçlü?

- Çok esnektir.
- Eksik veriyle çalışabilir.
- Ölçek tipine göre uyarlanabilir.

Bu yüzden, eğer raporunuzda tek bir güçlü ana metrik seçecekseniz, çoğu durumda **Krippendorff's Alpha** en güvenli tercihlerden biridir.

## 3.5. Weighted Kappa

Eğer etiketler sıralıysa, örneğin:

- düşük
- orta
- yüksek

gibi, tüm hatalar eşit sayılmamalıdır. "düşük" ile "orta" arasındaki hata, "düşük" ile "yüksek" arasındaki hata kadar büyük değildir.

Bu durumda **Weighted Cohen's Kappa** kullanılır.

### Ne zaman kullanılır?

- 2 annotator
- sıralı (ordinal) etiketler

5 kişi varsa yine ikili bazda weighted kappa hesaplanıp ortalama raporlanabilir.

## 3.6. Scott's Pi

Cohen's Kappa'ya benzeyen daha eski bir metriktir. Günümüzde daha az tercih edilir. Çoğu modern çalışma Cohen's Kappa, Fleiss' Kappa veya Krippendorff's Alpha kullanır.

Bu nedenle zorunlu değilse ayrıca kullanmanız gerekmez.

## 4. Akademik Olarak En Mantıklı Kurulum

Sizin 5 kişilik senaryonuz için pratik ve akademik açıdan güçlü bir kurulum şu olur:

1. Veri setinin `%10-%20` kadar bölümünü 5 kişinin tamamına verin.
2. Bu ortak bölüm için:
   - `Fleiss' Kappa` hesaplayın
   - mümkünse ayrıca `Krippendorff's Alpha` hesaplayın
3. Kalan veriyi 5 kişiye bölüştürün.
4. Etiketleme kılavuzunu netleştirin.
5. İlk pilot etiketlemeden sonra düşük agreement çıkarsa yönergeleri revize edin.

### Minimum öneri

En azından şu üç bilgiyi raporlayın:

- ortak örnek sayısı
- ham uyum oranı
- Fleiss' Kappa veya Krippendorff's Alpha

## 5. Sonuçlar Nasıl Yorumlanır?

Sık kullanılan kabaca yorum aralıkları:

- `< 0.00`: beklenenden kötü
- `0.00 - 0.20`: çok zayıf
- `0.21 - 0.40`: zayıf
- `0.41 - 0.60`: orta
- `0.61 - 0.80`: iyi
- `0.81 - 1.00`: çok iyi

Bu aralıklar çoğunlukla kappa benzeri metrikler için kullanılır. Ancak bunları kesin yasa gibi değil, **alan bağlamına göre yorumlanan rehber aralıklar** olarak düşünmek gerekir.

Özellikle:

- sınıf dağılımı çok dengesizse
- görev yoruma açıksa
- etiket sınırları belirsizse

orta seviyedeki bir agreement bile kabul edilebilir olabilir.

## 6. 5 Kişi İçin Örnek İş Akışı

Elinizde 5.000 veri örneği olduğunu varsayalım.

### Örnek plan

- 750 örnek: 5 kişinin hepsi tarafından etiketlenir
- 4.250 örnek: kişilere bölüştürülür

### Sonra

- 750 ortak örnek üzerinde `Fleiss' Kappa` hesaplanır
- gerekiyorsa `Krippendorff's Alpha` da hesaplanır
- 5 kişi arasındaki 10 farklı çift için ortalama `Cohen's Kappa` da ek analiz olarak üretilebilir

Bu kurulum hem yayın yazarken raporlamayı kolaylaştırır hem de veri kalitesini savunmayı güçlendirir.

## 6.1. 5 Dakika / Haber Varsayımıyla Somut Dağıtım Hesabı

Eğer her haber için ortalama `5 dakika` ayırıyorsanız, haftalık çalışma süreleri ve 2 haftalık toplam kapasite aşağıdaki gibi özetlenebilir:

| Kişi   | Haftalık çalışma saati | 2 haftalık toplam çalışma saati | 2 haftalık haber kapasitesi |
| ------ | ---------------------: | ------------------------------: | --------------------------: |
| Annotator-AS |                 7 saat |                         14 saat |                         168 |
| Annotator-FC |               7,5 saat |                         15 saat |                         180 |
| Annotator-MHC |                15 saat |                         30 saat |                         360 |
| Annotator-HTI |                 7 saat |                         14 saat |                         168 |
| Annotator-SK |              14,5 saat |                         29 saat |                         348 |
| Toplam |                51 saat |                        102 saat |                        1224 |

Haber kapasitesi hesabı şu varsayıma dayanır:

- `1 haber = 5 dakika`
- `1 saat = 12 haber`

Toplam etiketleme kapasitesi:

`168 + 180 + 360 + 168 + 348 = 1224`

Bu toplam, **etiketleme işlemi sayısını** ifade eder. Yani ortak verilen her haber, 5 kişi tarafından etiketlenecekse toplam kapasitede `5` kez sayılır.

### Kullanışlı formül

5 kişinin tamamına verilen ortak haber sayısı `O`, tekil haber sayısı `T` ise:

`Toplam kapasite = T + 5O`

Toplam benzersiz haber sayısı ise:

`Toplam benzersiz haber = T + O`

Alternatif olarak aynı ilişki şu şekilde de yazılabilir:

`Toplam kapasite = Toplam benzersiz haber + 4O`

Çünkü ortak verilen her haber, benzersiz haber sayısına göre toplam iş yüküne fazladan `4` ek etiketleme yükü bindirir.

## 6.2. Maksimum Senaryo: 824 Benzersiz Haber + 100 Ortak Haber

Eğer ortak haber sayısını `100` olarak sabitlerseniz:

`1224 - (100 x 5) = 724`

Yani geriye en fazla `724` tekil haber kapasitesi kalır.

Bu durumda maksimum benzersiz haber sayısı:

`724 tekil + 100 ortak = 824 benzersiz haber`

### Kişi başı maksimum tekil kapasite

| Kişi   | Toplam kapasite | 100 ortak sonrası tekil kapasite |
| ------ | --------------: | -------------------------------: |
| Annotator-AS |             168 |                               68 |
| Annotator-FC |             180 |                               80 |
| Annotator-MHC |             360 |                              260 |
| Annotator-HTI |             168 |                               68 |
| Annotator-SK |             348 |                              248 |
| Toplam |            1224 |                              724 |

Bu tablo, `100 ortak haber` verildiğinde kimlerin en fazla kaç tekil haber alabileceğini gösterir.

## 6.3. Dengeli Senaryo: 775 Benzersiz Haber + 100 Ortak Haber + 25 Intra Tekrar

Pratik ve akademik açıdan daha dengeli kurulum şu olabilir:

- toplam benzersiz haber: `775`
- ortak haber: `100`
- tekil haber: `675`
- intra-annotator tekrar haberi: `25`

Toplam etiketleme yükü:

`675 + (100 x 5) + 25 = 1200`

Bu kurulumda, kişi başı `5` haber ikinci kez etiketlenerek intra-annotator consistency analizi için ek yük ayrılmış olur.

### Önerilen kişi başı dağılım

| Kişi   | Ortak haber | Tekil haber | Intra tekrar | Toplam yük |
| ------ | ----------: | ----------: | -----------: | ---------: |
| Annotator-AS |         100 |          63 |            5 |        168 |
| Annotator-FC |         100 |          75 |            5 |        180 |
| Annotator-MHC |         100 |         243 |            5 |        348 |
| Annotator-HTI |         100 |          63 |            5 |        168 |
| Annotator-SK |         100 |         231 |            5 |        336 |
| Toplam |         500 |         675 |           25 |       1200 |

Bu dağılımda, intra-annotator kontrolü için her annotator'e eşit sayıda tekrar haber ayrılır.

Not: Toplam teorik kapasite `1224` olmasına rağmen bu tabloda toplam yük `1200` olarak tutulmuştur. Aradaki `24 haberlik` fark, özellikle daha yüksek kapasiteye sahip annotator'lerde bilinçli olarak bırakılan operasyonel tamponu ifade eder. Bu tampon, haber başına düşen gerçek sürenin uzaması, tekrar etiketleme gecikmeleri veya adjudication ihtiyacı gibi durumlarda planın esnek kalmasını sağlar.

### Neden 775 + 100 + 25 kurulumu güçlü?

- ortak etiketleme ile intra-annotator kontrolünü aynı plan içinde birleştirir
- toplam `775` benzersiz haber ile manuel anotasyon açısından güçlü bir corpus büyüklüğü sunar
- kişi başı `5` tekrar haber ile zamansal tutarlılık ölçülebilir
- agreement analizi için `100 ortak haber` güçlü bir alt küme oluşturur

Ortak örnek oranı:

`100 / 775 ≈ 0.129 = %12.9`

## 6.4. Kalite Kontrol Kullanımı

Ana `775 haber` kümesinin içinden küçük bir alt küme seçilerek kalite güvence süreci desteklenebilir.

Özellikle kişi başı `5` haber olmak üzere toplam `25 haber`, intra-annotator kontrolü için ikinci kez etiketlenebilir. Buna ek olarak, gerekirse ayrı bir küçük alt küme şu amaçlarla değerlendirilebilir:

- `pilot calibration`
- `guideline refinement`
- `adjudication check`
- `intra-annotator check`

Önemli nokta şudur:

- bu tekrar etiketlenecek haberler, `775` habere ek olmak zorunda değildir
- doğrudan `775` haberin içinden seçilebilir
- böylece toplam corpus büyüklüğü korunurken kalite güvencesi de sağlanmış olur

## 6.5. Makalede Nasıl Sunulabilir?

Bu plan, makalede aşağıdaki mantıkla sunulabilir:

1. Toplam `775` benzersiz haber seçildi.
2. Bu haberlerin `100` tanesi tüm annotator'ler tarafından bağımsız biçimde etiketlendi.
3. Kalan `675` haber annotator'ler arasında paylaştırıldı.
4. `100` ortak haber, inter-annotator agreement hesabında kullanıldı.
5. Her annotator için `5` haber yeniden etiketlenerek intra-annotator agreement analizi yapıldı.
6. Ana kümenin içinden seçilen küçük alt kümeler, kalite kontrol ve tutarlılık incelemeleri için ayrıca kullanılabildi.

Bu anlatım, sadece iş yükünü dengeleyen bir plan sunmaz; aynı zamanda veri setinin kalite güvencesiyle oluşturulduğunu da gösterir.

## 7. Intra-Annotator Agreement

`Intra-Annotator Agreement`, aynı annotator'ün aynı veya benzer örnekleri farklı zamanlarda ne kadar tutarlı etiketlediğini ölçer.

Bu kavram:

- `inter-annotator agreement` için annotator'ler arası tutarlılığı
- `intra-annotator agreement` için annotator içi zamansal tutarlılığı

ayırt etmek açısından önemlidir.

Kalite güvence sürecinde küçük bir alt küme yeniden etiketlenerek intra-annotator agreement analizi yapılabilir. Bu analiz, etiketleme yönergesinin açık olup olmadığını ve annotator kararlarının zaman içinde ne kadar kararlı kaldığını göstermede yararlıdır.

## 8. Kısa Özet

5 kişilik bir etiketleme düzeninde:

- grup düzeyinde ana metrik olarak `Krippendorff's Alpha`
- klasik ve anlaşılır grup metriği olarak `Fleiss' Kappa`
- ek analiz olarak çiftler arası `Cohen's Kappa`
- kalite kontrol amacıyla `Intra-Annotator Agreement`

birlikte değerlendirilebilir.

## 9. Sonuç

5 kişiye bölünecek bir etiketleme işinde, agreement hesabı yapabilmek için mutlaka ortak etiketlenen bir alt küme bırakılmalıdır. Akademik raporlama açısından en güvenli yaklaşım, ortak veri üzerinde **Fleiss' Kappa** ve/veya **Krippendorff's Alpha** hesaplamak; gerekiyorsa ek olarak **pairwise Cohen's Kappa** ve **intra-annotator agreement** sonuçlarını sunmaktır.
