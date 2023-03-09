## Videos
- [Promises explained exceptionally well](https://www.youtube.com/watch?v=bAlczbDUXx8&ab_channel=StevieJay)

[Logical AND (&&)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND)
```js
const a = 5;
const b = 3;

// short-circuit evaluation
const c = a > 3 && b // first condition evalutates to true, so c = b = 3;
const d = a < b && 5 // first condition evaluates to false, so d = a < b = false;
```

Use [structuredClone()](https://developer.mozilla.org/en-US/docs/Web/API/structuredClone) to do deep copy
```js
// nodejs v17.0.29, typescript v4.7
const obj = {key: "value"};
const deepCopyObj = structuredClone(obj);
```

[Logical AND assignment (&&=)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND_assignment)
```js
let a = 1;
let b = 0;

a &&= 2;
console.log(a);
// Expected output: 2

b &&= 2;
console.log(b);
// Expected output: 0
```

[Logical OR assignment (||=)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_OR_assignment)
```js
const a = { duration: 50, title: '' };

a.duration ||= 10;
console.log(a.duration);
// Expected output: 50

a.title ||= 'title is empty.';
console.log(a.title);
// Expected output: "title is empty"
```

[Nullish coalescing assignment (??=)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_assignment)
```js
const a = { duration: 50 };

a.duration ??= 10;
console.log(a.duration);
// Expected output: 50

a.speed ??= 25;
console.log(a.speed);
// Expected output: 25
```

[function*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*)
Generators are functions that can be exited and later re-entered. Their context (variable bindings) will be saved across re-entrances.
```js
function* generator(i) {
  yield i;
  yield i + 10;
}

const gen = generator(10);

console.log(gen.next().value);
// Expected output: 10

console.log(gen.next().value);
// Expected output: 20
```

[label](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/label)
```js
let str = '';

loop1:
for (let i = 0; i < 5; i++) {
  if (i === 1) {
    continue loop1;
  }
  str = str + i;
}

console.log(str);
// Expected output: "0234"
```

