###### TIL 20181120

# 자바스크립트 객체 프로퍼티

객체 프로퍼티는 크게 세 종류로 구분

- 데이터 프로퍼티
- 접근자 프로퍼티(동적 프로퍼티)
- 함수 프로퍼티(메서드)



## 접근자 프로퍼티

자연스러운 문법을 사용하면서, 부주의한 접근을 차단하는 장점

- setter
- getter



## 객체 프로퍼티

- 식별자를 사용하는 일반적인 프로퍼티
- 심볼, 표현식 사용하는 계산된 프로퍼티
- 메서드 단축 표기



### Object.getOwnPropertyDescriptor

getOwnPropertyDescriptor로 객체의 프로퍼티 속성을 확인할 수 있음



- writable - 프로퍼티 값을 바꿀 수 있는지 아닌지 판단
- enumerable - for..in / Object.keys, 확산 연산자에서 객체프로퍼티를 나열할 때 해당 프로퍼티가 포함될지 아닌지 판단
- configurable - 프로퍼티를 객체에서 삭제하거나 속성을 수정할 수 있는지 아닌지 판단



### Object.defineProperty

- 프로퍼티 속성을 컨트롤하거나 새 프로퍼티를 만들 수 있음
- 새 프로퍼티를 추가할 수 있음.
- 접근자 프로퍼티는 객체생성 이후에 추가할 수 없어, defineProperty를 활용해야 함
- 배열 프로퍼티 나열할 수 없도록 하는데 주로 사용됨



### Object.defineProperties

객체 프로퍼티 이름과 프로퍼티 정의를 연결하는 복수형 메서드



## 	객체 보호

### Object.freezing (객체동결)

- 프로퍼티 값 수정 또는 할당 🙅‍♂️
- 프로퍼티 값 수정하는 메서드 호출 🙅‍♂️
- setter 호출 🙅‍♂️
- 새 프로퍼티 추가 🙅‍♂️
- 새 메서드 추가 🙅‍♂️
- 기존 프로퍼티 메서드 설정 변경 🙅‍♂️
- Object.isFrozen 으로 동결여부 확인가능



### Object.seal (객체봉인)

- 새 프로퍼티 추가, 변경, 삭제 🙅‍♂️
- Object.isSealed 로 봉인여후 확인가능



### Object.preventExtensions (확장금지)

- 새 프로퍼티를 추가 🙅‍♂️
- 값 할당, 삭제, 속성 변경은 허용
- Object.isExtensible 로 확장금지 여부 확인가능

