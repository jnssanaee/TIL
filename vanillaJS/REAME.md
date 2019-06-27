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

.filter() : 콜백함수의 조건에 해당하는 배열을 생성하는 메서드
```javascript
var newArray = arr.filter(callbackFunc(element, index, array), thisArg);
//thisArg는 filter에서 사용될 this, 선언하지 않을 시 undefined 전달
```

---

Array.prototype.some() : callback이 어떤 배열 요소라도 참인(truthy) 값을 반환하는 경우 true  
> 빈 배열에서 호출 시 false 반환

```javascript
//구문
arr.some(callback[, thisArg])

//예제
var array = [1, 2, 3, 4, 5];

var even = function(element) {
  return element % 2 === 0;
};
array.some(even) // true
```

--- 

Array.prototype.every() : callback이 모든 배열 요소가 참인(truthy) 값을 반환하는 경우 true  
> 빈 배열에서 호출 시 true 반환  

```javascript
//구문
arr.every(callback[, thisArg])

//예제 1
[12, 5, 8, 130, 44].every(elem => elem >= 10); // false

//예제 2
function isBigEnough(element, index, array) { // element:처리할 현재 요소, index, 현재 요소 인덱스, array: every를 호출한 배열
  return element >= 10;
}
[12, 5, 8, 130, 44].every(isBigEnough); // false
```

--- 

Array.prototype.findIndex() : 주어진 판별 함수를 만족하는 배열의 첫 번째 요소에 대한 인덱스를 반환
> 만족하는 요소가 없으면 -1을 반환

```javascript
//구문
arr.findIndex(callback[, thisArg])
```

--- 

Array.prototype.slice() : 어떤 배열의 begin부터 end까지(end 미포함)에 대한 얕은 복사본을 새로운 배열 객체로 반환
> 원본 배열은 수정되지 않음

```javascript
//구문
arr.slice([begin[, end]])

//예제1
var animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];

animals.slice(2); // ["camel", "duck", "elephant"]

animals.slice(2, 4); // ["camel", "duck"]
```