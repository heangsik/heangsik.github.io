# 몽고디비

## 목차

-   [몽고디비](#몽고디비)
    -   [목차](#목차)
    -   [1. 계정 생성](#1-계정-생성)

## 1. 계정 생성

-   dev_user로 계정을 생성
-   dev_dada로 데이타 베이스 권한 주기

    ```
        -- root계정으로 로그인
        -- 몽고쉘 실행(MongoShell)
        use admin

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

-   계정에 디비 권한 추가

    ```
        -- 몽고쉘 실행(MongoShell)
        db.grant({
            user:"계정",
            db:"데이터베이스",
            roles: [{
                role: "readWrite",
                db: "데이터베이스"
            }]
        })

    ```
