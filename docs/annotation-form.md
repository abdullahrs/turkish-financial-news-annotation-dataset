# Kısa Etiketleme Formu Rehberi

Bu rehber, haber bazlı ve hızlı anotasyon için hazırlanmıştır. Amaç, tam bilgi grafı nesneleri üretmek değil; ontolojiye bağlı, temiz ve karşılaştırılabilir varlık ve ilişki kayıtları toplamaktır.

## Temel Yaklaşım

- Çıktı biçimi satır bazlıdır: her satır bir varlık kaydını veya bir ilişki kaydını temsil eder.
- `schema.json` içindeki **entity type** ve **relation** adları aynen kullanılmalıdır.
- Global canonicalization zorunlu değildir. Haber içinde geçen en açık ve açıklayıcı ifade kullanılabilir.
- ID üretimi annotator işi değildir.
- Attribute alanları yalnızca metinde açıkça varsa doldurulur.
- Evidence alanında kısa alıntı veya ilgili cümle/parça yazmak yeterlidir; karakter offset zorunlu değildir.

## Form Kolonları

Önerilen kolonlar:

- `doc_id`
- `head_text`
- `head_type`
- `relation`
- `tail_text`
- `tail_type`
- `attributes`
- `evidence`
- `notes`

## Kolon Açıklamaları

- `doc_id`: Haber kaydının kimliği. Dosya adı + haber sırası gibi pratik bir değer kullanılabilir.
- `head_text`: Kayıttaki ana varlık. İlişki varsa kaynak tarafıdır; ilişki yoksa tek başına varlık kaydıdır.
- `head_type`: Ana varlığın ontolojideki tipi. Örn: `Company`, `Person`, `Country`.
- `relation`: Ontolojide tanımlı ilişki adı. Örn: `employs`, `acquired`, `owns_stake_in`. İlişki yoksa boş bırakılır.
- `tail_text`: İlişkinin hedef tarafı. İlişki yoksa boş bırakılır.
- `tail_type`: Hedef varlığın ontolojideki tipi. İlişki yoksa boş bırakılır.
- `attributes`: Yalnızca açıkça verilen relation attribute'ları. Boş bırakılabilir veya `{}` yazılabilir.
- `evidence`: Haberden kısa bir alıntı, cümle veya ilişkiyi destekleyen bölüm.
- `notes`: Kararsızlık, belirsizlik veya açıklama notu için isteğe bağlı alan.

## Adlandırma Kuralı

Etiketleyici haber içinde geçen en açık yüzey formunu kullanabilir. Ancak aynı haber içindeki tüm satırlarda aynı varlık için aynı ad kullanılmalıdır.

Örnek:

- İlk geçişte `Türk Hava Yolları (THY)` yazıyorsa bunu kullanmak uygundur.
- Sonraki cümlede sadece `THY` geçse bile daha açıklayıcı olan biçim tercih edilebilir.
- Aynı haber içinde `Türk Telekom` ve `Şirket` geçiyorsa `Türk Telekom` tercih edilmelidir.
- Metinde `şirket`, `kurum`, `banka`, `o`, `kendisi` gibi bir zamir veya dolaylı gönderim varsa, formda zamir değil referans verilen açık varlık adı yazılmalıdır.

Amaç global varlık birleştirme değil, haber içi açık ve tutarlı kayıt üretmektir.

## Attributes Kuralı

Attribute alanlarını yalnızca açık bilgi varsa doldurun.

Örnek:

- `employs` için `role`
- `owns_stake_in` için `ownership_percentage`
- `acquired` için `amount`, `currency`, `time`

Bilgi açık değilse tahmin etmeyin.

## Evidence Kuralı

Evidence alanı için tam span çıkarmak zorunlu değildir. Aşağıdakilerden biri yeterlidir:

- ilgili cümle
- kısa alıntı
- ilişkiyi açıkça gösteren ifade

Kısa ve okunabilir olması yeterlidir.

## Bir Satır Ne Zaman Yazılır?

- Her geçerli ilişki için bir satır yazılır.
- İlişki yok ama ontolojiye uygun tek bir varlık açıkça geçiyorsa, yalnızca `head_text` ve `head_type` doldurularak satır açılabilir.
- Aynı ilişkiyi tekrar eden satırlar girilmez.
- Aynı varlık daha önce net biçimde kaydedildiyse gereksiz tekrar satırı açılmaz.
- Haberde ontolojiye uygun varlık veya ilişki yoksa haber boş geçilebilir.

## Boş Alan Kuralı

- İlişki yoksa `relation`, `tail_text` ve `tail_type` boş bırakılır.
- Attribute yoksa alan boş bırakılabilir veya `{}` yazılabilir.

## Inter/Intra Çalışma Düzeni

