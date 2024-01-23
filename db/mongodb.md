# 몽고디비

## 목차

- [몽고디비](#몽고디비)
  - [목차](#목차)
  - [PODMAN 실행법](#podman-실행법)
  - [데이터 베이스 생성 및 삭제](#데이터-베이스-생성-및-삭제)
  - [Collection 생성](#collection-생성)
  - [계정 생성](#계정-생성)
  - [계정확인](#계정확인)
  - [접속 법](#접속-법)

## PODMAN 실행법

- 옵션으로 아래의 값들을 넣는다.
  ```
  MONGO_INITDB_ROOT_USERNAME=admin
  MONGO_INITDB_ROOT_PASSWORD=password
  ```

## 데이터 베이스 생성 및 삭제

- 계정을 생성전에 계정에서 사용할 데이터 베이스를 만들어야 한다.

  ```
  -- 생성
  use database_name

  -- 삭제(꼭 삭제 하고 싶은 데이터 베이스에서 해야한다.)
  use database_name <-- 꼭 해야 한다.
  db.dropDatabase()
  ```

## Collection 생성

```
db.createCollection("books")
```

## 계정 생성

- dev_user로 계정을 생성
- dev_dada로 데이타 베이스 권한 주기

  ```JSON
      -- root계정으로 로그인
      -- 몽고쉘 실행(MongoShell)
      use admin <-- root계정으로 접속 해야 한다.

      db.createUser({
          user:"dev_user",
          pwd:"XXXXXXXX",
          roles:[{
              role:"readWrite",
              db:"dev_data"
          },
          {
              // 만약 여러개의 디비의 권한을 주길 원하면 배열로 넣는다.
          }]
      });
  ```

- 계정에 디비 권한 추가

  ```JSON
      -- 몽고쉘 실행(MongoShell)
      db.grantRolesToUser("user_id", [ { role: "readWrite", db: "데이터베이스" } ])
  ```

## 계정확인

```
- 몽고쉘
- show users -> 등록된 전체 유저 확인
- db.getUsers() -> 위와 똑같은 결과가 나옴
```

## 접속 법

> mongodb://{id}:{password}@localhost:27017/?authMechanism=DEFAULT
