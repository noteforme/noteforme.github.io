---
id: 67
title: HTTP
date: '2024-05-15T07:08:40+00:00'
author: Jon
layout: post
guid: 'https://jonblog.site/?p=67'
permalink: /2024/05/15/http/
categories:
    - NETWORK
---



## Post

https://jsonplaceholder.typicode.com/posts

request

```
:method: POST
:path: /posts
:authority: jsonplaceholder.typicode.com
:scheme: httpscontent-type: application/x-www-form-urlencodedcontent-length: 42
accept-encoding: gzipuser-agent: okhttp/5.0.0-SNAPSHOT
```

response

```
:status: 201
date: Wed, 17 Apr 2024 06:45:30 GMTcontent-type: application/json; charset=utf-8
content-length: 82
location: https://jsonplaceholder.typicode.com/posts/101
report-to: {"group":"heroku-nel","max_age":3600,"endpoints":[{"url":"https://nel.heroku.com/reports?ts=1713336330&sid=e11707d5-02a7-43ef-b45e-2cf4d2036f7d&s=Hua%2F7slOIpUhGTDPNaKu4xbP%2Bjuwplp9ehOh7C014ok%3D"}]}
reporting-endpoints: heroku-nel=https://nel.heroku.com/reports?ts=1713336330&sid=e11707d5-02a7-43ef-b45e-2cf4d2036f7d&s=Hua%2F7slOIpUhGTDPNaKu4xbP%2Bjuwplp9ehOh7C014ok%3D
nel: {"report_to":"heroku-nel","max_age":3600,"success_fraction":0.005,"failure_fraction":0.05,"response_headers":["Via"]}
x-powered-by: Express
x-ratelimit-limit: 1000
x-ratelimit-remaining: 999
x-ratelimit-reset: 1713336348
vary: Origin, X-HTTP-Method-Override, Accept-Encoding
access-control-allow-credentials: true
cache-control: no-cachepragma: no-cacheexpires: -1
access-control-expose-headers: Location
x-content-type-options: nosniffetag: W/"52-HKy3Gu5DcI5r2jj3f8TQwnYvlDs"
via: 1.1 vegurcf-cache-status: DYNAMICserver: cloudflarecf-ray: 875a73ddee780484-HKGalt-svc: h3=":443"; ma=86400
```

## GET

https://api.github.com/repos/square/okhttp/contributors

Request

```
:method: GET
:path: /repos/square/okhttp/contributors
:authority: api.github.com
:scheme: httpsaccept-encoding: gzipuser-agent: okhttp/5.0.0-SNAPSHOT
```

Response

```
:status: 200
server: GitHub.comdate: Wed, 17 Apr 2024 06:58:41 GMTcontent-type: application/json; charset=utf-8
cache-control: public, max-age=60, s-maxage=60
vary: Accept, Accept-Encoding, Accept, X-Requested-With
etag: W/"699b092aa0b0980f1b2f2bde5e810a75c5c1921ee2f53c4d9f28fc8ea5fca327"
last-modified: Wed, 17 Apr 2024 02:18:24 GMTx-github-media-type: github.v3; format=jsonlink: <https://api.github.com/repositories/5152285/contributors?page=2>; rel="next", <https://api.github.com/repositories/5152285/contributors?page=9>; rel="last"
x-github-api-version-selected: 2022-11-28
access-control-expose-headers: ETag, Link, Location, Retry-After, X-GitHub-OTP, X-RateLimit-Limit, X-RateLimit-Remaining, X-RateLimit-Used, X-RateLimit-Resource, X-RateLimit-Reset, X-OAuth-Scopes, X-Accepted-OAuth-Scopes, X-Poll-Interval, X-GitHub-Media-Type, X-GitHub-SSO, X-GitHub-Request-Id, Deprecation, Sunset
access-control-allow-origin: *
strict-transport-security: max-age=31536000; includeSubdomains; preloadx-frame-options: denyx-content-type-options: nosniffx-xss-protection: 0
referrer-policy: origin-when-cross-origin, strict-origin-when-cross-origincontent-security-policy: default-src 'none'
content-encoding: gzipx-ratelimit-limit: 60
x-ratelimit-remaining: 55
x-ratelimit-reset: 1713337679
x-ratelimit-resource: corex-ratelimit-used: 5
accept-ranges: bytesx-github-request-id: 200C:21C6BE:267A047:2720F58:661F7320
```