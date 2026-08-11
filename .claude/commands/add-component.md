---
description: src/components/ 폴더에 TypeScript + Tailwind CSS 기반 React 함수형 컴포넌트를 생성합니다
argument-hint: <컴포넌트-이름>
---

`$1`이라는 이름의 React 컴포넌트를 생성해줘.

1. `src/components/$1.tsx` 파일을 새로 생성한다. (이미 존재하면 덮어쓰지 말고 알려줘)
2. 아래 템플릿을 기반으로 TypeScript + Tailwind CSS를 사용하는 함수형 컴포넌트를 작성한다.

```tsx
interface $1Props {}

export default function $1({}: $1Props) {
  return (
    <div className="flex items-center justify-center">
      $1
    </div>
  );
}
```

3. `$1`은 파스칼케이스(PascalCase)로 정규화해서 컴포넌트명과 파일명에 사용한다.
4. 생성이 끝나면 파일 경로를 알려준다.
