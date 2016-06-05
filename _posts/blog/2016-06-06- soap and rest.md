---
layout: post
title: "SOAP ve REST Mimarilerin Genel Özellikleri"
modified:
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2016-06-05T18:25:40-04:00
---

## Tanım

Mimarilerin tanımlarına geçmeden önce web servislerin uygulamalar tarafından kullanılan metodların bir sunucu üzerinde çalıştırılıp değer döndürmesini sağlayan yapılar olduğunu bilmemiz gerekir.Böylece farklı uygulamalarda veya farklı teknolojilerle kullanabileceğimiz metodları bir kere yazıp platformdan bağımsız bir şekilde çalıştırabiliriz.

Web servis mimarisinin temeli HTTP üzerine kurulmuştur. Yani genel olarak web servise bir istek gelir ve web servis bu isteği yapıp bir sonuç döndürür. Web servisin bu işlemi yapabilmesi için tanımlanmış farklı yöntemler bulunmaktadır. Bu yazıda bahsedilecek olan yapılardan biri SOAP protokolü diğeri ise REST’dir.

**SOAP(Simple Object Access Protocol) Nedir?**

SOAP , uygulamalar ile web servislerin bilgi aktarımını sağlayan XML tabanlı bir protokoldür. Yani web servise giden bilgi XML olarak gönderilir, web servis bu bilgiyi yorumlar ve sonucunu XML olarak geri döndürür.  Bu web servis tanımlaması WSDL standardı ile yapılır. Burada dikkat edilmesi gereken en önemli durumlardan biri SOAP bizi XML tabanlı kullanıma mecbur bırakır. Bu konuda esnek değildir.

**REST(Representational State Transfer) Nedir?**

REST mimarisinde ise işlemler resource kavramıyla yapılır. Resource URI ile tanımlanır ve bir metod tanımlaması veya bir değişken olabilir. Yani REST’te SOAP’ta olduğu gibi XML yardımıyla metodlar çağırılmaz bunun yerine o metodu çağıracak URIler ile web servise HTTP protokolüyle istek yapılır. Böylece REST için WSDL gibi bir tanımlama diline ihtiyaç kalmaz işlemler tamamen HTTP metodları üzerinden yapılır. Örneğin, bir web servisin metodunu SOAP ile “getProductName” şeklinde çağırırken REST ile “/products/name/1″ URI’si ile çağırabiliriz. Ayrıca RESTin döndürdüğü veri tipinin de XML olması zorunlu değildir JSON, XML, TXT, HTML gibi istenen veri türünde değer döndürülebilir.

**SOAP ile REST Mimarilerin Karşılaştırılması**

* SOAP XML veri tipini desteklerken REST istenen veri türüyle işlem yapabilir. JSON veri tipi ile XML’den çok daha düşük boyutlarla veri tutulabildiği için REST ile daha hızlı işlem yapılabilir.

* Ayrıca SOAP için WSDL ile tanımlama yapmak gerekirken REST için böyle bir zorunluluk yoktur. (WADL REST için kullanılan WSDL’e benzer bir yapıdır fakat kullanma zorunluluğu yoktur.) Bir dile ihtiyaç duymadan HTTP metodlarıyla tasarlanabildiği için REST’i kullanması ve tasarlaması daha kolaydır.

* SOAP için birçok geliştirme aracı mevcuttur, REST için geliştirme araçlarına ihtiyaç duyulmaz, tasarlaması kolaydır.

* SOAP; XML-Scheme kullanırken REST; URI-scheme kullanır yani metotlar için URI’ler tanımlanır.

* Her ikisi de HTTP protokolünü kullanırlar. Fakat REST için HTTP zorunluluğu varken SOAP; TCP, SMTP gibi başka protokollerle de çalışabilir.

* Test ve hata ayıklama aşaması REST için daha kolaydır. Çünkü HTTP hatalarını döndürür ve bunlar bir toola ihtiyaç duyulmadan görülebilir. SOAP için hata ayıklama araçları gerekebilir.

* REST basit HTTP GET metodunu kullandığı için cacheleme işlemi daha kolaydır. SOAP ile cacheleme yapabilmek için karmaşık XML requestleri yapılmalıdır.

* Güvenlik açısından SOAP daha gelişmiştir çünkü hazır yapılar bulunmaktadır. Dokümantasyon bakımından SOAP daha gelişmiştir ve daha fazla kaynak bulunmaktadır.


Yazımı burada sonlandırıyorum. Umarım faydalı olmuştur. Okuduğunuz için teşekkürler. 

Saygılarımla.
