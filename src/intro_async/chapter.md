# Introducing `async/await`

This chapter gives a brief introduction to `async/await`, which are
Rust's built-in tools for writing asynchronous functions that look
like synchronous code. In this chapter we're going to make use of [the
runtime project][runtime] to write an asynchronous version of the
classic [Rust guessing game][rgg]. This is a simple game where you try
to guess a number between 1 and 100 -- each time you guess, you get given
a hint like "Too small!" or "Too big!". 

This is a network-enabled version of the guessing game: to play, you
telnet to a local port and interact with the server:

```bash
> telnet localhost 8080
21
Too small!
23
Too big!
22
You win!
```
