## URL Rewriting in Apache

**URL rewriting** is a technique used to manipulate URLs on the fly before they reach the web server or when they are returned to the client. In Apache HTTP Server, this is accomplished using the `mod_rewrite` module. Here’s a comprehensive guide on URL rewriting in Apache:

---

### **1. Introduction to `mod_rewrite`**

- **Purpose**: `mod_rewrite` provides a powerful and flexible way to perform URL manipulations and redirections. It allows you to rewrite URLs based on various conditions, enabling cleaner URLs, redirection, and access control.
- **Activation**: Ensure that `mod_rewrite` is enabled in Apache. You can enable it using the command:
  ```bash
  sudo a2enmod rewrite
  ```
  For CentOS/RHEL-based systems, `mod_rewrite` is usually enabled by default.

---

### **2. Configuration Directives**

- **`RewriteEngine`**: Enables or disables the rewriting engine.
  ```apache
  RewriteEngine on
  ```
  Without this directive set to `on`, no rewrite rules will be processed.

- **`RewriteCond`**: Defines conditions under which the rewrite rules will be applied. Conditions are evaluated before the `RewriteRule`.
  ```apache
  RewriteCond %{HTTP_HOST} ^example\.com$ [NC]
  ```
  - **`%{HTTP_HOST}`**: Represents the HTTP host header.
  - **`^example\.com$`**: Regular expression to match the host header.
  - **`[NC]`**: Flags indicating that the match is case-insensitive.

- **`RewriteRule`**: Defines the actual URL rewriting rule. It has the following syntax:
  ```apache
  RewriteRule pattern substitution [flags]
  ```
  - **`pattern`**: The regular expression pattern to match against the URL path.
  - **`substitution`**: The new URL or path to which the request will be redirected or rewritten.
  - **`[flags]`**: Optional flags that modify the behavior of the rule.

---

### **3. Common Rewrite Rules and Use Cases**

Apache's mod_rewrite module is a powerful tool for URL manipulation. Here are some common rewrite rules, along with explanations of how they work and when to use them:

#### ** Redirect HTTP to HTTPS**
This rule ensures that all traffic is securely redirected to HTTPS, which is essential for data security and user trust.

```apache
RewriteEngine on
RewriteCond %{SERVER_PORT} 80
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

- **`RewriteEngine on`**: Activates the mod_rewrite engine.
- **`RewriteCond %{SERVER_PORT} 80`**: This condition checks if the server is receiving traffic on port 80 (the default port for HTTP). If true, the rule will apply.
- **`RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]`**:
  - **`^(.*)$`**: Matches the entire URL path.
  - **`https://%{HTTP_HOST}%{REQUEST_URI}`**: Redirects the user to the same path and host, but with the HTTPS protocol.
  - **`[L,R=301]`**: 
    - **`L`**: Stops processing other rules if this rule matches.
    - **`R=301`**: Issues a 301 Permanent Redirect, signaling to search engines that the content has permanently moved to the HTTPS version.

#### ** Remove .php Extension**
This rule rewrites URLs to hide the `.php` file extension, making URLs cleaner and more user-friendly.

```apache
RewriteEngine on
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^([^/]+)/?$ $1.php [L]
```

- **`RewriteEngine on`**: Enables the rewrite engine.
- **`RewriteCond %{REQUEST_FILENAME} !-f`**: Ensures the request is not for a direct file (like an image or script).
- **`RewriteCond %{REQUEST_FILENAME} !-d`**: Ensures the request is not for a directory.
- **`RewriteRule ^([^/]+)/?$ $1.php [L]`**:
  - **`^([^/]+)/?$`**: Captures the requested path without a trailing slash and excludes the `.php` extension.
  - **`$1.php`**: Internally rewrites the URL to point to the `.php` file.
  - **`[L]`**: Stops processing any further rules if this rule matches.


#### ** Redirect to a Specific Domain**
This rule redirects requests from one domain to another, useful when changing domain names or consolidating traffic.

```apache
RewriteEngine on
RewriteCond %{HTTP_HOST} ^oldexample\.com$ [NC]
RewriteRule ^(.*)$ http://newexample.com/$1 [L,R=301]
```

- **`RewriteEngine on`**: Turns on the rewrite engine.
- **`RewriteCond %{HTTP_HOST} ^oldexample\.com$ [NC]`**: 
  - Matches the `oldexample.com` domain.
  - **`[NC]`**: Makes the match case-insensitive.
