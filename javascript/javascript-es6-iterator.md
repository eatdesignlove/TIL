###### TIL 20181114

# 이터레이터/제너레이터

Iterator / Generator

반복하는 주체, 생산하는 주체



## 이터레이터

여러 페이지를 가진 책에 책갈피끼워 어디있는지 파악할 수 있듯이 이터레이터는 순회가능한 객체의 순회지점을 확인 할 수 있는 책갈피 역할을 한다.

```javascript
const books = [
    '개미',
    '아버지들의 아버지',
    '나무'
];
```



### 이터레이터 만들기

```javascript
const it = books.values()
```

순회가능한 객체에 values 메서드를 통해 이터레이터를 만들 수 있다.



### 읽기 시작

```javascript
it.next(); // {value: '개미', done: false}
it.next(); // {value: '아버지들의 아버지', done: false}
it.next(); // {value: '나무', done: false}
it.next(); // {value: undefined, done: true}
```

생성된 이터레이터에 next 메서드를 통해 읽기를 시작할 수 있다. next를 통해 마지막 아이템까지 다다르더라도 계속해서 호출은 가능



### for ...of 흉내내기

```javascript
const it = books.values();
let current = it.next();
while(!current.done) {
    console.log(current.value);
    current = it.next();
}
```

이런 이터레이터의 원리를 이용한 것이 for of 반복문.



### 이터레이터는 독립적이다

```javascript
const it1 = books.values();
const it2 = books.values();

it1.next();
it1.next();

it2.next();
```

이터레이터는 생성될 때마다 처음에서 시작하기 때문에 같은 배열이라도 따로 활용이 가능하다.



### 이터레이션 프로토콜

클래스(Class)에 심볼 메서드 Symbol.iterator가 있고, 이 메서드가 value, done 프로퍼티가 있는 객체를 반환하는 next 메서드를 가진 객체를 반환한다면 해당 클래스의 인스턴스는 이터러블 객체로 본다.

이터레이션 프로토콜을 통해 모든 객체를 이터러블한 객체로 변환이 가능



#### 순회 가능한 로그 만들기

메세지에 타임스탬프를 붙이를 로그 클래스. 타임스탬프가 붙은 메시지는 내부적으로 배열에 저장한다고 할 때 로그를 기록한 항목을 순회할 수 있도록 만들어보자.

##### Log 클래스

```javascript
class Log {
    constructor() {
        this.messages = [];
    }
    add(message) {
        this.messages.push({ message, timestamp: Date.now() });
    } 
}
```



##### 이터러블 객체로 만들기

```javascript
class Log {
    constructor() {
        this.messages = [];
    }
    add(message) {
        this.messages.push({ message, timestamp: Date.now() });
    } 
    [Symbol.iterator]() { // Symbol.iterator 심볼메서드
        return this.messages.values();
       	// value와 done 프로퍼티가 있는 객체를 반환하는 next메서드를 가진 객체 반환
    }
}
```



##### 순회해보기

```javascript
const log = new Log();
log.add('무궁화');
log.add('꽃이');
log.add('피었습니다');

// 로그를 배열처럼 순회
for (let entry of log) {
    console.log(`${entry.messagew} @ ${entry.timestamp}`);
}
```



### 그래서 이터레이터는 무엇?

- 그 자체로 새로운 기능도 아니고 매우 쓸모있진 않지만, 더 다양한 방식으로 컬랙션을 다룰 수 있게하며 배열이나 객체를 통해 순회하는 패턴을 표준화한 것에 의미가 있음. 



## 제너레이터

제너레이터란 이터레이터를 사용해 자신의 실행을 제어하는 함수.

- 언제든 호출자에게 제어권을 넘길 수 있다. (yield)
- 호출된 즉시 실행되는 것이 아니라, 이터레이터를 반환하고 next메서드를 호출함에 따라 실행된다.



### 제너레이터 만들기

```javascript
function* history() {
    yield '삼국시대';
    yield '통일신라시대';
    yield '후삼국시대';
    yield '고려시대';
    yield '조선시대';
    yield '대한제국시대';
    yield '일제시대';
    yield '대한민국';
}
```

- function 뒤에 * 붙여서 만들 수 있다. 

- `yield`키워드를 통해 제어권을 넘길 수 있다.


### 제너레이터 호출하기

```javascript
const it = history();
it.next(); // {value: '삼국시대', done: false}
it.next(); // {value: '통일신라시대', done: false}
it.next(); // {value: '후삼국시대', done: false}
```

이터레이터를 이용함으로 next 메서드를 사용할 수 있다.



### 제너레이터의 몇 가지 특징

#### 양방향 통신과 yield

- 제너레이터와 호출자 사이에는 양방향 통신이 가능하다.

- 양방향 통신은 `yield`표현식을 통해 이뤄진다.

   

#### return

- 제너레이터에서 return문을 사용하면 위치와 관계없이 done은 true가 되고, vlaue프로퍼티는 return이 반환하는 값이 된다.
- 제너레이터가 반환하는 값을 쓸 때는 `yield`를 쓰고 `return`은 제너레이터를 중간에 종료할 때만 사용하자.



### 그래서 이터레이터와 제너레이터는 무엇?

제너레이터는 모든 연산을 지연시켰다 필요할 때만 수행하게 하는 것이 가능하기 때문에, 함수를 호출하는 부분에서 데이터를 제공하고, 호출한 함수가 완료되기 기다렸다 반환값을 받는 기존 사고 방식에 벗어날 수 있는 문을 만들어 줌



