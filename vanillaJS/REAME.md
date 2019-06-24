# vanillaJS 정리

Date객체 : 날짜 및 시간을 얻거나 다룰 수 있음
> Date(year, month, day) : 선언 날짜 정보 get,  
> month는 0~11사이 정수이므로 -1
```javascript
Date(year, month, day).toString().slice(0, 3).toUpperCase(); // 대문자 요일 반환
```

--- 