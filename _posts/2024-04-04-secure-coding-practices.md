# Building Secure Apps - 04: Secure Coding Practices

## Introduction

### What is Secure Coding?

**Secure Coding** is the practice of writing code with the intent to safeguard applications from vulnerabilities, breaches, and other security threats. By adopting secure coding practices, developers ensure that their applications are designed with defenses against common and advanced cyber threats, making them resilient to unauthorized access, data leaks, and exploitation.

In today’s digital landscape, where web applications handle vast amounts of sensitive user data—such as personal details, financial records, and health information—the importance of secure coding has never been higher. It is no longer optional but necessary for businesses and developers to proactively integrate security measures into their code, particularly during development. Without secure coding, applications become vulnerable entry points for attackers, resulting in costly breaches and data loss, eroded customer trust, and even legal repercussions.

To understand secure coding’s role in modern web development, let’s break down a few key reasons it is essential:

1. **Preventing Vulnerabilities Early**: Fixing security issues during development is far more cost-effective and less disruptive than addressing them post-deployment.
2. **Protecting Sensitive Data**: With secure coding practices, applications can safely store, process, and transmit user data, reducing the risk of leaks or breaches.
3. **Maintaining Trust and Compliance**: Secure applications build user trust and often meet industry compliance standards, which is crucial for businesses handling sensitive information.

#### Example of Insecure vs. Secure Code

Consider a login form that directly includes user input in a database query without validation. Such practices can lead to SQL injection attacks, where attackers can manipulate queries to access unauthorized data.

Insecure Code Example:

```javascript
const query = `SELECT * FROM users WHERE username = '${input.username}' AND password = '${input.password}'`;
```

Secure Code Example (with parameterized queries):

```javascript
const query = "SELECT * FROM users WHERE username = ? AND password = ?";
db.execute(query, [input.username, input.password]);
```

This secure code example prevents the possibility of SQL injection by using parameterized queries, ensuring that inputs are treated as data, not code.

### Goals of Secure Coding Practices

Secure coding practices are essential for achieving four primary goals, each contributing to the overall security, functionality, and compliance of the application:

1. **Minimizing Vulnerabilities**: By following secure coding guidelines, developers can significantly reduce the risk of common vulnerabilities, such as SQL injection, cross-site scripting (XSS), and cross-site request forgery (CSRF). This approach focuses on identifying and addressing potential issues in the code, making the application resilient to attacks.
2. **Protecting Sensitive Data**: Applications often handle sensitive user information, such as passwords, financial data, and personal records. Secure coding practices ensure that such data is protected through encryption, secure storage, and access controls. For example, using strong hashing algorithms like bcrypt for passwords ensures that even if a database is compromised, sensitive data remains secure.
3. **Ensuring Code Integrity**: Code integrity means maintaining the original functionality and logic of the application while preventing unauthorized code alterations. Secure coding practices ensure that code is less susceptible to tampering, guaranteeing that the application behaves as intended. This is critical for high-stakes applications, like financial systems and health platforms, where errors could lead to severe consequences.
4. **Maintaining Compliance**: Many industries have specific security requirements to comply with regulations such as GDPR, HIPAA, and PCI-DSS. Secure coding practices help applications meet these standards by integrating security at the code level, reducing the likelihood of non-compliance penalties.

By embedding secure coding practices into the development phase, developers can build applications that are resilient, efficient, and compliant with security regulations. This proactive approach not only minimizes potential risks but also strengthens user trust and long-term business success.

## Principles of Secure Coding

### Input Validation

Input validation is one of the cornerstones of secure coding, serving as a primary defense mechanism against a wide range of attacks, particularly injection attacks like SQL injection and command injection. Input validation verifies that user inputs meet specific criteria (such as type, format, length, or range) before processing them in the application.

By validating inputs early, applications reduce the risk of inadvertently executing malicious code injected by attackers. Common validation techniques include:

- **Type Checking**: Ensures the input data matches the expected data type, e.g., strings for usernames, integers for age.
- **Length Checks**: Limits the length of inputs to prevent buffer overflow attacks and other length-based exploits.
- **Sanitization**: Removes potentially dangerous characters or sequences from user inputs (e.g., `<script>` tags in an HTML form to prevent XSS).

#### Code Snippet: Input Validation in Node.js

Here’s an example of basic input validation for a form submission in a Node.js application using the `express-validator` library.

