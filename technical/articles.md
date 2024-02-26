[The Law of Leaky Abstractions](https://www.joelonsoftware.com/2002/11/11/the-law-of-leaky-abstractions/)
[react is holding me hostage](https://emnudge.dev/blog/react-hostage)
[making tanstack table 1000x faster](https://jpcamara.com/2023/03/07/making-tanstack-table.html)
[speeding up JS eco system](https://marvinh.dev/blog/speeding-up-javascript-ecosystem/)
[how one developer just broke Node Babel and Thousands of projects](https://www.theregister.com/2016/03/23/npm_left_pad_chaos/)
[THE CONFERENCE FOR JAVA & SOFTWARE INNOVATION](https://jaxlondon.com/blog/java-core-languages/the-error-of-our-ways-kevlin-henney/)
[Why Diverse Teams Are Smarter](https://hbr.org/2016/11/why-diverse-teams-are-smarter)
[How to be a -10x Engineer](https://taylor.town/-10x)
[Hotspot performance engineering fails](https://lemire.me/blog/2023/04/27/hotspot-performance-engineering-fails/)

[Casey - Uncle Bob: Clean Code QA](https://github.com/cmuratori/misc/blob/main/cleancodeqa.md)
```
things that I can agree:
- tests:
	- to save development time
	- to actually catch and prevent bugs
	- it should be able to pass and fail so that it is more trustworthy
- readability:
	- use descriptive names for functions, variables and types
	- better to follow the conventions used to explain mathematical formulae

this made me fking hate uncle bob
"In my work I don't care about nanoseconds. I almost never care about microseconds. I sometimes care about milliseconds. Therefore I make the software engineering tradeoff towards _programmer convenience_, and long term readability and maintainability. This means that I don't want to think about the hardware. I don't want to know about Ln caches, or pipelining, or SIMD, or even how many cores there are in my processor. I want all that abstracted away from me, and I am willing to spend billions of computer cycles to attain that abstraction and separation. My concern is _programmer cycles_ not machine cycles."
```

[Casey - Uncle Bob: Clean Code QA 2](https://github.com/cmuratori/misc/blob/main/cleancodeqa-2.md)
```
good points from Casey
"I'll stop there, since I mentioned a lot of things, but hopefully that gives the general idea. I use this same approach basically everywhere - anything that looks similar gets collapsed into one thing, with an enum or a flags field that differentiates it. And it tends to produce the polar opposite of "Clean Code"-style code, because that style typically does the opposite: it creates the maximum number of types and functions, whereas I am trying to produce a much smaller number - perhaps not the minimum, but certainly much less than Clean Code."

uncle bob summary:
- Carefully choose nice names.
- Keep your functions small.
- Keep your classes small.
- Manage your dependencies.
- Be careful with side effects.
- Express yourself in code where possible.
- Use polymorphism when types change faster than operations.
- Use switch when operations change faster than types.
- When possible create designs where the things that change fast are types.
- Test Driven Development
```

[Server-side rendering React in OCaml](https://sancho.dev/blog/server-side-rendering-react-in-ocaml)
[How Much Memory Do You Need to Run 1 Million Concurrent Tasks?](https://pkolaczk.github.io/memory-consumption-of-async/)
[DevOps Is Bullshit](https://blog.massdriver.cloud/posts/devops-is-bullshit/)
[Self-healing code is the future of software development](https://stackoverflow.blog/2023/06/07/self-healing-code-is-the-future-of-software-development/#:~:text=Developers%20love%20automating%20solutions%20to,at%20an%20entirely%20new%20level.)
[We rewrote our product in Go from scratch](We rewrote our product in Go from scratch)
[You Want Modules, Not Microservices](https://blogs.newardassociates.com/blog/2023/you-want-modules-not-microservices.html)
[From $erverless to Elixir](https://medium.com/coryodaniel/from-erverless-to-elixir-48752db4d7bc)
[Async Rust Is A Bad Language](https://bitbashing.io/async-rust.html)
[# Extreme HTTP Performance Tuning: 1.2M API req/s on a 4 vCPU EC2 Instance](Extreme HTTP Performance Tuning: 1.2M API req/s on a 4 vCPU EC2 Instance)
[The API Churn/Security Trade-off](https://intercoolerjs.org/2016/02/17/api-churn-vs-security.html)
[Schema Stitching vs GraphQL Federation](https://hygraph.com/blog/schema-stitching-vs-graphql-federation-vs-content-federation)
[How I write HTTP servers in GO after 13 years](https://grafana.com/blog/2024/02/09/how-i-write-http-services-in-go-after-13-years/)