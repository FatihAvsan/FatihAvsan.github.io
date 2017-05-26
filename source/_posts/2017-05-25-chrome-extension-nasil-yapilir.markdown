---
layout: post
title: "Chrome Extension Uygulaması Nasıl Geliştirilir?"
date: 2017-05-25 11:13:02 +0300
comments: true
categories: [Chrome Extension, Fatih Avsan, Javascript, Chrome Developer]
---

###1. Chrome Extension nedir?

Yazıma "Chrome Extension Uygulaması Nasıl Geliştirilir?" sorusundan önce, "Chrome Extension nedir?" sorusu ile başlamak istiyorum. Chrome Extension; Chrome Tarayıcısı'nın işlevselliğini değiştirebilen ve geliştirebilen küçük programlardır. Extensionlar; bir uygulamaya veya siteye _**(gmail, facebook, twitter v.b)**_ bağlı olabileceği gibi sadece kendine ait işlevi de olabilir. Chrome Extension'ların sağladığı en büyük yararlardan biri de bir siteye girmeden bazı işlemlerin daha az işlemle o extension üzerinden yapılabilmesidir. Örnek olarak verecek olursak; gmail hesabımıza girmeden gmail'e ait bir extension kurarak gelen maillerimizi browser'ımızda sağ üst köşede görebiliriz.

