# ADB Discovery

A8 bilgisayara bağlandığında ilk keşif için:

```bash
adb devices
adb shell getprop
adb shell pm list packages
adb shell dumpsys bluetooth_manager
adb logcat
```

## Test yöntemi
1. Logcat'i temizle.
2. Kaydı başlat.
3. A8/SAMSIM üzerinden yalnızca tek bir işlem yap (ör. arama başlat).
4. Kaydı durdur ve zaman damgasını not et.
5. Aynı işlemi cevaplama, kapatma, SMS ve durum sorguları için ayrı ayrı tekrarla.

Amaç, her kullanıcı işlemini sistemde oluşan olaylarla eşleştirmek ve platformdan bağımsız protokol tablosu çıkarmaktır.
