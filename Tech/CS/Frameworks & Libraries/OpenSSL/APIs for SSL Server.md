# API flow SSL
A typical OpenSSL API flow for an SSL/TLS server involves several steps, including initialization, certificate loading, connection handling, and cleanup. Below is the general flow:

---
**1. Initialize OpenSSL**
Before using OpenSSL, you must initialize the library:
```
SSL_library_init();           // Initialize OpenSSL
SSL_load_error_strings();     // Load error messages
OpenSSL_add_all_algorithms(); // Load encryption & hash algorithms
```
---
**2. Create an SSL Context**
Create an SSL context that defines SSL/TLS configurations:
```
const SSL_METHOD *method = TLS_server_method(); // Use TLS method
SSL_CTX *ctx = SSL_CTX_new(method);
if (!ctx) {
    ERR_print_errors_fp(stderr);
    exit(EXIT_FAILURE);
}
```
---
**3. Load Certificate and Private Key**
To establish a secure connection, the server must provide a certificate and private key:
```
if (SSL_CTX_use_certificate_file(ctx, "server.crt", SSL_FILETYPE_PEM) <= 0) {
    ERR_print_errors_fp(stderr);
    exit(EXIT_FAILURE);
}

if (SSL_CTX_use_PrivateKey_file(ctx, "server.key", SSL_FILETYPE_PEM) <= 0) {
    ERR_print_errors_fp(stderr);
    exit(EXIT_FAILURE);
}

if (!SSL_CTX_check_private_key(ctx)) {
    fprintf(stderr, "Private key does not match the certificate\n");
    exit(EXIT_FAILURE);
}
```
---
**4. Create and Bind a TCP Socket**
Create a standard TCP socket, bind it to a port, and listen for incoming connections:
```
int server_fd = socket(AF_INET, SOCK_STREAM, 0);
struct sockaddr_in addr;
addr.sin_family = AF_INET;
addr.sin_addr.s_addr = INADDR_ANY;
addr.sin_port = htons(4433); // Port 4433 for TLS

bind(server_fd, (struct sockaddr*)&addr, sizeof(addr));
listen(server_fd, 5);
```
---
**5. Accept Incoming Connection**
When a client connects, accept the connection and create an SSL object:
```
struct sockaddr_in client_addr;
socklen_t client_len = sizeof(client_addr);
int client_fd = accept(server_fd, (struct sockaddr*)&client_addr, &client_len);

SSL *ssl = SSL_new(ctx);
SSL_set_fd(ssl, client_fd);
```
---
**6. Perform SSL Handshake**
Once the connection is accepted, perform the SSL handshake:
```
if (SSL_accept(ssl) <= 0) {
    ERR_print_errors_fp(stderr);
} else {
    printf("SSL Handshake successful\n");
}
```
---
**7. Read/Write Data Over SSL**
After a successful handshake, the server can read and write encrypted data:
```
char buffer[1024] = {0};
int bytes = SSL_read(ssl, buffer, sizeof(buffer));
if (bytes > 0) {
    printf("Received: %s\n", buffer);
    SSL_write(ssl, "Hello, Client!", 14);
}
```
---
**8. Shutdown and Cleanup**
Close the connection, clean up SSL, and free allocated resources:
```
SSL_shutdown(ssl);
SSL_free(ssl);
close(client_fd);
```
After all connections are closed, clean up OpenSSL resources:
```
SSL_CTX_free(ctx);
EVP_cleanup();
ERR_free_strings();
```
---
**Summary of OpenSSL API Flow**
1. Initialize OpenSSL.
2. Create an SSL context (SSL_CTX).
3. Load certificates and private keys.
4. Create and bind a TCP socket.
5. Accept incoming client connections.
6. Create an SSL object (SSL).
7. Perform the SSL handshake.
8. Read/write encrypted data.
9. Shutdown and cleanup.

# API flow for SSL server with Client Auth
A typical OpenSSL API flow for an SSL server that performs **client authentication** follows these steps:

