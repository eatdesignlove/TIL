###### TIL 2018117

# react-app-rewired

React-app-rewired 모듈은 reacts-react-app을 커스터마이징하기 위해 필요했던 eject없이 설정의 덮어쓰기를 통해서 CRA를 사용할 수 있도록 만들어졌다. 해당 모듈을 통해 loader나 plugin의 추가가 가능하다.



## CRA에 적용하기

#### 1) react-app-rewired 설치

```sh
$ npm install react-app-rewired --save-dev
```



#### 2) config-overrides.js 생성하기

루트영역에 config-overrides.js를 생성하고 설정을 덮어씌우기 위한 코드를 추가한다.

```javascript
/* config-overrides.js */

module.exports = function override(config, env) {
  //do stuff with the webpack config...
  return config;
}
```



#### 3) package.json의 scripts 수정하기

```json
/* package.json */

"scripts": {
-   "start": "react-scripts start",
+   "start": "react-app-rewired start",
-   "build": "react-scripts build",
+   "build": "react-app-rewired build",
-   "test": "react-scripts test --env=jsdom",
+   "test": "react-app-rewired test --env=jsdom"
}
```



#### 4) Dev Server 실행하기

```sh
$ npm start
```



#### 5) 사용사례: Decorator 적용하기

```javascript
const { injectBabelPlugin } = require("react-app-rewired");

module.exports = function override(config, env) {
  config = injectBabelPlugin(['@babel/plugin-proposal-decorators', { 'legacy': true }], config);
  return config;
};
```



 

