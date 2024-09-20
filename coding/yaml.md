anchors and aliases
```yaml
some_key: &anchor ["this", "is", "an", "anchor"]
alias_some_key: *anchor # will have whatever is 'anchored/contained' the anchor has  
```

overrides and extensions
```yaml
# let's say we have an anchor
anchored_key: &fruit
	name: Apple
	color: Red
	taste: Sweet

# we can override and extend the anchored key by using <<:
alias_key:
	<<: *fruit
	name: Strawberry
	taste: Sour
	unique_characteristics: Has tiny holes with tiny hairs

# for arrays
tags: &anchored_tags ["a", "b", "c"]

tags: [*anchored_tags, "d", "e", "f"] # [a, b, c, d, e, f]
```

multiline
```yaml
# block style indicators
literal: |
	This is the first line,
	This is the second line which will appear the same way 'literally'
folded: >
	This is the first line,
	This is the second line that will be 'folded' to the first line
```

### References
- https://www.educative.io/blog/advanced-yaml-syntax-cheatsheet#validator
- https://yaml-multiline.info/