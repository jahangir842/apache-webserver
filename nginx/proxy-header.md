A **proxy header** refers to the HTTP headers that a proxy server (like Nginx) adds or modifies when forwarding client requests to a backend server. These headers provide additional context or information about the original client request, which the backend server might need for processing the request appropriately.

### Common Proxy Headers

1. **`Host` Header**:
   - **Purpose**: Indicates the original host requested by the client.
   - **Example**:
     ```nginx
     proxy_set_header Host $host;
     ```
   - **Usage**: This tells the backend server the hostname or domain the client intended to access. This is useful when the backend server hosts multiple sites or applications.

2. **`X-Real-IP` Header**:
   - **Purpose**: Passes the original client's IP address.
   - **Example**:
     ```nginx
     proxy_set_header X-Real-IP $remote_addr;
     ```
   - **Usage**: The backend server can use this header to log the client's real IP address, rather than the IP of the proxy server.

3. **`X-Forwarded-For` Header**:
   - **Purpose**: A list of IP addresses that includes the original client IP and any intermediate proxies.
   - **Example**:
     ```nginx
     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
     ```
   - **Usage**: This header is used to trace the original client IP when requests pass through multiple proxies. The backend can use this for logging, security, or other processing purposes.

4. **`X-Forwarded-Proto` Header**:
   - **Purpose**: Indicates the protocol (HTTP or HTTPS) used by the client.
   - **Example**:
     ```nginx
     proxy_set_header X-Forwarded-Proto $scheme;
     ```
   - **Usage**: This is important for backend applications that need to know whether the original request was made over a secure (HTTPS) connection or not.

5. **`X-Forwarded-Host` Header**:
   - **Purpose**: Indicates the original `Host` header value sent by the client.
   - **Example**:
     ```nginx
     proxy_set_header X-Forwarded-Host $host;
     ```
   - **Usage**: This is useful in scenarios where the proxy server changes the `Host` header, but the backend still needs to know the original host requested by the client.

### Why Proxy Headers are Important

- **Client Identity**: Proxy headers help the backend server understand who the original client was, even if requests are coming through a proxy server.
- **Logging and Auditing**: Many systems use the client's original IP address and protocol for logging and auditing purposes.
- **Security**: Knowing the client's original IP can be critical for security measures like rate limiting, IP-based access controls, or analyzing traffic patterns.
- **Application Logic**: Some applications behave differently based on the protocol (HTTP/HTTPS) or domain requested. Proxy headers provide this context to ensure the backend behaves correctly.

### How They Work

When a proxy server like Nginx forwards a request to a backend server, it can add or modify headers to provide the backend with more information about the request. These headers are then available to the backend application, which can use them as needed.

For example, when a client makes a request, the proxy server might add the `X-Forwarded-For` header to the request before forwarding it to the backend server. The backend server can then log this header or use it to determine the client's actual IP address, even though the request technically came from the proxy server's IP.
