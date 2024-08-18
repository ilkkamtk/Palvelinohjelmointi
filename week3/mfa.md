# Multi-Factor Authentication

## What is Multi-Factor Authentication?

* **MFA** is a security system that requires more than one method of authentication from independent categories of credentials to verify a user's identity.
* The goal of MFA is to create a layered defense and make it more difficult for an unauthorized person to access a target such as a physical location, computing device, network, or database.
* **Examples of MFA factors** include a combination of:
  * **Something you know** (e.g., password, PIN)
  * **Something you have** (e.g., smart card, security token, mobile device)
  * **Something you are** (e.g., biometric data like fingerprint, facial recognition)
  * **Somewhere you are** (e.g., geolocation, IP address)
  * **Something you do** (e.g., typing pattern, behavioral biometrics)

---

## Types of MFA
* **Two-factor authentication (2FA)**:
  * The most common implementation of MFA that requires the user to authenticate using two of the MFA factors.
  * **Two-step verification** / **Two-step authentication**:
    * Requires the user to authenticate using two steps, typically using two factors (e.g., password + OTP sent to a phone). The terms are often used interchangeably with 2FA, but "two-step" emphasizes the process rather than the number of factors.
  * **Universal 2nd Factor (U2F)**:
    * A specific 2FA method that uses a physical security key to authenticate the user after the primary factor (typically a password).

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
    * The user enters their password and uses a biometric factor (e.g., fingerprint, facial recognition) to authenticate.
  * **Password + security key**:
    * The user enters their password and uses a physical security key to authenticate. Physical security key could be a USB key or a Bluetooth-enabled device.

---

## Assignment: MFA Implementation in Node.js
Use the existing Auth API from previous course and implement Multi-Factor Authentication (MFA) using a time-based one-time password (TOTP) as the second factor. URL for the existing Auth API: https://media2.metropolia.fi/auth-api/ (VPM required)

`otpauth` [library](https://www.npmjs.com/package/otpauth) is One Time Password (HOTP/TOTP) library for Node.js, Deno, Bun and browsers.

1. Start a new express.js typescript project with [this template](https://github.com/ilkkamtk/ts_mongo_starter).
2. Install required packages: `npm install express otpauth qrcode`, `
   npm install --save-dev @types/express @types/qrcode`
3. Create a new MongoDB collection for storing user MFA data.
   * You can create a new MongoDB database (e.g., 2fa_db) to store 2FA data separately from your main user data. The new database will store information such as the user's ID, the 2FA secret, and whether 2FA is enabled.
4. Define the TS Type and Mongoose Schema and Model for 2FA.
   * Schema fields: userId (string, required, unique), twoFactorSecret (string, required), twoFactorEnabled (boolean, default `false`)
5. Connect to the MongoDB Database
6. 


