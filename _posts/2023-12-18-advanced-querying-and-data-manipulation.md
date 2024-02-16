# TypeORM - 03: Advanced Querying and Data Manipulation

## Introduction

TypeORM stands out in the modern development landscape for its ability to seamlessly integrate with TypeScript, providing developers with strong typing, and the use of decorators to define entity models. This synergy not only ensures type safety but also simplifies the development process by enabling a more declarative approach to define database schemas directly within code, reducing the gap between the database and application logic.

One of TypeORM's core strengths lies in its comprehensive support for various databases, including MySQL, PostgreSQL, SQLite, and more, making it a versatile choice for projects regardless of the underlying database technology. Additionally, its active community and continuous development ensure that it stays relevant and up-to-date with the latest advancements in database management and TypeScript.

## QueryBuilder: Building Complex Queries

The QueryBuilder interface in TypeORM is a powerful feature designed to construct complex database queries using a fluent API. It provides a more flexible and readable approach to querying data, especially when dealing with dynamic query conditions, joins, and subqueries. By leveraging QueryBuilder, developers can programmatically build SQL queries in a structured manner, improving maintainability and reducing the risk of SQL injection vulnerabilities.

### Selecting, Joining, and Where Conditions

QueryBuilder simplifies the process of selecting specific columns, joining tables, and applying conditional logic. For example, to fetch users with specific criteria, you might write:

```jsx
import { getConnection } from "typeorm";

const users = await getConnection()
  .getRepository(User)
  .createQueryBuilder("user")
  .leftJoinAndSelect("user.photos", "photo")
  .where("user.name = :name", { name: "John" })
  .andWhere("photo.isPublished = true")
  .getMany();
```

This snippet demonstrates how to select users and their published photos using joins and where conditions, showcasing the elegance and power of QueryBuilder in handling relational data.

### Aggregations and Subqueries

Aggregations and subqueries are essential for deriving complex data insights. TypeORM's QueryBuilder elegantly supports these operations, allowing for intricate data manipulation and analysis. For instance, to calculate the average age of users:

```jsx
const averageAge = await getConnection()
  .getRepository(User)
  .createQueryBuilder("user")
  .select("AVG(user.age)", "averageAge")
  .getRawOne();
```

This code calculates the average age of users directly from the database, illustrating QueryBuilder's capability to execute advanced SQL functions and operations seamlessly.

## Using Find Options for Querying

In the realm of database management with TypeORM, **Find Options** stand out as a powerful feature that significantly simplifies data retrieval by offering a fluent API to construct complex queries with ease. These options provide developers with a straightforward way to filter, order, and limit the results returned from the database, making it possible to precisely tailor the data to the application's needs.

### Simplifying Querying with Find Options

TypeORM's Find Options allow for the creation of dynamic queries using simple JavaScript objects. This method is particularly useful when dealing with various filtering conditions or when the query parameters are not known upfront. For example, to retrieve a list of users filtered by age and sorted by their last name, you can use Find Options as follows:

```jsx
import { getRepository } from "typeorm";
import { User } from "./entity/User";

const users = await getRepository(User).find({
  where: {
    age: 25,
  },
  order: {
    lastName: "ASC", // ASC for ascending, DESC for descending
  },
});
```

This approach not only enhances code readability but also reduces the likelihood of SQL injection attacks, as the query parameters are automatically sanitized by TypeORM.

### Filtering, Ordering, and Limiting Results

Find Options offer a range of functionalities for precise data querying:

- **Filtering**: You can specify conditions in the `where` clause to filter the records. It supports exact matches, like comparisons, in conditions, and more complex queries.

- **Ordering**: The `order` option allows sorting the results based on one or more fields. It's possible to specify ascending or descending order.

- **Limiting and Skipping**: To control the size of the result set, `take` and `skip` options can be used, which correspond to SQL's LIMIT and OFFSET clauses.

By leveraging these features, developers can construct robust queries that deliver precisely the data needed by the application.

## Transactions: Managing Transactional Operations

Transactions are fundamental in ensuring data integrity and consistency across multiple database operations. In scenarios where several operations need to be executed as a single unit of work, transactions come into play. TypeORM provides comprehensive support for transactions, ensuring that all operations within a transaction block are successfully completed or rolled back in case of an error.

### Handling Transactions in TypeORM

TypeORM offers several ways to handle transactions:

- **Transaction Manager**: This approach gives you complete control over the transaction. You manually begin, commit, or rollback the transaction. It's useful when you need fine-grained control over the transaction lifecycle.

```jsx
import { getConnection } from "typeorm";

await getConnection().transaction(async (transactionalEntityManager) => {
  await transactionalEntityManager.save(user1);
  await transactionalEntityManager.save(user2);
  // more operations...
});
```

