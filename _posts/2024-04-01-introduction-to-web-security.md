# Building Secure Apps - 01: Introduction to Web Security

## Introduction to Web Security

### What is Web Security?

**Web security** refers to the measures and practices that are implemented to protect websites, web applications, and web services from malicious attacks and vulnerabilities. As the internet has become the backbone of business operations, online transactions, and communication, web security has emerged as a critical area of focus for both businesses and developers.

In the digital age, web security is essential for ensuring the **confidentiality**, **integrity**, and **availability** of data. With the increasing reliance on web applications for everything from e-commerce to cloud storage, **web security** has become indispensable for preventing **data breaches**, **theft**, and **unauthorized access** to sensitive information.

#### Why Web Security Matters

1. **Preventing Data Breaches**

   - Every web application collects and processes various forms of data, from personal identifiable information (PII) to financial transactions. Without proper web security practices, this data becomes vulnerable to hackers who can exploit weaknesses to steal sensitive information.

2. **Maintaining Business Continuity**

   - A cyberattack can lead to service disruptions, making websites and web applications temporarily unavailable. This affects **business continuity** and can lead to financial losses, loss of reputation, and diminished customer trust.

3. **Protecting User Privacy**

   - Web security also safeguards user privacy. Inadequate security measures can expose users to identity theft, phishing attacks, and other threats that compromise their personal and financial information.

4. **Compliance with Legal Regulations**

   - Web security is essential for complying with laws and regulations such as **GDPR**, **HIPAA**, and **PCI DSS**. Failure to comply with these regulations can result in heavy fines and legal consequences.

**Example of a Data Breach:**

In 2017, **Equifax**, one of the largest credit reporting agencies, suffered a massive data breach due to a vulnerability in their web application. The breach exposed the personal information of approximately 147 million individuals, including names, addresses, social security numbers, and credit card details. The breach could have been prevented by patching known security vulnerabilities.

### The Increasing Threat Landscape

With the rapid advancement of technology, the **threat landscape** for web applications has grown more complex. Cybercriminals continuously evolve their tactics to exploit vulnerabilities in web applications, making it crucial for businesses and developers to stay one step ahead.

#### Overview of the Rise in Cyberattacks Targeting Web Applications

1. **Increased Frequency of Cyberattacks**

   - Web applications have become prime targets for cybercriminals because of their accessibility and the sensitive data they often store. According to reports, **43% of all cyberattacks** target small businesses, many of which rely heavily on their web presence for day-to-day operations.
   - Attack techniques such as **phishing**, **malware injection**, and **denial of service (DoS) attacks** are growing more sophisticated, leaving web applications more vulnerable than ever.

2. **Exploitation of Vulnerabilities**

   - Many web applications are built using third-party libraries, frameworks, and APIs. If these components are outdated or contain vulnerabilities, attackers can easily exploit them to gain unauthorized access. For example, the **Log4j vulnerability** exposed millions of applications worldwide in 2021, allowing attackers to remotely execute code on targeted systems.

#### How Vulnerabilities in Web Applications Expose Businesses and Users to Significant Risks

1. **Financial Losses**

   - Data breaches and other cyberattacks can result in **massive financial losses** for businesses. From the cost of recovering lost data to legal fees and fines, the financial impact can be devastating, especially for small to medium-sized businesses.

2. **Reputational Damage**

   - Businesses that experience security breaches often suffer reputational damage. Customers lose trust in the company's ability to protect their data, which can lead to **customer churn**, negative press, and long-term brand damage.

3. **User Impact**

   - For users, the risks associated with web application vulnerabilities are even more personal. Breaches can lead to **identity theft**, **unauthorized financial transactions**, and **privacy violations**. Users expect that businesses will protect their sensitive data, and when that expectation is not met, it can have a lasting impact on user trust and engagement.

**The Growing Threat Landscape Example:**

