# 더 좋은 액션 만들기

## 1. 원칙) 암묵적 입력과 출력은 적을수록 좋다

- 암묵적 입력/출력이 없는 함수 → 계산
- 모두 계산으로 만들 수 없지만 액션에서 암묵적 입력/출력을 줄이면 테스트하기 쉽다

## 2. 원칙) 설계는 엉켜있는 코드를 푸는 것이다

- 함수는 작을수록 재사용하기 쉽다
- 작은 함수는 이해가 쉽고 유지보수가 쉽다
- 작은 함수는 테스트하기 쉽다.

### 계산을 분류하기

MegaMart 장바구니 코드에서

- Cart(장바구니)에 대한 동작
- Item(단일물품)에 대한 동작
- 비즈니스 규칙
- 배열 유틸리티

로 분류. → 추후에 계층으로 구조를 분리하기 위한 사전작업!

### 연습문제

다음 코드를 하나의 분류에 속하도록 분리하기

```javascript
function update_shipping_icons(cart) {
  var buy_buttons = get_buy_buttons_dom();
  for (var i = 0; i < buy_buttons.length; i++) {
    var button = buy_buttons[i];
    var item = button.item;
    var new_cart = add_item(cart, item);
    if (gets_free_shipping(new_cart)) button.show_free_shipping_icon();
    else button.hide_free_shipping_icon();
  }
}
```

↓

```javascript
function display_free_shipping_icon(button, isFree) {
  if (isFree) {
    button.show_free_shipping_icon();
  } else {
    button.hide_free_shipping_icon();
  }
}

function checkNewItemFreeShipping(cart, item) {
  const new_cart = add_item(cart, item);
  return get_free_shipping(new_cart);
}

function get_item_from_button(button) {
  return button.item;
}

function update_shipping_icos(cart) {
  var buy_buttons = get_buy_buttons_dom();
  for (var i = 0; i < buy_buttons.length; i++) {
    var button = buy_buttons[i];
    const isFreeshipping = checkNewItemFreeshipping(
      cart,
      get_item_from_button(button)
    );
    display_free_shipping_ico(isFreeshipping);
  }
}
```
