---
layout: post
title: "Exchange Server Mailboxdan Mail Okuma Uygulaması"
modified:
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2016-05-30T18:25:40-04:00
---

## Exchange Server Nedir?

Microsoft tarafından üretilen bir haberleşme ürünüdür.Outlook hesabı açılarak mailbox tanımı yapıldıktan sonra bu üründen yararlanabilirsiniz.

## Exchange Server Mailboxdan Mailleri Okuma 

Java teknolojisini kullanarak mail kutumuza gelen mailleri okuyup bunlarla çeşitli işlemler yapabiliriz. Exchange serverdan mailleri okuma işlemini Java'nın bize sunduğu EWS Java API kütüphanesi ile aşağıdaki gibi yapabiliriz.

EWS Java API buradan , <http://www.java2s.com/Open-Source/Java_Free_Code/HTTP/Download_EWS_Java_API_Free_Java_Code.htm> indirip projenize dahil edebilirsiniz. Uygulamanızı Maven 'e çevirdiyseniz de, pom.xml 'e aşağıdaki dependencies leri eklediğinizde projenize otomatik olarak build edilir.

~~~
<!-- http://mvnrepository.com/artifact/com.microsoft.ews-java-api/ews-java-api -->
<dependency>
    <groupId>com.microsoft.ews-java-api</groupId>
    <artifactId>ews-java-api</artifactId>
    <version>2.0</version>
</dependency>
~~~

Gerekli kütüphaneyi ekledikten sonra aşağıdaki main classını düzenleyerek kendi mail kutunuzu okuyabilirsiniz.

~~~
import java.net.URI;
import java.util.ArrayList;
import java.util.Calendar;
import java.util.Date;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import microsoft.exchange.webservices.data.Appointment;
import microsoft.exchange.webservices.data.AppointmentSchema;
import microsoft.exchange.webservices.data.CalendarFolder;
import microsoft.exchange.webservices.data.CalendarView;
import microsoft.exchange.webservices.data.EmailMessage;
import microsoft.exchange.webservices.data.ExchangeCredentials;
import microsoft.exchange.webservices.data.ExchangeService;
import microsoft.exchange.webservices.data.ExchangeVersion;
import microsoft.exchange.webservices.data.FindItemsResults;
import microsoft.exchange.webservices.data.Folder;
import microsoft.exchange.webservices.data.Item;
import microsoft.exchange.webservices.data.ItemId;
import microsoft.exchange.webservices.data.ItemView;
import microsoft.exchange.webservices.data.PropertySet;
import microsoft.exchange.webservices.data.ServiceLocalException;
import microsoft.exchange.webservices.data.WebCredentials;
import microsoft.exchange.webservices.data.WellKnownFolderName;
/**
 * @author Ömer Yaylaaltı
 *
 */
