---
layout: post
title: DNS Kayıtları(Domain Name System-Alan Adı Sistemi) - Bolum 2
#subtitle: Each post also has a subtitle
gh-repo: fatlan
gh-badge: [star, follow]
#cover-img: /assets/img/path.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [dns, domain, record]
comments: true
---
**NS(Name Server)** Kaydı = Sunucu ismi kaydıdır, yani name server’ı işaret eden kayıttır. **DNS** server’ı işaret eden kayıttır. Herhangi bir **Domain Name**(Alan Adı) için geçerli sunucuyu belirleyen kayıttır.

**Örneğin** ; **godaddy.com**’den alınmış bir alan adınız var, aşağıdaki screenshot’tan da anlaşılacağın gibi, alan adı yönetim kısmından Ad Sunucuları kısmına **Name server** adresleri belirliyorum. Diyorum ki bu alan adı için geçerli olacak **DNS** kayıtları belirlediğim **NS**’te ki **DNS Server**’dır. Ayrıca o **DNS server**’da domain için oluşturduğum kayıtlarda da **NS** kayıtları olacaktır ki **godaddy.com** tarafında isteğimize karşılık gelsin. Evet o sunucu benim gibisinden.

**Domaini register** ettiğim tafafta **NS** yönlendirmesi aşağıdaki gibi,

![Crepe](assets/img/dns-bol2/dns-do-b01.png)

**DNS** kayıtları tarafında da karşılık gelecek **NS** bilgisi,

![Crepe](assets/img/dns-bol2/dns-do-b02.png)

**SOA(Start Of Authhority)** kaydı = **Zone**’deki ilk başlangıç ve **DNS server**’ın **zone**’den sorumlu olduğunu gösteren kayıttır. Yetkili birincil **DNS** sunucunun parametlerini tutan kayıttır.

**A(Address)** kaydı = Herhangi bir **Domain Name**’yi bir **ip(IPV4)** yönlendiren kayıttır. **AAAA** is **IPV6** karşılığıdır.

**www A 1.1.1.1** herhangi bir tarayıcıdan

ya da

**ftp A 1.1.1.1** kaydı girildiğinde, **ftp.fatihaslan.tr** gibi gibi...

![Crepe](assets/img/dns-bol2/dns-do-b03.png)

**MX(Mail Exchanger)** kaydı = Herhangi bir alan adının **mail** trafiğini, söz konusu olan kullanıcı hesaplarının barındığı **mail** sunucusuna yönlediren kayıttır.

mail A 1.1.1.1

mail2 A 2.2.2.2

fatihaslan.tr MX 10 mail

fatihaslan.tr MX 20 mail2 gibi...

ya da

f**atihaslan.tr MX 0 spamgtwy.domain.com** gibi gibi...

Burda **10** ve **20** değerleri **Priority** değeridir. Yani yapınızın yoğunluğuna yada yapısına göre oluşturduğunuz **mail** sunucunuzun cevap verme önceliğidir. Yani öncelik değeri manasına gelir.

**CNAME(Canonical Name Record)** kaydı = Birden fazla **host name**’i var olan bir **A** kaydına isim olarak yönlendirilebilen kayıttır. Karmaşık yapıdaki **FQDN**’leri daha sade ve şık olması açısından **CNAME** kullanılabilir. Veya daha farklı olarak **A** kaydı var olan bir **mailci.fatlan.co**m’a **CNAME** kullanarak **mail.fatihaslan.tr**’yi o servise yönlendirebilirsiniz.

**Örneğin** ; **fatlan.tr A 2.2.2.2** var olan bir **A** kaydı.

**aslan.fatih.tr CNAME fatlan.tr** gibi... Yani bir ip adresi kullanmadan yönlendirme işlemidir.

![Crepe](assets/img/dns-bol2/dns-do-b04.png)

**PTR(Pointer record)** kaydı = **RDNS(Reverse)** kaydı olarakta bilinen ters isim kaydıdır. **ISP(İnternet Servis Sağlayıcı)** servisleri tarafından girilen bir kayıttır. Bu kaydın kullanabilmesi için **IP**’nin sabit olması gerekmekte.

**Örneğin** ; Herhangi bir **ISP**’den sabit bir **IP** aldınız ve bu **IP**’yi **mail** sunucusunda kullanacaksınız, o zaman sunucunun **Host Name**’i o **IP**’ye tanımlanmalıdır. Ve böylelikle sizin sunucunuzda **PTR** kaydını sorgulayarak gelen sunucular olursa iletişim başarılı bir şekilde gerçekleşecektir.

**1.2.168.192.in-addr.arpa PTR mailim.fatih.tr** gibi...

