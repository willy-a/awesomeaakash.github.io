---
layout: post
title:  Bug-Hunting-Guide"
date:   2019-3-20 
categories: Guide
comments: true
---

Today I read Pentester Land Podcasts →

 [https://pentester.land/podcast/2019/03/18/the-bug-hunter-podcast-04-how-to-think-out-of-the-box.html](https://pentester.land/podcast/2019/03/18/the-bug-hunter-podcast-04-how-to-think-out-of-the-box.html)

It was amazing and I got many Ideas and learned so many things from this post.

So, I thought to create  a blog about it for newcomers and for existing bug hunters who recently started Bug Hunting like me

Here is the all the key points to get started in Bug Hunting →

1. Undertand 
    1. HTTP/S , 
    2. URL,
    3. Request,
    4. Response, 
    5. Session,
    6. Cookie,
    7. Burpsuite Features, 
    8. How website works?,
    9. How vulnerability detection tools work?
    10. SOP
    11. CORS
    12. HTTP Headers
    13. Basic concepts of Vulnerabilities
2. The Best tool for Bug Hunters is
    1. Clear Concepts of basic things which I already wrote above
    2. Brain
    3. Always be positive even from failure things
    4. Eagerness to Learn and explore more about bugs etc
    5. Patience
    6. Taking time in Recon as much as we can
    7. Taking different path from others
    8. Burpsuite and Browser
    9. Automation the boring stuffs by build own tools
3. Making own checklist and cheatsheets
    - But from podcast I got to know that no time waste much on this.

    But WHY ?

    ANSWER :→ 

    Purpose of Every website/Target is different but all are build on some programming Languages.

    And having same things almost like Some have → 
     Register/Login/Search/Comment/Forget/Remember me etc etc

    So , we all know these things very well, Thats why instead of making checklist spend time with target and explore every functionality

    Burpsuite and Browser is the best buddy for this

    Check all of those areas. All we need is to play with those fields and try for Hit & Trial error methods

    Example →

      target.com/anything?param=1&param=2

    So, just try Hit and Trial things there by changing parameters or add more parameters.

    All we need is to analyze Requests and Response

4. Practice all bugs/vulnerabilities on lab first to learn concepts about vulnerability. Figure out how to detect those vulnerabilities by learn about them
5. Build Cheatsheets or focus on readymade cheatsheets by others like OWASP Checklist
6. Learn to chain small bugs to higher bugs
7. Zseano Sir adviced to also focus on mobile website too to find bugs
8. Read writeups and learn from it by making notes and then Practice on it first in lab then on real Target 
9. To master on finding vulnerabilities first Pick a subject, Learn as much as we can about it, read writeups/blogs/whitepapers and watch video  from various source and practice on them and apply idea on that. Credit :- somdev sangwan
10. Follow OWASP TOP 10 Vulnerabilities Lists & Cheatsheets
11. Follow Pentester Land and Pranav's Newsletter

Ok, but wait, From past some days or can say week i made my own checklists.

Here is my checklists →

Checklists divided on 2 Parts → 1. Recon Part  2. Attacking Part

- [ ]  1.  Recon Part
- [ ]  Pick the target [Specially which have broad/Large Scopes.
- [ ]  Read their policies carefully
- [ ]  Find all subdomains
    1. Tools
        1. sublist3r
        2. amass
        3. aquatone
        4. subfinder
        5. subbrute
        6. knockpy
        7. Google Dorks
        8. Certificate Transparency
        9. Merge all results in one file and analyze them one by one of keen interest

- [ ]  Explore man target and subdomains all side by side
    - [ ]  Check robots.txt
    - [ ]  Check source code
    - [ ]  Check for which Client Side Code is used
- [ ]  Discover
    1. Directories [ gobuster,dirsearch,dirb,dirbuster,burp spider]
    2. All Entry points,Parameters,Hidden Links [ Arjun and Burp]
        1. Common Entry Points
            1. Login/Register 
            2. Search Box
            3. Comment Section
            4. Feedback Section
            5. Contact Us Pages
            6. GET & POST Requests
            7. Cookies
            8. Find out hidden Post Parameters and their Usage
            9. File Uploads
            10. Import things
    3. Domain Discovery like Email etc [ Domlink, JSfiles]
- [ ]  Subdomain Enumeration [ Check Shankar R medium post on this]
- [ ]  Port Scanning
    1. Nmap
    2. masscan
    3. aquatone
- [ ]  Visual Identification - Eyewitness [ Tip Amass + eyewitness]
- [ ]  Wayback Enumeration [ Must]
- [ ]  Know what CMS the website using [ whatweb,wappalyzer]
- [ ]  Github Recon and AWS Recon

2. Attacking Part →

1. Client Side Vulnerabilities
2. Server Side Vulnerabilities
3. API Testing

- [ ]  SQLI
- [ ]  RCE
- [ ]  SSRF
- [ ]  LFI/RFI
- [ ]  Path Traversal
- [ ]  XSS
- [ ]  CSRF
- [ ]  Session Related
    - [ ]  Cookie
        - [ ]  Sensitive info over cookie
        - [ ]  Cookie with httponly flag
        - [ ]  Cookie with no secure flag
        - [ ]  Path attribute not set in session cookie
        - [ ]  Apache HTTPOnly cookie disclosure
    - [ ]  Session
        - [ ]  Session Prediction
        - [ ]  Session randomness
        - [ ]  Session Expiration Mechanism legitimate?
        - [ ]  Session Hijacking
        - [ ]  Session token to URL/GET
        - [ ]  Check Session cookie before and after Login/Register
            - [ ]  If static - Vulnerable
            - [ ]  If not static - No Vulnerable
    - [ ]  Log Out Functionality
        - [ ]  Does Session Expire?
        - [ ]  Session Relay
        - [ ]  After logout test check temp folder for cache and sensitive info stored.
    - [ ]  Strict Transportation Policy Checking

- [ ]  Logical Flaws →
    1. Password Change/Forgetten Function
    2. Bruteforcable Login
    3. Verbose Failure Messages
    4. Insecure Credential Discription
    5. Predicatable Username/Initial Pass
    6. Password Account Actication Link /ID   
    7. Bad Password
    8. Fail-Open Login
    9. Defective Multi-Stage Login
    10. OAuth Flaws

- [ ]  OAuth Related
- [ ]  File Upload
    - [ ]  Upload security bypass via test.txt.jpg
    - [ ]  Upload test.aspx;.jpg | test.php;.jpg
    - [ ]  Change Extension to Capital and small letter in order to bypass
    - [ ]  Captcha bypass
- [ ]  SSTI
- [ ]  SAML
- [ ]  Cache Deception
- [ ]  Testing on Registration Page
    - [ ]  Captcha testing
    - [ ]  File uploading there?
    - [ ]  Weak Password Policy
    - [ ]  Stored XSS
    - [ ]  Any information leak in browser history
    - [ ]  Email or any type of verification bypass

There is huge list 

Now i am thinking to make my own Newsletter similarly almost PentesterLand and Pranav

Newsletter type → Vulnerability Newsletter

Vulnerability Newsletter :→

1. Vulnerability Names
    1. Blogs/Writeups/Resources I read
    2. About Vulnerability
    3. Mind Map/Dectection of Vulnerability
    4. Practice Area
    5. Tools
    6. Video I watch
    7. Tips/Tricks 

    Resources →

    [https://pentester.land/podcast/2019/03/18/the-bug-hunter-podcast-04-how-to-think-out-of-the-box.html](https://pentester.land/podcast/2019/03/18/the-bug-hunter-podcast-04-how-to-think-out-of-the-box.html)

    [https://blog.usejournal.com/bug-hunting-methodology-part-2-5579dac06150](https://blog.usejournal.com/bug-hunting-methodology-part-2-5579dac06150)

    [https://medium.com/@trapp3rhat/bug-hunting-methodology-part-1-91295b2d2066](https://medium.com/@trapp3rhat/bug-hunting-methodology-part-1-91295b2d2066)

    [https://github.com/awesomeaakash/CheatSheetSeries](https://github.com/awesomeaakash/CheatSheetSeries)

    [https://github.com/awesomeaakash/Web-Security-Learning](https://github.com/awesomeaakash/Web-Security-Learning)

    [https://github.com/awesomeaakash/bugbounty101](https://github.com/awesomeaakash/bugbounty101)

    [https://github.com/ghsec/](https://github.com/ghsec/)
    
    [https://hacker101.com](https://hacker101.com)

    [http://10degres.net/vulnerabilities-list/](http://10degres.net/vulnerabilities-list/)
         
    [https://jdow.io/blog/2018/03/18/web-application-penetration-testing-methodology/](https://jdow.io/blog/2018/03/18/web-application-penetration-testing-methodology/_

    [https://docs.google.com/document/d/1eVPh6jNn3jZbnHZitevbSSe9GDoi7PmrolfGv7FQdow/edit?fbclid=IwAR088EIT-lXLDgVGLuw1MrGP_hb3GrmN0KHP3z5v_6Lh6ihskbW5wOBJ-fo](https://docs.google.com/document/d/1eVPh6jNn3jZbnHZitevbSSe9GDoi7PmrolfGv7FQdow/edit?fbclid=IwAR088EIT-lXLDgVGLuw1MrGP_hb3GrmN0KHP3z5v_6Lh6ihskbW5wOBJ-fo)
    
