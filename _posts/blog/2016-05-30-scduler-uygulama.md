---
layout: post
title: "Scheduler Application Nedir?"
modified:
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2016-05-30T08:17:40-04:00
---

## Tanım

Scheduler application, kısaca belirli aralıklarla çalışmasını istediğimiz uygulamalardır diyebiliriz. Eğer bir uygulamanın istenilen zaman aralıklarıyla çalışmasını istiyorsak bunu için Java'da Quartz Api' yı kullanabiliriz.Ancak küçük bir ipucu verelim.

Eğer ki uygulamanız günde bir kere çalışacaksa , uygulamayı exe haline çevirip windows task olarak tanımlayabiliriz. Böyle yaptığımızda bellekten kazanç sağlamış oluruz. Kullanmak istemediğinizde ise Quartz Api işimizi görür elbette.

Quartz Api buradan , <http://www.java2s.com/Code/Jar/q/Downloadquartzjar.htm> indirip projenize dahil edebilirsiniz. Uygulamanızı Maven 'e çevirdiyseniz de, pom.xml 'e aşağıdaki dependencies leri eklediğinizde projenize otomatik olarak build edilir.

~~~
<dependencies>
		<!-- Quartz API -->
		<dependency>
			<groupId>opensymphony</groupId>
			<artifactId>quartz</artifactId>
			<version>1.6.3</version>
		</dependency>

		<dependency>
			<groupId>commons-collections</groupId>
			<artifactId>commons-collections</artifactId>
			<version>3.2.1</version>
		</dependency>

		<dependency>
			<groupId>org.apache.directory.studio</groupId>
			<artifactId>org.apache.commons.logging</artifactId>
			<version>1.1.1</version>
		</dependency>
</dependencies>
~~~

Projenize gerekli kütüphaneleri ekledikten sonra , nasıl çalışacağını anlatalım. 

DosyaAdı : MyJob

~~~
package com.omeryaylaalti.quartz;

import org.quartz.Job;
import org.quartz.JobExecutionContext;
import org.quartz.JobExecutionException;

public class MyJob implements Job
{
	public void execute(JobExecutionContext context)
	throws JobExecutionException {
		
		System.out.println("Quartz Api kullanıma hoşgeldiniz!");	
		
	}
	
}
~~~

Daha sonra ne kadar sürede çalışacağıyla ilgili olarak yapılan trigger tanımlamalarından bahsedelim.
Her 30 saniyede bir çalışmasını istiyorsak;

~~~
SimpleTrigger trigger = new SimpleTrigger();
    	trigger.setName("AppStartedTrigger");
    	trigger.setStartTime(new Date(System.currentTimeMillis() + 1000));
    	trigger.setRepeatCount(SimpleTrigger.REPEAT_INDEFINITELY);
    	trigger.setRepeatInterval(30000);
~~~

Son olarak uygulamanın çalışacağı ana sınıf tanımına geçelim.

Dosya Adı: AppStarted

~~~
package com.omeryaylaalti.app;

import java.util.Date;

import org.quartz.JobDetail;
import org.quartz.Scheduler;
import org.quartz.SimpleTrigger;
import org.quartz.impl.StdSchedulerFactory;

public class AppStarted 
{
    public static void main( String[] args ) throws Exception
    {
       	JobDetail job = new JobDetail();
    	trigger.setName("AppStartedTrigger");
    	job.setJobClass(MyJob.class);
    	
    	//configure the scheduler time
    	SimpleTrigger trigger = new SimpleTrigger();
    	trigger.setStartTime(new Date(System.currentTimeMillis() + 1000));
    	trigger.setRepeatCount(SimpleTrigger.REPEAT_INDEFINITELY);
    	trigger.setRepeatInterval(30000);
    	
    	//schedule it
    	Scheduler scheduler = new StdSchedulerFactory().getScheduler();
    	scheduler.start();
    	scheduler.scheduleJob(job, trigger);

    }
}
~~~

Gördüğünüz gibi main class ını çalıştırdığımızda uygulama 30 saniyede bir çalışır. 

Yazımı burada sonlandırıyorum. Okuduğunuz için teşekkürler. Bir sonraki yazımda jar uygulamayı exe ye çevirmeyi anlatacağım.

Saygılarımla.