```javascript
const { body, validationResult } = require("express-validator");

app.post(
  "/register",
  [
    body("username").isAlphanumeric().isLength({ min: 5, max: 15 }).trim(),
    body("email").isEmail(),
    body("password").isLength({ min: 8 }),
  ],
  (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }
    // Proceed with registration logic
  }
);
```

In this code, we validate that:

- `username` is alphanumeric and between 5-15 characters.
- `email` is a valid email format.
- `password` is at least 8 characters long.

### Output Encoding

Output encoding is crucial for preventing cross-site scripting (XSS) attacks and other injection vulnerabilities. While input validation checks the format of data before it’s stored or processed, output encoding ensures that user-supplied data is correctly displayed to prevent it from being executed as code.

Depending on where data is displayed, different encoding types are required:

- **HTML Encoding**: Escapes characters like `<`, `>`, `&`, which are used in HTML tags.
- **JavaScript Encoding**: Escapes characters within JavaScript code to prevent script injection.
- **URL Encoding**: Ensures URLs are valid by escaping special characters.

#### Code Snippet: Output Encoding in JavaScript with `he` Library

The following example uses the `he` library, which encodes special characters to prevent malicious HTML content from executing.

```javascript
const he = require("he");

app.get("/user", (req, res) => {
  const username = he.encode(req.query.username);
  res.send(`<h1>Hello, ${username}</h1>`);
});
```

Here, `he.encode` escapes any special characters in `username` before embedding it into the HTML. If a user tries to inject a script tag, it will be rendered harmlessly as text.

### Authentication and Authorization

Authentication and authorization are critical principles in secure coding:

- **Authentication** verifies user identity, ensuring that only legitimate users access the application. Secure practices include strong password storage (e.g., hashing with bcrypt) and multi-factor authentication (MFA).
- **Authorization** determines user access rights within the application. Proper authorization prevents privilege escalation, ensuring users only access their designated resources.

#### Code Snippet: Secure Authentication with bcrypt in Express

Below is a secure authentication example in Express using `bcrypt` for password hashing:

```javascript
const bcrypt = require('bcrypt');
const saltRounds = 10;

// Registering a user with hashed password
app.post('/register', async (req, res) => {
  try {
    const hashedPassword = await bcrypt.hash(req.body.password, saltRounds);
    // Store hashed password in database
    // User creation logic here
  } catch (error) {
    res.status(500).send('Error registering user');
  }
});

// Authenticating a user
app.post('/login', async (req, res) => {
  const user = /* Fetch user from database by username */
  const isMatch = await bcrypt.compare(req.body.password, user.password);
  if (isMatch) {
    // Generate token or session
    res.status(200).send('Login successful');
  } else {
    res.status(401).send('Invalid credentials');
  }
});
```

This code securely stores user passwords by hashing them before storing them in the database. During login, it verifies passwords by comparing the hashed password stored in the database with the provided password.

### Session Management

Effective session management is essential for secure web applications, as poorly managed sessions expose users to session hijacking, fixation, and unauthorized access risks. Best practices include:

- **Secure Cookies**: Mark cookies as `HttpOnly` and `Secure` to prevent JavaScript access and enforce HTTPS transmission.
- **Session Expiration**: Set session expiration policies to reduce the risk of stolen session cookies being reused.
- **Session Timeouts**: Terminate sessions after a period of inactivity.

#### Code Snippet: Session Management in Express with `express-session`

The following code configures secure sessions in an Express application:

```javascript
const session = require("express-session");

app.use(
  session({
    secret: "your-secret-key",
    resave: false,
    saveUninitialized: true,
    cookie: {
      httpOnly: true, // Prevents JavaScript access
      secure: true, // Ensures cookies are sent over HTTPS
      maxAge: 60 * 60 * 1000, // 1-hour session expiration
    },
  })
);

app.get("/dashboard", (req, res) => {
  if (req.session.userId) {
    res.send("Welcome to your dashboard");
  } else {
    res.status(401).send("Unauthorized");
  }
});
```

In this setup:

- The session cookie is set as `httpOnly` and `secure`, limiting access and transmission.
- The session expires after one hour, enforcing timely logouts.

These core principles—input validation, output encoding, authentication and authorization, and session management—form the foundation of secure coding practices. By consistently implementing these strategies, developers can build resilient applications that protect both users and data from security threats.

## Secure Coding Techniques in Modern Web Frameworks

