🌀 cURL Kullanım Mantığı

cURL, tarayıcının yaptığı işleri komut satırından taklit etmemizi sağlar.
Yani bir siteye GET isteği, POST isteği, kimlik doğrulama, cookie gönderme vs. gibi her şey cURL ile yapılabilir.

🔹 1. Temel İstekler

curl inlanefreight.com → Basit GET isteği

curl -O file.html → Dosya indir

curl -k https://... → SSL sertifikası geçersiz olsa bile devam et

🔹 2. Yanıtı Daha Detaylı Görmek

curl -I URL → Sadece başlıkları gör

curl -i URL → Başlık + içerik

curl -v URL → İstek/yanıt ayrıntılarını debug et

🔹 3. Başlık (Header) Manipülasyonu

-A 'Mozilla/5.0' → User-Agent değiştir (tarayıcı gibi görünmek)

-H 'Authorization: Basic ...' → Özel HTTP başlığı eklemek

-b 'PHPSESSID=...' → Cookie eklemek

🔹 4. Kimlik Doğrulama

curl -u admin:admin http://... → Basic Auth kullanıcı/şifre ile

curl http://admin:admin@host/ → URL içine gömülü kullanıcı/şifre

🔹 5. GET & POST Parametreleri

curl 'http://host/search.php?search=le' → GET parametresiyle istek

curl -X POST -d 'username=admin&password=admin' http://host/ → POST form verisi

curl -X POST -d '{"search":"london"}' -H 'Content-Type: application/json' http://host/ → JSON veriyle POST

🔹 6. API ile Çalışmak

curl http://host/api.php/city/london → Belirli kaydı oku

curl -X POST ... → Yeni kayıt ekle

curl -X PUT ... → Var olanı güncelle

curl -X DELETE ... → Sil

🖥️ Tarayıcı Geliştirme Araçları

cURL’den önce “tarayıcı ne yapıyor” diye gözlemlemek için kullanılır:

CTRL+SHIFT+I → Dev Tools aç

CTRL+SHIFT+E → Network sekmesi (arka planda hangi istekler gidiyor?)

CTRL+SHIFT+K → Konsol sekmesi (fetch vs. görmek için)

Örneğin:

Sen bir arama kutusuna yazıyorsun → tarayıcı search.php?search=... isteği gönderiyor.

Bunu Network’te yakalıyorsun → Sonra aynı isteği cURL ile terminalden kendin yapıyorsun.

🔑 Özet

Tarayıcı: İstekleri otomatik yapar, biz “Network” ile gizlice izleriz.

cURL: Aynı istekleri komut satırından manuel yapmamızı sağlar.

Mantık: Tarayıcı → İstekleri öğren → cURL ile taklit et → Parametreleri değiştir → Gizli bilgi (ör. flag) çek.
