# CSS 초기화

모든 브라우저는 각각의 요소에 적용되는 기본 스타일을 가지고 있다. 어떤 디자인을 화면에 구현함에 있어서 브라우저가 지니고 있는 기본 스타일을 바꾸어 쓰는 것은 필연적인데, 매번 특정 부분을 수정해서 쓰는 것보다 프로젝트를 시작하기 전에 처음부터 초기화 하고 쓰는 것이 효율적이다. 그래서 예전부터 다양한 형태의 초기화 CSS가 사용되어 왔는데, 특히 에릭 마이어(Eric Meyer)의 Reset CSSs는 보편적으로 많이 사용되어 왔다.


[Eric Meyer’s “Reset CSS” 2.0](http://cssreset.com/scripts/eric-meyer-reset-css/)
[Normalize.css](https://necolas.github.io/normalize.css/)
[Ress.css](https://github.com/filipelinhares/ress)

그러다 최근에 들어서 Reset CSS를 보완한 형태의 초기화 CSS들이 등장하고 있는데, 그 중 Normalize.css와 Ress.css가 눈에 띈다. 이들을 살펴보면서 단지 Reset.css 이후에 등장했다 혹은 최근에 많이 쓰인다는 이유로 이것들을 사용한다는 것은 좋은 프론트엔드 개발자의 자세가 아니라는 생각이 들었다. 특정 초기화 CSS가 더 최근에 나왔기 때문이 아니라, 진행하는 프로젝트의 특성에 따라 사용여부를 결정하는 것이 옳을 것이다. 무엇을 쓰던지 그 이유에 대해 스스로 명확히 인지하는 것이 필요하다는 생각에서 세 가지 css를 동일한 마크업에 적용하여 나타는 결과를 중심으로 살펴보았다. 

```html
<ul>
    <li>li: List Item</li>
    <li>li: List Item</li>
    <li>li: List Item</li>
    <li>li: List Item</li>
    <li>li: List Item</li>
  </ul>
  <h1>h1: Big letters</h1>
  <h2>h2: Less big of letters</h2>
  <h3>h3: Getting smaller</h3>  
  <h4>h4: Hello world</h4>  
  <h5>h5: Kid Rock</h5>  
  <h6>h6: Ant man</h6>  
  <article>
    <p>p: in arrticle.</p>
    <p>p: Lorem ipsum dolor sit amet.</p>
    <p>p: Lorem ipsum dolor sit amet, consectetur adipisicing elit. Fugit dignissimos iusto maxime, quibusdam ut harum consectetur <sup>sup: 1</sup> rem consequuntur sunt vero!</p>
    <p>p: Lorem ipsum dolor sit amet, consectetur adipisicing elit. Aperiam iste, soluta quibusdam ullam error qui atque <sub>sub: 7</sub>, expedita quod dicta vel laudantium tenetur, quos, voluptatibus aspernatur enim magni velit. Distinctio, adipisci.</p>    
  </article>
  <hr />
  <a href="##">a: Singles in your area</a><br />
  <button>button: Don't click</button>
  <i>i: fancy</i><br />
  <b>b: loud</b>  <br />
  <strong>strong: loud?</strong> <br />
  <small>small: copyright forever</small>
```

## Reset CSS
![Reset.css Result](https://github.com/eatdesignlove/TIL/blob/master/MarkupXStyling/css-initialization/images/reset.png)


## Normalize CSS
![Normalize Result](https://github.com/eatdesignlove/TIL/blob/master/MarkupXStyling/css-initialization/images/normalize.png)


## Ress CSS
![Normalize Result](https://github.com/eatdesignlove/TIL/blob/master/MarkupXStyling/css-initialization/images/normalize.png)


## 눈에 띄는 차이점


## Source code.
>[Reset.css](./reset-css.md)
[Normalize.css](./normalize-css.md)
[Ress.css](./ress-css.md)

## 참고자료
[reset vs normalize 비교](https://codepen.io/zachwolf/pen/bdZMZj/)

