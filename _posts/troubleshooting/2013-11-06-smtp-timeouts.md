---
layout: post
title:  "SMTP Timeouts on Amazon AWS EC2"
date:   2013-11-06 15:33:13
categories: Troubleshooting
---

<p class="lead">Amazon AWS EC2 enforces traffic throttling on SMTP causing intermittent timeouts when sending email.</p>

## Problem
Sending emails with a service like [Mandrill](https://mandrillapp.com) or through a normal SMTP server from your Amazon AWS EC2 instances can result in intermittent timeouts.

This is because of a [traffic throttling policy](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/smtp-connect.html) on the AWS side to prevent spamming from their servers.

## Resolution
You have the following options:

1. Use a different port other than 25.
2. If using Mandrill, use their RESTful API, instead of standard SMTP. This might not be very straight forward as the [Mandrill gem](https://mandrillapp.com/api/docs/index.ruby.html) is not a drop-in replacement.
3. Use another service like [Postmark](https://postmarkapp.com) where they have a drop-in gem which uses their RESTful API.