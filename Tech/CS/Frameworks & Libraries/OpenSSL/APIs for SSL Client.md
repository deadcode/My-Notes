# SSL flow with Client Auth
A typical OpenSSL API flow for an SSL/TLS **client** that performs **client authentication** (mutual TLS) involves the following steps:

---
**1. Initialize OpenSSL**
Before using OpenSSL, initialize the necessary components:
```
SSL_library_init();           // Initialize OpenSSL
SSL_load_error_strings();     // Load error messages
OpenSSL_add_all_algorithms(); // Load encryption & hash algorithms
```
---
**2. Create an SSL Context**
The SSL context (SSL_CTX) manages SSL/TLS configurations:
```
const SSL_METHOD *method = TLS_client_method(); // Use client-side TLS
SSL_CTX *ctx = SSL_CTX_new(method);
if (!ctx) {
    ERR_print_errors_fp(stderr);
    exit(EXIT_FAILURE);
}
```
---
**3. Load Client Certificate and Private Key**
Since the server requires **client authentication**, the client must present a certificate and private key:
```
if (SSL_CTX_use_certificate_file(ctx, "client.crt", SSL_FILETYPE_PEM) <= 0) {
    ERR_print_errors_fp(stderr);
    exit(EXIT_FAILURE);
}

if (SSL_CTX_use_PrivateKey_file(ctx, "client.key", SSL_FILETYPE_PEM) <= 0) {
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
**4. Load CA Certificates for Server Verification**
To verify the **server’s certificate**, the client must load the **Certificate Authority (CA) certificate**:
```
if (!SSL_CTX_load_verify_locations(ctx, "ca.crt", NULL)) {
    ERR_print_errors_fp(stderr);
    exit(EXIT_FAILURE);
}
```
This ensures the client only trusts servers whose certificates are signed by ca.crt.

---
**5. Create and Connect a TCP Socket**
Create a socket and connect to the server:
```
int sock = socket(AF_INET, SOCK_STREAM, 0);
struct sockaddr_in server_addr;
server_addr.sin_family = AF_INET;
server_addr.sin_port = htons(4433); // TLS port

inet_pton(AF_INET, "192.168.1.100", &server_addr.sin_addr); // Server IP
connect(sock, (struct sockaddr*)&server_addr, sizeof(server_addr));
```
---
**6. Create an SSL Object and Attach the Socket**
Create an SSL object and associate it with the socket:
```
SSL *ssl = SSL_new(ctx);
SSL_set_fd(ssl, sock);
```
---
**7. Perform SSL Handshake**
Start the SSL handshake, during which:
• The client **verifies the server’s certificate**.
• The server **requests and verifies the client’s certificate**.
```
if (SSL_connect(ssl) <= 0) {
    ERR_print_errors_fp(stderr);
    close(sock);
    SSL_free(ssl);
    SSL_CTX_free(ctx);
    exit(EXIT_FAILURE);
}
printf("SSL Handshake successful\n");
```
---
**8. Verify the Server Certificate**
After the handshake, the client should verify the server’s certificate:
```
X509 *server_cert = SSL_get_peer_certificate(ssl);
if (server_cert) {
    printf("Server certificate received\n");
    X509_free(server_cert);
} else {
    printf("No server certificate presented\n");
}

if (SSL_get_verify_result(ssl) != X509_V_OK) {
    printf("Server certificate verification failed\n");
    SSL_shutdown(ssl);
    SSL_free(ssl);
    close(sock);
    SSL_CTX_free(ctx);
    exit(EXIT_FAILURE);
}
printf("Server certificate verified successfully\n");
```
---
**9. Read/Write Secure Data**
Once the handshake is complete, the client can securely communicate with the server:
```
SSL_write(ssl, "Hello, Server!", 14);

char buffer[1024] = {0};
int bytes = SSL_read(ssl, buffer, sizeof(buffer));
if (bytes > 0) {
    printf("Received: %s\n", buffer);
}
```
---
**10. Shutdown and Cleanup**
Close the connection and free resources:
```
SSL_shutdown(ssl);
SSL_free(ssl);
close(sock);
SSL_CTX_free(ctx);
EVP_cleanup();
ERR_free_strings();
```
---
**Summary of OpenSSL API Flow for Client Authentication**
1. **Initialize OpenSSL** (SSL_library_init(), SSL_load_error_strings())
2. **Create SSL context** (SSL_CTX_new())
3. **Load client certificate and private key** (SSL_CTX_use_certificate_file(), SSL_CTX_use_PrivateKey_file())
4. **Load CA certificates for server verification** (SSL_CTX_load_verify_locations())
5. **Create and connect a TCP socket** (socket(), connect())
6. **Create an SSL object and attach the socket** (SSL_new(), SSL_set_fd())
7. **Perform SSL handshake** (SSL_connect())
8. **Verify the server certificate** (SSL_get_peer_certificate(), SSL_get_verify_result())
9. **Read/write secure data** (SSL_read(), SSL_write())
10. **Shutdown and cleanup** (SSL_shutdown(), SSL_free(), SSL_CTX_free())
---
