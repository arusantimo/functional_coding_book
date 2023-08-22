# 함수형 반복

## 함수형 도구 : map

X 값이 있는 배열을 Y 값이 있는 배열로 변환하는 방법

```javascript
function emailsForCustomers(customers, goods, bests) {
  return map(customers, function (customer) {
    return emailForCustomer(customer, goods, bests);
  });
}
```

### 함수를 전달하는 3가지 방법

1. 전역으로 정의하기

<code>greet</code> 이름으로 어디에서나 사용할 수 있다.

```javascript
function greet(name) {
  return "Hello, " + name;
}
```

2. 지역적으로 정의하기

```javascript
function greetEveryBody(friends) {
  var greeting;
  if (language === "English") {
    greeting = "Hello, ";
  } else {
    greeting = "Salut, ";
  }

  var greet = function (name) {
    return greeting + name;
  };
  return map(friends, greet);
}
```

<code>greet</code>라는 이름을 지역함수 내에서 사용하고, 범위 밖에서는 사용할 수 없다.

3. 인라인으로 정의하기

함수를 사용하는 곳에서 바로 정의.
함수를 변수에 넣지 않기 때문에 이름이 따로 없음.

```javascript
var friendGreetings = map(friendsNames, function (name) {
  return "Hello, " + name;
});
```

## 함수형 도구 : filter

배열에서 일부 항목을 선택하는 함수.

```javascript
function selectBestCustomers(customers) {
  return filter(customers, function (customer) {
    return customer.purchases.length >= 3;
  });
}
```

## 함수형 도구 : reduce

배열을 순회하면서 값을 누적. (값을 누적한다는 것은 추상적인 개념!)

```javascript
function countAllPurchases(customers) {
  return reduce(customers, 0, function (total, customer) {
    return total + customer.purchase.length;
  });
}
```

- Reduce로 할 수 있는 것들

실행 취소/실행 복귀

https://codepen.io/kimwils/pen/zYYmrbO

https://jsfiddle.net/van3L9ew/34/
