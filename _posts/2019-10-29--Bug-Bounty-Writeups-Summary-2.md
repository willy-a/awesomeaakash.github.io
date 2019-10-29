---
layout: post
title:  Bug-Bounty-Writeups-Summary-2
date:   2019-10-29 
categories: Bug-Bounty-Writeups-Summarry
comments: false
---
Hello friends, Here is Part 2 of Bug Hunting Writeup Summary ->

#### 1. https://medium.com/@chawdamrunal/when-i-found-iframe-injection-and-illegal-redirect-dom-based-cfbbcec21a7

When i found iframe injection and illegal redirect (dom based) - By MRunal

First lets know about iFrame Injection ->

An iframe is an HTML document embedded inside another HTML document
An iframe attack is when a hacker/attacker embeds malicious code in your website page that executes various malicious instructions.

An iFrame injection is a very common cross site scripting (or XSS) attack. It consists of one or more iFrame tags that have been inserted into a page or post’s content and typically downloads an executable program or conducts other actions that compromise the site visitors’ computers.

Iframe injection, which occurs when a frame on a vulnerable web page displays another web page via a user-controllable input.

Let's move to the writeup -->

* He got an GET  endpoint parameter -> 

        GET /search.jsp?query=%3Ciframe%20src=%22https://google.com/?%22%3E%3C/iframe%3E HTTP/1.1
        Host: ****.net