![extension](https://camo.githubusercontent.com/8aaff50592aae3fdf73978ae0ad48e9bf7861904/68747470733a2f2f7261772e6769746875622e636f6d2f6d61747462616e676f2f7265706c6163656d656e742d6368726f6d652d657874656e73696f6e2d69636f6e732f6d61737465722f707265766965772e706e67)

Bu kadar genel bilgiden sonra biraz da teknik konulara girelim. Chrome Extension'lar genelde _**HTML, JavaScript ve CSS**_ kodları kullanılarak yazılmaktadır. Bir Chrome Extension yazarken; Google’ın Chrome için resmi dökümantasyonlarını paylaştığı [developer chrome](http://developer.chrome.com/extensions/getstarted.html) ziyaret edilebilir. Bir Chrome Extension yazarken herhangi bir IDE'ye ihtiyaç yoktur. Notepad, Sublime gibi text editörleri işinizi görecektir. 
<!--more-->
---

###2.1. manifest.json

Bir Chrome Extension geliştirirken yapılması gereken ilk şey; gerekli yetkileri, isimlendirmeleri ve tanımlamaları yaptığımız **manifest.json** dosyası oluşturmaktır. Burada verilen izinler ne kadar fazlaysa extension o kadar fonksiyoneldir. İzinler ile ilgili daha fazla bilgi almak için [permissions](https://developer.chrome.com/extensions/declare_permissions) inceleyebilirsiniz. **manifest.json** dosyasında ayrıca projede kullandığımız dosyaları da burada tanımlamak zorundayız. Projeye ait iconları da burada tanımlamaktayız. Projeye iconları **"16", "48","128" pixel** boyutlarında ekleyebiliyoruz. Ayrıca extension'a ait popup kısmını da tanımlarız. Aşağıda manifest dosyasının içeriğine ait bir örnek mevcuttur.    

```
{
  "name": "Extension Name",
  "version": "Extension Verison Number",
  "description": "Extension Description",
  "manifest_version": 2,

  "background":{
    "scripts": ["background dosyası tanımlanıyor"],
  },

  "permissions": [ "Gerekli izinlerin bulunduğu kısım" ],

  "content_scripts": [
    {
      "scripts": ["Sayfanın içeriğinin tanımlandığı kısım"],
    }
  ],

  //Bu kısımda popup ile ilgili tanımlamalar yapılır

  "browser_action": {
    "default_title": "Popup Title",
    "default_icon": "Popup Icon",
    "default_popup": "Tanımlı Popup dosyası"
  },
}
```
---

###2.2. Popup

Popup iconun üzerine gelindiğinde veya icona tıklandığında küçük bir popup penceresi çıkmasını sağlar. Bu pencere; gerek işlem yapmak gerekse bilgi vermek için olabilir. Popup'u manifest dosyasının içinde tanımlamalıyız. Bu popupları **[Browser Action](https://developer.chrome.com/extensions/browserAction)** veya **[Page Action](https://developer.chrome.com/extensions/pageAction)** olarak tanımlayabiliriz. Browser Action ve Page Action arasındaki fark ise browser action olarak tanımlanan bir popup sayfaya bağlı değildir. Ama Page Action ise iconu active ve inactive etmemize bağlıdır. Aşağıda popup'a ilişkin bir örnek vardır.

![popup](https://developer.chrome.com/static/images/browser-action.png)

---

###2.3. Backgrond Script ve Content Script

**[Background Script](https://developer.chrome.com/extensions/background_pages)** asıl işin yapıldığı kısımdır. Burada extension'da kullanacağımız ana kütüphaneleri ve geliştireceğimiz JavaScript dosyalarını belirliyoruz. **[Content Script](https://developer.chrome.com/extensions/content_scripts)** web sayfaları bağlamında çalışan _JavaScript_ dosyalarıdır. _**Document Object Model (DOM)**_ kullanarak tarayıcı web sayfalarının ayrıntılarını okuyabilir veya değiştirebilir.

Önemli bir nokta vardır; Background Script ile Content Script birleştirilemez. Mesela bir api sorgusu yapıp onu bir sayfaya yazdırmak istersek api sorgusunu Background Script'de, sayfaya yazdırma kısmını ise Content Script'de yapmak gerekir. Bu aradaki bağlantıyı ise Chrome'un _Message Listener_ olayı ile yapmaktayız. Ayrıca Message Listener ile mesajın yanında Background Script'de çektiğimiz veriyi Content Script'e gönderebiliriz. Bir dosyadaki veriyi diğer dosyalarda kullanabilmek için ise [storage](https://developer.chrome.com/extensions/storage#property-sync)ları da kullanabiliriz. Eldeki veriyi bir storage'a koyup diğer dosyanın içinde bu veriyi çekebiliriz. Aşağıda Background Script ile Content Script'in haberleşmesi örnek olarak gösterilmiştir. Ayrıca aşağıdaki kodda data gönderimi de yapılmaktadır.

```
Background Script

chrome.tabs.sendMessage(activeTab.id, { data: data, greeting: 'hello' });


Content Script

chrome.runtime.onMessage.addListener(function (request, sender, sendResponse) {

    if (request.greeting == 'hello') {

      Request aktif olunca Content Script tarafında yapılan işler.

    }
});
```

Bir extension yazarken sayfanın Console kısmını çok iyi kullanmalıyız. Her yapılan işlemi Console'a yazdırarak yapmalıyız. Çünkü bu şekilde yaparsak yapılan hataları çok daha rahat görebiliriz. Ayrıca extension'da Content Script ile Background Script'in console'ları da farklı yerdedir. Content Script'in Console'u bulunduğumuz sayfada açılabilmektedir. Ama Bakckground Script'in Console'u ise extension'ı yüklediğimiz yerde inspect views: background page tıklanarak gösterilebilir. Aşağıdaki resimde Background Script'e ait Console'un nasıl açılacağı gösterilmiştir.

![console](https://i.stack.imgur.com/rSU9M.png)

---

###3. Chrome'a Yükleme

Son olarak oluşturduğunuz eklentiyi test ya da debug etmek için şu işlemleri yapın. Chrome tarayıcınızın sağ üstteki menüsüne gelerek **Settings > Extensions** seçin. Açılacak sayfadan sağ üstteki Developer Mode'unu seçerek aktif edin. Sol tarafta açılacak **"Load unpacked extension..."** butonundan eklentinizin klasörünü seçerek tarayıcınıza yükleyebilir ve burada test, debug işlemlerinizi gerçekleştirebilirsiniz.

![](https://coolkidsdesign.files.wordpress.com/2013/05/screen-shot-2013-05-12-at-4-22-57-pm-copy.png)
