# TypeORM - 02: Working with Entities and Relationships

## Introduction to TypeORM Entities and Relationships

**TypeORM** is a powerful Object-Relational Mapping (ORM) framework for Node.js that supports TypeScript and JavaScript. It allows developers to work with entities and relationships directly mapped to the database tables and relations, making database operations more intuitive and type-safe.

### Understanding Entities in TypeORM

Entities in TypeORM represent the model that directly maps to a database table. They are defined as classes with decorators that provide metadata to TypeORM about how to handle the class and its properties.

**Code Snippet: Defining a Simple Entity**:

```jsx
import { Entity, PrimaryGeneratedColumn, Column } from "typeorm";

@Entity()
class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  @Column()
  email: string;
}
```

In this example, the `User` class is an entity with three columns: `id`, `name`, and `email`. The `@Entity()` decorator marks the class as an entity, and the `@Column()` decorator is used to define properties that correspond to columns in the database table.

### Exploring Relationship Types

TypeORM supports various types of relationships such as:

- One-to-One

- One-to-Many / Many-to-One

- Many-to-Many

These relationships reflect the associations between tables in a database and are essential for representing complex data models in your application.

**Code Snippet: One-to-Many Relationship Example**:

```jsx
import { Entity, PrimaryGeneratedColumn, Column, OneToMany } from "typeorm";
import { Post } from "./Post";

@Entity()
class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  @OneToMany(() => Post, (post) => post.user)
  posts: Post[];
}
```

In this example, a `User` can have many `Posts`, establishing a One-to-Many relationship between the `User` and `Post` entities. The `@OneToMany()` decorator is used to define this relationship.

### The Repository Pattern

TypeORM uses the repository pattern to provide an abstraction layer over database operations. Repositories are classes that encapsulate the logic required to access data sources. They provide a collection of methods to perform CRUD operations on entities.

**Code Snippet: Using Repository to Save an Entity**:

```jsx
import { getRepository } from "typeorm";
import { User } from "./User";

async function createUser() {
  const userRepository = getRepository(User);
  const user = new User();
  user.name = "John Doe";
  user.email = "john@example.com";

  await userRepository.save(user);
}
```

This example demonstrates how to use a repository to save a new `User` entity to the database. The `getRepository` function is used to obtain a repository instance for the `User` entity.

## Relationship Types in TypeORM

Understanding the variety of relationship types available in TypeORM is crucial for designing effective data models in your applications. TypeORM supports all standard relationship types found in relational databases:

- **One-to-One**: A direct relationship where each entity of one type corresponds to a single entity of another type. For instance, consider a scenario where each user has one profile. This relationship can be established in TypeORM using the `@OneToOne` decorator.

```jsx
@Entity()
class User {
  @PrimaryGeneratedColumn()
  id: number;

  @OneToOne(() => Profile)
  @JoinColumn()
  profile: Profile;
}

@Entity()
class Profile {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  bio: string;
}
```

- **One-to-Many/Many-to-One**: These relationships link one entity to multiple entities. For example, a blog post may have multiple comments, establishing a One-to-Many relationship from the post to comments. Conversely, each comment is linked to one post, illustrating the Many-to-One aspect.

```jsx
@Entity()
class Post {
  @PrimaryGeneratedColumn()
  id: number;

  @OneToMany(() => Comment, (comment) => comment.post)
  comments: Comment[];
}

@Entity()
class Comment {
  @PrimaryGeneratedColumn()
  id: number;

  @ManyToOne(() => Post, (post) => post.comments)
  post: Post;
}
```

- **Many-to-Many**: Occurs when multiple entities from one type relate to multiple entities from another type. For example, students and courses where students can enroll in multiple courses and courses can have many students.

```jsx
@Entity()
class Student {
  @PrimaryGeneratedColumn()
  id: number;

  @ManyToMany(() => Course)
  @JoinTable()
  courses: Course[];
}

@Entity()
class Course {
  @PrimaryGeneratedColumn()
  id: number;

  @ManyToMany(() => Student)
  students: Student[];
}
```

These relationships may utilize `@JoinColumn` and `@JoinTable` decorators to specify the owner side of the relationship and to define the joining strategy.

## Loading Entities with Relations

