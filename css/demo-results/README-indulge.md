###### 반응형웹사이트 데모제작 

# Indulge

## 특징
- 그리드 시스템 기반의 사이트 디자인 (14px/1.5)
- 모듈화에 적합하도록 디자인된 화면 구성요소
- 모바일 PSD, 데스크탑 PSD가 제공됨

![Indulge Wireframe](./images/project-info/project-info-wireframe.jpg)

## 목표
- 유연한 그리드 시스템의 활용
- 모듈화를 통해 유지보수 및 효율적인 코드 작성
- 깔끔한 디자인 구현 및 사용자 인터렉션
- CSS to SASS 

## 이슈 & 리팩토링

### 비율을 유지하는 콘텐츠 디자인

버튼 / 웹 타이포그래피

화면의 크기가 변화함에도 불구하고 콘텐츠의 크기는 그에 맞게 대응하지 못해서 특정 화면에서 콘텐츠가 본래의 영역을 벗어나는 경우가 발생했다. 아래의 글을 참고삼아 vw기반의 Fluid Typography를 구현해보려고 했지만, 아직은 역부족. 이번 주가 지나기 전에 해결해내겠다.

[Getting Started With Fluid Typography Link](https://www.smashingmagazine.com/2016/05/fluid-typography/)

------

### 네비게이션의 모바일-데스크탑 전환

로고부분을 제외하면 상단의 네비게이션은 크게 GNB, UNB로 구성되는데, 데스크탑 화면의 여러 list items를 모바일 화면에서의 3가지 주요 메뉴로 전환시켜야 했다. 

처음에 단순히 스타일 추가를 통해 구현하려다 어려움을 겪고, 이 부분 역시 그리드 시스템을 활용하여 개선했다. 데스크탑 화면에서는 기존의 그리드 칼럼 값을 가지고, 모바일 화면에서는 3개의 그리드를 기준으로 배치할 수 있었다.

------

### hover에도 흔들리지 않는 네비게이션

주요 네이게이션 메뉴의 링크 요소에는 hover시 `font-weight`를 두껍게 처리하도록 했는데, 실제로 반영된 화면에서 확인한 결과 `bold`에 영향을 받아 오른쪽 링크들이 밀려나는 현상이 있었다. `text-shadow`를 이용해 두꺼운 폰트를 재현하여, 스타일에 의해 다른 요소가 영향을 받지 않도록 처리했다.

```css
text-shadow: 1px 0 0 currentColor;
```

>[text-shadow를 이용해 font-weight 대체하기](https://justmarkup.com/log/2015/11/quick-tip-using-text-shadow-instead-of-font-weight-bold-to-avoid-jumping/)

------

### 배경 이미지 효과를 이용한 사용자 인터랙션

처음 마크업한 결과물은 화면의 주요 모듈을 구성하는 `.list-card` 에 배경 이미지, 비율 조정을 위한 padding을 부여하고, 콘텐츠는 그 내부에 `.box-content`를 position으로 위치를 잡아주는 형태였다.

마우스 hover시에 배경화면 크기를 서서히 키우는 애니메이션 위해 `transition`속성과 `background-size`를 이용해 작업했는데, 이 부분을 시각적으로 더 집중시키기 위해 추가로 배경 이미지에 `opacity`속성을 이용해 어둡게 처리하고자 했다.

배경 이미지를 담고 있는 요소에 `opacity`를 부여하면 내부의 요소들까지 영향을 받게되기 때문에, 가상요소를 이용해 색상 레이어를 추가하는 방식으로 해당부분을 완성했다.

------

### CSS to SASS



------

