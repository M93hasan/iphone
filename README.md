# iConnect A8 Research

Amaç: iConnect A8 cihazının haberleşme/protokol yapısını belgelemek ve sonrasında iPhone için bağımsız bir istemci geliştirmek.

## Plan
1. A8'i ADB üzerinden tanımla.
2. Sistem, Bluetooth ve paket bilgilerini çıkar.
3. SAMSIM ile yapılan arama/SMS işlemlerinde logcat kaydı al.
4. Komut/cevap akışını docs/protocol.md içinde belgele.
5. Gerekirse yalnızca test amaçlı Android yardımcı uygulaması geliştir.
6. Asıl iPhone istemcisini Swift ile geliştir ve Mac/Xcode üzerinde gerçek cihazda test et.

## Güvenlik
Şifre, token, SIM PIN/PUK veya kişisel anahtarlar repoya eklenmemelidir.
