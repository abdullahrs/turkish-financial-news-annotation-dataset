# Varlik Tipleri (Entity Types)

## Person

Ekonomik, finansal veya yonetimsel rol ustlenen bireyler. CEO, yatirimci, ekonomist, bakan gibi.

**Ne zaman kullanilir:** Haberde ismiyle anilan ve bir kurumda gorevi olan veya ekonomik etkisi olan bir kisiden bahsedildiginde.

**Ne zaman kullanilmaz:** Sadece ismi gecen ama ekonomik/finansal baglami olmayan kisiler icin kullanilmaz.

| Attribute | Aciklama | Ornek |
|---|---|---|
| `name` | Tam isim | "Mehmet Simsek" |
| `date_of_birth` | Dogum tarihi | "1967-01-01" |
| `nationality` | Uyruk | "Turk" |
| `specialization` | Uzmanlik alani (gorevden farkli) | "makroekonomi", "enerji sektoru" |

> **Not:** Kisinin guncel gorevi/unvani (CEO, Bakan, Genel Mudur vb.) Person attribute degildir; iliski uzerinde tasinir: `Company/Institution/Organization` `employs` `Person {role: "..."}` veya `Country` `minister`/`president`/`prime_minister` `Person`.

---

## Company

Ticari faaliyet gosteren kuruluslar. Ozel sirketler, bankalar, holdingler, sigorta sirketleri dahil.

**Ne zaman kullanilir:** Kar amaci guderek mal/hizmet ureten veya satan her turlu ticari isletme.

**Company vs Organization:** Kar amaci varsa Company, yoksa (STK, universite, sendika) Organization.
**Company vs Institution:** Devlet/duzenleyici kurum ise Institution, ozel sektorse Company.

| Attribute | Aciklama | Ornek |
|---|---|---|
| `name` | Resmi sirket adi | "Turk Hava Yollari A.O." |
| `trade_name` | Ticari/bilinen adi | "THY" |
| `ticker_symbol` | Borsa kodu | "THYAO" |
| `category` | Sirket kategorisi | "banka", "holding", "sigorta" |
| `founding_date` | Kurulus tarihi | "1933" |
| `ownership_type` | Mulkiyet yapisi | "ozel", "kamu", "karma" |
| `revenue` | Gelir/ciro | "150 milyar TL" |
| `profit` | Kar | "23 milyar TL net kar" |
| `market_cap` | Piyasa degeri | "380 milyar TL" |
| `employee_count` | Calisan sayisi | "35000" |
| `public_private_status` | Halka aciklik durumu | "halka acik", "ozel" |

---

## Institution

Devlete bagli resmi/duzenleyici kurumlar. Bakanliklar, merkez bankalari, duzenleyici otoriteler.

**Ne zaman kullanilir:** Yasal yetkiyle duzenleyici, denetleyici veya idari islem yapan kamu kurumlari.

**Institution vs Organization:** Yasal/resmi otorite ise Institution, sivil toplum ise Organization.

| Attribute | Aciklama | Ornek |
|---|---|---|
| `name` | Tam resmi adi | "Sermaye Piyasasi Kurulu" |
| `acronym` | Kisaltma | "SPK" |
| `institution_type` | Kurum turu | "duzenleyici otorite" |
| `institution_type_detail` | Tur detayi | "sermaye piyasasi duzenleyicisi" |
| `establishment_date` | Kurulus tarihi | "1981" |
| `jurisdiction` | Yetki alani | "Turkiye" |
| `regulatory_scope` | Duzenleme kapsami | "sermaye piyasalari" |
| `mandate` | Gorev tanimi | "yatirimci haklarini korumak" |

---

## Organization

Kar amaci gutmeyen sivil kuruluslar. Universiteler, sendikalar, dernekler, STK'lar, federasyonlar.

**Ne zaman kullanilir:** Ticari olmayan ama ekonomik baglami olan sivil toplum kuruluslari.

| Attribute | Aciklama | Ornek |
|---|---|---|
| `name` | Kurulus adi | "TUSIAD" |
| `organization_type` | Tip | "is insanlari dernegi" |
| `organization_type_detail` | Tip detayi | "sanayici ve is insanlari platformu" |
| `founding_date` | Kurulus tarihi | "1971" |
| `mission_statement` | Misyon/amac | "sanayiciyi temsil etmek" |
| `membership_count` | Uye sayisi | "4500" |
| `scope` | Faaliyet kapsami | "ulusal", "uluslararasi" |

---

## Sector

Benzer mal/hizmet ureten sirketlerden olusan sanayi veya ekonomik alan.

**Ne zaman kullanilir:** Haberde bir endustri veya sektorel alandan bahsedildiginde. Sirketlerin faaliyet gosterdigi alani tanimlar.

| Attribute | Aciklama | Ornek |
|---|---|---|
| `name` | Sektor adi | "bankaci lik", "enerji", "madencilik" |
| `classification_code` | Siniflandirma kodu | "NACE 64" |
| `total_market_size` | Toplam piyasa buyuklugu | "2.5 trilyon TL" |

---

## Financial_Instrument

Finansal piyasalarda islem goren veya takip edilen varliklar. Tahvil, doviz, emtia, ETF, kripto para, yatirim fonlari ve finansal endeksler (BIST 100, S&P 500, DOW Jones) dahil.

**Ne zaman kullanilir:** Alim-satima konu olan veya degeri olculen her turlu finansal arac ile piyasa endeksleri. Endeksler `instrument_type: "endeks"` ile bu tipe dahildir; endeks → sirket uyeligi icin `includes`, endeks → borsa baglantisi icin `listed_on` kullanilir.

