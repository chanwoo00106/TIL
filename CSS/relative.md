# relative

원래 위치에서 top, left, right, bottom으로 옮길 수 있게 한다.


![relative](./images/relative.png)

```html
<div style="background: #81ecec; width: 150px; height: 150px; margin: 20px; color: black;">
    position: static
</div>

<div style="background: #81ecec; width: 150px; height: 150px; margin: 20px; color: black; position: relative; left: 20px; top: 20px; border: 1px solid black;">
    position: relative;
    left: 20px;<br>
    top: 20px;
</div>

<div style="background: #81ecec; width: 150px; height: 150px; margin: 20px; color: black;">
    position: static
</div>
```