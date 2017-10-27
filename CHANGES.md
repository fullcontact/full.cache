# 2.0.0

Breaking change: Serialization of memcache values (in the remote cache)
changed to newer version of nippy. This means memcached would need to be
cleared, or `:throw?` would need to be set to `false` temporarily for the cache
to reset itself to newly serialized values.
