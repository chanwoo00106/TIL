# linear-gradient

그러데이션 효과로 배경을 꾸민다


사용방법
```
background: linear-gradient(to <방향 or 각도>, <색상 중지점>, [색상 중지점 ....]]);
```

이때 왼쪽에서 오른쪽으로 가는 그러데이션이라면 `to right`를 사용하고<br>
왼쪽 아래에서 오른쪽 위로 변하는 그러데이션이면 `to right top` 또는 `to top right`로 사용한다.

예제

```html
<!DOCTYPE html>
<html lang="ko">

<head>
    <meta charset="UTF-8">
    <title>before after</title>
    <style>
        .test {
            height: 700px;
            background: linear-gradient(to right, #ecf0f1, #2c3e50);
        }
    </style>
</head>

<body>
    <div class="test"></div>
</body>

</html>
```