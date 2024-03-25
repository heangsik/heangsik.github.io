# NODE 추가정보

## NodeMon

- 설치

  npm i -g nodemon

- 사용법

  nodemon --watch src/ src/index.js

  src/ 하위를 모니터링

  src/index.js를 구동함

## Simple Json Server

- npx json-server --port 9999 --watch db.json
- 프로젝트 root에 db.json 파일을 따로 만들어야 함..

## Simple web server

- 설치
  - npm i -g local-web-server
  - g옵션을 글로벌 설치
- 사용
  - npx local-web-server
