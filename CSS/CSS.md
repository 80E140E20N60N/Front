## 精灵图
```
  特点：把一些比较小的图片整合到一张图片中，使用时利用CSS的background-image属性插入图片，然后通过background-position来移动背景图，从而显示出我们想要显示出来的部分。  

  作用:减少图片请求次数，缓解了服务器的压力，提高用户体验,但是如果精灵图不太方便修改

  写法示例：
  <style>
        div{
            display: inline-block;
            background: url(images/abcd.jpg) no-repeat;
        }
        .aa{
            width: 108px;
            height: 110px;
            background-position: 0 -9px;
        }
        .nn{
            width: 112px;
            height: 110px;
            background-position: -255px -276px;
        }
  </style>
```