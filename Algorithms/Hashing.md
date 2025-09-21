**Hashing** serves the purpose of ensuring integrity, i.e. making it so that if something is changed you can know that it’s changed. Technically, hashing takes arbitrary input and produce a fixed-length string that has the following attributes:
- The same input will always produce the same output.
- Multiple disparate inputs should not produce the same output.
- It should not be possible to go from the output to the input.
- Any modification of a given input should result in drastic change to the hash.

*Hashing* is used in conjunction with [[Algorithms/Misc#^a1|authentication]] to produce strong evidence that a given message has not been modified. This is accomplished by taking a given input, hashing it, and then signing the hash with the sender’s private key.

**Examples**: SHA-3, MD5
