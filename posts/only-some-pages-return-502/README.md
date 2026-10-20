---
title: Only some pages return 502
description: "The homepage is 200 and one path is 502. Compare the two responses and stop. The proxy is up. That path's upstream is not."
date: "2026-10-20"
validated: "2026-10-20"
---

The homepage loads. Checkout, or one API route, returns 502. The name resolved, the port opened, and the certificate was accepted. Restarting the whole server skips the only fact you have.

## 1. Curl the page that works and the page that does not

Run both from the machine where you are debugging. Use the path customers actually opened.

```
curl -sI --max-time 20 https://example.com/
curl -sI --max-time 20 https://example.com/checkout
```

Read the two status lines before you open a process manager.

- **Both are 502, or either command fails before a status line.** This is not a one-path failure. Walk the name, the port, and the certificate in [DNS works, the site does not](/blog/dns-works-the-site-does-not).
- **The homepage is 200 and the other URL is 502.** DNS, the port, and TLS worked for this host. The proxy in front answered. The next hop for that path did not. Stop looking at the domain.
- **Both are 200, and the browser still shows 502.** `curl` sent no cookies. The failure is attached to the browser's request. Stop before restarting the app.
- **The other URL is 404.** You asked for a path the server does not have. That is not a 502. A missing page is up on a Website check, as [When 403 means up](/blog/when-403-means-up) lists it.

A 301 to add a trailing slash is a hop, not a bad gateway. Follow it, then read the status of the URL you land on. A chain that repeats a URL is [Too many redirects](/blog/too-many-redirects).

## 2. Name the hop that failed

On the 502 response, the `Server` header is the proxy that gave up: Cloudflare, nginx, a load balancer. A 502 means that proxy did not get a valid response from the process behind this path.

The homepage's 200 means that proxy is running. Restarting it does not start the upstream this path uses. The fix is the process, the timeout, or the route that only this path is sent to.

A 504 is the same split with a different ending: the proxy waited and gave up. The homepage still proves the proxy itself answered.

## 3. Check the URL that failed

A check on `/` stays green through this outage. Add a Website or API check on the path that returned 502. 502 is down for both. What each code means is in [When 403 means up](/blog/when-403-means-up), and the form is [Add a check](/docs/checks).

Leave the confirmation period at three failures. One 502 is stored. It does not open an incident.
