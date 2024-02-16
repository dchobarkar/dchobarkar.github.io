# TypeORM - 01: Introduction to TypeORM

## Introduction

The modern web development landscape is rich with techniques and tools designed to streamline database interactions, one of which is Object-Relational Mapping (ORM). ORM serves as a bridge between the object-oriented world of application development and the relational world of databases. TypeORM, a prominent ORM library tailored for TypeScript and JavaScript applications, stands out for its seamless integration and robust feature set. This introduction aims to shed light on the essentials of ORM and the unique advantages TypeORM brings to the table.

## What is TypeORM?

TypeORM emerges as a powerful ORM library that simplifies data manipulation and interaction within TypeScript and JavaScript projects. It abstracts database operations, allowing developers to work with objects and classes instead of SQL queries, thereby enhancing code readability and maintainability. TypeORM's compatibility with both active record and data mapper patterns offers developers the flexibility to adopt an approach that best suits their project's architecture.

## Why Use TypeORM?

The adoption of TypeORM in web development projects brings numerous benefits. Its tight integration with TypeScript enables developers to utilize advanced features such as decorators for defining entities, thereby ensuring type safety and reducing runtime errors. TypeORM's database agnosticity means it supports a wide array of database engines, including MySQL, PostgreSQL, SQLite, and more, providing the freedom to switch between databases with minimal code changes. Moreover, TypeORM's support for advanced functionalities like transactions, migrations, and automatic database schema generation streamlines development workflows, making it an invaluable tool for modern web development projects.

## Comparison with Other ORMs

When evaluating TypeORM against other ORMs like Sequelize, Mongoose, and Knex.js, it's essential to consider several factors to understand its unique standing, especially for projects leveraging TypeScript.

**Syntax and Ease of Use**: TypeORM's syntax is highly influenced by its support for decorators, which allows for a more declarative way of modeling entities and relationships directly within class definitions. This approach is in contrast to Sequelize and Mongoose, which rely more on function calls and object literals to define models and relationships. Knex.js, being a query builder more than an ORM, provides a lower-level API for building SQL queries but lacks the higher-level abstractions for defining models and relationships.

**TypeScript Support**: TypeORM is designed with TypeScript as a first-class citizen, offering strong typing and decorators out of the box. Sequelize and Mongoose have made strides in improving their TypeScript support, but they were initially designed for JavaScript, which sometimes shows in their API design and typing ergonomics. Knex.js, while usable with TypeScript, also doesn't provide the same level of integration as TypeORM.

**Performance**: Performance can vary widely depending on the use case, queries, and database design. However, TypeORM's architecture aims to strike a balance between ease of use and performance. The differences in performance among these ORMs are generally not significant enough to be the sole deciding factor for most projects.

**Community Support and Ecosystem**: Sequelize and Mongoose benefit from being around longer, having a larger community, and more third-party resources. TypeORM, while younger, has been growing rapidly in popularity, especially within the TypeScript community. Knex.js enjoys robust support as both a standalone query builder and as the foundation for other ORMs like Objection.js.

## Setting Up TypeORM in Your Project

To integrate TypeORM into a Node.js project, follow these steps to get started:

1.  **Prerequisites**:

    - Ensure Node.js is installed.

    - Basic knowledge of TypeScript is recommended.

2.  **Initialize Your Node.js Project**:

    Create a new folder for your project and run `npm init -y` to generate a `package.json` file.

3.  **Install TypeORM and Database Driver**:

    Install TypeORM using npm by running `npm install typeorm --save`. Additionally, you'll need to install the database driver for your specific database (e.g., PostgreSQL, MySQL, SQLite). For PostgreSQL, you would use `npm install pg --save`.