**Ne zaman kullanilmaz:** Bir sirketin hisse senedi Financial_Instrument olarak modellenmez. Sirketin borsadaki varligi `Company` `listed_on` `Financial_Market` iliskisi ile ve sirketin kendi `ticker_symbol` attribute'u ile gosterilir. Financial_Instrument; sirketten bagimsiz varliklar (endeks, doviz, emtia, ETF, fon, kripto, devlet/kurumsal tahvili vb.) icin kullanilir.

**`instrument_type` degerleri ornekleri:** "tahvil", "doviz", "emtia", "endeks", "fon", "kripto", "ETF"

| Attribute | Aciklama | Ornek |
|---|---|---|
| `name` | Aracin adi | "BIST 100", "ABD Dolari" |
| `isin` | Uluslararasi kimlik kodu | "TREBIST10018" |
| `ticker_symbol` | Borsa sembol | "XU100" |
| `instrument_type` | Arac turu | "endeks", "doviz", "tahvil" |
| `instrument_type_detail` | Tur detayi | "agirlikli ortalama fiyat endeksi" |
| `issue_date` | Ihrac/baslangic tarihi | "1997-01-02" |
| `maturity_date` | Vade tarihi (tahviller icin) | "2030-06-15" |
| `face_value` | Nominal deger | "100 TL" |
| `coupon_rate` | Kupon orani (tahviller) | "%12.5" |
| `current_price` | Guncel fiyat | "9850 puan" |
| `currency` | Para birimi | "TRY" |
| `number_of_constituents` | Bilesen sayisi (endeksler) | "100" |
| `base_value` | Baz deger (endeksler) | "1000" |

---

## Financial_Market

Finansal araclarin islem gordugu platform veya borsa.

**Ne zaman kullanilir:** Bir borsa, piyasa veya islem platformundan bahsedildiginde.

| Attribute | Aciklama | Ornek |
|---|---|---|
| `name` | Piyasa adi | "Borsa Istanbul" |
| `market_type` | Piyasa turu | "", "doviz piyasasi" |
| `market_type_detail` | Tur detayi | "pay piyasasi ve vadeli islem" |
| `establishment_date` | Kurulus tarihi | "1985" |
| `market_cap` | Toplam piyasa degeri | "8 trilyon TL" |
| `number_of_listings` | Kote sirket/arac sayisi | "540" |

---

## Country

Egemen devlet/ulke. Finansal ve kurumsal faaliyetlerin konumunu veya yetki alanini belirtir.

| Attribute | Aciklama | Ornek |
|---|---|---|
| `name` | Ulke adi | "Turkiye" |
| `capital_city` | Baskent | "Ankara" |
| `currency_code` | Para birimi kodu | "TRY" |

---

## City

Bir ulke icindeki idari kentsel birim. Genellikle merkez veya piyasa konumu olarak kullanilir.

| Attribute | Aciklama | Ornek |
|---|---|---|
| `name` | Sehir adi | "Istanbul" |
| `major_industries` | Baskin endustriler | "finans, turizm, lojistik" |

---

## Location

Ulke veya sehir olmayan cografi alanlar. Makro bolgeler (Avrupa, MENA, Asya-Pasifik) ve alt bolgeler (sanayi bolgeleri, serbest bolgeler).

**Ne zaman kullanilir:** "Avrupa pazari", "Ortadogu", "Organize Sanayi Bolgesi" gibi ifadeler icin.

**Ne zaman kullanilmaz:** Eger bir ulke veya sehir ise Country/City kullanin.

| Attribute | Aciklama | Ornek |
|---|---|---|
| `name` | Bolge adi | "Avrupa", "Gebze OSB" |
| `location_type` | Bolge turu | "makro bolge", "sanayi bolgesi" |
| `location_type_detail` | Tur detayi | "Avrupa Birligi uye ulkeleri" |
| `primary_activities` | Temel faaliyetler | "otomotiv uretimi" |

---

## Product

Sirketlerin urettigi veya sundugu fiziksel urunler, hizmetler veya platformlar.

**Ne zaman kullanilir:** Bir sirketin urettigi/sattigi somut bir urun veya platformdan bahsedildiginde.

| Attribute | Aciklama | Ornek |
|---|---|---|
| `name` | Urun adi | "Togg T10X" |
| `product_category` | Urun kategorisi | "otomobil" |
| `product_category_detail` | Kategori detayi | "elektrikli SUV" |
| `brand_name` | Marka adi | "Togg" |
| `launch_date` | Piyasaya cikis tarihi | "2023-03" |
| `target_market` | Hedef pazar | "Turkiye ve Avrupa" |
| `price` | Fiyat | "1.3 milyon TL" |

---

## Facility

Sirket veya kurumun sahip oldugu/islettigi fiziksel tesis. Fabrika, sube, santral, veri merkezi, ofis.

**Ne zaman kullanilir:** Bir uretim tesisi, sube veya fiziksel operasyon noktasindan bahsedildiginde.

| Attribute | Aciklama | Ornek |
|---|---|---|
| `name` | Tesis adi | "Togg Gemlik Fabrikasi" |
| `facility_type` | Tesis turu | "fabrika" |
| `facility_type_detail` | Tur detayi | "elektrikli arac uretim tesisi" |
| `opening_date` | Acilis tarihi | "2022-10" |
| `capacity` | Kapasite | "yillik 175.000 arac" |
| `number_of_employees` | Calisan sayisi | "4300" |
| `investment_amount` | Yatirim tutari | "500 milyon dolar" |
| `operational_status` | Operasyonel durum | "aktif", "yapim asamasinda" |
| `address` | Adres | "Gemlik, Bursa" |
