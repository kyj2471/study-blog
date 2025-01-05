## - DB란 무엇인가?

- **데이터베이스(Database, DB)**: 체계적으로 데이터를 저장하고 관리하는 시스템.
- **DBMS(Database Management System)**: 데이터베이스를 관리하는 소프트웨어.

## - DB의 주요 역할

- **데이터 저장**: 데이터를 안전하고 효율적으로 저장합니다.
- **데이터 검색**: 저장된 데이터를 빠르고 정확하게 검색합니다.
- **데이터 관리**: 데이터의 추가, 수정, 삭제를 관리합니다.
- **데이터 무결성**: 데이터의 정확성과 일관성을 유지합니다.
- **데이터 보안**: 데이터에 대한 접근을 제어하고 보호합니다.
- **동시성 제어**: 여러 사용자가 동시에 데이터를 접근하고 수정할 수 있도록 관리합니다.
- **백업 및 복구**: 데이터 손실 시 데이터를 복구할 수 있도록 백업을 관리합니다.

## - DB의 핵심 기능들

### 데이터 정의 기능 (DDL: Data Definition Language)

- **테이블 생성**: 데이터를 저장할 테이블을 정의합니다.
    - 예: `CREATE TABLE users (id INT, name VARCHAR(100));`
- **스키마 변경**: 기존 테이블의 구조를 변경합니다.
    - 예: `ALTER TABLE users ADD COLUMN email VARCHAR(100);`
- **테이블 삭제**: 테이블을 삭제합니다.
    - 예: `DROP TABLE users;`

### 데이터 조작 기능 (DML: Data Manipulation Language)

- **데이터 삽입**: 테이블에 새로운 데이터를 추가합니다.
    - 예: `INSERT INTO users (id, name) VALUES (1, 'John Doe');`
- **데이터 수정**: 테이블의 기존 데이터를 수정합니다.
    - 예: `UPDATE users SET name = 'Jane Doe' WHERE id = 1;`
- **데이터 삭제**: 테이블의 데이터를 삭제합니다.
    - 예: `DELETE FROM users WHERE id = 1;`

### 데이터 조회 기능 (DQL: Data Query Language)

- **데이터 조회**: 테이블에서 데이터를 검색하고 조회합니다.
    - 예: `SELECT * FROM users WHERE id = 1;`

### 데이터 제어 기능 (DCL: Data Control Language)

- **권한 부여**: 사용자에게 데이터베이스 작업에 대한 권한을 부여합니다.
    - 예: `GRANT SELECT ON users TO user1;`
- **권한 회수**: 사용자의 데이터베이스 작업 권한을 회수합니다.
    - 예: `REVOKE SELECT ON users FROM user1;`

### 트랜잭션 제어 기능 (TCL: Transaction Control Language)

- **트랜잭션 시작**: 트랜잭션을 시작합니다.
    - 예: `BEGIN;`
- **트랜잭션 커밋**: 트랜잭션을 커밋하여 변경 내용을 확정합니다.
    - 예: `COMMIT;`
- **트랜잭션 롤백**: 트랜잭션을 롤백하여 변경 내용을 취소합니다.
    - 예: `ROLLBACK;`

## - 요약

- 데이터베이스는 데이터를 저장, 관리, 보호하는 중요한 시스템입니다.
- 데이터 정의, 조작, 조회, 제어, 트랜잭션 관리 등 다양한 기능을 통해 데이터를 효율적으로 관리합니다.
- DBMS를 통해 데이터 무결성, 보안, 동시성 제어, 백업 및 복구 등의 역할을 수행합니다.