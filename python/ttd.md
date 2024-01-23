# Today Things Done(오늘 한일)

## 설치

### 가상화 설정

- 프로잭트
- python -m venv ttd_v1
- python -m pip install --upgrade pip

## 설치 패키지

```
pip install fastapi
pip install "uvicorn[standard]"
pip install pytest
pip install pytest-asyncio
pip install httpx
pip install python-dotenv
pip install sqlalchemy
pip install pymysql
```

## 시작

- main.py 생성
  ```Python
    from fastapi import FastAPI
    app = FastAPI()
    @app.get("/hello")
    def hello():
    return {"message": " Hello Python~~"}
  ```
