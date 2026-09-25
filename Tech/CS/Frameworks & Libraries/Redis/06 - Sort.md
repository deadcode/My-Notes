Redis `SORT` is a command that sorts elements from a **list, set, or sorted set** and returns the sorted result. It is different from a Redis sorted set: `SORT` performs sorting when called, while sorted sets maintain ordering continuously.

# Basic usage
```redis
LPUSH numbers 10 2 30

SORT numbers           # 2, 10, 30
SORT numbers DESC      # 30, 10, 2
```
By default, sorting is numeric. Use `ALPHA` for lexicographic/string sorting:
```redis
SADD names alice bob charlie

SORT names ALPHA
SORT names ALPHA DESC
```
# Limit results
```redis
SORT numbers LIMIT 0 2
```
This skips zero elements and returns at most two results.

# Sort by external values

Suppose the set contains user IDs:
```redis
SADD users 101 102
SET score:101 90
SET score:102 75

SORT users BY score:* DESC GET #
```
`BY score:*` sorts by each user’s external score key, while `GET #` returns the original user ID.

# Store the result
```redis
SORT users BY score:* DESC STORE leaderboard
```

`STORE` saves the result as a **list**, not as a sorted set.

`SORT` has complexity `O(N + M log M)`, where `N` is the input size and `M` is the number of returned elements, so it can be expensive for large collections.