- `common 100` içindeki haberler inter-annotator agreement hesabı için kullanılır.
- Her annotator kendi dosyasını ayrı doldurur (ör. `annotations_AS.csv`, `annotations_FC.csv`, `annotations_MHC.csv`, `annotations_HTI.csv`, `annotations_SK.csv`).
- Intra-annotator kontrolü için her annotator kendi dosyasında ilk 5 anotasyonda kullandığı `doc_id` değerlerini dosyanın sonunda tekrar etiketler. 

## Mini Örnekler

```json
{
    "doc_id": "6d41c4acf8b8d1fc",
    "title": "Türk Hava Yolları'ndan yeni rota: Çin'de bir şehre daha uçacak",
    "content": "Türk Hava Yolları (THY), Çin Halk Cumhuriyeti'nin Urumçi şehrine sefer başlatma kararı aldığını duyurdu.THY'den vizesiz Avrupa için bilet kampanyasıŞirket amuyu Aydınlatma Platformu'nda (KAP) yapılan açıklamada, \"Ortaklığımız Yönetim Kurulu'nca, imkanlar ve pazar koşullarına bağlı olarak Çin Halk Cumhuriyeti'nin Urumçi şehrine tarifeli sefer başlatılmasına karar verilmiştir.\" ifadelerine yer verildi. Türk Telekom kullanıcı sayısını yüzde 12,4 artırdı",
    "url": "https://www.cnbce.com/sirket-haberleri/turk-hava-yollarindan-yeni-rota-cinde-bir-sehre-daha-ucacak-h24745",
    "date": "YAYIN TARİHİ, 10 Şubat 2026 21:00",
    "fetched_at": "2026-04-12T19:38:56.601804+00:00"
}
```

| doc_id | head_text | head_type | relation | tail_text | tail_type | attributes | evidence |
|---|---|---|---|---|---|---|---|
| 6d41c4acf8b8d1fc | Çin Halk Cumhuriyeti | Country | contains | Urumçi | Location |  | "Çin Halk Cumhuriyeti'nin Urumçi şehrine sefer" |
| 6d41c4acf8b8d1fc | Türk Hava Yolları (THY) | Company | operates_in_region | Urumçi | Location |  | "Türk Hava Yolları (THY), Çin Halk Cumhuriyeti'nin Urumçi şehrine sefer başlatma kararı aldığını duyurdu" |
| 6d41c4acf8b8d1fc | Türk Telekom | Company |  |  |  |  | "Türk Telekom" |

```json
{
    "doc_id": "7055c5d8e2e49d5c",
    "title": "SOCAR'dan, Türkiye'de 225 milyon dolarlık dev yatırım",
    "content": "Azerbaycan Cumhuriyeti Devlet Petrol Şirketi SOCAR’ın Türkiye iştiraki SOCAR Türkiye, Orta Anadolu’da bulunan bir kombine çevrim enerji santralinin devralınması için anlaşma aşamasına geçti. Söz konusu satın alma işleminin 225 milyon dolarlık bedelle gerçekleştirilmesinin planlandığı bildirildi.Interfax'ın haberine göre, Azerbaycan Başbakanı Ali Asadov, Azerbaycan-Türkiye Ortak Hükümetlerarası Komisyon Toplantısı’nda yaptığı açıklamada, SOCAR’ın Türkiye’de 870 MW kapasiteli Gama Enerji İç Anadolu enerji santralini satın almayı planladığını söyledi.Asadov, anlaşmanın 23 Aralık’ta Bakü’de düzenlenecek Azerbaycan-Türkiye Yatırım Forumu’nda imzalanmasının öngörüldüğünü belirtti. Açıklamaya göre, söz konusu işlemin tamamlanması halinde SOCAR Türkiye, Gama Enerji İç Anadolu santralinin yüzde 100 hissesinin sahibi olacak.Müzakereler daha önce başlamıştıBu yılın mayıs ayında, santralin mevcut sahibi olan Gama Enerji A.Ş.’nin, SOCAR Türkiye’den söz konusu varlık için 225 milyon ila 250 milyondolararalığında bağlayıcı olmayan bir teklif aldığı ve müzakerelere açık olduğunu duyurduğu bildirilmişti. Santralin satın alınmasının, SOCAR’ın büyüyen Türk elektrik piyasasındaki konumunu güçlendireceği ve arz fazlası koşullarında alternatif bir gelir kaynağı yaratacağı değerlendiriliyor.Portföyünde hidroelektrik ve rüzgar santralleri de bulunan Gama Enerji için olası satış, düzenlenmiş tarifeler nedeniyle karlılığı sınırlı olan doğalgazla çalışan elektrik üretiminden çıkış anlamına gelecek.SOCAR Türkiye’nin Türkiye’deki yatırımlarıSOCAR Türkiye Enerji, Petkim Petrokimya Holding’in yüzde 51 oranındaki kontrol hisselerine sahip bulunuyor. Şirket ayrıca Türkiye’de STAR petrol rafinerisini, bir konteyner terminalini ve bir elektrik santralini hayata geçirdi. SOCAR Türkiye, iç pazara doğalgaz sağlarken, Türkiye’nin birçok büyük şehrindeki havaalanlarında uçak yakıt ikmal noktalarına da petrol ürünleri tedarik ediyor. (Ekonomim)SOCAR, 90 yıllık enerji devi Italiana Petroli'yi satın aldı!MSC, İzmir’deki SOCAR Terminal’e ortak olduSocar'ın kritik Ege planı: Limanı satıyor muSOCAR ile Suriye hükümeti arasında enerji mutabakatıPetkim'den SOCAR'a 160 milyon dolarlık hisse satışıSOCAR, İGA ve THY ile ortak oldu",
    "url": "https://www.borsaningundemi.com/haber/socardan-turkiyede-225-milyon-dolarlik-dev-yatirim-1877911",
    "date": "23 Aralık 2025 - 13:07",
    "fetched_at": "2026-04-12T18:26:14.739864+00:00"
}
```

