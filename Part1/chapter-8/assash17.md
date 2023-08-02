### 계층형 설계
 - 함수는 바로 아래 계층 함수를 이용하여 만든다.
 - 계층은 어떻게 잘 구분해?

```javascript
// Page 172

/// Extracted

function freeTieClip(cart) {
  var hasTie = isInCart(cart, "tie");
  var hasTieClip = isInCart(cart, "tie clip");
  if(hasTie && !hasTieClip) {
    var tieClip = make_item("tie clip", 0);
    return add_item(cart, tieClip);
  }
  return cart;
}

function isInCart(cart, name) {
  for(var i = 0; i < cart.length; i++) {
    if(cart[i].name === name)
      return true;
  }
  return false;
}
```

반복문을 2번 돌지만 2중 for문이 아닌이상 상관없다?

### 직접 구현
 - 같은 추상화 단계에 있는 함수를 사용해야 한다.

과연 어디까지 일반적인 함수를 만들어야 하는가? 더 복잡해질 수 있다.
