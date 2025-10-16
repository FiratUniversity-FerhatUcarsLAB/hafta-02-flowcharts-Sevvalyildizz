Başla
  Ögrenci Girişi:
    ÖğrenciNo, Sifre ← Kullanıcıdan al
    Eğer (ÖğrenciNo ve Sifre doğru değilse) 
        Hata mesajı göster ve çık
    Sonra
    
  Ders Listesini Görüntüle:
    TümDersler ← Veritabanından al
    Her ders için:
        DersBilgilerini göster
    
  Ders Kayıt İşlemleri:
    KayıtTamam ← Hayır
    ToplamKredi ← 0
    
    Döngü (KayıtTamam = Hayır):
        SeçilenDers ← Kullanıcıdan al
        
        Kontroller:
          Eğer (SeçilenDers kontenjanı dolu ise)
              Uyarı: "Kontenjan dolu"
              Devam et
          Eğer (SeçilenDers'in ön koşul dersleri alınmamış ise)
              Uyarı: "Ön koşul dersleri tamamlanmamış"
              Devam et
          Eğer (SeçilenDers zaman çakışıyor ise)
              Uyarı: "Zaman çakışması var"
              Devam et
          Eğer (ToplamKredi + SeçilenDers.Kredi > 35)
              Uyarı: "Kredi limiti aşılıyor"
              Devam et
          Eğer (Öğrencinin GPA < 2.5 ve danışman onayı gerekli ise)
              DanışmanOnayı ← Kullanıcıdan al
              Eğer (DanışmanOnayı verilmedi)
                  Devam et
          
        Ders ekle:
          Öğrencinin kayıt listesine ekle
          ToplamKredi ← ToplamKredi + SeçilenDers.Kredi
          
        Kullanıcıya sor:
          "Başka ders eklemek istiyor musunuz?" 
          Eğer Hayır ise KayıtTamam ← Evet

  Kayıt Özeti Göster:
    Tüm seçilen dersleri ve toplam krediyi göster
    Onay al:
      Eğer onaylandı ise
          Kayıt başarılı mesajı göster
      Değilse
          Kayıt iptal mesajı göster

Bitir
