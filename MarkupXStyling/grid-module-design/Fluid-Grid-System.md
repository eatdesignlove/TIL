###### Grid-Module-Design

# Fluid 그리드 시스템

## 1. 개념
Fluid 그리드 시스템은 그 이름에서 알 수 있듯이, 물흐르는 듯 유연한 그리드 시스템을 일컫는다. Fluid 그리드 시스템과 비교해 볼 수 있는 것으로 Static 그리드 시스템이 있다. Fluid 그리드 시스템과 Static 그리드 시스템의 큰 특징을 아래에 정리해 보았다.

### Static 그리드 시스템의 특징
- **컬럼의 너비** : 고정된 px 또는 기준 폰트 사이즈를 기반으로 하는 em, rem 단위로 작성한다.
- **거터의 구현** : 마진(margin)을 활용해 거터를 구현한다. 

### Fluid 그리드 시스템의 특징
- **컬럼의 너비** : 퍼센트(%)를 기준으로 너비값을 지정하여, 화면의 움직임에 유연하게 대응된다.
- **거터의 구현** : 패딩(padding)을 활용해 거터를 구현한다. 

>너비를 퍼센트로 지정하면 마진 속성으로 거터를 구현하는 것이 어렵다. 패딩의 경우 box-sizing 속성을 이용해 패딩을 포함한 보더 영역까지를 너비로 인식하여 전체 영역을 비율에 맞게 구성할 수 있지만, 마진은 너비의 범위를 벗어나기 때문이다. 

## 2. 구현
현재 작성하는 내용의 그리드 기준은 12칼럼으로 한다.

### 그리드 모듈
float속성을 기반으로 만들어지는 그리드 시스템은 float 속성으로 인해 발생하는 문제를 해결하기 위해 grid 클래스에 clearfix를 적용한다.

```css
.grid::after {
  content: '';
  display: block;
  clear:both;
}
```

### 유닛 모듈
12칼럼의 Fluid 그리드를 구현하기 위해 각각의 유닛 너비를 퍼센트를 기준으로 작성한다. 전체 너비를 12개로 나눈 뒤, 유닛의 개수를 늘려가며 클래스를 정리한다.

```css
.col-1-12  { width: 8.3333%; }
.col-2-12  { width: 16.6666%; }
.col-3-12  { width: 25%; }
.col-4-12  { width: 33.3333%; }
.col-5-12  { width: 41.6666%; }
.col-6-12  { width: 50%; }
.col-7-12  { width: 58.3333%; }
.col-8-12  { width: 66.6666%; }
.col-9-12  { width: 75%; }
.col-10-12 { width: 83.3333%; }
.col-11-12 { width: 91.6666%; }
.col-12-12 { width: 100%; }
```

### 거터 모듈
앞서 정리한 것과 같이 Fluid 그리드 시스템에서의 거터는 패딩으로 구현한다.

```css
.gutter,
.gutter [class*="col-"] {
  padding-right: 1rem;
  padding-left:  1rem;
}
.no-gutter {
  padding-right:  0;
  padding-left:   0;
}
```

## 3. 거터와 컬럼의 표현
컬럼에 배경색을 적용해보면 거터와 컬럼이 구분되어 보이지 않는다. 이를 시각적으로 구분해주기 위해서 박스 모델의 컬럼에 background-clip(배경 색상인 경우) 또는 backgroud-origin(배경 이미지인 경우)에 content-box 를 설정해주어 영역을 구분할 수 있도록 할 수 있다.

```css
.grid [class*="col-"] {
  box-sizing: border-box;
  float: left;
  background: #e6e6e6;
  background-clip: content-box;
}
```
