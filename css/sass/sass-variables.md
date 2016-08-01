###### Sass variables 

# !global & !default

## !global

Sass에서는 네임스페이를 기준으로 변수의 `scope`관계가 형성된다. 문법 내에서 `{}` 또는 `선택자 아래 들여쓰기`를 하고 내부에 변수를 선언하면 해당 변수는 지역변수가 되고, 외부의 변수는 전역변수가 된다.

이때, 지역변수 뒤에 `!global`을 달아주면 전역변수로 처리가 가능하다.

```sass
//전역변수
$global-variable: #dedede

//지역변수
.example
  $local-variable-one: red

//전역변수화 된 지역변수
.example
  $local-variable-two: blue !global
```

## !default

변수에 특정값이 할당되지 않았을 때, !default 를 달아줌으로써 변수의 기본값을 설정할 수 있다. 추후에 변수에 값이 할당되면 해당 값이 적용되게 되며, 변수의 값이 null일 경우에 !default로 지정된 값을 보여준다.

```sass
//For example
$content: "First content";
$content: "Second content?" !default;
$new_content: "First time reference" !default;

#main {
  content: $content;
  new-content: $new_content;
}
```

```sass
//Compiled
#main {
  content: "First content";
  new-content: "First time reference"; }
```
