---
layout: post
title: "Launch4j Nedir?"
modified:
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2016-05-30T18:25:40-04:00
---

## Tanım

Java teknolojisi kullanılarak yapılacak olan bir konsol uygulamasında proje bitirilip export edildiğinde jar uzantılı bir dosyaya ulaşılır.

Linux ve Mac Os-x kullanıcıları konsoldan direk jar uzantılı dosya çalıştırabilirken Windows kullanıcılarının böyle bir şansı yoktur. Windows ortamında projemizi exe'ye çevirip, konsoldan çalıştırabiliriz.
Bu işi Launch4j indirip , aşağıdaki adımları takip ederek yapabilirsiniz.

İndirmek için , <https://sourceforge.net/projects/launch4j/files/launch4j-3/3.8/> 

Jar dosyayı exe haline çevirirken syntax düzeninin bozulmaması için Antlr kurulması gereklidir.Bu eklentiyi buradan 
<http://www.antlr.org/download.html> indirip kurabilirsiniz.

Lauch4j klasör yapısı resimdeki gibidir. Demo uygulamalara bakarak kendi uygulamanızı convert edebilirsiniz.

![alt text](http://localhost:4000/images/launch4j.png "Lauch4j")


Uygulama klasörünüzün içeriğine gelecek olursak , src klasörünü projeninizin bulunduğu dizindeki src klasörü ile değiştirin. Uygulama source dosyalarını buradan çekip , çalıştıracak.
lib klasörünün içerisine projenin jar dosyalarını atın. Pom.xml kullanıyorsanız da Maven paketi altındaki jarları atmalısınız.
jar dosyanızı da oluşturduğunuz yeni klasörün içine attıktan sonra build.xml 'i kendi uygulamanıza göre düzenleyip, konsoldan built.bat dosyasını çalıştırdığınızda exe dosyası otomatik olarak oluşacaktır.
 

Yazımı burada sonlandırıyorum. Okuduğunuz için teşekkürler. Bir sonraki yazımda exchange server dan mail içeriği okuma işlemlerini anlatacağım.

Saygılarımla.
