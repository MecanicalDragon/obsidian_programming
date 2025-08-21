Java provides two ways to generate random numbers:
- `java.util.Random` which generates pseudo random numbers
- `java.security.SecureRandom` which generates cryptographically strong pseudo random numbers

**Random** is pretty plain and uses “a 48-bit seed which is modified using a linear congruential formula” and it is “not cryptographically secure”. It is the one of the oldest algorithms to generate random numbers, and basically it does the following: `X_(n+1) = (aX_n + c) mod m`; where `m` – modulus, `a` – multiplier, `c` – increment, `X_0` – the seed (`start value XOR System.nanoTime()`) The problem is it is not difficult to predict the next random number in the sequence if you know the two before.

**SecureRandom** has an absolutely distinct implementation. First, it has a bunch of algorithms to use:
- DRBG
- SHA1PRNG (stands for SHA-1 Platform Random Number Generator)
- NATIVEPRNGBLOCKING
- NATIVEPRNGNONBLOCKING
- NATIVEPRNG (default, platform dependent and configured in `java.security` file)

Up to Java 8 `java.security` resides in `$JAVA_HOME/jre/lib/security`, in later implementations it's in `$JAVA_HOME/conf/security`.

Among other properties in `java.security` file there exist these entries:
- List of security providers (13 entries in Java 11-17). `security.provider.1=sun.security.provider.Sun`. In runtime security providers are queried in this list’s order to provide requested RNG algorithm, so *Sun* is the first candidate.
- `securerandom.source=file:/dev/random`. This is a primary source of seed data for the *NativePRNG*, *SHA1PRNG* and *DRBG* SecureRandom implementations in the *Sun* provider. Other implementations might also use this property.

`securerandom.source` points to one of special *Entropy Gathering Devices* (*EGD*) that are represented in Unix-like-systems as files `/dev/random` and `/dev/urandom`. So, on Unix-like systems *NativePRNG*, *SHA1PRNG* and *DRBG* implementations obtain seed data from the specified *EGD*. `securerandom.source` can be overridden with `java.security.egd` System property:
`java -Djava.security.egd=file:/dev/urandom …`

`/dev/random` is a full entropy source, meaning it only gives you truly random numbers read from physical phenomena such as your mouse movements; it blocks thread until it has enough entropy to satisfy a read request with unpredictable data.

`/dev/urandom` is the output of a RNG, it derives pseudo-randomness from whatever is available, without blocking. ***BUT IT DOESN’T MEAN URANDOM IS INSECURE!*** The only instant where `/dev/urandom` might imply a security issue due to low entropy is during the first moments of a fresh, automated OS install; if the machine booted up to a point where it has begun having some network activity then it has gathered enough entropy to provide randomness of high enough quality for all practical usages.
[Myths about /dev/urandom](https://www.2uo.de/myths-about-urandom/) and [point why DRBG is preferred](https://stackoverflow.com/questions/58991966/what-java-security-egd-option-is-for)

So, Java reads `securerandom.source` security property and `java.security.egd` System property. The default behavior is to obtain seed from `/dev/random` and random numbers from `/dev/urandom`. "`file`" is the only currently supported protocol type. If an exception occurs while accessing the specified URL:
- For *NativePRNG* default value `/dev/random` will be used.  If neither are available, implementation will be disabled.
- For *SHA1PRNG* and *DRBG* the traditional system/thread activity algorithm will be used.

*NativePRNG* actually comes in three following variants:

| NativePRNG Variant    | Seed Source  | Pseudo Random Source |
| --------------------- | ------------ | -------------------- |
| NativePRNG            | /dev/random  | /dev/urandom         |
| NATIVEPRNGBLOCKING    | /dev/random  | /dev/random          |
| NATIVEPRNGNONBLOCKING | /dev/urandom | /dev/urandom         |
Variants can’t be switched by property but obtained in the code with `SecureRandom.getInstance("NATIVEPRNGBLOCKING")` if available. Before Java 9 *SHA1PRNG* is selected when no *PRNG* algorithm is available, or when *NativePRNG* is unavailable. *SHA1PRNG* is a pure Java implementation, that may or may not use `/dev/[u]random`, depending on the `java.security.egd` System property or `securerandom.source` Security property.

Java 9 introduces *JEP 273*, which contains the implementation of three Deterministic Random Bit Generator (*DRBG*) methods: hash Function based *Hash_DRBG* and *HMAC_DRBG*, and block Cipher based *CTR_DRBG*

There is a new section in `java.security` file regarding to *DRBG* and `securerandom.drbg.config` Security parameter. The value of this property is a comma-separated list of all configurable aspects. The aspects can appear in any order, but the same aspect can only appear at most once. All subtleties can be found in `java.security` file. Default value is an empty string, which is equivalent to `securerandom.drbg.config=Hash_DRBG,SHA-256,128,none`

It is important to know that you need a randomness source to initialize/seed the *DRBG* method, thus a DRBG is actually composed of a DRBG method and a randomness source. As the name indicates, a DRBG is initialized with a seed from a randomness source, and then generates random bits with an algorithm, so it is a deterministic process. Some recommendations for how to use DRBG are described at the end of [this](https://metebalci.com/blog/everything-about-javas-securerandom/) article.

On Windows, since it has neither `/dev/random` nor `/dev/urandom` URL fails to resolve. This triggers a final mechanism to generate randomness and can delay initialization by around 5 seconds. Specifying these files enables the native Microsoft CryptoAPI seeding mechanism for *SHA1PRNG* and *DRBG*. For *NativePRNG* Windows native implementation uses *SunMSCAPI* provider. *DRBG* is default for Windows since Java 9.

[source](https://www.baeldung.com/java-security-egd) and [source 2](https://metebalci.com/blog/everything-about-javas-securerandom/)
