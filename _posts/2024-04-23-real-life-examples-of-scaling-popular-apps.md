# Building Scalable Systems - 08: Real-Life Examples of Scaling Popular Apps

## Introduction: The Crucial Role of Scalability in the Digital Era

In today’s hyper-connected world, scalability is no longer a luxury—it’s a necessity. As businesses strive to reach millions, sometimes billions, of users across the globe, the ability to handle increasing loads efficiently has become a cornerstone of success. Whether it’s a streaming service delivering movies to a global audience, a social network connecting people across continents, or a platform facilitating millions of bookings, scalability determines whether an application thrives or falters.

### Why Scalability Matters

Scalability ensures that systems can accommodate growth without a decline in performance. In the digital era, user expectations are sky-high. People demand seamless, real-time experiences, whether they’re streaming their favorite show, posting on social media, or booking a vacation rental. A scalable system not only handles increased traffic but also adapts to changing demands while maintaining reliability and speed.

For businesses, scalability isn’t just about performance—it’s a strategic advantage. It enables companies to expand their user base, launch in new markets, and handle seasonal spikes in traffic, such as Black Friday sales or holiday booking rushes, without missing a beat.

### Overcoming Scalability Challenges

Scaling a system to support millions or even billions of users isn’t easy. It requires addressing a myriad of challenges, including:

- Handling high traffic volumes without system crashes.
- Ensuring data consistency across distributed systems.
- Minimizing latency in geographically dispersed regions.
- Balancing cost with performance.

Industry giants like Netflix, Facebook, and Airbnb have faced these challenges head-on. Their journeys provide invaluable insights into how to build systems that not only scale but also deliver exceptional user experiences.

### Case Studies: Learning from the Best

This article dives deep into three real-life examples of scalability in action:

1. **Netflix**: A pioneer in global scalability, Netflix transitioned from a monolithic architecture to a microservices-based system, leveraging cloud infrastructure to deliver uninterrupted streaming to over 200 million subscribers worldwide.
2. **Facebook**: From a college project to a social media behemoth, Facebook revolutionized database scalability with innovative solutions like TAO and RocksDB to handle billions of users and interactions daily.
3. **Airbnb**: A disruptor in the hospitality industry, Airbnb optimized its platform using Kubernetes, caching, and dynamic pricing algorithms to handle peak loads during major travel seasons.

These case studies will explore the strategies, tools, and lessons that propelled these companies to scalability success. By examining their approaches, we aim to uncover actionable insights that can inspire and guide the design of your own scalable systems.

Scalability isn’t just a technical goal—it’s a key driver of innovation and growth. Let’s delve into how these industry leaders turned challenges into opportunities, building platforms that set the benchmark for resilience, reliability, and performance.

## Case Study: Netflix – Building a Globally Scalable System with Microservices

### Background: The Growth of Netflix and the Need for Scalability

Netflix began its journey as a DVD rental service in the late 1990s, but its pivot to streaming in 2007 changed the entertainment landscape forever. By 2010, as Netflix expanded globally, it was clear that its monolithic architecture could not keep pace with the explosive growth in users, devices, and streaming demands.

The monolithic system, while functional, faced severe limitations:

- **Scaling bottlenecks**: Scaling required duplicating the entire system, which was resource-intensive and inefficient.
- **Deployment issues**: Updating one part of the system risked affecting the entire application, leading to longer deployment cycles.
- **Resilience challenges**: A single failure point in the monolith could cause widespread outages.

To address these issues, Netflix embarked on a bold transformation: transitioning to a microservices architecture powered by cloud infrastructure.

### Transition to Microservices: The Netflix Approach

**Why Netflix Adopted Microservices**

Microservices offered Netflix the flexibility to scale individual components independently. Each microservice was responsible for a specific function, such as user authentication, recommendations, or streaming. This allowed teams to develop, deploy, and scale services autonomously.

**The Role of AWS**

Netflix’s partnership with AWS was pivotal. By leveraging AWS’s cloud infrastructure, Netflix gained:

- **Elasticity**: The ability to scale resources up or down based on demand.
- **Global reach**: AWS data centers enabled Netflix to deliver content seamlessly across continents.
- **Resilience**: Built-in redundancy reduced the risk of service outages.

### Key Scalability Strategies

**1. Auto-Scaling with AWS EC2**

