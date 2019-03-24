---
layout: post
title:  Bug-Hunting-Tips/Tricks
date:   2019-3-24 
categories: Bug-Hunting-Tips
comments: false
---

Here I will write all the tips tricks i got from twitter and even advices which when i got from others and daily update this blog post

## Bug Hunting Tip 1

Functions that handle paths,links and domains as part of a parameter or input are a high priority to Bug Hunters. 
They are often subject to SSRF,Open Redirect,Path Traversal or LFI

Examples ->
* https://someapp.com/src?redir=something.com
* https://someapp.com/src?site=http://something.com
* https://someapp.com/src?dir=/path/to/something

=========================

## Bug Hunting Tip 2

