# CS50 Week 10 — Cybersecurity

## Summary
Cybersecurity protects systems, networks, and data from unauthorized access and attacks. Authentication relies on three factors: something you know (password), something you have (phone/key), and something you are (biometric). Passwords should never be stored in plaintext — hashing with a salt (using bcrypt or Argon2) makes cracking infeasible even if the database is stolen. Cryptography uses symmetric keys (same key to encrypt/decrypt, e.g. AES) and asymmetric keys (public/private pair, e.g. RSA) to secure data in transit. HTTPS uses a certificate chain to verify server identity and establish an encrypted connection. Passkeys replace passwords entirely using a public/private key pair stored on your device, making phishing impossible. Full-disk encryption and secure deletion protect data at rest.

---

## Notes

### Authentication Factors

Three factors of authentication — attackers need all that you're using:

| Factor | Type | Example |
|---|---|---|
| **Knowledge** | Something you know | Password, PIN |
| **Possession** | Something you have | Phone (OTP), hardware key |
| **Inherence** | Something you are | Fingerprint, face ID |

- **MFA (Multi-Factor Authentication)** — uses 2+ factors simultaneously
- **2FA** — most common: password + OTP sent to phone
- **OTP (One-Time Password)** — 6-digit code valid for ~30 seconds (TOTP)
- **SIM swapping** — attacker convinces carrier to transfer your number; defeats SMS 2FA

---

### Password Managers

- Store unique, random, long passwords for every site
- You only memorize one master password
- Better than reusing passwords — one breach doesn't compromise everything
- Examples: Bitwarden (open source), 1Password, Dashlane

---

### Hashing & Salting

**Never store passwords in plaintext.**

```
Plaintext:  "password123"
MD5 hash:   "482c811da5d5b4bc6d497ffa98491e38"  ← always the same
```

**Problems with unsalted hashing:**
- **Rainbow table** — precomputed table of hash → plaintext mappings
- Identical passwords produce identical hashes — attacker knows multiple accounts share a password

**Salting:**
```
Salt:       "x7Kp9Q"         (random per user, stored alongside hash)
Salted:     "password123x7Kp9Q"
Hash:       (unique even for same password)
```

- **bcrypt / Argon2** — slow hashing algorithms designed for passwords (makes brute force expensive)
- **MD5 / SHA-1** — fast, NOT suitable for passwords

---

### Cryptography

**Symmetric encryption** — same key encrypts and decrypts:
```
plaintext + key → ciphertext
ciphertext + key → plaintext
```
- Example: **AES** (Advanced Encryption Standard)
- Problem: how do you securely share the key?

**Asymmetric encryption** — public/private key pair:
```
encrypt with PUBLIC key → only PRIVATE key can decrypt
sign with PRIVATE key → anyone with PUBLIC key can verify
```
- Examples: **RSA**, **ECC**
- Public key can be shared openly; private key never leaves your device
- Used in HTTPS, SSH, passkeys

---

### HTTPS & Certificate Chain

1. Browser connects to `https://example.com`
2. Server sends its **SSL certificate** (contains public key + identity)
3. Certificate is signed by a trusted **Certificate Authority (CA)**
4. Browser verifies the chain: site cert → intermediate CA → root CA (trusted by OS)
5. Browser and server negotiate a **symmetric session key** (using asymmetric crypto)
6. All traffic encrypted with that session key

- **TLS** (Transport Layer Security) — protocol behind HTTPS
- Green padlock = encrypted connection, but doesn't guarantee the site is safe

---

### Passkeys

Passkeys replace passwords using asymmetric cryptography:

1. Device generates a **public/private key pair** for the site
2. Public key stored on the server; private key never leaves your device
3. To log in: server sends a challenge → device signs it with private key → server verifies with public key
4. **Phishing-resistant** — key pair is tied to the exact domain, fake sites can't use it
5. **WebAuthn** — the web standard behind passkeys

---

### End-to-End Encryption (E2EE)

- Messages encrypted on sender's device, only decrypted on recipient's device
- Server never sees plaintext — can't hand over readable data even if compelled
- **Signal, iMessage** — E2EE by default
- **WhatsApp** — E2EE in transit, but metadata collected
- Standard HTTPS — encrypted in transit, but server can read it

---

### Deletion & Full-Disk Encryption

**Why "deleting" isn't really deleting:**
- Emptying the trash only removes the file's pointer — data still on disk
- Secure deletion overwrites the actual bytes
- SSDs make secure deletion harder (wear leveling moves data around)

**Full-Disk Encryption (FDE):**
- Entire drive encrypted — unreadable without password/key at boot
- **BitLocker** (Windows), **FileVault** (macOS)
- Protects against physical theft

---

### Ransomware

- Malware that **encrypts all your files** using asymmetric crypto
- Attacker holds private key — demands payment for decryption
- Defense: regular offline backups (3-2-1 rule: 3 copies, 2 media types, 1 offsite)

---

## Cue Column

| Term | Definition |
|---|---|
| **Authentication factors** | Knowledge / Possession / Inherence — what you know/have/are |
| **MFA** | Multi-Factor Authentication — requires 2+ independent factors |
| **OTP** | One-Time Password — time-based 6-digit code valid ~30 seconds |
| **Rainbow table** | Precomputed hash→plaintext lookup table used to crack passwords |
| **Salt** | Random string appended to password before hashing — unique per user |
| **bcrypt/Argon2** | Slow, memory-hard hashing algorithms designed for passwords |
| **Symmetric encryption** | Same key encrypts and decrypts (e.g. AES) |
| **Asymmetric encryption** | Public key encrypts, private key decrypts (e.g. RSA, ECC) |
| **Certificate Authority** | Trusted third party that signs SSL certificates to verify identity |
| **Passkey** | Password replacement using device-stored public/private key pair |
| **E2EE** | End-to-End Encryption — only sender and recipient can read messages |
| **FDE** | Full-Disk Encryption — entire drive encrypted at rest |
| **Ransomware** | Malware that encrypts files and demands payment for the key |