To handle surges in traffic—such as during the release of a popular show—Netflix implemented auto-scaling policies with AWS EC2. Auto-scaling allowed Netflix to automatically spin up additional servers during peak times and scale down during off-peak hours, optimizing resource usage and cost.

**Code Example: Setting Up Auto-Scaling Policies with AWS**

Here’s a simplified example of configuring an auto-scaling policy using the AWS CLI:

```bash
aws autoscaling create-auto-scaling-group \
    --auto-scaling-group-name netflix-streaming-asg \
    --launch-configuration-name netflix-streaming-launch-config \
    --min-size 2 \
    --max-size 20 \
    --desired-capacity 5 \
    --availability-zones us-west-1a us-west-1b
```

This script creates an auto-scaling group for Netflix’s streaming service, ensuring sufficient capacity to handle fluctuating traffic.

**2. Using Cassandra for Globally Distributed Data Storage**

Netflix adopted Apache Cassandra, a NoSQL database known for its scalability and fault tolerance, to manage its massive data requirements. Cassandra’s distributed architecture enabled Netflix to:

- Store data redundantly across regions, ensuring low latency for users worldwide.
- Handle millions of reads and writes per second without compromising performance.

**3. Chaos Engineering for Fault Tolerance**

To ensure reliability, Netflix pioneered **Chaos Engineering**—a practice of intentionally introducing failures to test the system’s resilience. Netflix’s Chaos Monkey tool randomly shuts down instances in production to validate fault-tolerance mechanisms.

### Lessons from Netflix's Scalability Journey

Netflix’s shift to microservices and cloud infrastructure was not without challenges, including managing inter-service communication and debugging a highly distributed system. However, the benefits far outweighed the difficulties:

- **Resilience**: Downtime was minimized, even during failures.
- **Flexibility**: Teams could iterate faster with isolated microservices.
- **Performance**: The system handled global traffic with ease, even during high-demand events.

Netflix’s journey demonstrates how a strategic approach to microservices, combined with robust cloud infrastructure and innovative practices like Chaos Engineering, can create a system capable of serving millions—if not billions—of users.

## Case Study: Facebook – Scaling Database Infrastructure

### Background: Facebook's Growth and Database Challenges

In the early 2000s, Facebook began as a social network for college students, but its rapid growth soon turned it into a global phenomenon. By 2010, Facebook had hundreds of millions of users, each generating posts, likes, comments, and connections. This exponential growth exposed significant challenges in the scalability of their database infrastructure.

Initially, Facebook relied on **MySQL**, a relational database system that was reliable and well-suited for early-stage use. However, as traffic surged:

- **Bottlenecks emerged** in handling the vast amounts of social graph data (the relationships between users and their activities).
- **Scalability issues** with MySQL’s single-node architecture began to hinder performance.
- **Downtime risks** increased, as a centralized database became a single point of failure.

Recognizing these limitations, Facebook invested heavily in rethinking their database infrastructure.

### Innovative Solutions

**1. TAO: The Associations and Objects**

To efficiently manage their social graph (the intricate network of relationships between users and content), Facebook developed **TAO (The Associations and Objects)**, a data store specifically designed for the unique demands of their platform.

- **How TAO Works**:

  - It simplifies the retrieval of association data, such as "who liked this post" or "which friends attended this event."
  - TAO acts as a caching layer on top of MySQL, reducing the load on the primary database while maintaining low latency.

- **Benefits**:

  - Significant performance improvements for fetching relationship data.
  - Reduced the number of queries hitting the MySQL database.

**2. RocksDB: Optimizing Storage and Query Performance**

As Facebook's data needs grew, they required a solution to optimize their storage and query performance. Enter **RocksDB**, a high-performance, key-value store optimized for flash storage.

- RocksDB is a **log-structured merge-tree (LSM)** database designed to handle massive write and read operations efficiently.
- It enabled Facebook to power features like Messenger, where low-latency and high-throughput data operations are critical.

### Scaling Techniques

**1. Sharding Strategies for User Data**

To distribute the growing dataset across multiple servers, Facebook employed **sharding**, where data is partitioned based on user IDs.

- **How It Works**:

  - Each shard contains a subset of the user data, ensuring that no single database instance is overwhelmed.
  - Queries are routed to the appropriate shard based on the user ID, ensuring efficiency.