In the past few years, **ransomware** attacks have seen a sharp rise. Hackers target businesses by encrypting their web applications or databases and demanding a ransom to restore access. Many organizations, lacking proper security measures and backups, have been forced to pay large sums to recover their data.

Code Snippet Example – **Basic Security Header Setup**:

```javascript
// Example of setting security headers in Node.js using Helmet middleware
const express = require("express");
const helmet = require("helmet");
const app = express();

app.use(helmet()); // Helmet sets various HTTP headers to secure the app

// Example of setting custom security headers
app.use((req, res, next) => {
  res.setHeader("X-Content-Type-Options", "nosniff");
  res.setHeader(
    "Strict-Transport-Security",
    "max-age=31536000; includeSubDomains"
  );
  res.setHeader("X-Frame-Options", "DENY");
  next();
});

app.listen(3000, () => {
  console.log("Server running on port 3000");
});
```

**Explanation:**

In this example, we use the **Helmet** middleware for Node.js, which automatically sets security headers to help mitigate common web vulnerabilities. Additionally, we manually set **X-Content-Type-Options**, **Strict-Transport-Security**, and **X-Frame-Options** headers to enforce further security measures such as preventing clickjacking and ensuring the app only communicates over HTTPS.

## Why Web Security Matters

### Impact on Businesses

Web security has a direct and significant impact on businesses, regardless of their size or industry. The consequences of failing to secure a web application can be far-reaching, affecting a company’s financial health, reputation, and legal standing. Here’s why it’s crucial for businesses to prioritize security:

**Financial Losses Due to Breaches**

Cyberattacks and data breaches can lead to substantial financial losses. Companies often face costs related to:

- **Recovering data**
- **Paying fines or settlements**
- **Rebuilding infrastructure**
- **Implementing stronger security systems post-breach**
- **Paying for legal actions or regulatory penalties**

Here are a few high-profile examples of security breaches and their financial impact:

1. **Target (2013)**

   - Target experienced a significant data breach where hackers stole credit and debit card information from over **40 million customers**.
   - Financial Impact: Target reported **$162 million** in expenses related to the breach, including settlements with banks and customers.

2. **Equifax (2017)**

   - Equifax’s breach, due to a web application vulnerability, exposed the sensitive personal information (including social security numbers) of approximately **147 million** individuals.
   - Financial Impact: Equifax faced **$700 million** in fines and settlement fees related to the breach, not to mention long-term reputational damage.

3. **Marriott (2018)**

   - Marriott’s breach compromised the personal data of around **500 million guests** due to insufficient security controls over its web services.
   - Financial Impact: The company faced fines of **$23.8 million** for violating **GDPR** and other regulations.

The financial burden of these attacks extends beyond immediate losses. Businesses often face increased costs due to loss of business, compensation for affected customers, and investments in improved cybersecurity infrastructure to prevent future attacks.

**Reputational Damage Caused by Lack of Security**

Beyond financial losses, security breaches can lead to **reputational damage** that can be difficult to recover from. Businesses rely on user trust to retain customers and drive engagement. A breach can erode that trust, and rebuilding it can take years.

- **Customer Churn**: After a security breach, customers often lose confidence in the company’s ability to protect their data. This leads to a significant drop in customer retention rates, as consumers prefer to engage with businesses they perceive as secure.
- **Brand Damage**: In today’s media-driven environment, a security breach can quickly become headline news, causing long-lasting harm to a company’s reputation. Negative press coverage can discourage potential customers from engaging with the brand, resulting in decreased sales and long-term customer hesitancy.
- **Loss of Partnerships**: Many businesses rely on partnerships with other companies. A security breach can tarnish those relationships, as partners may no longer trust the business’s security measures, potentially ending strategic collaborations.

**Example: Facebook’s Cambridge Analytica Scandal**

Although this was not a direct breach, Facebook faced immense reputational damage following the **Cambridge Analytica** scandal in 2018, where user data was mishandled and exploited without consent. The scandal led to **#DeleteFacebook** movements, declining user engagement, and increased scrutiny on Facebook’s data policies.

