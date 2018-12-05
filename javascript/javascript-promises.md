###### TIL 20181204

# Promise

- 콜백 패턴이 갖는 기존 비동기 프로그래밍의 단점을 보완하여 만들어진 패턴.
- 비동기 처리 로직의 추상화한 객체, 그것을 조작하는 방식.
- E언어에서 처음 고안된 개념. 



## API

```javascript
// 1. Constructor
var promise = new Promise((resolve, reject) => {
   // 비동기 처리 작성
   // resolve or reject
});

// 2.Instance Method
promise
    .then(onFulfilled, onRejected)
	.catch(onRejected)

// 3.Static Method
Promise.all()
Promise.resolve()
Promise.reject()
Promise.then
Promise.prototype.catch
Promise.all
Promise.race

// 4.State
// "has-resolution" - Fulfilled => onFulfilled
// "has-rejection" - Rejected => onRejected
// "unresolved" - Pending => Promise 객체가 생성된 초기 상태

// Setteld(불변) - 성공 또는 실패 했을 때의 상태
```



## Promise의 특징

- 로직에 관계없이 항상 비동기로 처리됨 
- 새로운 Promise 객체를 반환하는 then
- 예외처리가 되지 않는 onRejected
- 콜백-헬의 해결 🙅‍♂️ 완화 👌



#### 비동기 콜백을 절대 동기적으로 호출하지 마라

- 데이터를 즉시 사용할 수 잇어도, 절대 비동기 콜백을 동기적으로 호출하지 마라.
- 비동기 콜백을 동기적으로 호출하면 기대한 연산의 순서를 방해하고, 예상치 않은 코드의 간섭을 초래할 수 있다.
- 비동키 콜백을 동기적으로 호출하면 스택 오버플로우나 처리되지 않는 예외를 초래할 수 있다.
- setTimeout같은 비동기 API를 통해 비동기 콜백을 스케줄링하라.

**<effective javascript>, 데이비드 허먼**





## Promise의 테스트





## Promise 고급





## 궁금증

- Promises/A+와 ES6 Promises는 무슨 관계일까. 









## 참고자료

- https://developers.google.com/web/fundamentals/primers/promises?hl=ko
- https://en.wikipedia.org/wiki/E_(programming_language)