| doc_id | head_text | head_type | relation | tail_text | tail_type | attributes | evidence |
|---|---|---|---|---|---|---|---|
| 7055c5d8e2e49d5c | SOCAR | Company | headquartered_in | Azerbaycan Cumhuriyeti | Country |  | "Azerbaycan Cumhuriyeti Devlet Petrol Şirketi SOCAR" |
| 7055c5d8e2e49d5c | SOCAR Türkiye | Company | subsidiary_of | SOCAR | Company |  | "SOCAR'ın Türkiye iştiraki SOCAR Türkiye" |
| 7055c5d8e2e49d5c | SOCAR Türkiye | Company | operates_in_region | Türkiye | Country |  | "SOCAR'ın Türkiye iştiraki SOCAR Türkiye" |
| 7055c5d8e2e49d5c | Azerbaycan Cumhuriyeti | Country | prime_minister | Ali Asadov | Person |  | "Azerbaycan Başbakanı Ali Asadov" |
| 7055c5d8e2e49d5c | Gama Enerji A.Ş. | Company | operates_facility | Gama Enerji İç Anadolu enerji santrali | Facility |  | "santralin mevcut sahibi olan Gama Enerji A.Ş." |
| 7055c5d8e2e49d5c | SOCAR Türkiye | Company | owns_stake_in | Petkim Petrokimya Holding | Company | {"ownership_percentage": "%51"} | "SOCAR Türkiye Enerji, Petkim Petrokimya Holding'in yüzde 51 oranındaki kontrol hisselerine sahip bulunuyor" |
| 7055c5d8e2e49d5c | SOCAR Türkiye | Company | operates_facility | STAR petrol rafinerisi | Facility |  | "Şirket ayrıca Türkiye'de STAR petrol rafinerisini ... hayata geçirdi" |
| 7055c5d8e2e49d5c | SOCAR | Company | acquired | Italiana Petroli | Company |  | "SOCAR, 90 yıllık enerji devi Italiana Petroli'yi satın aldı!" |
| 7055c5d8e2e49d5c | SOCAR | Company | partners_with | İGA | Company |  | "SOCAR, İGA ve THY ile ortak oldu" |
| 7055c5d8e2e49d5c | SOCAR | Company | partners_with | Türk Hava Yolları (THY) | Company |  | "SOCAR, İGA ve THY ile ortak oldu" |
| 7055c5d8e2e49d5c | MSC | Company | operates_facility | SOCAR Terminal | Facility |  | "MSC, İzmir'deki SOCAR Terminal'e ortak oldu" |
| 7055c5d8e2e49d5c | SOCAR Terminal | Facility | located_in | İzmir | City |  | "MSC, İzmir'deki SOCAR Terminal'e ortak oldu" |
| 7055c5d8e2e49d5c | Türkiye | Country | contains | Orta Anadolu | Location |  | "SOCAR Türkiye, Orta Anadolu'da bulunan bir kombine çevrim enerji santralinin devralınması için anlaşma aşamasına geçti" |
| 7055c5d8e2e49d5c | Suriye | Country |  |  |  |  | "SOCAR ile Suriye hükümeti arasında enerji mutabakatı" |

## Hızlı Kontrol Listesi

- Type adı ontolojide var mı?
- Daha açık bir yüzey form kullanabilir miyim?
- Aynı varlığı haber boyunca aynı adla mı yazdım?
- Zamir yerine açık varlık adını mı kullandım?
- Relation adı ontolojide var mı?
- Kaynak-hedef yönü doğru mu?
- Metinde açıkça geçmeyen attribute ekledim mi?