**Compliance and Regulatory Consequences**

Failing to secure web applications can result in **legal repercussions** and regulatory penalties, especially for businesses handling personal data, financial information, or healthcare records. Regulatory frameworks such as **GDPR** (General Data Protection Regulation), **HIPAA** (Health Insurance Portability and Accountability Act), and **PCI DSS** (Payment Card Industry Data Security Standard) have established stringent requirements for the protection of sensitive data.

1. **GDPR (Europe)**

   - GDPR imposes hefty fines on organizations that fail to protect the personal data of European Union (EU) citizens.
   - Maximum penalties for GDPR violations can reach up to **€20 million** or **4% of global revenue**, whichever is higher.

2. **HIPAA (United States)**

   - HIPAA regulates the handling of healthcare data and imposes penalties on organizations that mishandle patient information.
   - Fines for HIPAA violations range from **$100 to $50,000** per violation, depending on the severity.

3. **PCI DSS (Global)**

   - PCI DSS sets security standards for companies processing credit card payments. Non-compliance can result in fines ranging from **$5,000 to $100,000** per month, in addition to the loss of the ability to process payments.

**Code Snippet – Basic Data Encryption Example**:

```javascript
// Example of encrypting data using Node.js crypto module
const crypto = require("crypto");
const algorithm = "aes-256-cbc";
const key = crypto.randomBytes(32);
const iv = crypto.randomBytes(16);

function encrypt(text) {
  let cipher = crypto.createCipheriv(algorithm, Buffer.from(key), iv);
  let encrypted = cipher.update(text);
  encrypted = Buffer.concat([encrypted, cipher.final()]);
  return { iv: iv.toString("hex"), encryptedData: encrypted.toString("hex") };
}

function decrypt(text) {
  let iv = Buffer.from(text.iv, "hex");
  let encryptedText = Buffer.from(text.encryptedData, "hex");
  let decipher = crypto.createDecipheriv(algorithm, Buffer.from(key), iv);
  let decrypted = decipher.update(encryptedText);
  decrypted = Buffer.concat([decrypted, decipher.final()]);
  return decrypted.toString();
}

const encrypted = encrypt("Sensitive Data");
console.log(encrypted); // Output: Encrypted data
```

**Explanation**:

This example uses Node.js’s **crypto** module to encrypt sensitive data before storing or transmitting it. AES (Advanced Encryption Standard) ensures data is securely encrypted using a key and an initialization vector (IV).

### Impact on Users

The impact of poor web security extends beyond businesses to the individual users whose data is compromised. A security breach puts users at significant risk, from identity theft to financial fraud.

**Privacy Concerns**

1. **Exposure of Personal and Financial Data**

   - Users entrust businesses with their personal and financial information, such as email addresses, passwords, and credit card numbers. A breach can lead to this data being sold on the dark web or used in fraudulent activities, compromising user privacy and security.

2. **Identity Theft**

   - Breaches involving personal information, like social security numbers and identification details, can lead to **identity theft**. Victims of identity theft often face long-term consequences, such as damaged credit scores, fraudulent loans, and unauthorized purchases.

**User Trust and Engagement**

1. **Erosion of Trust**

   - Users expect that their data is being handled with care. When a breach occurs, that trust is immediately broken. Even if a company quickly addresses the breach, users may still be hesitant to continue using the service.

2. **User Engagement and Loyalty**

   - Web security is a cornerstone of user loyalty. Businesses that fail to provide adequate security often experience lower **user retention** and engagement. On the other hand, companies that prioritize security build stronger, longer-lasting relationships with their users.

**Example of Trust Erosion**: Following the **Yahoo data breach** in 2013, where the personal information of all **3 billion** of its users was compromised, the company’s reputation suffered immensely. Many users abandoned Yahoo services for more secure alternatives, and the company faced numerous lawsuits.

## Common Web Security Threats

