Profiler는 React 애플리케이션이 렌더링하는 빈도와 렌더링 “비용”을 측정합니다. Profiler의 목적은 메모이제이션 같은 성능 최적화 방법을 활용할 수 있는 애플리케이션의 느린 부분들을 식별해내는 것입니다.

Profiler 
```jsx
id 
onRender -> callback 

중첩해서 사용 가능 
<Profiler id="Panel" onRender={callback}>
  <Panel {...props}>
    <Profiler id="Content" onRender={callback}>
      <Content {...props} />
    </Profiler>
    <Profiler id="PreviewPane" onRender={callback}>
      <PreviewPane {...props} />
    </Profiler>
  </Panel>
</Profiler>
Profiler는 가벼운 컴포넌트이지만 필요할 때만 사용해야 합니다. 
각 Profiler는 애플리케이션에 조금의 CPU와 메모리 비용을 추가하게 됩니다.


```

### onRenderCallback

```jsx
function onRenderCallback(
  id, // 방금 커밋된 Profiler 트리의 "id"
  phase, // "mount" (트리가 방금 마운트가 된 경우) 혹은 "update"(트리가 리렌더링된 경우)
  actualDuration, // 커밋된 업데이트를 렌더링하는데 걸린 시간
  baseDuration, // 메모이제이션 없이 하위 트리 전체를 렌더링하는데 걸리는 예상시간 
  startTime, // React가 언제 해당 업데이트를 렌더링하기 시작했는지
  commitTime, // React가 해당 업데이트를 언제 커밋했는지
  interactions // 이 업데이트에 해당하는 상호작용들의 집합
) {
  // 렌더링 타이밍을 집합하거나 로그...
}
```


### 잘 모르는 것 
-> . React.memo, useMemo, shouldComponentUpdate