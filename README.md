# Chug
[![License](https://img.shields.io/static/v1?style=flat-square&label=license&message=MIT&color=red)](LICENSE)

A [symmetric-key algorithm](https://en.wikipedia.org/wiki/Symmetric-key_algorithm) and collection of padding algorithms.

## Table of Contents

- [Disclaimer](#disclaimer)
- [Security Rating](#security-rating)
- [Known Vulnerabilities](#known-vulnerabilities)
- [Overview](#overview)
    - [Padding](#padding)
        - [Zero Suffixed Random](#zero-suffixed-random)
        - [Length Prefixed Random](#length-prefixed-random)
    - [Encryption](#encryption)
    - [Decryption](#decryption)
    - [Variable Definitions](#variable-definitions)
- [Use as a One-time Pad](#use-as-a-one-time-pad)
- [Implementations](#implementations)
- [Suggestions & Bug Reports](#suggestions--bug-reports)
- [Credits](#credits)
- [License](#license)

## Disclaimer

Chug is an enthusiast algorithm and is not intended for use in any security applications.

## Known Vulnerabilities

- [Known plaintext attack](https://gist.github.com/sblmnl/222c786ddebd00e2dbae7ab361fb2618)

Have a vulnerability to report? Open an [issue](https://github.com/sblmnl/Chug/issues) and attach any code or documentation you have for the vulnerability.

## Security Rating

The security rating of [Chug](https://github.com/sblmnl/chug), if used as a conventional [symmetric-key algorithm](https://en.wikipedia.org/wiki/Symmetric-key_algorithm), is **8-bits**.

## Overview

### Padding

#### Zero Suffixed Random

Given a list of numbers `P` where `N1` is the amount of numbers in `P`, an empty list of numbers `Z` where `N2` is the  
amount of numbers in `Z`, and a block size of `L`, append random non-zero numbers to `Z` until `N2` is 1 less than the  
difference between `N1` and the smallest multiple of `L` that is greater than `N1`. Then, append 1 zero to `Z`.  
Finally, append all numbers from `P` to `Z`.
  
Example
```
P = (0, 1, 2)
L = 8
Z = (?, ?, ?, ?, 0, 0, 1, 2)
```

#### Length Prefixed Random

Given a list of numbers `P` where `N1` is the amount of numbers in `P`, an empty list of numbers `Z` where `N2` is the  
amount of numbers in `Z`, and a block size of `L`, append random numbers to `Z` until `N2` is 1 less than the  
difference between `N1` and the smallest multiple of `L` that is greater than `N1`. Then, insert `N2` into the front of  
 `Z`. Finally, append all numbers from `P` to `Z`.
  
Example
```
P = (0, 1, 2)
L = 8
Z = (4, ?, ?, ?, ?, 0, 1, 2)
```

### Encryption

```
K = (0, 1, 2, 3)
P = (0, 1, 2, 3, 4)
C = (
        (((P(1) + K(1)) - K(2)) + K(3)) - K(4),
        (((P(2) + K(2)) - K(3)) + K(4)) - K(1),
        (((P(3) + K(3)) - K(4)) + K(1)) - K(2),
        (((P(4) + K(4)) - K(1)) + K(2)) - K(3),
        (((P(5) + K(1)) - K(2)) + K(3)) - K(4)
    ) = (-2, 3, 0, 5, 2)
```

### Decryption

```
K = (0, 1, 2, 3)
C = (-2, 3, 0, 5, 2)
P = (
        (((C(1) + K(4)) - K(3)) + K(2)) - K(1),
        (((C(2) + K(1)) - K(4)) + K(3)) - K(2),
        (((C(3) + K(2)) - K(1)) + K(4)) - K(3),
        (((C(4) + K(3)) - K(2)) + K(1)) - K(4),
        (((C(5) + K(4)) - K(3)) + K(2)) - K(1)
    ) = (0, 1, 2, 3, 4)
```

### Variable Definitions

- **P** : The plaintext
- **L** : The block size
- **Z** : The padded message
- **K** : The key
- **C** : The ciphertext

## Use as a [One-time pad](https://en.wikipedia.org/wiki/One-time_pad)

To use [Chug](https://github.com/sblmnl/chug) as a [One-time pad (OTP)](https://en.wikipedia.org/wiki/One-time_pad) it would require the use of the key in place of the ciphertext.

This means that instead of keeping the key secret, the ciphertext must be kept secret as it is effectively the key.

**Example**  
Alice wants to say `hello` to Bob, but she wants everyone to think she's saying `goodbye` to Bob. Alice would encrypt `hello` as her plaintext with `goodbye` as her key and send `goodbye` to Bob. Alice must then secretly exchange the resulting ciphertext of encrypting `hello` with Bob.

## Implementations

- **C#** - [**ChugSharp**](https://github.com/sblmnl/ChugSharp) by [**sblmnl**](https://github.com/sblmnl)

## Suggestions & Bug Reports

If you would like to suggest a feature or report a bug please see the [**issues**](https://github.com/sblmnl/chug/issues) page.

## Credits

- [**sblmnl**](https://github.com/sblmnl) - **Creator**

## License

This project is licensed under the **MIT** license - see the [**LICENSE**](LICENSE) file for details.