---
**1. Initialize OpenSSL**
Before using OpenSSL, initialize the necessary libraries:
```
SSL_library_init();           // Initialize OpenSSL
SSL_load_error_strings();     // Load error messages
OpenSSL_add_all_algorithms(); // Load cryptographic algorithms
```
---
**2. Create an SSL Context**
The SSL context (SSL_CTX) manages configurations for the SSL server:
```
const SSL_METHOD *method = TLS_server_method(); // Use TLS method
SSL_CTX *ctx = SSL_CTX_new(method);
if (!ctx) {
    ERR_print_errors_fp(stderr);
    exit(EXIT_FAILURE);
}
```
---
**3. Load Server Certificate and Private Key**
The server needs a certificate and private key:
```
if (SSL_CTX_use_certificate_file(ctx, "server.crt", SSL_FILETYPE_PEM) <= 0) {
    ERR_print_errors_fp(stderr);
    exit(EXIT_FAILURE);
}

if (SSL_CTX_use_PrivateKey_file(ctx, "server.key", SSL_FILETYPE_PEM) <= 0) {
    ERR_print_errors_fp(stderr);
    exit(EXIT_FAILURE);
}

// Ensure private key matches the certificate
if (!SSL_CTX_check_private_key(ctx)) {
    fprintf(stderr, "Private key does not match the certificate\n");
    exit(EXIT_FAILURE);
}
```
---
**4. Configure Client Authentication**
To require the client to present a certificate, configure client authentication:
```
SSL_CTX_set_verify(ctx, SSL_VERIFY_PEER | SSL_VERIFY_FAIL_IF_NO_PEER_CERT, NULL);
```
• SSL_VERIFY_PEER: Requires the client to present a certificate.
• SSL_VERIFY_FAIL_IF_NO_PEER_CERT: Rejects the connection if no client certificate is provided.

**Set CA Certificate for Client Authentication**
The server must load the Certificate Authority (CA) certificate that issued the client certificates:
```
if (!SSL_CTX_load_verify_locations(ctx, "ca.crt", NULL)) {
    ERR_print_errors_fp(stderr);
    exit(EXIT_FAILURE);
}
```
This ensures the server verifies client certificates using ca.crt.

---
**5. Create and Bind a TCP Socket**
Set up a standard TCP socket, bind it to a port, and listen for connections:
```
int server_fd = socket(AF_INET, SOCK_STREAM, 0);
struct sockaddr_in addr;
addr.sin_family = AF_INET;
addr.sin_addr.s_addr = INADDR_ANY;
addr.sin_port = htons(4433); // Port 4433 for TLS

bind(server_fd, (struct sockaddr*)&addr, sizeof(addr));
listen(server_fd, 5);
```
---
**6. Accept Client Connection**
Accept an incoming client connection:
```
struct sockaddr_in client_addr;
socklen_t client_len = sizeof(client_addr);
int client_fd = accept(server_fd, (struct sockaddr*)&client_addr, &client_len);
```
---
**7. Create SSL Object and Perform Handshake**
1. **Create a new SSL object** for the client connection.
2. **Associate the socket** with the SSL object.
3. **Perform the SSL handshake**.
```
SSL *ssl = SSL_new(ctx);
SSL_set_fd(ssl, client_fd);

if (SSL_accept(ssl) <= 0) {
    ERR_print_errors_fp(stderr);
    close(client_fd);
    SSL_free(ssl);
    return;
}
printf("SSL Handshake successful\n");
```
---
**8. Verify Client Certificate**
After a successful handshake, verify the client certificate:
```
X509 *client_cert = SSL_get_peer_certificate(ssl);
if (client_cert) {
    printf("Client certificate presented\n");
    X509_free(client_cert);
} else {
    printf("No client certificate presented\n");
}

if (SSL_get_verify_result(ssl) != X509_V_OK) {
    printf("Client certificate verification failed\n");
    SSL_shutdown(ssl);
    SSL_free(ssl);
    close(client_fd);
    return;
}
printf("Client certificate verified successfully\n");
```
---
**9. Read/Write Secure Data**
Now the server can securely exchange data with the client:
```
char buffer[1024] = {0};
int bytes = SSL_read(ssl, buffer, sizeof(buffer));
if (bytes > 0) {
    printf("Received: %s\n", buffer);
    SSL_write(ssl, "Hello, Client!", 14);
}
```
---
**10. Shutdown and Cleanup**
1. **Terminate the SSL connection:**
```
SSL_shutdown(ssl);
SSL_free(ssl);
close(client_fd);
```
2. **Cleanup OpenSSL and release memory:**
```
SSL_CTX_free(ctx);
EVP_cleanup();
ERR_free_strings();
```
---
**Summary of OpenSSL API Flow for Client Authentication**
1. **Initialize OpenSSL** (SSL_library_init(), SSL_load_error_strings())
2. **Create SSL context** (SSL_CTX_new())
3. **Load server certificate and private key** (SSL_CTX_use_certificate_file(), SSL_CTX_use_PrivateKey_file())
4. **Enable client authentication** (SSL_CTX_set_verify(), SSL_CTX_load_verify_locations())
5. **Create and bind TCP socket** (socket(), bind(), listen())
6. **Accept incoming client connections** (accept())
7. **Perform SSL handshake** (SSL_accept())
8. **Verify client certificate** (SSL_get_peer_certificate(), SSL_get_verify_result())
9. **Securely read/write data** (SSL_read(), SSL_write())
10. **Shutdown and cleanup** (SSL_shutdown(), SSL_free(), SSL_CTX_free())
---