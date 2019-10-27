---
layout: post
title:  Bug-Bounty-Writeups-Summary-1
date:   2019-6-18 
categories: Bug-Bounty-Writeups-Summarry
comments: false
---
Hello Friends,After a very long time I am updating my blog. I didn't continue my bug hunting day wise blog becuase of my personal problems.

But now I will start daily blog posts but now on Bug Bounty Writeups Summary , so that we learn from writeups more easily.

I will post daily 5 Summaries of Bug Bounty Writeups. Please guys read this and tell me if i need any improvement in this or you have any ideas then please message me on my Twitter
[https://twitter.com/LearnerHunter]

Thanks

 Bug Hunting Writeup Summary/Notes/KeyLearn
-----------------------------------------------------------------------------------

#####1.  https://blog.usejournal.com/xss-in-microsoft-subdomain-81c4e46d6631

XSS in Microsoft Subdomain Writeup by Sudhanshu Rajbhar

Key Learn from this writeup ->

* He watched and read about XSS and reproduction steps and noted down it.
* For target Mirosoft he checked for input field for xss
* He saw a button : Ask Questions and clicked on it
* There was body field like “insert link,insert html,insert images”
* He put xss payloads there
* In “insert link”  he put xss payloads in link,title,tooltip field and he got XSS
	* Web Address
	* Text
	* Tooltip
	* There in above fields he putted payloads

Target link : https://social.microsoft.com/Forums/en-US/home
Payload : “`><img src=x onerror=prompt(document.domain)>`

---------------------------------------------------------------------------------
---------------------------------------------------------------------------------

#####2. https://medium.com/@saurabh5392/how-i-earned-by-finding-confidential-customer-data-including-plain-text-passwords-f93c4ce2631

How I earned $$$$ by finding confidential customer data including plain-text passwords! - By Sushant Soni

KeyPoints to Learn :->

* How directory indexing and file path traversal led to confidential customer data in plain sight.
* First step he used for find Subdomain
 * Using **AssetFinder** tool [https://github.com/tomnomnom/assetfinder] & **Httprobe** tool [https://github.com/tomnomnom/httprobe] mean finding subdomain using assetfinder tool and then output was fed for live subdomains using httprobe tool.
 * Using that approach he got one **domain**-> https://api.xxxx.com
* Then he did **Directory Searching** for searching Files/Directories using tool Dirsearch using seclists wordlists
* He got one result -> **https:/api.xxxx.com/application/logs** which was acessible and indexing was enabled
* Directory was log which **had logs data of old**, and checked all files specially recent ones
* He got one file in **compressed** version -> **log-09–09–2019.php.gz**
* After compressed he got all of the customer’s data like **email,phone number,credit card numbers,password and FB OAuth tokens**

---------------------------------------------------------------------------------
---------------------------------------------------------------------------------

#####3 https://medium.com/@sudhanshur705/story-about-my-first-bug-bounty-9fe710be8241

Story about my first bug bounty - By Sudhanshu Rajbhar
He found 2 DOM XSS in ucweb.com

KeyPoints to learn :->


* What he did is he checked scopes and policies of Alibaba websites and then he went to Youtube for searching bugs/pocs which are already found in Alibaba website so that he got idea about the target and what other’s found already in that site.
* He mostly see XSS pocs there.
* He picked some domains randomly liked -> 
		Alipay.com, ucweb.com and some more
* Now he did RECON Part started with https://virustotal.com/ to checking available subdomains and he started checking one by one
* Now he dig with ucweb.com subdomains and found this subdomain ->
		Samsung.ucweb.com
* That subdomain gave him 403 Forbidden
* At this stage normally person skip this part but this time he remembered this thing ->
`if you encounter any page like this google the site you may find a endpoint which is accessible.`

##### First XSS ->
* So, he used Google Dork -> 
		Site:samsung.ucweb.com
He got this result ->

![](https://miro.medium.com/max/1366/1*bSCqTJwlhu-SYlVYXBENrw.png)

* He opened URL ->
--	He started to testing that parameter
--	**title**  parameter was reflecting the input so he checked if any filter or something
--	He used **`<b></b>`** and found that there was no filter

![](https://miro.medium.com/max/1366/1*V4xY_c6wTcumgt2C8rMvfQ.png)

--	Then he used payload -> <script>alert(1)</script> but didn’t work but this payload worked ->
		`<img src=x onerror=alert(‘XSS’)>`

##### Second XSS ->

* He found xss on same domain. That website owner fixing the previous bug by removing that endpoint and if he open that endpoint he got 404 error found.
* He did **Discovery Content** using Dirbuster/Gobuster/Dirsearch but only Dirsearch worked for him
* **Dirsearch** gave result of Directory -> **/test/**
* Once he opened this -> http://samsung.ucweb.com/test/  he found this -> http://samsung.ucweb.com/test/classify.html?dataKey=New&title=    
mean same URL and once he used the same payload he got xss popup


Key things to learn here  is ->
**don’t forget to use directory bruteforce tools on the subdomains of your target**


--------------------------------------------------------------------
--------------------------------------------------------------------

#####4 https://blog.usejournal.com/reflected-xss-in-zomato-f892d6887147

Reflected XSS in Zomato - By Sudhanshu Rajbhar

KeyThing to learn from this Writeup ->

* He used **Wolframalpha** (https://www.wolframalpha.com/) which is used for subdomain enumeration
* He entered zomato.com there
* That site shows “Daily Visitors” of a particular subdomain, so this feature is really good for us if we are looking for a less visited areas.

![](https://miro.medium.com/max/527/1*mpyP--cLsJTUKfZ2yoCZmA.png)

* Out of above subdomains “**secretx.zomato.com**” is looking interesting
* When he opened that subdomain, he saw a **button** with **“sign in with zomato”**
 ![](https://miro.medium.com/max/459/1*JXLfYZ6ZFkIRLnI5hWTNJA.png)
* He checked source code first, but got nothing interesting [key point here is always checking source code]
* Once he clicked on the button, it redirected him to “**zomato.com**” and a box was there saying -> “SecretX Client wants to access you Zomato Account. Accept or Recject”

* So, he went back to website and from source code he took the URL -> https://auth2.zomato.com/oauth2/auth?response_type=code&client_id=80b39918-90be-49d2-ac52-4a8b1a25bcf1&redirect_uri=https%3A%2F%2Fsecretx.zomato.com%2Fuser%2Foauth2%2Fredirect_uri&state=2BHoBnVFFKP29L6SerHgEb7OCnBDPO
* This above URL was that which was getting load when the button was clicked
* Another thing I want to point here is we can intercept the request once he clicked on that button and copy that URL from Burpsuite
* He, **added** some **JUNK VALUES** to the **parameters** to **check** for any **oauth misconfiguration**, and he got some error

![](https://miro.medium.com/max/1366/1*ok_5xHf-rT8bPIcY0WlQPA.png)

* The parameter value was reflected on the page,source code, So he checked for <,> to see any filter there or not  

![](https://miro.medium.com/max/1366/1*UNe7QIF0GlqNOlUnjO8h0g.png)

* So, there was no filter
* But once he used xss payloads, that was not working because of WAF
* He checked for waf bypass reports for XSS and this payload worked for thim -> <center>`<marquee loop=1 width=0 onfinish=alert`1`>XSS</marquee>`</center>
* But still he didn’t able to access DOM because of alert(),confirm(),prompt() were getting blocked by WAF
* He even tried URL ENCODING
* He checked XSS Cheatsheet -> https://github.com/s0md3v/AwesomeXSS
* Payload from that cheatsheet -> co\u006efirm()
* That payload worked for him

`Prateek Tiwari mentioned that problem with HYDRA, which is an OAuth 2.0 and OpenID Connect Provider which zomato using for authentication on their service`

Side note from my side -> we can also use cheatsheet from ->
https://portswigger.net/research/one-xss-cheatsheet-to-rule-them-all
https://portswigger.net/web-security/cross-site-scripting/cheat-sheet

Report -> https://hackerone.com/reports/456333


Heyy there,

I have found a xss in auth2.zomato.com

Full url:
https://auth2.zomato.com/oauth2/fallbacks/error?error=xss&error_description=xss&error_hint=xss

Vulnerable Parameters: All available parameters are vulnerable

XSS Payload: `<marquee loop%3d1 width%3d0 onfinish%3dco\u006efirm(document.cookie)>XSS<%2fmarquee>`

Steps To Reproduce the xss

Just copy paste and load this url in your firefox browser and tadaa you will get the xss popup

`https://auth2.zomato.com/oauth2/fallbacks/error?error=xss&error_description=xsssy&error_hint=%3Cmarquee%20loop%3d1%20width%3d0%20onfinish%3dco\u006efirm(document.cookie)%3EXSS%3C%2fmarquee%3E`

Impact

An attacker can send this url with payload to an already login user and can steal the cookie.

From there -> https://medium.com/@sudhanshur705/cve-2019-8400-reflected-xss-in-ory-hydra-6b785000d9ff

Let’s move to POC

Just take an example this is the url:

`https://auth2.site.com/oauth2/auth?response_type=code&client_id=123dsds-90be-49d2-ac52-4a8b1a25bcf1&redirect_uri=https://site.com/user/oauth2/redirect_uri&state=2BHoBnVFFKP29L6SerHgEb7OCn21as`

Make some changes in the redirect_uri parameter value, and you will be redirected to an Error page.

`https://auth2.site.com/oauth2/fallbacks/error?error=xss&error_description=xss&error_hint=xss`


What I got idea is from here is to check subdomains and see if any subdomain use ORY Hydra service for authentication


----------------------------------------------------------------------
----------------------------------------------------------------------

######5. https://medium.com/@sudhanshur705/how-recon-helped-me-to-to-find-a-facebook-domain-takeover-58163de0e7d5

How Recon helped me to to find a Facebook domain takeover - By Sudhanshu Rajbhar
 
 
KeyLearn from Writeup ->
 
 
* Best way to find subdomain  : https://0xpatrik.com/asset-discovery/
		Horizonatl Domain Correlation <- he wrote this in his writeup

So, first lets know about this ->

* Vertical domain correlation — Given the domain name, vertical domain correlation is a process of finding domains share the same base domain. This process is also called subdomain enumeration 1.

*  Horizontal domain correlation — Given the domain name, horizontal domain correlation is a process of finding other domain names, which have a different second-level domain name but are related to the same entity 1



![](https://0xpatrik.com/content/images/2018/04/hvv.png)

Hmm nice, Now lets move to keypoint learn

* He first checked whois result of facebook.com
![](https://miro.medium.com/max/423/1*iHv1YMjJQYv7cNfTadZAhw.png)

* He saw -> Registrant Email: domain@fb.com
* We can use this email to find all the other sites which have the same registrant email as facebook.com
* For reverse WHOIS  he used -> https://tools.whoisxmlapi.com/reverse-whois-search  & https://viewdns.info/
* But  for the site viewdns-info  result is limited  
* For the same thing mean for Horizontal domain correlation we can use tools like **amass** or **domlink** 
* For now go to https://tools.whoisxmlapi.com/reverse-whois-search and in the search field, enter the email : **domain@fb.com**
* He got 2,756 Unique domains which all have “domain@fb.com”
* He said, not stop here, check for more domains now for **Registrant Name** which had Domain Admin and now see difference,
* Got 3,441 more domains
* Save all of those in one file and remove duplicate -> 
		sort filename | uniq |tee outputFileName 
* So , after sort and unique he got 4k unique domains  which have either  Facebook Inc or domain@fb.com
* After he collected all of the domains, he used tool -> **filter-resolved**  which is used to resolve all the domains
		cat fb2.txt | ~/tools/filter-resolved |tee live-domains.txt
* Now he used tool “**Subfinder**”  to find all the subdomains of the domains which were in live-domains.txt
		subfinder -dL live-domains.txt -o subdomains.txt
* He repeated the process like , he used **filter-resolved** tool again on subdomains.txt to resolve all the subdomains which got from **subfinder** tool.
* Now, after this , he used tool “**webscreenshot**” for taking screenshots of the subdomains
* He saw one screenshot from those screenshots and found this subdomain -> http://www.buckbuild.com/  which giving 404  error  [github page]
* Now, he read this article -> https://0xpatrik.com/takeover-proofs/
* After read that article  he uploaded something to verify the takeover was successful or not.
![](https://miro.medium.com/max/1366/1*dyHbTkdLflDzvFqu1cSH-Q.png)

![](https://miro.medium.com/max/1366/1*0LEfB3w1XP-u396G4LZ35A.png)

Takeover :->

Wow this site -> https://0xpatrik.com/takeover-proofs/
Is really cool, so to take over on subdomain we can use that site
Also finding subdomains with different styles in this writeup is really cool
