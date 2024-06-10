---
title: "Install and configure Certificates in SSRS"
datePublished: Fri Jun 23 2023 22:00:00 GMT+0000 (Coordinated Universal Time)
cuid: clx980ghp000009la18njek3n
slug: install-and-configure-certificates-in-ssrs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1718038839281/aa93fac2-5543-4d4a-baad-30392b159319.png
tags: certificates, ssrs, azure-vm

---

### Install the Certificate in the Certificate Store

1. Open Microsoft Management Console (MMC)
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718037738360/3a293099-9f1d-4b19-9323-86deca9dd182.png align="center")

2. Go to File → Add/Remove Snap-In
    
3. Select "Certificates" in the left list
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718037768750/c80c7b94-0376-49d7-bc3a-0b997ca53027.png align="center")

4. Click Add &gt;
    
5. Specify the "Computer Account" option and click Next
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718037808103/b968e2b1-a59d-4e95-97ef-7acd74f70f8a.png align="center")

6. Specify "Local computer" and click Finish
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718037834204/997f204c-f717-4e14-866f-11a786185d9e.png align="center")

7. Back to the "Add or Remove Snap-in" dialog, click OK
    
8. Navigate to Console Root → Certificates (Local Computer) → Personal → Certificates
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718037957306/a935db1d-89f4-4091-a215-301895e7ef74.png align="center")

9\. Right-click the "Certificates" folder → All Tasks → Import

10. The "Local Machine" option should be selected and read-only. Click Next
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718037993048/08b30490-04f8-408e-ae24-cc44cce42921.png align="center")

11. Browse and select the certificate file and click Next
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718038057508/207d36b1-2013-4201-825b-bdc3a9ed27c6.png align="center")

12. If it is protected by a password, insert password and click next
    

13. Select "Place all certificates in the following store" and specify "Personal" in the Certificate Store textbox and click “Next”
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718038102058/ed0c4f72-1f7e-4989-bb56-e5bde7950730.png align="center")

14. Click “Finish”
    
15. The new certificate now appear in the Certificates folder
    
16. Right-click the certificate → Properties
    
17. Specify a friendly name
    

### Configure SSRS to Use the New Certificate

18. Open "Report Server Configuration Manager"
    
19. Connect to the report server (and start it if stopped)
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718038243618/de55f71d-1533-44f7-96f1-412b6c1d3521.png align="center")

20. Select “Web Service URL”
    
21. Change HTTPS certificate selecting the new friendly name
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718038325718/afb80975-94a0-4863-9ba0-38fd030ae412.png align="center")

22. Click Advanced
    
23. Remove old certificate from web portal url
    
24. Add new certificate to web portal url in both (All IPv4) and (All IPv6)
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718038397421/30364535-4cd4-445c-a23d-24046853d11e.png align="center")

25. Select “Web Portal URL”
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718038450384/18b30a15-dc2f-4f67-a95c-221f1c7c8493.png align="center")

26. Click “Advanced”
    
27. Remove old certificate from web portal url
    
28. Add new certificate to web portal url in both (All IPv4) and (All IPv6)
    
29. Click “Apply” and wait for it to process