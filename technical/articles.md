[The Law of Leaky Abstractions](https://www.joelonsoftware.com/2002/11/11/the-law-of-leaky-abstractions/)
```
- abstraction over an unreliable system
- .forward files?
- manually first then use a tool
```

[react is holding me hostage](https://emnudge.dev/blog/react-hostage)
```
- "Hooks created a lot of problems that simply do not exist in idiomatic JS; Difficult to optimize; Too heavily invested in runtime semantics which makes it difficult to evolve in a direction that make more use of compilers." ~ Evan You
- Complexity of front-end have been moved to React, in the case of Hooks, the complexity of Class components have been moved there
- VDOM is the consequence of React
- React allows so much foot guns
- "When I build libraries for React, ironically, I don't really use hooks like useState, useReducer, etc. One of the best perks (and footguns) of managing your state *outside* of react is that you get to have full control over when a component should rerender." ~ Tanner Linsley
- React libraries exists to fix React shortcomings
- React has component level reactivity not fine-grained reactivity
```

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
[Extreme HTTP Performance Tuning: 1.2M API req/s on a 4 vCPU EC2 Instance](https://talawah.io/blog/extreme-http-performance-tuning-one-point-two-million/)
[The API Churn/Security Trade-off](https://intercoolerjs.org/2016/02/17/api-churn-vs-security.html)
[Schema Stitching vs GraphQL Federation](https://hygraph.com/blog/schema-stitching-vs-graphql-federation-vs-content-federation)
[How I write HTTP servers in GO after 13 years](https://grafana.com/blog/2024/02/09/how-i-write-http-services-in-go-after-13-years/)
[The Shodan Programmer](https://mozinc.wordpress.com/2017/06/18/99/)
[How Developers Stop Learning: Rise of the Expert Beginner](https://daedtech.com/how-developers-stop-learning-rise-of-the-expert-beginner/)
[Scrum vs Kanban vs Lean vs XP](https://www.objectstyle.com/blog/agile-scrum-kanban-lean-xp-comparison)
[Dotnet Evolutionary Architecture By Example](https://github.com/evolutionary-architecture/evolutionary-architecture-by-example)
```
CHAPTER 1: Simplicity, namespaces, feature slices
CHAPTER 2: Maintainability, modularity
CHAPTER 3: Scale, micro services, distributed architecture
CHAPTER 4: Complexity, Tactical DDD
```

[Service Granularity, Disintegrators](https://klotzandrew.com/blog/service_granularity_disintegrators/)

[B-trees and database indexes](https://planetscale.com/blog/btrees-and-database-indexes)
```
nodes = pages = disk blocks

# B-tree
- Each node in the tree stores N key/value pairs, where N is greater than 1 and less than or equal to K.
- Each internal node has at least N/2 key-value pairs (an internal node is one that is not a leaf or the root).
- Each internal node has N+1 children.
- The root node has at least one value and two children, unless it is the sole node.
- All leaves are on the same level.
- Searching for a key uses binary search as the keys are strongly sorted

# B+tree - similar to B-tree with a change to the rules
- Key-value pairs are stored only at the leaf nodes.
- Non-leaf nodes store only keys and the associated child pointers.
- pages are 16K by default

# MySQL B+tree - additional rules
- Non-leaf nodes store N child pointers instead of N+1.
- All nodes also contain "next" and "previous" pointers, allowing each level of the tree to also act as a doubly-linked list.
- Because MySQL loads the entire associated page by default, any query that uses less than the page size is not really performance optimizing

# InnoDB - the B+tree database engine
- all tables data are stored in B+tree with the table's primary key as the tree key
- more columns = less leaf rows, less columns = more leaf rows
- each secondary indexes will create a new B+tree instance, and a query using the secondary index will first lookup on its B+tree returns the associated primary key and then lookup on the primary key B+tree

# Buffer pool - in memory cache for InnoDB pages
- InnoDB caches unique node visits to the buffer pool

# Note on perfomance
- Use smaller primary keys to create shallower B+trees by fitting the internal nodes with more keys
- Use predictable primary keys for fewer I/O requests on insertions
- Consider using created_at timestamps for tables that does sorting or querying by created_at columnn as it is predictable
```

[12 factor app](https://12factor.net)
```
Codebase -> One codebase tracked in revision control, many deploys
- there is always a 1:1 correlation between the codebase and the app
- multiple apps sharing the same code is a violation
- shared code should be factored into libraries which can be included through a dependency manager

Dependencies -> Explicitly declare and isolate dependencies
- dependencies is declared completely and exactly, via a dependency declaration manifest (eg; package-lock.json, Gemfile, etc...)
- ensure no implicit dependencies leak in from the surrounding system via a dependency isolation tool
- the app code need to be able to be build using a deterministic build command
- the app do not rely on system tools

Config -> Store config in the  environment
- config should be separated from code
- store config in environment variables

Backing services -> Treat backing services as attached resources
- local and third party services should be swappable via config
- resources can be attached and detached from deploys at will

Build, release, run -> Strictly separate build and run stages
- use strict separation between build, release, and run stages
- Build
	- can be more complex as it has less commercial impact
- Release
	- should always have a unique release ID
	- append-only and cannot be mutated
- Runtime
	- should be kept to as few moving parts as possible

Processes -> Execute the app as one or more stateless processes
- processes are stateless and share-nothing
- persisted data must be stored in a stateful backing services

Port binding -> Export services via port binding
- app is completely self-contained
- a routing layer handles routing requests from a public-facing hostname to the port-bound web processes

Concurrency -> Scale out via the process model
- processes are a first class citizen following the unix process model
- diverse type of work is assigned to a process type
- processes should never daemonize, instead should rely on the operating system's process manager

Disposability -> Maximize robustness with fast startup and graceful shutdown
- processes can be started or stopped at a moment's notice
- strive to minimize startup time
- processes need to shut down gracefully

Dev/prod parity -> Keep development, staging and production as similar as possible
- make the time gap small
- make the personnel gap small
- make the tools gap small
- use the same backing services between development and production

Logs -> Treat logs as event streams
- each running process writes its event stream, unbuffered, to stdout
- in staging or production deploys, each process' stream will be captured and managed by the execution environment
- the log streams are routed to a sink for viewing and long-term archival

Admin processes -> Run admin/management tasks as one-off processes
- admin processes should be run in an identical environment, with the same codebase, config, and release
- the same dependency isolation should be used
```