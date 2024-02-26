# [Tokio](https://tokio.rs/tokio/tutorial)

## Hello Tokio

```rust
use mini_redis::{client, Result};

#[tokio::main] // tokio attribute macro
async fn main() -> Result<()> {
    let mut client = client::connect("localhost:6379").await?;
    
    client.set("hello", "world".into()).await?;

    let result = client.get("hello").await?;

    println!("got value from the server; result={:?}", result);
    
    Ok(())
}
```

>Although other languages implement async/await too, Rust takes a unique approach. Primarily, Rust's async operations are lazy. This results in different runtime semantics than other languages.

>This is very interesting because a runtime like node for example does polling to get the current status of the current task. How Tokio does async operations is by yielding back control to the thread when the .await called on the return value. Of course this only happens within and async fn scope. This makes Tokio use less resource and more reliable

>Another cool thing about tokio is that it uses a multi-threaded scheduler and a hight performance timer unlike nodejs single-threaded timer

async example
```rust
async fn lazy_await() {
	println!("I arrive late");
}

async fn main() {
	// this just returns the representation of the operation
	let late = lazy_await();

	println!("I am first");

	late.await; // now it returns a value
}
```

## Spawning

concurrency with a single thread using tasks
```rust
#[tokio::main]
async fn main() {
    let listener = TcpListener::bind("localhost:6379").await.unwrap();

    loop {
        let (socket, _) = listener.accept().await.unwrap();

		// this here is cool
		// so it moves ownership of socket to the spawn task
		// and then making the lifetime of the variable 'static bound
		// 'static bound means that the type is 'static not the value
		// which does not cause a memory leak
        tokio::spawn(async move {
			// this spawns a new task
            todo!()
        });
    }
}
```

>A Tokio task is an asynchronous green thread. They are created by passing an async block to tokio::spawn. The tokio::spawn function returns a JoinHandle, which the caller may use to interact with the spawned task. The async block may have a return value. The caller may obtain the return value using .await on the JoinHandle.

JoinHandle
```rust
#[tokio::main]
async fn main() {
    let handle = tokio::spawn(async {
        // Do some async work
        "return value"
    });

    // Do some other work

    let out = handle.await.unwrap();
    println!("GOT {}", out);
}
```

>Tasks in Tokio are very lightweight. Under the hood, they require only a single allocation and 64 bytes of memory. Applications should feel free to spawn thousands, if not millions of tasks.

## Shared State

>Note, `std::sync::Mutex` and not `tokio::sync::Mutex` is used to guard the HashMap. A common error is to unconditionally use `tokio::sync::Mutex` from within async code. An async mutex is a mutex that is locked across calls to `.await`.

>A synchronous mutex will block the current thread when waiting to acquire the lock. This, in turn, will block other tasks from processing. However, switching to `tokio::sync::Mutex` usually does not help as the asynchronous mutex uses a synchronous mutex internally.

>As a rule of thumb, using a synchronous mutex from within asynchronous code is fine as long as contention remains low and the lock is not held across calls to `.await`. Additionally, consider using `parking_lot::Mutex` as a faster alternative to `std::sync::Mutex.

std::sync::Mutex
```rust
// This works!
async fn increment_and_do_stuff(mutex: &Mutex<i32>) {
    {
        let mut lock: MutexGuard<i32> = mutex.lock().unwrap();
        *lock += 1;
    } // lock goes out of scope here

    do_something_async().await;
}
```

tokio::sync::Mutex
```rust
use tokio::sync::Mutex; // note! This uses the Tokio mutex

// This compiles!
// (but restructuring the code would be better in this case)
async fn increment_and_do_stuff(mutex: &Mutex<i32>) {
    let mut lock = mutex.lock().await;
    *lock += 1;

    do_something_async().await;
} // lock goes out of scope here
```

## Channel

>The answer is to use message passing. The pattern involves spawning a dedicated task to manage the client resource. Any task that wishes to issue a request sends a message to the client task. The client task issues the request on behalf of the sender, and the response is sent back to the sender.

>Tokio provides a [number of channels](https://docs.rs/tokio/1/tokio/sync/index.html), each serving a different purpose.

-   [mpsc](https://docs.rs/tokio/1/tokio/sync/mpsc/index.html): multi-producer, single-consumer channel. Many values can be sent.
-   [oneshot](https://docs.rs/tokio/1/tokio/sync/oneshot/index.html): single-producer, single consumer channel. A single value can be sent.
-   [broadcast](https://docs.rs/tokio/1/tokio/sync/broadcast/index.html): multi-producer, multi-consumer. Many values can be sent. Each receiver sees every value.
-   [watch](https://docs.rs/tokio/1/tokio/sync/watch/index.html): single-producer, multi-consumer. Many values can be sent, but no history is kept. Receivers only see the most recent value.

>If you need a multi-producer multi-consumer channel where only one consumer sees each message, you can use the [`async-channel`](https://docs.rs/async-channel/) crate. There are also channels for use outside of asynchronous Rust, such as [`std::sync::mpsc`](https://doc.rust-lang.org/stable/std/sync/mpsc/index.html) and [`crossbeam::channel`](https://docs.rs/crossbeam/latest/crossbeam/channel/index.html). These channels wait for messages by blocking the thread, which is not allowed in asynchronous code.