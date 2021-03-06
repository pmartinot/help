---
layout: post
template: two-col
title:  "SMTP timeouts on Amazon AWS EC2"
so_title: "smtp"
date:   1900-11-06 15:33:13
categories: 
lead: Amazon AWS EC2 enforces traffic throttling on SMTP causing intermittent timeouts when sending email.
search-tags: ['']
tags: ['Troubleshooting']
tutorial: true
difficulty: 0
---


## Problem
Sending emails with a service like [Mandrill](http://mandrill.com) or through a normal SMTP server from your Amazon AWS EC2 instances can result in intermittent timeouts.

This is because of a [traffic throttling policy](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/smtp-connect.html) on the AWS side to prevent spamming from their servers.

## Resolution
You have the following options:

<ol class="article-list">
<li>Use a different port other than 25.</li>
<li>If using Mandrill, use their RESTful API, instead of standard SMTP. This might not be very straight forward as the <a href="https://mandrillapp.com/api/docs/index.ruby.html">Mandrill gem</a> is not a drop-in replacement.</li>
<li>Use another service like <a href="https://postmarkapp.com">Postmark</a> where they have a drop-in gem which uses their RESTful API.</li>
</ol>