Web applications face a variety of security threats that can expose sensitive data, disrupt services, and cause financial or reputational damage to businesses. Many of these threats are well-documented, with organizations like the **Open Web Application Security Project (OWASP)** providing guidelines for identifying and mitigating common vulnerabilities. In this section, we will explore the **OWASP Top 10**, along with real-world examples and solutions for protecting web applications against these threats.

### Overview of the Most Prevalent Web Threats

The **OWASP Top 10** is a well-known list of the most critical security risks to web applications. It includes the most prevalent vulnerabilities and provides recommendations for mitigating them. Below is a brief overview of the most common threats:

1. **Injection Attacks (SQL Injection)**: Malicious data is sent to an interpreter via forms or URLs to manipulate databases or execute unauthorized commands.
2. **Broken Authentication**: Weak or improperly implemented authentication methods can lead to unauthorized access to accounts or systems.
3. **Sensitive Data Exposure**: Insufficient protection of sensitive data such as financial information or passwords.
4. **XML External Entities (XXE)**: Vulnerability related to outdated or improperly configured XML processors.
5. **Broken Access Control**: Weak access control allows attackers to gain access to data or functionality they should not be able to reach.
6. **Security Misconfigurations**: Default or insecure settings in software can leave applications exposed.
7. **Cross-Site Scripting (XSS)**: Malicious scripts injected into trusted websites to steal user data or manipulate page content.
8. **Insecure Deserialization**: Deserialization of untrusted data that leads to attacks like remote code execution.
9. **Using Components with Known Vulnerabilities**: Outdated third-party components or libraries that introduce vulnerabilities.
10. **Insufficient Logging and Monitoring**: Failure to log critical security events or monitor systems for potential breaches.

These vulnerabilities can be exploited by attackers to compromise web applications, gain unauthorized access to sensitive information, or disrupt operations. Let’s take a closer look at some of the most common threats.

### Detailed Breakdown of Common Threats

#### 1. SQL Injection

**Definition**: **SQL injection** is one of the oldest and most dangerous web application vulnerabilities. It occurs when an attacker manipulates a web application’s query parameters or form inputs to inject malicious SQL code into the backend database. This allows the attacker to **read**, **modify**, or **delete** data from the database without authorization.

- **How it Works**: Attackers manipulate SQL queries by injecting malicious input into fields like search boxes, login forms, or URL parameters.

**Example of Vulnerable Code**:

```javascript
// Vulnerable to SQL injection
const userId = req.query.userId;
const query = `SELECT * FROM users WHERE id = '${userId}'`;
db.query(query, (err, result) => {
  if (err) throw err;
  console.log(result);
});
```

In this example, the `userId` is directly injected into the SQL query, allowing an attacker to inject arbitrary SQL code.

**Impact**:

- Data exposure, modification, or deletion.
- Potential control of the underlying database.
- Denial of service by dropping critical tables.

**Example of Secure Code Using Prepared Statements**:

```javascript
// Using parameterized queries to prevent SQL injection
const userId = req.query.userId;
const query = `SELECT * FROM users WHERE id = ?`;
db.query(query, [userId], (err, result) => {
  if (err) throw err;
  console.log(result);
});
```

**Explanation**: By using **prepared statements** or **parameterized queries**, the user input is treated as data, not executable code, preventing malicious SQL from being injected.

#### 2. Cross-Site Scripting (XSS)

**Definition**: **Cross-Site Scripting (XSS)** allows attackers to inject malicious scripts into webpages viewed by other users. These scripts can steal user cookies, session tokens, or sensitive data, or perform unauthorized actions on behalf of the user.

- **How it Works**: The attacker injects malicious JavaScript or HTML code into web pages through vulnerable input fields (such as comments or search boxes), which are then executed by the browser of an unsuspecting user.

**Example of Vulnerable Code**:

```html
<!-- Vulnerable to XSS attacks -->
<div>
  Search results for: <b><?= $_GET['searchQuery'] ?></b>
</div>
```

