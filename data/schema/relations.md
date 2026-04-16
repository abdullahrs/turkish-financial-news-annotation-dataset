# Iliskiler (Relations)

## Genel Kurallar

- **Iliski mi, attribute mu?** Eger bilgi iki farkli varlik arasinda bir bag kuruyorsa iliski kullanin. Eger bilgi tek bir varliga ait bir ozellikse attribute kullanin.
  - "THY'nin 35.000 calisani var" → `employee_count` attribute
  - "THY, Mehmet Bey'i CEO olarak istihdam ediyor" → `employs` iliskisi
- **Yonlu iliskiler:** Tum iliskiler kaynak → hedef yonundedir. "A, B'nin bagla sirketidir" → A `subsidiary_of` B.

---

## Istihdam ve Uyelik

### employs
**Kaynak:** Company, Institution, Organization → **Hedef:** Person

Bir kurumun bir kisiyi istihdam etmesi. Yonetici atamalari, gorev degisiklikleri dahil. Gecmis/gelecek zaman farki gozetilmez: atama, ayrilma veya istifa haberleri de bu iliski ile ifade edilir. Liderlik pozisyonlari ("X Kurumu Baskani Y", "Tesla CEO'su Elon Musk") icin her zaman `employs` kullanilir. Bir kisi ayni anda birden fazla kurum tarafindan istihdam edilebilir (ornek: bir holdingin yonetim kurulu uyesi ayrica bagli sirketin CEO'su).

| Attribute | Aciklama |
|---|---|
| `role` | Gorev/pozisyon |
| `start_date` | Baslangic tarihi |
| `end_date` | Bitis tarihi |

> "Garanti BBVA, Recep Bastug'u CEO olarak atadi." → Garanti BBVA `employs` Recep Bastug {role: "CEO"}

### member_of
**Kaynak:** Person, Country → **Hedef:** Organization, Institution

Bir kisinin bir kurulusa uyeligi veya bir ulkenin uluslararasi orgut/kurulusa uyeligi. (Ulke uyeligi icin "Devlet Yonetimi" bolumundeki ornege bakin.)

| Attribute | Aciklama |
|---|---|
| `start_date` | Uyeligin baslangic tarihi |
| `end_date` | Uyeligin bitis tarihi |
| `role` | Uyelikteki ozel rol (yalnizca Person kaynak icin; ornek: "danisma kurulu uyesi") |

> "Mehmet Simsek, G20 danisma kurulu uyesidir." → Mehmet Simsek `member_of` G20 {role: "danisma kurulu uyesi"}

### has_members
**Kaynak:** Organization → **Hedef:** Company, Person

Bir organizasyonun uyelerini belirtir. `member_of`'un tersi yonudur.

| Attribute | Aciklama |
|---|---|
| `since_date` | Uyeligin baslangic tarihi |
| `role` | Uyenin organizasyondaki rolu |

> "TUSIAD'in uyeleri arasinda Koc Holding var." → TUSIAD `has_members` Koc Holding

---

## Kurumsal Yapi

### subsidiary_of
**Kaynak:** Company → Company | Institution → Institution

Bir sirketin/kurumun baska bir sirketin/kurumun bagli kurulusu olmasi. Yalnizca dogrudan baglilik icin kullanin.

| Attribute | Aciklama |
|---|---|
| `ownership_percentage` | Ana sirketin/kurumun bagli kuruluştaki sahiplik orani |
| `since_date` | Bagliligin baslangic tarihi |

> "Yapi Kredi, Koc Finansal Hizmetler'in bagla sirketidir." → Yapi Kredi `subsidiary_of` Koc Finansal Hizmetler

### spun_off_from
**Kaynak:** Company → **Hedef:** Company

Bir sirketin baska bir sirketten ayrilarak kurulmasi.

| Attribute | Aciklama |
|---|---|
| `spinoff_date` | Ayrilma/bagimsizlasma tarihi |

> "PayPal, eBay'den ayrilarak bagimsiz bir sirket oldu." → PayPal `spun_off_from` eBay

### acquired
**Kaynak:** Company → **Hedef:** Company

Bir sirketin baska bir sirketi satin almasi. Tarihsel olarak ayni sirket farkli zamanlarda farkli sirketler tarafindan satin alinabilir; bu iliski cardinality acisindan m:n olarak modellenir.

| Attribute | Aciklama |
|---|---|
| `amount` | Satin alma tutari |
| `currency` | Para birimi |
| `time` | Islem tarihi |

> "Getir, FreshDirect'i 50M dolara satin aldi." → Getir `acquired` FreshDirect {amount: "50M", currency: "USD"}

### founded_by
**Kaynak:** Company → **Hedef:** Person

Sirketin kurucusu.

| Attribute | Aciklama |
|---|---|
| `founding_date` | Kurulus tarihi |

> "Togg, Mehmet Gurcan Karakas onculugunde kuruldu." → Togg `founded_by` Mehmet Gurcan Karakas

---

## Sahiplik ve Yatirim

### owns_stake_in
**Kaynak:** Person, Company, Institution → **Hedef:** Company, Financial_Instrument

Bir varliktaki hisse/pay sahipligi.

| Attribute | Aciklama |
|---|---|
| `ownership_percentage` | Sahiplik orani |
| `paid_amount` | Odenen tutar |

> "TVF, Turk Telekom'un %6.68 hissesini aldi." → TVF `owns_stake_in` Turk Telekom {ownership_percentage: "%6.68"}

### invests_in
**Kaynak:** Person, Company, Institution, Country → **Hedef:** Company, Country, Financial_Instrument, Financial_Market

Yatirim yapmak. `owns_stake_in`'den farki: dogrudan sahiplik orani belirtilmez, genel yatirim ifadesi icin kullanilir.

| Attribute | Aciklama |
|---|---|
| `investment_amount` | Yatirim tutari |
| `investment_date` | Yatirim tarihi |

> "Suudi Arabistan Varlik Fonu, Turkiye'ye 5 milyar dolar yatirim yapacak." → Suudi Arabistan `invests_in` Turkiye {investment_amount: "5 milyar dolar"}

### issues
**Kaynak:** Company, Institution → **Hedef:** Financial_Instrument

Bir sirketin veya kurumun finansal arac ihrac etmesi (tahvil, bono vb.). Hazine, merkez bankasi gibi devlet kurumlarinin tahvil ihraci `Institution` kaynak ile modellenir; ulkeler dogrudan ihrac eden taraf olarak kullanilmaz.

| Attribute | Aciklama |
|---|---|
| `issue_date` | Ihrac tarihi |
| `issue_amount` | Ihrac tutari |
| `currency` | Para birimi |

> "Hazine 10 yillik tahvil ihrac etti." → Hazine `issues` 10 yillik tahvil
> Not: Sirket hisseleri Financial_Instrument olarak ihrac edilmez; sirketin borsa varligi `Company` `listed_on` `Financial_Market` iliskisi ile gosterilir.

---

## Konum ve Cografi

### headquartered_in
**Kaynak:** Company, Institution → **Hedef:** City, Country

Sirket veya kurum merkezi konumu. Institution icin de kullanilir (TCMB, SPK gibi kurumlarin merkez konumu icin); Institution icin ayrica `located_in` tanimli degildir.

| Attribute | Aciklama |
|---|---|
| `since_date` | Merkez konumun bu lokasyonda olmaya basladigi tarih |

> "Koc Holding'in merkezi Istanbul'dadir." → Koc Holding `headquartered_in` Istanbul
> "TCMB, Ankara'da bulunmaktadir." → TCMB `headquartered_in` Ankara

### operates_in_region
**Kaynak:** Company → **Hedef:** Country, City, Location

Sirketin faaliyet gosterdigi cografi bolge.

| Attribute | Aciklama |
|---|---|
| `since_date` | Bolgede faaliyetin baslama tarihi |

> "THY, 130 ulkeye ucus gerceklestiriyor." → THY `operates_in_region` [ilgili ulkeler]

### operates_in_sector
**Kaynak:** Company → **Hedef:** Sector

Sirketin faaliyet gosterdigi sektor.

| Attribute | Aciklama |
|---|---|
| `since_date` | Sektorde faaliyetin baslama tarihi |

> "Sabanci Holding enerji sektorunde faaliyet gosteriyor." → Sabanci Holding `operates_in_sector` enerji

### located_in
**Kaynak:** Organization, Financial_Market, Facility → **Hedef:** City, Country, Location

Genel konum iliskisi. `headquartered_in`'den farki: Company/Institution disindaki varliklar (organizasyon, borsa, tesis vb.) icin kullanilir.

| Attribute | Aciklama |
|---|---|
| `since_date` | Bu konumda olmaya baslama tarihi (Organization ve Facility icin; Financial_Market.located_in attribute almaz) |

> "Borsa Istanbul, Istanbul'da konumlanmistir." → Borsa Istanbul `located_in` Istanbul

### contains
**Kaynak:** Country, City, Location → **Hedef:** City, Location, Country, Financial_Market

Cografi icerir iliskisi. Ulke sehirleri/borsalari icerir, sehir bolgeleri/borsalari icerir, makro bolgeler ulke/sehir/borsalar icerebilir.

> "Turkiye, Istanbul'u icerir." → Turkiye `contains` Istanbul
> "Istanbul, Borsa Istanbul'u icerir." → Istanbul `contains` Borsa Istanbul
> "Avrupa bolgesi London Stock Exchange'i icerir." → Avrupa `contains` London Stock Exchange

---

## Piyasa ve Listeleme

### listed_on
**Kaynak:** Company, Financial_Instrument → **Hedef:** Financial_Market

Borsada kote/islem gormek veya bir endeksin/araclarin ana borsaya/piyasaya bagli olmasi. Sirket borsada islem goruyorsa, finansal arac (tahvil, endeks vb.) borsada koteyse veya alt endeks ana piyasaya baglaniyorsa kullanilir.

Attribute setleri kaynak tipine gore farklilasir:

**Company kaynak icin:**

| Attribute | Aciklama |
|---|---|
| `ticker_symbol` | Borsa islem kodu |

**Financial_Instrument kaynak icin:**

| Attribute | Aciklama |
|---|---|
| `listing_start_date` | Listelemenin baslama tarihi |
| `listing_end_date` | Listelemenin bitis tarihi |

> Not: Sirketin genel borsa kodu Company entity'sinin `ticker_symbol` attribute'unda tutulur. `listed_on` iliskisindeki `ticker_symbol` ise o sirketin o borsada islem gordugu kod icin kullanilir (ayni sirketin farkli borsalarda farkli kodlari olabilir).

> "THY, Borsa Istanbul'da THYAO koduyla islem goruyor." → THY `listed_on` Borsa Istanbul {ticker_symbol: "THYAO"}

### includes
**Kaynak:** Sector, Financial_Instrument → **Hedef:** Company, Product, Financial_Instrument

Bilesim/icerik iliskisi. Iki farkli baglamda kullanilir:
- **Sektor:** Sektore dahil sirket/urunler
- **Endeks:** Endekse dahil sirket/araclar (`instrument_type: "endeks"` olan Financial_Instrument icin). Bir sirket endekste yer aliyorsa bu iliski kullanilir.

> "BIST 30 endeksi, THY'yi iceriyor." → BIST 30 `includes` THY
> "Bankacilik sektoru, Garanti BBVA'yi kapsiyor." → Bankacilik `includes` Garanti BBVA

---

## Uretim ve Hizmet

### produces
**Kaynak:** Company → **Hedef:** Product

Fiziksel urun uretimi. Fabrikada uretilen, somut urunler icin.

> "Togg, T10X elektrikli aracini uretiyor." → Togg `produces` T10X

### offers_service
**Kaynak:** Company → **Hedef:** Product

Hizmet veya dijital urun sunma. Fiziksel uretim olmayan hizmetler icin.

> "Garanti BBVA, mobil bankacilik hizmeti sunuyor." → Garanti BBVA `offers_service` mobil bankacilik

**`produces` vs `offers_service`:** Eger urun fiziksel olarak uretiliyorsa `produces`, hizmet/platform/dijital urun ise `offers_service`.

### operates_facility
**Kaynak:** Company → **Hedef:** Facility

Sirketin islettigi tesis.

> "Ford Otosan, Golcuk fabrikasini isletiyor." → Ford Otosan `operates_facility` Golcuk Fabrikasi

### component_of
**Kaynak:** Product → **Hedef:** Product

Bir urunun baska bir urunun parcasi olmasi.

> "Turkcell cip, Togg T10X'te kullaniliyor." → Turkcell cip `component_of` Togg T10X

### targets_sector
**Kaynak:** Product → **Hedef:** Sector

Urunun hedefledigi sektor.

> "Bu yazilim bankacilik sektorune yonelik." → yazilim `targets_sector` bankacilik

---

## Rekabet ve Isbirligi

### competes_with
**Kaynak:** Company → **Hedef:** Company

Dogrudan rekabet iliskisi.

| Attribute | Aciklama |
|---|---|
| `market` | Rekabetin gerceklestigi pazar/sektor/cografi alan |

> "THY ile Pegasus ayni pazarda rekabet ediyor." → THY `competes_with` Pegasus {market: "havayolu tasimaciligi"}

### partners_with
**Kaynak:** Company, Institution, Organization → **Hedef:** Company, Organization, Institution

Resmi ortaklik veya isbirligi.

| Attribute | Aciklama |
|---|---|
| `partnership_type` | Ortaklik turu |

> "Koc Holding, Fiat ile ortak girisimleri var." → Koc Holding `partners_with` Fiat {partnership_type: "ortak girisim"}

### trades_with
**Kaynak:** Country → **Hedef:** Country, Organization, Company, Institution

Ulkeler arasi veya ulke-kurum arasi ticaret iliskisi.

> "Turkiye, Almanya ile dis ticaret yapiyor." → Turkiye `trades_with` Almanya

---

## Duzenleme ve Hukuk

### regulates
**Kaynak:** Institution → **Hedef:** Company, Financial_Market, Financial_Instrument, Sector

Duzenleyici kurumun denetim/duzenleme yetkisi.

| Attribute | Aciklama |
|---|---|
| `regulation_scope` | Duzenlemenin kapsami/alani |

> "SPK, sermaye piyasalarini duzenliyor." → SPK `regulates` sermaye piyasalari {regulation_scope: "sermaye piyasalari"}

### has_legal_issues_with
**Kaynak:** Person, Company, Country → **Hedef:** Company, Institution, Country

Hukuki uyusmazlik, dava veya sorusturma. Yalnizca yapisal nitelikteki davalar icin (olay-bazli gunluk haberler icin degil). Sirket-ulke veya kisi-ulke davalari da bu iliski ile ifade edilir.

| Attribute | Aciklama |
|---|---|
| `case_type` | Dava turu |
| `description` | Aciklama |

> "Rekabet Kurulu, Google'a sorusturma acti." → Google `has_legal_issues_with` Rekabet Kurulu {case_type: "rekabet sorusturmasi"}

### sanctions
**Kaynak:** Country → **Hedef:** Company, Country

Ulkenin baska bir ulkeye veya sirkete uyguladigi yaptirim.

| Attribute | Aciklama |
|---|---|
| `sanction_type` | Yaptirim turu |

> "ABD, Rusya'ya ekonomik yaptirim uyguladi." → ABD `sanctions` Rusya {sanction_type: "ekonomik"}

### appoints_trustee
**Kaynak:** Institution → **Hedef:** Company, Organization

Kayyum atanmasi. Bir duzenleyici kurumun bir sirkete/kurulusa kayyum atamasi.

| Attribute | Aciklama |
|---|---|
| `appointment_date` | Atama tarihi |
| `reason` | Atama gerekcesi |

> "TMSF, Bank Asya'ya kayyum atadi." → TMSF `appoints_trustee` Bank Asya {appointment_date: "2016-05"}

---

## Derecelendirme

### assigns_rating_to
**Kaynak:** Company, Institution → **Hedef:** Company, Country, Financial_Instrument

Kredi derecelendirme kurulusu veya kurumun not vermesi.

| Attribute | Aciklama |
|---|---|
| `rating_value` | Verilen not |
| `rating_date` | Not tarihi |

> "Fitch, Turkiye'nin kredi notunu BB+ olarak teyit etti." → Fitch `assigns_rating_to` Turkiye {rating_value: "BB+"}

---

## Devlet Yonetimi

### president
**Kaynak:** Country → **Hedef:** Person

Ulkenin cumhurbaskani. Eski cumhurbaskanlari da bu iliski ile modellenir; mevcut/eski ayrimi `start_date`/`end_date` attributelari ile yapilir.

| Attribute | Aciklama |
|---|---|
| `start_date` | Gorev baslangic tarihi |
| `end_date` | Gorev bitis tarihi (mevcut gorevde ise bos birakilir) |

> "Turkiye Cumhurbaskani Erdogan..." → Turkiye `president` Erdogan {start_date: "2014-08-28"}

### prime_minister
**Kaynak:** Country → **Hedef:** Person

Ulkenin basbakani.

| Attribute | Aciklama |
|---|---|
| `start_date` | Gorev baslangic tarihi |
| `end_date` | Gorev bitis tarihi |

> "Ingiltere Basbakani Starmer..." → Ingiltere `prime_minister` Starmer

### minister
**Kaynak:** Country → **Hedef:** Person

Ulkenin bakani.

| Attribute | Aciklama |
|---|---|
| `ministry_name` | Bakanlik adi |
| `start_date` | Gorev baslangic tarihi |
| `end_date` | Gorev bitis tarihi |

> "Mehmet Simsek, Hazine ve Maliye Bakani." → Turkiye `minister` Mehmet Simsek {ministry_name: "Hazine ve Maliye Bakanligi"}

### central_bank
**Kaynak:** Country → **Hedef:** Institution

Ulkenin merkez bankasi.

> "TCMB, Turkiye'nin merkez bankasidir." → Turkiye `central_bank` TCMB

### member_of (Country kaynak ozeti)

Country kaynagi icin `member_of` iliskisi. Tam tanim icin "Istihdam ve Uyelik" bolumundeki [member_of](#member_of) maddesine bakin. Ulke kaynagi icin attribute olarak yalnizca `start_date` ve `end_date` kullanilir; `role` Person kaynagina ozeldir.

> "Turkiye, NATO uyesidir." → Turkiye `member_of` NATO {start_date: "1952-02-18"}
