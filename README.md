# Simple Rewrite Body

Simple Rewrite Body is a middleware plugin for [Traefik](https://github.com/traefik/traefik) which rewrites the HTTP response body by replacing a search regex by a replacement string.

## Configuration

### Static

```toml
[pilot]
  token = "xxxx"

[experimental.plugins.rewritebody]
  modulename = "github.com/demsking/traefik-plugin-rewritebody"
  version = "v0.3.1"
```

### Dynamic

To configure the `Simple Rewrite Body` plugin you should create a [middleware](https://doc.traefik.io/traefik/middlewares/overview/) in 
your dynamic configuration as explained [here](https://doc.traefik.io/traefik/middlewares/overview/).
The following example creates  and uses the `rewritebody` middleware plugin to replace all `foo` occurrences by `bar` in the HTTP response body.

If you want to apply some limits on the response body, you can chain this middleware plugin with the [Buffering middleware](https://doc.traefik.io/traefik/middlewares/http/buffering/) from Traefik.

```toml
[http.routers]
  [http.routers.my-router]
    rule = "Host(`localhost`)"
    middlewares = ["rewrite-foo"]
    service = "my-service"

[http.middlewares]
  [http.middlewares.rewrite-foo.plugin.rewritebody]
    # Keep Last-Modified header returned by the HTTP service.
    # By default, the Last-Modified header is removed.
    lastModified = true

    # Rewrites all "foo" occurences by "bar"
    [http.middlewares.rewrite-foo.plugin.rewritebody.rewrite]
      regex = "foo"
      replacement = "bar"

[http.services]
  [http.services.my-service]
    [http.services.my-service.loadBalancer]
      [[http.services.my-service.loadBalancer.servers]]
        url = "http://127.0.0.1"
```
