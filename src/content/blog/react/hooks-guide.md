---
title: 'React Hooks 완벽 가이드'
description: 'useState, useEffect부터 커스텀 훅까지'
pubDate: 'Jan 06 2026'
heroImage: '../../../assets/blog-placeholder-4.jpg'
---

React Hooks는 함수형 컴포넌트에서 상태와 라이프사이클 기능을 사용할 수 있게 해주는 강력한 도구입니다.

## useState - 상태 관리

```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}
```

## useEffect - 사이드 이펙트

```jsx
import { useState, useEffect } from 'react';

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    fetchUser(userId).then(setUser);
  }, [userId]); // userId가 변경될 때만 실행

  if (!user) return <div>Loading...</div>;
  return <div>{user.name}</div>;
}
```

## 커스텀 훅

```jsx
function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    const saved = localStorage.getItem(key);
    return saved ? JSON.parse(saved) : initialValue;
  });

  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);

  return [value, setValue];
}

// 사용
function App() {
  const [name, setName] = useLocalStorage('name', '');
  // ...
}
```

## 주요 Hooks

- **useState**: 상태 관리
- **useEffect**: 사이드 이펙트
- **useContext**: Context API 사용
- **useReducer**: 복잡한 상태 로직
- **useMemo**: 값 메모이제이션
- **useCallback**: 함수 메모이제이션
- **useRef**: DOM 참조 또는 값 보관

## Best Practices

1. 훅은 최상위에서만 호출
2. React 함수 내에서만 호출
3. 의존성 배열 정확히 명시
4. 커스텀 훅으로 로직 재사용

## 다음 학습 주제

- useReducer vs useState
- React Query로 서버 상태 관리
- 성능 최적화 패턴
