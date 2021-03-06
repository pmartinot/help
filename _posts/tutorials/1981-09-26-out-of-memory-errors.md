---
layout: post
template: two-col
title:  "Out of memory errors during deployment"
so_title: "memory"
date:   1905-09-26 15:33:13
categories: 
lead: You may not have enough memory during deployment
search-tags: ['']
tags: ['Troubleshooting']
tutorial: true
difficulty: 0
---

## The Problem
When you are deploying your stack (particularly in a non-development environment) asset pipeline compilation takes place during the deployment process.

If your server does not have sufficient memory available to perform the asset pipeline compilation you may receive one of the following errors:

- "Cannot allocate memory"
- "Killed"
- "Out of memory"

These are more likely on servers with low memory availability.
It is possible that your initial deployment succeeds, and subsequent deployments fail, and this is due to the fact that after your initial deployment you have additional memory usage of your web server.
You can also use a [manifest file](http://help.cloud66.com/building-your-stack/getting-started-with-manifest-files) to specify a value in MB for reserved&#95;server&#95;memory - this may help with Passenger-based stacks by preventing Cloud 66 from allowing passenger to allocate additional processes.

<div class="notice">
    <h3>Note</h3>
    <p>Irregular vendor issues in memory allocation (like greedy neighbours on the same physical instance) could also cause this issue, though that very vendor/infrastructure specific.</p>
</div>

## Possible Resolutions
<ol class="article-list">
<li>Compile the assets on your own box and disable asset pipeline compilation on the stack going forward.</li>
<li>Configure your code not to use asset pipeline pre-compilation and use live compilation (on-demand). <a href="http://help.cloud66.com/building-your-stack/asset-pipeline-compilation">More information on Asset Pipeline compilation</a>.</li>
<li>Resize your box to a bigger box either via a new stack, or <a href="http://help.cloud66.com/managing-your-stack/scaling">vertical scaling</a> if available.</li>
<li><a href="https://www.digitalocean.com/community/articles/how-to-add-swap-on-ubuntu-12-04">Setup swap files on your server</a>. This is automatically done for 512MB and 1GB DigitalOcean servers.</li>
<li>Manually reduce memory usage on your server before deployments (ie. manually stop your webserver).</li>
<li>Reduce memory usage on your server by limiting Passenger memory usage (using a <a href="http://help.cloud66.com/building-your-stack/getting-started-with-manifest-files">manifest file</a> to specify a value for reserved&#95;server&#95;memory).</li>
</ol>