---
title: Printing Raw HTTP Requests / Responses in C#
date: 2021-02-06 11:34:00
tags:
 - .NET
 - C#
---

When debugging web services in .NET, I have an occasional need to print raw HTTP requests and responses. Tools like Fiddler are very helpful for this purpose, but a bug can still occur in cloud environments where Fiddler cannot capture traffic.

I am surprised C# does not have built-in methods to print raw HTTP request and response strings. The HttpResponseMessage class, for example, has a ToString() method that will return most response properties and headers. But the returned string is not in an HTTP message format, and the response body is omitted entirely.

I haven’t found an elegant solution for this problem on answer sites like StackOverflow, so I decided to implement my own. The Gist below contains extension methods to print raw HTTP requests and responses. One file is server-side using ASP.NET Core. The other is client-side using HttpClient.

<script src="https://gist.github.com/jdbrowndev/15d00435d9ce87f2c4040448be3d01ce.js"></script>

Here is an example output showing a request and response to my website:

```
Request:

GET https://www.jordanbrown.dev/ HTTP/1.1
traceparent: 00-0820ddd5f231ed40968349c797de441b-9223fee0b518fb44-00

Response:

HTTP/1.1 200 OK
Connection: keep-alive
Server: GitHub.com
Access-Control-Allow-Origin: *
ETag: "5f2e08f8-1625"
Cache-Control: max-age=600
x-proxy-cache: MISS
X-GitHub-Request-Id: AED0:0390:2A7A99:33E790:601EBC83
Accept-Ranges: bytes
Date: Sat, 06 Feb 2021 15:57:55 GMT
Via: 1.1 varnish
Age: 0
X-Served-By: cache-pwk4954-PWK
X-Cache: MISS
X-Cache-Hits: 0
X-Timer: S1612627075.415438,VS0,VE23
Vary: Accept-Encoding
X-Fastly-Request-ID: 71b3b2eb2825de941124dd3920903e869a3254df
Content-Length: 5669
Content-Type: text/html; charset=utf-8
Last-Modified: Sat, 08 Aug 2020 02:07:52 GMT
Expires: Sat, 06 Feb 2021 16:07:55 GMT

(HTML document would be printed here)
```

I can’t guarantee every request and response will be printed exactly to the HTTP specification. Moreover, for obvious security reasons, you should not log request and response bodies in production environments. But this code should work nicely for troubleshooting development environments.