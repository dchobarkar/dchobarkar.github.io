# Building Secure Apps - 05: Authentication and Authorization

## Introduction

### Overview of Authentication and Authorization in Web Security

In the realm of web security, **authentication** and **authorization** are two fundamental pillars that determine how securely users interact with applications. **Authentication** is the process of verifying a user’s identity, ensuring they are who they claim to be. **Authorization**, on the other hand, is the system’s method of granting or restricting a user’s access to resources or actions within the application based on their role or permissions.

For example, in a banking application, authentication validates a user’s credentials (such as username and password) to log them into the system. Once logged in, authorization restricts or grants access to actions—such as viewing balance or transferring funds—based on the user’s role (e.g., regular user, admin).

When implemented securely, authentication and authorization mechanisms provide robust protection for sensitive user data, reduce the risk of unauthorized access, and ensure that users can only perform actions they are permitted to. However, improper implementation can open doors to various security risks, including unauthorized data access, session hijacking, and privilege escalation attacks.

### Importance of Secure Authentication and Authorization for Protecting User Data and Enforcing Access Control

As web applications continue to evolve, they become increasingly attractive targets for cyberattacks due to the vast amounts of sensitive data they handle—personal information, financial data, health records, and more. Implementing secure authentication and authorization mechanisms is essential for:

1. **Protecting User Data**: With a secure authentication process, applications ensure that only legitimate users gain access, minimizing the likelihood of unauthorized individuals accessing sensitive data.
2. **Enforcing Access Control**: By implementing robust authorization protocols, applications can enforce strict control over which resources and functionalities are available to different users, reducing the risk of data breaches and malicious actions.
3. **Maintaining Trust and Compliance**: Secure authentication and authorization practices not only build trust among users but are also vital for regulatory compliance (e.g., GDPR, HIPAA), which mandates stringent data protection measures.

## Implementing Secure Authentication Mechanisms

### OAuth

OAuth (Open Authorization) is a widely adopted framework for **delegated authorization**. It allows applications to access resources on behalf of a user without needing to directly access their credentials. Rather than sharing passwords, OAuth enables users to authorize third-party applications to access specific resources or perform actions on their behalf. This is commonly used for accessing APIs or allowing login via third-party providers, such as Google or Facebook.

#### OAuth Grant Types

OAuth 2.0 defines several **grant types**, each serving different use cases:

- **Authorization Code Grant**: The most secure grant type, commonly used in web applications. This flow involves redirecting users to the authorization server, where they log in and grant access. The server then issues an authorization code, which the application exchanges for an access token.
- **Implicit Grant**: Typically used in single-page applications (SPAs). The application receives the access token directly without an authorization code, making it faster but less secure than the authorization code grant.
- **Client Credentials Grant**: Used for server-to-server authentication. In this flow, the application directly requests an access token from the authorization server using its client credentials (ID and secret) without a user context.
- **Resource Owner Password Credentials Grant**: Suitable for trusted applications, this flow allows the application to obtain an access token by directly exchanging the user’s credentials. Due to security risks, this grant type is less commonly used.

#### Code Snippet: Implementing OAuth 2.0 Authorization Code Grant Flow

Here’s a simple example of implementing the Authorization Code Grant Flow in an Express application using `passport` and `passport-oauth2`.

```javascript
const express = require("express");
const passport = require("passport");
const OAuth2Strategy = require("passport-oauth2");

passport.use(
  new OAuth2Strategy(
    {
      authorizationURL: "https://authorization-server.com/oauth/authorize",
      tokenURL: "https://authorization-server.com/oauth/token",
      clientID: "YOUR_CLIENT_ID",
      clientSecret: "YOUR_CLIENT_SECRET",
      callbackURL: "http://localhost:3000/auth/callback",
    },
    (accessToken, refreshToken, profile, done) => {
      // Use accessToken to fetch user data
      return done(null, profile);
    }
  )
);

const app = express();

app.get("/auth", passport.authenticate("oauth2"));

app.get(
  "/auth/callback",
  passport.authenticate("oauth2", { failureRedirect: "/" }),
  (req, res) => {
    // Successful authentication
    res.redirect("/dashboard");
  }
);
```

