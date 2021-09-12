
# Bitmasks

Bitmasks are a way of encoding certain information in a small amount of memory by treating every bit in an integer as a placeholder for a boolean value (represented by a 1 or 0). I tend to think of bitmasks as a series of flags and associated operations for checking those flags.

Bitmasks are useful, but can be obtuse and confusing if you're not use to seeing them.

## Use Case: Using Bitmasks to represent sets of data

One use case for bitmasks is to represent a set of values within a range. This can be useful in partitioning schemes or load balancing algorithms. Let's say you want to represent a set of values from 0-31, you can use a 32 bit integer. In Go, this could look like this:

```
var bitmask uint32
```

When initialized, the bitmask is assigned the zero-value for an unsigned 32-bit integer, which is 0. To add a value to the bitmask, we have to perform a left shift operation and then perform a bitwise OR on the result. For example, if we want to represent a set with the values 2, 5, and 12, we can do:

```
bitmask |= 1 << 2
bitmask |= 1 << 5
bitmask |= 1 << 12
```

To see how this works, consider the operation `1 << 2`. This takes the value 1 (represented in binary as 1) and shifts it left 2 places, resulting in `100`. In other words, if each bit were 0 indexed, we've set the bit with the index 2 to 1. The result is then assigned to bitmask using the `|=` or "or-equals" operator. This takes the bits in each operand and combines them. For example:

```
var bitmask uint32  // bitmask is 0
bitmask |= 1 << 2   //    0      |= 0100.                bitmask is now 0100.
bitmask |= 1 << 5   // 0100      |= 0001 0000            bitmask is now 0001 0100
bitmask |= 1 << 12  // 0001 0100 |= 0001 0000 0000 0000  bitmask is now equal to 0001 0000 0001 0100.
```

I break binary numbers up into nibbles (4-bits) because I find it easier to read that way.

### Finding Subsets

In this use case, since we're using a bitmask to represent a set of integers, it's natural to want to check to see if x is a subset of y. In order to do this, we can perform a bitwise AND operation. For example, if x is the integer 132 (binary 1000 0100) and y is the integer 172 (binary 1010 1100) then:

```
  1000 0100  // x
& 1010 1100  // y
-----------
  1000 0100
```

Notice that the result is equal to x? So asking `if x&y==x` is the same as asking `is x a subset of y` or `is every element in the set x also in the set y`.

### Removing Bits

We saw above how to add bits from one bitmask to another (using `|=`). Now let's say you want to remove all of the bits in the bitmask y from the bitmask x. To do this, you can perform a bitwise AND with the negated value. In other words, if x is the integer 132 (binary 1000 0100) and y is the integer 172 (binary 1010 1100) we want to remove all of the set bits in y from x. The negation of y is:

```
^(1010 1100) = 0101 0011
```

So now we can perform a bitwise AND on x and this value:

```
  1000 0100
& 0101 0011
-----------
  0000 0000
```

The result is x with all of the bits in the set y removed. Because x is a subset of y, this results in 0 bits being set in the bitmask. Exercise for the reader: reverse the operation - remove all of the elements in x from y.

To summarize, to remove all of the elements in the set represented by y from the set represented by x, do:

```
x &= ^y
```
