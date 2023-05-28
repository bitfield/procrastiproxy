# A procrastination-blocking web proxy

Self-discipline is a wonderful thing, but sometimes we could all use a little help. Despite the best intentions, we can end up spending time on websites we didn't intend to, and frittering away our precious lives in endless scrolling.

Wouldn't it be great if your computer could protect you from this by banning you from certain websites when you ask it to? Well, that's your task today: to write a proxy server in Go that will block requests to a given list of sites or domains.

For example, if you find that too much of your time is spent on Reddit, you could configure the proxy to block all URLs matching `reddit.com`. You can then set your browser to use the proxy, and it will help reinforce your willpower throughout the day.

## 1. Blocking everything

This is actually the easiest to do. Write a program that listens on a specified port for HTTP requests, and responds to all of them with status 403 (Forbidden).

Test your server not just by making HTTP requests to it (though that's a good start), but also by using an HTTP client configured with the proxy. For example:

```go
client := &http.Client{
    Transport: &http.Transport{
        Proxy: http.ProxyURL(&url.URL{
            Scheme: "http",
            Host:   "localhost:XXXX",
        }),
    },
}
resp, err := client.Get("some URL")
```

## 2. Allowing everything

Instead of responding to all requests with 403, have the proxy _make_ the request, and return the response to the user.

## 3. Static blocklist

Have the proxy block requests if they match some global, static blocklist, but allow them otherwise.

## 4. Configurable blocklist

Allow the proxy server administrator to configure the blocklist when starting the server (perhaps by passing it in as a parameter, for example).

## 5. Dynamic blocklist

Add a special 'admin' endpoint to the proxy server, so that administrators can connect to it via HTTP and block or unblock specific domains.

For example, they might make a request like this:

```
/admin/block/reddit.com
```

or

```
/admin/unblock/facebook.com
```

Test that the proxy server responds correctly when its blocklist is changed. Make sure that the blocklist is concurrency-safe, so that no data races occur if two simultaneous requests try to update it.

## 6. Office hours

Add an 'office hours' feature, so that the proxy's blocklist is only enforced during certain hours (9am to 5pm, for example). The exact times should be configurable by the administrator when the server is started.

## 7. Productisation

Add the remaining bits and pieces you'll need to make this a shipping product, including TLS and websocket support, fixing weird edge cases and errors, documentation, release automation, and so on.

## 8. Extensions

Add whatever features you would like, or think would be useful.

Some ideas:

* Content-aware blocking (for example, block any web page containing certain words)
* Per-user blocklists (for example, user `bukola` might be allowed access to Reddit, but user `john` might not)
* Time-limited blocking (for example, you might be allowed 15 minutes a day on Reddit, but no more)
* Warning-only mode (for example, when browsing a blocked site, you might see a warning message at the top of the page: "This site can seriously damage your free time.")
* Content caching
