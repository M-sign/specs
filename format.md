# Data formats

## General data format

The data is represented as an ASCII string with a specific prefix and finished with a newline character.
The prefix is a four-character string that represents the type of data. 
Data is a sequence of bytes that is represented as a BASE64 encoded string.

### Private key format

```
Prefix: "KEY:"
Version: 1 byte - 0x01
Private Key: Ed25519 private key (bytes) - 64 bytes
KeyID: Last 6 bytes of SHA256(<Public Key>) - 6 bytes
Key data version 1: Last 6 bytes of SHA256(<KeyID> + <Private Key>) + <KeyID> + <Private Key>
Data: Base64(<Version> + <Key data version 1>)

Key message: <Prefix>:<Data><newline>
```

### Public key format

```
Prefix: "PUB:"
Version: 1 byte - 0x01
Public Key: Ed25519 public key (bytes) - 32 bytes
Public data version 1: Last 6 bytes of SHA256(<Public Key>) + <Public Key>
Data: Base64(<Version> + <Public Key data version 1>)

Key message: <Prefix>:<Data><newline>
```

### Signature format

```
Prefix: "SIG:"
Version: 1 byte - 0x01
Private Key: Ed25519 private key (bytes) - 64 bytes
Public Key: Ed25519 public key (bytes) - 32 bytes
KeyID: Last 6 bytes of SHA256(<Public Key>) - 6 bytes
Message: <sequence of bytes>
Signature: Ed25519_Signature(<Private Key>, SHA512(<Message>))
Signature data version 1: Last 6 bytes of SHA256(<KeyID> + <Signature>) + <KeyID> + <Signature>
Data: Base64(<Version> + <Signature data version 1>)

Signature message: <Prefix>:<Data><newline>
```
