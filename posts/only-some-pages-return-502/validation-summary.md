# Validation summary

Status
validated

Date: 2026-10-20

Host: scaledpapers.com

## 1. Two live paths, then a missing one

```
curl -sI --max-time 20 https://scaledpapers.com/
HTTP/2 200
server: cloudflare

curl -sI --max-time 20 https://scaledpapers.com/blog/too-many-redirects/
HTTP/2 200
server: cloudflare

curl -sI --max-time 20 https://scaledpapers.com/no-such-path-502-check
HTTP/2 404
server: cloudflare
```

The homepage and the blog path both returned 200 from Cloudflare. The missing path returned 404, which is the server answering. No URL returned 502.

Branch: both live URLs are 200. This host is not a one-path 502 from here.
