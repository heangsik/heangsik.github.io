# Python Project Initialization

- [Python Project Initialization](#python-project-initialization)
  - [1. 프로젝트 생성](#1-프로젝트-생성)
  - [2. pip업데이트](#2-pip업데이트)
  - [3. Streamlit](#3-streamlit)
    - [설치](#설치)
    - [구동](#구동)
  - [4. FastAPI](#4-fastapi)
    - [설치](#설치-1)
    - [실행](#실행)
  - [logger](#logger)
    - [설치](#설치-2)
    - [사용](#사용)

## 1. 프로젝트 생성

- 희망하는 위치에 프로젝트 디렉토리를 만든다.

  ```cmd
    c:\>mkdir project_1(enter)
    c:\>cd project_1(enter)
    c:\project_1>
  ```

- python 가상환경 만들기

  > c:\project_1>python -m venv pro_api

  - python -m venv -> 파이썬 모듈 중 venv를 사용한다.
  - pro_api -> 생성할 가상환경 이름
  - 진행 후 pro_api디렉토리가 생성된다.

  ```cmd
    c:\project_1> cd pro_api\Script(enter)
    c:\project_1\pro_api\Script> activate(enter)
    (pro_api) PS c:\project_1\pro_api\Script
  ```

  - **activate** -> 가상환경 사용 시작
  - 만약 가상환경에서 나오기 위해서는 **deactivate**를 입력 하면 된다.

## 2. pip업데이트

```
python -m pip install --upgrade pip
```

## 3. Streamlit

### 설치

> pip install streamlit

### 구동

> streamlit run myfile.py

## 4. FastAPI

### 설치

> pip install fastapi
> pip install "uvicorn[standard]"

### 실행

> uvicorn main:app --reload --port=80

## logger

### 설치

> pip install loguru

### 사용

> from loguru import logger
> logger.infl("message")
