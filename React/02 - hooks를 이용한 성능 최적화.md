# 02. hooks를 이용한 성능 최적화

## 1. useMemo

```javascript
const value = useMemo(() => a * b, [a, b]);
```

- **useMemo**는 의존성 배열 내부의 상태가 변경되지 않았을 경우, 리렌더링 시 **이전의 연산 값**을 사용한다.
- 리렌더링 시의 무지성 값 재연산을 방지하므로 성능 최적화에 용이하다.

## 2. useCallback

```javascript
const add = useCallback(() => a + b, [a, b]);
const value = add();
```

- **useCallback**은 의존성 배열 내부의 상태가 변경되지 않았을 경우, 리렌더링 시 **이전의 함수**를 사용한다.
- 리렌더링 시의 무지성 함수 재생성을 방지하므로 성능 최적화에 용이하다.
- useMemo 기반으로 만들어진 함수임을 알 수 있다.

  ```javascript
  // 위의 예제와 같은 의미이다.
  const value = useMemo(() => () => a + b, [a, b]);
  ```

## 3. React.memo

```javascript
function Component01({...}) {...}
export default React.memo(Component01);
```

- **React.memo**는 컴포넌트의 Props가 변경되지 않았을 경우, 컴포넌트의 리렌더링을 방지한다.
