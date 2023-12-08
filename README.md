# Randstorm

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Description
From 2011 - 2015 many popular crypto exchanges used BitcoinJS to generate private keys. 

There was an issue in BitcoinJS. Many browsers lacked the definition of `window.crypto.random`, which poses an issue for Bitcoin wallets utilizing a pre-2015 version of jsbn. 

Although `window.crypto.random` was present in Netscape Navigator 4.x, by the time BitcoinJS gained traction, this function was no longer available in the browsers used for generating cryptocurrency wallets. Consequently, this led to a silent failure of the `window.crypto.random` call in JSBN when employed by early versions of BitcoinJS, forcing entropy to be gathered from `Math.random()`.

It's important to note that using `Math.random()` for cryptographic key material generation is discouraged. However, during the 2011-2015 timeframe, `Math.random()` experienced issues on all major browsers. Despite its inadequacy for cryptographic purposes, it was unfortunately relied upon during that period.

![Project Image](SecureRandom.png)

This program generates a Random Private Key using the python equivalent to SecureRandom() in JSBN javascript library with Math.random(). 
Rather than using the more secure libraries to generate private keys, this private key generator function emulates the weak private key generation used in the JSBN library from 2011 - 2015:

```python
class MathRandomSimulator:
    # Equivalent to SecureRandom() used in JSBN javascript library with Math.random()
    def __init__(self, psize=32):
        self.rng_pool = bytearray()
        self.rng_pptr = 0
        self.rng_psize = psize
        
        while len(self.rng_pool) < self.rng_psize:
            t = int(random.random() * 65536) 
            self.rng_pool.extend(t.to_bytes(2, 'big'))

        self.rng_pptr = 0

    def next_bytes(self, size):
        return self.rng_get_bytes(size)

    def rng_get_bytes(self, size):
        result = bytes(self.rng_pool[self.rng_pptr:self.rng_pptr + size])
        self.rng_pptr += size
        return result
```

The HEX string is created from MathRandomSimulator and converted to a P2P Bitcoin address in thie following format:

<pre>
    Hex: f4389d0921ead29f272294ea790cf4112140e86e347d1933fc302373fb451bdc
    P2P: 1FaVN8XPyNHchgkNRZMwBQGqTMf531yebX
</pre>

The generated P2P Bitcoin address is cross-checked against the addresses in the database. 
## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

## Installation

Provide step-by-step instructions on how to install your project. You can include code snippets or commands if necessary.

```bash
npm install