In this example, users are redirected to the OAuth provider’s authorization page. After authenticating, they are redirected back to the application’s callback route with an access token.

### OpenID Connect (OIDC)

OpenID Connect (OIDC) is an identity layer built on top of OAuth 2.0. While OAuth allows applications to access resources on behalf of users, OIDC focuses on **user authentication** by adding an identity token (ID token). OIDC enables applications to verify a user's identity and obtain their profile information securely.

#### Benefits of OIDC for Secure Authentication

- **Identity Verification**: OIDC provides an identity token that contains information about the user, enabling applications to verify the user’s identity securely.
- **Interoperability**: OIDC is widely supported by major providers (Google, Microsoft, Auth0), making it easy to implement third-party logins.
- **Access to User Information**: OIDC allows applications to retrieve user profile data, which can be used to personalize the user experience.

#### Code Snippet: Implementing OIDC with Google

Below is an example of using OIDC with Google in an Express application:

```javascript
const passport = require("passport");
const GoogleStrategy = require("passport-google-oauth20").Strategy;

passport.use(
  new GoogleStrategy(
    {
      clientID: "YOUR_GOOGLE_CLIENT_ID",
      clientSecret: "YOUR_GOOGLE_CLIENT_SECRET",
      callbackURL: "http://localhost:3000/auth/google/callback",
    },
    (token, tokenSecret, profile, done) => {
      // Use profile information
      return done(null, profile);
    }
  )
);

app.get(
  "/auth/google",
  passport.authenticate("google", { scope: ["openid", "profile", "email"] })
);

app.get(
  "/auth/google/callback",
  passport.authenticate("google", { failureRedirect: "/" }),
  (req, res) => {
    res.redirect("/dashboard");
  }
);
```

In this example, users are directed to Google’s OIDC login page. Upon successful login, they are redirected back with an identity token that contains their profile data.

### JWT (JSON Web Token)

JWT (JSON Web Token) is a compact, URL-safe way to represent **authentication data** between parties. JWTs are commonly used in **stateless authentication**, where the server verifies the token’s authenticity without needing to store session data. A JWT consists of three parts: the header, payload, and signature.

#### Structure of a JWT

1. **Header**: Contains metadata about the token, such as the signing algorithm (e.g., HS256).
2. **Payload**: Contains the token’s claims, including user-specific data (e.g., user ID) and expiration time.
3. **Signature**: A cryptographic signature that verifies the token’s integrity.

JWT enables secure, stateless authentication as the token itself contains all necessary information. This can reduce server load and improve scalability.

#### Code Snippet: Generating and Verifying a JWT in Node.js

Here’s an example of generating and verifying JWTs using the `jsonwebtoken` library in Node.js:

```javascript
const jwt = require("jsonwebtoken");

// Secret key for signing
const SECRET_KEY = "your_secret_key";

// Generating a JWT
const payload = { userId: 123, username: "user123" };
const token = jwt.sign(payload, SECRET_KEY, { expiresIn: "1h" });
console.log("Generated JWT:", token);

// Verifying a JWT
try {
  const decoded = jwt.verify(token, SECRET_KEY);
  console.log("Verified Payload:", decoded);
} catch (err) {
  console.error("Invalid token:", err.message);
}
```

In this example:

- A JWT is generated with user-specific data (`userId` and `username`) and a 1-hour expiration.
- The server verifies the token upon receipt to ensure its integrity and authenticity. If valid, the decoded payload can be used to identify the user; otherwise, an error is returned.

## Best Practices for Managing User Sessions Securely

### Session Management

Session management is a critical component of web application security, especially when handling **authenticated sessions**. After a user logs in, a session is established, allowing the application to remember the user’s identity across multiple requests. Secure session management ensures that sensitive session data remains protected from unauthorized access.

#### Session Storage Options

When managing sessions, storage options vary depending on the application’s needs:

