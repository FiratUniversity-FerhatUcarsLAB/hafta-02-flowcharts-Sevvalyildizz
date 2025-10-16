1. Başla

2. Hasta Kimlik Doğrulama:
    a. Kullanıcıdan TC_no al
    b. Eğer TC_no veritabanında varsa:
           "Kimlik doğrulama başarılı" mesajı
           Devam et
       Aksi halde:
           "Kimlik doğrulama başarısız" mesajı
           Bitir

3. İşlem Döngüsü Başlat:
    Döngü başı:
        a. Kullanıcıya işlem seçeneği sun:
            1: Randevu Al
            2: Tahlil Sonucu Gör

4. Eğer Kullanıcı İşlem = 1 (Randevu Al) ise:
    a. Doktor listesini göster
    b. Kullanıcıdan poliklinik seçimi al
    c. Seçilen polikliniğe uygun saatleri göster
    d. Kullanıcıdan randevu saati seçimi al
    e. Randevuyu veritabanına kaydet
    f. Kullanıcıya "Randevu başarılı" mesajı göster
    g. SMS gönderimi yap

5. Eğer Kullanıcı İşlem = 2 (Tahlil Sonucu Gör) ise:
    a. Veritabanında hastaya ait tahlil var mı kontrol et
        Eğer tahlil yoksa:
            "Tahlil bulunamadı" mesajı göster
        Eğer tahlil varsa:
            Sonuç hazır mı kontrol et
                Eğer sonuç hazırsa:
                    Sonuçları ekranda göster
                    PDF indir seçeneği sun
                Eğer sonuç hazır değilse:
                    "Sonuçlar henüz hazır değil" mesajı göster

6. Kullanıcıya sor:
    "Başka işlem yapmak ister misiniz? (E/H)"
    Eğer Evet ise:
        Döngü başına dön
    Eğer Hayır ise:
        Döngüyü bitir

7. Bitir
