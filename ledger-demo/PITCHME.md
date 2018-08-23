# How to get IOTA on the Ledger

Wolfgang Welz

@fa[github] wollac

---

## IOTA on the Ledger Nano S

* 2nd Berlin IOTA Meetup<br>
[github.com/Wollac/iota-talks](https://github.com/Wollac/iota-talks)
* Demo

![](https://i.imgur.com/dPVgEL9.png)

---

## Personal Security Devices "Hardware Wallets"

* True Random Number Generator (TRNG)
* Isolate cryptographic secrets (Bitcoin Keys, IOTA Seed) in a tamper-proof and eavesdropping-proof Secure Element
* Private keys are never exposed
* Confirm transactions on the device
* Pin code for physical security

+++

### Ledger Nano S

* ST31 Secure Element, 32-bit, 30 MHz, 2 KB
* Crypto Chip for encryption, hashing...
* No Support for Ternary, Kerl/Curl, Winternitz

![](https://www.ledgerwallet.com/images/products/lns/video/transaction.png)

---

### Team & Project History

[IOTA-Ledger](https://github.com/IOTA-Ledger)/**[blue-app-iota](https://github.com/IOTA-Ledger/blue-app-iota)**

@fa[github] peterwilli ![peterwilli](https://avatars2.githubusercontent.com/u/1212814?s=32) Peter Willemsen<br>[codebuffet.co](https://codebuffet.co)

@fa[github] tylerhann ![diskings](https://avatars0.githubusercontent.com/u/5952757?s=32) diskings

@fa[github] wollac ![wollac](https://avatars1.githubusercontent.com/u/4930426?s=32) Wolfgang Welz

@fa[github] muXxer ![muXxer](https://avatars2.githubusercontent.com/u/32371094?s=32) muXxer

---

### Generate Address/Key

* One master seed to derive all currencies, accounts
* Hierarchical Deterministic wallet, BIP32/BIP39
* 24-word mnemonic out of 2048 words (BIP39)

![](https://i.imgur.com/JENRRN2.png)

+++

### IOTA Address

* Registered BIP44 path prefix<br>
```m / 44' / 0x107A' / ...```
* BIP32 defined on elliptic curves <i class="fa fa-long-arrow-right"></i><br>
Kerl derived key to get IOTA Seed
* BIP32 path is used for accounts/pages 
* IOTA has own concept of address index
* Virtually infinity accounts without reset

+++

### Limitations

* (Sub)accounts not "auditable"
* Address generation takes &asymp;2s
* Signing a bundle takes up to 20s
* Specific bundle structure &leq;4 txs

| Transaction | Properties
| --- | ---- |
| output     | mandatory, arbitrary, positive
| input      | mandatory, seed, negative
| input | optional, seed, negative
| change | optional, seed, positive

---

[IOTA-Ledger](https://github.com/IOTA-Ledger)/**[blue-app-iota](https://github.com/IOTA-Ledger/blue-app-iota)**

#### Developers

* Toolchain as Virtual Box
[fix](https://github.com/fix)/**[ledger-vagrant](https://github.com/IOTA-Ledger/lendger-vagrant)**
* Node.js library
[IOTA-Ledger](https://github.com/IOTA-Ledger)/**[hw-app-iota.js](https://github.com/IOTA-Ledger/hw-app-iota.js)**

#### Users

* Installer VM image
[IOTA-Ledger](https://github.com/IOTA-Ledger)/**[blue-app-iota-loader-alpine](https://github.com/IOTA-Ledger/blue-app-iota-loader-alpine)**
* Seed recovery
[IOTA-Ledger](https://github.com/IOTA-Ledger)/**[recover-iota-seed-from-ledger-mnemonics](https://github.com/IOTA-Ledger/recover-iota-seed-from-ledger-mnemonics)**
* Web wallet
**[iota-ledger.github.io/romeo.html](https://iota-ledger.github.io/romeo.html/)**

---

### Want to Say Hi<br>or Donate?

* Discord<br>
[discord.gg/U3qRjZj](https://discord.gg/U3qRjZj)

@snap[east sidebar]
![](assets/ADLJXS9S-small.png)
@snapend

@snap[south-west]
```
ADLJXS9SK YQKMVQFXR 9JDUUJHJW
GDNWHQZMD GJFGZOX9B ZEKDSXBSP
ZTTWEYPTN M9OZMYDQW ZXFHRTXRC
```
@snapend
