# Circuit Breakers

Circuit Breakers are a concept that I first read about in Michael Nygard's excellent book [Release It!](https://pragprog.com/titles/mnee2/release-it-second-edition/). They are useful when making RPC (e.g. an HTTP request) from one service to another and are designed to isolate failures and prevent them from cascading.

As the name suggests, the concept comes from electrical circuit breakers in houses. The idea there being that, in order to prevent a fire from an electrical circuit overload, a breaker will open the circuit, cutting off power to some parts of the house. When the circuit is reset to a closed state, electricity can flow again.

A software circuit breaker works the same way. The circuit begins in the "closed" state, and requests can be made to another service.

![A circuit breaker in a closed state](./circuit-breaking-closed.png)