---
layout: post
title: "Chrome Extension Nasıl Yapılır?"
date: 2017-05-25 11:13:02 +0300
comments: true
categories: [Chrome Extension]
---

Yazıma Chrome Extension nasıl yapılır sorusundan önce, Chrome Extension nedir? sorusu ile başlamak istiyorum. Chrome Extension; Chrome Tarayıcısı'nın işlevselliğini değiştirebilen ve geliştirebilen küçük programlardır. Extensionlar; bir uygulamaya veya siteye (gmail, facebook, twitter v.b) bağlı olabileceği gibi sadece kendine ait işlevi de olabilir. Chrome Extension'ların sağladığı en büyük yararlardan biri de bir siteye girmeden bazı işlemlerin daha az işlemle o extension üzerinden yapılabilmesidir. Örnek olarak verecek olursak; gmail hesabimıza girmeden gmail'e ait bir extension kurarak gelen maillerimizi browser'ımızda sağ üst köşede görebiliriz.

Bu kadar genel bilgiden sonra biraz da teknik konulara girelim. Chrome Extension'lar genelde HTML, JavaScript ve CSS kodları kullanılarak yazılmaktadır. Bir Chrome Extension yazarken en önemli kaynak; Google’ın Chrome için resmi dökümantasyonlarını paylaştığı <em>http://developer.chrome.com/extensions/getstarted.html</em> linkidir. Bir Chrome Extension yazarken herhangi bir IDE'ye ihtiyaç yoktur. Notepad, Sublime gibi text editörleri işinizi görecektir. 

Bir Chrome Extension geliştirirken yapılması gereken ilk şey; gerekli yetkileri, isimlendirmeleri ve tanımlamaları yaptığımız manifest.json dosyası oluşturmaktır. Burada verilen izinler ne kadar fazlaysa extension o kadar fonksiyoneldir. İzinler ile ilgili daha fazla bilgi almak için <em>https://developer.chrome.com/extensions/declare_permissions</em> burayı inceleyebilirsiniz. manifest.json dosyasında ayrıca projede kullandığımız dosyaları da burda tanımlamak zorundayız. Projeye ait iconları da burada tanımlamaktayız. Projeye iconları "16", "48","128" pixel boyutlarında ekleyebiliyoruz. Ayrıca extension'a ait popup kısmını da tanımlarız. Bu popupları browser action veya page action olarak tanımlayabiliriz. Browser action ve page aciton arasındaki fark ise browser action olarak tanımlanan bir popup sayfaya bağlı değildir. Ama page action ise iconu active ve inactive etmemize bağlıdır.

Background Script asıl işin yapıldığı kısımdır. Burada extension'da kullanacağımız ana kütüphaneleri ve geliştireceğimiz JavaScript dosyalarını belirliyoruz. Content Script web sayfaları bağlamında çalışan JavaScript dosyalarıdır. Document Object Model (DOM) kullanarak tarayıcı web sayfalarının ayrıntılarını okuyabilir veya değiştirebilir.

Önemli bir nokta vardır; bakground script ile content script birleştirilemez. Mesela bir api sorgusu yapıp onu bir sayfaya yazdırmak istersek api sorgusunu Background Script'de, sayfaya yazdırma kısmını ise Content Script'de yapmak gerekir. Bu aradaki bağlantıyı ise Chrome'un Message Listener olayı ile yapmaktayız. Ayrıca Message Listener ile mesajın yanında Background Script'de çektiğimiz veriyi Content Script'e gönderebiliriz. Bir dosyadaki veriyi diğer dosyalarda kullanabilmek için ise storageları da kullanabiliriz. Eldeki veriyi bir storage'a koyup diğer dosyanın içinde bu veriyi çekebiliriz.


Bir extension yazarken sayfanın Console kısmını çok iyi kullanmalıyız. Her yapılan işlemi Console'a yazdırarak yapmalıyız. Çünkü bu şekilde yaparsak yapılan hataları çok daha rahat görebiliriz. Ayrıca extension'da Content Script ile Background Script'in console'ları da farklı yerdedir. Content Script'in Console'u bulunduğumuz sayfada açılabilmektedir. Ama Bakckground Script'in Console'u ise extension'ı yüklediğimiz yerde inspect views: background page tıklanarak gösterilebilir.

Ayrıca popup kısmı da tanımlayabiliriz. Popup'u manifest dosyasındaki browser action'un içinde tanımlamalıyız. Popup iconun üzerine gelindiğinde veya icona tıklandığında küçük bir popup penceresi çıkmasını sağlar. Bu pencere gerek işlem yapılabilir gerekse bilgi vermek için olabilir.

Son olarak oluşturduğunuz eklentiyi test ya da debug etmek için şu işlemleri yapın. Chrome tarayıcınızın sağ üstteki menüsüne gelerek Araçlar > Uzantılar ‘ı seçin. Açılacak sayfadan sağ üstteki “Geliştirici Mod”unu seçerek aktif edin. Sol tarafta açılacak “Paketlenmemiş uzantıyı yükle…” butonundan eklentinizin klasörünü seçerek tarayıcınıza yükleyebilir ve burada test, debug işlemlerinizi gerçekleştirebilirsiniz.

