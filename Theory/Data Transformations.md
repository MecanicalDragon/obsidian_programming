## Encoding
**Encoding** is a data transformation so that it can be properly (and safely) consumed by a different type of system, (e.g. binary data being sent over email). The goal is not to keep information secret, but rather to ensure that it’s able to be properly consumed. *Encoding* transforms data into another format using a scheme that is publicly available so that it can easily be reversed. It does not require a key as the only thing required to decode it is the algorithm that was used to encode it.

**Examples**: ASCII, Unicode, URL Encoding, Base64
## Encryption
**Encryption** is a data transformation in order to keep it secret from others. The goal is to ensure the data cannot be consumed by anyone other than the intended recipient. *Encryption* transforms data into another format in such a way that only specific individual(s) can reverse the transformation. It uses a key, which is kept in secret, and the algorithm, in order to perform the encryption operation. As such, the algorithm and key are all required to turn encrypted data back to the plaintext.

**Examples**: AES, Blowfish, [[RSA]]
## Hashing
**Hashing** serves the purpose of ensuring integrity, i.e. making it so that if something is changed you can know that it’s changed. Technically, hashing takes arbitrary input and produce a fixed-length string that has the following attributes:
- The same input will always produce the same output.
- Multiple disparate inputs should not produce the same output.
- It should not be possible to go from the output to the input.
- Any modification of a given input should result in drastic change to the hash.

*Hashing* is used in conjunction with [[Theory/Misc#^a1|authentication]] to produce strong evidence that a given message has not been modified. This is accomplished by taking a given input, hashing it, and then signing the hash with the sender’s private key.

**Examples**: SHA-3, MD5
## Obfuscation
**Obfuscation** is used to make something harder to understand, usually for the purposes of making it more difficult to attack or to copy. One common use is the obfuscation of source code so that it’s harder to replicate a given product if it is reverse engineered. It’s important to note that obfuscation is not a strong control (like properly employed *Encryption*) but rather an obstacle. It, like encoding, can often be reversed by using the same technique that obfuscated it.

**Examples**: JavaScript Obfuscator, ProGuard

