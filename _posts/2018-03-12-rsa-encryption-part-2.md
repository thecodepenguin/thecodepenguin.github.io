
# Table of Contents



    ---
    title: RSA Encryption Part 2
    layout: post
    date: 2018-03-12
    categories: Programming
    ---

For the second part of the RSA Encryption tutorial we will be finally diving
into some real-world code (Rust in this case).

First things first we need to write a function that generates a random prime
greater than \[ \sqrt{2}*2^(k-1) \]

    fn randomPrime(bits: u32) -> u32{
        /*
        Generates a random k-bit prime greater than sqrt(2)*2^(k-1)
        */
        let min: f32 = 2f32.sqrt()*(2u32.pow(bits-1) as f32);
    
        let a: u32 = 2u32.pow(bits) - 1;
        let max: f32 = a as f32;
    
        let mut rng = rand::thread_rng();
        let p = rng.gen_range(min, max) as u32;
    
        if isPrime(p) {
            return p;
        } else {
            return p-1;
        }
    }

Next we need to define a couple of helper functions that we need to get various
mathematical results. We will start with the \`isPrime\` function that we are
using in \`randomPrime\`.

    fn isPrime(number: u32) -> bool {
    
        if number <= 1 {
            return false;
        } else if number <= 3 {
            return true;
        } else if number % 2 == 0 || number % 3 == 0 {
            return false;
        } else {
            let mut i = 5;
            while i*i <= number {
                if number % i == 0 || number % (i+2) == 0 {
                    return false;
                }
                i  = i+6;
            }
            return true;
        }
    }

As the name implies, this function checks whether a number is a prime or not. It
returns a boolean depending on the result.

Then we have two functions that are implementations of two purely mathematical
concepts. First, \`gcd\` which, as the name implies, finds the *greatest common
divisor*.

    fn gcd(mut a: u32, mut b: u32) -> u32 {
        let mut t: u32;
    
        while b != 0 {
            t = b;
            b = a % b;
            a = t;
        }
    
        return a;
    }

The second function is called \`lcm\` which finds the *least common multiple*,
which as explained in the first part of this guide is used to find the *lambda*.

    fn lcm(a: u32, b: u32) -> u32 {
        return (a*b)/gcd(a, b);
    }

Finally we define a struct to hold the keys and a function that, given the size
of the keys (in bits) will generate a public key (in two parts) and a private
key.

    struct keys {
        pub_key_1: u64,
        pub_key_2: u32,
        priv_key: u32
    }
    
    
    fn generate(keysize: u32) -> keys {
        let e: u32 = 2u32.pow(63);
        let mut p: u32;
        let mut q: u32;
        let mut lambda: u32;
    
        while {
            p = randomPrime(keysize/2);
            q = randomPrime(keysize/2);
            lambda = lcm(p-1, q-1);
    
            gcd(e, lambda) != 1 || ((p-q) as i64).abs() >= 2i64.pow(keysize/2 - 100)
        } {}
    
        return keys {pub_key_1: (p*q) as u64, pub_key_2: e, priv_key: (1/e % lambda) }
    }

In order to then encrypt or decrypt a message we define two aptly named functions.

    fn encrypt(message: u32, pub_key_1: u64, pub_key_2: u32) -> u64 {
        return message.pow(pub_key_2) as u64 % pub_key_1;
    }
    
    fn decrypt(message: u64, priv_key: u32, pub_key_1: u64) -> u64 {
        return message.pow(priv_key) as u64 % pub_key_1;
    }

They each take the message and the required keys (as explained in the first part
of the guide) to decrypt or encrypt the message.

The full example.

     extern crate rand;
     use rand::Rng;
     use std::u64;
    
     struct keys {
         pub_key_1: u64,
         pub_key_2: u32,
         priv_key: u32
     }
    
    
     fn generate(keysize: u32) -> keys {
         let e: u32 = 2u32.pow(63);
         let mut p: u32;
         let mut q: u32;
         let mut lambda: u32;
    
         while {
             p = randomPrime(keysize/2);
             q = randomPrime(keysize/2);
             lambda = lcm(p-1, q-1);
    
             gcd(e, lambda) != 1 || ((p-q) as i64).abs() >= 2i64.pow(keysize/2 - 100)
         } {}
    
         return keys {pub_key_1: (p*q) as u64, pub_key_2: e, priv_key: (1/e % lambda) }
    
     }
    
     fn encrypt(message: u32, pub_key_1: u64, pub_key_2: u32) -> u64 {
         return message.pow(pub_key_2) as u64 % pub_key_1;
     }
    
     fn decrypt(message: u64, priv_key: u32, pub_key_1: u64) -> u64 {
         return message.pow(priv_key) as u64 % pub_key_1;
     }
    
     fn randomPrime(bits: u32) -> u32{
         /*
         Generates a random k-bit prime greater than sqrt(2)*2^(k-1)
         */
         let min: f32 = 2f32.sqrt()*(2u32.pow(bits-1) as f32);
    
         let a: u32 = 2u32.pow(bits) - 1;
         let max: f32 = a as f32;
    
         let mut rng = rand::thread_rng();
         let p = rng.gen_range(min, max) as u32;
    
         if isPrime(p) {
             return p;
         } else {
             return p-1;
         }
     }
    
     fn gcd(mut a: u32, mut b: u32) -> u32 {
         let mut t: u32;
    
         while b != 0 {
             t = b;
             b = a % b;
             a = t;
         }
    
         return a;
     }
    
     fn lcm(a: u32, b: u32) -> u32 {
         return (a*b)/gcd(a, b);
     }
    
     fn isPrime(number: u32) -> bool {
    
         if number <= 1 {
             return false;
         } else if number <= 3 {
             return true;
         } else if number % 2 == 0 || number % 3 == 0 {
             return false;
         } else {
             let mut i = 5;
             while i*i <= number {
                 if number % i == 0 || number % (i+2) == 0 {
                     return false;
                 }
                 i  = i+6;
             }
             return true;
         }
    }

