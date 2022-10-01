## Implementing redis serialisation protocol

This is a follow-up post from [Building redis with CodeCrafters](https://www.yaphc.com/building-redis-with-codecrafters-challenge).

When I first started the redis challenge, I did the [quick and dirty](https://github.com/hanchiang/codecrafters-redis-rust/blob/0.0.1/src/request_response/client_input.rs) way of bypassing the redis serialisation protocol in order to make things work.

**Redis serialisation protocol implementation**

In the [next release](https://github.com/hanchiang/codecrafters-redis-rust/releases/tag/0.2.0), I implemented the [redis serialisation protocol](https://redis.io/docs/reference/protocol-spec/), which can handle a few data types: simple string, bulk string, error, integer, array.

It was a great exercise that puts recursion into practice. Working with raw bytes is much more performant than converting them to string every time.

**Integrate redis serialisation protocol**

Next up is to [integrate](https://github.com/hanchiang/codecrafters-redis-rust/releases/tag/0.3.0) the parser with the codebase. There was quite a bit of code because the older method of parsing input is totally different from this new implementation.

This shows the importance of design a clean interface before writing code. The underlying implementation may change, but a big rewrite will be avoided as long as the interface is still valid.
In this case, the interface of these two methods is completely different, resulting in a breaking change in the parser.

Finally, examples of usage are updated to be clearer and user friendly

Check it out [here](https://github.com/hanchiang/codecrafters-redis-rust).