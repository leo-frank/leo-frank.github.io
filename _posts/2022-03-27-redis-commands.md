# redis commands
所有 signal command 都满足 atomic
2011-12-31-new-years-eve-is-awesome.md
https://redis.io/commands/

## INCR
All the Redis operations implemented by single commands are atomic, including the ones operating on more complex data structures. So when you use a Redis command that modifies some value, you don't have to think about concurrent access.

不要这样做：

```
x = GET count
x = x + 1
set count x
```

## TTL, PTTL, EXPIRE, PEXPIRE, PERSIST
TTL return value

-1 => never expire

-2 => the key does not exist anymore

如果多次set同一个key，那么这个key的ttl会被reset

## Notice

不需要先去创建key，再添加value。可以直接对某个不存在的key添加value，从而自动创建这个不存在的key。

## LIST: RPUSH, LPUSH, LLEN, LRANGE, LPOP, RPOP

```
RPUSH friends "Alice"

LRANGE friends 0 -1 => 1) "Sam", 2) "Alice", 3) "Bob"

LPOP friends => "Sam"

RPOP friends => "Bob"

LLEN friends => 1

RPUSH friends 1 2 3 => 4
```

## SET: SADD, SREM, SISMEMBER, SUNION, SPOP, SRANDMEMMBER

SREM removes the given member from the set, returning 1 or 0 to signal if the member was actually there or not.

```
SADD superpowers "x-ray vision" "reflexes"
SREM superpowers "reflexes" => 1
SREM superpowers "making pizza" => 0
SMEMBERS superpowers => 1) "flight", 2) "x-ray vision"
SUNION superpowers birdpowers => 1) "pecking", 2) "x-ray vision", 3) "flight"
```

## Sorted Set: ZADD, ZRANGE

Sets are a very handy data type, but as they are unsorted they don't work well for a number of problems. This is why Redis 1.2 introduced Sorted Sets. A sorted set is similar to a regular set, but now each value has an associated score. This score is used to sort the elements in the set.

## Hashes: HSET, HGETALL, HMSET, HGET

Hashes are maps between string fields and string values, so they are the perfect data type to represent objects (eg: A User with a number of fields like name, surname, age, and so forth):

```
HSET user:1000 name "John Smith"

HSET user:1000 email "john.smith@example.com"

HSET user:1000 password "s3cret"

HGETALL user:1000

HMSET user:1001 name "Mary Jones" password "hidden" email "mjones@example.com"

HGET user:1001 name => "Mary Jones"
```