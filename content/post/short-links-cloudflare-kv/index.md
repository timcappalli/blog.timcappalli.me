---
title: Creating a short link service using Cloudflare Workers KV
description: A guide on creating a basic short link / redirect service for free using Cloudflare Workers KV
slug: short-links-cloudflare-kv
draft: false
date: 2024-09-06T01:20:10.466Z
readingTime: true
license: false
toc: false
# image: 
categories:
    - How To
tags:
    - Utility
    - Web Service
    - Cloudflare
keywords: 
    - cloudflare workers
    - cloudflare kv
    - short link 
    - bit.ly
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---

## Overview

Sometimes it can be helpful to have short, stable links for web resources. The commercial "bit.ly" service made these popular many years ago.

I use these short links often in my presentations to add reference links using the " tcslides.link/{shortname} " namespace. I also use them when I know content will be moving in the future.

This guide outlines a quick method for establishing a free, easy-to-configure, and easy-to-maintain short link redirect service utilizing Cloudflare Workers and KV. The Worker is used to process the incoming request and provide a response. KV is used to store the short code to destination URL mapping as key-value pairs.

While this approach offers simplicity and cost-effectiveness, it's important to note that it lacks granular analytics on a per-link basis. Although Cloudflare provides information on the overall number of Worker executions, specific link performance metrics are not available. For my use case, this is not a concern.

## Pre-Requisites

This guide assumes:

- you already have a Cloudflare account
- you already have a domain
- that domain is already set up in Cloudflare

## Steps

### 1: Create a Workers application

Navigate to "Workers & Pages", and click **Create**.

{{<screenshot image="cfaka-1-workeroverview.png" alt="A screenshot of the Cloudflare dashboard showing the Workers & Pages overview screen">}}

On the "Workers" tab, click **Create Worker**.

{{<screenshot image="cfaka-2-createapp.png" alt="A screenshot of the Cloudflare dashboard showing an empty Create an application screen">}}

Give the worker a friendly name and click **Deploy**.

{{<screenshot image="cfaka-3-deployscript.png" alt="A screenshot of the Cloudflare dashboard showing the confirmation screen prior to deployment of a worker">}}

### 2: Set Up KV

Now it's time to set up KV.

In the main menu on the left, navigate to _Workers & Pages_ > _KV_, and click **Create a namespace**.

{{<screenshot image="cfaka-4-newkv.png" alt="A screenshot of the Cloudflare dashboard showing the Workers & Pages KV screen with a Create a namespace button">}}

Give the namespace a name, and click **Add**.

{{<screenshot image="cfaka-5-createkv.png" alt="A screenshot of the Cloudflare dashboard showing the Create a namespace section, with a text field for the namespace name, a Cancel button and a Add button">}}

Now navigate back to the Worker (_Workers & Pages_ > _Overview_ > _{Worker Name}_ ).

On the _Settings_ tab, go to _Variables_, scroll down to _KV Namespace Bindings_, and click **Add binding**.

{{<screenshot image="cfaka-6-variablesmenu.png" alt="A screenshot of the Cloudflare dashboard showing the Settings menu for the worker with a left menu with Variables selected and a section showing KV Namespace Bindings and a Add binding button">}}

Add a variable name and select the KV Namespace that you just created. Next to the **Deploy** button, click the downwards arrow, and click the **Save** menu item, then the **Save** button.

{{<screenshot image="cfaka-7-mapsave.png" alt="A screenshot of the Cloudflare dashboard showing the KV Namespace Bindings configuration with an input field for Variable name, with the text redirects, and a dropdown field named KV Namespace with aka-redirect selected. There is a Cancel button and a Deplot button with an arrow which is selected, and the Save option is selected from the menu">}}

### 3: Set up the redirection code

Now lick the **< / > Edit Code** button at the top right.

Copy the code below into the editor.

```js
export default {
  async fetch(request, env) {
    const url = new URL(request.url);
    const shortname = url.pathname.slice(1).toLowerCase()

    try {
      const value = await env.redirects.get(shortname)
      if (value === null) {
        return Response.redirect("https://blog.timcappalli.me", 301)
      }
      return Response.redirect(value, 302)
    } catch (err) {
      return Response.redirect("https://blog.timcappalli.me", 301)
    }
  }
};
```

Replace `redirects` on line 7 (`env.redirects.get`) with the variable name you configured in the KV binding.

Replace both instance of `https://blog.timcappalli.me` with the website that you'd like to be the default redirect destination if there is either an error or no short link match.

Click **Deploy** at the top right, then **Save and deploy** in the dialog.

{{<screenshot image="cfaka-8-deploycode.png" alt="A screenshot of the Cloudflare dashboard showing an instance version, the text latest, and a Deploy button">}}

### 4: Add entries

To add entries to KV, from the main menu on the left, navigate to _Workers & Pages_ > _KV_ and click **View** for the KV name

Add the short name as "Key" and the destination URL as "Value", then click **Add Entry**.

{{<screenshot image="cfaka-9-kventry.png" alt="A screenshot of the Cloudflare dashboard showing the KV entry screen with an input field named Key with the value thispost and another input field named Value with the value https://blog.timcappalli.me/p/short-links-cloudflare-kv">}}

### 5: Test

Now you can test! Go back to your Workers main page, and right click on the **Visit** link at the top right and copy the link address.

{{<screenshot image="cfaka-10-test.png" width="50%" alt="A screenshot of the Cloudflare dashboard showing a link with the text Visit, and the browser right click menu with Open Link Address highlighted">}}

Open a new tab, paste this link, and add the short name that you just added to KV, to the end of the URL. 

Hit enter, and you should be redirected to the destination you defined!

### 6: Bind to your domain

Lastly, you'll want to set use your own domain for the Worker.

In the Worker, go to _Settings_ > _Triggers_ and click the **Add Custom Domain** button.

Add the domain, verify the DNS record looks OK, then click **Add Custom Domain**.

{{<screenshot image="cfaka-11-domain.png" alt="A screenshot of the Cloudflare dashboard showing the Add Custom Domain section with an input field named Domain with the value aka.timcappalli.me and a table previewing the DNS entry, a Cancel button and a Add Custom Domain button">}}

Wait a few minutes and then it should be live!

Hope this was helpful!