Ayrıca bu linki kontrol edebilirsiniz. [https://ulakbim.tubitak.gov.tr/sites/images/Ulakbim/onur_b_ters_dns_kaydi.pdf](https://ulakbim.tubitak.gov.tr/sites/images/Ulakbim/onur_b_ters_dns_kaydi.pdf)

**SPF(Sender Policy Framework)**, **TXT** kaydı = Sahip olduğunuz **Alan Adı(Domain Name)** için oluşturduğunuz **mail** sunucunuz için gönderme iznine sahip olduğunu gösteren kayıttır. **TXT** olarak **DNS** kayıtlarına eklenir ki sunucunuzun **SPAM** yapan bir sunucu olarak algılanmasının önüne geçen kayıttır. Bu kaydı çift tırnak içinde girmelisiniz.

v=spf1 a mx ip4:192.168.1.1 ~all

![Crepe](assets/img/dns-bol2/dns-do-b05.png)

**SRV (Service Locator)** kaydı = Özel bir servisin hangi **IP** üzerinden verildiğini tutan kayıt olarak, bu kayıt ile o servise yönlendirme yapılabilir. **Internel DNS** kayıtlarında **Active Directory** bu kaydı kullanarak **LDAP** ve **Kerberos** servislerine ulaşır. Dışarda da kullanılan bir özel servis için yönlendirme yapılabilir.

**Örneğin**; (hizmet.protol) **SRV** (öncelik, ağırlık ve port) (hedef) sıralamasıyla aşağıdaki gibi kayıt girilir. Tabi öncesinde **TTL** istenilen gibi ayarlanmalıdır.

**SRV** istek kayıt tablosu...

![Crepe](assets/img/dns-bol2/dns-do-b06.png)

Başka bir **SRV DNS** kayıt girdisi...

![Crepe](assets/img/dns-bol2/dns-do-b07.png)

**DKIM (DomainKeys Identified Mail)** kaydı = Sunucu tarafından şifreleme sistemi kullanılarak gönderilen her emailin barkodlanması denebilir. Yahoo tarafından ilk olarak **DomainKeys** tasarlanmıştır. **DKIM** Türkçe karşılığı ile Alan adı anahtarıyla e-posta kimlik doğrulaması; **phishing spoofing** (sahtekarlık, kimlik hırsızlığı) gibi kötü niyetli aksiyonların ve email sahtekarlığının önüne geçmek için kullanılan domain adı ile emaili eşleştirme yöntemdir. Bu yöntemle kişi veya organizasyon emailin gerçekten kendisi tarafından gönderildiğini doğrulatır.

Email gönderen kişi veya organizasyon, kendisi tarafından gönderilen her emaili dijital (**kriptografik**) olarak imzalar, böylelikle emailin kendisi tarafından gönderildiğini teyit eder. Bu imzanın doğrulanmasında **Private** ve **Public** olmak üzere 2 anahtar kullanılır. **Private key** gönderilen emaili imzalamak için kullanılır ve gizli olması gerekir. **Public key** sadece bu imzayı doğrulamak için kullanılacağı için açık olarak kullanılabilir. Emaili gönderen sunucu tarafında bir **Private key** tanımlanır. Bu anahtar, her gönderilen email başlığına (**Internet Headers**) eklenir. **Public key DNS** sunucuya **TXT** kaydı olarak eklenir. Alıcıya gelen email deki **DKIM** imzası DNS kaydı ile kontrol edilir. Bu eşleşmenin sağlanması durumunda gönderen güvenli olarak sını andırılır.

![Crepe](assets/img/dns-bol2/dns-do-b08.png)

**NOT : DNS KAYITLARININ DOĞRU ÇALIŞMASI YANİ KARŞILIK GELEN İP Yİ ÇÖZEBİLMESİ İÇİN; PANELDEN PANELE DEĞİŞİKLİK GÖSTEREBİLİR (Örnekler DirectAdmin üzerinden gösterilmiştir), GİRİLEN KAYDI SONLANDIRMA ANLAMINA GELEN ‘.’ nokta İŞERETİNİ KOYMAYI UNUTMAYIN**

Daha Sonrasında girilen **DNS** kayıtlarını localden ‘**nslookup**‘ komutu ile sorgulayabilirsiniz.

![Crepe](assets/img/dns-bol2/dns-do-b09.png)

Online olarakta;

[https://www.whatsmydns.net/](https://www.whatsmydns.net/) <br>
[http://mxtoolbox.com/](http://mxtoolbox.com/) <br>
[http://www.intodns.com/](http://www.intodns.com/) servislerini kullanabilirsiniz.

**Extrenal DNS** örnek kayıt girdileri,

![Crepe](assets/img/dns-bol2/dns-do-b10.png)

**Internal DNS** örnek kayıt girdileri,

![Crepe](assets/img/dns-bol2/dns-do-b11.png)

