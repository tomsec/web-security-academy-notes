# WebSockets Basics

## What are WebSockets?
- **Definition**: WebSockets enable continuous communication between a user's browser and a server, unlike traditional HTTP requests that close after delivering a response.
- **Example**: In a stock trading app, WebSockets are used to keep stock prices updated in real-time without the need for the user to refresh the page.

## Security Vulnerabilities with WebSockets

### Input Handling Issues
- **Issue**: Data sent and received via WebSockets might not be sanitized, leading to vulnerabilities.
- **Example**: An application accepts a message object `{"message": "text"}`. If "text" is user-controlled and unsanitized, malicious input like `{"message": "<script>maliciousCode()</script>"}` could execute harmful scripts.

### Blind Vulnerabilities
- **Issue**: Some vulnerabilities, like those not producing immediate visible errors, may require indirect methods to detect.
- **Example**: A backend service that processes WebSocket messages might be vulnerable to SQL injection, which only shows up in database behavior or logs, making it detectable via OAST methods.

## Manipulating WebSocket Messages

- **Example Scenario**: A chat application uses WebSockets to send messages. An attacker could send a harmful script embedded in the message payload.
- **Exploit Example**: The attacker sends `{"message": "<script>alert('Hacked!')</script>"}`. This script runs in every user's browser who sees the message, showing an alert box.

## WebSocket Handshake Vulnerabilities

- **Example of Misuse**: Trusting the `X-Forwarded-For` header to verify a user's IP during a WebSocket handshake can be problematic if an attacker can spoof this header to bypass IP-based restrictions.

## Cross-Site WebSocket Hijacking

### What is it?
- **Description**: If a site doesn't adequately secure WebSocket handshakes, an attacker could hijack a user's connection to perform actions on their behalf.
- **Example**: An attacker creates a malicious site that triggers a WebSocket connection to a banking site using the victim's cookies, allowing unauthorized transactions.

### Example of a Cross-Site WebSocket Hijacking Attack
- **Vulnerable Handshake**: A handshake relying solely on cookies for session management is susceptible.
- **Attack Flow**:
  1. Victim logs into `normal-website.com`.
  2. Attacker lures the victim to `malicious-website.com`.
  3. `malicious-website.com` executes a script to open a WebSocket with `normal-website.com` using the victim's cookies.
  4. The attacker can now send and receive messages as the victim.

## How to Secure WebSocket Connections

- **Use TLS**: Change from `ws://` to `wss://` to secure the data in transit.
  - **Why it matters**: Just like https, `wss://` encrypts the data, preventing eavesdroppers from seeing the transmitted information.
- **CSRF Protection**: Add a CSRF token to the WebSocket handshake to ensure that the request is coming from your site, not an attacker's.
  - **Implementation Tip**: Include a CSRF token in the handshake headers or initial payload.

## Last Moment Revision Points
1. WebSockets are for continuous, two-way communication.
2. Always sanitize input/output to prevent XSS and SQL injections.
3. Secure the handshake to prevent CSRF in WebSocket connections.
4. Use `wss://` for encrypted communication to protect data integrity and confidentiality.
5. Monitor WebSocket traffic for unusual patterns that might indicate exploitation.
