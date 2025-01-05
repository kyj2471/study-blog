## - API 문서란?

### 정의

- **API 문서**: API(Application Programming Interface)의 사용 방법과 기능을 설명하는 문서.
- **목적**: 개발자들이 API를 이해하고 효율적으로 사용할 수 있도록 돕기 위함.

### API 문서의 중요성

- **명확한 커뮤니케이션**: 개발자들 간의 원활한 소통을 가능하게 합니다.
- **개발 효율성 증대**: API 사용 방법을 명확히 이해함으로써 개발 시간을 단축합니다.
- **유지보수 용이**: API 변경 시 문서를 참고하여 빠르게 수정할 수 있습니다.

### API 문서의 구성 요소

- **엔드포인트(Endpoints)**: API가 제공하는 URI(Uniform Resource Identifier).
- **HTTP 메서드**: GET, POST, PUT, DELETE 등 각 엔드포인트에서 지원하는 메서드.
- **요청(Request)**: API를 호출할 때 필요한 파라미터, 헤더, 바디 등의 정보.
- **응답(Response)**: API 호출 결과로 반환되는 데이터 구조, 상태 코드, 예시 응답 등.
- **에러 코드**: API 호출 시 발생할 수 있는 오류 코드와 설명.

## - Swagger 소개

### Swagger란?

- **Swagger**: RESTful API를 설계하고 문서화하기 위한 오픈 소스 도구.
- **목적**: API를 자동으로 문서화하고, 테스트할 수 있는 인터페이스를 제공하여 개발 생산성을 높임.

### Swagger의 주요 기능

- **API 문서화**: 주석이나 설정 파일을 기반으로 자동으로 API 문서를 생성.
- **API 테스트**: 웹 인터페이스를 통해 API를 쉽게 테스트할 수 있음.
- **코드 생성**: 다양한 언어와 플랫폼을 위한 API 클라이언트 코드와 서버 스텁을 생성.
- **API 버전 관리**: API 버전에 따라 문서를 관리하고 업데이트.

### Swagger 구성 요소

- **Swagger Editor**: Swagger 문서를 작성하고 편집할 수 있는 웹 기반 도구.
- **Swagger UI**: Swagger 문서를 시각적으로 표시하고, API를 테스트할 수 있는 웹 인터페이스.
- **Swagger Codegen**: Swagger 문서를 기반으로 API 클라이언트와 서버 코드 생성기.

### Swagger 설치 및 기본 사용법

여기 예시에 대해 좀 더 자세히 설명하겠습니다. 실제로 실행할 수 있는 방법도 함께 안내할게요.

> **1. Swagger 설치**

Swagger는 API 문서화를 쉽게 도와주는 도구입니다. 이 예제에서는 `swagger-ui-express`와 `swagger-jsdoc`을 사용하여 Swagger를 Express.js 서버에서 사용할 수 있도록 합니다.

먼저, 프로젝트 디렉토리에서 `npm install` 명령어를 사용하여 Swagger 관련 패키지를 설치합니다.

```bash
npm install swagger-ui-express swagger-jsdoc

```

> **2. Swagger 설정**

다음 코드는 Express 서버에 Swagger를 설정하는 방법입니다.

```jsx
const express = require('express');
const swaggerUi = require('swagger-ui-express');
const swaggerJsdoc = require('swagger-jsdoc');
const usersRouter = require('./routes/users'); // users 라우터 가져오기

const app = express();

const options = {
    definition: {
        openapi: '3.0.0',
        info: {
            title: 'My API',
            version: '1.0.0',
        },
    },
    apis: ['./routes/*.js'], // routes 폴더의 모든 .js 파일에서 Swagger 주석을 찾음
};

const specs = swaggerJsdoc(options);
app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(specs));

// /users 경로에 대해 users 라우터 사용
app.use('/users', usersRouter);

app.listen(3000, () => {
    console.log('Server is running on <http://localhost:3000>');
});
```

이 설정으로 API 문서를 자동으로 생성하고, `http://localhost:3000/api-docs`에서 Swagger UI를 통해 문서를 볼 수 있습니다.

> **3. Swagger 주석을 이용한 API 문서 작성 예제**

API에 대한 문서를 작성할 때는 Swagger의 주석 형식을 사용합니다. 아래는 `/users` 경로에 대한 `GET` 요청을 정의한 예시입니다.

```jsx
const express = require('express');
const router = express.Router();

/**
 * @swagger
 * /users:
 *   get:
 *     summary: Retrieve a list of users
 *     responses:
 *       200:
 *         description: A list of users
 *         content:
 *           application/json:
 *             schema:
 *               type: array
 *               items:
 *                 type: object
 *                 properties:
 *                   id:
 *                     type: integer
 *                   name:
 *                     type: string
 */
router.get('/', (req, res) => {
    res.json([{ id: 1, name: 'John Doe' }]);
});

module.exports = router;
```

이 코드로 서버에 `/users` 경로가 정의되고, 사용자 목록을 반환하는 API를 사용할 수 있습니다. `/api-docs`에서 이 API에 대한 Swagger 문서를 볼 수 있습니다.

### 실행 방법

1. **Express 앱 생성**: 위 코드를 `app.js` 파일로 저장하세요.
2. **Node.js 실행**: `node app.js` 명령어로 서버를 실행합니다.
3. **Swagger UI 접속**: 웹 브라우저에서 `http://localhost:3000/api-docs`로 접속하면 API 문서를 볼 수 있습니다.

이 방법으로 실제로 Swagger를 설치하고 API 문서를 작성해볼 수 있습니다.

## - 요약

- **API 문서**는 API의 사용 방법과 기능을 설명하며, 개발자들이 API를 효율적으로 활용할 수 있도록 돕습니다.
- **Swagger**는 RESTful API를 설계하고 문서화하기 위한 도구로, API 문서화를 자동화하고 테스트를 용이하게 합니다.
- Swagger를 사용하여 API 문서를 쉽게 생성하고 관리할 수 있으며, 개발 생산성을 높일 수 있습니다.