---
title: "Send html emails with SSIS"
datePublished: Mon Jul 20 2020 08:13:15 GMT+0000 (Coordinated Universal Time)
cuid: ckrq2nq8j09cazcs163mi045u
slug: send-html-emails-with-ssis
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1718039376202/039fa40b-561a-4698-b706-ab919079704f.png
tags: sql-server, csharp, html-emails, emails, ssis, sql-server-integration-services

---

**GOAL**: send an html email using Sql Server Integration Service

**PROBLEM**: the Send Mail Task in SSIS allows only email in text format and not in html format.

**SOLUTION**: use a C# script instead the Send Mail Task to create and send the email.

1. Add a Script Task to the solution and choose C# as language
    
2. Open the task and click on Edit Script
    
3. Under the "region namespaces" part, add this namespace:
    
    *using System.Net.Mail;*
    
4. After the "// TODO: Add your code here" add the following code (replace the highlighted part with your parameters):
    

```plaintext
   //Create email
   MailMessage msg = new MailMessage();

   //From
   msg.From = new MailAddress("example@gmail.com");
   string user = "massimiliano.figini";
   string password = "***************";

   //To
   msg.To.Add("example@yahoo.com");

   //CC
   msg.To.Add("example2@yahoo.it");

   //Subject
   msg.Subject = "subject here";

   //Text in html
   msg.Body = "<html><body><h1>Test</h1><p>Test <b>test</b> message here.</p></body></html>";
   msg.IsBodyHtml = true;

   //Attachment
   msg.Attachments.Add(new Attachment("C:\\Test.txt"));

   //Send email
   SmtpClient smtp = new SmtpClient();
   smtp.Host = "smtp.gmail.com";
   smtp.Port = 587;
   smtp.UseDefaultCredentials = false;
   smtp.Credentials = new System.Net.NetworkCredential(user, password);
   smtp.EnableSsl = true;
   smtp.Send(msg);
```