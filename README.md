# 📊 React-Table 라이브러리로 만든 예제 파일.
:octocat: 바로 가기 : https://light9639.github.io/React-Table-TypeScript/

![light9639 github io_React-Table-TypeScript_](https://user-images.githubusercontent.com/95972251/229794207-d786225b-4915-4836-971f-3aef31f0dcf0.png)

:sparkles: 📊 React-Table 라이브러리로 만든 예제 파일. :sparkles:
## :tada: React 프로젝트 생성
- React 생성
```bash
npm create-react-app my-app
# or
yarn create react-app my-app
```

- vite를 이용하여 프로젝트를 생성하려면
```bash
npm create vite@latest
# or
yarn create vite
```
- 터미널에서 실행 후 프로젝트 이름 만든 후 React 선택, Typescirpt-SWC 선택하면 생성 완료.
## 🛰️ react-table, sass 라이브러리 설치
- react-table, sass 라이브러리 설치하기
```bash
$ npm install react-table sass
# or
$ yarn add react-table sass
```
## ✒️ App.tsx, App.scss, MOCK_DATA.json 수정 및 작성
### ⚡ MOCK_DATA.json
- 완성본과 똑같이 만들고 싶다면, 현재 폴더에 있는 `MOCK_DATA.json`의 자료를 다운 받아 사용하면 된다. 
- 만약 다른 자료를 이용하고 싶다면 `mockup data` 생성 사이트에서 데이터를 생성한 데이터로 작성하면 된다.
- 더미데이터를 만들고 싶다면 <a href="https://wooncloud.tistory.com/68">더미데이터 만들기 글</a>을 참고하여 <a href="https://www.mockaroo.com/">mockaroo</a>사이트에서 자료를 생성할 수 있다.
### ⚡ App.tsx
- `useMemo`를 사용하여 데이터가 변경될 때만 사용하도록 만듬.
- `react-table` 라이브러리를 이용하여 `MOCK_DATA.json`의 자료들을 가져와 테이블에 자료를 넣는다.
```typescript
import "./App.scss";
import fakeData from "./MOCK_DATA.json";
import * as React from "react";
import { useTable } from "react-table";

export default function App(): JSX.Element {
  const data = React.useMemo(() => fakeData, []);
  const columns = React.useMemo(
    () => [
      {
        Header: "ID",
        accessor: "id",
      },
      {
        Header: "First Name",
        accessor: "first_name",
      },
      {
        Header: "Last Name",
        accessor: "last_name",
      },
      {
        Header: "Email",
        accessor: "email",
      },
      {
        Header: "Gender",
        accessor: "gender",
      },
      {
        Header: "University",
        accessor: "university",
      },
    ],
    []
  );

  const { getTableProps, getTableBodyProps, headerGroups, rows, prepareRow } = useTable({ columns, data });

  return (
    <div className="App">
      <div className="Title">
        <img src="https://react-table-v7.tanstack.com/_next/static/images/logo-light-66d4dd9109004332c863391e6d1cb309.svg" alt="React_table" />
      </div>
      <div className="container">
        <table {...getTableProps()}>
          <thead>
            {headerGroups.map((headerGroup: { getHeaderGroupProps: () => JSX.IntrinsicAttributes & React.ClassAttributes<HTMLTableRowElement> & React.HTMLAttributes<HTMLTableRowElement>; headers: any[]; }) => (
              <tr {...headerGroup.getHeaderGroupProps()}>
                {headerGroup.headers.map((column) => (
                  <th {...column.getHeaderProps()}>
                    {column.render("Header")}
                  </th>
                ))}
              </tr>
            ))}
          </thead>
          <tbody {...getTableBodyProps()}>
            {rows.map((row: { getRowProps: () => JSX.IntrinsicAttributes & React.ClassAttributes<HTMLTableRowElement> & React.HTMLAttributes<HTMLTableRowElement>; cells: any[]; }) => {
              prepareRow(row);
              return (
                <tr {...row.getRowProps()}>
                  {row.cells.map((cell) => (
                    <td {...cell.getCellProps()}> {cell.render("Cell")} </td>
                  ))}
                </tr>
              );
            })}
          </tbody>
        </table>
      </div>
    </div>
  );
}
```
### ⚡ App.scss
- `@include`를 이용하여 반응형 작업을 수월하게 제작하였음.
- `@keyframes` 애니메이션을 사용하여 배경화면의 색이 변하도록 작성함.
- `hover`시에 테이블의 투명도가 변하도록 스타일링.
```scss
@mixin response {
  @media (max-width: 1200px) {
    @content;
  }
}

html,
body {
  height: 100%;
}

body {
  margin: 0;
  background: linear-gradient(-45deg, #ee7752, #e73c7e, #23a6d5, #23d5ab);
  background-size: 400% 400%;
  animation: gradient 15s ease infinite;
  height: 100vh;
  font-family: sans-serif;
  font-weight: 100;
}

@keyframes gradient {
  0% {
    background-position: 0% 50%;
  }

  50% {
    background-position: 100% 50%;
  }

  100% {
    background-position: 0% 50%;
  }
}

.container {
  position: absolute;
  top: 50%;
  left: 50%;
  max-height: 600px;
  overflow-y: scroll;
  transform: translate(-50%, -50%);
  margin: 0 auto;

  @include response {
    max-width: 95%;
    max-height: 80%;
    transform: translate(-50%, -40%);
  }

  &::-webkit-scrollbar {
    width: 12px;
  }

  &::-webkit-scrollbar-thumb {
    height: 25%;
    background: linear-gradient(-45deg, #ee7752, #e73c7e, #23a6d5, #23d5ab);
    border-radius: 8px;
  }

  &::-webkit-scrollbar-track {
    background-color: rgba(255, 255, 255, 0.2);
    border-radius: 0 5px 5px 0;
  }
}

.Title {
  position: absolute;
  top: 0%;
  left: 50%;
  transform: translate(-60%, 50%);
  width: 15rem;
  margin: 0px auto;

  @include response {
    width: 12.5rem;
    transform: translate(-52.5%, 50%);
  }

  img {
    width: 100%;
  }
}

table {
  width: 800px;
  height: 800px;
  border-collapse: collapse;
  overflow: hidden;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
  border-radius: 5px;
}

th,
td {
  padding: 15px;
  background-color: rgba(255, 255, 255, 0.2);
  color: #fff;
  text-align: center;

}

thead th {
  background-color: #55608f;
}

tbody {
  & tr:hover {
    background-color: rgba(255, 255, 255, 0.3);
  }

  & td {
    position: relative;

    &:hover:before {
      content: "";
      position: absolute;
      left: 0;
      right: 0;
      top: -9999px;
      bottom: -9999px;
      background-color: rgba(255, 255, 255, 0.2);
      z-index: -1;
    }
  }
}
```