TypeORM offers two strategies for loading related entities: eager and lazy loading. Eager loading automatically fetches the related entities, while lazy loading fetches them on-demand.

- **Eager Loading**:

```jsx
@Entity()
class User {
  @PrimaryGeneratedColumn()
  id: number;

  @OneToOne(() => Profile, { eager: true })
  @JoinColumn()
  profile: Profile;
}
```

- **Lazy Loading**:

```jsx
@Entity()
class User {
  @PrimaryGeneratedColumn()
  id: number;

  @OneToOne(() => Profile, { lazy: true })
  @JoinColumn()
  profile: Promise<Profile>;
}
```

For more complex queries, the `QueryBuilder` can be used to dynamically construct SQL queries for entities and their relations.

- **Using QueryBuilder**:

```jsx
const userWithPosts = await userRepository
  .createQueryBuilder("user")
  .leftJoinAndSelect("user.posts", "post")
  .where("user.id = :id", { id: 1 })
  .getOne();
```

These tools and strategies provided by TypeORM for managing relationships and entity loading offer a robust solution for handling data persistence in Node.js applications, enabling developers to focus on business logic rather than boilerplate code for database operations.

## Using Repository API for CRUD Operations

The Repository API in TypeORM simplifies the interaction with database entities, offering an elegant abstraction layer to manage CRUD operations seamlessly. Repositories are linked to each entity and are used to manage entity instances. Let's explore how to utilize this API for basic and complex operations:

- **Creating a New Entity Instance**: Begin by instantiating your entity object, then utilize the repository's `save` method to persist it in the database.

```jsx
const user = new User();
user.name = "John Doe";
user.email = "john@example.com";
const userRepository = dataSource.getRepository(User);
await userRepository.save(user);
```

- **Reading Entities**: Fetch entities using the `find` method, optionally passing find options to filter results.

```jsx
const users = await userRepository.find({ where: { isActive: true } });
```

- **Updating an Entity**: Update entities by modifying their properties and saving them.

```jsx
user.name = "Jane Doe";
await userRepository.save(user);
```

- **Deleting an Entity**: Remove entities either by instance or criteria.

```jsx
await userRepository.delete(user.id);
```

- **Advanced Repository Methods**: For more complex scenarios, methods like `increment` and `decrement` can be particularly useful.

```jsx
await userRepository.increment({ id: 1 }, "loginCount", 1);
```

## Best Practices

Efficiency and optimization are paramount when working with entities and relationships in TypeORM. Here are some guidelines to ensure your development process is smooth and effective:

- **Efficient Entity and Relationship Definitions**: Define entities and their relationships clearly to avoid confusion and ensure smooth database operations.

- **Avoiding Common Pitfalls**: Be mindful of common issues such as circular dependencies or performance bottlenecks in relation loading.

- **Optimizing Repository Operations**: Leverage indexing, query optimization techniques, and TypeORM's built-in methods to enhance performance.

## Conclusion

We've journeyed through the essentials of working with entities, relationships, and repositories in TypeORM. These concepts form the backbone of TypeORM and empower developers to build robust, efficient applications.

As you continue exploring TypeORM, remember that experimentation and hands-on practice are invaluable. The next topics in our series will delve into more advanced features and practical integration examples, further expanding your TypeORM expertise. Stay tuned, and happy coding!

---

Hi there, I'm Darshan Jitendra Chobarkar, a freelance web developer who's managed to survive the caffeine-fueled world of coding from the comfort of Pune. If you found the article you just read intriguing (or even if you're just here to silently judge my coding style), why not dive deeper into my digital world? Check out my portfolio at [https://darshanwebdev.com/](https://darshanwebdev.com/) – it's where I showcase my projects, minus the late-night bug fixing drama.

For a more 'professional' glimpse of me (yes, I clean up nice in a LinkedIn profile), connect with me at [https://www.linkedin.com/in/dchobarkar/](https://www.linkedin.com/in/dchobarkar/). Or if you're brave enough to see where the coding magic happens (spoiler: lots of Googling), my GitHub is your destination at [https://github.com/dchobarkar](https://github.com/dchobarkar). And, for those who've enjoyed my take on this blog article, there's more where that came from at [https://dchobarkar.github.io/](https://dchobarkar.github.io/). Dive in, leave a comment, or just enjoy the ride – looking forward to hearing from you!
