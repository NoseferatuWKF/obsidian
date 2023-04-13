[[cheat-sheets/js|cheat-sheets]] 
[best-practices](https://github.com/goldbergyoni/nodebestpractices) - good shit for a bad language

## Guideline
- use loops when mutating data and higher order array functions when not  - airbnb
- In general, it's good practice to always use block statements
- Use maps over objects when keys are unknown until run time, and when all keys are the same type and all values are the same type
- Use objects when there is logic that operates on individual elements
- Currently, all modern engines ship a mark-and-sweep garbage collector

## When you don't need JS
- multi-threaded operations / parallel computing
- strict and verbose control-flow
- bitwise operations - [IEEE 754](https://en.wikipedia.org/wiki/Double-precision_floating-point_format)
- precision math
- functional programming (lots of copying, immutable, two space indents)

## Videos
- [Promises explained exceptionally well](https://www.youtube.com/watch?v=bAlczbDUXx8&ab_channel=StevieJay)

## Installation

>because installing it without a version manager sucks

fnm
```bash
fnm install --lts 
# or this
fnm use lts/latest
```

## Basics

[Logical AND (&&)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND)
```js
const a = 5;
const b = 3;

// short-circuit evaluation
const c = a > 3 && b // first condition evalutates to true, so c = b = 3;
const d = a < b && 5 // first condition evaluates to false, so d = a < b = false;

// object
const val = true;
const val2 = 123;
const obj = {
...(typeof val === 'boolean' && {val}), // will add {val: true}
...(typeof val2 === 'boolean' && {val2}), // will do nothing
}
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

[Generators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*)
>Generators are functions that can be exited and later re-entered. Their context (variable bindings) will be saved across re-entrances.
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

[yield*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/yield*)
```js
function* func1() {
  yield 42;
}

function* func2() {
  yield* func1();
}

const iterator = func2();

console.log(iterator.next().value);
// Expected output: 42
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

[BigInt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt)
```js
const previouslyMaxSafeInteger = 9007199254740991n

const alsoHuge = BigInt(9007199254740991)
// 9007199254740991n

const hugeString = BigInt("9007199254740991")
// 9007199254740991n

const hugeHex = BigInt("0x1fffffffffffff")
// 9007199254740991n

const hugeOctal = BigInt("0o377777777777777777")
// 9007199254740991n

const hugeBin = BigInt("0b11111111111111111111111111111111111111111111111111111")
// 9007199254740991n

// When tested against `typeof`, a BigInt value (`bigint` primitive) will give `"bigint"`:
typeof 1n === 'bigint'           // true
typeof BigInt('1') === 'bigint'  // true

// Most operators that can be used between numbers can be used between BigInt values as well.
// BigInt addition
const a = 1n + 2n; // 3n
// Division with BigInts round towards zero
const b = 1n / 2n; // 0n
// Bitwise operations with BigInts do not truncate either side
const c = 40000000000000000n >> 2n; // 10000000000000000n
```

## Advanced

Configuring memory
```bash
node --max-old-space-size=6000 index.js # heap allocation
node --expose-gc --inspect index.js # exposing gc to debug memory
```

Javascript typed arrays
```js
const buffer = new ArrayBuffer(16);
const int32View = new Int32Array(buffer); // creating a 32-bits view into the buffer
// now can access like a normal array
// This fills out the 4 entries in the array (4 entries at 4 bytes each makes 16 total bytes) with the values 0, 2, 4, and 6.
for (let i = 0; i < int32View.length; i++) {
  int32View[i] = i * 2;
}
const int16View = new Int16Array(buffer); // create another view but now its 16-bits
for (let i = 0; i < int16View.length; i++) {
  console.log(`Entry ${i}: ${int16View[i]}`); // 0, 0, 2, 0, 4, 0, 6, 0
}
```

[zlib](https://nodejs.dev/en/api/v19/zlib/)

[stream](https://nodejs.dev/en/api/v19/stream/)

[child process](https://nodejs.dev/en/api/v19/child_process/)

[events](https://nodejs.dev/en/api/v19/events/) - I love this

[promisify](https://nodejs.org/docs/latest-v14.x/api/util.html#util_util_promisify_original)

[pipeline](https://nodejs.org/docs/latest-v14.x/api/stream.html#stream_stream_pipeline_streams_callback)

[isDeepStrictEqual](https://nodejs.org/docs/latest-v14.x/api/util.html#util_util_isdeepstrictequal_val1_val2)

Event loop
```
   ┌───────────────────────────┐
┌─>│           timers          │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │     pending callbacks     │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │       idle, prepare       │
│  └─────────────┬─────────────┘      ┌───────────────┐
│  ┌─────────────┴─────────────┐      │   incoming:   │
│  │           poll            │<─────┤  connections, │
│  └─────────────┬─────────────┘      │   data, etc.  │
│  ┌─────────────┴─────────────┐      └───────────────┘
│  │           check           │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
└──┤      close callbacks      │
   └───────────────────────────┘
```




