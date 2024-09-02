# Passkey Authentication

https://www.youtube.com/watch?v=2xdV-xut7EQ

Passwords have several drawbacks: they are hard to remember, easy to forget, and vulnerable to being cracked. Users
often reuse passwords across multiple services or create weak passwords, which increases their risk of being
compromised. While traditional username/password authentication can be made more secure with multi-factor
authentication (MFA), this adds complexity to the authentication process.

To address these issues, the W3C has published
the [Web Authentication (WebAuthn) standard](https://en.wikipedia.org/wiki/WebAuthn), which is part of
the [FIDO2 framework for passwordless authentication](https://fidoalliance.org/fido2/). WebAuthn is a browser API that
allows users to authenticate without using passwords. Instead, it leverages public-key cryptography to authenticate
users, providing a higher level of security than traditional passwords. The credentials generated through this process,
known as passkeys, are unique to each service and cannot be reused across different services.

Passkeys enable users to authenticate without needing to remember or manage passwords. Authentication can be based on
the device's built-in security mechanisms, such as Touch ID, Face ID, or a PIN, making it significantly more secure than
traditional passwords or even two-factor authentication (2FA) methods.

## How It Works

1. A smartphone, security key or biometric device generates a public/private key pair.
2. The public key is sent to the server, while the private key remains securely stored on the user's device.
3. The server stores the public key and associates it with the user's account.
4. When authenticating, the user verifies their identity using their device's lock mechanism (e.g., Touch ID, Face ID,
   or a PIN).
5. The device then uses the private key to sign a challenge sent by the server.
6. The server verifies the signature using the stored public key. If the signature is valid, the user is authenticated.

---

## Libraries and Tools

- https://simplewebauthn.dev/
- https://www.hanko.io/
    - https://www.hanko.io/blog/passkeys-node-js-app/

## Assignment: Passkey Authentication Implementation in Node.js

### Objective:

Implement passkey authentication in a Node.js application using the WebAuthn standard.

### Passkey Registration Process:

1. **User Registration to Auth API:**
    - The client sends user details (e.g., username, email, password) to the server.
    - The server registers the user with the Auth API.

2. **Generate Registration Options:**
    - The server generates Passkey registration options using the `generateRegistrationOptions` function, which includes
      a challenge for secure registration.

3. **Store Challenge and Register User in Database:**
    - The generated challenge is stored in the database.
    - A new user entry is created in the `PasskeyUser` collection with no associated devices initially.

4. **Send Registration Options to Client:**
    - The server sends the registration options and the user's email back to the client.

5. **Client Starts Registration:**
    - The client uses the registration options to initiate Passkey registration via `startRegistration`.

6. **Verify Registration Response:**
    - The client sends the registration response back to the server.
    - The server verifies the registration response using `verifyRegistrationResponse`.

7. **Store Authenticator Device:**
    - If verification is successful, the server checks if the device is already registered.
    - If not, the server stores the new authenticator device in the `AuthenticatorDevice` collection and links it to the
      user.

8. **Complete Registration:**
    - The challenge is deleted from the database, and the server fetches the user's details from the Auth API.
    - The server responds with the user details, completing the registration process.

### Passkey Login Process:

1. **Generate Authentication Options:**
    - The client sends the user's email to the server.
    - The server retrieves the user's devices from the database and generates authentication options
      using `generateAuthenticationOptions`.

2. **Send Authentication Options to Client:**
    - The server sends the authentication options back to the client.

3. **Client Starts Authentication:**
    - The client uses the authentication options to initiate Passkey authentication via `startAuthentication`.

4. **Verify Authentication Response:**
    - The client sends the authentication response back to the server.
    - The server verifies the authentication response using `verifyAuthenticationResponse`.

5. **Authenticate User:**
    - If verification is successful, the server updates the authenticator's counter to prevent replay attacks.
    - The server deletes the used challenge from the database.

6. **Generate JWT and Complete Login:**
    - The server retrieves the user's details from the Auth API and generates a JWT token.
    - The server sends the JWT token and user details back to the client, completing the login process.

Use the existing Auth API from previous course and implement Passkey authentication using the [simplewebauthn](https://simplewebauthn.dev/) library. Study the [server](https://simplewebauthn.dev/docs/packages/server) and [client](https://simplewebauthn.dev/docs/packages/browser) examples and implement the registration and login processes by replacing the TODOs in the provided code.

1. Clone these repositories to the same parent directory:
   * [Server](https://github.com/ilkkamtk/passkey-server-starter)
   * [Client](https://github.com/ilkkamtk/passkey-client-starter)
   * [Types](https://github.com/ilkkamtk/hybrid-types)
2. Install required packages: `npm i`
3. Create a new MongoDB collection for storing user passkey data.
   * You can create a new MongoDB database (e.g., passkey) to store passkey data separately from your main user data. The
     new database will store information such as the user, challenge, and authenticator devices.
4. Fill out the TODOs in the server and client code to implement passkey authentication.

