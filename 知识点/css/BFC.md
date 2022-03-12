# 定义

闭包让你可以在一个内层函数中访问到其外层函数的作用域。在 JavaScript 中，每当创建一个函数，闭包就会在函数创建的同时被创建出来。

# 闭包的不同形式

## 声明变量的形式 

### 基本形式

```javascript
function makeFunc() {
    var name = "Mozilla";
    function displayName() {
        alert(name);
    }
    return displayName;
}

var myFunc = makeFunc();
myFunc();
/** 

本例子中，`myFunc` 是执行 `makeFunc` 时创建的 `displayName` 函数实例的引用。`displayName` 的实例维持了一个对它的词法环境（变量 `name` 存在于其中）的引用。因此，当 `myFunc` 被调用时，变量 `name` 仍然可用，其值 `Mozilla` 就被传递到`alert`中

*/
```

### 参数的形式

```js
function makeAdder(x) {
  return function(y) {
    return x + y;
  };
}

var add5 = makeAdder(5);
```

### 循环中使用闭包 

```js
// 这里使用的匿名闭包；也可使用let；forEach()
function showHelp(help) {
 console.log(help)
}

function setupHelp() {
  var helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];
// 闭包
  for (var i = 0; i < helpText.length; i++) {
    var item = helpText[i];
    document.getElementById(item.id).onfocus = function (item.help) {
      return function() {
        showHelp(help);
      };
	}
  }
    
// 匿名
  for (var i = 0; i < helpText.length; i++) {
    (function() {
       var item = helpText[i];
       document.getElementById(item.id).onfocus = function() {
         showHelp(item.help);
       }
    })(); // 马上把当前循环项的item与事件回调相关联起来
  }
// forEach()   
  helpText.forEach(function(text) {
    document.getElementById(text.id).onfocus = function() {
      showHelp(text.help);
    }
  });
}

setupHelp();
/*
为什么自执行函数、let、forEach() 不需要return 一个函数呢？
因为 绑定了块作用域的变量，这意味着不再需要额外的闭包
*/
```
