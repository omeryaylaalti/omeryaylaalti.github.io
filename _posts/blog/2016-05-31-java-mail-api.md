---
layout: post
title: "Java Mail API ile Mail Gönderme"
modified:
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2016-05-30T18:25:40-04:00
---

## Tanım

Java uygulamaları içerisinden platform bağımsız olarak gmail hesabınızı kullanarak istediğiniz kişiye mail gönderimi yapabilirsiniz. 

Bunun için Java'nın bize sunduğu JavaMail API  kütüphanesi ile aşağıdaki gibi yapabiliriz.

JavaMail API  buradan , <http://www.java2s.com/Code/Jar/j/Downloadjavamail144jar.htm> indirip projenize dahil edebilirsiniz. Uygulamanızı Maven 'e çevirdiyseniz de, pom.xml 'e aşağıdaki dependencies leri eklediğinizde projenize otomatik olarak build edilir.

~~~
<!-- http://mvnrepository.com/artifact/javax.mail/mail -->
<dependency>
    <groupId>javax.mail</groupId>
    <artifactId>mail</artifactId>
    <version>1.4.7</version>
</dependency>
~~~

Gerekli kütüphaneyi ekledikten sonra aşağıdaki main classını düzenleyerek kendi mail gönderme işlemini yapabilirsiniz.

~~~
package org.javablog;
 
import java.util.Properties;
 
import javax.activation.DataHandler;
import javax.activation.FileDataSource;
import javax.mail.Authenticator;
import javax.mail.Message;
import javax.mail.MessagingException;
import javax.mail.Multipart;
import javax.mail.PasswordAuthentication;
import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeBodyPart;
import javax.mail.internet.MimeMessage;
import javax.mail.internet.MimeMultipart;
 
/*
 * @author Ömer Yaylaaltı
 */
 
public class SimpleMailSender
{
	private String smtpHost; //Host
	private String smtpAuthUser;
	private String smtpAuthPassword;
	private String mailBody;
	private String mailSubject;
	private String mailSender;
 
	private String[] mailList;
	private String[] attachmentFiles; 
 
	public SimpleMailSender(String host, String authUser,
			String authPassword, String body, String subject,
			String[] list, String sender, String[] files)
	{
		smtpHost = host;
		smtpAuthUser = authUser;
		smtpAuthPassword = authPassword;
		mailBody = body;
		mailSubject = subject;
		mailSender = sender;
		mailList = list;
		attachmentFiles = files;
 
	}
 
	public void sendMail() throws MessagingException
	{
		Properties properties = new Properties();
		properties.put("mail.transport.protocol", "smtp");
		//this property is required
		properties.put("mail.smtp.starttls.enable","true");
		properties.put("mail.smtp.host", smtpHost);
		//required if SMTP server requires authentication
		properties.put("mail.smtp.auth", "true");
 
		Authenticator auth = new SMTPAuthenticator();
		Session session = Session.getDefaultInstance(properties, auth);
 
		//InternetAddress class represents an Internet email address
		//Hence, lets model our email addresses
 
		InternetAddress mailFrom = new InternetAddress(mailSender);
		InternetAddress[] mailTo = new InternetAddress[mailList.length];
 
		for(int i = 0; i < mailList.length; i++)
		{
			mailTo[i] = new InternetAddress(mailList[i]);
		}
 
		//Modeling an email message
		//Message abstract class helps us to do this
		//MimeMessage which extends Message class represents a MIME style email message
		//We will represent entire email message in this form
		//Let's create an instance
 
		Message message = new MimeMessage(session);
		message.setFrom(mailFrom);
		message.setRecipients(Message.RecipientType.TO, mailTo);
		message.setSubject(mailSubject);
 
		/* Construct mail body parts */
 
		Multipart multipart = new MimeMultipart();
 
		//This represents message body part of mail
		MimeBodyPart bodyPartMessage = new MimeBodyPart();
		bodyPartMessage.setText(mailBody);
 
		//Add first body part to multipart
		multipart.addBodyPart(bodyPartMessage);
 
		//This represents message attachment part
		//will be added multipart later
 
		MimeBodyPart bodyPartAttachment = new MimeBodyPart();;
		FileDataSource fileDataSource;
 
		for(int i = 0; i < attachmentFiles.length; i++)
		{
			fileDataSource = new FileDataSource(attachmentFiles[i]);
			bodyPartAttachment.setDataHandler(new DataHandler(fileDataSource));
			bodyPartAttachment.setFileName(fileDataSource.getName());
			//Add all attachment files to bodypart in multipart object
			multipart.addBodyPart(bodyPartAttachment);
		}
 
		//Add entire mail body to the message object
		message.setContent(multipart);
 
		//Finally send the message
		Transport.send(message);
 
	}
	
	//Testing our class
	public static void main(String[] args) throws MessagingException
	{
		String[] to = new String[2];
		to[0] = "java@gmail.com";
		to[1] = "mail@gmail.com";
		String[] files = new String[2];
		files[0] = "file1.txt";
		files[1] = "file2.txt";
 
		SimpleMailSender mailSender = new SimpleMailSender(
				"smtp.gmail.com", "authUser@gmail.com", "authPassword", "Mail Body",
				"Mail Subject", to, "sender@gmail.com",
				files
				);
		mailSender.sendMail();
		System.out.println("Mail has been sent successfully");
	}
 
	/*
	 * used to do simple authentication when the SMTP
	 * server requires it.
	 */
 
	private class SMTPAuthenticator extends Authenticator
	{
 
		public PasswordAuthentication getPasswordAuthentication() {
			String username = smtpAuthUser;
			String password = smtpAuthPassword;
			return new PasswordAuthentication(username, password);
		}
	} //end of SMTPAuthenticator class
 
} // end of SimpleMailSender class
~~~

Yukarıda dikkat edilmesi gereken yerler, protokol, host, e-mail , parola kısmıdır. Gerekli açıklamaları kodların içerisinde ingilizce olarak yazmaya çalıştım. Bilgileri doğru girdiğiniz takdirde java uygulamanızdan mail gönderme işlemi yapılmış olacaktır.

Yazımı burada sonlandırıyorum. Okuduğunuz için teşekkürler. 

Saygılarımla.
