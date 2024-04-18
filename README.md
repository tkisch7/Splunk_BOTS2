<h1>Advanced Hunting APTs with Splunk</h1>

<h2>Description</h2>
In this lab, we are assuming the persona of Alice Bluebird, the analyst who successfully assisted Wayne Enterprises and was recommended to Grace Hoppy to assist them with their recent issues.
<br />


<h2>Languages and Utilities Used</h2>

- <b>Splunk</b> 
- <b>SPL</b>
- <b>Virsu Total</b>
- <b>CyberChef</b>

<h2>Environments Used </h2>

- <b>Linux Virtual Machine</b>

<h2>Lab Walk-through</h2>

<p align="center">
1. The first objective is to find out what Amber’s IP address is by searching for a string that matches Amber with the Pan traffic source type: <br/>
<img src="https://imgur.com/TCtCH8x.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
<br />
<br />
2. Since we are looking for the competitor’s site, we can use some of the fields to help us narrow down the options. Here we created a table and removed duplicates.:  <br/>
<img src="https://imgur.com/umJILdL.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
<br />
<br />
3. Since we know that Amber works in the beer industry, we can try this as a keyword. We now know that the competitor’s site is www[.]berkbeer[.]com.: <br/>
<img src="https://imgur.com/nDEni3W.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
<br />
<br />
4. We know that Amber had found the Executives contact information let’s look for the image that displayed the contact information. Using what we have so far, we can investigate the URI_path field and see all the images.:  <br/>
<img src="https://imgur.com/6EXeDx2.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
 <br />
<br />
5. Now that we have a piece of the executive’s name, we can pivot to the SMTP traffic and see if we can find out his first name. First, we can look into what Ambers's work email address is. After some searching, we can find her address is aturing@froth[.]ly.:  <br/>
<img src="https://imgur.com/46NLEdH.png" height="80%" width="80%" alt="Splunk Searching Steps"/><br />
<br />
6. I am going to use Amber’s email to help us find all conversations with emails ending in the berkbeer domain. Here we can investigate the emails sent between MBERK@berkbeer[.]com aka Martin Berk. :  <br/>
<img src="https://imgur.com/rPbzOFK.png" height="80%" width="80%" alt="Splunk Searching Steps"/><br />
<br />
7. We want to see the name of the file attachment that Amber sent to the competitors. After looking at all the emails that were sent, we can see one has attachments included.:  <br/>
<img src="https://imgur.com/Lyn7iWZ.png" height="80%" width="80%" alt="Splunk Searching Steps"/><br />
<br />
8. We also see that the competitors have asked for Amber to provide a personal email. Trying to look through the last message I noticed that the email is encoded in base64. And after throwing this into cyber chef we can very easily see what Ambers's personal email is.:  <br/>
<img src="https://imgur.com/23ma72V.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
 <br />
<br />
9. Amber downloaded TOR to obfuscate her internet browsing and we can find the version by searching for the install file. After searching using the keyword TOR we can find the install file using the fields to our advantage. The version is 7.0.4.:  <br/>
<img src="https://imgur.com/6RFKnyk.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
 <br />
<br />
10. To find the IP of Brewertalk[.]com we can look at the HTTP stream and look for the most common public destination address associated with the specific string.:  <br/>
<img src="https://imgur.com/4iIHmnn.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
 <br />
<br />
11. The IP that was found scanning the Brewertalk server is also likely to be used to attack a URI path. We can find this by searching the attacker's IP as the src IP. This returns a lot of events so sampling by 1:100 can help narrow these down. Now we can see that the most common URI path is /members.php. :  <br/>
<img src="https://imgur.com/0MfCcLo.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
 <br />
<br />
12. To find the SQL function we can look at the details coming in from the form_data. After a quick look we can see a repeated function of updatexml.:  <br/>
<img src="https://imgur.com/CUYPRj0.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
 <br />
<br />
13. Now we need to focus on identifying a cookie value that was transmitted as an XSS attack. I searched for the string “kevin” under the HTTP stream source type you can find the value that is repeated several times in the lastvist cookie.:  <br/>
<img src="https://imgur.com/PKeYy8h.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
 <br />
<br />
14. Maybe we can also find the username that was created on this search as well, so after searching the page we find the username klagerfield.:  <br/>
<img src="https://imgur.com/7EaIqin.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
 <br />
<br />
15. We are tasked with finding out what the PowerPoint is renamed on Mallory’s MacBook. We can start by searching using Mallory as a keyword and then seeing if we can find any hosts that are from a MacBook. To filter this even more we can search for .ppt and .pptx file extensions.:  <br/>
<img src="https://imgur.com/x6XLeJW.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
 <br />
<br />
16. A Game of Thrones movie file was also encrypted. To find this I used the “.crypt” file extension that we found on the encrypted PowerPoint file. After searching on the host the first couple of results lead us to the answer. Season 7 episode 2 was encrypted by the ransomware.:  <br/>
<img src="https://imgur.com/0XGpt3m.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
 <br />
<br />
17. We want to find out the USB vendor that was likely used by Kevin to download malware on Mallory’s Personal laptop. Starting off we know that Mallory’s personal host is called Kutekitten. Using the knowledge provided we can search for usb in the Kutekitten host. After looking at some field values we can find some columns that are included in the OSquery results that point to the vendor ID of the USB device. With a quick Google search, we can find the vendor and model of the device that was most likely used.:  <br/>
<img src="https://imgur.com/bleO1i7.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
 <br />
<br />
18. To find the file that was downloaded we can traverse through Mallory’s user files. After looking at the downloads folder we found an interesting file named important HR information. Taking the hash that is generated by Splunk and searching for it on Virus Total we can see that it is indeed the malicious file we are looking for and find out that the file is written in PERL. On Virus Total we can also see the Dynamic DNS destinations that the malware uses to communicate to the C2C server.:  <br/>
<img src="https://imgur.com/cEkPpGw.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
 <br />
<br />
19. Now we need to find the name of the attachment that was sent as a part of a phishing email. We know that the malicious file is a compressed zip file so to search for this we are going to look into the SMTP stream and look for any file ending in the “.zip” extension This will be fairly easy to find as we see the attached file named invoice.zip. :  <br/>
<img src="https://imgur.com/2MhuhgF.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
 <br />
<br />
20. If we would like to know the password of the zip file we can look at the same search and look within the email content. After a little bit of analyzing the content, we find the password to be 912345678.:  <br/>
<img src="https://imgur.com/kp0pRhW.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
 <br />
<br />
21. We know that the Taedonggang APT uses SSL to encrypt their traffic, so if we want to know the SSL issuer we can conduct a search using the IP we found scanning a server. Here we can look at the interesting fields and find the ssl_issuer is “C = US”:  <br/>
<img src="https://imgur.com/kxa59v6.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
 <br />
<br />
22. The Winsys32.dll file causes a file to be downloaded that would be usual for an American company. When doing the initial search you can see that the FTP client is running. After searching for it in the FTP stream and looking at the download load way, we see the file that was downloaded.:  <br/>
<img src="https://imgur.com/qXbwVk2.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
 <br />
<br />
23. The APT is known to create scheduled tasks to maintain persistence within  a system. Starting off we can search for the schtasks executable, and then pick up on anything that looks odd. Looking at the keypath “\\software\\microsoft\\network”  and the sourcetype winregistry we can find 4 items with base64 encoded text. Here we can see the Process.php being called several times.:  <br/>
<img src="https://imgur.com/LBV5QCH.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
