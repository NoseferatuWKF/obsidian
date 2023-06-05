
## BigO
1. complexity grows with input
2. discard any constants
3. always assume worst case

## Recursion
1. base case
3. recurse
	1. pre-order
	2. in-order
	3. post-order

## Binaries

carry over after summing two arrays
```go
a := []int{1, 1, 0, 1}
b := []int{1, 0, 1}
c := 0
r := []int{}

for i, j := len(a) - 1, len(b) - 1; i >= 0 || j >= 0 || c != 0; {
	if i >= 0 {
		carry += a[i]
		i--
	}
	if j >= 0 {
		carry += a[j]
		j--
	}
	// this will carry the var forward for next iteration while
	// appending results of current iteration binary sum
	r = append([]int{carry & 1}, r...)
	// if this returns -1 that means the value is being carried forward
	carry = -(carry >> 1)
}
```

## Paradigms

### Functional

currying
```js
function curry(f) { // curry(f) does the currying transform
  return function(a) {
    return function(b) {
      return f(a, b);
    };
  };
}

// usage
function sum(a, b) {
  return a + b;
}

let curriedSum = curry(sum);

alert( curriedSum(1)(2) ); // 3
```