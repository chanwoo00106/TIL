# css 정리

## 선택자

자식 태그 : 그 부모 태그 `바로 아래 태그들만`을 가리킴<br>
자손 태그 : 그 부모 태그 아래의 `모든 태그들`을 가리킴

대충 `>`가 있으면 자식 태그 선택자이다

css|설명
--|--
*|모든 엘리먼트 선택
#heaer|`id`가 header인 엘리먼트 선택(id가 header인 태그는 단 하나만 존재할 수 있음)
div|태그 이름이 div인 `모든` 엘리먼트 선택
.header|`class`가 header인 `모든` 엘리먼트 선택
div div|div 태그의 자식 중 `태그 이름`이 div인 `모든` 엘리먼트 선택
div.header|태그 이름이 div이면서 `class`가 header인 `모든` 엘리먼트 선택
div .header|태그 이름이 div인 태그의 자식 중 `class`가 header인 `모든` 엘리먼트 선택
div > div|div 태그의 자식 중 `태그 이름`이 div인 `모든` 태그 선택
div > .header|div 태그의 자식 중 `class가` header인 `모든` 태그 선택
h1 + p|같은 레벨에 태그 중 `첫번째`를 선택
h1 ~ p|같은 레벨에 태그 `전부`를 선택
a[href]|a 태그중 href 가 있는 모든 태그 선택
a[target = _blank]|a태그중 target이 "_blank"인 모든 태그 선택  


## display

- [Flexbox](flexbox.md)

- [Grid](grid.md)

## position

포지션은 문서 상에 요소를 배치하는 방법을 지정한다

static : 모든 태그의 기본값

- [relative](relative.md)

- [absolute](absolute.md)

<br>

## [transform](transform.md)

## [overflow](overflow.md)

## [visibility](visibility.md)

## opacity

자식까지 투명하게 만드는 것

0.0(투명) ~ 1.0(불투명)

## background

- [background-repeat](background-repeat.md)
- [background-position](background-position.md)
- [background-size](background-size.md)
- [linear-gradient](linear-gradient.md)

## text-align

값|설명
---|---
left|왼쪽 정렬
right|오른쪽 정렬
center|가운데 정렬
jutify|양쪽 정렬

## [text-decoration](text-decoration.md)

## [transition](transition.md)