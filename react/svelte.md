# SVELTE 셋팅

## 목차

- [SVELTE 셋팅](#svelte-셋팅)
  - [목차](#목차)
  - [프로젝트 설치](#프로젝트-설치)
  - [포트 변경](#포트-변경)

## 프로젝트 설치

```
npm create svelte@latest project_name
cd procject name
npm i
npm run dev
```

## 포트 변경

```json
  "scripts": {
    "build": "cross-env NODE_ENV=production webpack",
    "dev": "webpack serve --content-base public --port 5566"
  }
```
