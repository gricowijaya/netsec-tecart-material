
# Web Security: JWT Injection with Burp Suite

---
## What is the Web?

Sebuah _Web Application_ merupakan perangkat lunak yang dibuat menggunakan teknologi seperti HTML, JavaScript, dan CSS. Mereka biasanya diakses melalui browser web dan dapat digunakan untuk melakukan berbagai tugas, mulai dari belanja dan perbankan hingga streaming media dan mengelola keuangan.

---
### Web Architecture? Wait There’s an Web Architect?

Arsitektur dari sebuah web merupakan salah satu kerangka penting agar sebuah _web application_ dapat dikatakan menjadi sebuah aplikasi web berikut merupakan beberapa komponen arsitekturnya:

![Web Architecture](https://drive.google.com/uc?id=12wcK4QqzHftG8r4ufcQ0V4lF69q5vBf0)
---

1. **Presentation Layer**, yang merupakan sebuah user interface, _presentation layer_ digunakan dalam memberikan layanan yang berinteraksi dengan pengguna. Pada _layer_ ini terdapat sebuah _client_ dapat berupa _web browser_.
2. **Application Layer**, merupakan sebuah logika dari interaksi yang digunakan pada bisnis sebuah aplikasi untuk menjalankan fungsi-fungsi tertentu dalam memproses permintaan _client_ Pada _layer_ ini terdapat sebuah beberapa _server_ untuk menjalani proses komputasi.
3. **Data Layer**, pada bagian ini terdapat sebuah data yang disimpan dalam sebuah _persistent disk_ pada sebuah server. Pada _layer_ ini biasanya berupa sebuah _database_ yang digunakan untuk penyimpanan data dan server lainnya untuk menyimpan jenis data yang dibutuhkan oleh _web application_.

### HTTP Protocol? Huh?

![HTTP Protocol](https://drive.google.com/uc?id=1hwEqsVgHs8JZMSFt6UhFnfKzjSHyvj2N)

_The Hypertext Transfer Protocol_ (HTTP) merupakan protokol utama yang digunakan untuk komunikasi antara sebuah _client_ dan _server_ pada _web application_. HTTP mendefinisikan bagaimana sebuah pesan diformat dan dikirimkan dan juga bagaimana sebuah _server_ dan _client_ harus membalas sebuah _request_. Poin-poin penting yang harus dimengerti dalam Protokol HTTP adalah:

1. Bekerja diatas Protocol TCP (_Transmission Control Protocol_) merupakan sebuah protokol yang digunakan dalam _reliable data transmission_ (pengiriman data antar internet)
2. Mendefinisikan bagaimana sebuah pesan atau data diformat dan di transmit antara _client_ dan _server_.
3. Dieksekusi ketika sebuah _client_ melakukan _request_ kepada _server_, serta _server_ melakukan _response_ kepada _client_.
4. Terdapat beberapa metode HTTP yang paling penting untuk melakukan CRUD (_Create Replicate Update Delete_) untuk diingat adalah
   1. **GET**, hanya mengambil data dari _server_ berupa dokumen seperti HTML, CSS, JS dan lainnya.
   2. **POST**,digunakan untuk _submit_ data yang mengubah _state_ atau kondisi dari _server_ dengan sebuah form data atau _body_ _payload_.
   3. **PUT**, digunakan untuk mengubah data yang sudah ada pada _server_ dengan beberapa _payload_ untuk identifikasi data apa yang akan diubah.
   4. **DELETE**, menghapus entitas data yang ada pada server sehingga mengubah _state_ dari sebuah _server_.

### Client Side Code vs Server Side Code

_Web applications_ melakukan eksekusi kode program dengan menjalankan kode pada _client-side_ (Web browser pengguna) dan juga _server-side_ (Web Server atau komputer penyedia layanan). Kode Program yang digunakan variatif, pada teknologi modern banyak bahasa pemrograman yang menjadi sebuah _framework full stack_ seperti:

1. **PHP** dengan Laravel (Client Side, Server Side)
2. **Python** dengan PyReact (Client Side), Flask (Server Side)
3. **Javascript** dengan ReactJS (Client Side), Express (Server Side), Hapi (Server Side)
4. **Go** dengan Gin (Server Side)
5. **Java**dengan Spring (Server Side)

## What is Web Security?

Keamanan web adalah kategori solusi keamanan yang luas yang melindungi pengguna, perangkat, dan jaringan Anda yang lebih luas dari di internet. Adapun beberapa serangan dibawah ini yang menjadi contoh-contoh mengapa keamanan pada Web Application menjadi penting.

1. **Pelanggaran data**: Serangan injeksi dapat membahayakan data sensitif yang tersimpan dalam basis data, seperti kredensial pengguna, informasi pribadi, atau catatan keuangan.
2. **Unauthorized Access**: Penyerang dapat memperoleh akses tidak sah ke sistem, melewati mekanisme autentikasi dan mendapatkan kendali atas akun pengguna atau hak administratif.
3. **Data Manipulation**: Serangan injeksi dapat memungkinkan penyerang untuk memodifikasi atau memanipulasi data di dalam basis data, yang berpotensi menyebabkan kerusakan data atau perubahan yang tidak sah.
4. **System Compromise**: Dalam beberapa kasus, serangan injeksi dapat memungkinkan penyerang untuk menjalankan perintah sewenang-wenang pada sistem operasi yang mendasarinya, yang berpotensi membahayakan seluruh sistem.
5. **Application Disruption**: Serangan injeksi dapat mengganggu fungsi normal aplikasi web, yang menyebabkan terhentinya layanan, tidak tersedianya layanan, atau menurunnya kinerja.
6. **Kerugian Finansial**: Organisasi dapat mengalami kerugian finansial karena biaya yang terkait dengan respons insiden, pemulihan data, tindakan hukum, dan kerusakan reputasi.
7. **Privacy Violation**: Serangan injeksi dapat mengekspos informasi pengguna yang sensitif, melanggar peraturan privasi dan merusak kepercayaan pengguna.
8. **Malware distribution**: Penyerang dapat menggunakan serangan injeksi sebagai sarana untuk mendistribusikan malware atau mendapatkan pijakan untuk eksploitasi lebih lanjut.

## What Cause of Web Insecurities?

Beberapa hal dibawah ini menjadi salah satu mengapa sebuah web dapat menjadi _vulnerable_.

1. **Input Validation**: Gagal memvalidasi input pengguna dapat memungkinkan penyerang menyuntikkan kode berbahaya ke dalam aplikasi web Anda
2. **Unsecure Code Practices**: Praktik pengkodean yang buruk, seperti tidak menggunakan kueri yang diparameterisasi atau pernyataan yang disiapkan, dapat membuat aplikasi Anda rentan terhadap serangan injeksi
3. **Failed Software Update**: Mengabaikan penerapan patch keamanan dan pembaruan dapat membuat aplikasi Anda terpapar pada kerentanan yang telah diketahui.
4. **Weak Access Control**: Kontrol akses yang tidak memadai dapat memungkinkan penyerang mendapatkan akses tidak sah ke data atau fungsionalitas yang sensitif.
5. **Less Encryption**: Kegagalan mengenkripsi data sensitif dapat memudahkan penyerang untuk mencegat dan memanipulasi informasi.
6. **Unsecure Network**: Jaringan yang tidak dikonfigurasi dengan benar dapat memberikan kesempatan bagi penyerang untuk mengeksploitasi kerentanan dalam sistem Anda.
7. **Bad Logging and Monitoring**: Tanpa mekanisme pencatatan dan pemantauan yang tepat, akan sulit untuk mendeteksi dan merespons serangan injeksi secara tepat waktu.
8. **Vulnerabilities on Packages/Library**: Menggunakan komponen perangkat lunak pihak ketiga yang sudah ketinggalan zaman atau tidak aman dapat menimbulkan kerentanan pada aplikasi Anda.
9. **Social Engineering**: Penyerang dapat menggunakan teknik* social engineering*, seperti phishing atau pretexting, untuk mengelabui pengguna agar mengungkapkan informasi sensitif atau mengeksekusi kode berbahaya.

Penyebab-penyebab ini menyoroti pentingnya menerapkan praktik pengkodean yang aman, memperbarui perangkat lunak secara teratur, dan mempertahankan langkah-langkah keamanan yang kuat untuk melindungi dari peretasan web dan serangan injeksi

Untuk mengurangi risiko ini, sangat penting untuk menerapkan praktik pengkodean yang aman, seperti validasi input, _query params_. Audit keamanan secara teratur dan selalu mengikuti perkembangan patch keamanan terbaru juga penting untuk mencegah serangan injeksi peretasan web.

## What are the attacks on Web?

### Cross Site Scripting (**XSS)**

Serangan XSS melibatkan penyuntikan skrip berbahaya ke dalam situs web tepercaya, yang kemudian dieksekusi oleh peramban pengguna yang tidak menaruh curiga. Hal ini memungkinkan penyerang untuk mencuri informasi sensitif, memanipulasi konten web, atau melakukan tindakan jahat lainnya

### **SQL Injection**

Serangan _SQL Injection_ terjadi ketika penyerang menyisipkan kode SQL berbahaya ke dalam kueri basis data aplikasi web. Hal ini dapat memungkinkan akses yang tidak sah, manipulasi data, atau bahkan membahayakan seluruh sistem

### Denial of Service (**DoS**)and Distributed Denial of Service (**DDoS**)

Serangan DoS dan DDoS bertujuan untuk membanjiri server atau jaringan target dengan lalu lintas yang membludak, sehingga tidak dapat diakses oleh pengguna yang sah. Serangan ini mengganggu operasi normal dan dapat menyebabkan pemadaman layanan

### **API Security Issues**

Masalah keamanan API melibatkan kerentanan dalam desain, implementasi, atau penggunaan antarmuka pemrograman aplikasi (API). Kerentanan ini dapat menyebabkan akses yang tidak sah, pemaparan data, atau pelanggaran keamanan lainnya

### **XML Injection**

Serangan injeksi XML mengeksploitasi kerentanan pada pengurai atau prosesor XML untuk memanipulasi pertukaran data berbasis XML. Penyerang dapat memodifikasi konten XML, menyuntikkan kode berbahaya, atau mendapatkan akses tidak sah ke informasi sensiti

### Server-Side Request Forgery (**SSRF**)

Serangan SSRF melibatkan penipuan / pemalsuan server untuk membuat permintaan ke sumber daya internal atau sistem eksternal atas nama penyerang. Hal ini dapat menyebabkan akses data yang tidak sah, eksekusi kode jarak jauh, atau eksploitasi lainnya

### **Session Hijacking**

_Session Hijacking_ mengacu pada pengambilalihan sesi pengguna secara tidak sah oleh penyerang. Dengan mencuri pengidentifikasi sesi atau cookie, penyerang dapat menyamar sebagai pengguna yang sah dan mendapatkan akses tidak sah ke akun mereka

### **Brute Force Attack**

Serangan _brute force_ melibatkan secara sistematis mencoba semua kemungkinan kombinasi kata sandi atau kunci enkripsi hingga yang benar ditemukan. Penyerang menggunakan alat otomatis untuk melakukan pencarian yang melelahkan ini dan mendapatkan akses yang tidak sah.

### **File Upload Vulnerability**

Kerentanan pengunggahan file terjadi ketika aplikasi web tidak memvalidasi atau membersihkan file yang diunggah pengguna dengan benar. Penyerang dapat mengeksploitasi kelemahan ini untuk mengunggah file berbahaya yang dapat membahayakan server atau mengeksekusi kode arbitrer.

### **OS Injection**

Serangan injeksi OS mengeksploitasi kerentanan dalam sistem operasi untuk menjalankan perintah sewenang-wenang pada sistem target. Penyerang dapat memperoleh akses yang tidak sah, meningkatkan hak istimewa, atau mengkompromikan seluruh sistem

## Before Continuing to JWT Injection Let’s Dive in on What Is JWT

### JSON Web Tokens (**JWT)**

JSON Web Token atau JWT (dibaca “jot”) merupakan merupakan sebuah standar format secara kriptografi yang digunakan sesuai standard [RFC 7519](https://tools.ietf.org/html/rfc7519) untuk melakukan sebuah pertukaran data secara aman dari satu pihak ke pihak lainnya. Hal yang menjadi poin penting dalam JWT adalah:

1.  Ukurannya yang kecil membuat sebuah token JWT dapat dikirim melalui URL, dengan POST method, atau dengan HTTP Header dengan lebih cepat.
2.  JWT memberikan informasi dengan enkripsi yang sudah di _sign_ atau tertanda secara digital.
3.  JWT membawa informasi digital seperti user ID, nama User dan lain-lainnya sesuai dengan kebutuhan sistem.
4.  _Sign_ JWT dilakukan dengan memberikan algoritma **HMAC** atau dapat dengan menggunakan public/private key seperti **RSA.**

### Why Use JWT?

1. **Statelessness**: Tidak seperti _session tokens_, yang menggunakan server-side storage to untuk menyimpan _state_ dari user, JWT tidak diperlukan .
2. **Scalability**: Karena JWTs _stateless_, JWT mudah untuk digunakan dalam layanan-layanan they can be easily scaled across multiple servers or services
3. **Flexibility**: JWTs can be used for various purposes, such as authentication, authorization, or data exchange.
4. **Security**: JWTs use digital signatures to ensure that the token has not been tampered with and that the claims contained within it are valid.

### JWT Structure

```
+------------------------+
 Header 
+------------------------+
 Payload 
+------------------------+
 Signature 
+------------------------+
```

1. **Header**, biasanya terdiri dari dua bagian: jenis token, yaitu JWT, dan algoritma _sign_ yang digunakan, seperti HMAC SHA256 atau RSA. Sebagai contoh: { "alg": "HS256" , "typ": "JWT" }. Kemudian, JSON ini dikodekan dengan Base64Url untuk membentuk bagian pertama dari JWT.
2. **Payload**, payload berisikan sebuah klaim. Klaim adalah pernyataan tentang entitas (biasanya, pengguna) dan data tambahan. Ada tiga jenis klaim: klaim terdaftar, publik, dan pribadi. Klaim terdaftar adalah klaim yang telah ditentukan sebelumnya yang tidak wajib tetapi direkomendasikan untuk menyediakan satu set klaim yang berguna dan dapat dioperasikan. Klaim publik dapat didefinisikan sesuka hati oleh mereka yang menggunakan JWT. Tetapi untuk menghindari tabrakan, klaim tersebut harus didefinisikan di IANA JSON Web Token Registry atau didefinisikan sebagai URI yang berisi ruang nama yang tahan terhadap tabrakan.
3. **Signature**, yang digunakan untuk memverifikasi integritas token dan memastikan bahwa token tersebut belum dirusak. Tanda tangan dihitung dengan mengkodekan header dan payload menggunakan pengkodean Base64Url dan menggabungkannya dengan pemisah titik. String ini kemudian ditandatangani menggunakan kunci rahasia atau kunci pribadi

Contoh sebuah token JWT beserta dengan Komponennya dapat dilihat pada link berikut [JWT.io](https://jwt.io/#debugger-io?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c)

# But is It Secure Enough?

### **JWT Injection**

Serangan injeksi JWT (JSON Web Token) menargetkan manipulasi token berbasis JSON yang digunakan untuk tujuan otentikasi dan otorisasi. Dengan merusak JWT, penyerang dapat melewati langkah-langkah keamanan dan mendapatkan akses yang tidak sah. JWT membawa informasi-informasi yang digunakan dalam

Penyerangan-penyerangan ini memberikan banyak dampak yang buruk terhadap situs web yang tidak aman seperti malicious code kedalam aplikasi web. Serangan injeksi peretasan web, seperti injeksi JWT, injeksi perintah OS, dan injeksi SQL, dapat menyebabkan berbagai konsekuensi dan risiko keamanan.

# Referensi

1. [How the web works - Learn web development | MDN](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/How_the_Web_works)
2. [An overview of HTTP - HTTP | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)
3. [HTTP - Wikipedia](https://en.wikipedia.org/wiki/HTTP)
4. [Web Application Architecture [Complete Guide & Diagrams] | by SoftKraft | Medium](https://medium.com/@softkraft/web-application-architecture-complete-guide-diagrams-1b2d77fe3d2e)
5. [Client-Side vs. Server-Side Code: What's the Difference? (seguetech.com)](https://www.seguetech.com/client-server-side-code/)
6. [Client-Server Architecture - Design Your Software Architecture Using Industry-Standard Patterns - OpenClassrooms](https://openclassrooms.com/en/courses/6397806-design-your-software-architecture-using-industry-standard-patterns/6896156-client-server-architecture)
7. [What Are Injection Attacks and How Can You Prevent Them? (makeuseof.com)](https://www.makeuseof.com/what-are-injection-attacks-how-to-prevent-them/)
8. [Working with JWTs in Burp Suite - PortSwigger](https://portswigger.net/burp/documentation/desktop/testing-workflow/session-management/jwts)
9. [JSON Web Tokens (auth0.com)](https://auth0.com/docs/secure/tokens/json-web-tokens)
10. [JSON Web Token Introduction - jwt.io](https://jwt.io/introduction/)
11. [JWT attacks | Web Security Academy (portswigger.net)](https://portswigger.net/web-security/jwt)
12. [Cross Site Scripting (XSS) | OWASP Foundation](https://owasp.org/www-community/attacks/xss/)
13. [What is SQL Injection? Tutorial & Examples | Web Security Academy](https://portswigger.net/web-security/sql-injection)
14. [What is a denial-of-service (DoS) attack? | Cloudflare](https://www.cloudflare.com/learning/ddos/glossary/denial-of-service/)
15. [Denial-of-service attack - Wikipedia](https://en.wikipedia.org/wiki/Denial-of-service_attack)
16. [What is a distributed denial-of-service (DDoS) attack? | Cloudflare](https://www.cloudflare.com/learning/ddos/what-is-a-ddos-attack/)
17. [What is a Cross-Site Scripting (XSS) attack: Definition & Examples](https://www.ptsecurity.com/ww-en/analytics/knowledge-base/what-is-a-cross-site-scripting-xss-attack/)
18. [Web Server and its Types of Attacks - GeeksforGeeks](https://www.geeksforgeeks.org/web-server-and-its-types-of-attacks/)
19. [What is a Denial-of-Service (DoS) Attack? | Rapid7](https://www.rapid7.com/fundamentals/denial-of-service-attacks/)
20. [How Hackers Take Over Web Sites with SQL Injection and DDoS](https://www.howtogeek.com/97971/htg-explains-how-hackers-take-over-web-sites-with-sql-injection-ddos/)
21. [SQL Injection | OWASP Foundation](https://owasp.org/www-community/attacks/SQL_Injection)
22. [What is a SQL Injection Attack? - CrowdStrike](https://www.crowdstrike.com/cybersecurity-101/sql-injection)
23. [What Is a Cross-Site Scripting (XSS) Attack? - CrowdStrike](https://www.crowdstrike.com/cybersecurity-101/cross-site-scripting-xss/)
24. [What is cross-site scripting (XSS) and how to prevent it? | Web Security Academy](https://portswigger.net/web-security/cross-site-scripting)
25. [What is a denial of service attack (DoS) ? - Palo Alto Networks](https://www.paloaltonetworks.com/cyberpedia/what-is-a-denial-of-service-attack-dos)
26. [Denial-of-Service (DoS) Attack: Examples and Common Targets](https://www.investopedia.com/terms/d/denial-service-attack-dos.asp)
27. [What is a Denial of Service (DoS) Attack? | Webopedia](https://www.webopedia.com/definitions/dos-attack/)
28. [What Is a DDoS Attack? Distributed Denial of Service - Cisco](https://www.cisco.com/c/en/us/products/security/what-is-a-ddos-attack.html)
29. [DDoS attacks: Definition, examples, and techniques | CSO Online](https://www.csoonline.com/article/571981/ddos-attacks-definition-examples-and-techniques.html)
