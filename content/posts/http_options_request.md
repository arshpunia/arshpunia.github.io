---
title: "CORS, Preflight Requests, and OPTIONS Method"
date: 2022-04-24T09:05:07-04:00
draft: false
---



## Background ##

In working on a .NET backend recently, we ran into the following error whenever a request was made from the browser: 

```
Cross Origin Rrequest Blocked: The Same Origin Policy disallows reading the remote resource at <backend-deployment-url>.
(Reason: CORS header 'Access-Control-Allow-Origin' missing). Status Code: 405.
```

It is worth noting that the GET/POST requests were wroking fine using `curl`.

## CORS, Preflight Requests, and the curious case of 200 OK ##

From the [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

> Cross-Origin Resource Sharing (CORS) is an HTTP-header based mechanism that allows a server to indicate any origins (domain, scheme, or port) other than its own from which a browser should permit loading resources. 

> An example of a cross-origin request: the front-end JavaScript code served from https://domain-a.com uses `XMLHttpRequest` to make a request for https://domain-b.com/data.json.

> For security reasons, browsers restrict cross-origin HTTP requests initiated from scripts. For example, `XMLHttpRequest` and the `Fetch API` follow the same-origin policy. This means that a web application using those APIs can only request resources from the same origin the application was loaded from unless the response from other origins includes the right CORS headers.

In other words, the browser will restrict HTTP requests to your server if:
- Your server is hosted on a different domain than the one that served the web-page, AND
- Said server does not explicitly allow cross-origin requests.

With the exception of a few [cases](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#simple_requests), the browser normally checks if the server supports cross-origin requests by using Preflight requests. Simply put, a preflight request is an HTTP OPTIONS request sent to the server to determine whether or not the server allows cross-origin requests from the requesting domain [^1]. 

This is where things can get _slightly_ tricky. If the server supports CORS from the requesting domain, it returns a `200 OK` response and sets the CORS headers. The browser sees the headers and does not restrict future cross-origin requests to said server. If, however the request is denied, the server still returns a `200 OK` response, albeit without the requisite CORS headers. The browser subsequently restricts any GET/POST/PUT/DELETE requests to that server and you get the error message above. 

## The Fix ##

Because this was a university project, and the deadline was less than 6 hours away, we went brute-force and added the following snippet to our `Program.cs` file: 

```
builder.Services.AddCors(options =>
{
    options.AddDefaultPolicy(policy =>
        {
            policy.WithOrigins("*");
        });
});
```

This allowed the browser to load resources from the backend server and the application was behaving as expected.

The above snippet is by no means a safe option. For a production-like environment, you may want to exercise a finer level of control as described in the Microsoft documentation [here](https://docs.microsoft.com/en-us/aspnet/core/security/cors?view=aspnetcore-6.0#preflight-requests).

Special thanks to Shawn Verma and Tyler Thomson for their help in identifying and resolving the issue. 

[^1]: It is important to remember that preflight requests are sent **before** any cross-origin GET/POST/PUT/DELETE can be made. 