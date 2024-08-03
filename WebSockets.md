```markdown
# WebSockets Security

## Introduction to WebSockets

WebSockets are a modern technology used in web applications to create long-lasting connections that allow data to be sent and received in real-time. Here are some key points:

- **Initiation:** WebSockets start as an HTTP request and then upgrade to a WebSocket connection.
- **Bidirectional Communication:** They enable two-way communication between the client and server.
- **Use Cases:** Commonly used for live updates, chat applications, and real-time notifications.
- **Security Risks:** They share similar vulnerabilities with HTTP, such as injection attacks and cross-site scripting (XSS).

## Security Vulnerabilities in WebSockets

### User-Supplied Input Vulnerabilities

WebSockets can be vulnerable to unsafe processing of user input, leading to issues like:

- **SQL Injection:** Malicious input can alter SQL queries.
- **XML External Entity (XXE) Injection:** Unsecured XML processing can lead to data exposure.

#### Example: SQL Injection

If an application sends a user search query via WebSockets:

```json
{"search":"john"}
```

And this input is directly included in an SQL query:

```sql
SELECT * FROM users WHERE name = 'john'
```

An attacker could inject SQL commands:

```json
{"search":"john' OR '1'='1"}
```

Resulting in:

```sql
SELECT * FROM users WHERE name = 'john' OR '1'='1'
```

### Blind Vulnerabilities

Some vulnerabilities can only be detected using out-of-band techniques, as they don't directly display results to the attacker.

### Client-Side Vulnerabilities

If attacker-controlled data is transmitted via WebSockets to other users, it might lead to XSS or other client-side issues.

#### Example: XSS in Chat Application

Normal message sent via WebSocket:

```json
{"message":"Hello Carlos"}
```

Displayed in another user's browser as:

```html
<td>Hello Carlos</td>
```

An attacker can send:

```json
{"message":"<img src=1 onerror='alert(1)'>"}
```

This could execute JavaScript in the recipient's browser.

## Manipulating WebSocket Messages

Tampering with WebSocket messages can exploit various vulnerabilities.

#### Example: Chat Application XSS

Attack:

```json
{"message":"<img src=1 onerror='alert(1)'>"}
```

## WebSocket Handshake Manipulation

### Design Flaws

Vulnerabilities can occur during the WebSocket handshake due to:

- **Misplaced Trust in Headers:** Relying on headers like `X-Forwarded-For`.
- **Session Handling Issues:** The session context is determined during the handshake.

#### Example: Misplaced Trust in Headers

An attacker can forge the `X-Forwarded-For` header to bypass IP restrictions:

```http
GET /chat HTTP/1.1
Host: example.com
X-Forwarded-For: 123.456.789.000
```

## Cross-Site WebSocket Hijacking

### Understanding the Attack

This attack involves exploiting a CSRF vulnerability during the WebSocket handshake. The attacker’s page opens a WebSocket connection to the vulnerable site.

#### Example: Vulnerable WebSocket Handshake

```http
GET /chat HTTP/1.1
Host: example.com
Sec-WebSocket-Version: 13
Sec-WebSocket-Key: wDqumtseNBJdhkihL6PW7w==
Connection: keep-alive, Upgrade
Cookie: session=KOsEJNuflw4Rd9BDNrVmvwBF9rEijeE2
Upgrade: websocket
```

### Impact of Hijacking

- **Unauthorized Actions:** Attackers can perform actions on behalf of the victim.
- **Sensitive Data Access:** Attackers can intercept sensitive data.

#### Example Attack Script

```javascript
var socket = new WebSocket("wss://example.com/chat");
socket.onmessage = function(event) {
  console.log("Received data: " + event.data);
};
socket.send("Perform unauthorized action");
```

## Securing WebSocket Connections

- **Use TLS:** Employ `wss://` for encrypted connections.
- **Hardcode Endpoint URL:** Avoid user-controllable URLs.
- **Protect Handshake:** Implement CSRF protection on the handshake.
- **Treat Data as Untrusted:** Sanitize and validate data both ways.

## Key Points for Quick Revision

1. **WebSockets Basics:**
   - Initiate over HTTP, provide bidirectional communication.
   - Used for real-time updates and chat.

2. **Common Vulnerabilities:**
   - SQL injection, XXE, and XSS.
   - Blind vulnerabilities detected via OAST.

3. **Exploiting WebSocket Messages:**
   - Tamper with messages to exploit vulnerabilities.
   - Example: XSS in chat applications.

4. **Handshake Manipulation:**
   - Design flaws like misplaced trust in headers.
   - Session handling issues.

5. **Cross-Site WebSocket Hijacking:**
   - CSRF vulnerability on the handshake.
   - Enables unauthorized actions and data interception.

6. **Security Measures:**
   - Use `wss://` protocol.
   - Hardcode WebSocket URL.
   - Implement CSRF protection.
   - Treat all WebSocket data as untrusted.

By following these guidelines and examples, you can better understand and secure WebSocket implementations.
```
