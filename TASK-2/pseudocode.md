BAŞLA

// Kullanıcı girişi kontrolü
EĞER kullanıcı_girişi = DOĞRU İSE
    YAZ "Hoş geldiniz!"
DEĞİLSE
    YAZ "Lütfen giriş yapınız."
    GİRİŞ YAP()
    EĞER giriş_başarılı = YANLIŞ İSE
        YAZ "Giriş başarısız, sistemden çıkılıyor."
        BİTİR
    SON
SON

// Ürün kategorilerinde gezinme (döngü)
TEKRARLA
    YAZ "Lütfen bir kategori seçin:"
    kategori ← KATEGORİ_SEÇ()
    ürün_listesi ← KATEGORİDEKİ_ÜRÜNLERİ_GÖSTER(kategori)
    ürün ← ÜRÜN_SEÇ(ürün_listesi)

    // Stok kontrolü (koşul)
    EĞER ürün.stok > 0 İSE
        YAZ "Ürün sepete eklendi."
        SEPETE_EKLE(ürün)
    DEĞİLSE
        YAZ "Seçilen ürün stokta yok."
    SON

    YAZ "Başka ürün eklemek ister misiniz? (E/H)"
    cevap ← GİRİŞ_AL()
SONA KADAR TEKRARLA cevap = "E"

// Sepeti görüntüleme ve düzenleme (döngü)
TEKRARLA
    SEPETİ_GÖSTER()
    YAZ "Sepeti düzenlemek ister misiniz? (E/H)"
    düzenle ← GİRİŞ_AL()
    EĞER düzenle = "E" İSE
        SEPET_DÜZENLE()
    SON
SONA KADAR TEKRARLA düzenle = "E"

// İndirim kodu kontrolü
YAZ "İndirim kodunuz var mı? (E/H)"
cevap ← GİRİŞ_AL()
EĞER cevap = "E" İSE
    kod ← GİRİŞ_AL("İndirim kodunu giriniz: ")
    EĞER KOD_GEÇERLİ(kod) İSE
        İNDİRİM_UYGULA(kod)
        YAZ "İndirim uygulandı."
    DEĞİLSE
        YAZ "Geçersiz kod."
    SON
SON

// Minimum 50 TL kontrolü
EĞER SEPET_TOPLAMI < 50 İSE
    YAZ "Minimum sipariş tutarı 50 TL olmalıdır."
    BİTİR
SON

// Kargo ücreti hesaplama
EĞER SEPET_TOPLAMI ≥ 200 İSE
    kargo_ücreti ← 0
    YAZ "Kargo ücretsiz."
DEĞİLSE
    kargo_ücreti ← 30
    YAZ "Kargo ücreti 30 TL eklendi."
SON

// Ödeme yöntemi seçimi
YAZ "Ödeme yöntemi seçiniz: 1-Kredi Kartı, 2-Havale, 3-Kapıda Ödeme"
ödeme ← GİRİŞ_AL()

EĞER ödeme = 1 İSE
    YAZ "Kredi kartı bilgilerini giriniz."
    ÖDEME_YAP("Kredi Kartı")
EĞER ödeme = 2 İSE
    YAZ "Havale bilgileri gösteriliyor."
    ÖDEME_YAP("Havale")
EĞER ödeme = 3 İSE
    YAZ "Kapıda ödeme seçildi."
    ÖDEME_YAP("Kapıda Ödeme")
SON

// Sipariş onayı
EĞER ÖDEME_BAŞARILI İSE
    YAZ "Sipariş başarıyla oluşturuldu. Teşekkür ederiz!"
DEĞİLSE
    YAZ "Ödeme başarısız. Lütfen tekrar deneyiniz."
SON

BİTİR