- **Code Example: Configuring Sharding in MySQL**

  Here's a simple example of configuring a sharded database setup:

  ```sql
  -- Shard 1
  CREATE DATABASE user_data_shard_1;
  USE user_data_shard_1;
  CREATE TABLE users (
      user_id INT PRIMARY KEY,
      name VARCHAR(100),
      email VARCHAR(100)
  );

  -- Shard 2
  CREATE DATABASE user_data_shard_2;
  USE user_data_shard_2;
  CREATE TABLE users (
      user_id INT PRIMARY KEY,
      name VARCHAR(100),
      email VARCHAR(100)
  );
  ```

  Application logic is then used to direct queries to the correct shard based on the user ID.

**2. Asynchronous Replication for High Availability**

To ensure data availability and fault tolerance, Facebook adopted **asynchronous replication**, where writes are first committed to a primary database and then propagated to replicas.

- **Advantages**:

  - Reduced latency for write operations, as they are not delayed by synchronous replication.
  - Increased reliability, as replicas can serve read operations and act as failovers in case the primary database goes down.

### Lessons from Facebook's Database Scaling Journey

Facebook's approach to database scaling is a testament to innovation at scale:

- Custom-built solutions like **TAO** and **RocksDB** showcase the importance of tailoring systems to meet specific business needs.
- Scaling techniques like **sharding** and **asynchronous replication** demonstrate the effectiveness of distributing data and operations to manage explosive growth.
- By continuously optimizing their database infrastructure, Facebook has maintained seamless user experiences, even under immense traffic.

This case study underscores the critical role of database architecture in supporting a platform's scalability and reliability.

## Case Study: Airbnb – Optimizing for High User Loads

### Background: The Rise of Airbnb and Scalability Challenges

Airbnb transformed the hospitality industry with its platform, enabling millions of users worldwide to book accommodations. As the platform grew, so did the challenges of managing high traffic, especially during peak seasons like holidays or major events.

- **Scalability Challenges**:

  - Handling simultaneous searches, bookings, and communications from users worldwide.
  - Dealing with high variability in traffic, which could spike unpredictably.
  - Ensuring smooth performance for users on both the host and guest sides of the platform.

### Optimization Strategies

**1. Leveraging Kubernetes for Container Orchestration**

To address its dynamic scaling needs, Airbnb adopted **Kubernetes**, a container orchestration platform. Kubernetes allowed Airbnb to manage and scale its microservices efficiently.

- **Benefits**:

  - Automated scaling of services based on traffic patterns.
  - Improved resource utilization by running multiple microservices on shared infrastructure.
  - Enhanced fault tolerance by automatically restarting failed containers.

- **Implementation**:

  Airbnb deployed its services in Docker containers and used Kubernetes to orchestrate their deployment, scaling, and management. This setup enabled seamless scaling during traffic surges.

**2. Implementing GraphQL for Efficient Data Querying**

Airbnb switched to **GraphQL** for its API layer to improve efficiency and reduce over-fetching and under-fetching of data.

- **Why GraphQL?**

  - Clients can request exactly the data they need, reducing payload size and improving performance.
  - Simplifies API management for complex queries involving multiple data sources.

- **Example**:

  Instead of multiple REST calls to retrieve user profiles, booking details, and reviews, a single GraphQL query retrieves all necessary data:

  ```graphql
  query {
    user(id: "123") {
      name
      bookings {
        property {
          name
          location
        }
        date
      }
      reviews {
        rating
        comment
      }
    }
  }
  ```

  This approach reduced server load and improved response times for Airbnb’s high-traffic use cases.

**3. Dynamic Pricing Algorithms to Manage Server Loads**

Dynamic pricing plays a dual role at Airbnb:

- It optimizes host revenue by adjusting prices based on demand.
- It indirectly balances server loads by influencing booking patterns.

**How It Works**:

- Algorithms analyze demand, location, seasonal trends, and event schedules.
- Prices are adjusted in real-time, encouraging bookings during off-peak periods and managing demand during surges.

This strategy not only boosts revenue but also helps stabilize server traffic during high-demand periods.

### Key Tools in Airbnb's Scalability Toolbox

**Redis for Caching**:

Airbnb uses **Redis**, an in-memory data store, to cache frequently accessed data such as search results and user session information.

