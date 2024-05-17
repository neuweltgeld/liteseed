Liteseed node kurduktan sonra stake işlemini yapmalısınız. Kısaca bundan bahsedip işlemleri hızlıca tamamlayacağız.
Henüz node kurmadıysanız Rues hocamın şu https://github.com/ruesandora/Liteseed şuradaki reposundan kurabilirsiniz. 
Test tokenleriniz de geldiğinde stake adımına buradan devam edebilirsiniz.

Uyarı : Öncelikle eğer her hangi bir domain ile stake ettiyseniz unstake edin. 
Unstake komutu
```
sudo docker run -v liteseed:/data edge unstake "eskidomain"
```

Bir adet domaine ihtiyacınız var. Ben namecheap kullanıyorum, basit anlaşılır. Hali hazırda domaininiz varsa subdomain ekleyerek de işlemlere devam edebilirsiniz.
Domain paneline giriyoruz ve ```Manage```tıklıyoruz
<img width="1068" alt="image" src="https://github.com/neuweltgeld/liteseed/assets/101174090/d534f62c-202f-4f04-a6fe-21544fca30d1">


![image](https://github.com/neuweltgeld/liteseed/assets/101174090/2ace7c15-eaaa-4a6c-acd1-8d0acfa45ff3)

Advanced DNS sekmesine giriyoruz ve gerekli ayarlara başlıyoruz. Öncelikle 1 numaradaki gibi A record ekliyoruz. Subdomain ile kullanacaksanız ```liteseed```ya da istediğiniz bir isim verebilirsiniz. Value olarak sunucu ip girip onaylıyoruz.





