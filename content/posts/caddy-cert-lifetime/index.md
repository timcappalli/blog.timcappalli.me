---
title: Increasing the certificate lifetime for the Caddy internal CA
description: A very short guide on increasing the leaf certificate lifetime for Caddy's internal CA.
summary: A very short guide on increasing the leaf certificate lifetime for Caddy's internal CA.
slug: caddy-cert-lifetime
draft: false
date: 2025-07-09T12:16:07.511Z
showTableOfContents: false
categories:
    - How To
tags:
    - Caddy
    - TLS
    - Web
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---

## Overview

Caddy's docs are excellent, but it's easy to get lost in all the details. The default certificate lifetime for the internal Caddy CA is 12 hours and I wanted to increase it to 48 hours (2 days). I could not find a good example of a working configuration, so hopefully this post helps someone else!

## Configuration

You'll need a `tls` directive with an `issuer internal` subdirective. You cannot use the `tls internal {}` shorthand. This is what tripped me up.

In this example, I'm using snippets, which allows the configuration to be reused for individual site/service definitions (via `import`).

```shell
(tls-int-48h) {
  tls {
    issuer internal {
      ca local
      lifetime 48h
    }
  }
}

login.example.com:443 {
  import tls-int-48h
  reverse_proxy 127.0.0.1:3001
}
```