- **Why Redis?**

  - Lightning-fast read/write operations reduce database queries.
  - Built-in data expiration helps manage cache freshness.

**RabbitMQ for Task Queuing**:

To handle background tasks like email notifications, data synchronization, and analytics, Airbnb relies on **RabbitMQ**.

- **Advantages**:

  - Asynchronous processing of non-critical tasks.
  - Ensures system responsiveness by offloading work from main application threads.

### Code Example: Integrating Kubernetes for Scaling Airbnb-Like Platforms

Here’s a simplified example of how Airbnb leverages Kubernetes to scale its services dynamically:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: search-service
spec:
  replicas: 2
  selector:
    matchLabels:
      app: search-service
  template:
    metadata:
      labels:
        app: search-service
    spec:
      containers:
        - name: search-service-container
          image: airbnb/search-service:latest
          resources:
            requests:
              memory: "512Mi"
              cpu: "500m"
            limits:
              memory: "1Gi"
              cpu: "1000m"
---
apiVersion: v1
kind: Service
metadata:
  name: search-service
spec:
  selector:
    app: search-service
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: LoadBalancer
```

- **How It Works**:

  - The `Deployment` defines the number of replicas for the **search-service** and sets resource limits.
  - The `Service` provides load balancing and exposes the application to external traffic.

As traffic increases, Kubernetes automatically adjusts the number of replicas, ensuring consistent performance.

### Airbnb's Success in Scalability

By combining advanced tools like Kubernetes, Redis, and RabbitMQ with innovative practices such as dynamic pricing and GraphQL APIs, Airbnb has created a platform capable of handling millions of users seamlessly. These strategies ensure not only scalability but also a smooth user experience, even during peak demand periods.

## Comparison of Strategies: Scaling Popular Apps

Scalability is a universal challenge for large-scale platforms, but each company approaches it differently, leveraging unique innovations and industry-best practices to meet their specific needs. Let’s explore some common scaling strategies and the unique methods that set Netflix, Facebook, and Airbnb apart.

### Common Approaches

**1. Use of Distributed Databases**

A fundamental strategy for scalability is the adoption of distributed databases. These systems distribute data across multiple nodes, ensuring high availability, fault tolerance, and the ability to handle massive traffic.

- **Examples**:

  - Netflix employs **Cassandra**, a distributed NoSQL database, to manage globally distributed user data and streaming metadata.
  - Facebook uses **RocksDB**, a high-performance key-value database optimized for flash storage, to handle its vast amount of social graph data.

- **Why Distributed Databases?**

  - They scale horizontally, accommodating increasing loads by adding more nodes.
  - They offer replication and partitioning to manage high availability and consistency.

**2. Adoption of Cloud-Native Architectures**

Cloud-native architectures allow companies to leverage the elasticity and scalability of cloud platforms like AWS, Google Cloud, and Azure.

- **Benefits**:

  - On-demand resource provisioning ensures cost-effectiveness.
  - Cloud services simplify infrastructure management while providing advanced tools like managed Kubernetes and auto-scaling.

- **Examples**:

  - Airbnb uses **Kubernetes** for container orchestration, enabling seamless scaling of microservices.
  - Netflix relies on **AWS EC2 Auto Scaling** to dynamically adjust server capacity based on traffic.

**3. Automation for Scaling Infrastructure**

Automation plays a critical role in scaling by reducing manual intervention and enabling systems to adapt in real time.

- **Techniques**:

  - Auto-scaling groups for infrastructure management.
  - Continuous delivery pipelines to deploy updates without downtime.
  - Monitoring and alerting tools like Prometheus and Grafana for proactive responses.

### Unique Innovations

**1. Netflix’s Chaos Engineering**

Netflix’s approach to scalability includes a unique practice called **Chaos Engineering**, which tests the resilience of its systems by intentionally introducing failures.

- **How It Works**:

  - Tools like **Chaos Monkey** randomly terminate instances in production to simulate failures.
  - Engineers identify weaknesses in the system and implement fixes, ensuring fault tolerance under real-world conditions.

- **Impact**:

  - Chaos Engineering has enabled Netflix to maintain high availability, even during large-scale outages.

**2. Facebook’s Custom Database Solutions**

Facebook’s scalability strategies heavily rely on in-house innovations to meet its unique challenges.

- **TAO (The Associations and Objects)**:

  - A custom database solution designed to handle the social graph at scale.
  - Optimized for low-latency reads and high-throughput writes.

- **RocksDB**:

  - A high-performance key-value database that efficiently manages storage and query performance for billions of active users.

- **Sharding Techniques**:

  - Facebook employs advanced sharding strategies to partition user data and ensure balanced distribution across servers.

**3. Airbnb’s Use of Kubernetes and GraphQL**

Airbnb focuses on modern tools to simplify scalability and improve user experience.

- **Kubernetes**:

  - Enables dynamic scaling of services based on traffic.
  - Handles fault tolerance through container orchestration.

- **GraphQL**:

  - Reduces over-fetching and under-fetching of data.
  - Allows clients to request exactly what they need, improving API efficiency and reducing server load.

- **Dynamic Pricing Algorithms**:

  - Adjust pricing based on demand, indirectly balancing server loads during peak times.

### Key Takeaways

| **Strategy**          | **Netflix**                       | **Facebook**                     | **Airbnb**                     |
| --------------------- | --------------------------------- | -------------------------------- | ------------------------------ |
| **Database**          | Cassandra for distributed storage | RocksDB and TAO for social graph | Redis for caching and RabbitMQ |
| **Scaling Technique** | AWS EC2 Auto Scaling              | Sharding and replication         | Kubernetes for orchestration   |
| **Unique Innovation** | Chaos Engineering for resilience  | Custom database solutions        | GraphQL for efficient querying |
| **Key Tools**         | AWS, Cassandra, Chaos Monkey      | RocksDB, TAO, MySQL              | Kubernetes, Redis, RabbitMQ    |

Each company’s strategies highlight the importance of tailoring scalability solutions to specific business needs. Netflix emphasizes resilience, Facebook focuses on optimizing data storage, and Airbnb leverages modern tools for operational efficiency.

By examining these approaches, businesses can adopt a combination of common practices and unique innovations to achieve scalability tailored to their specific challenges.

## Lessons Learned from Scaling Popular Apps

Scaling systems to handle millions of users and requests is a complex but essential task for modern applications. By analyzing the approaches of industry leaders like Netflix, Facebook, and Airbnb, we can extract valuable lessons that apply to a wide range of projects. These insights emphasize the importance of planning, making strategic trade-offs, and leveraging the right tools and frameworks.

### The Importance of Early Planning for Scalability

One of the most critical lessons from successful scaling stories is the necessity of planning for scalability from the start. Waiting until a system is under strain often leads to rushed, suboptimal solutions that could have been avoided with proactive planning.

- **Example from Netflix**:

  - During its early shift from DVDs to streaming, Netflix realized the limitations of its monolithic architecture. Early adoption of **microservices** and **cloud infrastructure** allowed the company to scale seamlessly as demand grew globally.
  - **Takeaway**: Design systems with scalability in mind, even if the current traffic doesn’t demand it.

- **Best Practices**:

  - Use modular architectures like microservices to isolate scaling challenges to specific components.
  - Conduct traffic simulations to anticipate future needs.
  - Incorporate flexibility for scaling into infrastructure and database design.

### Balancing Performance, Cost, and Complexity

Scalability often comes with trade-offs, and balancing performance, cost, and complexity is an ongoing challenge. Leaders like Facebook and Airbnb demonstrate how thoughtful decision-making can minimize these trade-offs.

- **Performance vs. Cost**:

  - **Facebook** optimized its infrastructure using custom tools like **RocksDB** and **TAO** for data efficiency, reducing resource costs while maintaining high performance.
  - For smaller teams or startups, adopting managed services like **AWS DynamoDB** or **Google Cloud Firestore** can offset the complexity of managing infrastructure.

- **Cost vs. Complexity**:

  - **Airbnb’s adoption of Kubernetes** simplified container orchestration and resource scaling but required an investment in developer expertise to manage this complexity.
  - Startups might consider simpler tools like **Docker Compose** or managed Kubernetes solutions to balance simplicity with scalability.

- **Key Takeaways**:

  - Avoid over-engineering solutions. Choose tools that align with your team's expertise and your project’s scale.
  - Consider managed services for cost efficiency when operational overhead is a concern.

### Leveraging the Right Tools and Frameworks

Choosing the right tools and frameworks for specific needs is a cornerstone of scalable architecture. The key is to select solutions that align with your project’s goals, anticipated growth, and technical expertise.

- **Distributed Databases**:

  - **Netflix** uses **Cassandra**, which excels at globally distributed data storage with high availability.
  - **Facebook** developed **RocksDB** to handle its unique social graph requirements.

- **Cloud Infrastructure**:

  - **Airbnb** leverages **Kubernetes** and cloud-native tools to orchestrate services and scale dynamically.
  - For teams with fewer resources, solutions like **AWS Auto Scaling Groups** or **Azure App Service** provide scalability without extensive operational complexity.

- **Monitoring and Observability**:

  - Tools like **Prometheus**, **Grafana**, and **DataDog** are vital for tracking system performance and identifying bottlenecks before they affect users.

- **Key Recommendations**:

  - Evaluate tools based on their ability to handle current and projected loads.
  - Prioritize tools with strong community support and documentation for smoother integration.

The success stories of Netflix, Facebook, and Airbnb highlight the universal importance of scalability while also showcasing how varied the solutions can be based on context. Early planning, thoughtful trade-offs, and leveraging the right tools allow businesses to scale effectively without compromising performance or user experience.

By applying these lessons, even smaller teams can design systems that are not only scalable but also robust, efficient, and aligned with business goals.

## Conclusion: Building Resilient Systems Through Scalability Lessons

The journey of scaling systems for global applications, as demonstrated by Netflix, Facebook, and Airbnb, offers invaluable lessons for businesses of all sizes. These tech giants have faced and overcome the challenges of handling massive user loads, maintaining performance, and ensuring reliability—all while continuously innovating and optimizing their architectures.

### Recap of Scalability Lessons

- **Netflix** taught us the power of adopting **microservices** and leveraging **cloud infrastructure** for seamless scaling. By using **auto-scaling policies** and fault-tolerant systems like Cassandra, they built a globally accessible platform capable of handling millions of users simultaneously.

- **Facebook** showcased how custom solutions like **TAO** and **RocksDB** can resolve unique challenges at scale. Their expertise in database sharding and replication highlights the importance of tailoring infrastructure to specific application needs.

- **Airbnb** demonstrated the value of **containerization** with Kubernetes and **efficient data querying** with GraphQL. Their approach to dynamic resource allocation and distributed task management serves as a blueprint for optimizing high-traffic platforms.

### Encouragement to Adapt and Innovate

While the scale of operations at Netflix, Facebook, and Airbnb may seem daunting, the principles they apply can be tailored to any organization. By focusing on modular architectures, strategic tooling, and proactive scalability planning, businesses can create resilient systems that grow alongside their user base.

- **Start Small, Scale Smart**: Begin with scalable designs that suit your current needs and build in flexibility for future growth.
- **Leverage the Right Tools**: Adopt frameworks and platforms that align with your technical expertise and project requirements.
- **Innovate Continuously**: Embrace experimentation and learn from failures, just as Netflix did with Chaos Engineering.

Scalability isn’t just about handling millions of requests—it’s about delivering a seamless, reliable experience to every user, every time. By learning from these case studies and applying their lessons, businesses can pave the way for robust, scalable, and future-proof systems.

---

Hi there, I'm Darshan Jitendra Chobarkar, a freelance web developer who's managed to survive the caffeine-fueled world of coding from the comfort of Pune. If you found the article you just read intriguing (or even if you're just here to silently judge my coding style), why not dive deeper into my digital world? Check out my portfolio at [https://darshanwebdev.com/](https://darshanwebdev.com/) – it's where I showcase my projects, minus the late-night bug fixing drama.

For a more 'professional' glimpse of me (yes, I clean up nice in a LinkedIn profile), connect with me at [https://www.linkedin.com/in/dchobarkar/](https://www.linkedin.com/in/dchobarkar/). Or if you're brave enough to see where the coding magic happens (spoiler: lots of Googling), my GitHub is your destination at [https://github.com/dchobarkar](https://github.com/dchobarkar). And, for those who've enjoyed my take on this blog article, there's more where that came from at [https://dchobarkar.github.io/](https://dchobarkar.github.io/). Dive in, leave a comment, or just enjoy the ride – looking forward to hearing from you!
