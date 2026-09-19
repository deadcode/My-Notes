# Basic Key Values - Strings
All values in Redis are strings.

```mermaid
---
title: Redis Strings
config:
  theme: "base"
  useWidth: 2
  padding: 2
  themeVariables:
    primaryColor: "lightblue"
    secondaryColor: "blue"
    lineColor: "red"
---
block
	classDef green fill:#9f6,stroke:#333,stroke-width:2px;
	classDef boxheight height:48px
	columns 1
	block
		columns 3
		t("Commands For Strings"):3
		class t boxheight
		GET[/"GET"/]
		GETEX
		GETSET
		MGET
		GETRANGE
		SET[/"SET"/]
		SETEX
		SETNX
		MSET
		MSETNX
		GETDEL
		SETRANGE
		APPEND
		STRLEN
		SUBSTR
		1{{"LCS"}}
		space
		DEL
		class SET,GET green
	end
```

# Numbers
Numbers are also stored as strings. There are some commands to operate on numbers without manually get-modify-set.
```mermaid
---
title: Redis Numbers
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
	classDef green fill:#9f6,stroke:#333,stroke-width:2px;
	block:numbers
	   columns 1
		block:foo
		   title("Commands For Numbers")
		   style foo color:red
		end
		block:setget["Set & Get"]
		   columns 3
		   SET
		   space
		   GET
		   MGET
		   MSET
		   DEL
		   class SET,GET green
		   style setget color:red;padding-bottom:6em
		end
		block:change["Modify"]
		   columns 3
		   INCR
		   space
		   INCRBY
		   DECsR
		   DECRBY
		   INCRBYFLOAT
		   class INCR,DECR green
	           style change color:red;padding-bottom:6em
		end
	end
```
# Data Structures

## Hash
Commands for attributes in the hash.
## Set
