A Redis **sorted set** is a collection of unique members, each associated with a numeric **score**. Redis keeps members ordered by score; ties are ordered lexicographically.

```redis
ZADD leaderboard 150 alice
ZADD leaderboard 220 bob
ZADD leaderboard 180 charlie

ZREVRANGE leaderboard 0 2 WITHSCORES
```

Result: highest scores first—`bob`, `charlie`, `alice`. `ZRANGE` returns lowest-to-highest; `ZREVRANGE` returns highest-to-lowest.

# Common commands

| Command                           | Purpose                     |
| --------------------------------- | --------------------------- |
| ZSCORE leaderboard alice          | Get Alice's score           |
| ZRANK leaderboard alice           | Ascending rank              |
| ZREVRANK leaderboard alice        | Descending rank             |
| ZINCRBY leaderboard 25 alice      | Add 25 points               |
| ZCARD leaderboard                 | Count members               |
| ZREM leaderboard alice            | Remove a member             |
| ZRANGEBYSCORE leaderboard 150 200 | Members with scores 150–200 |

`ZADD` updates a member’s score if that member already exists, and `ZINCRBY` increments the score.

```mermaid
---
title: Redis Ordered Set
config:
  theme: "base"
  useWidth: 1
  padding: 8
  themeVariables:
    primaryColor: "lightblue"
    secondaryColor: "blue"
    lineColor: "red"
---
block
	classDef subtitle fill:#9f6,stroke:#444,stroke-width:3px,color:#FF0000,font-weight:bold,font-size:20px;
	classDef title fill:#FF9999,stroke:#555,stroke-width:4px,color:#101010,font-weight:bold,font-size:32px;
    classDef Red fill:#FF9999;
    classDef Amber fill:#FFDEAD;
    classDef Green fill:#BDFFA4;
	block
	   	columns 2
		title("Commands For Ordered SET"):2
		class title title
		block:basic
			columns 3
			t1("Basic Operations"):3
			ZADD
			ZCARD
			ZCOUNT
			ZINCRBY
			ZCORE
			ZRANK
			ZLEXCOUNT
			ZREM
			class t1 subtitle
			class ZADD Amber
		end
		block:ReadM
			columns 3
			%% Bug in Mermaid render, making t2 3 wide breaks all other blocks
			t2("ReadMultiple Values"):1
			ZRANGE
			ZREVRANK
			ZREVRANGE
			ZREVRANGEBYLEX
			ZREVRANGEBYSCORE
			ZSCAN
			ZRANGEBYLEX
			ZRANVEBYSCORE
			ZRANGESTORE
			class t2 subtitle
		end
		block:misc
			columns 3
			t3("Misc Oper"):3
			ZMSCORE
			ZRANDMEMBER
			space:3
			class t3 subtitle
		end
		block:inter
			columns 3
			t4("Intersection"):3
			BZMPOP
			BZPOPMAX
			BZPOPMIN
			space:3
			class t4 subtitle
		end
		block:diff
			columns 3
			t5("Difference"):3
			ZDIFF
			ZDIFFSTORE
			space:3
			class t5 subtitle
		end
		block:union
			columns 3
			t6("Union"):3
			class t6 subtitle
			ZUNION
			ZUNIONSTORE
			space:3
		end
		block:rem
			columns 3
			t7("Remove Values"):3
			class t7 subtitle
			ZMPOP
			ZPOPMIN
			ZPOPMAX
			ZREMRANGEBYRANK
			ZREMRANGEBYLEX
			ZREMRANGEBYSCORE
		end
		block:intersect
			columns 3
			t8("Intersection"):3
			class t8 subtitle
			ZINTER
			ZINTERCARD
			ZINTERSTORE
			space:3
		end
	end
```

# Common use cases

- Leaderboards
- Priority queues
- Time-ordered events, using timestamps as scores
- Sliding-window rate limiters
- Scheduling tasks for execution at a specific time

Most sorted-set operations are `O(log N)`. Large `ZRANGE` results can be expensive because the cost also depends on how many members are returned.
