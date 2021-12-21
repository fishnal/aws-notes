# Symmetric and Asymmetric Cryptography

## Symmetric Cryptography

Symmetric cryptography uses a single key for both encryption and decryption.
* If the key gets intercepted during transmission, then any data encrypted with that key is now easy to decrypt by bad actors
* Process:
	1. Alice generates a single key
	2. Alice encrypts data using key
	3. Alice transmits key securely to Bob
	4. Bob decrypts data using key
* Common algorithms: AES, DES, Triple-DES, Blowfish

## Asymmetric Cryptography

Asymmetric cryptography uses two keys, one for encryption and one for decryption.
* Both keys are created at the same time
* One key is usually private, while the other is public
* Public keys can be shared with anyone (because it's public...)
* Both keys are required to decrypt
* Process:
	1. Bob generates two keys, one public and private, and publish the public key for others to download.
	2. Alice encrypts her data using Bob's public key
	3. Bob decrypts data using Bob's private key
* Common algorithms: RSA, Diffie-Hellman, DSA

## Differences
* Symmetric is much faster
* Symmetric is more at risk because of MITM attacks.
