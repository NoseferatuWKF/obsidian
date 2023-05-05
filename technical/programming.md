## Fundamentals

### BigO
1. complexity grows with input
2. discard any constants
3. always assume worst case

### Recursion
1. base case
3. recurse
	1. pre-order
	2. in-order
	3. post-order

## Coding

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