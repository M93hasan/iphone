# iConnect A8 / SamSIM — Güncel Çalışma Durumu

Son güncelleme: 2026-10-03

## Amaç

iConnect A8 cihazının iPhone ile kullanımında görülen yaklaşık 1–2 saniyelik konuşma gecikmesini ve düşük/derin ses kalitesini teşhis etmek. Hedef; gecikmenin A8 cihazı, Android ses yönlendirmesi, Bluetooth/SCO katmanı veya SamSIM uygulaması taraflarından hangisinde oluştuğunu ayırmak ve sonrasında daha iyi bir istemci geliştirmek.

## Cihaz

- Model: iConnect A8
- Android model adı: A8
- Ürün adı: SAMSIM
- SoC / platform: MediaTek MT6761
- Android: 9 (API 28)
- Build: Samsim_v01_20250117
- ADB cihaz seri numarası: 19A285B8E533
- ADB bağlantısı doğrulandı.

## BLE / GATT bulguları

A8 üzerinde aşağıdaki servis ve karakteristikler doğrulandı:

- FFE1
  - FF11
  - FF16
- FFE2
  - FF12
- FFE3
  - FF13
  - FF17
- FFE4
  - FF14
  - FF18

FF11, FF12, FF13 ve FF14 üzerinde CCCD bulunduğu görüldü.

Android Bluetooth dump içinde GATT server olarak `com.smartdetonator` görünüyor.

Daha önce FF13 Notify üzerinden `AT#...` ile başlayan uzun JSON-benzeri veri akışları görüldü.

## Bluetooth bulguları

- Bluetooth açık ve servis çalışıyor.
- `A2dpOffloadEnabled: false`
- `persist.bluetooth.a2dp_offload.disabled=true`
- İncelenen boşta durum kaydında Bluetooth SCO aktif değildi.
- Aynı kayıtta gerçek telefon görüşmesi aktif değildi.
- Bu nedenle SCO davranışı kesin olarak ancak arama açıkken alınacak dump ile değerlendirilecek.

## Audio bulguları

- `USAGE_VOICE_COMMUNICATION` kullanan bir ses akışı görüldü.
- AudioFlinger içinde 8000 Hz mono konuşma akışı görüldü.
- Ses yolu hoparlörden earpiece yönüne geçmiş durumda.
- Deep-buffer çıkışı mevcut.
- İncelenen örnek kayıtta deep-buffer üzerinden veri yazımı görülmediği için 1–2 saniyelik gecikmenin doğrudan deep-buffer kaynaklı olduğu henüz söylenemez.
- Şu ana kadar alınan kayıtlar arama aktif değilken alındı; kesin teşhis için arama sırasında kayıt şart.

## Pazartesi yapılacak kritik test

1. A8 USB/ADB ile bilgisayara bağlı olacak.
2. SamSIM ile iPhone bağlantısı aktif olacak.
3. Logcat temizlenecek:

```bat
adb shell logcat -c
```

4. Gerçek telefon görüşmesi başlatılacak ve **arama devam ederken** aşağıdaki komutlar çalıştırılacak:

```bat
adb shell dumpsys audio > "%USERPROFILE%\Desktop\call_audio.txt"
adb shell dumpsys media.audio_flinger > "%USERPROFILE%\Desktop\call_audioflinger.txt"
adb shell logcat -d > "%USERPROFILE%\Desktop\call_log.txt"
```

5. Görüşme kapatıldıktan sonra şu üç dosya analiz edilecek:

- `call_audio.txt`
- `call_audioflinger.txt`
- `call_log.txt`

## Analizde bakılacak noktalar

- Görüşme sırasında Bluetooth SCO gerçekten açılıyor mu?
- Audio route hangi cihaza gidiyor?
- 8 kHz / 16 kHz konuşma yolu nasıl kuruluyor?
- SamSIM kendi AudioTrack/AudioRecord zincirini mi kullanıyor?
- Buffer boyutları ve gecikmeye işaret eden kuyruklar var mı?
- Android telefon çağrısı ile uygulama içi ses yönlendirmesi arasında geçiş gecikmesi var mı?
- Gecikmenin A8, Android, Bluetooth veya iPhone uygulaması tarafında olup olmadığı.

## Not

Repo içine SIM PIN/PUK, token, parola veya kişisel anahtar yazılmamalıdır. Çalışma notlarında gereksiz kişisel telekom bilgileri tutulmamalıdır.
