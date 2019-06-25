# vanillaJS 정리

Date객체 : 날짜 및 시간을 얻거나 다룰 수 있음
```javascript
//Date(year, month, day) : 선언 날짜 정보 get
//month는 0~11사이 정수이므로 -1
Date(year, month, day).toString().slice(0, 3).toUpperCase(); // 대문자 요일 반환
```

--- 

.propertyName : 해당 속성을 읽거나 쓰거나!
```javascript
element.propertyName.includes('flex') //flex속성을 가진 엘리먼트
```

---

.toggle() : 해당 클래스 Toggle
```javascript
element.classList.toggle('open') //open클래스 Toggle
```

---

.filter() : 콜백함수의 조건에 해당하는 배열을 생성하는 기능   
```javascript
var newArray = arr.filter(callbackFunc(element, index, array), thisArg);
```