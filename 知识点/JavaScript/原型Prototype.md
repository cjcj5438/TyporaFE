

# 构造函数

### 什么是构造函数？

`constructor` 返回创建实例对象时构造函数的引用。此属性的值是对函数本身的引用，而不是一个包含函数名称的字符串。

```js
function Parent(age) {
    this.age = age;
}

var p = new Parent(50);
p.constructor === Parent; // true
p.constructor === Object; // false

// new 生成实例的函数就是构造函数
```



### Symbol 是构造函数吗

基本数据类型，但作为构造函数来说它并不完整，因为它不支持语法 `new Symbol()`,但是 `Symbol.prototype.constructor` 返回创建实例原型的函数

### constructor 值只读吗？

### 模拟实现 new

```js
// 参考答案：2.更完整的实现
/**
* 模拟实现 new 操作符
* @param  {Function} ctor [构造函数]
* @return {Object|Function|Regex|Date|Error}      [返回结果]
*/
function newOperator(ctor){
   if(typeof ctor !== 'function'){
     throw 'newOperator function the first param must be a function';
   }
   // ES6 new.target 是指向构造函数
   newOperator.target = ctor;
   // 1.创建一个全新的对象，
   // 2.并且执行[[Prototype]]链接
   // 4.通过`new`创建的每个对象将最终被`[[Prototype]]`链接到这个函数的`prototype`对象上。
   var newObj = Object.create(ctor.prototype);
   // ES5 arguments转成数组 当然也可以用ES6 [...arguments], Aarry.from(arguments);
   // 除去ctor构造函数的其余参数
   var argsArr = [].slice.call(arguments, 1);
   // 3.生成的新对象会绑定到函数调用的`this`。
   // 获取到ctor函数返回结果
   var ctorReturnResult = ctor.apply(newObj, argsArr);
   // 小结4 这些类型中合并起来只有Object和Function两种类型 typeof null 也是'object'所以要不等于null，排除null
   var isObject = typeof ctorReturnResult === 'object' && ctorReturnResult !== null;
   var isFunction = typeof ctorReturnResult === 'function';
   if(isObject || isFunction){
       return ctorReturnResult;
   }
   // 5.如果函数没有返回对象类型`Object`(包含`Functoin`, `Array`, `Date`, `RegExg`, `Error`)，那么`new`表达式中的函数调用会自动返回这个新的对象。
   return newObj;
}
// 测试
function company(name, address) {
   this.name = name;
   this.address = address;
 }
 
var company1 = newOperator(company, 'yideng', 'beijing');
console.log('company1: ', company1);
```

### new操作符做了什么 

new 运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例。new 关键字会进行如下的操作： 

1. 创建一个空的简单JavaScript对象（即{}）；
2.  链接该对象（即设置该对象的构造函数）到另一个对象 ； 
3. 将步骤1新创建的对象作为this的上下文 ； 
4. 如果该函数没有返回对象，则返回this。

# 原型

### prototype**原型对象**

<img src="https://chenjing-oss.oss-cn-hangzhou.aliyuncs.com/typora/2019-07-24-60302.jpg" alt="image-20190210223838636" style="zoom:50%;" />

构造函数 `Parent` 有一个指向原型的指针，原型 `Parent.prototype` 有一个指向构造函数的指针 `Parent.prototype.constructor`，如上图所示，其实就是一个循环引用。

<img src="https://chenjing-oss.oss-cn-hangzhou.aliyuncs.com/typora/2019-07-24-060303.jpg" alt="image-20190211154751602" style="zoom:80%;" />

### `__proto__`

用 `p.__proto__` 获取对象的原型，`__proto__` 是每个实例上都有的属性，`prototype` 是构造函数的属性，这两个并不一样，但 `p.__proto__` 和 `Parent.prototype` 指向同一个对象。

```js
function Parent() {}
var p = new Parent();
p.__proto__ === Parent.prototype
// true
```

所以构造函数 `Parent`、`Parent.prototype` 和 `p` 的关系如下图。

<img src="http://resource.muyiy.cn/image/2019-07-24-060305.jpg" alt="image-20190211200314401" style="zoom:50%;" />

### 注意点 

`__proto__`是每个实例上都有的属性，prototype是构造函数的属性，这两个并不一样，但p.`__proto__`和Parent.prototype指向同一个对象。

`__proto__`属性在es6时被标准化。但是因为性能问题不推荐使用，推荐使用Object.getPrototypeOf( )

```
// 获取
Object.getPrototypeOf()
Reflect.getPrototypeOf()

// 修改
Object.setPrototypeOf()
Reflect.setPrototypeOf()
```



### 优化实现new

​	

```js
function create() {
	// 1、获得构造函数，同时删除 arguments 中第一个参数
    Con = [].shift.call(arguments);
	// 2、创建一个空的对象并链接到原型，obj 可以访问构造函数原型中的属性
    var obj = Object.create(Con.prototype);
	// 3、绑定 this 实现继承，obj 可以访问到构造函数中的属性
    var ret = Con.apply(obj, arguments);
	// 4、优先返回构造函数返回的对象
	return ret instanceof Object ? ret : obj;
};
```



# 原型链

每个对象拥有一个原型对象，通过 _ _proto_ _ 指向上一个原型，并从继承方法和属性，同时原型对象也可能拥有原型，这样一层一层，最终指向null，这就是**原型链** 

```js
function Parent(age) {
    this.age = age;
}
var p = new Parent(50);

p;	// Parent {age: 50}
p.__proto__ === Parent.prototype; // true
p.__proto__.__proto__ === Object.prototype; // true
p.__proto__.__proto__.__proto__ === null; // true
```

### instanceof 原理及实现

就是一层一层查找 `__proto__`，如果和 `constructor.prototype` 相等则返回 true，如果一直没有查找成功则返回 false。

### 原型链继承

原型链继承方案有很多问题，实践中很少会单独使用