Başla

Eğer Sistem_Aktif_mi? ise
    Sürekli olarak:  // Sensör okuma döngüsü
        Sensör_Verilerini_Oku()

        // HAREKET SENSÖRÜ KONTROLÜ
        Eğer Hareket_Sensörü_Algıladıysa:
            Eğer Ev_Sahibi_Evde_mi? ise
                Alarmı_Sıfırla()
            Değilse
                Alarm_Seviyesini_Belirle("hareket")  // 1-düşük, 2-orta, 3-yüksek
                Kamera_Aktivasyonu()
                Bildirim_Gönder(Alarm_Seviyesi)

        // KAPI / PENCERE SENSÖRÜ KONTROLÜ
        Eğer Kapı_Pencere_Sensörü_Algıladıysa:
            Eğer Ev_Sahibi_Evde_mi? ise
                Alarmı_Sıfırla()
            Değilse
                Alarm_Seviyesini_Belirle("kapı/pencere")  // 1-düşük, 2-orta, 3-yüksek
                Kamera_Aktivasyonu()
                Bildirim_Gönder(Alarm_Seviyesi)

        // ALARM SEVİYESİNE GÖRE AKSİYONLAR
        Eğer Alarm_Seviyesi == 1:  // Düşük
            Sadece App_Bildirim_Gönder()
        
        Eğer Alarm_Seviyesi == 2:  // Orta
            App_Bildirim_Gönder()
            SMS_Gönder()
        
        Eğer Alarm_Seviyesi == 3:  // Yüksek
            App_Bildirim_Gönder()
            SMS_Gönder()
            Email_Gönder()
            Kamera_Kaydı_Başlat()

        Bekle(Belirli_Zaman)  // Örn. 5 saniye
        Tekrar_Kontrol_Et()

Değilse
    Sistem_Devre_Dışı()

Bitir
