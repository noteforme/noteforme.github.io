---
id: 67
title: https_ca
date: '2024-05-15T07:08:40+00:00'
author: Jon
layout: post
categories:
    - NETWORK
---

# TLS

[SSL Server Test (Powered by Qualys SSL Labs)](https://www.ssllabs.com/ssltest/)

https://gist.github.com/Karewan/4b0270755e7053b471fdca4419467216

# Check CA

```
openssl x509 -in ca.crt -text -noout
```

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/http/202503291813422.png)

https://www.youtube.com/watch?v=JA0vaIb4158&t=876s

https://www.youtube.com/watch?v=AlE5X1NlHgg

![](https://raw.githubusercontent.com/BlogForMe/ImageServer/main/http/202503291845440.png)

https://www.youtube.com/watch?v=j9QmMEWmcfo

## Post

https://jsonplaceholder.typicode.com/posts

request

```
:method: POST
:path: /posts
:authority: jsonplaceholder.typicode.com
:scheme: httpscontent-type: application/x-www-form-urlencodedcontent-length: 42
accept-encoding: gzipuser-agent: okhttp/5.0.0-SNAPSHOT
```

response

```
:status: 201
date: Wed, 17 Apr 2024 06:45:30 GMTcontent-type: application/json; charset=utf-8
content-length: 82
location: https://jsonplaceholder.typicode.com/posts/101
report-to: {"group":"heroku-nel","max_age":3600,"endpoints":[{"url":"https://nel.heroku.com/reports?ts=1713336330&sid=e11707d5-02a7-43ef-b45e-2cf4d2036f7d&s=Hua%2F7slOIpUhGTDPNaKu4xbP%2Bjuwplp9ehOh7C014ok%3D"}]}
reporting-endpoints: heroku-nel=https://nel.heroku.com/reports?ts=1713336330&sid=e11707d5-02a7-43ef-b45e-2cf4d2036f7d&s=Hua%2F7slOIpUhGTDPNaKu4xbP%2Bjuwplp9ehOh7C014ok%3D
nel: {"report_to":"heroku-nel","max_age":3600,"success_fraction":0.005,"failure_fraction":0.05,"response_headers":["Via"]}
x-powered-by: Express
x-ratelimit-limit: 1000
x-ratelimit-remaining: 999
x-ratelimit-reset: 1713336348
vary: Origin, X-HTTP-Method-Override, Accept-Encoding
access-control-allow-credentials: true
cache-control: no-cachepragma: no-cacheexpires: -1
access-control-expose-headers: Location
x-content-type-options: nosniffetag: W/"52-HKy3Gu5DcI5r2jj3f8TQwnYvlDs"
via: 1.1 vegurcf-cache-status: DYNAMICserver: cloudflarecf-ray: 875a73ddee780484-HKGalt-svc: h3=":443"; ma=86400
```

## GET

https://api.github.com/repos/square/okhttp/contributors

Request

```
:method: GET
:path: /repos/square/okhttp/contributors
:authority: api.github.com
:scheme: httpsaccept-encoding: gzipuser-agent: okhttp/5.0.0-SNAPSHOT
```

Response

```
:status: 200
server: GitHub.comdate: Wed, 17 Apr 2024 06:58:41 GMTcontent-type: application/json; charset=utf-8
cache-control: public, max-age=60, s-maxage=60
vary: Accept, Accept-Encoding, Accept, X-Requested-With
etag: W/"699b092aa0b0980f1b2f2bde5e810a75c5c1921ee2f53c4d9f28fc8ea5fca327"
last-modified: Wed, 17 Apr 2024 02:18:24 GMTx-github-media-type: github.v3; format=jsonlink: <https://api.github.com/repositories/5152285/contributors?page=2>; rel="next", <https://api.github.com/repositories/5152285/contributors?page=9>; rel="last"
x-github-api-version-selected: 2022-11-28
access-control-expose-headers: ETag, Link, Location, Retry-After, X-GitHub-OTP, X-RateLimit-Limit, X-RateLimit-Remaining, X-RateLimit-Used, X-RateLimit-Resource, X-RateLimit-Reset, X-OAuth-Scopes, X-Accepted-OAuth-Scopes, X-Poll-Interval, X-GitHub-Media-Type, X-GitHub-SSO, X-GitHub-Request-Id, Deprecation, Sunset
access-control-allow-origin: *
strict-transport-security: max-age=31536000; includeSubdomains; preloadx-frame-options: denyx-content-type-options: nosniffx-xss-protection: 0
referrer-policy: origin-when-cross-origin, strict-origin-when-cross-origincontent-security-policy: default-src 'none'
content-encoding: gzipx-ratelimit-limit: 60
x-ratelimit-remaining: 55
x-ratelimit-reset: 1713337679
x-ratelimit-resource: corex-ratelimit-used: 5
accept-ranges: bytesx-github-request-id: 200C:21C6BE:267A047:2720F58:661F7320
```

### Key Points

- **Deprecation of TLS 1.0 and 1.1:**  
  Android 10 disables TLS 1.0 and TLS 1.1 by default due to known vulnerabilities in these older protocols.
- **Enhanced Security:**  
  By requiring TLS 1.2 or newer, Android 10 and later versions improve the security of network communications.
- **Developer Implications:**  
  Apps that rely on legacy servers supporting only TLS 1.0 or TLS 1.1 may encounter connection issues on devices running Android 10 or higher. Developers are encouraged to update their server configurations and client code to support TLS 1.2 or TLS 1.3.
- **Industry Alignment:**  
  This move is in line with broader industry trends, where many platforms and browsers have also phased out support for TLS 1.0 and 1.1.

If you're maintaining an app or server infrastructure, it's important to verify that your connections support TLS 1.2 or higher to ensure compatibility with Android 10+ devices.

# Self‑Signed CA

## Step 1. Create a Self‑Signed CA with SANs

#### 1.1 Create an OpenSSL Configuration File for the CA (ca.cnf)

Create a file named **ca.cnf** with the following content. This file tells OpenSSL to include SANs in the CA certificate and sets the proper CA extensions.

ini

Copy

```
[ req ]
default_bits       = 4096
prompt             = no
default_md         = sha256
distinguished_name = dn
x509_extensions    = v3_ca

[ dn ]
C  = US
ST = State
L  = City
O  = MyOrg
OU = MyUnit
CN = MyLocalCA

[ v3_ca ]
# Add Subject Alternative Names to the CA certificate
subjectAltName = @alt_names
# Indicate this certificate is a CA
basicConstraints = critical, CA:TRUE, pathlen:0
keyUsage = critical, digitalSignature, cRLSign, keyCertSign
subjectKeyIdentifier = hash

[ alt_names ]
DNS.1 = myca.realmjon.mynetgear.com
DNS.2 = ca.realmjon.mynetgear.com
```

#### 1.2 Generate the CA Key and Certificate

Run these commands in your terminal:

bash

Copy

```
# Generate the CA private key
openssl genrsa -out ca.key 4096

# Generate the self‑signed CA certificate (valid for 10 years)
openssl req -x509 -new -nodes -key ca.key -sha256 -days 3650 \
  -out ca.crt -config ca.cnf
```

After this step, **ca.crt** is your CA certificate (including the SANs defined in **ca.cnf**).

---

## Step 2. Create a Server Certificate for "realmjon.mynetgear.com" with SANs

### 2.1 Create an OpenSSL Configuration File for the Server (server.cnf)

Create a file named **server.cnf** with the following content:

ini

Copy

```
[ req ]
default_bits       = 2048
prompt             = no
default_md         = sha256
distinguished_name = dn
req_extensions     = req_ext

[ dn ]
C  = US
ST = State
L  = City
O  = MyOrg
OU = MyUnit
CN = realmjon.mynetgear.com

[ req_ext ]
# Add Subject Alternative Names for the server certificate
subjectAltName = @alt_names

[ alt_names ]
DNS.1 = realmjon.mynetgear.com
DNS.2 = www.realmjon.mynetgear.com
DNS.3 = api.realmjon.mynetgear.com
```

Adjust or add additional DNS (or IP) entries as needed.

### 2.2 Generate the Server Key and CSR

Run the following commands:

bash

Copy

```
# Generate the server private key
openssl genrsa -out server.key 2048

# Create a Certificate Signing Request (CSR) using the config file
openssl req -new -key server.key -out server.csr -config server.cnf
```

### 2.3 Sign the Server CSR with Your CA

Sign the CSR with your CA certificate and key to generate the server certificate. This process will embed the SANs into the certificate.

bash

Copy

```
openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial \
  -out server.crt -days 3650 -sha256 -extfile server.cnf -extensions req_ext
```

Now, **server.crt** is your server certificate for **realmjon.mynetgear.com** with the specified SANs.

---

## Step 3. Package the Server Certificate and Key into a PKCS#12 Keystore

Spring Boot can load your certificate and key from a PKCS#12 keystore. Create it with:

bash

Copy

```
openssl pkcs12 -export -in server.crt -inkey server.key \
  -out server.p12 -name springboot -CAfile ca.crt -caname root
```

When prompted, enter an export password (which you’ll later use in your Spring Boot configuration).

---

## Step 4. Configure Spring Boot to Use the Keystore

Place **server.p12** in your Spring Boot project’s **src/main/resources** directory, and update your **application.properties** (or YAML) file with:

properties

Copy

```
server.port=8443
server.ssl.enabled=true
server.ssl.key-store=classpath:server.p12
server.ssl.key-store-password=your_keystore_password
server.ssl.key-store-type=PKCS12
server.ssl.key-alias=springboot
```

Replace **your_keystore_password** with the password you set during the PKCS#12 export.

---

## Step 5. Verify the Certificates with OpenSSL

### 5.1 Verify CA Certificate SANs

Run this command to print the details of your CA certificate and check its SANs:

bash

Copy

```
openssl x509 -in ca.crt -text -noout | grep -A1 "Subject Alternative Name"
```

You should see something like:

ruby

Copy

            `X509v3 Subject Alternative Name:                 DNS:myca.realmjon.mynetgear.com, DNS:ca.realmjon.mynetgear.com`

### 5.2 Verify Server Certificate SANs

Similarly, check your server certificate:

```
openssl x509 -in server.crt -text -noout | grep -A1 "Subject Alternative Name"
```

Expected output:

            `X509v3 Subject Alternative Name:                 DNS:realmjon.mynetgear.com, DNS:www.realmjon.mynetgear.com, DNS:api.realmjon.mynetgear.com`

### Step6. OkHttp Demo Code

Below is an example Java class demonstrating how to load the CA from assets, configure the SSL context for TLSv1.3, and make an asynchronous HTTPS request using OkHttp. Make sure to execute network calls off the main thread (this example uses a separate thread):

```
import android.content.Context;
import android.util.Log;
import java.io.InputStream;
import java.security.KeyStore;
import java.security.cert.Certificate;
import java.security.cert.CertificateFactory;
import javax.net.ssl.SSLContext;
import javax.net.ssl.TrustManager;
import javax.net.ssl.TrustManagerFactory;
import javax.net.ssl.X509TrustManager;
import okhttp3.Call;
import okhttp3.Callback;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;
import javax.net.ssl.HostnameVerifier;
import javax.net.ssl.SSLSession;
import java.io.IOException;

public class OkHttpSelfSignedDemo {

    public static void makeRequest(final Context context) {
        new Thread(() -> {
            try {
                // Load the CA certificate from the assets folder
                CertificateFactory cf = CertificateFactory.getInstance("X.509");
                InputStream caInput = context.getAssets().open("ca.crt");
                Certificate ca = cf.generateCertificate(caInput);
                caInput.close();

                // Create a KeyStore containing the trusted CA
                KeyStore keyStore = KeyStore.getInstance(KeyStore.getDefaultType());
                keyStore.load(null, null);
                keyStore.setCertificateEntry("ca", ca);

                // Create a TrustManager that trusts the CA in our KeyStore
                TrustManagerFactory tmf = TrustManagerFactory.getInstance(
                        TrustManagerFactory.getDefaultAlgorithm());
                tmf.init(keyStore);
                TrustManager[] trustManagers = tmf.getTrustManagers();
                X509TrustManager trustManager = null;
                for (TrustManager tm : trustManagers) {
                    if (tm instanceof X509TrustManager) {
                        trustManager = (X509TrustManager) tm;
                        break;
                    }
                }
                if (trustManager == null) {
                    throw new IllegalStateException("No X509TrustManager found");
                }

                // Create an SSLContext for TLSv1.3
                SSLContext sslContext = SSLContext.getInstance("TLSv1.3");
                sslContext.init(null, new TrustManager[]{trustManager}, null);

                // Build the OkHttpClient with the custom SSL settings
                OkHttpClient client = new OkHttpClient.Builder()
                        .sslSocketFactory(sslContext.getSocketFactory(), trustManager)
                        .hostnameVerifier(new HostnameVerifier() {
                            @Override
                            public boolean verify(String hostname, SSLSession session) {
                                // Accept the specific IP address (replace with your actual server IP)
                                return hostname.equals("192.168.1.100");
                            }
                        })
                        .build();

                // Build and execute the HTTPS request (adjust URL, port, and path as needed)
                Request request = new Request.Builder()
                        .url("https://192.168.1.100:8443/")
                        .build();

                client.newCall(request).enqueue(new Callback() {
                    @Override
                    public void onFailure(Call call, IOException e) {
                        Log.e("OkHttpDemo", "Request failed", e);
                    }

                    @Override
                    public void onResponse(Call call, Response response) throws IOException {
                        if (!response.isSuccessful()) {
                            throw new IOException("Unexpected code " + response);
                        }
                        String responseBody = response.body().string();
                        Log.d("OkHttpDemo", "Response: " + responseBody);
                    }
                });
            } catch (Exception e) {
                Log.e("OkHttpDemo", "Error setting up HTTPS connection", e);
            }
        }).start();
    }
}
```

HTTPS

[Springboot配置openssl生成的证书 - lovelylily - 博客园](https://www.cnblogs.com/wolf-python-lily/p/17945001)

[Springboot 证书配置实战Springboot证书配置实战 创建证书 生成CA证书 生成CA证书 将CA证书加到 - 掘金](https://juejin.cn/post/7119487774461263902)

generate host "realmjon.mynetgear.com" self-sign CA make both server.crt ca.crt have subjectalternativenames , config in the springboot

###### ChatGPT said:

Reasoned for 17 seconds

Below is a complete guide that shows you how to create a self‑signed CA and use it to sign a server certificate for the host **realmjon.mynetgear.com**, ensuring that both the CA certificate and the server certificate include Subject Alternative Names (SANs). Finally, you’ll see how to package the server certificate into a PKCS#12 keystore for Spring Boot configuration.

## Summary

1. **Local CA Creation:**
   
   - Created a self‑signed CA certificate (**ca.crt**) that includes SANs (via **ca.cnf**).

2. **Server Certificate:**
   
   - Generated a CSR and signed it with the CA to produce **server.crt** for **realmjon.mynetgear.com** with SANs (via **server.cnf**).

3. **PKCS#12 Keystore:**
   
   - Combined the server certificate and key (with the CA certificate) into **server.p12**.

4. **Spring Boot Configuration:**
   
   - Configured Spring Boot to use **server.p12** for HTTPS.

5. **Verification:**
   
   - Used OpenSSL commands to verify that both certificates contain the correct SAN entries.

Following these steps ensures that both your CA and server certificates include the desired Subject Alternative Names and that your Spring Boot application is correctly configured for HTTPS.
