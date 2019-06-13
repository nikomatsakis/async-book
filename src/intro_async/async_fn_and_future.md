# Async functions and futures

Writing an asynchronous function is easy: you just take a normal
function but add the `async` keyword in front:

```rust
async fn my_async_function(input: u32) -> u32 { input * 2 }
```

The difference between a normal function and an async function is
that, when you call an async function, you don't execute the function
immediately.  Instead, you get back a **future** -- a future is
basically a suspended version of the function, one that is *ready* to
execute when you ask it to:

```rust
let future: impl Future<Output = u32> = my_async_function(22);
```

In order to get the value out from the future, you use the `await`
keyword -- the precise syntax is `future.await`[^suffix]. This will execute the
future and return the final result:

```rust
let future: impl Future<Output = u32> = my_async_function(22);
let result: u32 = future.await;
assert_eq!(result, 44);
```

## Note on syntax: postfix await

If you've used async-await in other languages like JavaScript, you may
be familiar with prefix await expressions like `await future`. Rust
works a bit differently, which allows for better integration other
postfix operations like `?` (to handle errors) or method chaining. As
we'll see in the upcoming example, many future operations involve I/O,
and hence include the possition of error, so it's very common to see
something like `read().await?`, which first awaits the result of a
read and then propagates any resulting error.

