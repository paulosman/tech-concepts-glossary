# Consistency

Consistency in a distributed system describes how different nodes agree or disagree on the state of the system. When performing atomic operations (operations that can only succeed or fail, there can be no partial failures), there is no question about the state of a system after each operation. For example, imagine that incrementing an integer is an atomic operation (whether it actually is or not is an implementation detail, but let's pretend for a second). After each step of the following pseudo code, we can read the value of the integer and be confident that the most recent increment will be reflected in the new value:

```
i = 0

print(i) // prints 0
i += 1
print(i) // prints 1
i += 1
print(i) // prints 2
```

This seems pretty intuitive and obvious, but now imagine that setting a value of a variable requires coordination amongst nodes in a distributed system. For example, in a database system that does replication, you can have a single node that accepts all writes, and various nodes that can answer reads:

