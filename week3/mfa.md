# Multi-Factor Authentication

## What is Multi-Factor Authentication?

* **MFA** is a security system that requires more than one method of authentication from independent categories of
  credentials to verify a user's identity.
* The goal of MFA is to create a layered defense and make it more difficult for an unauthorized person to access a
  target such as a physical location, computing device, network, or database.
* **Examples of MFA factors** include a combination of:
    * **Something you know** (e.g., password, PIN)
    * **Something you have** (e.g., smart card, security token, mobile device)
    * **Something you are** (e.g., biometric data like fingerprint, facial recognition)
    * **Somewhere you are** (e.g., geolocation, IP address)
    * **Something you do** (e.g., typing pattern, behavioral biometrics)

---

## Pros and Cons of MFA

* **Pros**:
    * **Enhanced security**:
        * MFA provides an additional layer of security beyond just a password, making it more difficult for attackers to
          gain unauthorized access.
    * **Reduced risk of unauthorized access**:
        * Even if one factor is compromised, the attacker would still need to bypass the other factor(s) to gain access.
    * **Compliance**:
        * MFA is often required by regulations and standards to protect sensitive data.
    * **User-friendly**:
        * MFA can be implemented in a user-friendly way, such as using push notifications or biometric authentication.

* **Cons**:
    * **Complexity**:
        * MFA can add complexity to the authentication process, which may lead to user frustration or confusion.
        * **Dependency on additional factors**:
            * Users may not always have access to the additional factors required for MFA, such as a mobile device or
              security key.
            * Battery life, network connectivity, or hardware failure could also impact the availability of the
              additional factors.

---

## Types of MFA

* **Two-factor authentication (2FA)**:
    * The most common implementation of MFA that requires the user to authenticate using two of the MFA factors.
    * **Two-step verification** / **Two-step authentication**:
        * Requires the user to authenticate using two steps, typically using two factors (e.g., password + OTP sent to a
          phone). The terms are often used interchangeably with 2FA, but "two-step" emphasizes the process rather than
          the number of factors.
    * **Universal 2nd Factor (U2F)**:
        * A specific 2FA method that uses a physical security key to authenticate the user after the primary factor (
          typically a password).

* **Time-based One-Time Password (TOTP)**:
    * Generates a time-limited, one-time passcode (e.g., via an app like Google Authenticator) as a second factor.

* **Push notifications**:
    * A push notification is sent to the user’s device, requiring approval to complete the authentication.

* **Biometric authentication**:
    * Uses biometric data (e.g., fingerprint, facial recognition) as one of the factors.

---

## MFA in Web Applications

* **MFA in web applications**:
    * **Password + OTP**:
        * The user enters their password and receives a one-time passcode (OTP) via SMS, email, or an authenticator app.
    * **Password + biometric**:
        * The user enters their password and uses a biometric factor (e.g., fingerprint, facial recognition) to
          authenticate.
    * **Password + security key**:
        * The user enters their password and uses a physical security key to authenticate. Physical security key could
          be a USB key or a Bluetooth-enabled device.

---

## Assignment: MFA Implementation in Node.js

### Objective:

Implement Multi-Factor Authentication (MFA) in a Node.js application using a time-based one-time password (TOTP) as the
second factor.

### MFA Registration Process:

1. **User Registration to Auth API:**
    - The client sends user details (name, email, password) to the server.
    - The server registers the user with the Auth API.

2. **Generate 2FA Secret:**
    - The server generates a new 2FA secret using `OTPAuth.Secret`.

3. **Create TOTP Instance:**
    - A TOTP (Time-based One-Time Password) instance is created with the user's email and the generated secret.

4. **Store 2FA Data:**
    - The server stores the user's 2FA secret and related information (e.g., `userId`, `email`) in the MongoDB database.

5. **Generate and Return QR Code:**

    - The server generates a QR code using the TOTP instance and sends the QR code URL back to the client.

6. **Display QR Code:**

    - The client displays the QR code to the user, who scans it with an authenticator app (e.g., Google Authenticator).

### MFA Login Process:

1. **User Enters 2FA Code:**

    - The user enters their email and the 2FA code from their authenticator app.

2. **Verify 2FA Code:**

    - The server retrieves the stored 2FA data (secret) from the database.
    - The server creates a new TOTP instance using the retrieved secret.
    - The TOTP instance validates the provided 2FA code.

3. **User Authentication with Auth API:**

    - If the 2FA code is valid, the server retrieves the user's information from the Auth API.

4. **JWT Token Generation:**

    - The server generates a JWT (JSON Web Token) using the user's information and a secret key.

5. **Return Login Response:**

    - The server sends the JWT token and a success message back to the client.

6. **User Access:**

    - The client uses the JWT token for subsequent authenticated requests.

Use the existing Auth API from previous course and implement Multi-Factor Authentication (MFA) using a time-based
one-time password (TOTP) as the second factor. URL for the existing Auth API: https://media2.edu.metropolia.fi/auth-api/ (
VPN required at home)

`otpauth` [library](https://www.npmjs.com/package/otpauth) is One Time Password (HOTP/TOTP) library for Node.js, Deno,
Bun and browsers.

1. Clone these repositories to the same parent directory:
    * [Server](https://github.com/ilkkamtk/MFA-starter)
    * [Client](https://github.com/ilkkamtk/MFA-client-starter)
    * [Types](https://github.com/ilkkamtk/hybrid-types)
2. Install required packages: `npm i`
3. Create a new MongoDB collection for storing user MFA data.
    * You can create a new MongoDB database (e.g., 2fa_db) to store 2FA data separately from your main user data. The
      new database will store information such as the user's ID, the 2FA secret, and whether 2FA is enabled.
4. Fill out the TODOs in the server and client code to implement MFA.


