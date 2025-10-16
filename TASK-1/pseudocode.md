Ad Soyad : Şevval Yıldız
Öğrenci no: 250541025

# Değişkenlerin başlatılması
YAZ "Kartı takınız..."
OKU kart_ok  # (sadece akış için; gerçek sürümde kart verisi okunur)

# Hesap verileri (örnek/başlangıç değerleri)
account_PIN_doğru <- 1234          # sistemde kayıtlı PIN (örnek)
hesap_bakiye <- 5000               # örnek bakiye (TL)
gunluk_limit <- 2000               # günlük çekim limiti (TL)
gunluk_cekilen <- 0                # bugünkü çekilen toplam (başlangıç)

# PIN doğrulama - 3 hak
max_hak <- 3
kalan_hak <- max_hak
pin_dogrulandi <- HAYIR

DONGU (kalan_hak > 0) YAP
    YAZ "Lütfen 4 haneli PIN'inizi giriniz:"
    OKU girilen_PIN
    EGER-ISE (girilen_PIN = account_PIN_doğru) O ZAMAN
        pin_dogrulandi <- EVET
        YAZ "PIN doğrulandı."
        DUR  # PIN doğrulandı, döngüden çık
    DEGILSE
        kalan_hak <- kalan_hak - 1
        EGER-ISE (kalan_hak > 0) O ZAMAN
            YAZ "Hatalı PIN. Kalan hak: " + kalan_hak
        DEGILSE
            YAZ "Kartınız bloke edildi. Lütfen bankayla iletişime geçin."
            # Kart bloke işlemi burada yapılır (sisteme bildirim vs.)
            SON  # Program sonlanır veya ana menüye dönmez
        BITIR_EGER
    BITIR_EGER
BITIR_DONGU

EGER-ISE (pin_dogrulandi = HAYIR) O ZAMAN
    SON  # PIN doğrulanmadıysa işlem sona erer
BITIR_EGER

# İşlem döngüsü - kullanıcı tekrar işlem yapmak isteyebilir
islem_tekrar <- EVET
DONGU (islem_tekrar = EVET) YAP

    # Kullanıcıdan işlem seçimi (sadece para çekme için)
    YAZ "İşlem seçiniz: 1) Para Çekme  2) Çıkış"
    OKU secim
    EGER-ISE (secim = 2) O ZAMAN
        YAZ "Çıkış yapılıyor. Hoşça kalın."
        islem_tekrar <- HAYIR
        DUR
    BITIR_EGER

    # Para çekme adımları
    YAZ "Çekmek istediğiniz tutarı giriniz (TL):"
    OKU tutar

    # Tutar geçerlilik kontrolleri
    EGER-ISE (tutar <= 0) O ZAMAN
        YAZ "Geçersiz tutar. Lütfen pozitif bir tutar girin."
        # İşlemi tekrar sormak için döngüye geri dön
        YAZ "Yeni işlem yapmak ister misiniz? (Evet/Hayır)"
        OKU cevap1
        EGER-ISE (cevap1 = "Evet" VEYA cevap1 = "evet" VEYA cevap1 = "E") O ZAMAN
            islem_tekrar <- EVET
            DUR
        DEGILSE
            islem_tekrar <- HAYIR
            SON
        BITIR_EGER
    BITIR_EGER

    # 20 TL katları kontrolü
    EGER-ISE (tutar MOD 20 <> 0) O ZAMAN
        YAZ "Tutar 20 TL'nin katı olmalıdır. Lütfen 20, 40, 60 ... gibi bir tutar girin."
        YAZ "Yeni işlem yapmak ister misiniz? (Evet/Hayır)"
        OKU cevap2
        EGER-ISE (cevap2 = "Evet" VEYA cevap2 = "evet" VEYA cevap2 = "E") O ZAMAN
            islem_tekrar <- EVET
            DUR
        DEGILSE
            islem_tekrar <- HAYIR
            SON
        BITIR_EGER
    BITIR_EGER

    # Bakiye kontrolü
    EGER-ISE (tutar > hesap_bakiye) O ZAMAN
        YAZ "Yetersiz bakiye. Hesabınızda " + hesap_bakiye + " TL bulunmaktadır."
        YAZ "Yeni işlem yapmak ister misiniz? (Evet/Hayır)"
        OKU cevap3
        EGER-ISE (cevap3 = "Evet" VEYA cevap3 = "evet" VEYA cevap3 = "E") O ZAMAN
            islem_tekrar <- EVET
            DUR
        DEGILSE
            islem_tekrar <- HAYIR
            SON
        BITIR_EGER
    BITIR_EGER

    # Günlük limit kontrolü
    EGER-ISE ((gunluk_cekilen + tutar) > gunluk_limit) O ZAMAN
        kalan_limit <- gunluk_limit - gunluk_cekilen
        EGER-ISE (kalan_limit <= 0) O ZAMAN
            YAZ "Bugün için çekebileceğiniz tutar bitmiştir. Günlük limit: " + gunluk_limit + " TL"
        DEGILSE
            YAZ "Günlük limit aşılıyor. Kalan çekilebilir tutar bugün için: " + kalan_limit + " TL"
        BITIR_EGER

        YAZ "Yeni işlem yapmak ister misiniz? (Evet/Hayır)"
        OKU cevap4
        EGER-ISE (cevap4 = "Evet" VEYA cevap4 = "evet" VEYA cevap4 = "E") O ZAMAN
            islem_tekrar <- EVET
            DUR
        DEGILSE
            islem_tekrar <- HAYIR
            SON
        BITIR_EGER
    BITIR_EGER

    # Tüm kontroller geçti -> işlem onayı
    YAZ "İşlem onayı: " + tutar + " TL çekilecek. Onaylıyor musunuz? (Evet/Hayır)"
    OKU onay
    EGER-ISE (onay = "Evet" VEYA onay = "evet" VEYA onay = "E") O ZAMAN
        # Hesap güncellemeleri
        hesap_bakiye <- hesap_bakiye - tutar
        gunluk_cekilen <- gunluk_cekilen + tutar

        # Nakit hazırlama simülasyonu
        YAZ "Lütfen paranızı alınız..."
        YAZ tutar + " TL verildi."

        # Makbuz seçeneği
        YAZ "Makbuz almak ister misiniz? (Evet/Hayır)"
        OKU makbuz
        EGER-ISE (makbuz = "Evet" VEYA makbuz = "evet" VEYA makbuz = "E") O ZAMAN
            YAZ "Makbuz yazdırılıyor..."
            YAZ "Makbuz: Çekilen tutar: " + tutar + " TL, Kalan bakiye: " + hesap_bakiye + " TL, Bugün çekilen toplam: " + gunluk_cekilen + " TL"
        DEGILSE
            YAZ "Makbuz tercih edilmedi."
        BITIR_EGER

    DEGILSE
        YAZ "İşlem onaylanmadı. Para çekme işlemi iptal edildi."
    BITIR_EGER

    # İşlem sonrası: tekrar ister mi?
    YAZ "Başka işlem yapmak ister misiniz? (Evet/Hayır)"
    OKU tekrar_cevap
    EGER-ISE (tekrar_cevap = "Evet" VEYA tekrar_cevap = "evet" VEYA tekrar_cevap = "E") O ZAMAN
        islem_tekrar <- EVET
    DEGILSE
        islem_tekrar <- HAYIR
        YAZ "Kartınızı alınız. Hoşça kalın."
        SON
    BITIR_EGER

BITIR_DONGU  # islem_tekrar döngüsü biter

SON