- **In-Memory Storage**: Stores sessions in the server's memory. This is fast but limited to single-server applications. If the server restarts, session data is lost. Therefore, it’s mainly suitable for development or testing environments.
- **Database Storage**: Stores session data in a database (e.g., MySQL, MongoDB). This approach is persistent and suitable for distributed applications since session data persists across multiple servers. However, it adds some latency due to database access.
- **Cookie Storage**: Stores session data directly in cookies, which are sent to the client and returned with each request. Care must be taken to secure cookies with attributes like `HttpOnly`, `Secure`, and `SameSite` to prevent attacks like XSS and CSRF. This option is common for stateless sessions, where only a session token (JWT) is stored.

### Securing Cookies

Cookies are commonly used to store session tokens and other user-related data, making their security critical. Cookies should be configured with specific attributes to enhance security:

- **HttpOnly**: Prevents JavaScript access to cookies, reducing the risk of XSS attacks.
- **Secure**: Ensures cookies are only sent over HTTPS, protecting them from being intercepted in transit.
- **SameSite**: Controls whether cookies are sent with cross-site requests, helping prevent CSRF attacks. The `SameSite` attribute can be set to:
  - `Strict`: Blocks cookies from being sent with cross-site requests.
  - `Lax`: Allows cookies to be sent with safe requests (e.g., GET).
  - `None`: Cookies are sent with all requests but must be marked as `Secure`.

#### Code Snippet: Setting Secure Cookies in Express.js

Here’s an example of configuring secure cookies in an Express.js application:

```javascript
const express = require("express");
const session = require("express-session");

const app = express();

app.use(
  session({
    secret: "your_secret_key",
    resave: false,
    saveUninitialized: true,
    cookie: {
      httpOnly: true, // Prevents JavaScript access
      secure: process.env.NODE_ENV === "production", // Enables Secure in production
      sameSite: "Strict", // Prevents cross-site cookie sending
      maxAge: 3600000, // Sets cookie expiration time (e.g., 1 hour)
    },
  })
);

app.get("/", (req, res) => {
  req.session.user = { id: "user123" }; // Sample session data
  res.send("Session initialized");
});

app.listen(3000, () => console.log("Server running on http://localhost:3000"));
```

In this example:

- **HttpOnly** and **Secure** attributes enhance cookie security.
- **SameSite** is set to `Strict` to prevent cross-site attacks.
- **maxAge** controls the session duration by setting the cookie expiration time.

### Session Expiry and Renewal

Setting a reasonable expiration time for sessions is crucial for minimizing security risks associated with abandoned sessions. Here are some strategies to manage session expiry and renewal:

1. **Session Expiration**: Automatically logs users out after a predefined period of inactivity or a maximum session lifetime. This approach protects against unauthorized access if a session is left open.
2. **Sliding Sessions**: Resets the session expiration timer each time the user interacts with the application. This provides a more user-friendly experience, as active users won’t be logged out due to inactivity.
3. **Refresh Tokens**: For long-lived sessions, refresh tokens enable secure session renewal without constantly re-authenticating. The application issues a short-lived access token and a longer-lived refresh token. When the access token expires, the client can use the refresh token to request a new one, without re-entering credentials.

#### Code Snippet: Implementing Session Expiration and Sliding Sessions in Express.js

Here’s an example of setting up session expiration and sliding sessions in an Express.js application:

```javascript
const express = require("express");
const session = require("express-session");

const app = express();

// Session configuration
app.use(
  session({
    secret: "your_secret_key",
    resave: false,
    saveUninitialized: false,
    cookie: {
      httpOnly: true,
      secure: process.env.NODE_ENV === "production",
      sameSite: "Strict",
      maxAge: 15 * 60 * 1000, // Session expires in 15 minutes
    },
  })
);

// Middleware to implement sliding sessions
app.use((req, res, next) => {
  if (req.session) {
    req.session.cookie.expires = new Date(Date.now() + 15 * 60 * 1000); // Reset session expiry
  }
  next();
});

app.get("/login", (req, res) => {
  req.session.user = { id: "user123" }; // Sample login simulation
  res.send("Logged in");
});

app.get("/logout", (req, res) => {
  req.session.destroy(); // Clears session data
  res.send("Logged out");
});

app.listen(3000, () => console.log("Server running on http://localhost:3000"));
```

In this setup:

- **Session Expiry**: The session expires after 15 minutes of inactivity, enforcing a secure session timeout.
- **Sliding Session**: The middleware resets the session expiration timer with each request, so active users remain logged in.

## Role-Based Access Control (RBAC) and Least Privilege Enforcement

### Role-Based Access Control (RBAC)

Role-Based Access Control (RBAC) is an authorization mechanism used to control access to resources based on predefined user roles and permissions. RBAC helps ensure that only authorized users can access specific functionalities within an application, reducing the risk of unauthorized actions and data exposure.

#### Importance of RBAC in Secure Authorization

RBAC simplifies access management by assigning users to specific roles, which each have associated permissions. For example:

- **Admin**: Full access to all resources, including user management and configuration.
- **Editor**: Access to create, edit, and delete content but restricted from administrative functions.
- **Viewer**: Read-only access to resources.

By assigning roles, RBAC restricts users to only the permissions required to fulfill their responsibilities, limiting potential misuse or accidental changes.

#### Defining Roles and Permissions in a Web Application

To implement RBAC, you need to define:

1. **Roles**: Categories of users (e.g., Admin, Editor, Viewer).
2. **Permissions**: Specific actions that can be performed within the application (e.g., read, write, delete).
3. **Role-Permission Mapping**: Assign permissions to each role.
4. **User-Role Mapping**: Assign users to roles based on their responsibilities.

Let’s look at an example of implementing RBAC using **Express.js** and **Node.js**.

#### Code Snippet: Implementing RBAC in Express.js

In this example, we’ll create a middleware function that checks a user’s role and enforces access control based on predefined permissions.

```javascript
const express = require("express");
const app = express();

// Define roles and their permissions
const roles = {
  admin: ["read", "write", "delete"],
  editor: ["read", "write"],
  viewer: ["read"],
};

// Middleware to enforce RBAC
const checkPermission = (role, action) => (req, res, next) => {
  if (roles[role] && roles[role].includes(action)) {
    next(); // Permission granted
  } else {
    res.status(403).json({ message: "Access denied" }); // Permission denied
  }
};

// Sample routes with role-based access control
app.get("/view", checkPermission("viewer", "read"), (req, res) => {
  res.send("Viewing content");
});

app.post("/edit", checkPermission("editor", "write"), (req, res) => {
  res.send("Editing content");
});

app.delete("/delete", checkPermission("admin", "delete"), (req, res) => {
  res.send("Deleting content");
});

app.listen(3000, () => console.log("Server running on http://localhost:3000"));
```

In this example:

- The `checkPermission` middleware takes a role and action as arguments, then checks if the role has the necessary permissions.
- Specific routes are protected based on role and action requirements, ensuring users only access features permitted by their role.

### Least Privilege Principle

The **Least Privilege Principle** is a security concept that limits users' access rights to only what is necessary to complete their tasks. By enforcing least privilege, organizations can reduce the risk of accidental or malicious actions that could compromise the application or data.

#### Overview of the Least Privilege Concept

Implementing the least privilege principle means:

1. **Granting Minimal Access**: Only allow users access to the resources and actions necessary for their role.
2. **Limiting High-Privilege Accounts**: Restrict access to critical resources to a minimal set of privileged users.
3. **Restricting Access to Sensitive Data**: Limit who can view or edit sensitive information (e.g., financial data or personal information).
4. **Regularly Reviewing Permissions**: Periodically review user access levels to ensure they align with users' current roles and responsibilities.

#### Best Practices for Enforcing Least Privilege

- **Minimal Role Assignment**: When assigning roles, start with the lowest level of permissions and gradually increase if necessary.
- **Periodic Access Reviews**: Conduct regular audits to verify that users have appropriate permissions, adjusting roles if needed.
- **Use Temporary Privileges**: For critical operations, consider assigning temporary elevated access that expires after the task is completed.
- **Restrict High-Impact Permissions**: Limit actions like delete, write, or administrative access to specific trusted roles, minimizing the potential for critical changes.

By combining RBAC with the least privilege principle, you create a robust security framework that not only controls who can access resources but also restricts access to only what’s essential.