This code directly outputs user input (`searchQuery`) without validating or escaping it, which makes it vulnerable to XSS attacks.

**Impact**:

- Theft of session cookies, leading to account hijacking.
- Unauthorized actions performed on behalf of users.
- Malicious scripts spreading across the application.

**XSS Prevention Techniques**:

1. **Input Validation**: Ensure that all user inputs are properly validated, rejecting malicious characters or scripts.
2. **Output Encoding**: Encode data before displaying it in the browser to prevent it from being executed as a script.

**Example of Secure Code Using Output Encoding**:

```html
<!-- Escaping user input to prevent XSS -->
<div>
  Search results for:
  <b><?= htmlspecialchars($_GET['searchQuery'], ENT_QUOTES, 'UTF-8') ?></b>
</div>
```

**Explanation**: By using `htmlspecialchars()` in PHP, we encode special characters to prevent them from being interpreted as code by the browser, thus mitigating XSS attacks.

#### 3. Cross-Site Request Forgery (CSRF)

**Definition**: **Cross-Site Request Forgery (CSRF)** is a type of attack in which an attacker tricks the user into submitting an unwanted request to a web application in which the user is authenticated. This can result in unauthorized actions such as changing account settings, transferring funds, or making purchases on the user’s behalf.

- **How it Works**: The attacker tricks a victim into clicking a malicious link or loading a malicious image on a legitimate site. This results in an unintended action being performed in the user’s account.

**Example of Vulnerable Code**:

```html
<!-- Vulnerable form without CSRF protection -->
<form action="/change-email" method="POST">
  <input type="email" name="newEmail" value="victim@example.com" />
  <input type="submit" value="Change Email" />
</form>
```

In this example, without CSRF protection, an attacker could trick the user into submitting this form, changing the user's email address without consent.

**Impact**:

- Unauthorized actions performed on behalf of the user.
- Compromise of sensitive data and account information.
- Disruption of application functionality.

**Preventing CSRF Using CSRF Tokens**:

- CSRF tokens are unique, unpredictable values generated for each session and form submission. They ensure that requests are valid and come from the authenticated user.

**Example of Secure Code Using CSRF Tokens**:

```html
<!-- Form with CSRF protection -->
<form action="/change-email" method="POST">
  <input
    type="hidden"
    name="csrf_token"
    value="<?= $_SESSION['csrf_token'] ?>"
  />
  <input type="email" name="newEmail" />
  <input type="submit" value="Change Email" />
</form>
```

**Explanation**: The `csrf_token` is stored in the user’s session and must be submitted with the form. If the token doesn’t match the session value, the request is rejected, preventing CSRF attacks.

**Backend CSRF Token Validation (Example in Node.js)**:

```javascript
// Example of validating CSRF token in Node.js
app.post("/change-email", (req, res) => {
  const { csrf_token, newEmail } = req.body;

  // Check if token matches the one stored in the session
  if (csrf_token !== req.session.csrf_token) {
    return res.status(403).send("Invalid CSRF token");
  }

  // Process email change
  // ...
});
```

## Key Areas of Focus for Securing Modern Web Applications

Securing modern web applications requires addressing several critical areas, from authentication to data encryption and input validation. Let’s dive into the key areas of focus to ensure web applications are safe from common vulnerabilities and threats.

### Authentication and Authorization

**Authentication** and **Authorization** are two foundational concepts in web security. Without proper controls in place, attackers can exploit weak points in these areas to gain unauthorized access to sensitive data and systems.

**Importance of Secure Authentication (OAuth, JWT, etc.)**

1. **Authentication**: Ensures that users are who they claim to be. Web applications often use a combination of credentials (like usernames and passwords) to verify a user’s identity.
2. **OAuth (Open Authorization)**: A widely-used protocol for secure authentication, OAuth allows users to authorize applications to interact with their data without sharing credentials (e.g., **Sign in with Google**).
3. **JWT (JSON Web Tokens)**: JWT is used to securely transmit information between parties. In web applications, JWT is often used to authenticate users after they log in. Tokens are generated upon login and then used to authenticate the user for subsequent requests.