- **Transactional Decorator**: For applications structured around services, the `@Transactional()` decorator provided by `typeorm-transactional-cls-hooked` package simplifies transaction management by automatically handling the transaction lifecycle around a method.

```jsx
import { Transactional } from "typeorm-transactional-cls-hooked";

class UserService {
  @Transactional()
  async createUser(user: User) {
    // operation is automatically wrapped in a transaction
  }
}
```

### Best Practices for Using Transactions

When utilizing transactions in TypeORM, consider the following best practices to ensure optimal performance and data integrity:

- **Scope Transactions Appropriately**: Keep transactions as short as possible, encompassing only the necessary operations to maintain data consistency.

- **Handle Rollbacks**: Ensure proper error handling within transactions to catch any exceptions and rollback changes when an operation fails.

- **Test Transactional Logic**: Thoroughly test the transactional behavior of your application under various scenarios to guarantee the correctness of your business logic.

By adhering to these guidelines and effectively using TypeORM's transactional capabilities, you can build reliable and robust Node.js applications that correctly manage state across complex operations.

## Working with Subscribers and Listeners for Entity Events

TypeORM provides powerful tools for automatically reacting to entity lifecycle events through its **Subscribers and Listeners**. These features allow developers to write clean, maintainable code by separating concerns and implementing logic that needs to run in response to specific events in the lifecycle of entities, such as insertions, updates, or deletions.

### Explanation of Subscribers and Listeners

- **Subscribers** are classes that listen to entity lifecycle events globally, across all instances of entities.

- **Listeners** are methods annotated within entity classes that respond to lifecycle events for instances of that entity.

### Use Cases

- Automatically validating or sanitizing data before saving to the database.

- Logging changes for audit trails.

- Cascading soft deletes or updates to related entities.

### Setting Up Subscribers and Listeners

To set up a subscriber, you create a class that implements the `EntitySubscriberInterface` and define the lifecycle hooks you're interested in:

```jsx
import {
  EntitySubscriberInterface,
  EventSubscriber,
  InsertEvent,
} from "typeorm";

@EventSubscriber()
class MyEntitySubscriber implements EntitySubscriberInterface<MyEntity> {
  /**
   * Called before entity insertion.
   */
  beforeInsert(event: InsertEvent<MyEntity>) {
    console.log(`BEFORE ENTITY INSERTED: `, event.entity);
  }
}
```

For listeners, you annotate methods within your entity class:

```jsx
import { Entity, Column, BeforeInsert } from "typeorm";

@Entity()
export class User {
  @Column()
  name: string;

  @BeforeInsert()
  setNameToUpperCase() {
    this.name = this.name.toUpperCase();
  }
}
```

### Practical Examples

Implementing a listener for sanitizing user input upon creation:

```jsx
@BeforeInsert()
sanitizeEmail() {
    this.email = this.email.trim().toLowerCase();
}
```

Setting up a subscriber for logging every update made to an entity:

```jsx
afterUpdate(event: UpdateEvent<any>) {
    console.log(`AFTER ENTITY UPDATED: `, event.entity);
}
```

## Conclusion

Throughout this series, we've delved into the powerful querying capabilities and data manipulation techniques that TypeORM offers. From basic CRUD operations to complex transactions and advanced querying, TypeORM stands out as a comprehensive ORM solution for TypeScript projects.

We encourage you to dive deeper into TypeORM's extensive documentation and engage with the community for more insights and best practices. Whether you're working on a small project or a large-scale application, TypeORM has the tools and features to support your database management needs effectively.

Stay tuned for upcoming articles where we'll explore more specific features of TypeORM, such as integration with other technologies and advanced patterns for data modeling and service layer abstraction. Happy coding!

---

Hi there, I'm Darshan Jitendra Chobarkar, a freelance web developer who's managed to survive the caffeine-fueled world of coding from the comfort of Pune. If you found the article you just read intriguing (or even if you're just here to silently judge my coding style), why not dive deeper into my digital world? Check out my portfolio at [https://darshanwebdev.com/](https://darshanwebdev.com/) – it's where I showcase my projects, minus the late-night bug fixing drama.

For a more 'professional' glimpse of me (yes, I clean up nice in a LinkedIn profile), connect with me at [https://www.linkedin.com/in/dchobarkar/](https://www.linkedin.com/in/dchobarkar/). Or if you're brave enough to see where the coding magic happens (spoiler: lots of Googling), my GitHub is your destination at [https://github.com/dchobarkar](https://github.com/dchobarkar). And, for those who've enjoyed my take on this blog article, there's more where that came from at [https://dchobarkar.github.io/](https://dchobarkar.github.io/). Dive in, leave a comment, or just enjoy the ride – looking forward to hearing from you!
