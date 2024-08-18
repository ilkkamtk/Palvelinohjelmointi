# Passkey Authentication

Passwords have several drawbacks: they are hard to remember, easy to forget, and vulnerable to being cracked. Users often reuse passwords across multiple services or create weak passwords, which increases their risk of being compromised. While traditional username/password authentication can be made more secure with multi-factor authentication (MFA), this adds complexity to the authentication process.

To address these issues, the W3C has published the [Web Authentication (WebAuthn) standard](https://en.wikipedia.org/wiki/WebAuthn), which is part of the [FIDO2 framework for passwordless authentication](https://fidoalliance.org/fido2/). WebAuthn is a browser API that allows users to authenticate without using passwords. Instead, it leverages public-key cryptography to authenticate users, providing a higher level of security than traditional passwords. The credentials generated through this process, known as passkeys, are unique to each service and cannot be reused across different services.

Passkeys enable users to authenticate without needing to remember or manage passwords. Authentication can be based on the device's built-in security mechanisms, such as Touch ID, Face ID, or a PIN, making it significantly more secure than traditional passwords or even two-factor authentication (2FA) methods.

## How It Works

1. A smartphone, security key or biometric device generates a public/private key pair.
2. The public key is sent to the server, while the private key remains securely stored on the user's device.
3. The server stores the public key and associates it with the user's account.
4. When authenticating, the user verifies their identity using their device's lock mechanism (e.g., Touch ID, Face ID, or a PIN).
5. The device then uses the private key to sign a challenge sent by the server.
6. The server verifies the signature using the stored public key. If the signature is valid, the user is authenticated.

---

https://simplewebauthn.dev/

