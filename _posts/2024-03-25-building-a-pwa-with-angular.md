# Mastering PWAs - 10: Building a PWA with Angular

## Introduction to PWAs with Angular

Progressive Web Apps (PWAs) are revolutionizing how we build and interact with web applications. They blend the best features of native mobile apps with the reach and accessibility of web technologies, creating a more immersive, reliable, and performant user experience. Angular, a powerful web framework, offers built-in tools and a scalable architecture, making it an ideal choice for developing PWAs. In this section, we’ll dive into what PWAs are, why Angular is well-suited for building them, showcase successful Angular-based PWAs, and explore Angular’s key features for PWA development.

### Overview of Progressive Web Apps (PWAs)

#### Definition and Key Features of PWAs

A **Progressive Web App (PWA)** is a type of web application designed to deliver a native app-like experience on the web. PWAs are built with modern web technologies, ensuring fast, reliable, and engaging interactions, even in uncertain network conditions.

Key features of PWAs include:

1. **Offline Support**:

   - PWAs utilize **Service Workers** to cache important resources, enabling offline functionality. This means that users can continue interacting with the app even without an active internet connection. Service Workers handle tasks such as caching and intercepting network requests, ensuring the app runs smoothly offline.

2. **Responsiveness**:

   - PWAs are designed to be responsive, meaning they adapt seamlessly to any device, whether it's a desktop, tablet, or mobile phone. The goal is to deliver a consistent and optimized user experience across all screen sizes and orientations.

3. **Push Notifications**:

   - One of the standout features of PWAs is the ability to send push notifications, even when the app isn’t open. This allows businesses to re-engage users with timely updates and offers, similar to how native mobile apps send notifications.

4. **App-Like Behavior**:

   - PWAs mimic the look and feel of native apps, thanks to their use of full-screen mode, smooth animations, and navigation. They can be installed on a user’s home screen without the need for an app store, providing a fast, immersive experience.

5. **Secure by Default**:

   - PWAs are served over HTTPS to ensure secure communication between the user and the server, protecting sensitive data and preventing attacks like man-in-the-middle.

#### Importance of PWAs in Modern Web Development

PWAs have become increasingly important in modern web development because they combine the best features of web and mobile apps, offering enhanced functionality and engagement.

- **User Engagement**: PWAs provide a more engaging experience through offline access, fast load times, and re-engagement tools like push notifications. This leads to higher retention rates and better conversion rates compared to traditional web apps.

- **Reach and Accessibility**: Unlike native apps, PWAs are accessible through any modern web browser without the need for installation from app stores. This reduces friction and allows businesses to reach a broader audience across platforms.

- **Performance and Reliability**: With Service Workers caching assets and handling offline requests, PWAs load faster, even on slow or unreliable networks, providing a reliable experience for users.

### Why Use Angular for PWA Development?

#### Benefits of Angular’s Framework for PWA Development

Angular is one of the most powerful and widely-used frameworks for building robust web applications, and it’s particularly well-suited for Progressive Web App development. Some of the benefits of using Angular for PWAs include:

1. **Modularity**:

   - Angular’s modular architecture allows developers to break down an application into smaller, reusable components. This is especially useful for PWAs, as it makes the app easier to scale and maintain. Angular’s use of modules simplifies the organization of code, making large-scale PWA projects more manageable.

2. **TypeScript Support**:

   - Angular is built with **TypeScript**, a superset of JavaScript that adds static typing. This allows developers to catch potential bugs during development and enhances productivity with features like auto-completion and type-checking. The use of TypeScript in Angular PWAs leads to more maintainable and reliable code.

3. **Built-in Tools for PWAs**:

   - Angular has **built-in support** for PWAs through its Angular CLI. With just a single command, developers can add PWA capabilities (such as Service Workers and Web App Manifest) to their Angular applications, making the setup process simple and efficient.

   **Command to add PWA support:**

   ```bash
   ng add @angular/pwa
   ```

4. **Scalability**:

   - Angular is highly scalable, making it ideal for developing large-scale PWAs with complex features. It supports lazy loading and code splitting, which ensures that only the necessary parts of the application are loaded on demand, reducing initial load times and improving performance.

#### Comparison of Angular PWAs vs. PWAs Built with Other Frameworks

Angular stands out in comparison to other popular frameworks such as **React** and **Vue** due to its structured, opinionated nature, and rich set of built-in features:

- **React**: While React offers more flexibility and is lightweight, it requires third-party libraries and tools to manage things like routing and state management. Angular, on the other hand, provides a full suite of tools out of the box, including routing, forms, and HTTP handling, making it a more complete solution for building PWAs.

- **Vue**: Vue is similar to Angular in terms of ease of use but has a smaller community and ecosystem compared to Angular. Angular’s TypeScript integration and large community make it a more robust option for enterprise-level applications, including PWAs.

### Examples of Successful Angular PWAs

Several large companies have adopted Angular to build their PWAs, leveraging its capabilities to enhance performance and engagement.

1. **Microsoft Office**:

   - Microsoft’s PWA for **Office** leverages Angular to deliver a fast and responsive experience across devices. Users can access Office tools like Word, Excel, and PowerPoint directly from their browser with offline access, push notifications, and app-like behavior.

2. **Forbes**:

   - The Forbes PWA, built with Angular, focuses on speed and performance, particularly in environments with slow internet connections. Forbes achieved a **43% increase in sessions per user** and a **20% increase in impressions per page** by leveraging Angular’s powerful PWA features.

3. **BMW**:

   - BMW’s PWA, developed with Angular, allows users to explore car models, book test drives, and stay connected with the brand. The PWA is fast, mobile-first, and offers a rich user experience with offline access and push notifications.

#### Advantages Brought by Angular PWAs:

- Enhanced performance and faster loading times, especially in regions with slow network speeds.

- Higher user engagement through offline access and push notifications, improving conversion rates and session times.

- Seamless cross-device experience, ensuring that users have the same interaction whether on mobile, tablet, or desktop.

### Key Angular Features for PWA Development

#### Angular CLI’s Built-in PWA Support

The Angular CLI makes it incredibly easy to add PWA capabilities to an existing Angular project. By running the command:

```bash
ng add @angular/pwa
```

The CLI automatically configures key files, such as the **Web App Manifest** and **Service Worker**. It also updates the **`index.html`** file with meta tags necessary for the PWA.

#### How Angular’s Modular Structure Helps in Managing Large-Scale PWA Projects

Angular’s modular structure helps in organizing code by breaking it into **modules**, making it easy to manage and scale large applications. Each module in Angular can contain components, services, and directives, all of which can be independently developed and tested.

- **Lazy Loading**: Angular allows developers to load modules only when they are needed, which reduces the initial load time and improves performance. This is crucial for PWAs, where fast load times are essential for user engagement.

- **Separation of Concerns**: Angular enforces a clear separation of concerns, ensuring that each module handles a specific responsibility within the app. This makes the app easier to maintain and extend, which is particularly important for large-scale PWAs.

## Setting Up an Angular PWA Project

Building a Progressive Web App (PWA) with Angular is straightforward thanks to Angular’s powerful CLI, which provides built-in support for PWA features. In this section, we’ll cover the prerequisites for setting up an Angular PWA, walk through the process of creating a new Angular PWA project, explain key files and directories, and guide you through customizing PWA features such as the Web App Manifest and Service Workers.

### Prerequisites

Before creating an Angular PWA project, you need to ensure that you have the right tools installed and a basic understanding of Angular fundamentals.

#### Tools Needed:

1. **Node.js**: Angular requires Node.js for development. You can download it from [nodejs.org](https://nodejs.org).

   - Verify the installation by running the following command:

     ```bash
     node -v
     ```

2. **Angular CLI**: The Angular CLI is the command-line tool for creating and managing Angular projects. It provides built-in support for PWA features such as Service Workers and the Web App Manifest.

   - To install Angular CLI globally, use the following command:

     ```bash
     npm install -g @angular/cli
     ```

3. **Basic Knowledge of Angular**: It’s essential to have an understanding of Angular fundamentals such as components, services, and the structure of Angular projects. Familiarity with TypeScript is also beneficial, as Angular uses TypeScript for building applications.

#### Installing Angular CLI and Creating a New Project with PWA Support

With Node.js and Angular CLI installed, you can now create a new Angular project that includes PWA functionality.

1. **Create a New Angular Project**:

   Use the following command to create a new Angular project. The `--service-worker` flag ensures that the project is set up with PWA features from the start.

   ```bash
   ng new my-angular-pwa --service-worker
   ```

   The Angular CLI will prompt you for some basic information, such as project name, style sheet format (CSS, SCSS, etc.), and whether to enable routing. Choose the options that fit your project.

2. **Navigating to the Project**:

   After the project is created, navigate to the project folder:

   ```bash
   cd my-angular-pwa
   ```

### Step-by-Step Guide to Setting Up an Angular PWA

#### Creating a New Angular PWA Project

When you run the `ng new` command with the `--service-worker` flag, Angular sets up the project with default PWA features. Let’s break down the project structure and explore the key files relevant to PWAs.

#### Project Structure:

Here’s an overview of the key directories and files that you’ll be working with:

```plaintext
my-angular-pwa/
├── src/
│   ├── app/
│   ├── assets/
│   ├── environments/
│   ├── index.html
│   ├── manifest.webmanifest
│   ├── ngsw-config.json
│   └── ...
├── angular.json
├── package.json
└── ...
```

- **`manifest.webmanifest`**: This file defines the metadata for your PWA, such as the app name, icons, and start URL. This file is crucial for making your app installable.

- **`ngsw-config.json`**: This file contains the configuration for the Angular Service Worker, defining what assets should be cached and how requests should be handled when the app is offline.

- **`index.html`**: This is the main HTML file where the app’s metadata and links to the Web App Manifest are defined.

#### Configuring Angular PWA Features

Angular’s built-in PWA support includes a Web App Manifest and a Service Worker configuration file. These need to be customized to fit the needs of your project.

#### Setting up the Web App Manifest and Customizing It

The **Web App Manifest** is responsible for defining how your PWA behaves when installed on a user’s device. The default `manifest.webmanifest` file is generated automatically, but you can customize it to change how your app appears on users' devices.

Here’s the default content of the `manifest.webmanifest` file:

```json
{
  "name": "My Angular PWA",
  "short_name": "AngularPWA",
  "theme_color": "#1976d2",
  "background_color": "#fafafa",
  "display": "standalone",
  "scope": "/",
  "start_url": "/",
  "icons": [
    {
      "src": "assets/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "assets/icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

- **`name`**: The full name of your PWA, which appears during installation.

- **`short_name`**: A shorter version of the app name, typically shown on the home screen.

- **`theme_color`**: The color of the browser UI when the app is launched.

- **`background_color`**: The background color displayed during the app’s launch.

- **`display`**: Specifies whether the app should open in a browser (`browser`) or in standalone mode (`standalone`).

- **`icons`**: Defines the app icons for different screen sizes.

You can customize these fields to match your app’s branding. For example, if you want to update the theme color and icons:

```json
{
  "name": "My Custom PWA",
  "short_name": "CustomPWA",
  "theme_color": "#ff5722",
  "background_color": "#ffffff",
  "display": "standalone",
  "icons": [
    {
      "src": "assets/icons/custom-icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "assets/icons/custom-icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

#### Configuring Service Workers in the `ngsw-config.json` File

The **`ngsw-config.json`** file controls how the Angular Service Worker behaves. By default, it is configured to cache the application shell and other important assets, but you can customize it to cache additional resources or define more complex caching strategies.

Here’s a basic configuration of the `ngsw-config.json` file:

```json
{
  "index": "/index.html",
  "assetGroups": [
    {
      "name": "app",
      "installMode": "prefetch",
      "resources": {
        "files": [
          "/favicon.ico",
          "/index.html",
          "/manifest.webmanifest",
          "/*.css",
          "/*.js"
        ]
      }
    },
    {
      "name": "assets",
      "installMode": "lazy",
      "updateMode": "prefetch",
      "resources": {
        "files": ["/assets/**"]
      }
    }
  ]
}
```

- **`installMode`**: Determines how resources are cached. The `prefetch` mode caches resources during the Service Worker installation, while `lazy` caches resources only when they are requested.

- **`resources`**: Specifies which files should be cached. You can define specific files or use wildcards like `/*.css` to cache all CSS files.

#### Code Snippet: Customizing the Service Worker Configuration

If you want to cache additional resources, such as external API requests, you can add a new asset group in the `ngsw-config.json` file:

```json
{
  "index": "/index.html",
  "assetGroups": [
    {
      "name": "external-api",
      "installMode": "lazy",
      "updateMode": "prefetch",
      "resources": {
        "urls": ["https://api.example.com/**"]
      }
    }
  ]
}
```

This configuration caches responses from `https://api.example.com` when they are requested for the first time.

### Running and Testing the PWA in Development Mode

After configuring your Web App Manifest and Service Worker, you can run and test the PWA locally.

#### Running the App Locally

To run the Angular app locally and test its PWA features, use the following command:

```bash
ng serve
```

By default, the Service Worker is disabled in development mode to prevent caching issues during development. To test the PWA features such as offline access, you need to build the app for production.

#### Testing PWA Features (Offline Access, Caching)

1. **Build for Production**:

   Run the following command to create a production build where the Service Worker is enabled:

   ```bash
   ng build --prod
   ```

2. **Serve the Production Build**:

   To test the app in a production-like environment, you can use a local server such as **http-server**:

   ```bash
   npm install -g http-server
   http-server ./dist/my-angular-pwa
   ```

3. **Testing Offline Access**:

   - Open the app in Chrome and go to **DevTools**.

   - In the **Application** tab, check the **Service Workers** section to ensure the Service Worker is active.

   - Turn off your internet connection and refresh the page. If the Service Worker is functioning correctly, the app should still load, even without an internet connection.

## Service Workers and Manifest in Angular

Service Workers and the Web App Manifest are critical components of a Progressive Web App (PWA). Service Workers enable offline functionality, caching strategies, and background sync, while the Web App Manifest provides metadata that allows the PWA to be installable and behave like a native app. In this section, we will explore how Service Workers function in Angular, how to manage them, caching strategies, handling updates, and customizing the Web App Manifest.

### What are Service Workers?

#### Role of Service Workers in PWAs

A **Service Worker** is a JavaScript file that runs in the background, separate from the main application. It allows PWAs to offer features such as:

1. **Caching**: Service Workers intercept network requests and can cache resources like HTML, CSS, JavaScript, and images. This improves load times and enables offline access.

2. **Offline Functionality**: By caching assets, Service Workers ensure that users can continue interacting with the app even when they are offline or on a slow network.

3. **Background Sync**: Service Workers can synchronize data in the background, such as sending updates to a server when the network
   connection is restored.

4. **Push Notifications**: Service Workers can handle push notifications, allowing apps to engage users even when the app is not open.

#### Life Cycle of a Service Worker in Angular

The Service Worker life cycle includes three key phases:

1. **Install**: The Service Worker is installed and caches the necessary resources. This is where you define which assets should be cached.

   ```javascript
   self.addEventListener("install", (event) => {
     event.waitUntil(
       caches.open("my-cache").then((cache) => {
         return cache.addAll(["/index.html", "/styles.css", "/script.js"]);
       })
     );
   });
   ```

2. **Activate**: The Service Worker is activated after installation. This phase is often used to clean up old caches from previous versions of the app.

   ```javascript
   self.addEventListener("activate", (event) => {
     const cacheWhitelist = ["my-cache"];
     event.waitUntil(
       caches.keys().then((cacheNames) => {
         return Promise.all(
           cacheNames.map((cacheName) => {
             if (!cacheWhitelist.includes(cacheName)) {
               return caches.delete(cacheName);
             }
           })
         );
       })
     );
   });
   ```

3. **Fetch**: The Service Worker intercepts network requests and serves cached responses, falling back to the network if necessary.

   ```javascript
   self.addEventListener("fetch", (event) => {
     event.respondWith(
       caches.match(event.request).then((response) => {
         return response || fetch(event.request);
       })
     );
   });
   ```

### Managing Service Workers in Angular

#### Angular’s Built-in Service Worker Support

Angular provides built-in support for Service Workers, making it easy to integrate them into your application. The Angular CLI includes PWA capabilities, and the default Service Worker is configured through the `ngsw-config.json` file.

By default, Angular’s Service Worker is configured to:

- Cache the application shell (HTML, CSS, JavaScript).
- Provide offline access to cached content.
- Automatically update the cache when new content is available.

#### Customizing the Default Service Worker for Advanced Use Cases

While Angular’s default Service Worker configuration works well for most applications, you may need to customize it to cache additional resources, handle specific network requests, or manage more complex caching strategies.

Here’s how to modify the Service Worker in Angular:

1. **Open the `ngsw-config.json` File**:

   This file is located in the root of your project and defines the caching behavior of the Service Worker.

2. **Add Custom Caching Rules**:

   For example, if you want to cache API responses, you can define a new `dataGroups` section.

   **Code Snippet: Modifying the Service Worker to Cache API Responses**

   ```json
   {
     "index": "/index.html",
     "assetGroups": [
       {
         "name": "app",
         "installMode": "prefetch",
         "resources": {
           "files": ["/favicon.ico", "/index.html", "/styles.css", "/main.js"]
         }
       }
     ],
     "dataGroups": [
       {
         "name": "api-calls",
         "urls": ["https://api.example.com/**"],
         "cacheConfig": {
           "strategy": "freshness",
           "maxSize": 100,
           "maxAge": "1h"
         }
       }
     ]
   }
   ```

In this configuration:

- **`dataGroups`**: Defines how API calls are cached.
- **`urls`**: Specifies the API endpoints to be cached.
- **`strategy`**: Sets the caching strategy, in this case, **freshness**, which tries the network first and falls back to the cache if the network is unavailable.
- **`maxSize`**: Limits the number of cached responses.
- **`maxAge`**: Specifies how long cached data is valid (in this case, 1 hour).

### Caching Strategies for Angular PWAs

Caching strategies determine how your Service Worker handles network requests. Angular’s Service Worker supports multiple caching strategies, each suited for different use cases.

#### Overview of Caching Strategies

1. **Cache-First**:

   - The Service Worker checks the cache first and serves the cached response if available. If not, it fetches the resource from the network and caches it for future requests. This strategy is best for static assets like images or CSS files.

   **Code Snippet: Cache-First Strategy**

   ```json
   {
     "cacheConfig": {
       "strategy": "performance",
       "maxSize": 50,
       "maxAge": "7d"
     }
   }
   ```

2. **Network-First**:

   - The Service Worker fetches the resource from the network first and falls back to the cache if the network is unavailable. This strategy is ideal for dynamic content like API data.

   **Code Snippet: Network-First Strategy**

   ```json
   {
     "dataGroups": [
       {
         "name": "api-data",
         "urls": ["https://api.example.com/**"],
         "cacheConfig": {
           "strategy": "freshness",
           "maxSize": 100,
           "maxAge": "1h"
         }
       }
     ]
   }
   ```

3. **Stale-While-Revalidate**:

   - The Service Worker serves the cached response while simultaneously fetching the latest version from the network and updating the cache. This strategy ensures users see the fastest response while keeping the cache updated.

   **Code Snippet: Stale-While-Revalidate Strategy**

   ```json
   {
     "dataGroups": [
       {
         "name": "dynamic-content",
         "urls": ["/dynamic/**"],
         "cacheConfig": {
           "strategy": "performance",
           "maxSize": 20,
           "maxAge": "1d"
         }
       }
     ]
   }
   ```

### Handling Updates and Versioning in Angular PWAs

#### How Angular Handles Service Worker Updates

When a new version of your app is deployed, the Service Worker needs to update the cached assets. Angular handles Service Worker updates by installing the new version in the background. However, the new version does not take control immediately; it waits until all tabs of the old version are closed.

#### Prompting Users to Refresh When a New Version is Available

To notify users when a new version of the app is available, you can listen for update events and prompt the user to refresh.

**Code Snippet: Customizing the Update Prompt Logic**

```typescript
import { SwUpdate } from "@angular/service-worker";
import { MatSnackBar } from "@angular/material/snack-bar";
import { Component } from "@angular/core";

@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
})
export class AppComponent {
  constructor(updates: SwUpdate, private snackBar: MatSnackBar) {
    updates.available.subscribe((event) => {
      const snackBarRef = this.snackBar.open(
        "New version available",
        "Reload",
        { duration: 6000 }
      );
      snackBarRef.onAction().subscribe(() => {
        updates.activateUpdate().then(() => document.location.reload());
      });
    });
  }
}
```

In this example, a snackbar is shown to the user when a new version is available. If the user clicks "Reload", the new version is activated.

### Understanding the Web App Manifest in Angular

#### Role of the Web App Manifest in Making an Angular App Installable

The **Web App Manifest** is a JSON file that provides metadata about your app, such as its name, icons, and theme color. This file allows the app to be installed on a user’s home screen, making it look and feel like a native app.

#### Key Properties of the `manifest.json` File

1. **`name` and `short_name`**: Define how your app’s name appears during installation and on the home screen.
2. **`icons`**: Specifies the app’s icons for different screen sizes and resolutions.
3. **`start_url`**: Defines the URL to load when the app is launched from the home screen.
4. **`display`**: Controls how the app is displayed—`standalone` for full-screen or `browser` for opening in the browser window.
5. **`background_color` and `theme_color`**: Set the colors for the splash screen and browser interface.

**Code Snippet: Customizing the Web App Manifest**

```json
{
  "

name": "Angular PWA",
  "short_name": "PWA",
  "theme_color": "#1976d2",
  "background_color": "#ffffff",
  "display": "standalone",
  "start_url": "/",
  "icons": [
    {
      "src": "assets/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "assets/icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

Customizing the `manifest.json` file allows you to tailor how the app looks and behaves when installed on a user’s device.
