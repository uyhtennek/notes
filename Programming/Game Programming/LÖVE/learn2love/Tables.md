# Table vs Hash
Lua vs others

Other languages have different names for this data type
* `object`, `hash`, `map`, `dictionary`
* their features may vary from one programming language to another

Tables are used to builid *composite data types*
* eg. lists, trees, or a big green orc running across the screen
* composited with primitive data types
	* like numbers and strings

basic syntax
```lua
-- an empty table
my_cool_table = {}
```
___

# Table as List

```lua
-- lists
groceries = {
	[1] = "beans",
	[2] = "bananas",
	[3] = "buns"
}
-- the order doesn't matter, this is the same as above
groceries = {
	[3] = "buns",
	[1] = "beans",
	[2] = "bananas"
}
-- this is also the same
groceries = {
	"beans",    -- 1
	"bananas",  -- 2
	"buns"      -- 3
}

-- access data in table
print(groceries[1]) --Output: beans

-- get the length of the table
print(#groceries)   --Output: 3
```

`[1]` is the *key*
* as this one is numeric so we can call it *index* as well

if it's missing any comma between items, it gives quite the error message
`[string "groceries = {..."]:3: '}' expected (to close '{' at line 1) near '['`

if we try to access `groceries[4]`,
it gives `nil`
___

# Table as Dictionary

```lua
-- dictionary
coins = {
	["half"] = "50 cents",
	["quarter"] = "25 cents",
	["dime"] = "10 cents",
	["nickel"] = "5 cents",
	["penny"] = "1 cent"
}

-- add an item
coins["silver dollar"] = "1 dollar" -- 1.
coins.silver_dollar = "1 dollar"    -- 2.

coins.silver dollar = "1 dollar"    -- X
coins.100 = "1 dollar"              -- X
```

we can use variable names for keys when creating the table
```lua
color = "purple"
description = "the best color"
colors = {
	[color] = description
}
```

### Avoid `crazy_list`
below tables might work sometimes, but not preferred

```lua
-- 1.
crazy_list = {
	[true] = "works",
	[false] = "works",
	["true"] = "not the same",
	["false"] = "not the same"
}
print(crazy_list[true])
print(crazy_list[false])
print(crazy_list["true"])
print(crazy_list["false"])
--[[ Output:
works
works
not the same
not the same
--]]

-- 2.
crazy_key = {}
crazy_list = {
	[crazy_key] = "works"
}
print(crazy_list[crazy_key])
-- Output: works

-- 3.
crazy_list = {
	[nil] = "doesn't work!"
}
print(crazy_list[nil])
-- Output:
-- [string "crazy_list = {..."]:2: table index is nil
```
___

# Function as value

```lua
cat = {
	color = "gray",
	smelly = true,
	make_sound = function()
		print("meyuow!")
	end
}

cat.make_sound()
-- Output: meyuow!
```
___
