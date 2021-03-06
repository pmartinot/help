---
layout: post
template: two-col
title:  "Errors due to different group configs in database.yml"
so_title: "database.yml"
date:   2039-09-26 15:33:13
categories: 
lead: Errors can occur if your specified adapter is different between your "development" and "test" groups
search-tags: ['']
tags: ['Troubleshooting']
tutorial: true
difficulty: 0
---


## The basics

Errors can occur during deployments due to there being different adapters defined in the "development" and the "test" groups in your database.yml file.
Your error will differ depending on the adapters you've chosen.

For example, if your database.yml file's "development" group contains:

<pre class="prettyprint">
adapter: postgresql
</pre>

And it also contains a "test" group with:

<pre class="prettyprint">
adapter: mysql2
</pre>

This will result in the following slightly obtuse error during your code deployment:

<pre class="prettyprint">
uninitialized constant Mysql2
</pre>