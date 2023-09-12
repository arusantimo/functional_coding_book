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

**계층**
- 인터랙션 계층: 바깥 세상에 영향을 주고 받는 액션
- 도메인 계층: 비즈니스 규칙을 정의하는 계산
- 언어 계층: 언어 유틸리티와 라이브러리

> 액션: 부수효과, 순수하지 않은 함수      
> 계산: 순수함수, 수학함수        
> 데이터: 이벤트에 대한 사실
    

        


