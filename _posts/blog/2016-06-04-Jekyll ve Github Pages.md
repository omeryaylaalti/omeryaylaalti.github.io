---
layout: post
title: "Jekyll ve Github Pages ile Blog Oluşturma"
modified:
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2016-06-04T15:20:40-04:00
---

## Giriş

Merhabalar, bu yazımda Jekyll ve Github Pages kullanarak web sitesi oluşturmayı anlatcağım. Öncelikle Jekyll ve Github Pages nedir ? Bu kavramlardan bahsedelim.

**Jekyll Nedir?**

Jekyll , altyapı olarak ruby ve python kullanan statik site oluşturucusudur. Markdown formatında yazıdığınız yazıları otomatik olarak statik web sitesine çevirir.
Bir sonraki yazımda Markdown formatından bahsedeceğim. 

Kullanımına gelecek olursak , 

Jekyll kullanımı son derece basit bir uygulama. Veritabanı olmadığı için çok performanslı. Jekyll kullanırken veritabanı yönetimi veya hosting (Github Pages sayesinde) gibi şeylerle uğraşmanız gerekmiyor. Yazılarınızı Markdown gibi bir formatta yazın, jekyll build komutunu girin ve Jekyll size sitenizin statik HTML çıktısını versin. Bu kadar basit.

**Github Pages Nedir?**

Github, açık kaynak prensibine sahip , projeleriniz için web sayfası oluşturabileceğiniz ve projenizi tanıtabileceğiniz bir hosting hizmeti veriyor. Kullanımı ücretsizdir. Birçok açık kaynaklı proje ve yazılımcı, projelerini ve kişisel sayfalarını Github pages üzerinde tutuyor.

Github üzerinde sayfa açmak için birkaç çeşit yol var ancak ben size en kolay yoldan kişisel sayfanızı nasıl oluşturabileceğinizi anlatacağım. İşlem son derece basit.

Öncelikle, Github’a kayıtlı değilseniz kayıt olun ve daha sonra Repositories sekmesine gelip New butonuna tıklayın. Repository adı githubHesapAdınız.github.io formatında olmalı. Bu şekilde Github, bu reponun sizin kişisel sayfanız olacağını anlıyor.

**Jekyll ile Site Oluşturma**

Jekyll çalıştırmak için bazı uygulamalara ihtiyacımız var. Öncelikle Ruby ve Ruby Gem'in kurulu olması gerekmektedir.

~~~
sudo gem install jekyll
~~~

yazarak Jekyll uygulamasını kurun. Aynı şekilde ruby ve ruby gem i de terminalden kurmanız gerekmektedir.

Gerekli altyapı kurduktan sonra ilk sitemizi oluşturalım. 

~~~
jekyll new myBlog
cd myBlog
jekyll serve
~~~

komutlarını girerek Jekyll 'i çalıştırın.Eğer herşey doğruysa http://localhost:4000 adresine girdiğinizde Jekyll ile karşılaşırsınız.

**Jekyll için Tema İndirme**

<http://jekyllthemes.org/> adresinde açık kaynaklı Jekyll temaları bulabilirsiniz. İndirip oluşturduğunuz klasörün içeriğiyle değiştirdiğiniz takdirde sitenizin teması otomatik olarak değişecektir.


**Dikkat Edilmesi Gerekenler**

Öncelikle _site klasörü , Jekyll tarafından generate edilen statik dosyaların tutulduğu klasördür. Root olarak düşünün. Bunun içerisinde herhangi bir değişiklik yapmamalısınız.

_posts klasörü Markdown formatında yazdığınız yazıların tutulduğu klasör. Markdown syntaxı çok kolay ve yüksek ihtimalle daha önce farkında olmadan kullandınız. Popüler siteler (stackoverflow, Reddit, bazı forumlar gibi) Markdown formatını kullanıyor. Detaylı bilgiye <http://daringfireball.net/projects/markdown/syntax> adresinden ulaşabilirsiniz.

config.yml dosyası ise sitenizin özelliklerini barındıran dosyadır. Burada yaptığınız değişiklikler tüm ortamları etkiler. Size ait gerçek bilgiler girmeniz faydanızadır.

 
**Deployment İşlemi**

config.yml içerisindeki url değerini http://githubHesapAdınız.github.io olarak değiştirin. (Çünkü assetler artık localhost üzerinden sunulmayacak) Daha sonra:

~~~
  cd site_klasoru
    git init
    git remote add origin https://github.com/githubHesapAdınız/githubHesapAdınız.github.io.git
    git add -A
    git commit -m "Initial deployment"
    git push origin master
~~~
	
komutlarını girerek dosyaları remote reponuza pushlayın. Birkaç dakika içerisinde http://githubHesapAdınız.github.io adresine giriş yapın ve siteniz sizi karşılıyor olacaktır.

Yazımı burada sonlandırıyorum. Okuduğunuz için teşekkürler. 

Saygılarımla.
