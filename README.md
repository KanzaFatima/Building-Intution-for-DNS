<p align="center">
<img src="https://i.imgur.com/MZ3iJ8R.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Building Intuition for DNS(Azure)</h1>
In this tutorial we will observe A-Record, CNAME Record and Local DNS Cache.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- DNS

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)

<h2>Prerequisites</h2>

- Antive Directory Installed
- Client and Domain Controller Connected

<h2>Actions and Observations</h2>


Login to DC-1 as your domain admin account (mydomain.com\kanza_admin)


Login to Client-1 as an admin (mydomain\kanza_admin)

</p>
<br />
<p>
<img src="https://i.imgur.com/v9lG0Iv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
From Client-1 try to ping “mainframe” notice that it fails, you can also Nslookup “mainframe” and it will also fails (no DNS record)

</p>
<br />

<p>
<img src="https://i.imgur.com/8oZFJUB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a DNS A-record on DC-1 for “mainframe” and point to the DC-1’s Private IP address. 
  
Go to DC1>Server Manager>tools>DNS
</p>
<br />

<p>
<img src="https://i.imgur.com/cY4qr6D.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  
<img src="https://i.imgur.com/leYeNC9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
>DC-1>Expand dc1>Forward lookup zone>mydomain.com>right click and create another A-Record for main frame and type DC-1 IP Address>add host>ok>done.
  
  
Minimize DC1 and go back to client 1.
</p>
<br />

<p>
<img src="https://i.imgur.com/r7A07Gw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
ping mainframe and Observe that it work.
</p>
<br />

<p>
<img src="https://i.imgur.com/He1bWHm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
You can also type nslookup mainframe.
</p>
<br />

<p>
<img src="https://i.imgur.com/JiAjvZM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Look at the DNS cache on client 1, and to do that type ip config /displaydns. 

</p>
<br />

<p>
<img src="https://i.imgur.com/XrjEntI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Local DNS Cache Exercise
  
  
Go to DC-1 and change mainframe’s record address to 8.8.8.8

  
Go back to Client-1 and ping “mainframe” again. Observe that it still pings the old address. Because old ip address is still exist in the local dns cache on client 1 computer.

</p>
<br />

<p>
<img src="https://i.imgur.com/lCVrhV7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/Qwxjll7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Type ipconfig /displaydns we can observe mainframe with the old ip address.
</p>
<br />

<p>
<img src="https://i.imgur.com/m2crOE4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we can use the command flushdns. It will wipe out all the previous cache. 

</p>
<br />

<p>
<img src="https://i.imgur.com/znPxbnv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Open Command Line as an local admin and type Ipconfig /displaydns to observe the local DNS cache, then Ipconfig /flushdns , it will successfully flush the dns. Then again type Ipconfig /displaydns and observe that it’s empty.

</p>
<br />

<p>
<img src="https://i.imgur.com/69m3OTV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Ping mainframe and it will resolve to 8.8.8.8 because it didn’t have anything in the cache, so it will force the dns server. 

</p>
<br />

<p>
<img src="https://i.imgur.com/NdmmFv2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Ipconfig / displaydns and we can see the latest A record mainframe with 8.8.8.8.
</p>
<br />


CNAME Record Exercise


<p>
<img src="https://i.imgur.com/VAKvNMi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go to Client-1 and ping search and it will fail.
</p>
<br />

<p>
<img src="https://i.imgur.com/My4zJvY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Go back to DC-1 and create a CNAME record that points the host “search” to www.google.com.
</p>
<br />

<p>
<img src="https://i.imgur.com/rA1Ebrv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Then on Client-1 ping search, and observe the results of the CNAME record, we can see google's ip address.
  
  
Browse search.mydomain.com but it will not going to match the certificate but we still force it to resolve it to google through cname record.

</p>
<br />

<p>
<img src="https://i.imgur.com/iA0OCvE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Ipconfig /displaydns, Ipconfig /flushdns, Ping search, Ipconfig /displaydns
 

</p>
<br />

<p>  
<img src="https://i.imgur.com/rA1Ebrv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
nslookup “search”, and observe the results of the CNAME record.
</p>
<br />

<img src="https://i.imgur.com/i8noAKR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
</p>
<p>
You can also observe Root Hints.
