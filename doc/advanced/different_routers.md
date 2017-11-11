# Different Routers

Reitit ships with several different implementations for the `Router` protocol, originally based on the [Pedestal](https://github.com/pedestal/pedestal/tree/master/route) implementation. `router` function selects the most suitable implementation by inspecting the expanded routes. The implementation can be set manually using `:router` option, see [configuring routers](advanced/configuring_routers.md).

| router                        | description |
| ------------------------------|-------------|
| `:linear-router`              | Matches the routes one-by-one starting from the top until a match is found. Works with any kind of routes. Slow, but works with all route trees.
| `:lookup-router`              | Fast router, uses hash-lookup to resolve the route. Valid if no paths have path or catch-all parameters and there are no [Route conflicts](../basics/route_conflicts.md).
| `:mixed-router`               | Creates internally a `:prefix-tree-router` and a `:lookup-router` and used them to effectively get best-of-both-worlds. Valid only if there are no [Route conflicts](../basics/route_conflicts.md).
| `::single-static-path-router` | Super fast router: sting-matches the route. Valid only if there is one static route.
| `:prefix-tree-router`         | Router that creates a [prefix-tree](https://en.wikipedia.org/wiki/Radix_tree) out of an route table. Much faster than `:linear-router`. Valid only if there are no [Route conflicts](../basics/route_conflicts.md).

The router name can be asked from the router:

```clj
(require '[reitit.core :as r])

(def router
  (r/router
    [["/ping" ::ping]
     ["/api/:users" ::users]]))

(r/router-name router)
; :mixed-router
```