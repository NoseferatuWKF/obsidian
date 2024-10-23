**PEM** 
>[P]rivacy [E]nhanced [M]ail which concatenates certificate containers

**DOM** 
>[D]ocument [O]bject [M]odel that is stored in memory to represent what you see in html

**Shadow DOM** 
>nested/embeded DOM inside a custom html component/node. It does not leak its attribute upwards. It can be enabled in chrome by opening up settings and enable shadow dom agent

**SCM** 
>[S]oftware [C]onfiguration [M]anagement

**SSL** 
>[S]ecured [S]ocket [L]ayer, replaced by TLS

**TLS**
>[T]ransport [L]ayer [S]ecurity

**OAuth** 
>is an open-standard authorization protocol or framework that provides applications secured access. It does not handle authentication (while some applications try to hack their way to do this) only authorization

**OIDC** 
>open id connect is a small layer/extension on top of oauth, in order to handle authentication. It provides the client/application with identity

**SAML** 
>[S]ecurity [A]ssertion [M]arkup [L]anguage. It is an XML-based open-standard for transferring identity data between two parties: an identity provider (IdP) and a service provider (SP).

**PKCE** 
>[P]roof [K]ey [C]ode [E]xchange

**PAKE**
>[P]assword-based [A]uthenticated [K]ey [E]xchange

**CGI** 
>[C]ommon [G]ateway [I]interface, takes stdin as the body, env as headers, cli args as query params and prints the response using stdout

**WCF** 
>[W]indows [C]ommunication [F]oundation

**DSL** 
>[D]omain [S]pecific [L]anguage

**CNCF** 
>[C]loud [N]ative [C]omputing [F]oundation

**Dogfooding** 
>building something that yourself use

**sentinel value** 
>a special value in the context of an algorithm to signify termination

**exponential backoff** 
>an algorithm that uses feedback to multiplicative decrease the rate of some process, in order to gradually find an acceptable rate.

**resource contention** 
>conflict over a shared resource between several components

**BSD**
>[B]erkeley [S]oftware [D]istribution

**Lambda** vs **Closure**
>Lambda are anonymous functions, while closures are functions(so it can be a lambda as well) that encloses an environment which have access to variables outside of its current environment

**Thunk**
>A function with no arguments

**POSIX**
>[P]ortable [O]perating [S]ystem [I]nterface for Un[ix]

**SUS**
>[S]ingle [U]nix [S]pecification

**Mutual Exclusion aka mutex**
>Only one change can affect a value at a time

**AST**
>[A]bstract [S]yntax [T]ree

**semaphore**
>An unsigned integer variable shared among multiple processes/threads. It is used to sync and the changes on semaphores are atomic and can only be changed by wait() and post(). Also this different than mutex locks

**SIMD**
>[S]ingle [I]nstruction [M]ultiple [D]ata, processing multiple data with a single instruction, ie; processing data in a loop

**TCO**
>[T]ail [C]all [O]ptimization, the last action in a function is a call to another function

**raw pointer**
>A pointer without a lifetime

**segmented memory**
>memory allocated based on the size of the program. The downside of segmented memory is fragmentation. Segmented memory is atomic and cannot be split, so if a program wants to use the ram, the whole segmented memory of the program will be swapped to disk, and the other program can use the ram (provided there is enough space, and the ram is not fragmented)

**paged memory**
>memory is split into small, equal sized pages/frames. Each program has a mapped logical memory to physical memory / swap file via a page table.

**page in / out**
>unused pages are paged out to swap file, and paged in back once there is a need

**swap file / virtual memory**
>a supplementary secondary storage for physical memory

**disk threshing**
>when memory is low, excessive swapping can happen

**PCI-DSS**
>[P]ayment [C]ard [I]ndustry [D]ata [S]ecurity [S]tandard

**AML**
>[A]nti [M]oney [L]aundering

**KYC**
>[K]now [Y]our [C]ustomer

**FFI**
>[F]oreign [F]unction [I]nterface

**Byzantine Fault**
>Any fault presenting different symptoms to different observers

**Amdahl's Law**
>The overall performance improvement gained by optimizing a single part of a system is limited by the fraction of time that the improved part is actually used

**Brooke's Law**
>Adding manpower to a late software project makes it later

**Conway's Law**
>Organizations which design systems are constrained to produce designs which are copies of the communication structures of these organizations.

**Atoi**
>[A]scii to [I]nteger

**Itoa**
>[I]nteger to [A]ascii

LDAP
>[L]ightweight [D]irectory [A]ccess [P]rotocol

nonce
>In cryptography, a nonce is an arbitrary number that can be used just once in a cryptographic communication. It is often a random or pseudo-random number issued in an authentication protocol to ensure that old communications cannot be reused in replay attacks. They can also be useful as initialization vectors and in cryptographic hash functions.

TLB
>[T]ranslation [L]ook-aside [B]uffer, is a unit in CPU to translate virtual address to physical address

Optimistic locking
>lock with versioning (dates/timestamps/checksums/hashes). If record is dirty i.e version changed, then transaction is aborted and can be restarted. This strategy is applicable to high-volume systems and three-tier architectures where the application does not maintain a connection to the database for the session

Pessimistic locking
>mutex lock for exclusive use of a single transaction until it finishes. this is how two-phase commit works

syscall
>called by software running on the OS to services provided by the OS

interrupt
>usually called by hardware, but can also be called by software (soft interrupt) notifying the CPU/Microprocessor about an event that needs handling in software (usually a driver)

copyleft
>legal techniques of granting the same rights to derivative works/copies