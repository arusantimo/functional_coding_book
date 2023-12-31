## 🧱 패턴2: 추상화 벽

- 세부 구현은 감춘다.
- 구현을 몰라도 함수만 가져다 사용한다. (라이브러리, API ...)
- 🔄 세부 함수 구현을 바꿔도 가져다 사용하는 상위 함수에는 영향이 없다. (서로 독립적이다. 의존하지 않는다.)
- 📖 상위 함수를 가져다 사용하는 쪽에서 코드를 읽기 쉽다.
- 🔎 주어진 문제에 집중하기 위해:
  - 추가, 삭제, 검색 기능이 있다면, 해당 기능 함수만 가져다 쓰면 되고 구현은 몰라도 된다.
  - 구현에서 어떤 자료구조를 쓰는지, 어떤 알고리즘을 쓰는지 등을 전혀 알 필요가 없다.

### _세부 구현은 감추고, 필요한 부분만 노출하자!_

## 🧩 패턴3: 작은 인터페이스

- 새로운 기능을 만들 때 상위 계층에 만드는 것이 작은 인터페이스 패턴 (215 페이지)
- 상위 계층에 만들 때, 가능한 현재 계층에 있는 함수로 구현해야 한다. (219 페이지)

### _하위 계층의 조합으로 상위 계층 기능을 구현하자!_

## 🏗️ 패턴4: 편리한 계층 (220 페이지)

- 😭 비즈니스 문제를 해결하기 위해 일하고 있는 개발자로서 이처럼 거대한 추상 계층을 만들 시간적 여유는 없습니다. 너무 오래 걸리고 비즈니스는 기다려주지 않습니다.
- 코드를 작성할 때 코드가 편리합니까? 그렇다면 설계를 조금 멈추세요.
- 그러나 너무 많은 구체적인 것을 알아야 합니까? 그렇다면 다시 설계 패턴을 적용해 보세요.

### _적당히 사용할 만 합니까? 그럼 일단 설계(계층화, 추상화)를 멈추고 기능 구현에 집중해봅시다!_

## 📐 비기능적 요구사항

- 유지보수성
- 테스트성
- 재사용성

### 수정하기 쉬운 정도: 상위 계층 > 하위 계층

- 🔄 하위 계층은 이미 많은 상위 계층에서 사용하기 때문에, 수정하려면 상위 계층도 모두 바꿔줘야 합니다.

### 하위 계층 코드들은 테스트가 중요하다.

- ✔️ 하위 계층 코드가 잘 동작해야 상위 계층이 정상적으로 동작하기 때문입니다.
- 🔄 상위 계층 코드는 자주 바뀔 가능성이 있습니다.

### 하위 계층 코드들이 재사용하기 좋다.

- 🔁 상위 계층은 구체적이라 적용 가능한 경우가 많지 않습니다.
- 🎯 하위 계층은 일반화되어 있어 적용 가능한 경우가 많습니다.
