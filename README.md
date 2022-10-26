# NAME

Cache::RedisDB - RedisDB based cache system

# DESCRIPTION

This is just a wrapper around RedisDB to have a single Redis object and connection per process. By default uses server redis://127.0.0.1, but it may be overwritten by REDIS\_CACHE\_SERVER environment variable. It transparently handles forks.

# COMPATIBILITY AND REQUIREMENTS

Redis 2.6.12 and higher strongly recommended.  Required if you want to use
extended options in ->set().

# SYNOPSIS

    use Cache::RedisDB;
    Cache::RedisDB->set("namespace", "key", "value");
    Cache::RedisDB->get("namespace", "key");

# METHODS

## redis\_uri

Returns a `redis://` redis URI which will be used for the initial Redis connection.

This will default to localhost on the standard port, and can be overridden with the
`REDIS_CACHE_SERVER` environment variable.

## redis\_connection

Creates new connection to a Redis server and returns the corresponding [RedisDB](https://metacpan.org/pod/RedisDB) object.

## redis

Returns a singleton [RedisDB](https://metacpan.org/pod/RedisDB) instance.

## get

Takes a `$namespace` and `$key` parameter, and returns the scalar value
corresponding to that cache entry.

This will automatically deserialise data stored with [Sereal](https://metacpan.org/pod/Sereal). If no data
is found, this will return `undef`.

## mget

Retrieve values for multiple keys in a single call.

Similar to ["get"](#get), this takes a `$namespace` as the first parameter,
but it also accepts a list of `@keys` to look up.

Returns an arrayref in the same order as the original keys. For any
key that had no value, the resulting arrayref will contain `undef`.

## set

Creates or updates a Redis key under `$namespace`, `$key` using the scalar `$value`.
Also takes an optional `$exptime` as expiration time in seconds.

    $redis->set($namespace, $key, $value);
    $redis->set($namespace, $key, $value, $expiry_time);

Can also be provided a callback which will be executed once the command completes.

## set\_nw($namespace, $key, $value\[, $exptime\])

Same as _set_ but do not wait confirmation from server. If the server returns
an error, it will be ignored.

## del($namespace, $key1\[, $key2, ...\])

Delete given keys and associated values from the cache. _$namespace_ is common for all keys.
Returns number of deleted keys.

## keys($namespace)

Return an arrayref of all known keys in the provided `$namespace`.

## ttl($namespace, $key)

Return the Time To Live (in seconds) of a key in the provided _$namespace_.

### flushall

Delete all keys and associated values from the cache.

# AUTHOR

binary.com, `<perl at binary.com>`

# BUGS

Please report any bugs or feature requests to `bug-cache-redisdb at rt.cpan.org`, or through
the web interface at [http://rt.cpan.org/NoAuth/ReportBug.html?Queue=Cache-RedisDB](http://rt.cpan.org/NoAuth/ReportBug.html?Queue=Cache-RedisDB).  I will be notified, and then you'll
automatically be notified of progress on your bug as I make changes.

# SUPPORT

You can find documentation for this module with the perldoc command.

    perldoc Cache::RedisDB

You can also look for information at:

- RT: CPAN's request tracker (report bugs here)

    [http://rt.cpan.org/NoAuth/Bugs.html?Dist=Cache-RedisDB](http://rt.cpan.org/NoAuth/Bugs.html?Dist=Cache-RedisDB)

- AnnoCPAN: Annotated CPAN documentation

    [http://annocpan.org/dist/Cache-RedisDB](http://annocpan.org/dist/Cache-RedisDB)

- CPAN Ratings

    [http://cpanratings.perl.org/d/Cache-RedisDB](http://cpanratings.perl.org/d/Cache-RedisDB)

- Search CPAN

    [http://search.cpan.org/dist/Cache-RedisDB/](http://search.cpan.org/dist/Cache-RedisDB/)
