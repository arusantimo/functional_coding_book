
## 14. 중첩된 데이터에 함수형 도구 사용하기

### 객체를 다루기 위한 고차 함수 Update

>**고차 함수(Higher order function)**   
함수를 인자로 전달받거나 함수를 결과로 반환하는 함수

```javascript
// modify => callback f
// cbf : increasement, decreasement, double ... do something... 
function update (object, key, cbf) {
    const value = object[key]
    const newValue = cbf(value)
    const newObject = objectSet(object, key, newValue) 
    return newObject
}

// copy on write
function objectSet(object, key, value) {
    var copy = Object.assign({}, object);
    copy[key] = value;
    return copy;
}
```

### 중첩된 객체에서의 문제 발생

```javascript
function increasementSizeByName (cart, name) {
    return update(cart, name, (item) => {
        return update(item, 'options', (options) => {
            return update(options, 'size', (size) => size += 1)
        })
    })
}
```

니즈 발생
1. 중첩된 깊이 상관없이 사용하고싶다.
2. 데이터 구조를 외우고 싶지 않다. 

### 재귀

**안전한 재귀를 사용하는 조건**
1. 종료조건 : 재귀는 스택메모리를 사용하기 때문에 반드시 있어야 한다.  stack overflow 
2. 재귀 호출 : 최소 하나의 재귀 호출을 해야한다. 
3. 종료 조건에 다가가기 : 무한 반복에 빠지지 않도록 종료 조건에 가까워져야 한다.

```javascript
function nestedUpdate(object, keys, modify) {
    if(keys.length === 0)
        return modify(object);
    var key1 = keys[0];
    var restOfKeys = drop_first(keys);
    return update(object, key1, function(value1) {
        return nestedUpdate(value1, restOfKeys, modify);
    });
}
```

### 깊이 중첩된 데이터에 추상화 벽 사용하기
> **추상화 벽**     
> 구현을 감추기 때문에 함수를 쓸 때 어떻게 구현되어 있는지 몰라도 된다.
```javascript
// 12번째 게시글의 글쓴이의 이름을 대문자로 수정하는 함수

// 방법 1
nestedUpdate(blogCatogory, ['posts', '12', 'author', 'name'], capitalize);


// 방법 2 추상화의 벽
updatePostById(blogCatogory, '12', (post) => {
    return updateAuthor(post, capitalizeName)
})

function updatePostById (category, id, cbf) {
    return nestedUpdate(category, ['posts', 'id'], cbf)
}

function updateAuthor (post, modifyUser) { 
    return update(post, 'author', modifyUser)
}

function capitalizeName (user) {
    return update(user, 'name', capitalize)
}
```


### 재귀와 반복문
<img width="600" alt="스크린샷 2023-08-30 오전 9 08 01" src="https://github.com/arusantimo/functional_coding_book/assets/22004468/9295312c-86d4-4dda-89a1-055d430e219e">

### 꼬리재귀
너무 많은 재귀 호출은 메모리 초과 (Stack overflow) 오류를 발생시킬 수 있다.    
이런 단점을 해결해주는 꼬리재귀        
**사용 전 컴파일러가 꼬리재귀 기능을 제공하는지 알고 사용해야한다**

일반 재귀를 사용한 팩토리얼 함수 예시

```javascript
function factorial(n) {
    if (n === 1) {
        return 1;
    }
    return n * factorial(n-1);
}
```

factorial(3) 을 실행했을때 벌어지는 일            
<img width="600" alt="스크린샷 2023-08-30 오전 9 11 06" src="https://github.com/arusantimo/functional_coding_book/assets/22004468/b3cb4194-24a6-4bf1-b81c-4843f0046bb6">

    
```javascript
function factorial(n, total = 1){
    if(n === 1){
        return total;
    }
    return factorial(n - 1, n * total);
}
```
    
<img width="600" alt="스크린샷 2023-08-30 오전 9 12 51" src="https://github.com/arusantimo/functional_coding_book/assets/22004468/aa401230-3126-44ec-9096-879c57a64565">



