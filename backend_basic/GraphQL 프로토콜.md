
## - GraphQL이란?

### 개념

- **GraphQL**: Facebook에서 개발한 쿼리 언어로, API의 데이터를 요청하고 조작하는 방법을 제공합니다.
- **특징**:
    - 클라이언트가 필요한 데이터만 선택하여 요청할 수 있습니다.
    - 서버는 클라이언트의 요청에 대해 정확한 형태로 데이터를 반환합니다.
    - 단일 엔드포인트를 통해 모든 쿼리와 변형을 처리합니다.

### 장점

- **효율적인 데이터 요청**: 클라이언트는 필요한 데이터만 요청하여 과도한 데이터 전송을 방지합니다.
- **강력한 타입 시스템**: GraphQL 스키마를 통해 API의 구조를 명확히 정의할 수 있습니다.
- **단일 엔드포인트**: 모든 요청이 단일 엔드포인트를 통해 처리됩니다.

##  NestJS에서 GraphQL 설정

### 1. 설치 및 설정

**1. NestJS 프로젝트 생성**

```bash
nest new graphql-example
cd graphql-example
```

**2. 필요한 패키지 설치**

```bash
npm install @nestjs/graphql graphql-tools graphql apollo-server-express
```

**3. GraphQL 모듈 설정**

- `src/app.module.ts` 파일을 열고, `GraphQLModule`을 설정합니다.

```tsx
// src/app.module.ts
import { Module } from '@nestjs/common';
import { GraphQLModule } from '@nestjs/graphql';
import { ApolloDriver, ApolloDriverConfig } from '@nestjs/apollo';
import { TypeOrmModule } from '@nestjs/typeorm';
import { UserModule } from './user/user.module';

@Module({
  imports: [
    TypeOrmModule.forRoot({
      type: 'sqlite',
      database: 'db.sqlite',
      entities: [__dirname + '/**/*.entity.{js,ts}'],
      synchronize: true,
    }),
    GraphQLModule.forRoot<ApolloDriverConfig>({
      driver: ApolloDriver,
      autoSchemaFile: true, // 스키마 자동 생성
    }),
    UserModule,
  ],
})
export class AppModule {}
```

### 2. GraphQL 스키마 정의

**1. User 엔티티 정의**

- `src/user/user.entity.ts` 파일을 생성하고, User 엔티티를 정의합니다.

```tsx
// src/user/user.entity.ts
import { ObjectType, Field, Int } from '@nestjs/graphql';
import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm';

@ObjectType()
@Entity()
export class User {
  @Field(() => Int)
  @PrimaryGeneratedColumn()
  id: number;

  @Field()
  @Column()
  name: string;

  @Field()
  @Column()
  email: string;
}
```

**2. User Resolver 정의**

- `src/user/user.resolver.ts` 파일을 생성하고, GraphQL 쿼리와 변형을 정의합니다.

```tsx
// src/user/user.resolver.ts
import { Resolver, Query, Mutation, Args } from '@nestjs/graphql';
import { User } from './user.entity';
import { UserService } from './user.service';
import { CreateUserInput } from './dto/create-user.input';

@Resolver(() => User)
export class UserResolver {
  constructor(private readonly userService: UserService) {}

  @Query(() => [User])
  async users(): Promise<User[]> {
    return this.userService.findAll();
  }

  @Mutation(() => User)
  async createUser(@Args('createUserInput') createUserInput: CreateUserInput): Promise<User> {
    return this.userService.create(createUserInput);
  }
}
```

**3. User Service 정의**

- `src/user/user.service.ts` 파일을 생성하고, 데이터베이스와의 상호작용을 처리합니다.

```tsx
// src/user/user.service.ts
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { User } from './user.entity';
import { CreateUserInput } from './dto/create-user.input';

@Injectable()
export class UserService {
  constructor(
    @InjectRepository(User)
    private readonly userRepository: Repository<User>,
  ) {}

  async findAll(): Promise<User[]> {
    return this.userRepository.find();
  }

  async create(createUserInput: CreateUserInput): Promise<User> {
    const user = this.userRepository.create(createUserInput);
    return this.userRepository.save(user);
  }
}
```

**4. CreateUserInput DTO 정의**

- `src/user/dto/create-user.input.ts` 파일을 생성하고, 사용자 입력을 위한 DTO를 정의합니다.

```tsx
// src/user/dto/create-user.input.ts
import { InputType, Field } from '@nestjs/graphql';

@InputType()
export class CreateUserInput {
  @Field()
  name: string;

  @Field()
  email: string;
}
```

### 3. GraphQL 쿼리 및 변형 테스트

- **GraphQL Playground**를 통해 쿼리와 변형을 테스트할 수 있습니다. 서버를 시작한 후, `http://localhost:3000/graphql`로 접속하여 GraphQL Playground를 사용합니다.

**예제 쿼리**

```graphql
query {
  users {
    id
    name
    email
  }
}

mutation {
  createUser(createUserInput: { name: "John Doe", email: "john.doe@example.com" }) {
    id
    name
    email
  }
}
```

## - 요약

- **GraphQL**은 클라이언트가 필요한 데이터를 요청할 수 있게 하며, 단일 엔드포인트를 통해 모든 쿼리와 변형을 처리합니다.
- **NestJS**를 사용하여 GraphQL API를 설정하고, 엔티티, 리졸버, 서비스, DTO를 정의하여 GraphQL 스키마를 구성할 수 있습니다.
- **GraphQL Playground**를 통해 쿼리와 변형을 테스트하며, 실시간으로 API를 검증할 수 있습니다.