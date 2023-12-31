# 계층형 설계

## 내용 정리

- 소프트웨어 설계 원칙 (코드를 만들고, 테스트 하고, 유지보수하기 쉽게 미적? 감각으로 설계 하는 것)
- 계층형 설계 페턴 중 직접 설계 패턴 

### 직접 설계 패턴(계층설게)

- 직적 작성한 함수 다이어그램을 통해 설계 진행 (함수끼리 화살표(선)으로 관계도 만들기)
- 관계도를 계층으로 분류
  1. 특정 비지니스 규칙 (특정 상황등) > 서비스 특화
  2. 일반적 비지니스 규칙 (일반적 상황등) > 서비스 특화
  3. 기능의 기본 동작 (게시판의 기능, 장바구니의 기능) > 범용 
  4. 카피온 라이트
  5. 네이티브 기능
- 줌레벨을 통해 문제 해결, 분류
- 화살표의 길이는 일정해야함. (줌 레벨을 통해 해결할 수 있음)


특징
- 특정 로직?등의 기능 단계에 집중 가능
- 호출 그래프를 통해 설계 도움
- 함수를 추출하며 관리 용이
- 일반적인 함수로 분리하고, 일반적인 함수일수 록 관리 재사용하기 편함.
- 복잡성을 감추진 않음.(서로 볼것 못볼것 다 보여줌.)


### 의문점. 

네이티브 기능의 분리가 어떤 장점 혹은 가치가 있는지 모르겠음



### 생각 정리

계층별로 분리된 다이어그램을 작성하면 설계에 도움이 될듯,  
화살표의 길이는 일정해야 하는것도 이해됨. (모든 문제를 같은 레벨로 생각하면서 진행 가능) 하지만 현실적인 것은 아님.

이런 계층 설계 또한 컴포넌트 설계와 비슷한것으로 보임. 



> 클린아키텍쳐 설계 이론에서  
![img.png](img.png)
> 이런식으로 계층을 건너 뛰는 형식을 만들어 내는것이 설계상의 문제라고 한것이 있는데, 그때 이해했던 것과 같이 생각해 보면, 테스트를 하기
> 힘들어 지는것을 포함해서 위에 언급한 문제가 결합되는 것으로 보인다.


