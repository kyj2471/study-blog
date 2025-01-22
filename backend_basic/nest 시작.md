
## 프로젝트 생성하기

### 1. NestJS CLI 설치

NestJS CLI는 NestJS 프로젝트를 생성하고 관리하는 데 유용한 도구입니다. 먼저 NestJS CLI를 전역으로 설치합니다.

```bash
npm install -g @nestjs/cli
```

설치가 완료되면 CLI의 버전을 확인하여 설치가 제대로 되었는지 확인합니다.

```bash
nest --version
```

### 2. 새로운 NestJS 프로젝트 생성

다음 명령어를 사용하여 새로운 NestJS 프로젝트를 생성합니다. `my-project`는 프로젝트의 이름으로, 원하는 이름으로 변경할 수 있습니다.

```bash
nest new my-project
```

이 명령어를 실행하면, CLI가 몇 가지 설정을 묻습니다. 예를 들어, 패키지 매니저를 선택하라는 메시지가 나타날 수 있습니다. `npm` 또는 `yarn` 중에서 선택하면 됩니다.

<aside> 💡

## Tip. 커맨드 라인에서 VSCode 여는 방법

> Cmd(윈도우는 Ctrl) + Shift + P 입력 후 Shell Command install 진행

![Screenshot 2024-07-21 at 2.50.25 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/d26625f7-ed50-4b2e-9145-8266d5906d5d/ac5aa9e1-55f6-4cfd-93d6-f83f74be73a5/Screenshot_2024-07-21_at_2.50.25_PM.png)

</aside>

### 프로젝트 구조 이해하기

프로젝트가 생성되면, 다음과 같은 기본적인 폴더 구조를 확인할 수 있습니다:

```
my-project/
|-- src/
|   |-- app.controller.ts
|   |-- app.module.ts
|   |-- app.service.ts
|-- test/
|-- node_modules/
|-- .gitignore
|-- package.json
|-- tsconfig.json
|-- README.md

```

- **`src/` 폴더**: 애플리케이션의 소스 코드가 위치합니다.
    - **`app.controller.ts`**: 기본 컨트롤러 파일로, HTTP 요청을 처리합니다.
    - **`app.module.ts`**: 애플리케이션의 루트 모듈을 정의합니다. 다른 모듈들을 가져오고 설정합니다.
    - **`app.service.ts`**: 비즈니스 로직을 구현하는 서비스 파일입니다.
- **`test/` 폴더**: 유닛 테스트 및 e2e 테스트를 위한 기본 템플릿이 포함되어 있습니다.
- **`node_modules/` 폴더**: 프로젝트의 의존성 모듈이 설치되는 폴더입니다.
- **`.gitignore`**: Git 버전 관리에서 제외할 파일 및 폴더를 정의합니다.
- **`package.json`**: 프로젝트의 의존성, 스크립트, 메타 정보를 정의합니다.
- **`tsconfig.json`**: TypeScript 컴파일러 옵션을 설정합니다.
- **`README.md`**: 프로젝트에 대한 정보를 설명하는 파일입니다.

### 프로젝트 실행하기

프로젝트 생성이 완료되면, 생성된 디렉토리로 이동하여 애플리케이션을 실행합니다.

```bash
cd my-project
npm run start

```

애플리케이션이 실행되면, 기본적으로 [](http://localhost:3000/)[http://localhost:3000](http://localhost:3000)에서 애플리케이션을 확인할 수 있습니다. 브라우저에서 해당 URL을 열어 "Hello World!" 메시지를 확인해 보세요.

### 개발 모드에서 실행하기

개발 모드에서 애플리케이션을 실행하면, 코드 변경 시 자동으로 애플리케이션이 재시작됩니다.

```bash
npm run start:dev

```

이 명령어를 실행하면, 변경 사항이 실시간으로 반영되므로 개발 작업이 더욱 효율적입니다.

### 요약

- **NestJS CLI 설치**: `npm install -g @nestjs/cli` 명령어로 NestJS CLI를 설치합니다.
- **프로젝트 생성**: `nest new my-project` 명령어로 새로운 NestJS 프로젝트를 생성합니다.
- **프로젝트 구조**: 생성된 프로젝트의 기본 폴더와 파일 구조를 이해합니다.
- **애플리케이션 실행**: `npm run start` 명령어로 애플리케이션을 실행하고, `npm run start:dev`로 개발 모드에서 실행합니다.