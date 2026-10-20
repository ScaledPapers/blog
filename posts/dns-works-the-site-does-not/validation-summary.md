# Validation summary

Status
validated

Date: 2026-10-20

Host: scaledpapers.com

## 1. DNS

The three resolvers returned the same two addresses. The order differed. The post treats a rotating CDN answer as agreement, not a split.

```
dig +short scaledpapers.com A
104.21.20.57
172.67.191.206

dig +short scaledpapers.com A @1.1.1.1
172.67.191.206
104.21.20.57

dig +short scaledpapers.com A @8.8.8.8
104.21.20.57
172.67.191.206
```

Branch: they agree.

## 2. TCP 443

```
nc -vz scaledpapers.com 443
Connection to scaledpapers.com port 443 [tcp/https] succeeded!
```

Branch: succeeded.

## 3. Certificate and status

```
curl -sI --max-time 15 https://scaledpapers.com/
HTTP/2 200
```

Headers came back, so TLS succeeded for this name. The status line is HTTP/2 200.

Branch: response headers came back, status 200.
