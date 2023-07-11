# 액션과 계산, 데이터의 차이를 알기. (7월 12일)

## 장보기 과정에서 분류

만약 우리 실제 개발 과정에서 분류를 하기 위해서는 어떻게 해야 할까?  
1. 우리가 어떤 기능을 만들때에도 이야기 흐름을 작성한다.
2. 액션으로 구성된 기능의 흐름을 시각화 해본다.
3. 액션에서 데이터와 계산을 찾아 본다.
4. 우선 액션에서 사용되는 데이터 부터 찾아서 빼본다.
5. 데이터를 조회 조작 변경 하는 계산을 찾아본다. 계산은 보통 파이프 라인으로 연결되거나, 계층화 해서 쪼갤수 있다.
6. 다시 시각화 해서 의존적인 부분을 확인하고, 시간에 의존적인 부분은 비동기 처리하는 것까지 시각화 해본다.

## 쿠폰북 보내기 

내가 생각한 액션스토리보드
1. 구폰 데이터 베이스 조회
2. 쿠폰 종류 수량 확인하기
3. 이메일 데이터 베이스 조회
4. 이메일 보내기 위한 추천 수에 맞는 쿠폰 발송 데이터 구조 만들기
5. 데이터 베이스를 통해 사용자 에게 이메일 발송
   
<img width="250" alt="image" src="https://github.com/arusantimo/functional_coding_book/assets/4675672/0c6a6d04-9727-4f4e-a5dd-211741ffe833">


데이터를 조회 할때 쿠폰을 보내야 하는 유저로 필터링 하는 쿼리 과정이 있으면 불필요한 계산이 생략될수 있다.  


## 액션과 계산과 데이터를 기존 코드 로직에서 분리하는 방법

1. 다루는 데이터를 분류한다.
2. 유닛 테스트가 깔끔하게 되는 함수인지 판단하고, 그렇지 않으면 액션으로 생각한다.
3. 액션 안에서 유닛 테스트 할수 있는 깔끔한 순수함수(계산)을 빼낼수 있는지 확인해 본다.
4. 데이터는 기록의 상태이기 때문에 순수함수로 빼낼수 없는 형태이면, 기록의 상태로 즉 데이터로 액션을 쪼갤수 있다.

```js
import "./styles.css";

document.getElementById("app").innerHTML = `
<h1>현재 시각 우리 동네 날씨 알기.</h1>
`;

const sendWeather = () =>
  new Promise((resolve, reject) => {
    setTimeout(function () {
      resolve("비가 억수로 옴 ");
    }, 250);
  });

const sendLocation = () =>
  new Promise((resolve, reject) => {
    setTimeout(function () {
      resolve("1001 ");
    }, 250);
  });

// 1. 우리동네 주소 요청을 위한 정보 알기
// 2. 주소 정보로 기상청 데이터 요청
// 3. 받아온 데이터를 화면에 표현

async function legacy_getLoactionInfo(addr) {
  const addrNum = addr.split(" ")[3];
  const enableAddrList = ["911-99", "911-10"];
  const enable = enableAddrList.includes(addrNum);
  if (enable) {
    const location = await sendLocation(addrNum);
    return location;
  }
  return null;
}

async function getWeather(location) {
  const weather = await sendWeather(location);
  return weather;
}

async function legacy_renderWeather(addr) {
  document.getElementById("app").append("우리 동네 날씨는?");
  const location = await legacy_getLoactionInfo(addr);
  const weather = await getWeather(location);
  document.getElementById("app").append(weather);
}

// legacy_renderWeather("서울시 관악구 관악로 911-99");

// 새로 짜보기 *(데이터(기록) 순수함수 더 사용해서)

// 데이터
const outputText = {
  myLocation: "",
  wearther: ""
}; // 타입 스크립트면 더 좋음.

// 순수 함수
const getAddrNum = (addr) => addr.split(" ")[3];
const checkEnable = (addrNum, dic) => dic.includes(addrNum);
const getWeaterText = (fullAddr, textObj) =>
  `${fullAddr} :: ${textObj.myLocation} : ${textObj.wearther}`;

// 액션
async function getLoactionInfo(addrNum) {
  const location = await sendLocation(addrNum);
  return location;
}

async function renderWeather(addr) {
  const addrNum = getAddrNum(addr);

  outputText.myLocation = checkEnable(addrNum, ["911-99", "911-10"])
    ? await getLoactionInfo(addrNum)
    : "알수 없음";
  outputText.wearther = await getWeather(outputText.myLocation);

  const text = getWeaterText(addr, outputText);

  document.getElementById("app").append(text);
}

renderWeather("서울시 관악구 관악로 911-99");

```


코드를 분리하면서 느낀점은 결국 액션에 남는건 해당 서비스 도메인에 의존적인 로직만 남는것으로 보임.  
바뀌기 쉽고, 유지보수 빈도가 높은것은 서비스 도메인 로직이므로 유지보수 하기 편해질것 임.
