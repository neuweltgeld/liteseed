Liteseed node kurduktan sonra stake işlemini yapmalısınız. Kısaca bundan bahsedip işlemleri hızlıca tamamlayacağız.
Henüz node kurmadıysanız Rues hocamın şu https://github.com/ruesandora/Liteseed şuradaki reposundan kurabilirsiniz. 
Test tokenleriniz de geldiğinde stake adımına buradan devam edebilirsiniz.

Uyarı : Öncelikle eğer her hangi bir domain ile stake ettiyseniz unstake edin. 
Unstake komutu
```
sudo docker run -v liteseed:/data edge unstake "eskidomain"
```

```success```çıktısı almalısınız.

Balance kontrolü yapın 1000 tokeniniz geri gelmiş ve Staked No yazmış olmalı.
```
sudo docker run -v liteseed:/data edge balance
```

Gelelim yeni adımlara, bir adet domaine ihtiyacınız var. Ben namecheap kullanıyorum, basit anlaşılır. Hali hazırda domaininiz varsa subdomain ekleyerek de işlemlere devam edebilirsiniz.
Domain paneline giriyoruz ve ```Manage```tıklıyoruz
<img width="1068" alt="image" src="https://github.com/neuweltgeld/liteseed/assets/101174090/d534f62c-202f-4f04-a6fe-21544fca30d1">


![image](https://github.com/neuweltgeld/liteseed/assets/101174090/2ace7c15-eaaa-4a6c-acd1-8d0acfa45ff3)

Advanced DNS sekmesine giriyoruz ve gerekli ayarlara başlıyoruz. Öncelikle 1 numaradaki gibi A record ekliyoruz. Subdomain ile kullanacaksanız host kısmına ```liteseed```ya da istediğiniz bir isim verebilirsiniz. Value olarak sunucu ip girip onaylıyoruz.

Terminale dönüp certbot kurulumu yapalım.
```
sudo apt install -y certbot nginx
```

Config yapılandırmasını başlatıyoruz. emailadresi ve domain kısmını değiştirin. Subdomain kullandıysanız subdomainle beraber yazın.

```
sudo certbot certonly --manual --preferred-challenges dns \
--email <emailadresi> \
-d <domain>
```

Komutu girdikten sonra 2 kez yes diyoruz ve size bir TXT key verecek. Bu keyi yukarıdaki resimde 2 nolu gösterdiğim gibi girmemiz gerek. Ben ```liteseed```subdomaini kullanıyorum. Ona göre ayarları şöyle;

Type : TXT record

Host : _acme-challenge.liteseed

Value: Size verilen TXT key

Onayladıktan sonra 5dk bekleyin. Sonra şu adresteki ```https://dnschecker.org/#TXT/_acme-challenge.liteseed.neuweltgeld.xyz``` domaini kendinize göre düzenleyip size verilen key dağıtılmış mı kontrol edin.
Örnek;

<img width="484" alt="image" src="https://github.com/neuweltgeld/liteseed/assets/101174090/fd81db52-d8b3-422c-84fe-2afc10f1962a">

Yeşil tikleri gördünüz, terminale dönüp enter diyoruz ve başarılı olduğuna dair çıktı alıyoruz.

Sonraki aşama ```ngix```ayarı

```
sudo nano /etc/nginx/sites-available/default
```

içeriğini komple siliyoruz. Kısa yol olarak ctrl + k basılı tutun.

Sonra şu koddaki domain adresini (subdomain kullandıysanız subdomain olacak şekilde) düzenleyin. 4 yerde adres değiştiriyorsunuz.

```
# Force redirects from HTTP to HTTPS
server {
    listen 80;
    listen [::]:80;
    server_name liteseed.neuweltgeld.xyz;

    location / {
        return 301 https://$host$request_uri;
    }
}

# Forward traffic to your node and provide SSL certificates
server {
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name liteseed.neuweltgeld.xyz;

    ssl_certificate /etc/letsencrypt/live/liteseed.neuweltgeld.xyz/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/liteseed.neuweltgeld.xyz/privkey.pem;

    location / {
        proxy_pass http://localhost:8080; # or your port you changed at 6.
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_http_version 1.1;
    }
}
```



