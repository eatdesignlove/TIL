###### HTML5

# Web Storage

HTML5부터 Web Storage기술을 사용할 수 있는데, Web Storage에는 Local Storage와 Session Storage 두 가지가 존재한다.

Local Storage는 기존의 쿠키보다 많은 양의 데이터를 저장할 수 있는데, 이때 서버와의 통신은 하지 않는다.

Local Storage는 데이터의 유효기간이 없고, 
Session Storage는 브라우저를 닫을 때 데이터가 초기화된다.

## Cookie

##### 단점
- 쿠키는 매번 서버로 전송된다.
- 개수(20개), 용량 제한(4kb)

```javascript
function setCookie(cname, cvalue, exdays) {
    var d = new Date();
    d.setTime(d.getTime() + (exdays*24*60*60*1000));
    var expires = "expires="+d.toUTCString();
    document.cookie = cname + "=" + cvalue + "; " + expires;
}

function getCookie(cname) {
    var name = cname + "=";
    var ca = document.cookie.split(';');
    for(var i=0; i < ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0)==' ') c = c.substring(1);
        if (c.indexOf(name) != -1) return c.substring(name.length,c.length);
    }
    return "";
}
```

## Local Storage

로컬스토리지는 별도의 함수선언 없이 localStorage.항목이름 으로 저장, 불러오기가 모두 가능하다.

##### 장점
- 구조화된 객체를 저장할 수 있다.
- 용량에 제한이 없다.
- 영구 데이터 저장이 가능하다.

```javascript
//cde라는 항목에 345라는 값을 저장
localStorage.cde = "345"

//cde라는 항목에 저장된 값 불러오기
localStorage.cde
```

[WebStorage 참고](http://nubiz.tistory.com/540)
[WebStorage 관련](http://m.mkexdev.net/59)
