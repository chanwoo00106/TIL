# css 정리

## 선택자

자식 태그 : 그 부모 태그 `바로 아래 태그들만`을 가리킴<br>
자손 태그 : 그 부모 태그 아래의 `모든 태그들`을 가리킴

대충 `>`가 있으면 자식 태그 선택자 이다

css|설명
--|--
*|모든 엘리먼트선택
#heaer|`id`가 header인 엘리먼트 선택(id가 header인 태그는 단 하나만 존재할 수 있음)
div|태그 이름이 div인 `모든` 엘리먼트 선택
.header|`class`가 header인 `모든` 엘리먼트 선택
div div|div 태그의 자식중 `태그 이름`이 div인 `모든` 엘리먼트 선택
div.header|태그 이름이 div 이면서 `class`가 header인 `모든` 엘리먼트 선택
div .header|태그 이름이 div 인 태그의 자식중 `class`가 header인 `모든` 엘리먼트 선택
div > div|div 태그의 자식중 `태그 이름`이 div인 `모든` 태그 선택
div > .header|div 태그의 자식중 `class가` header인 `모든` 태그 선택


## display

[Flexbox](https://github.com/chanwoo00106/css/blob/master/flexbox)

[Grid](https://github.com/chanwoo00106/css/blob/master/grid)

## position

포지션은 문서 상에 요소를 배치하는 방법을 지정한다

static : 모든 태그의 기본값

[relative](https://github.com/chanwoo00106/css/blob/master/relative)

[absolute](https://github.com/chanwoo00106/css/blob/master/absolute)

## transform

[transform](https://codepen.io/chanwoo00106/pen/gOmXLeL)