**Code Snippet – JWT Authentication Example**:

```javascript
// JWT authentication in Node.js using jsonwebtoken package
const jwt = require("jsonwebtoken");

// Generate JWT token on login
function generateToken(user) {
  const token = jwt.sign({ id: user.id, role: user.role }, "secret_key", {
    expiresIn: "1h",
  });
  return token;
}

// Verify JWT token in protected routes
function verifyToken(req, res, next) {
  const token = req.headers["authorization"];
  if (!token) return res.status(403).send("No token provided");

  jwt.verify(token, "secret_key", (err, decoded) => {
    if (err) return res.status(500).send("Failed to authenticate token");
    req.userId = decoded.id;
    next();
  });
}
```

**Explanation**: The `generateToken` function creates a JWT token using the user's ID and role, while the `verifyToken` middleware function ensures that only authenticated users can access certain routes.

**Ensuring Role-Based Access Control (RBAC)**

**Authorization** defines what a user can do after they have been authenticated. Role-Based Access Control (RBAC) ensures that users only have access to the resources and actions they are authorized for, based on their role (e.g., admin, user, or guest).

**Key Steps for Implementing RBAC**:

- Define roles (e.g., Admin, Editor, Viewer).
- Assign permissions to each role (e.g., Admins can delete users, Editors can create content).
- Assign users to roles.

**Code Snippet – RBAC Implementation**:

```javascript
// Middleware for role-based access control
function checkRole(role) {
  return function (req, res, next) {
    if (req.userRole !== role) {
      return res.status(403).send("Access Denied");
    }
    next();
  };
}

// Route only accessible by Admins
app.post("/admin", verifyToken, checkRole("Admin"), (req, res) => {
  res.send("Welcome Admin");
});
```

**Explanation**: The `checkRole` function ensures that only users with the specified role can access certain routes. If a user tries to access a route they are not authorized for, they will receive an "Access Denied" response.

### Data Encryption

Securing data in transit and at rest is critical for protecting sensitive information from unauthorized access or tampering. Encryption ensures that even if data is intercepted, it remains unreadable without the proper decryption keys.

**Securing Data in Transit (SSL/TLS)**

Data transmitted between the client and server should always be encrypted to prevent interception by malicious actors. SSL (Secure Sockets Layer) and TLS (Transport Layer Security) are cryptographic protocols used to secure data in transit by encrypting it.

- **SSL/TLS Certificates**: A valid SSL/TLS certificate ensures secure communication over HTTPS, preventing man-in-the-middle attacks.

**Code Snippet – Setting Up HTTPS in Node.js**:

```javascript
const https = require("https");
const fs = require("fs");

// Load SSL certificates
const options = {
  key: fs.readFileSync("key.pem"),
  cert: fs.readFileSync("cert.pem"),
};

// Create HTTPS server
https
  .createServer(options, (req, res) => {
    res.writeHead(200);
    res.end("Secure Connection");
  })
  .listen(443);
```

**Securing Data at Rest (AES, RSA)**

Data at rest, such as stored files or database records, must also be encrypted. Symmetric encryption algorithms like **AES (Advanced Encryption Standard)** and asymmetric encryption like **RSA** are commonly used for this purpose.

**Code Snippet – AES Encryption Example**:

```javascript
const crypto = require("crypto");
const algorithm = "aes-256-ctr";
const secretKey = crypto.randomBytes(32);
const iv = crypto.randomBytes(16);

// Encrypt data
function encrypt(text) {
  const cipher = crypto.createCipheriv(algorithm, secretKey, iv);
  const encrypted = Buffer.concat([cipher.update(text), cipher.final()]);
  return { iv: iv.toString("hex"), content: encrypted.toString("hex") };
}

// Decrypt data
function decrypt(hash) {
  const decipher = crypto.createDecipheriv(
    algorithm,
    secretKey,
    Buffer.from(hash.iv, "hex")
  );
  const decrypted = Buffer.concat([
    decipher.update(Buffer.from(hash.content, "hex")),
    decipher.final(),
  ]);
  return decrypted.toString();
}
```