4.  **Configure TypeORM**:

        Create a file named `ormconfig.json` in your project root. This file will contain your database connection settings. Here’s an example configuration for a PostgreSQL database:

        ```jsx

    {
    "type": "postgres",
    "host": "localhost",
    "port": 5432,
    "username": "test",
    "password": "test",
    "database": "test",
    "entities": ["src/entity/**/*.ts"],
    "synchronize": true
    }

    ```

    - `type`: Specifies the database type.

    - `host`, `port`, `username`, `password`, `database`: Database connection options.

    - `entities`: Path to your entity classes.

    - `synchronize`: If set to true, TypeORM will automatically synchronize the database schema with your models. This is useful in development but should be disabled in production.

    ```

5.  **Write Your First Entity**:

    Define a simple entity as a TypeScript class in a file named `User.ts` within the `src/entity` directory:

    ```jsx
    import { Entity, PrimaryGeneratedColumn, Column } from "typeorm";

    @Entity()
    export class User {
      @PrimaryGeneratedColumn()
      id: number;

      @Column()
      name: string;

      @Column()
      email: string;
    }
    ```

6.  **Compile and Run**:

    Ensure your `tsconfig.json` is correctly set up for your TypeScript project, then compile your TypeScript code using `tsc`. Run your application with `node`, pointing to the compiled JavaScript files.

    This setup provides a solid foundation for developing a Node.js application with TypeORM, leveraging TypeScript's strong typing and the ORM's powerful data modeling capabilities. As you progress, explore more complex mappings, relationships, and TypeORM's advanced features to fully utilize its potential in your projects.

## Creating Your First Entity

Entities are at the heart of TypeORM, serving as the bridge between your TypeScript classes and the database tables. They allow for a seamless translation of database records into objects that can be used within your application. Here’s how you can start defining entities with TypeORM:

1. **Define an Entity**: An entity represents a table in your database. To create an entity, you'll use the `@Entity()` decorator to decorate a class. This tells TypeORM that the class should be linked with a table of the same name.

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

   In this code snippet, we define a `User` entity with three columns: `id`, `name`, and `email`. The `@PrimaryGeneratedColumn()` decorator is used to indicate that the `id` column is an auto-incremented primary key, while the `@Column()` decorator is used for other table columns.

2. **Working with Repositories**: With an entity in place, you can use a repository to perform CRUD operations. TypeORM repositories provide a rich API to interact with entities.

   ```jsx
   import { getRepository } from "typeorm";
   import { User } from "./entity/User";

   async function createUser() {
     const userRepository = getRepository(User);
     const newUser = userRepository.create({
       name: "John Doe",
       email: "john@example.com",
     });

     await userRepository.save(newUser);
     console.log("User created with id:", newUser.id);
   }

   createUser();
   ```

   This example demonstrates how to create a new user and save it to the database using the `User` repository. The `getRepository` function is used to obtain a repository instance for the `User` entity.

## Conclusion

Throughout this series, we've explored the fundamentals of TypeORM and its integration with TypeScript. From setting up your project and connecting to a database to defining entities and performing CRUD operations, TypeORM offers a robust solution for managing database interactions in a type-safe manner.

As you continue to build more complex applications, I encourage you to dive deeper into the TypeORM documentation. There's a wealth of advanced features waiting to be discovered, including custom repositories, complex relations, query builders, and much more. Embracing these capabilities will enable you to craft efficient, scalable, and maintainable applications.

Remember, the key to mastering TypeORM—or any technology—is consistent practice and exploration. So, don't hesitate to experiment with different features and apply them to your projects. Happy coding!

---

Hi there, I'm Darshan Jitendra Chobarkar, a freelance web developer who's managed to survive the caffeine-fueled world of coding from the comfort of Pune. If you found the article you just read intriguing (or even if you're just here to silently judge my coding style), why not dive deeper into my digital world? Check out my portfolio at [https://darshanwebdev.com/](https://darshanwebdev.com/) – it's where I showcase my projects, minus the late-night bug fixing drama.

For a more 'professional' glimpse of me (yes, I clean up nice in a LinkedIn profile), connect with me at [https://www.linkedin.com/in/dchobarkar/](https://www.linkedin.com/in/dchobarkar/). Or if you're brave enough to see where the coding magic happens (spoiler: lots of Googling), my GitHub is your destination at [https://github.com/dchobarkar](https://github.com/dchobarkar). And, for those who've enjoyed my take on this blog article, there's more where that came from at [https://dchobarkar.github.io/](https://dchobarkar.github.io/). Dive in, leave a comment, or just enjoy the ride – looking forward to hearing from you!
