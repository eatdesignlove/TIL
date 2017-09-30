###### #css #layout

# Fluid Layout including Fixed Element

디자인을 구현하다보면 다양한 이유에서 비롯되어 특정 영역은 고정값을 유지하면서 동시에 유연한 레이아웃을 갖추어야 할 때가 있다. 오늘도 이와 유사한 디자인 구현이 필요해졌고, 다양한 고민 끝에 구글링을 했는데 조금 독특한 해결책을 발견하여 기록으로 남긴다.

일반적으로 두개 칼럼의 유연한 레이아웃을 구성한다면 다음과 같이 구성할 가능성이 높다.

```html
<div class="col-1"></div>
<div class="col-2"></div>
```

```css
div {
	box-sizing: border-box;
}
.col-1 {
	float: left;
	width: 66.66%;
	padding: 0 10px;
}
.col-2 {
	float: left;
	width: 33.33%;
	padding: 0 10px;
}
```

이때 왼쪽 칼럼은 유연하게, 오른쪽 칼럼은 고정너비를 갖게 구성하고자 한다면 어떻게 해야할까. 다양한 방법을 시도해보았지만 성공하지는 못했고, 구글링을 통해서 [재미있는 방법](http://jsfiddle.net/ew65G/1/)을 발견했다.

```html
<div class="fixed-col"></div>
<div class="fluid-col"></div>
```

```css
.fixed-col {
	float: right;
	width: 300px;
}
.fluid-col {
	display: table-cell;
	float: none;
}
```

바로 고정시키고자 하는 칼럼을 Fluid 칼럼의 앞쪽으로 마크업하고, `width`와 `float: right`을 부여하고, 나머지 하나의 칼럼에는 비율너비와 `display: tabel-cell`을 적용해주는 방법이다.


###### edited

작업하는 과정에서 table-cell을 레이아웃 요소로 활용하면서 내부에 콘텐츠의 양이 적절하지 않으면 왼쪽으로 붙어보이는 문제를 발견했다. 여러가지로 고민하던 끝에 유연하게 처리해야하는 부분에 컨테이너를 씌우고 padding을 고정영역만큼 추가하는 방식으로 table-cell을 사용하지 않고 처리가 가능했다. 코드는 다음과 같다.

```html
<div class="grid">
	<div class="fixed-col"></div>
	<div class="fluid-container">
		<div class="fluid-col"></div>
	</div>
</div>
```

```css
.fixed-col {
	float: right;
	width: 300px;
}
.fluid-container {
	padding-right: 300px;
}
.fluid-col {
	width: 100%;
	float: none;
}
```

유용한 방법인 것 같다. Flex를 활용하면 더 쉽게 처리할 수 있을지 모르겠다는 생각이 든다. 그래서 시도해 보니 너무나 쉽게 적용이 된다. 언젠가 IE 하위버전을 고려하지 않아도 될쯤이면 위의 방식은 추억으로 남을 듯 하다.

```html
<div class="grid">
	<div class="fixed-col"></div>
	<div class="fluid-col"></div>
</div>
```

```css
.grid {
	display: flex;
}
.fixed-col {
	width: 300px;
}
```
