<h1>Advanced Hunting APTs with Splunk</h1>

<h2>Description</h2>
In this lab, we are assuming the persona of Alice Bluebird, the analyst who successfully assisted Wayne Enterprises and was recommended to Grace Hoppy to assist them with their recent issues.
<br />


<h2>Languages and Utilities Used</h2>

- <b>Splunk</b> 
- <b>SPL</b>

<h2>Environments Used </h2>

- <b>Linux</b> (21H2)

<h2>Lab Walk-through</h2>

<p align="center">
The first objective is to find out what Amber’s IP address is by searching for a string that matches Amber with the Pan traffic source type: 10[.]0[.]2[.]101: <br/>
<img src="https://imgur.com/TCtCH8x.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
<br />
<br />
Since we are looking for the competitor’s site, we can use some of the fields to help us narrow down the options. Here we created a table and removed duplicates.:  <br/>
<img src="https://imgur.com/umJILdL.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
<br />
<br />
Since we know that Amber works in the beer industry, we can try this as a key word. We now know that the competitor’s site is www[.]berkbeer[.]com.: <br/>
<img src="https://imgur.com/a/HvAyGpe.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
<br />
<br />
We know that Amber had found the Executives contact information let’s look for the image that displayed the contact information. Using what we have so far, we can investigate the URI_path field and see all the images.:  <br/>
<img src="https://imgur.com/6EXeDx2.png" height="80%" width="80%" alt="Splunk Searching Steps"/>
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