public class MSExchangeEmailService {
private static ExchangeService service;
private static Integer NUMBER_EMAILS_FETCH =5; // only latest 5 emails/appointments are fetched.

static{
try{
service = new ExchangeService(ExchangeVersion.Exchange2010_SP1);
//service = new ExchangeService(ExchangeVersion.Exchange2007_SP1); //depending on the version of your Exchange. 
service.setUrl(new URI("https://webmail.xxxx.com/ews/Exchange.asmx"));
}catch (Exception e) {
e.printStackTrace();
}
}

public MSExchangeEmailService() {
ExchangeCredentials credentials = new WebCredentials("USERNAME","PWD","DOMAIN_NAME");
service.setCredentials(credentials);
}
/**
 * Reading one email at a time. Using Item ID of the email.
 * Creating a message data map as a return value.   
 */
public Map readEmailItem(ItemId itemId){
Map messageData = new HashMap();
try{
Item itm = Item.bind(service, itemId, PropertySet.FirstClassProperties);
EmailMessage emailMessage = EmailMessage.bind(service, itm.getId());
messageData.put("emailItemId", emailMessage.getId().toString());
messageData.put("subject", emailMessage.getSubject().toString());
messageData.put("fromAddress",emailMessage.getFrom().getAddress().toString());
messageData.put("senderName",emailMessage.getSender().getName().toString());
Date dateTimeCreated = emailMessage.getDateTimeCreated();
messageData.put("SendDate",dateTimeCreated.toString());
Date dateTimeRecieved = emailMessage.getDateTimeReceived();
messageData.put("RecievedDate",dateTimeRecieved.toString());
messageData.put("Size",emailMessage.getSize()+"");
messageData.put("emailBody",emailMessage.getBody().toString());
}catch (Exception e) {
e.printStackTrace();
}
return messageData;
}
/**
 * Number of email we want to read is defined as NUMBER_EMAILS_FETCH, 
 */
public List> readEmails(){
List> msgDataList = new ArrayList>();
try{
Folder folder = Folder.bind( service, WellKnownFolderName.Inbox );
FindItemsResults results = service.findItems(folder.getId(), new ItemView(NUMBER_EMAILS_FETCH));
int i =1;
for (Item item : results){
Map messageData = new HashMap();
messageData = readEmailItem(item.getId());
System.out.println("\nEmails #" + (i++ ) + ":" );
System.out.println("subject : " + messageData.get("subject").toString());
System.out.println("Sender : " + messageData.get("senderName").toString());
msgDataList.add(messageData);
}
}catch (Exception e) { e.printStackTrace();}
return msgDataList;
}
/**
 * Reading one appointment at a time. Using Appointment ID of the email.
 * Creating a message data map as a return value.   
 */
public Map readAppointment(Appointment appointment){
Map appointmentData = new HashMap();
try {
appointmentData.put("appointmentItemId", appointment.getId().toString());
appointmentData.put("appointmentSubject", appointment.getSubject());
appointmentData.put("appointmentStartTime", appointment.getStart()+"");
appointmentData.put("appointmentEndTime", appointment.getEnd()+"");
//appointmentData.put("appointmentBody", appointment.getBody().toString());
} catch (ServiceLocalException e) {
e.printStackTrace();
}
return appointmentData;
}
/**
  *Number of Appointments we want to read is defined as NUMBER_EMAILS_FETCH,
  *  Here I also considered the start data and end date which is a 30 day span.
  *  We need to set the CalendarView property depending upon the need of ours.   
 */
public List> readAppointments(){
List> apntmtDataList = new ArrayList>();
Calendar now = Calendar.getInstance();
Date startDate = Calendar.getInstance().getTime();
now.add(Calendar.DATE, 30);
        Date endDate = now.getTime();  
try{
CalendarFolder calendarFolder = CalendarFolder.bind(service, WellKnownFolderName.Calendar, new PropertySet());
CalendarView cView = new CalendarView(startDate, endDate, 5);
cView.setPropertySet(new PropertySet(AppointmentSchema.Subject, AppointmentSchema.Start, AppointmentSchema.End));// we can set other properties as well depending upon our need.
FindItemsResults appointments = calendarFolder.findAppointments(cView);
int i =1;
List appList = appointments.getItems();
for (Appointment appointment : appList) {
System.out.println("\nAPPOINTMENT #" + (i++ ) + ":" );
Map appointmentData = new HashMap();
appointmentData = readAppointment(appointment);
System.out.println("subject : " + appointmentData.get("appointmentSubject").toString());
System.out.println("On : " + appointmentData.get("appointmentStartTime").toString());
apntmtDataList.add(appointmentData);
}
}catch (Exception e) {
e.printStackTrace();
}
return apntmtDataList;
}
public static void main(String[] args) {
MSExchangeEmailService msees = new MSExchangeEmailService();
msees.readEmails();
msees.readAppointments();
}
}
~~~

Yukarıda dikkat edilmesi gereken yerler, kullanıcı adı, parola ve domain name kısmıdır. Doğru girdiğiniz takdirde java ile entegrasyon işlemi yapılmış olacaktır.

Yazımı burada sonlandırıyorum. Okuduğunuz için teşekkürler. 

Saygılarımla.
