
## NestJS 프로젝트 구조

NestJS는 **모듈화된** 구조를 기반으로 하며, 다음과 같은 주요 구성 요소로 나뉩니다:

- **Modules**: 애플리케이션의 기능을 모듈 단위로 나누어 관리합니다.
- **Controllers**: 클라이언트의 HTTP 요청을 처리하고, 응답을 반환합니다.
- **Services**: 비즈니스 로직을 처리하고, 데이터베이스와 상호작용합니다.
- **Repositories**: TypeORM을 사용하여 데이터베이스와의 상호작용을 처리합니다.

전체적인 프로젝트 구조는 다음과 같습니다:

```bash
src/
|-- app.module.ts
|-- app.controller.ts
|-- app.service.ts
|-- modules/
    |-- user/
        |-- user.module.ts
        |-- user.controller.ts
        |-- user.service.ts
        |-- user.entity.ts
        |-- user.repository.ts
    |-- auth/
        |-- auth.module.ts
        |-- auth.controller.ts
        |-- auth.service.ts
        |-- auth.entity.ts
        |-- auth.repository.ts
```

## Module (모듈)

### 개념

- **Module**은 애플리케이션의 기능을 그룹화하여 관리하는 단위입니다. 하나의 모듈은 관련된 컨트롤러와 서비스를 포함하며, 기능별로 애플리케이션을 구조화하는 데 사용됩니다.

### 역할

- **구성 요소 그룹화**: 관련된 컨트롤러, 서비스, 그리고 다른 모듈들을 그룹화하여 코드의 재사용성과 유지보수성을 높입니다.
- **의존성 주입**: 모듈은 다른 모듈에 대한 의존성을 주입할 수 있습니다.

### 코드 예시

```tsx
// user.module.ts
import { Module } from '@nestjs/common';
import { UserController } from './user.controller';
import { UserService } from './user.service';
import { TypeOrmModule } from '@nestjs/typeorm';
import { UserEntity } from './user.entity';
import { UserRepository } from './user.repository';

@Module({
  imports: [TypeOrmModule.forFeature([UserEntity])],
  controllers: [UserController],
  providers: [UserService, UserRepository],
  exports: [UserService],
})
export class UserModule {}

```

## Controller (컨트롤러)

### 개념

- **Controller**는 클라이언트의 HTTP 요청을 처리하고, 응답을 반환하는 역할을 합니다. 요청을 처리하기 위해 비즈니스 로직을 서비스에 위임합니다.

### 역할

- **HTTP 요청 처리**: 클라이언트로부터의 요청을 수신하고, 적절한 응답을 반환합니다.
- **라우팅**: 요청 URL을 기반으로 적절한 메서드를 호출하여 응답을 처리합니다.

### 코드 예시

```tsx
// user.controller.ts
import { Controller, Get, Post, Body, Param } from '@nestjs/common';
import { UserService } from './user.service';
import { CreateUserDto } from './dto/create-user.dto';

@Controller('users')
export class UserController {
  constructor(private readonly userService: UserService) {}

  @Post()
  async create(@Body() createUserDto: CreateUserDto) {
    return this.userService.create(createUserDto);
  }

  @Get(':id')
  async findOne(@Param('id') id: string) {
    return this.userService.findOne(id);
  }
}

```

## Service (서비스)

### 개념

- **Service**는 비즈니스 로직을 구현하며, 컨트롤러에서 호출되어 데이터 처리와 비즈니스 규칙을 적용합니다. 데이터베이스와의 상호작용을 처리합니다.

### 역할

- **비즈니스 로직**: 데이터 처리와 비즈니스 규칙을 구현합니다.
- **데이터베이스 상호작용**: TypeORM Repository를 사용하여 데이터베이스와의 상호작용을 수행합니다.

### 코드 예시

```tsx
// user.service.ts
import { Injectable } from '@nestjs/common';
import { UserRepository } from './user.repository';
import { CreateUserDto } from './dto/create-user.dto';

@Injectable()
export class UserService {
  constructor(private readonly userRepository: UserRepository) {}

  async create(createUserDto: CreateUserDto) {
    const user = this.userRepository.create(createUserDto);
    return this.userRepository.save(user);
  }

  async findOne(id: string) {
    return this.userRepository.findOne(id);
  }
}

```

## Repository (레포지토리)

### 개념

- **Repository**는 TypeORM을 사용하여 데이터베이스와의 상호작용을 관리합니다. Entity를 기반으로 데이터베이스 CRUD(Create, Read, Update, Delete) 작업을 처리합니다.

### 역할

- **데이터베이스 작업**: CRUD 작업을 수행하며, 데이터베이스 쿼리를 처리합니다.
- **Entity와 매핑**: Entity를 데이터베이스 테이블과 매핑합니다.

### 코드 예시

```tsx
// user.repository.ts
import { EntityRepository, Repository } from 'typeorm';
import { UserEntity } from './user.entity';

@EntityRepository(UserEntity)
export class UserRepository extends Repository<UserEntity> {}

```

## 요약

- **Module**: 애플리케이션의 기능을 그룹화하고, 다른 모듈과의 의존성을 관리합니다.
- **Controller**: HTTP 요청을 처리하고, 응답을 반환합니다. 서비스에 비즈니스 로직을 위임합니다.
- **Service**: 비즈니스 로직을 구현하고, 데이터베이스와의 상호작용을 처리합니다.
- **Repository**: TypeORM을 사용하여 데이터베이스와의 CRUD 작업을 처리합니다.

Controller 구현, Service 구현, DTO 정의, Entity 모델 정의, 그리고 Validation 구현의 다섯 가지 주요 부분 확인

```jsx
npm i --save class-validator class-transformer
```

## 1. 포스트 Entity 모델 정의

먼저 Post Entity 모델을 post.entity.ts 파일에 정의

```tsx
export class Post {
  id: number;
  title: string;
  content: string;
  authorId: number;
}
```

## 2. DTO 정의

다음으로 DTO(Data Transfer Object)를 정의. create-post.dto.ts 파일을 생성

```tsx
import { IsNotEmpty, IsString, IsNumber } from 'class-validator';

export class CreatePostDto {
  @IsNotEmpty()
  @IsString()
  title: string;

  @IsNotEmpty()
  @IsString()
  content: string;

  @IsNotEmpty()
  @IsNumber()
  authorId: number;
}
```

update-post.dto.ts 파일 생성

```tsx
import { IsOptional, IsString, IsNumber } from 'class-validator';

export class UpdatePostDto {
  @IsOptional()
  @IsString()
  title?: string;

  @IsOptional()
  @IsString()
  content?: string;

  @IsOptional()
  @IsNumber()
  authorId?: number;
}
```

## 3. 포스트 서비스 구현

post.service.ts 파일에 서비스를 구현

```tsx
import { Injectable } from '@nestjs/common';
import { Post } from './post.entity';
import { CreatePostDto } from './dto/create-post.dto';
import { UpdatePostDto } from './dto/update-post.dto';
import { PostRepository } from './post.repository';

@Injectable()
export class PostService {
  constructor(private readonly postRepository: PostRepository) {}

  async findAll(): Promise<Post[]> {
    return this.postRepository.find();
  }

  async findOne(id: number): Promise<Post | null> {
    return this.postRepository.findOne({ where: { id } });
  }

  async create(createPostDto: CreatePostDto): Promise<Post> {
    const post = this.postRepository.create(createPostDto);
    return this.postRepository.save(post);
  }

  async update(id: number, updatePostDto: UpdatePostDto): Promise<Post | null> {
    await this.postRepository.update(id, updatePostDto);
    return this.postRepository.findOne({ where: { id } });
  }

  async remove(id: number): Promise<void> {
    await this.postRepository.delete(id);
  }
}

```

## 4. 포스트 레포지토리 구현 (임시 코드)

`post.repository.ts`

```tsx
import { Injectable } from '@nestjs/common';
import { Post } from './post.entity';

@Injectable()
export class PostRepository {
  private posts: Post[] = [];
  private idCounter = 1;

  find(): Post[] {
    return this.posts;
  }

  findOne({ where: { id } }: { where: { id: number } }): Post | null {
    return this.posts.find((post) => post.id === id) || null;
  }

  create(model): Post {
    const newPost: Post = { id: this.idCounter++, ...model };
    this.posts.push(newPost);
    return newPost;
  }

  save(post: Post): Post {
    const index = this.posts.findIndex((p) => p.id === post.id);
    if (index !== -1) {
      this.posts[index] = post;
    } else {
      this.posts.push(post);
    }
    return post;
  }

  update(id: number, model): void {
    const index = this.posts.findIndex((post) => post.id === id);
    if (index !== -1) {
      this.posts[index] = { ...this.posts[index], ...model };
    }
  }

  delete(id: number): void {
    this.posts = this.posts.filter((post) => post.id !== id);
  }
}

```

## 5. 포스트 컨트롤러 구현

post.controller.ts 파일에 컨트롤러를 구현

```tsx
import {
  Controller,
  Get,
  Post,
  Put,
  Delete,
  Body,
  Param,
  ValidationPipe,
  ParseIntPipe,
} from '@nestjs/common';
import { PostService } from './post.service';
import { CreatePostDto } from './dto/create-post.dto';
import { UpdatePostDto } from './dto/update-post.dto';

@Controller('posts')
export class PostController {
  constructor(private readonly postService: PostService) {}

  @Get()
  findAll() {
    return this.postService.findAll();
  }

  @Get(':id')
  findOne(@Param('id', ParseIntPipe) id: number) {
    return this.postService.findOne(id);
  }

  @Post()
  create(@Body(ValidationPipe) createPostDto: CreatePostDto) {
    return this.postService.create(createPostDto);
  }

  @Put(':id')
  update(
    @Param('id', ParseIntPipe) id: number,
    @Body(ValidationPipe) updatePostDto: UpdatePostDto,
  ) {
    return this.postService.update(id, updatePostDto);
  }

  @Delete(':id')
  remove(@Param('id', ParseIntPipe) id: number) {
    return this.postService.remove(id);
  }
}

```

## 6. 포스트 모듈 생성

마지막으로 post.module.ts 파일에 모듈을 생성

```tsx
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { PostController } from './post.controller';
import { PostService } from './post.service';
import { PostRepository } from './post.repository';
import { Post } from './post.entity';

@Module({
  // imports: [TypeOrmModule.forFeature([Post])],
  controllers: [PostController],
  providers: [PostService, PostRepository],
})
export class PostModule {}
```

이제 app.module.ts 파일을 업데이트하여 PostModule과 TypeOrmModule을 포함

```tsx
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { PostModule } from './post/post.module';

@Module({
  imports: [
    // TypeOrmModule.forRoot({
    //   type: 'sqlite',
    //   database: 'database.sqlite',
    //   entities: [__dirname + '/**/*.entity{.ts,.js}'],
    //   synchronize: true,
    // }),
    PostModule,
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}

```