As modern web frameworks like React, Angular, and Vue become central to application development, understanding their built-in security features and secure coding practices is essential for preventing vulnerabilities like XSS, CSRF, and injection attacks. Here, we explore secure coding techniques in each framework and provide code examples.

### React

React is a popular front-end library that promotes component-based architecture. While React mitigates many common security issues, developers still need to adopt secure practices, especially when handling dynamic content.

- **Using `dangerouslySetInnerHTML` with Caution**: React warns developers about `dangerouslySetInnerHTML` because it can introduce XSS vulnerabilities by directly injecting HTML. Avoid using this method unless absolutely necessary, and sanitize inputs carefully.
- **Preventing XSS in React Components**: By default, React escapes values embedded in JSX, which prevents XSS. However, developers must sanitize inputs when working with untrusted content and avoid rendering HTML directly without thorough validation.

#### Code Snippet: Input Sanitization and Safe Rendering in React

To safely render user inputs in React, you can use libraries like `DOMPurify` for sanitization:

```javascript
import DOMPurify from "dompurify";

function SafeContent({ userInput }) {
  const sanitizedContent = DOMPurify.sanitize(userInput);

  return <div dangerouslySetInnerHTML={{ __html: sanitizedContent }} />;
}
```

Here, `DOMPurify.sanitize` ensures that any potentially harmful HTML or scripts in `userInput` are removed before rendering. This approach is useful when rendering dynamic HTML content while maintaining security.

### Angular

Angular is a powerful framework with a strong emphasis on security, offering built-in sanitization and features to help prevent common vulnerabilities like XSS and injection attacks.

- **Built-in Sanitization for Templates**: Angular automatically sanitizes any data bound to the DOM via templates. For example, Angular sanitizes HTML, URL, style, and resource URLs in templates, providing an extra layer of protection.
- **Using `HttpClient` and `DomSanitizer`**: Angular’s `HttpClient` is the recommended module for handling HTTP requests securely. Additionally, `DomSanitizer` allows you to safely bypass sanitization when absolutely necessary, though it should be used cautiously.

#### Code Snippet: Safe HTTP Request Handling and Sanitization in Angular

```typescript
import { HttpClient } from "@angular/common/http";
import { DomSanitizer, SafeHtml } from "@angular/platform-browser";

export class SafeComponent {
  safeContent: SafeHtml;

  constructor(private http: HttpClient, private sanitizer: DomSanitizer) {}

  fetchData(url: string) {
    this.http.get(url, { responseType: "text" }).subscribe((data) => {
      this.safeContent = this.sanitizer.bypassSecurityTrustHtml(data);
    });
  }
}
```

In this example, `HttpClient` is used to make HTTP requests securely, and `DomSanitizer` is only applied to HTML content when strictly necessary. This approach allows Angular to handle most content safely, while developers should ensure they aren’t bypassing sanitization for untrusted content.

### Vue

Vue.js provides a reactive framework that allows for a modular and data-driven approach to building UIs. With powerful templating and binding features, Vue also requires secure coding practices to prevent issues like XSS.

- **Safe Handling of `v-html`**: The `v-html` directive can be used to bind raw HTML. However, it’s advisable to avoid `v-html` unless absolutely required, as it can introduce XSS vulnerabilities if improperly sanitized.
- **Using `v-bind` and `v-on` Modifiers**: Vue offers `v-bind` for safe rendering and `v-on` event modifiers to handle user interactions securely. Avoid using untrusted data directly with these directives.

#### Code Snippet: Securing Dynamic Content and Events in Vue

```javascript
import DOMPurify from "dompurify";

export default {
  props: ["userInput"],
  computed: {
    sanitizedContent() {
      return DOMPurify.sanitize(this.userInput);
    },
  },
  template: `
    <div v-html="sanitizedContent"></div>
  `,
};
```

Here, `DOMPurify.sanitize` removes potentially harmful content from `userInput` before rendering it with `v-html`. This prevents XSS attacks while allowing you to safely use raw HTML.

For events, use the `v-on` directive to bind event handlers securely:

```html
<button v-on:click.prevent="submitForm">Submit</button>
```

The `.prevent` modifier on `v-on` ensures the default action is prevented, which adds a layer of control over event handling.

Each of these frameworks includes built-in features to enhance security, but it’s essential to understand and implement secure coding practices proactively. By combining these built-in mechanisms with additional security measures, developers can better protect web applications from vulnerabilities.
