
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