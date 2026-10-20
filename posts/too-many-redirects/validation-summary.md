# Validation summary

Status
validated

Date: 2026-10-20

Host: scaledpapers.com

## 1. One response, then the followed chain

```
curl -sI --max-time 20 http://scaledpapers.com/
HTTP/1.1 301 Moved Permanently
Location: https://scaledpapers.com/

curl -sI -L --max-redirs 8 --max-time 20 http://scaledpapers.com/
HTTP/1.1 301 Moved Permanently
HTTP/2 200

curl -sI --max-time 20 https://scaledpapers.com/
HTTP/2 200
```

`https://scaledpapers.com/` appears once, as the Location of the 301, and the followed chain ends on HTTP/2 200. No URL repeated.

Branch: the chain ends on a 200, and no URL repeats.