**Explanation**: This example demonstrates AES encryption in Node.js, where sensitive data is encrypted and stored in an unreadable format, and decrypted when needed.

### Input Validation and Output Encoding

Preventing injection attacks such as **SQL injection** and **XSS** requires careful validation of user inputs and proper encoding of output before rendering it on a web page.

**Preventing Injection Attacks by Validating User Inputs**

Properly validating user inputs ensures that only legitimate data is accepted, reducing the risk of attacks that rely on malicious input.

**Code Snippet – Example of Input Validation**:

```javascript
// Simple input validation using express-validator
const { check, validationResult } = require("express-validator");

app.post(
  "/submit",
  [check("email").isEmail(), check("age").isInt({ min: 0, max: 120 })],
  (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }
    res.send("Data is valid");
  }
);
```

**Explanation**: In this code, the `express-validator` package is used to validate user inputs, ensuring that only valid emails and age values are accepted.

**Output Encoding to Prevent XSS**

Encoding output before rendering it in the browser helps prevent **XSS attacks** by ensuring that user-submitted data is treated as text rather than executable code.

**Code Snippet – Secure Output Encoding**:

```html
<!-- Escaping user input to prevent XSS -->
<div>Comment: <b>{{ comment | escape }}</b></div>
```

**Explanation**: By escaping special characters in the user-submitted `comment` field, the web application prevents it from being executed as malicious JavaScript.

### Secure API Development

APIs are a key target for attackers because they often expose sensitive data and business logic. Following best practices for securing APIs is essential to protecting both the server and the clients interacting with the API.

**Best Practices for Securing APIs (Rate Limiting, Authentication, etc.)**

1. **Authentication**: Use robust authentication mechanisms like **OAuth 2.0** or **API keys** to ensure that only authorized users can access the API.
2. **Rate Limiting**: Implement rate limiting to prevent abuse of the API by restricting the number of requests a user can make within a given time frame.
3. **Data Validation**: Always validate data received from API requests to prevent malicious inputs.

**Code Snippet – Securing a REST API Endpoint with JWT**:

```javascript
// Middleware for verifying JWT in API requests
function authenticateJWT(req, res, next) {
  const token = req.header("Authorization");
  if (!token) return res.status(401).send("Access Denied");

  jwt.verify(token, "secret_key", (err, user) => {
    if (err) return res.status(403).send("Invalid Token");
    req.user = user;
    next();
  });
}

// Secure API route
app.get("/api/protected", authenticateJWT, (req, res) => {
  res.json({ message: "Protected data" });
});
```

**Explanation**: This code demonstrates how to secure an API endpoint using **JWT authentication**. Only users with a valid JWT token can access the protected route.

### **Session Management**

Proper session management is essential to ensure that user sessions are secure and not vulnerable to hijacking or fixation attacks.

**How to Securely Manage User Sessions (Using Secure Cookies, Expiration Policies)**

1. **Secure Cookies**: Use the `Secure` and `HttpOnly` flags for cookies to prevent them from being accessed by JavaScript or sent over unencrypted connections.

2. **Session Expiration**: Implement session expiration policies to automatically log users out after a period of inactivity.

**Code Snippet – Secure Cookie Settings in Express.js**:

```javascript
app.use(session({
  secret: 'your_secret_key',
  resave: false,
  saveUninitialized: true,
  cookie: {
    secure: true, // Ensures cookie is

 sent only over HTTPS
    httpOnly: true, // Prevents JavaScript from accessing cookie
    maxAge: 60000 // Session expires after 60 seconds of inactivity
  }
}));
```

**Explanation**: In this example, cookies are set to **secure** and **HttpOnly**, preventing them from being accessed through JavaScript or sent over non-HTTPS connections.
