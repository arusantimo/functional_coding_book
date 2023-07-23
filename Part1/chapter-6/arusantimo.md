# 6. 불변성 유지하기

핵심 요약

1. 불변 데이터를 읽는 것은 계산이다. -> (바뀌지 않는 것에 대한 믿음.)
2. 불변 데이터는 다루기 쉽고, 부수효과를 일으키지 않는다. 내가 제어한 흐름 안에 있기 때문.


- 실제로 프론트 개발시 객체의 상태를 직접 핸들링 할 일이 많지 않음.
- 서버의 데이터를 변경 하여 작업하는 경우가 더 많음.
- 내부 UI의 상태에 바인딩 된 상태는 변경이 오히려 클라이언트 상태 보단 서버 상태에 바인딩 되어 있는게 더 많음.
- 최소화 시킬수 있음.

```ts
const object = Object.freeze({
  key: 'key',
});

const object2 = {
  key: 'key'
} as const;


// 배열 복사 (얕은 복사)
const list = [].slice();
// 객체 복사 (얕은 복사)
const copy = Object.assign({}, target);
const copy2 = {...target};

// 깊은 복사  
const list2 = JSON.parse(JSON.stringify([]));
const copy3 = JSON.parse(JSON.stringify(target));
// 문제점 
// 비용이 많이듬.
// 참조 오류 날씨 바로 런타임 오류 발생.

// immer , immutableJS 등 라이브러리 사용하는게 가장 비용 적고 안전함.

```


> 실제로 프로그래밍 할때 가장 유용 했던 방법은 복사를 하되 내가 핸들링 하기 편한 구조로 복사하여 사용하면서 함수형으로 짜는게 좋은것 같음.
> 리코일 셀렉터등, 
