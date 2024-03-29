# interface

> 인터페이스란 객체와 객체 사이에서 일어나는 상호 작용의 매개로 쓰인다.<br>
> 서로 이어주는 다리 역할과 프로젝트의 설계도로 생각하면 됩니다.

> 인터페이스는 예약어로 class 대신 "interface" 키워드를 사용하면 되며,<br>
> 접근 제어자로는 public 또는 default를 사용합니다.

## 인터페이스의 사용 이유 (장점)

- 개발 시간을 단축 시킬 수 있습니다.
- 클래스간 결합도를 낮출 수 있습니다.
- 표준화가 가능합니다.


> 수천명이나 되는 회사에서 각자 소스코드를 작성할 때 같은 기능을 하는 함수라도 각자 네이밍하는 부분이 다르기 때문에<br>
> 이 부분을 강제적으로 공통화 할 수 있는 장점이 있습니다. 인터페이스는 해당 인터페이스를 사용하는 클래스들이 많을 때 그 진가를 발휘하는 녀석입니다.