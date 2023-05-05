converting set to array and vice versa
```js
Array.from(mySet);
[...mySet2];

mySet2 = new Set([1, 2, 3, 4]);
```

getting swagger json from nestjs
```bash
http://path/to/swagger-json # add json at the end
```

get server timezone
```js
Intl.DateTimeFormat().resolvedOptions().timeZone
```

## Other cases

>` for...in` loops through the properties in the prototype chain, therefore we need to add to do a check using `hasOwnProperty()`. Better yet, we can use `Object.keys`
```js
// need to do checking
for (const key in user) {
	if (user.hasOwnProperty(key)) {
		console.log(`${key}: ${user[key]}`);
	} 
}
// with Object.keys()
for (const key of Object.keys(user)) {
	// no checking here
}
```

string matching performance
>indexOf > slice > startsWith > regexStart