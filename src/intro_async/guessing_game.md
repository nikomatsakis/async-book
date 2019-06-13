# Guessing game

So let's get started with the guessing game. As we mentioned before,
we're going to build on the runtime project here, which makes it very
convenient to do basic I/O using async functions in a portable way. If
you're getting started with an async app, runtime is a good default
choice, because it provides a common interface that allows you to
switch out the precise scheduler implementation very easily. (For our
purposes, the default, naive scheduler will do just fine.)

To start with, our project needs a `main` function, just like any other
Rust program. In the runtime project, a `main` function is written as an
`async fn`, but with the `#[runtime::main]` annotation:

```rust
#[runtime::main]
async fn main() -> Result<(), failure::Error> {
    ...
}
```

This annotation is a procedural macro that will generate the *actual*
`main` function which creates the async I/O runtime and so forth. (If
we wished, we could select a runtime explicitly here; for example,
`#[runtime::main(tokio)]` would use [Tokio].)

[Tokio]: XXX

```rust
#[runtime::main]
async fn main() -> Result<(), failure::Error> {
    ...
}
```


Most of our project will take play in a `play` function. This function
takes a tcp-stream and returns either `()` or an error:

```rust
use runtime::net::TcpStream;

async fn play(stream: TcpStream) -> Result<(), failure::Error> {
  ...
}
```



The basic structure of our project is two functions:

