## - CORS란?

### 개념

- **CORS (Cross-Origin Resource Sharing)**: 웹 애플리케이션에서 브라우저가 서로 다른 출처(도메인, 프로토콜, 포트 등)에서 자원을 요청할 때, 서버가 이를 허용하거나 거부하는 메커니즘입니다.
- 브라우저는 보안을 위해 기본적으로 다른 출처에서의 요청을 제한합니다. CORS는 이러한 제약을 완화하여 특정 출처에서의 요청을 허용합니다.

### 주요 헤더

- **Access-Control-Allow-Origin**: 클라이언트가 요청할 수 있는 출처를 지정합니다.
- **Access-Control-Allow-Methods**: 허용된 HTTP 메서드를 지정합니다 (GET, POST, PUT 등).
- **Access-Control-Allow-Headers**: 허용된 HTTP 헤더를 지정합니다 (Content-Type, Authorization 등).
- **Access-Control-Allow-Credentials**: 쿠키나 인증 정보를 허용할지 여부를 설정합니다.

### 예시

브라우저에서 A 도메인([A.com](http://a.com/))에서 B 도메인([B.com](http://b.com/))으로 요청을 보내면, B 도메인에서 `Access-Control-Allow-Origin: A.com` 헤더를 반환해야 A 도메인에서 응답을 받을 수 있습니다.

## - CORS 문제 발생 상황

### 1. 문제의 원인

- **출처 불일치**: 클라이언트와 서버가 다른 출처일 때 발생할 수 있습니다.
- **잘못된 CORS 설정**: 서버가 올바르게 CORS를 설정하지 않은 경우 문제가 발생할 수 있습니다.

### 2. CORS 오류 메시지 예시

- `Access to XMLHttpRequest at '<http://example.com>' from origin '<http://localhost:3000>' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.`

## - NestJS에서 CORS 설정

### 1. NestJS CORS 설정 방법

**1. 기본 CORS 설정**

- `main.ts` 파일에서 CORS를 활성화합니다.

```tsx
// src/main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // CORS 설정
  app.enableCors({
    origin: '<http://localhost:3000>', // 클라이언트의 출처
    methods: 'GET,POST,PUT,DELETE', // 허용할 HTTP 메서드
    allowedHeaders: 'Content-Type, Authorization', // 허용할 헤더
    credentials: true, // 쿠키나 인증 정보 허용
  });

  await app.listen(3000);
}
bootstrap();

```

**2. CORS 설정을 환경에 따라 조정**

- **개발 환경**: 모든 출처에서의 요청을 허용할 수 있습니다. (`origin: '*'`)

```tsx
app.enableCors({
  origin: '*',
});

```

- **배포 환경**: 특정 출처만 허용합니다.

```tsx
app.enableCors({
  origin: '<https://your-frontend-domain.com>',
});

```

### 2. CORS 문제 해결

**1. 올바른 CORS 설정 확인**

- 서버의 CORS 설정이 클라이언트의 출처와 일치하는지 확인합니다.

**2. 네트워크 탭 및 콘솔 로그 확인**

- 브라우저의 개발자 도구에서 네트워크 탭과 콘솔 로그를 통해 CORS 관련 오류를 확인합니다.

**3. 프록시 서버 사용**

- 개발 중에 프록시 서버를 사용하여 CORS 문제를 우회할 수 있습니다. 예를 들어, `http-proxy-middleware`를 사용하여 로컬 개발 환경에서 CORS 문제를 피할 수 있습니다.

**4. CORS 미들웨어 활용**

- 다른 미들웨어나 라이브러리를 사용하여 CORS 문제를 해결할 수도 있습니다.

## - 요약

- **CORS**는 클라이언트와 서버가 다른 출처에서 상호작용할 때 보안상의 제약을 처리하는 메커니즘입니다.
- **CORS 설정**을 올바르게 구성하면 클라이언트와 서버 간의 요청을 성공적으로 처리할 수 있습니다.
- **NestJS**에서는 `app.enableCors()` 메서드를 사용하여 CORS를 설정하고, 다양한 옵션을 통해 세부 설정을 조정할 수 있습니다.
- **CORS 오류**가 발생하면 서버의 CORS 설정을 확인하고, 개발자 도구를 사용하여 문제를 파악하고 해결합니다.