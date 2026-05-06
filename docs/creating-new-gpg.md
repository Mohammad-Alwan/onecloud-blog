---
date: '2026-05-04T22:35:01+07:00'
---
# Creating new GPG Key
## Initiating Key-Pair Generation 
1. To initiate key-pair generation interactive console, run the following command:

    ```bash
    # gpg --full-generate-key
    ```
2. Full fill interactive output
    ```bash hl_lines="11 13 21 23 27 28 29"
    gpg (GnuPG) 2.2.20; Copyright (C) 2020 Free Software Foundation, Inc.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.

    Please select what kind of key you want:
       (1) RSA and RSA (default)
       (2) DSA and Elgamal
       (3) DSA (sign only)
       (4) RSA (sign only)
      (14) Existing key from card
    Your selection? → (fullfil, default 1)
    RSA keys may be between 1024 and 4096 bits long.
    What keysize do you want? (2048) 4096 → (fullfil, default 2048)
    Requested keysize is 4096 bits
    Please specify how long the key should be valid.
             0 = key does not expire
          <n>  = key expires in n days
          <n>w = key expires in n weeks
          <n>m = key expires in n months
          <n>y = key expires in n years
    Key is valid for? (0) → (fullfil, default 0)
    Key does not expire at all
    Is this correct? (y/N) → (fullfil)

    GnuPG needs to construct a user ID to identify your key.

    Real name:  → (fulfill)
    Email address: → (fulfill, default)
    Comment:  → (fulfill)
    You selected this USER-ID:
        "<NAME> (<comment>) <email>"

    Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o
    We need to generate a lot of random bytes. It is a good idea to perform
    some other action (type on the keyboard, move the mouse, utilize the
    disks) during the prime generation; this gives the random number
    generator a better chance to gain enough entropy.
    We need to generate a lot of random bytes. It is a good idea to perform
    some other action (type on the keyboard, move the mouse, utilize the
    disks) during the prime generation; this gives the random number
    generator a better chance to gain enough entropy.
    gpg: key <keyid> marked as ultimately trusted
    gpg: revocation certificate stored as '/root/.gnupg/openpgp-revocs.d/<KEYID>.rev'
    public and secret key created and signed.

    pub   rsa4096 2026-04-16 [SC]
          <KEYID>
    uid   <NAME> (<comment>) <email>
    sub   rsa4096 2026-04-16 [E]
    ```


### List Public Keys (to get KEYID)
```bash
gpg --list-key
```

### Export GPG Public Key (ASCII-Armored Format)
```bash
gpg --armor --export <KEYID> > /path/to/source/<gpg-pub-file>
```

### Export GPG Private Key (ASCII-Armored Format)
```bash
gpg --armor --export-secret-keys <KEYID> > /path/to/source/<gpg-private-file>
```

### Create GPG Passphrase as File

```
# sample generate random using openssl_
openssl rand -base64 12 > /path/to/source/<passphrase-file>.txt
```


### Example encrypt and decrypt

=== "Encrypt"
    ```bash
    gpg --encrypt --recipient-file "/path/to/source/<gpg-pub-file>" --output "<output>.gpg" "<file>.txt"
    ```

=== "Encrypt"
    ```bash
    gpg --batch --yes --pinentry-mode loopback --decrypt --passphrase-file "/path/to/source/<passhphrase-file>.txt" --output "<output>.txt" "<file>.gpg"
    ```

