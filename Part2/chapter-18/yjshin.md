## 18 반응형 아키텍처와 어니언 아키텍처

### 반응형 아키텍처
이벤트에 대한 반응으로 일어날 일을 지정하는 것(이벤트 핸들러)     

### 반응형 아키텍처 사용  
- 원인과 효과가 결합한 것을 분리한다.      
m * n => m + n
- 여러 단계를 파이프라인으로 처리한다.   
JS Promise 를 활용해 약션과 계산을 조합해 파이프 라인을 구현할 수 있다. 
- 타임라인이 유연해진다.

    

>파이프라인 패턴  
>연속적인 데이터의 흐름을 처리하는 일련의 함수들을 나열한 것이 파이프라인 패턴이다.

### 일급상태 ValueCell
Redux Store, Recoil Atom 와 같은 역할 
```javascript
function ValueCell(initialValue) {
    var currentValue = initialValue;
    /*
    * 같은 개념을 가진 이름들
    * watcher/ovserver/listener/callback/eventHandler
    * */
    var watchers = [];
    return {
        val: function() {
            return currentValue;
        },
        update: function(f) {
            var oldValue = currentValue;
            var newValue = f(oldValue);
            if(oldValue !== newValue) {
                currentValue = newValue;
                forEach(watchers, function(watcher) {
                    watcher(newValue);
                });
            }
        },
        // 감시자 기능을 추가해 반응형으로 만든다.
        addWatcher: function(f) {
            watchers.push(f);
        }
    };
}
```

### 파생된 값을 계산하는 FormulaCell
```javascript
function FormulaCell(upstreamCell, f) {
  var myCell = ValueCell(f(upstreamCell.val()));
  upstreamCell.addWatcher(function(newUpstreamValue) {
    myCell.update(function(currentValue) {
      return f(newUpstreamValue);
    });
  });
  // 값을 직접 바꿀 수 없음, upstreamCell(valueCell) 값이 바뀌면 바뀐다.
  return {
    val: myCell.val,
    addWatcher: myCell.addWatcher
  };
}
```


### 사용
```javascript
const shoppingCart = ValueCell({});
const cartTotal = FormulaCell(shoppingCart, calcTotal);

function addItemToCart(name, price) {
    const item = makeCartItem(name, price);
    shoppingCart.update(cart => addItem(cart, item));
}

shoppingCart.addWatcher(updateShippingIcons);
cartTotal.addWatcher(setCartTotalDom);
cartTotal.addWatcher(updateTaxDom);
```

### 어니언 아키텍처
반응형 아키텍처보다 더 넓은 범위에 사용하고, 서비스 전체를 구성하는데 사용한다.        

<img width="250" alt="스크린샷 2023-09-13 오전 9 09 34" src="https://github.com/arusantimo/functional_coding_book/assets/22004468/d65bc71d-e9c2-414a-8adc-c6361a89011e">

>규칙    
>1.현실 세계와 상호작용은 인터렉션 계층에서 해야한다.    
>2.계층에서 호출하는 방향은 바깥 -> 중심 이다.    
>3.계층은 외부에 어떤 계층이 있는지 모른다.     

**계층**
- 인터랙션 계층: 바깥 세상에 영향을 주고 받는 액션
- 도메인 계층: 비즈니스 규칙을 정의하는 계산
> 도메인    
> 프로그래밍으로 문제를 해결하기 위해 만들 소프트웨어 프로그램을 위한 요구사항, 용어, 기능을 정의하는 학문 영역이 도메인 공학이다. - 위키백과
- 언어 계층: 언어 유틸리티와 라이브러리

> 액션: 부수효과, 순수하지 않은 함수      
> 계산: 순수함수, 수학함수        
> 데이터: 이벤트에 대한 사실
    
### 전통적인 계층형 아키텍처와 함수형 아키텍처
전통적인 계층형 아키텍처는 **데이터베이스를 기반**으로 하며, 도메인 계층은 **데이터베이스 동작**으로 만든다. 
그리고 웹 인터페이스는 웹 요청을 도메인 동작으로 변환합니다.

함수형 아키텍쳐는 도메인 계층이 데이터베이스 계층에 의존하지 않는다. 
데이터베이스 동작은 값을 바꾸거나 데이터베이스에 접근하기 때문에 **액션**이다.

<img width="500" alt="스크린샷 2023-09-13 오전 8 49 59" src="https://github.com/arusantimo/functional_coding_book/assets/22004468/8b0b64ef-3e2d-4d32-b8f5-b531c40db6cd">

### 변경과 재사용이 쉬운 아키텍처
어니언 아키텍처는 데이터베이스나 API 호출과 같은 외부 서비스(인터렉션 영역)를 바꾸기 가장 바꾸기 쉽다.    
도메인 계층은 외부 서비스에 의존하지 않아서 테스트하기 좋다.

<img width="774" alt="스크린샷 2023-09-13 오전 9 04 56" src="https://github.com/arusantimo/functional_coding_book/assets/22004468/569ee714-85a9-4ea1-925a-b8c5e17f69b3">

### 도메인 규칙은 도메인 용어를 사용합니다.
도메인을 계산으로 바꾸는건 
액션과 도메인 동작(계산) 동작을 
