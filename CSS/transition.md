# transition

트랜지션 css 는 css 속성을 변경할때 애니메이션 속도를 조절하는 방법으로 쓰인다.<br>
주로 javascript와 함께 쓰인다. 브라우저마다 달라서 속성마다 브라우저 접두어를 사용하여야 한다.
여러 속성들이 존재한다. 모든 transition 속성들을 transition 속성 하나만으로 한꺼번에 쓸 수 있다.<br>
IE는 버전 10부터 지원한다.

값|설명
---|---
transition-delay|전환효과의 시작을 연기하는 시간을 설정한다.
transition-duration|전환효과 총 시간을 설정한다.
transition-property|요소에 추가할 전환효과를 설정한다.
transition-timing-function|전환효과의 진행속도를 설정한다.


## transition-delay
transition-delay : time | initial | inherit
기본값은 0s 이다.
시간 단위는 초(s) 또는 1/1000(ms)를 사용한다.
initial : 기본값으로 설정함
inherit : 부모 요소 상속

## transition-duration
transition-delay와 같다

## transition-property
transition-property : none | all | property | initial | inherit
none : 모든 속성에 적용하지 않음
all : 모든 속성에 적용
property : 속성을 정한다. 여러개 속성을 지정할 경우 쉼표로 구분한다.
initial : 기본값으로 설정함
inherit : 부모 요소 상속

## transition-timing-function
몰라 몰라 뭔소린지 하나도 몰라

