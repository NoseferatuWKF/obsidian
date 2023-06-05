data structures
```haskell
-- list: can only contain one type
[123]
['a','b','c']

-- tuple: can contain multiple types
("abc", 123)

-- characters: list of characters
"abc" -- equivalent to  a:b:c:[]
```

functions
```haskell
-- get first data in tuple
fst in ("abc", 123) -- abc
-- map
map (+1) [1,2,3] -- [2,3,4]
filter (==5) [1,2,3,4,5] -- [5]
-- function declaration
let square x = x * x
```

variables
```haskell
let x = 5
```

pattern matching
```haskell
(a,b,c) = [15,17,20] -- a:15, b:17, c:20
```

running ghc
https://wiki.archlinux.org/title/Haskell