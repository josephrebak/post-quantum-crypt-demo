# Post-Quantum Cryptography Demo

A minimal, hands-on demonstration of **ML-KEM** (Module-Lattice-Based Key-Encapsulation Mechanism) — one of NIST's newly standardized post-quantum cryptographic algorithms. This notebook walks through a complete key exchange between two parties using lattice-based cryptography that is secure against attacks from future quantum computers.

---

## Why This Matters

Classical key exchange algorithms like Diffie-Hellman and RSA rely on the hardness of factoring large numbers or computing discrete logarithms. Shor's algorithm — running on a sufficiently powerful quantum computer — can solve both in polynomial time, rendering them obsolete.

**ML-KEM** (formerly known as CRYSTALS-Kyber) is built on the hardness of lattice problems, specifically the **Module Learning With Errors (MLWE)** problem. No known quantum algorithm offers a meaningful speedup against MLWE, making it one of the strongest candidates for long-term cryptographic security.

In 2024, NIST finalized ML-KEM as a post-quantum standard in [FIPS 203](https://csrc.nist.gov/pubs/fips/203/final).

---

## The Key Exchange at a Glance

```
Alice                              Bob
─────                              ───
keygen()
  → public_key                ──► encaps(public_key)
  → private_key                        → shared_key
                                        → ciphertext
                        ◄──  ciphertext
decaps(private_key, ciphertext)
  → shared_key

✓ Alice's shared_key == Bob's shared_key
```

Neither party ever transmits the shared secret directly. Bob encapsulates it inside a ciphertext that only Alice (with her private key) can open.

---

## Setup

```bash
pip install kyber-py jupyter
jupyter notebook "PQC Demo.ipynb"
```

---

## What's Inside

The notebook uses **ML-KEM-512** — the 512-bit security variant — from the [`kyber-py`](https://github.com/GiacomoPope/kyber-py) library.

| Step | Party | Operation |
|------|-------|-----------|
| 1 | Alice | `ML_KEM_512.keygen()` → `(public_key, private_key)` |
| 2 | Alice→Bob | transmit `public_key` |
| 3 | Bob | `ML_KEM_512.encaps(public_key)` → `(shared_key, ciphertext)` |
| 4 | Bob→Alice | transmit `ciphertext` |
| 5 | Alice | `ML_KEM_512.decaps(private_key, ciphertext)` → `shared_key` |
| 6 | — | assert `alice_key == bob_key` ✓ |

---

## Further Reading

- [NIST FIPS 203 — ML-KEM Standard](https://csrc.nist.gov/pubs/fips/203/final)
- [CRYSTALS-Kyber paper](https://pq-crystals.org/kyber/)
- [kyber-py library](https://github.com/GiacomoPope/kyber-py)
- [NIST Post-Quantum Cryptography project](https://csrc.nist.gov/projects/post-quantum-cryptography)
