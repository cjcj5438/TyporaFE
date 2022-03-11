# 块格式化上下文（Block Formatting Context，BFC)

## 定义

BFC 就是块级格式上下文，是页面盒模型布局中的一种 CSS 渲染模式，相当于 一个独立的容器，里面的元素和外部的元素相互不影响

## 怎样触发BFC

- html 根元素
- float 浮动
- 绝对定位
- overflow 不为 visiable
- display 为表格布局或者弹性布局

## BFC的规则

- `BFC`就是一个块级元素，块级元素会在垂直方向一个接一个的排列
- `BFC`就是页面中的一个隔离的独立容器，容器里的标签不会影响到外部标签
- 垂直方向的距离由margin决定， 属于同一个`BFC`的两个相邻的标签外边距会发生重叠
- 计算`BFC`的高度时，浮动元素也参与计算

## BFC解决了什么问题

- ### 使用Float脱离文档流，高度塌陷

  ```html
  
      <style>
          .box {
              margin: 100px;
              width: 100px;
              height: 100px;
              background: red;
              float: left;
          }
          .container {
              background: #000;
              display: inline-block; //add line： 形成BFC 防止高度塌陷
          }
      </style>
      <div class="container">
          <div class="box"></div>
          <div class="box"></div>
      </div>
  
  ```

- ### 两栏布局 相当于 阻止浮动元素覆盖

  ```html
  
      <style>
              div {
                   width: 200px;
                   height: 100px;
                   border: 1px solid red;
              }
  
      </style>
      <div style="float: left;">
          两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局
      </div>
      <div style="width: 300px;overflow:hidden">  // 创建BFC 阻止浮动元素覆盖
          我是蛙人，如有帮助请点个赞叭，如有帮助请点个赞叭，如有帮助请点个赞叭，如有帮助请点个赞叭，如有帮助请点个赞叭，如有帮助请点个赞叭
      </div>
  
  ```

  

