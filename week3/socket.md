# WebSocket Assignment

## Objective

Add WebSockets to the previous MFA assignment to delete the need for login button in the form. So when the user enters email and the 2FA code without the need to submit the form, the server will send the user a message to the client that the user is authenticated.

Create a new branch `websocket` to both server and client before starting the assignment.

Yes, that's correct! With Socket.IO, you can achieve a seamless and interactive authentication flow where the server can notify the client in real time when the user is successfully authenticated after entering the 2FA code. Here's how this process could work:

### Real-Time 2FA Authentication Flow with Socket.IO

1. **User Input:**
    - The user enters their email and 2FA code into the form on the client side. The form does not need to be submitted manually.

2. **Real-Time Validation:**
    - As the user enters the 2FA code, the client sends the email and code to the server using Socket.IO.

3. **Server-Side Authentication:**
    - The server receives the email and 2FA code, validates the code using the secret stored for that user, and checks if the code is correct.

4. **Server Response:**
    - If the 2FA code is correct, the server generates a JWT or session token and sends a message back to the client indicating successful authentication.
    - If the 2FA code is incorrect or expired, the server sends a message back to the client indicating that the authentication failed.

5. **Client Update:**
    - Upon receiving the authentication response, the client can update the UI accordingly. For example, it can redirect the user to a dashboard on success or show an error message on failure.
