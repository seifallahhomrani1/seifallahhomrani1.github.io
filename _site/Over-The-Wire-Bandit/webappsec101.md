---
title : 'TryHackMe: Lazy Admin'
author : Seif-Allah
description : In this room, we will walk through how to testing an application in the perspective of a hacker/penetration tester.
layout : post
category : CTF-WriteUp
image : /assets/images/writeups/thm/webappsec101/index.png
toc : true
---


## Introduction

This room is a small vulnerable web application. In the [OWASP Juice shop], we looked at how some basic vulnerabilities worked. In this room, we'll walk though the methodology and approach of testing a web application. As an ethical hacker, you need to test the web application from the perspective of an attacker. We'll be using this mindset to establish a strategy to look for weaknesses in the web application.

## Web Crawlers

The crawl phase of a scan involves navigating around the application, following links, submitting forms, and logging in where necessary, to catalog the content of the application and the navigational paths within it.

To read more about web crawlers follow this [link](https://portswigger.net/burp/documentation/scanner/crawling)