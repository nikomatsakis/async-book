# Async blocks and closures

## Async blocks

Async blocks allow you to define futures outside of the context of a
function. As a simple example, we can create a future that reads data
from a stream and parses it (unwrapping on errors for simplicity):

```rust
fn add_two_futures(
    a: impl Future<Output = u32>,
    b: Vec<u32>,
) -> impl Future<Output = u32> {
    let result: impl Future<Output = u32> = async {
        b.len() - a.await
    };
    
    result
}
```

In fact, the function above is equivalent to writing an async function:

```rust
async fn add_two_futures(
    a: impl Future<Output = u32>,
    b: Vec<u32>,
) -> u32 {
    b.len() - a.await
}
```

### Desugaring an async fn to an async block

In general, an `async fn` can be desugared to a normal `fn` that makes
use of an async block. This can be useful, because it allows you to
take some actions *immediately* when the function is called, and defer
others until the future is ready to execute. It also lets you have a future
that doesn't capture *all* of its arguments.

For example, the `add_two_futures` example could be rewritten to avoid
capturing the vector `b`, and instead just capturing the length. This would
allow the vector `b` to be freed earlier:

```rust
fn add_two_futures(
    a: impl Future<Output = u32>,
    b: Vec<u32>,
) -> impl Future<Output = u32> {
    let l = b.len();

    let result: impl Future<Output = u32> = async {
        l - a.await
    };
    
    result
}
```

### Explicit lifetimes when desugaring

```rust
fn add_two_futures<'b>(
    a: impl Future<Output = u32>,
    b: &'b [u32],
) -> impl Future<Output = u32> + 'b {
    let result: impl Future<Output = u32> = async {
        b.len() - a.await
    };
    
    result
}


## Async closures

Async closures are not yet supported, but the plan is eventually to
support a syntax like:

```rust



Just as an "async fn" defines a function that,
when called, returns a future, an async *block* defines a block of
code that, when executed
Describe 