- **`RewriteRule ^(.*)$ http://newexample.com/$1 [L,R=301]`**:
  - **`^(.*)$`**: Captures the entire requested URL path.
  - **`http://newexample.com/$1`**: Redirects to the new domain, appending the original path.
  - **`[L,R=301]`**: 
    - **`L`**: Stops further rules from processing.
    - **`R=301`**: Issues a 301 Permanent Redirect.

#### ** SEO-Friendly URLs**
This rule converts query strings into clean, readable URLs, which is beneficial for search engine optimization (SEO).

```apache
RewriteEngine on
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^products/([0-9]+)/([a-zA-Z0-9_-]+)$ product.php?id=$1&name=$2 [L,QSA]
```

- **`RewriteEngine on`**: Enables the rewrite functionality.
- **`RewriteCond %{REQUEST_FILENAME} !-f`**: Ensures the request isn't for an existing file.
- **`RewriteCond %{REQUEST_FILENAME} !-d`**: Ensures the request isn't for an existing directory.
- **`RewriteRule ^products/([0-9]+)/([a-zA-Z0-9_-]+)$ product.php?id=$1&name=$2 [L,QSA]`**:
  - **`^products/([0-9]+)/([a-zA-Z0-9_-]+)$`**: Matches URLs like `/products/123/product-name`, capturing the numeric ID and the product name.
  - **`product.php?id=$1&name=$2`**: Internally rewrites the URL to the `product.php` script with `id` and `name` parameters.
  - **`[L,QSA]`**:
    - **`L`**: Stops further rules from applying.
    - **`QSA`**: Appends any additional query string parameters to the rewritten URL.

---

### **4. Rewrite Flags**

- **`[L]`**: Last rule. Stops processing further rules if this one matches.
- **`[R=301]`**: Redirect with a 301 status code (permanent redirect).
- **`[QSA]`**: Query string append. Appends the original query string to the new URL.
- **`[NE]`**: No escape. Prevents special characters from being escaped in the substitution URL.
- **`[NC]`**: No case. Makes the match case-insensitive.

---

### **5. Example of RewriteRule**

```apache
RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
```

#### **Breaking Down the RewriteRule:**

- **`^`**: 
  - This is a regular expression that matches the beginning of the requested URL path. Since it's empty (`^`), it matches any URL path, effectively meaning "all requests."

- **`https://%{SERVER_NAME}%{REQUEST_URI}`**:
  - **`https://`**: This explicitly sets the protocol to HTTPS.
  - **`%{SERVER_NAME}`**: This variable represents the server name (e.g., `jahangir.blog` or `www.jahangir.blog`). It's dynamically replaced with the domain name that was used in the request.
  - **`%{REQUEST_URI}`**: This variable represents the full requested URI, including the path and query string (e.g., `/path/to/resource?query=param`).

  The combination of `https://%{SERVER_NAME}%{REQUEST_URI}` ensures that the original requested URL is preserved but accessed over HTTPS instead of HTTP.

- **`[END,NE,R=permanent]`**:
  - **`END`**: Stops any further rewriting after this rule is applied. It prevents other rewrite rules from being processed after this one.
  - **`NE`**: Stands for "No Escape." It prevents special characters in the URL from being escaped. For example, `%20` would not be turned into a space.
  - **`R=permanent`**: This triggers a 301 permanent redirect, telling browsers and search engines that the resource has been permanently moved to the new HTTPS URL. This is important for SEO and ensures that future requests will use HTTPS.

#### **In Summary:**

The rule `RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]` forces all incoming HTTP requests to be redirected to the equivalent HTTPS URL, ensuring secure communication and a consistent URL structure. This is particularly useful for enforcing HTTPS across your entire site.

---

### **6. Testing and Debugging**

- **LogLevel**: Increase the logging level to debug rewrite issues.
  ```apache
  LogLevel alert rewrite:trace6
  ```
  Check the Apache error log to see detailed information about rewrite processing.

- **Apache Configuration Test**: Always test configuration changes.
  ```bash
  sudo apachectl configtest
  ```

---

### **7. Common Issues and Troubleshooting**

- **Rules Not Applied**: Ensure that `mod_rewrite` is enabled and that the `RewriteEngine` is set to `on`.
- **Infinite Loops**: Be cautious with redirection rules to avoid infinite redirect loops.
- **File and Directory Conditions**: Use conditions like `RewriteCond %{REQUEST_FILENAME} !-f` to prevent rewriting when the file or directory exists.

---

### **8. Advanced Use Cases**

- **Conditional Redirects**: Redirect based on geographic location or other headers.
- **Complex URL Transformations**: Use advanced regular expressions for more complex URL patterns.

---

This guide covers the basics and some advanced aspects of URL rewriting with `mod_rewrite` in Apache. For more details, consult the [Apache Documentation on mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html).
