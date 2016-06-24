# full.cache

[![Clojars Project](https://img.shields.io/clojars/v/fullcontact/full.cache.svg)](https://clojars.org/fullcontact/full.cache)
[![Build Status](https://travis-ci.org/fullcontact/full.cache.svg?branch=master)](https://travis-ci.org/fullcontact/full.cache)

2 level caching (In memory + memcache) for Clojure.


## Configuration

Memcached server url can be set in the yaml config file(s), it's loaded via
[full.core](https://github.com/fullcontact/full.core):

```yaml
memcache-url: localhost:11211
```

## Usage

All methods in `full.cache` follow naming conventions:

* methods starting with `r` will only use remote cache (`rget`, for example)
* methods starting with `l` will only use localc cache (`lget`, for example)
* methods without `r` or `l` prefix will use both caches (on writes - writing
  to both, on reads - using local first & fallbacking to remote)

* methods ending with a  `>` return a core.async channel with the value
* methods not ending with a `>` are blocking

`full.cache` provides methods for working with cache in any of desired
combinations (local only, remote only, or both)

```clojure
;  method that tries to load a cool value & fallbacks to the loader method
;  if value is absent.

(get-or-load>
  "cool-value"
  (fn []
    (go
      (clojure.core.async/<! (clojure.core.async/timeout 3000))
      42))
  100)

; read from remote cache only
(rget "cool-value")

; fetching or loading from local cache only

(lget-or-load>
  "cool-value"
  (fn []
    (go
      (clojure.core.async/<! (clojure.core.async/timeout 3000))
      42))
  100)
```
