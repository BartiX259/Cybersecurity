# Breaking RSA

**Objective:** Extract private RSA key from a vulnerable public key.

**Reconnaissance & Enumeration:**
- Scanned the target with `nmap`. Found open ports 21 (FTP) and 80 (HTTP).
- Enumerated web directories using `dirb`. Discovered a hidden public key at `/development/id_rsa.pub`.

**Analysis:**
- Inspected the public key with `ssh-keygen -l`. Confirmed a 4096-bit key length.

**Exploitation:**
- Wrote a Python script to reconstruct the private key. Used Fermat's factorization to derive prime numbers `p` and `q` from modulus `n`.
- Calculated the modular inverse to find private exponent `d`.
- Exported the forged private key to `id_rsa.pem`.

**Exploit Code:**
```python
from Crypto.PublicKey import RSA
from math import lcm, isqrt

def modinv(x, p):
    return pow(x, -1, p)

def factorize(n):
    if (n & 1) == 0:
        return (n/2, 2)
    a = isqrt(n)
    if a * a == n:
        return a, a

    while True:
        a = a + 1
        bsq = a * a - n
        b = isqrt(bsq)
        if b * b == bsq:
            break

    return a + b, a - b

with open("id_rsa.pub", "r") as f:
    key_str = f.readline()
key = RSA.import_key(key_str)
p, q = factorize(key.n)
d = modinv(key.e, lcm(p - 1, q - 1))
print(p - q)
private_key = RSA.construct((key.n, key.e, d))
k = private_key.export_key()
with open("id_rsa.pem", "w") as f:
    f.write(k.decode())
    f.write('\n')
```

**Results:**
- Numerical difference between `p` and `q`: `1502`.
- Flag captured: `breakingRSAissuperfun20220809134031`.
