The `proxy_pass` directive in Nginx is used to define where Nginx should forward or "proxy" requests that it receives. Essentially, it tells Nginx to pass client requests to another server (often called an "upstream" server), which could be another web server, an application server, or any other service running on a specified URL or IP address.

### How `proxy_pass` Works

When Nginx receives a request that matches a particular location block, the `proxy_pass` directive inside that block specifies the address of the backend server where the request should be forwarded. Nginx acts as an intermediary, forwarding the request to the backend server and then returning the response from the backend server to the client.

### Basic Syntax

```nginx
location /somepath/ {
    proxy_pass http://backend-server:port;
}
```

- **`/somepath/`**: The URL path that, when matched, triggers the proxying behavior.
- **`http://backend-server:port`**: The address of the backend server to which the request should be forwarded.

### Example Usage

Let's consider an example where you have an Nginx server that serves static files and proxies API requests to a backend server running on `localhost` at port `3000`.

```nginx
server {
    listen 80;

    server_name example.com;

    # Serve static files
    location / {
        root /var/www/html;
        try_files $uri $uri/ =404;
    }

    # Proxy API requests
    location /api/ {
        proxy_pass http://127.0.0.1:3000/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Breakdown of the Example

1. **Static Files**:
   - Requests to `http://example.com/` (e.g., `/index.html`) are served directly from the `/var/www/html` directory.

2. **Proxying API Requests**:
   - Requests that start with `/api/` (e.g., `http://example.com/api/users`) are forwarded to `http://127.0.0.1:3000/`.
   - Nginx forwards the request to the backend server (running on `localhost:3000` in this case), and any response from the backend is returned to the client.

### Key Points

- **URL Modifications**: By default, Nginx will forward the part of the URL that comes after the location match directly to the backend server. However, you can adjust how Nginx constructs the URL sent to the backend.
  
- **Proxy Headers**: Typically, `proxy_pass` is used in conjunction with proxy headers like `X-Forwarded-For`, `Host`, `X-Real-IP`, etc., to ensure that the backend server has the necessary context about the original client request.

- **Load Balancing**: When combined with upstream blocks, `proxy_pass` can be used to distribute requests across multiple backend servers for load balancing.

### Advanced Use Cases

- **Passing URIs**: If you want to modify the URI that gets passed to the backend, you can use the `$uri` variable or other Nginx variables to customize the request.

- **SSL/TLS**: `proxy_pass` can be used to forward requests to HTTPS backends, in which case you'll prefix the address with `https://` and may need additional configurations for certificates and SSL options.

### Example of a Custom URI

```nginx
location /api/ {
    proxy_pass http://127.0.0.1:3000/newapi/;
}
```

In this case, a request to `http://example.com/api/users` would be forwarded to `http://127.0.0.1:3000/newapi/users`.

### Summary

- **`proxy_pass`**: Directs Nginx to forward requests to a specified backend server.
- **Commonly used in**: Reverse proxy setups, load balancing, and isolating different services.
- **Example**: Routing API requests to an application server while serving static files directly.
