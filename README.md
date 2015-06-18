# javascript-Common-problem
##JavaScript常见面试题及注意方面

### 变量域解析
如：
```
var a = 10;
alert(a);
//alert(a);   //在很多的其他语言中，变量需要先申明再使用，但是在js中我们可以先使用再申明（虽然这里的结果不是10，是undefined）
//var a = 10;
//在js中，js解析器会对我们的代码做一些初始化分析的工作，其他包含了这么一项内容，解析器会把程序中变量的申明在程序代码执行之前做一个初始化，  解析器会把变量的申明提前处理，上面的先调用，后申明其实也可以是下面这样

解析顺序为：
//var a;  //只是申明提前，赋值不会

//alert(a);

//a = 10;

```
```
var a = 10;

function fn() { //函数申明
    //var a;
    alert(a);
    var a = 100;    //申明被提前到了当前作用域的最开始
    alert(a);
}
fn(); //undefined 100

解析顺序为：

/*
* var a;
*
* fn;
*
* a = 10;
*
* fn();
* */

```

```
alert(fn1); //undefined

//函数表达式，申明一个变量fn1，赋值了一个函数
var fn1 = function() {
    alert(1);
}

alert(fn2); //function

function fn2() {
    alert(2);
}

```

###闭包

函数嵌套函数，内部函数可以访问外部函数的变量

如：

```
function fn1() {
    var a = 1;

    function fn2() {
        //我们是可以访问到fn1中定义a的值的
        //alert(a);
        alert(a++);
    }

    fn2();
}

fn1();
fn1();

```
当一个函数被执行的时候，和函数有关的一些变量会被申明（出现在内存中），当这个函数执行完成以后，如果函数中申明的变量没有再被其他地方所调用，则和这个函数有关的数据自动会被销毁

```
function fn1() {
    var a = 1;

    function fn2() {
        alert(a++); // alert(a); a=a+1;
        //alert(++a) // a=a+1; alert(a);
    }

    return fn2; //返回出去的是一个引用类型的值
}

var f = fn1();

f();
f();
/*f = 1;
f = fn1();
f();*/

```

###callee

```
function fn() {
    //arguments.callee => 当前函数
    //alert(arguments.callee);
}
fn();
```

###函数自执行

函数的自执行，申明并立即执行
```
(function fn1() {
    alert(1);
})();

(function() {
    alert(1);
})();

fn1();

```

setInterval 和 setTimeout

```
var a = 1;

/*function interval() {
    setTimeout(function() {
        document.title = a++;
        interval();
    }, 100);
}

interval(); */

(function() {
    var _arguments = arguments;
    setTimeout(function() {
        document.title = a++;
        _arguments.callee();
    }, 100);
})();

```

###caller
调用该函数的函数

```
function fn1() {
    console.log(1);
    console.log(fn1.caller);
}

function fn2() {
    console.log(2);
    console.log(fn2.caller);
    fn1();
}
//fn1();

fn2();  //2 -> null -> 1 -> fn2

```

###call,apply
```
function fn1(a, b) {
    console.log(this);
    console.log(a + b);
}
//fn1.call(document, 1, 2);

//apply 和call基本一致，不同的是参数的传递上

//fn1.apply(document, [1, 2]);    //第二个参数就是fn1中的arguments

```
例：找出数组中最大值
```
var arr = [1,6,4,8,3,10,2];

//console.log( Math.max(1,6,4,8,3,10,2) );  //arguments => [1,6,4,8,3,10,2]

console.log( Math.max.apply(null, arr) );   //arguments => arr
```

###this

函数外 ： 

* window

函数内 ：

* 当一个函数被对象调用，则该函数内的this指向调用该函数的对象

* 当一个函数被事件调用，则该函数内的this指向触发该事件的对象

* 通过call、apply去调用一个函数，那么这个函数指向call、apply的第一个参数（如果call、apply的第一个参数是undefined/null，则this指向调用该函数的对象）

###语法

```

alert( null == undefined ); //true
alert( true == 5 ); //false //== 把两端的值转换成数字，然后比数字是否相等
true toNumber => 1  1 == 5
alert( Number([]) ); //true
alert( Boolean([]) ) //true
alert( [] == ![] ); //true

/*
* [] == ![]
* 0 == !true
* 0 == false
* 0 == 0
* */

```

###冒泡排序

```
var arr = [3,4,1,2,7,5,6];

for (var j=0; j<arr.length-1; j++) {
    var a = arr[j];
    var b = arr[j+1];

    if (a < b) {
        arr[j] = b;
        arr[j+1] = a;
    }

    //alert(arr);

}
//[4, 3, 2, 7, 5, 6, 1]
```
```
for (var i=0; i<arr.length-1; i++) {

    var f = false;

    for (var j=0; j<arr.length-1-i; j++) {
        var a = arr[j];
        var b = arr[j+1];

        if (a < b) {
            f = true;
            arr[j] = b;
            arr[j+1] = a;
        }
    }

    if (!f) {
        break;
    }

    //alert(arr);

}

//[7, 6, 5, 4, 3, 2, 1]

```
随机

```
//console.log( Math.random() );   //0-1

//0-10

//console.log( Math.random() * 10 );

//10 - 20    0:10   1:20   随机数 * 最小值 + 差值
//console.log( Math.random() * 10 + 10 );

```

###优化

```
function show() {
	//获取id
	//document.getElementById();  可能需要多次调用document对象，在调用document，当前show函数中是没有定义document，程序会根据作用域链去查找

	//我们可以这么做
	var doc = document;

	//doc.getElementById();

}

```

