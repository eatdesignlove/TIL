###### TIL 20181120

# 자바스크립트 Proxy

- ES6에 새로 추가된 메타프로그래밍(프로그램이 자기 자신을 수정하는 것) 기능
- 객체에 대한 작업을 가로채고, 필요한 경우 작업 자체를 수정



## 적용예

```javascript
const coeffiients = {
    a: 1,
    b: 2,
    c: 5
};

function evaluate(x, co) {
    return co.a + co.b * x + co.c * Math.pow(x,2);
}
```



#### 일부 계수가 빠진 객체일 경우

```javascript
const coefficients = {
    a: 1,
    c: 3
};
evaluate(5, coeffiients) // NaN
```



Coefficients.b에 0을 할당할 수도있으나 프락시를 사용해볼 수 있음

```javascript
const betterCoefficients = new Proxy(coefficients, {
    get(target, key) {
        return target[key] || 0;
    }
})
```

여기서 get함수는 프로퍼티 접근자 get이 아니라 proxy에서 제공하는 일반적인 프로퍼티, 접근자 프로퍼티 모두 사용가능한 핸들러

프로퍼티 값을 할당할 때 set 핸들러로 가로챌 수 있음

```javascript
const cook = {
    name: 'Walt',
    redPhosphorus: 100,
    water: 500
};
const proectedCook = new Proxy(cook, {
    set(target, key, value) {
        if(target.allowDangerousOperations)
            return target.redPhosphorus = value;
        else
            return console.log("Too Dangerous!");
    }
    target[key] = value;
});

protectedCook.water = 550;
protectedCook.redPhosphorus = 150;

projectedCook.allowDangerousOperations = true;
protectedCook.redPhosphorus = 150; 

```





## 보충자료

- http://2ality.com/2014/12/es6-proxies.html
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy