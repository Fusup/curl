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


📘 Gelişmiş cURL Kullanım Kılavuzu
🔹 1. Temel GET İstekleri
# Basit GET isteği
curl https://example.com

# Yanıtın sadece HTTP başlıklarını al
curl -I https://example.com

# Başlık + içerik birlikte
curl -i https://example.com

# İstek/yanıt sürecini detaylı gör (debug için)
curl -v https://example.com

🔹 2. Yanıtı Dosyaya Kaydetmek
# Orijinal dosya adını koruyarak indir
curl -O https://example.com/index.html

# Dosyayı farklı adla kaydet
curl -o sayfa.html https://example.com/index.html

🔹 3. Kimlik Doğrulama (Authentication)
# Kullanıcı adı/şifre ile giriş (Basic Auth)
curl -u admin:admin http://<IP>:<PORT>/

# URL’de kullanıcı adı/şifre
curl http://admin:admin@<IP>:<PORT>/

# Base64 Authorization header ekleyerek
curl -H "Authorization: Basic YWRtaW46YWRtaW4=" http://<IP>:<PORT>/


👉 Örnek:

curl -v -H "Authorization: Basic YWRtaW46YWRtaW4=" http://94.237.54.192:51937/

🔹 4. GET Parametreleri
# Basit GET parametresi
curl "http://<IP>:<PORT>/search.php?search=london"

# Birden fazla parametre
curl "http://<IP>:<PORT>/search.php?city=london&country=uk"


👉 Örnek (Flag denemesi):

curl "http://94.237.54.192:51937/search.php?search=flag" \
-H "Authorization: Basic YWRtaW46YWRtaW4="

🔹 5. POST İstekleri
# Form datası gönderme
curl -X POST -d "username=admin&password=admin" http://<IP>:<PORT>/login.php

# JSON data ile POST isteği
curl -X POST -d '{"search":"london"}' \
-H "Content-Type: application/json" \
http://<IP>:<PORT>/search.php


👉 Örnek (API’ye veri ekleme):

curl -X POST http://<IP>:<PORT>/api.php/city/ \
-d '{"city_name":"HTB_City", "country_name":"HTB"}' \
-H "Content-Type: application/json"

🔹 6. Çerez Kullanımı (Cookies)
# Çerez gönderme
curl -b "PHPSESSID=xyz" http://<IP>:<PORT>/

# Çerezleri dosyaya kaydet
curl -c cookies.txt http://<IP>:<PORT>/

# Kaydedilen çerezleri kullanarak tekrar istek
curl -b cookies.txt http://<IP>:<PORT>/

🔹 7. Proxy Kullanımı
# Proxy üzerinden bağlan
curl -x http://127.0.0.1:8080 http://example.com

# Proxy kullanıcı adı/şifre ile
curl --proxy-user user:pass -x http://127.0.0.1:8080 http://example.com


👉 Sızma testi senaryosu:
BurpSuite’i 127.0.0.1:8080 portuna aç → tüm cURL trafiğini buraya yönlendir → isteği yakala ve değiştir.

🔹 8. Dosya Yükleme (Upload)
# Tek dosya yükle
curl -F "file=@local.txt" http://<IP>:<PORT>/upload.php

# Birden fazla dosya yükle
curl -F "file1=@a.txt" -F "file2=@b.txt" http://<IP>:<PORT>/upload.php

🔹 9. İleri Seviye Örnekler
🔍 9.1 Header Manipülasyonu
curl -H "User-Agent: Mozilla/5.0" \
-H "X-Forwarded-For: 127.0.0.1" \
http://<IP>:<PORT>/


➡️ Bu sayede bazen WAF/IPS’leri bypass edebilirsin.

🔍 9.2 SQLi / XSS Testleri (Parametre Manipülasyonu)
curl "http://<IP>:<PORT>/search.php?search=' OR '1'='1" -v
curl "http://<IP>:<PORT>/search.php?search=<script>alert(1)</script>"

🔍 9.3 API CRUD İşlemleri
# Veri oku (GET)
curl http://<IP>:<PORT>/api.php/city/london

# Veri oluştur (POST)
curl -X POST http://<IP>:<PORT>/api.php/city/ \
-d '{"city_name":"Paris","country_name":"France"}' \
-H "Content-Type: application/json"

# Veri güncelle (PUT)
curl -X PUT http://<IP>:<PORT>/api.php/city/london \
-d '{"city_name":"NewLondon","country_name":"UK"}' \
-H "Content-Type: application/json"

# Veri sil (DELETE)
curl -X DELETE http://<IP>:<PORT>/api.php/city/NewLondon


⚡ Özet:
cURL = Tarayıcı gibi istek gönderebilen, ama bize parametreleri değiştirme ve manipüle etme gücü veren bir araçtır.
➡️ Web pentestlerinde (SQLi, XSS, Auth bypass, brute-force, API testleri) kullanılır.
