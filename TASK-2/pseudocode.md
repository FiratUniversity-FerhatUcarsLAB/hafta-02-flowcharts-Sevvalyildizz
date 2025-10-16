BAŞLA

// 1. Kullanıcı Girişi
YAZ "Kullanıcı adı giriniz:"
KULLANICI_ADI ← GİRİŞ AL
YAZ "Şifre giriniz:"
ŞİFRE ← GİRİŞ AL

EĞER (KULLANICI_ADI ve ŞİFRE doğruysa) İSE
    YAZ "Giriş başarılı. Hoş geldiniz!"
DEĞİLSE
    YAZ "Kullanıcı adı veya şifre hatalı! Çıkılıyor..."
    BİTİR
SON

// 2. Ürün Kategorileri Arasında Gezinme
TEKRAR
    YAZ "Kategoriler: 1-Elektronik, 2-Giyim, 3-Kitap, 4-Çıkış"
    KATEGORİ ← GİRİŞ AL
    
    EĞER KATEGORİ = 4 İSE
        ÇIKIŞ DÖNGÜSÜ
    DEĞİLSE
        YAZ "Seçilen kategoriye ait ürünler listeleniyor..."
        // Ürün Listesi Göster
        YAZ "Sepete eklemek istediğiniz ürün ID'sini giriniz (veya 0: kategoriye dön):"
        ÜRÜN_ID ← GİRİŞ AL
        
        EĞER ÜRÜN_ID = 0 İSE
            DEVAM ET
        DEĞİLSE
            // 3. Stok Kontrolü
            STOK ← STOK_KONTROL(ÜRÜN_ID)
            EĞER STOK > 0 İSE
                YAZ "Ürün sepete eklendi."
                SEPETE_EKLE(ÜRÜN_ID)
            DEĞİLSE
                YAZ "Üzgünüz, ürün stokta yok!"
            SON
        SON
    SON
TEKRARLA

// 4. Sepeti Görüntüleme ve Düzenleme
TEKRAR
    YAZ "Sepetiniz:"
    SEPET_GÖSTER()
    YAZ "1-Ürün Sil, 2-Adet Değiştir, 3-İndirim Kodu Uygula, 4-Ödemeye Geç"
    SEÇİM ← GİRİŞ AL

    EĞER SEÇİM = 1 İSE
        YAZ "Silmek istediğiniz ürün ID'sini giriniz:"
        ÜRÜN_ID ← GİRİŞ AL
        SEPETTEN_SİL(ÜRÜN_ID)
    EĞER SEÇİM = 2 İSE
        YAZ "Adet değiştirilecek ürün ID'sini giriniz:"
        ÜRÜN_ID ← GİRİŞ AL
        YAZ "Yeni adet:"
        ADET ← GİRİŞ AL
        SEPET_ADET_GÜNCELLE(ÜRÜN_ID, ADET)
    EĞER SEÇİM = 3 İSE
        YAZ "İndirim kodunu giriniz:"
        KOD ← GİRİŞ AL
        EĞER KOD = "INDIRIM10" İSE
            SEPET_TOPLAMI ← SEPET_TOPLAMI * 0.9
            YAZ "10% indirim uygulandı."
        DEĞİLSE
            YAZ "Geçersiz indirim kodu."
        SON
    SON
TEKRARLA SEÇİM ≠ 4 OLDUĞU SÜRECE

// 5. Minimum Tutar Kontrolü
EĞER SEPET_TOPLAMI < 50 İSE
    YAZ "Ödeme için sepet toplamı en az 50 TL olmalıdır."
    GİT SEPET_EKRANI
SON

// 6. Kargo Ücreti Hesaplama
EĞER SEPET_TOPLAMI ≥ 200 İSE
    KARGO ← 0
    YAZ "Kargo ücretsiz!"
DEĞİLSE
    KARGO ← 29.90
    YAZ "Kargo ücreti: " + KARGO + " TL"
SON

GENEL_TOPLAM ← SEPET_TOPLAMI + KARGO

// 7. Ödeme Yöntemi Seçimi
YAZ "Ödeme Yöntemi Seçiniz: 1-Kredi Kartı, 2-Banka Kartı, 3-Havale/EFT"
ÖDEME ← GİRİŞ AL

EĞER ÖDEME = 1 VEYA 2 İSE
    YAZ "Kart bilgilerini giriniz:"
    YAZ "Kart numarası:"
    KART_NO ← GİRİŞ AL
    YAZ "Son kullanma tarihi:"
    SKT ← GİRİŞ AL
    YAZ "CVV:"
    CVV ← GİRİŞ AL
    YAZ "Ödeme işleniyor..."
DEĞİLSE EĞER ÖDEME = 3 İSE
    YAZ "IBAN: TR00 0000 0000 0000"
    YAZ "Lütfen belirtilen hesaba havale yapınız."
DEĞİLSE
    YAZ "Geçersiz ödeme yöntemi!"
    BİTİR
SON

// 8. Sipariş Onayı
YAZ "Sipariş toplamı: " + GENEL_TOPLAM + " TL"
YAZ "Sipariş onaylanıyor..."
YAZ "Sipariş başarıyla oluşturuldu. Teşekkür ederiz!"

BİTİR
