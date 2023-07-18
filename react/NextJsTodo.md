# NextJs Todo 만들기

## 1. 프로젝트 생성

    npx create-next-app todo

    Would you like to use TypeScript? › No / Yes(타입스크립스 사용 할꺼야? yes)
    Would you like to use ESLint? › No / Yes(eslint사용할꺼야? yes)
    Would you like to use Tailwind CSS? › No / Yes(tailwind 사용할꺼야? yes)
    Would you like to use `src/` directory? › No / Yes(src 폴더 사용할꺼야? no)
    Would you like to use App Router? (recommended) › No / Yes(app router사용할꺼야? yes)
    Would you like to customize the default import alias? › No / Yes(기본 import alias를 변경 할꺼야? no)

    cd ../todo
    code .

## 2. 기본 설정

-   /app/page.tsx 수정

    ```TypeScript
    export default function Home() {
        return <div>todo</div>;
    }

    ```
