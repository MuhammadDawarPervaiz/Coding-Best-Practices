# TypeORM Coding Best Practices

Welcome to the TypeORM Coding Best Practices guide! This document provides essential guidelines for writing clean, efficient, and maintainable code with TypeORM, an ORM for TypeScript and JavaScript.

## Table of Contents

- [General Guidelines](#general-guidelines)
- [Entity Design](#entity-design)
- [Repositories and Data Access](#repositories-and-data-access)
- [Query Building](#query-building)
- [Transaction Management](#transaction-management)
- [Error Handling](#error-handling)
- [Performance Optimization](#performance-optimization)
- [Testing](#testing)
- [Documentation](#documentation)
- [Resources](#resources)

## General Guidelines

1. **Use TypeScript**: Leverage TypeScript's static typing and decorators to define entities and manage migrations effectively.

2. **Organize Your Code**: Structure your project with clear separations between entities, repositories, services, and controllers.

```
src/
├── entities/
├── repositories/
├── services/
└── controllers/
```

3. **Keep Your Code DRY**: Avoid duplicating code by utilizing shared services, utility functions, and reusable entities.

## Entity Design

1. **Define Entities with Decorators**: Use TypeORM decorators (`@Entity`, `@Column`, `@PrimaryGeneratedColumn`, etc.) to define your entities and their relationships.

   ```typescript
   import { Entity, PrimaryGeneratedColumn, Column, ManyToOne } from 'typeorm';

   @Entity()
   export class User {
       @PrimaryGeneratedColumn()
       id: number;

       @Column({ type: 'varchar', length: 100 })
       name: string;

       @ManyToOne(() => Group, (group) => group.users)
       group: Group;
   }
   ```

2. **Use Proper Data Types**: Choose appropriate data types for your columns to ensure data integrity and optimize performance.

   ```typescript
   @Column({ type: 'int', default: 0 })
   age: number;

   @Column({ type: 'varchar', length: 255, nullable: true })
   bio?: string;
   ```

3. **Define Relationships Clearly**: Use TypeORM’s relationship decorators (`@ManyToOne`, `@OneToMany`, `@OneToOne`, `@ManyToMany`) to define relationships between entities.

   ```typescript
   @Entity()
   export class Group {
       @PrimaryGeneratedColumn()
       id: number;

       @OneToMany(() => User, (user) => user.group)
       users: User[];
   }
   ```

## Repositories and Data Access

1. **Use Repositories for Data Access**: Use TypeORM repositories or custom repositories to handle data access and business logic.

   ```typescript
   import { Repository, EntityRepository } from 'typeorm';
   import { User } from '../entities/User';

   @EntityRepository(User)
   export class UserRepository extends Repository<User> {
       async findByName(name: string): Promise<User[]> {
           return this.find({ where: { name } });
       }
   }
   ```

2. **Keep Business Logic in Services**: Avoid placing business logic directly in repositories. Instead, use services to encapsulate business rules and interactions.

   ```typescript
   import { UserRepository } from '../repositories/UserRepository';
   import { Injectable } from '@nestjs/common';

   @Injectable()
   export class UserService {
       constructor(private readonly userRepository: UserRepository) {}

       async getUserByName(name: string): Promise<User[]> {
           return this.userRepository.findByName(name);
       }
   }
   ```

## Query Building

1. **Use QueryBuilder for Complex Queries**: Utilize TypeORM’s `QueryBuilder` for complex queries that involve multiple joins, conditions, or aggregations.

   ```typescript
   const users = await getRepository(User)
       .createQueryBuilder('user')
       .leftJoinAndSelect('user.group', 'group')
       .where('user.age > :age', { age: 18 })
       .orderBy('user.name', 'ASC')
       .getMany();
   ```

2. **Avoid Raw Queries for Standard Operations**: Use TypeORM’s built-in methods (`find`, `save`, `remove`, etc.) for standard CRUD operations to maintain consistency and leverage TypeORM’s features.

   ```typescript
   // Good
   const user = await userRepository.findOne(userId);

   // Bad
   const user = await getConnection().query('SELECT * FROM user WHERE id = ?', [userId]);
   ```

## Transaction Management

1. **Use Transactions for Multiple Operations**: Ensure consistency and integrity by using transactions for operations involving multiple database actions.

   ```typescript
   import { getManager } from 'typeorm';

   await getManager().transaction(async (transactionalEntityManager) => {
       await transactionalEntityManager.save(User, user);
       await transactionalEntityManager.save(Order, order);
   });
   ```

2. **Handle Transaction Failures**: Ensure proper handling and rollback of transactions in case of failures.

   ```typescript
   try {
       await getManager().transaction(async (transactionalEntityManager) => {
           await transactionalEntityManager.save(User, user);
           await transactionalEntityManager.save(Order, order);
       });
   } catch (error) {
       console.error('Transaction failed:', error);
   }
   ```

## Error Handling

1. **Handle Errors Gracefully**: Catch and handle errors appropriately in your application logic to provide meaningful error messages and maintain stability.

   ```typescript
   try {
       const user = await userRepository.findOne(userId);
       if (!user) {
           throw new Error('User not found');
       }
   } catch (error) {
       console.error('Error fetching user:', error);
   }
   ```

2. **Use Custom Error Classes**: Define custom error classes for specific scenarios to make error handling more descriptive and organized.

   ```typescript
   export class UserNotFoundException extends Error {
       constructor(message: string) {
           super(message);
           this.name = 'UserNotFoundException';
       }
   }
   ```

## Performance Optimization

1. **Use Indexes Appropriately**: Create indexes on columns that are frequently queried or used in joins to improve query performance.

   ```typescript
   @Entity()
   @Index('IDX_USER_EMAIL', ['email'])
   export class User {
       @PrimaryGeneratedColumn()
       id: number;

       @Column({ type: 'varchar', length: 255 })
       email: string;
   }
   ```

2. **Avoid N+1 Query Problem**: Use eager or lazy loading, or joins, to avoid the N+1 query problem when fetching related entities.

   ```typescript
   // Eager loading
   const users = await userRepository.find({ relations: ['group'] });

   // Using join
   const users = await getRepository(User)
       .createQueryBuilder('user')
       .leftJoinAndSelect('user.group', 'group')
       .getMany();
   ```

3. **Limit Data Returned**: Use `select` and `take` methods to limit the amount of data returned by queries to improve performance.

   ```typescript
   const users = await userRepository.createQueryBuilder('user')
       .select(['user.id', 'user.name'])
       .take(10)
       .getMany();
   ```

## Testing

1. **Write Unit Tests**: Write unit tests for your repositories and services to ensure that they function correctly.

2. **Use In-Memory Databases for Tests**: Use an in-memory database or a test database to run tests, avoiding interference with production data.

   ```typescript
   import { createConnection, getConnection } from 'typeorm';
   import { User } from './entities/User';

   beforeEach(async () => {
       await createConnection({
           type: 'sqlite',
           database: ':memory:',
           entities: [User],
           synchronize: true,
       });
   });

   afterEach(async () => {
       await getConnection().close();
   });
   ```

## Documentation

1. **Document Entities and Repositories**: Maintain documentation for your entities, relationships, and repository methods to make it easier for others to understand and use your code.

2. **Comment Complex Queries and Logic**: Use comments to explain complex queries, transactions, or logic in your services and repositories.

   ```typescript
   // Get the most recent orders for each user
   const recentOrders = await getRepository(Order)
       .createQueryBuilder('order')
       .select('order.userId, MAX(order.date)', 'recentDate')
       .groupBy('order.userId')
       .getRawMany();
   ```

## Resources

- [TypeORM Documentation](https://typeorm.io/)
- [TypeORM Best Practices](https://typeorm.io/#best-practices)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)
- [MDN Web Docs](https://developer.mozilla.org/en-US/)

Feel free to contribute to this guide or suggest improvements. Happy coding!
```

This `README.md` file provides a thorough overview of best practices for working with TypeORM, ensuring that your code is efficient, maintainable, and robust.