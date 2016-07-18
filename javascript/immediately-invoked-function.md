###### javascript

# 즉시 실행 함수 Immediately-invoked function

## 개념 
**익명함수를 응용한 형태**로 한 번 수행한 뒤, 다시 호출할 수 없다. 최초의 한 번만 실행을 필요로 하기 때문에 초기화 코드로 많이 사용된다. 글로벌 네임스페이스에 변수를 추가하지 않아도 되기 때문에 코드 충돌이 없이 구현할 수 있어 **플러그인이나 라이브러리 등을 만들 때 많이 사용**된다.

## 예제 ( Toggle-Baseline.js )
```javascript
(function(document){
  // 사용자가 Shift + G 키를 누르면
  // 토글 베이스라인이 설정된다.
  document.addEventListener('keydown', function(event) {
    var shift = event.shiftKey;
    var key = event.keyCode;
    if(shift && key === 71) {
      this.body.classList.toggle('show-baseline');
    }
  });
})(this.document);
```

## 참고자료
>- [Javascript : 함수(function) 다시 보기](http://www.nextree.co.kr/p4150/)
- [즉시 실행 함수](http://codedragon.tistory.com/199)
