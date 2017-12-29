---
title: (Part 1) A mathematical intro to the RSA algorithm
date: 2017-12-29 23:00:00
layout: post
categories: Cryptography
---

RSA (Rivest-Shamir-Adleman) is one of the oldest asymmetrical encryption
algorithms. An asymmetrical (or public-key) algorithm uses pairs of keys:
*public keys* which, as the name suggests can be made public, and *private
keys* which are known only by the owner of each [private] key.

RSA is relatively slow and as such is not generally used to compile user data.
It is quite useful when transferring messages though. It's also one of the
algorithms supported by [GPG](https://gnupg.org) among others.

For a simple example, let's take two GPG users who want to exchange encrypted
messages with each other. Both of them are using GPG for the first time. 

The first thing that happens is that GPG generates a key pair (public and
private). Both of these keys are basically a pair of strings. User A wants to
send a message to User B but to encrypt said message in order for it to be
read only by User B, he needs User B's public key (let's call this *PubB*) so
he asks for it. User B sends it to User A who takes *PubB*, and through GPG
encrypts the message (let's call the encrypted message *EncMessage*). What
*EncMessage* is is pretty much just a bunch of gibberish impossible to read by
any human or machine. User A sends *EncMessage* to User B who receives it. Now
because it's encrypted using *PubB*, it can only be decrypted with the private
key counterpart of *PubB* (User B's private key, let's call this private key
*PrivB*). *PrivB* is (or at least should be at all times) private, and known
only by User B. This way anyone can send encrypted messages to User B who can
only be decrypted by User B.


# The Algorithms

Alright, now that we established the higher-level usage of RSA, let's get down
to the mathematical explanation. First, the key generation:


## Key generation

1.  Choose two random, large, distinct prime integers *p* and *q*.
2.  Compute *n=pq*.
3.  Compute $$ \lambda(n)=lcm(p-1, q-1) $$
4.  Choose an integer *a* such that $$ 1 < a < \lambda(n) $$ and $$ gcd(a,
           \lambda(n)) = 1 $$; in other words, $$ a $$ and $$ \lambda(n) $$ are
           coprime.
5.  Determine $$d$$ as $$ d \equiv a^{-1} \pmod{\lambda(n)} $$

The *public key* consists of $$n$$ and the exponent $$a$$. The *private key*
consists of $$n$$ and the exponent $$d$$.


## Encryption

To encrypt a message $$ M $$ you need to compute the cyphertext $$c$$ using a
*public exponent* $$a$$:

$$ c \equiv M^a \pmod{n} $$


## Decryption

$$ M $$ can then be recovered from $$c$$ by using the private exponent $$d$$ by
computing:

$$ c^d \equiv (M^a)^d \equiv M \pmod{n} $$


# Mathematical example

-   Choose two distinct prime numbers (NB: they should be randomly generated
    and must be quite large, but for the purposes of this example we will use
    two small primes instead).
    
    $$ p = 13 $$ and $$ q = 29 $$

-   Compute $$ n = pq $$:
    
    $$ n = 13 \times 29 = 377 $$

-   Compute $$ \lambda(n) = lcm(p-1, q-1) $$
    
    $$ \lambda(377) = lcm(12, 28) = 84 $$

-   Choose a number $$ 1 < a < 84 $$ that is a coprime to 84.
    
    $$ a = 67 $$

-   Compute $$d$$:
    
    $$ d \equiv a^{-1} \pmod{\lambda(n)} $$
    $$ d \times 67 \pmod{84} = 1 $$
    $$ d = 79 $$

The public key is ($$ n = 377 $$, $$ a = 67 $$). The encryption function is:

$$ c(M) = M^67 \pmod{377} $$

The private key is (n = 377, d = 79). The decryption function is:

$$ M(c) = c^79 \mod 377 $$

*Whew*, it wasn't that brief, was it? Hopefully you made it until here. Next
week I will publish Part 2 with a code example that shows the algorithm in
action.