![](https://miro.medium.com/max/1317/1*HUtjRWSvhqA6iBKw4CX4Zg.png)

* So, we can see google.com there in iframe 
* Then he used some payloads like ->


`http://target.com/something.jsp?query=<script>eval(location.hash.slice(1))</script>#alert(1)`

`<iframe src="data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg=="></iframe> (Firefox, Chrome, Safari)`

`</iframe><iframe src="vbscript:msgbox(1)"></iframe> (IE)<iframe src="data:text/html,<script>alert(0)</script>"></iframe> (Firefox, Chrome, Safari)`
`<iframe src="javascript:alert(1)">`
`<iframe src="vbscript:msgbox(1)"></iframe> (IE)`

* Accepting user-supplied data as iframe source URL may lead to malicious content being loaded within the Visualforce page.

Frame Spoofing vulnerabilities occur when:
1. Data enters a web application through an untrusted source.
2. The data is used as an iframe URL without being validated.

This way, if an attacker provides a victim with the iframesrc parameter set to a malicious website, the frame will be rendered with the content of the malicious website.

`<iframe src="http://evildomain.com/">`

### Redirection (DOM based) ->
* DOM based open redirect vulnerability. Open redirect occurs when a web page is being redirected to another URL in another domain via a user-controlled input.

        URL : https://*******.net/index.htm?url=http://evil.com/?orignal_url/
        Parameter Name :url
        Parameter Type :GET
        Pattern :http://evil.com/?orignal_url/


Impact ->

An attacker can use this vulnerability to redirect users to other malicious websites, which can be used for phishing and similar attacks.


Key Summary from this writeup ->

* Try iframe payloads on user input data which accepting user input

****
##### 2. https://medium.com/@D0rkerDevil/how-i-tookover-a-ldap-server-703209161001

How to Takover a ldap server - By Ashish Kunwar

First lets know about LDAP Server ->

LDAP (Lightweight Directory Access Protocol) is a software protocol for enabling anyone to locate organizations, individuals, and other resources such as files and devices in a network, whether on the public Internet or on a corporate intranet.


KeyPoint from Writeup ->

* He choosed a TOOL -> **BREXET** [But hey this was his personal tool]
		This tool gather lots of subdomains and ran nmap over every subdomain
* So, idea is here to run a tool to gather subdomains and run nmap on those subdomains
* He saw a port opened -> **389** with anonymous bind enabled
* His next move to use **SHODAN**
		Shodan Query -> ssl:target Port:”389" 

Anonymous LDAP Binding allows a client to connect and search the directory (bind and search) without logging in. You do not need to include binddn and bindpasswd.

* Now for this ldap he used a tool **"ldapsearch"** which can be install on linux using `apt install ldapscripts`

* Tool use ->

        ldapsearch -h <TARGET IP> 389 -x -s base -b ‘’ “(objectClass=*)” “*” +


![](https://miro.medium.com/max/1365/1*D4bvWvXmPqQtAz09k7ZTUg.png)

* He took a note of this ->  **defaultnamingcontext: dc=xxx,dc=xxx,dc=xx**

* Using that he can **enum ldap users and their access details and uids etc**

* Command he use for that purpose

`ldapsearch -h <TARGET IP> -p 389 -x -b “dc=xxx,dc=xxx,dc=xx”`

![](https://miro.medium.com/max/1365/1*9_aZnlKSRn9sH0qRcJQBCQ.png)

* He got lots of ldap users and other info there

* What can attacker do with above information ? -> 
    *   Can try for bruteforcing passwords or even can check for usernames and passwords with a default list using nmap
    * Command ->

    `nmap -p 389 — script ldap-brute — script-args ldap.base=’”dc=xxx,dc=xxxx,dc=xx”’ <target ip>`

* He said he can use a tool "http://jxplorer.org/" for the same task , we just need to connect to the port using it

JXplorer is a cross platform LDAP browser and editor. It is a standards compliant general purpose LDAP client that can be used to search, read and edit any standard LDAP directory, or any directory service with an LDAP or DSML interface.

* Another thing he told to  do NMAP  scan on all subs if the target scope is big

What I learned from this writeup ->

* Use NMAP scan on subdomains and see if any ports open for ldap and then enumerate it  and use tool like ldapsearch and even JXplorer etc etc
* Just need to do some more research or googling to learn more about ldap pentesting
* Best thing is make a script only for some common ports which you really wanna see to search for like for example here ldap , there may be more ports like that which can be useful for us, instead of searching for full port just check for common ports of interesting for time saving as automate stuff


****

##### 3. https://medium.com/@pranaybafna/graphql-introspection-leads-to-sensitive-data-disclosure-65b385452d7f

GraphQL Introspection leads to Sensitive Data Disclosure. - By Pranay Bafna

KeyPoint from Writeup ->

* First he learned about GraphQL Vulnerability  from **bug reports** and from **nahemsec's video**

* He searched for graphql endpoint in his target  and found ->	target.qa/infosec/graphql

* GraphQL is actually  an alternative to Rest-API

* Two BurpSuite Extension he used ->

	* GraphQL Raider -> testing endpoints implementing GraphQL
	* JSON Beautifier -> beautify JSON Content


* In his account he checked his Profile Update page and intercept it using Burpsuite and he send the request to Repeater

*  In Repeater once he send request he checked GrapQL Tab to see query there and he saw **Query** named as **"mutation**

*  In the Respone he saw this -> **“__typename”:”User”**
<br>
![](https://miro.medium.com/max/1906/1*e7RW7ylt8RgH0aqh_zQQrg.png)
<br>

* He played with that query and after some errors he once replaced **__typename with userHash** he got Hash Values


![](https://miro.medium.com/max/1280/1*PWo85FOc01wz6WMieAVH-A.jpeg)

* He got from GOOGLE -> **The Introspection Query**

* We must use and play with **The Introspection Query** as this can be different for every target website

* The query he used in that writeup i will not paste here, its a just our task to learn and play with this type of query for our Target

* This below was the final request and response he got ->


![](https://miro.medium.com/max/1275/1*CQHd-NTSXPc55jiir5KxAA.jpeg)


The best thing we can learn from this writeup is to just try to find GraphQL endpoints from directory bruteforcing and try to play qith query and try to get ERROR first and then understand that error to compromise the attack


****


##### 4. https://medium.com/@saadahmedx/bypassing-cors-13e46987a45b

Bypassing CORS - By Saad Ahmed

First he is not disclosing target name and assumed it as : **redact.com**

* So, the attacker login on the target and was looking for CSRF Attacks but he saw that the Current Password parameter need Current Password of victim  to exploit this feature

* Also during test he saw following Two Header ->

        Access-Control-Allow-Origin: https://redact.com
        Access-Control-Allow-Credentials: true

* In the Origin header he tried -> attacker.com instead of target website name but that didn't work - Consider it as TRY First Step during our Bug Hunting Time when we saw that **ACAO and ACAC** HEADER

* Now to bypass that feature he replaced  attacker.com to  target.com.attacker.com and it worked for him - So, consider it as our 2nd Try as Bypassing CORS


![](https://miro.medium.com/max/1284/1*KBvtXaVx0f_2GFiu_rcRUQ.png)



Q: What an attacker can do with this type of CORS Bypass ?

ANS: As a beginner i had question like above and i searched on google and got to know this below ->

Attacker would treat many victims to visit attacker's website, if victim is logged in, then his personal information is recorded in attacker's server.

It's mean attacker can get Victim's Detail on his server if the victim logged in

Also , If the site specifies the header Access-Control-Allow-Credentials: true, third-party sites may be able to carry out privileged actions and retrieve sensitive information. Even if it does not, attackers may be able to bypass any IP-based access controls by proxying through users' browsers.

This is called a great IMPACT of CORS Bypass :D

Source : https://hackerone.com/reports/426165

So, attacker now can fetch request to steal account information page & display it on evil.com like this ->

![](https://miro.medium.com/max/1099/1*pYvngRqdh-kFaoqmu6lcJA.png)


Takeway from this writeup ->

1. Check for ACAC Header and try evil.com or check for CORS Bypass payloads from google

***

##### 5. http://firstsight.me/2018/04/idor-at-private-bug-bounty-program-that-could-leads-to-personal-data-leaks/

IDOR (at Private Bug Bounty Program) that could Leads to Personal Data Leaks
Author: YoKo Kho

This blog is really very awesome 

* Best part to learn from this writeup is that once Author was lost interest to test this application as he saw that this private invite was since 2015 but when he saw there is 29 reports resolved so then he thought to try.

* What we can learn from this thing is that don't think if site is old or even if reports resolved is less , we must give a try for testing, afterall we get experience by testing such applications


* Another thing i loved from this writeup is that the Author first understand the Flow of Application and he wrote about it here

Here are the General Flow ->

![](http://firstsight.me/wp-content/uploads/2018/04/Screen-Shot-2018-04-17-at-8.13.45-PM-e1523970867477.png)


Learn from the above Application Flow ->

a) User choosed Destination like Hotel, and After that choice he submitted the request and then application  generated a temporary URL that saved his session that contains his choice and send the POST request that contains his personal Data which saved in his profile

     POST /booking2/numbers_here/save_session/unique_sessions_over_here HTTP/1.1
        
     Host: target.com

    POST Value: the detail that has been saved into our profile

b) Now user fill the personal details and need to choose Payment Method. After submitted the request, application will change the URL from **"save_session"** into **"order"** request

    POST /booking2/numbers_here/order/unique_sessions_over_here HTTP/1.1
    Host: target.com

    POST Value: the detail that has been saved into our profile including the chosen payment method

c) When the POST request has check correctly by the server (in other words, there is no needed data anymore), then the application will automatically send the request to the function (including the unique hash and payment numbers) that proceed the payment to communicate with the 3rd party service (we guess it because the name of the function is very similar with the company that provides this kind of service). To censored the name, then we will name it “xyzabc”.

    GET /xyzabc/unique_hash_over_here/payment_numbers_over_here/payment HTTP/1.1
    Host: target.com

d) After the unique hash and payment numbers are generated at the URL, then the application send this request to the POST Method to checkout the payment process to the 3rd party service. Here are a sample requests that sent by the application.

    POST /xyzabc/checkout/ HTTP/1.1
    Host: target.com

    csrfmiddlewaretoken=random_value_over_here&payment_id=means_payment_numb
    
e) It doesn’t matter if the detail is correct or not, the application will automatically generate the booking number. In this case, if the payment was failed, then the booking number (transaction ID) will contain the failed payment process information at the page.

    GET /order/X-12312312/instalment/resolve/?NTO=random_character_here HTTP/1.1
    Host: target.com
    

So, that was the flow of Application and after he analyze this all he concluded that if there is any problem in 4th step we can get success or failure booking.
So, in 4th step we need to manipulate **payment_id**  parameter to get success or failure booking 


Few words about **Insecure Direct Object Reference**

This kind of vulnerability could allow the Attacker to gain the access into some purposes without the need to has a valid authorization.



In this writeup, the issue for IDOR  was in **payment_id** parameter because the server was not validate the session in the checkout request.

By changing this parameter, then automatically the system will generate the other user’s booking number that could be used to enumerate the data that has the relation with those booking number. 


The Author also show clearance Example ->

if we change into another ID (at this case, we change from 4055809 to 4055311), we will get redirected into another user Booking Number (B-213xxxxx). At this part, we will get the information that someone with cencored@cencored.com as their email address was ordered the Hotel with 1 day trip duration. We also could get the other information such as total transaction, departure month, group size, location, and other.


Wow perfect example to learn about it



Here are the Pics of Proof to understand it more visually --->

![](http://firstsight.me/wp-content/uploads/2018/04/Screenshot-1-Payment-Request-copy-1024x343.png)

![](http://firstsight.me/wp-content/uploads/2018/04/Screenshot-2-IDOR-Response-copy-1024x553.png)


And to automate this result using Intruder -> setting the redirect to always

![](http://firstsight.me/wp-content/uploads/2018/04/Screenshot-3-Sample-of-Another-Response-copy-1024x701.png)


Flow of Attack as Recap ->

![](http://firstsight.me/wp-content/uploads/2018/04/Screen-Shot-2018-04-17-at-8.36.47-PM.png)


