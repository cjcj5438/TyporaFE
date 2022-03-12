

# JavaScript

### Object

### Array

### this

### 作用域 

### API

### Class

###### 实现一个类？怎么实例化这个类？

1. 构造函数法：

   用 new 关键字生成实例对象

   缺点是用到了 this 和 prototype，编写复杂，可读性差

   ```
   function Mobile(name, price) {
     this.name = name;
     this.price = price;
   }
   Mobile.prototype.sell = function () {
     alert(this.name + ",售价 $" + this.price);
   };
   var iPhone = new Mobile("iPhone", 1000);
   iPhone.sell();
   ```

   

2. Object.create法

   用`Object.create()` 生成实例对象。

   缺点是不能实现私有属性和私有方法，实例对象之间也不能共享数据

   ```
   var Person = {
     firstName: "Mark",
     lastName: "Yun",
     age: 25,
     introduce: function () {
       alert("I am " + Person.firstName + " " + Person.lastName);
     },
   };
   
   var person = Object.create(Person);
   person.introduce();
   
   // Object.create 要求IE9,低版本浏览器可以自行部署
   if (!Object.create) {
     Object.create = function (o) {
       function F() {}
       F.prototype = o;
       return new F();
     };
   }
   ```

3. **极简主义**

   消除 this 和 prototype，调用`createNew()` 得到实例对象

   优点是容易理解，结构清晰，符合传统的面向对象编程的构造

   ```
   var Cat = {
     age: 3, // 共享数据，定义在类对象内，createNew外
     createNew: function () {
       var cat = {};
       cat.name = "cat1";
       var sound = "didi"; // 私有属性，定义在createNew 内，输出对象外
       cat.makeSound = function () {
         alert(sound); // 暴露私有属性
       };
       cat.changeAge = function (num) {
         Cat.age = num; // 修改共享数据
       };
       return cat;
     },
   };
   var cat = Cat.crateNew();
   cat.makeSound();
   ```

   

4. ES6 语法糖 class

   用 new 关键字生成实例对象

   ```
   class Point {
     constructor(x, y) {
       this.x = x;
       this.y = y;
     }
     toString() {
       return "(" + this.x + "," + this.y + ")";
     }
   }
   var point = new Point();
   ```

   



