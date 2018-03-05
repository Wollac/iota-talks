# IOTA on the Ledger Nano S

Wolfgang Welz

<i class="fa fa-github"></i> wollac

---

## Personal Security Devices "Hardware Wallets"

* True Random Number Generator (TRNG)
* Isolate cryptographic secrets (PGP, Bitcoin Keys) in a tamper-proof and eavesdropping-proof Secure Element
* Private keys are never exposed
* Confirm transactions on the device
* Pin code for physical security

+++

### Generate Address/Key

What if I Lose My Hardware Wallet?

* Device generates public/private keys
* Hierarchical Deterministic wallet, BIP39/BIP44
* Single seed (512 bit) is used to derive all keys
* 24-word mnemonic out of 2048 words (BIP39)

![](https://i.imgur.com/kJ0Eb3M.png)

+++

### Transactions

![](https://www.ledgerwallet.com/images/products/lns/video/transaction.png)

* Are signed on the device with private key
* Need to be confirmed and verified on the device
* Transferred back to the host


---

## Ledger Nano S

* USB, OLED display, two buttons
* Security environment BOLOS

![](https://www.ledgerwallet.com/images/products/lns/ledger-nano-s-plug-large.png)

+++

### Hardware Architecture

ST31 Secure Element and "Router" STM32 MCU

![](http://ledger.readthedocs.io/en/latest/_images/bolos_architecture.png)

+++

### Blockchain Open Ledger Operating System

> BOLOS allows users to (...) install applications that let them do more with their cryptographic secrets, while protecting the device and other applications from malicious code.

+++

### Software

* Embedded C
* ARM compiler
* Code runs on ST31
* Slow, hardware support for crypto functions
* 4K total RAM, a little over 2K usable

Ledger Nano S SDK and Python Loader

```
make
make load
```

---

### Team & Project History

[IOTA-Ledger](https://github.com/IOTA-Ledger)/**[blue-app-iota](https://github.com/IOTA-Ledger/blue-app-iota)**

* Peter Willemsen
<i class="fa fa-github"></i> peterwilli
[codebuffet.co](http://codebuffet.co/)

* diskings
<i class="fa fa-github"></i> tylerhann

* Wolfgang Welz
<i class="fa fa-github"></i> wollac

---

## C Implementation of IOTA using 2KB RAM

+++

#### IOTA -- Balanced Ternary

$$\sum_{k=0}^n t_k \cdot 3^k,\ t_k\in\{-1,0,1\}$$

* One trit (t) $\in \{-1,0,1\}$
* One tryte (T) of 3t: $\sum_{k=0}^2 t_k \cdot 3^k \in\{-13,\dots,13\}$
* Base-27 encoding, each tryte $\in \{N,O,\dots,Z,9,A,B,\dots,M\}$
* Least significant trit first (Little Endian)

+++

#### IOTA -- Hashing

* Kerl (Curl replacement) -- 243t chunks
* Keccak-384 (48B hash)
* 243t <i class="fa fa-long-arrow-right"></i> 386b <i class="fa fa-long-arrow-right"></i> 48B and 2b
* 48B two's complement in Big Endian

| Type | 3-Size | 2-Size |
| +++ | +++--:| +++-- |
| Address | 243t | 81 chars |
| Private Key | 13,122t | 2,592B |
| Signature | 13,122t | 4,274 chars |

:::info
Work on `uint32_t[12]` where possible!
:::

+++

#### IOTA -- Public Address

* Private key is generated from seed and index
* "Squeezed" for Public Address

![](https://upload.wikimedia.org/wikipedia/commons/thumb/7/70/SpongeConstruction.svg/320px-SpongeConstruction.svg.png)

:::info
* Complete private key is not needed
* 243t (48B) chunks $P_0,\dots$ are sufficient
* Keccak context is ~400B <i class="fa fa-arrow-right"></i> ~1KB for generation
:::

Unset last trit in binary & hardware Keccak <i class="fa fa-arrow-right"></i> 2s

+++

#### IOTA -- Transaction Bundle

$$\underbrace{\text{address}}_{243t},\underbrace{ \underbrace{\text{value}}_{81t}, \underbrace{\text{tag}}_{81t}, \underbrace{\text{idx}}_{27t}, \underbrace{\text{last}}_{27t}, \underbrace{\text{timestamp}}_{27t}}_{243t}$$

Each transaction (96B) is absorbed for hash

```python=
  while('M' in normalized_hash) {
    increment_trits(bundle.transaction[0].tag)
    normalized_hash = normalized_bundle_hash(bundle)
  }
```

:::info
* Store up to 10 transactions <i class="fa fa-arrow-right"></i> ~1KB
* $|\text{Bundle}| = |\text{Out}| + 2\cdot |\text{In}|$
:::

+++

#### IOTA -- Input Signing

* Winternitz signature (chunk 1T) of normalized hash (no checksum) <i class="fa fa-arrow-right"></i> 19,683t i.e. 2,187T per tx

![](https://upload.wikimedia.org/wikipedia/commons/thumb/7/70/SpongeConstruction.svg/320px-SpongeConstruction.svg.png)

```python=
  z[i] = bytes = keccak.digest()
  keccak.reset()
  bytes = [~byte for byte in bytes] #flip bits
  keccak.absorb(bytes)
```

:::info
48B are sufficient to preserve Kerl state after $Z_k$
Generate and transmit 9 fragments of 243T <i class="fa fa-arrow-right"></i> 1s
:::

---

### IOTA on the Ledger -- State

* Working PoC IOTA implementation
**TODO:**
* I/O, APDU API
* Web Wallet, integration into Trinity, etc.
* Getting accepted by Ledger
* "baby proofing"

+++

### Shortcomings, Attack Vectors

:::danger
IOTA is great for M2M, a little less for humans!
:::

:::warning
Invalid transactions are useless in BTC etc., but can be used in IOTA to induce key reuse.
:::
1. Invalid transactions by the wallet,
2. Faked balances by the full node,
3. Second ledger (same seed)

can trick the user into signing an input twice.

:::info
<i class="fa fa-arrow-right"></i> user confirmation on display
<i class="fa fa-arrow-right"></i> keep track of address index in Ledger
:::

---

### Want to Say Hi or Donate?

* Discord
[discord.gg/U3qRjZj](https://discord.gg/U3qRjZj)
* IOTA
![](https://i.imgur.com/KWW3C10.png)

```
J9KPGBTWIKTRBIWXNDCZUWWWVVESYVISFJIY9GCMGVLQXFJBDAKLLN9PNAZOOU
ZFZDGDSFPWCTJYILDF9WOEVDQVMY
```
