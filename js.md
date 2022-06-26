# JsvaScript

# J****avaScript中的数据类型****

在`JavaScript`中，我们可以分成两种类型：

- 基本类型
    - Number
        
        ```jsx
        console.log(0/0); // NaN
        console.log(-0/+0); // NaN
        ```
        
    - String
    - Boolean
        
        ```jsx
        数据类型      				转换为 true 的值      				转换为 false 的值
         String        			非空字符串          					"" 
         Number 				    非零数值（包括无穷值）					0 、 NaN 
         Object 					  任意对象 							      null, []
         Undefined 					N/A （不存在） 						    undefined
        ```
        
    - Undefined
        
        ```jsx
        let message; // 这个变量被声明了，只是值为 undefined
        
        console.log(message); // "undefined"
        console.log(age); // 没有声明过这个变量，报错
        ```
        
    - null
        
        ```jsx
        //逻辑上讲， null 值表示一个空对象指针，这也是给typeof传一个 null 会返回 "object" 的原因
        let car = null;
        console.log(typeof car); // "object"
        
        //undefined 值是由 null值派生而来
        console.log(null == undefined); // true
        ```
        
    - symbol
        
        ```jsx
        //Symbol （符号）是原始值，且符号实例是唯一、不可变的。符号的用途是确保对象属性使用唯一标识符，不会发生属性冲突的危险
        
        let genericSymbol = Symbol();
        let otherGenericSymbol = Symbol();
        console.log(genericSymbol == otherGenericSymbol); // false
        
        let fooSymbol = Symbol('foo');
        let otherFooSymbol = Symbol('foo');
        console.log(fooSymbol == otherFooSymbol); // false
        ```
        
    - bigInt
- 引用数据类型
    - object
    

# 判断类型的几种办法

1. typeof（不能判断null，复杂类型）
    
    ```jsx
    typeof 1 // 'number'
    typeof '1' // 'string'
    typeof undefined // 'undefined'
    typeof true // 'boolean'
    typeof Symbol() // 'symbol'
    typeof null // 'object'
    typeof [] // 'object'
    typeof {} // 'object'
    typeof console // 'object'
    typeof console.log // 'function'
    ```
    
2. instanceof（不能正确判断基础数据类型）
    
    `instanceof`运算符用于检测构造函数的 `prototype`属性是否出现在某个实例对象的原型链上
    
    ```jsx
    //object为实例对象，constructor为构造函数
    object instanceof constructor
    ```
    
    构造函数通过`new`可以实例对象，`instanceof`能判断这个对象是否是之前那个构造函数生成的对象
    
    ```jsx
    // 定义构建函数
    let Car = function() {}
    let benz = new Car()
    benz instanceof Car // true
    let car = new String('xxx')
    car instanceof String // true
    let str = 'xxx'
    str instanceof String // false
    ```
    
    原理
    
    ```jsx
    function myInstanceof(left, right) {
        // 这里先用typeof来判断基础数据类型，如果是，直接返回false
        if(typeof left !== 'object' || left === null) return false;
        // getProtypeOf是Object对象自带的API，能够拿到参数的原型对象
        let proto = Object.getPrototypeOf(left);
        while(true) {
            if(proto === null) return false;
            if(proto === right.prototype) return true;//找到相同原型对象，返回true
            proto = Object.getPrototypeof(proto);
        }
    }
    ```
    

1. toString()
    
    ```jsx
    Object.prototype.toString({})       // "[object Object]"
    Object.prototype.toString.call({})  // 同上结果，加上call也ok
    Object.prototype.toString.call(1)    // "[object Number]"
    Object.prototype.toString.call('1')  // "[object String]"
    Object.prototype.toString.call(true)  // "[object Boolean]"
    Object.prototype.toString.call(function(){})  // "[object Function]"
    Object.prototype.toString.call(null)   //"[object Null]"
    Object.prototype.toString.call(undefined) //"[object Undefined]"
    Object.prototype.toString.call(/123/g)    //"[object RegExp]"
    Object.prototype.toString.call(new Date()) //"[object Date]"
    Object.prototype.toString.call([])       //"[object Array]"
    Object.prototype.toString.call(document)  //"[object HTMLDocument]"
    Object.prototype.toString.call(window)   //"[object Window]"
    ```
    
2. ****constructor****

# 类型转换机制

两种类型转换的方式

- ****数学运算符中的类型转换****
- **逻辑语句的类型转换**

****数学运算符中的类型转换****

1. 减乘除运算

**我们在对各种非`Number`类型运用数学运算符(`- * /`)时，会先将非`Number`类型转换为`Number`类型。**

```
1 - true // 0， 首先把 true 转换为数字 1， 然后执行 1 - 1
1 - null // 1,  首先把 null 转换为数字 0， 然后执行 1 - 0
1 * undefined //  NaN, undefined 转换为数字是 NaN
2 * ['5'] //  10， ['5']首先会变成 '5', 然后再变成数字 5
```

1. 加法的特殊性

为什么加法要区别对待？因为JS里 `+`还可以用来拼接字符串。谨记以下3条：

- 当一侧为`String`类型，被识别为字符串拼接，并会优先将另一侧转换为字符串类型。
- 当一侧为`Number`类型，另一侧为原始类型，则将原始类型转换为`Number`类型。
- 当一侧为`Number`类型，另一侧为引用类型，将引用类型和`Number`类型转换成字符串后拼接。

```jsx
123 + '123' // 123123   （规则1）
123 + null  // 123    （规则2）
123 + true // 124    （规则2）
123 + {}  // 123[object Object]    （规则3）
```

**逻辑语句的类型转换**

1. 如果只有单个变量，会先将变量转换为Boolean值。
    
    只有 `null，undefined，''，NaN，0，fals`e这几个是 `false`，其他的情况都是 `true`
    
2. 使用==比较中的5条规则
    - 规则 1：`NaN`和其他任何类型比较永远返回`false`（包括和他自己）。
    - 规则 2：Boolean 和其他任何类型比较，Boolean 首先被转换为 Number 类型。
    - 规则 3：`String`和`Number`比较，先将`String`转换为`Number`类型。
    - 规则 4：`null == undefined`比较结果是`true`，除此之外，`null`
    、`undefined`和其他任何结果的比较值都为`false`。
    - 规则 5：`原始类型`和`引用类型`做比较时，引用类型会依照`ToPrimitive`
    规则转换为原始类型。
    
    ****To Primitive作用：将任意值转换成原始值，针对引用类型来说的ToPrimitive(input [, PreferredType])**
    
    该抽象操作接受一个参数`input`和一个可选的参数`PreferredType`。该抽象操作的目的是把参数`input`转化为非对象数据类型，也就是原始数据类型。
    
    如果`input`的数据类型是对象，执行下述步骤：
    
    1. 如果没有传入`PreferredType`参数，让`hint`等于`"default"`；
    2. 如果`PreferredType`是`hint String`，让`hint`等于`"string"`；
    3. 如果`PreferredType`是`hint Number`，让`hint`等于`"number"`；
    4. 让`exoticToPrim`等于`GetMethod(input, @@toPrimitive)`，大概语义就是获取参数`input`的`@@toPrimitive`方法；
    5. 如果`exoticToPrim`不是`Undefined`，那么：
        1. 让`result`等于`Call(exoticToPrim, input, « hint »)`，大概语义就是执行`exoticToPrim(hint)`；
        2. 如果`result`是原始数据类型，返回`result`；
        3. 抛出类型错误的异常；
    6. 如果`hint`是`"default"`，让`hint`等于`"number"`；
    7. 返回`OrdinaryToPrimitive(input, hint)`抽象操作的结果。
    

# ==和===区别是什么

==会做类型转换再做判断，隐式zhuan hua

===严格运算，直接判断，不会做类型转换

# 什么是闭包

**什么是闭包**

**闭包就是指一个函数能够访问其外层词法作用域内的变量所组合成的结构体**

在`JavaScript`中，每创建一个函数，闭包在函数创建的同时被创建出来，作为内部函数与外部连接起来的一座桥梁

**特点：**

1. 闭包可以访问当前函数以外的变量
2. 即使外部函数已经返回，闭包仍能访问外部函数定义的变量
3. 闭包可以更新外部变量的值

****使用闭包应该注意什么****

- **内存泄漏：** 程序的运行需要内存。对于持续运行的服务进程，必须及时释放不再用到的内存，否则占用越来越高，轻则影响系统性能，重则导致进程崩溃。不再用到的内存，没有及时释放，就叫做内存泄漏。
    
    **出现情况：**
    
    - 意外的全局变量，this也可能创建（严格模式避免）
    - 定时器也常会造成内存泄露
    - 闭包
    - 没有清理对`DOM`元素的引用同样造成内存泄露
    - 对Dom元素的监听不及时清理
- **this指向：** 闭包的this指向的是window

**应用场景**

闭包通常用来创建内部变量，使得这些变量不能被外部随意修改，同时又可以通过指定的函数接口来操作。例如 `setTimeout` 传参、回调、IIFE、函数防抖、节流、柯里化、模块化等等

```jsx
//原生的setTimeout传递的第一个函数不能带参数
setTimeout(function(param){
    alert(param)
},1000)

//通过闭包可以实现传参效果
function myfunc(param){
    return function(){
        alert(param)
    }
}
var f1 = myfunc(1);
setTimeout(f1,1000);
```

错误使用：

```jsx
**在循环中创建闭包
var data = []

for (var i = 0; i < 3; i++) {
  data[i] = function () {
    console.log(i)
  }
}
data[0]()   // 3
data[1]()   // 3
data[2]()   // 3**

```

# 什么是作用域及作用域链

**作用域就是一个独立的地盘，让变量不会外泄、暴露出去**。也就是说**作用域最大的用处就是隔离变量，不同作用域下同名变量不会有冲突。**

在ES6之前，JS的scope只有两种，**全局作用域**和**函数作用域**，但是在ES6种出现了块级作用域，即使用let/const可以定义块级作用域

作用域链：当我们运行一段js代码的时候，v8会帮我创造出一个函数调用栈，在初始化的时候，会在那个函数调用栈中

# var和let/const区别

1. **变量提升：**var可以在声明的上面访问变量，而let和const有**暂存死区**，在声明的上面访问变量会报错
2. **暂时性死区：**在此作用域中用 let 或者 const 声明的变量会先在作用域中被创建出来，但此时**还未进行词法绑定**，所以是不能被访问的，如果访问就会抛出错误。因此，在这运行流程进入作用域创建变量，到变量可以被访问之间的这段时间，就称之为暂时死区。
3. **禁止重复声明：**var声明变量可以重复声明，而let和const不可以重复声明
4. **块级作用域**：var不存在块级作用域，let和const有块级作用域
5. var会与window相映射（会挂一个属性），而let和const不与window相映射
6. const声明之后必须赋值，否则会报错,let不会报错。const声明一个只读的常量，改变了就会报错，let定义变量。

# for…in…和for…of…区别

**for in 遍历的是可枚举属性，for of 遍历的是可迭代**

1. 推荐在循环对象属性的时候，使用`for...in`,在遍历数组的时候的时候使用`for...of`。
2. `for...of`不能循环普通的对象，需要通过和`Object.keys()`搭配使用

for...of语句在可迭代对象(包括 Array, Map, Set, String, TypedArray，arguments 对象等等)上**创建一个迭代循环**，对每个不同属性的属性值,调用一个自定义的有执行语句的迭代挂钩.

也就是说，for of只可以循环可迭代对象的可迭代属性，不可迭代属性在循环中被忽略了后。

```jsx
Object.prototype.objCustom = function () {}; 
Array.prototype.arrCustom = function () {};

let iterable = [3, 5, 7];
iterable.foo = "hello";

for (let i in iterable) {
  console.log(i); // logs 0, 1, 2, "foo", "arrCustom", "objCustom"
}

for (let i of iterable) {
  console.log(i); // logs 3, 5, 7
}
```

# 获取对象属性

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/39e4f495-2298-4a8e-b47a-350aff6a34c4/Untitled.png)

# 浅拷贝与深拷贝

- 浅拷贝是创建一个新对象，这个对象有着原始对象属性值的一份精确拷贝。**如果属性是基本类型，拷贝的就是基本类型的值，如果属性是引用类型，拷贝的就是内存地址** ，所以**如果其中一个对象改变了这个地址，就会影响到另一个对象**。
- 深拷贝是将一个对象从内存中完整的拷贝一份出来,从堆内存中开辟一个新的区域存放新对象,且**修改新对象不会影响原对象**。
1. **浅拷贝实现方法**
    1. Object.assign()
        
        ```jsx
        let obj1 = { person: {name: "kobe", age: 41},sports:'basketball' };
        let obj2 = Object.assign({}, obj1);
        ```
        
    2. 展开运算符...
        
        ```jsx
        let obj1 = { name: 'Kobe', address:{x:100,y:100}}
        let obj2= {... obj1}
        ```
        
    3. Array.prototype.concat()
        
        ```jsx
        let arr = [1, 3, {
            username: 'kobe'
            }];
        let arr2 = arr.concat();
        ```
        
    4. Array.prototype.slice()
        
        ```jsx
        let arr = [1, 3, {
            username: ' kobe'
            }];
        let arr3 = arr.slice();
        ```
        

1. **深拷贝实现方法**
    1. JSON.parse(JSON.stringfy())
        
        ```jsx
        let arr = [1, 3, {
            username: ' kobe'
        }];
        let arr4 = JSON.parse(JSON.stringify(arr));
        ```
        
    2. 手写递归函数
        
        ```jsx
        function deepClone(oldObject) {
        
            const newObject = Array.isArray(oldObject) ? [] : {}
        
            for(let key in oldObject) {
                if(typeof oldObject[key] === 'object') {
                    newObject[key] = deepClone(oldObject[key])
                }else {
                    newObject[key] = oldObject[key]
                }
            }
        
            const symbols = Object.getOwnPropertySymbols(oldObject)
            for(let keys of symbols) {
                newObject[keys] = oldObject[keys]
            }
            return newObject
        }
        ```
        

# Object.defineprototype(obj, prop, descriptor)属性描述符和存取描述符

**这都是对象的东西**

1. 数据属性描述符
    1. configurable: 该属性不可删除，false
    2. enumerable： 是否可以枚举，false
    3. writable：是否是可写入值，false
    4. value：设置的值，默认为undifined
2. 存取描述符
    1. get： function（）{return value}
    2. set： function(newValue) { value = newValue}
    
    **用途：**
    

隐藏某个私有的值

截获某一个属性，并且设置值，做拦截

存取描述符可以和configurable，enumerable搭配使用

 

# 原型和原型链

**对象的原型**

早期的ECMA提供了一个属性`__proto__`（ES5中通过`Object.getPrototypeOf()`）,每个对象都有一个`[[prototype]]`，称为隐式原型。所有的对象上级都是`Object.prototype` ,`Object.prototyp`    的上级就是null

**函数的原型**

函数有显式原型`prototype`这是个对象，`prototype`对象地址里有一个属性`constructor`，指向构造函数本身，同时`prototype`对象还有个`__proto__`属性指向`Object.prototype`

通过构造函数构造出来的对象有一个__proto__属性指向构造函数的prototype属性

```jsx
function Person() {
}
const p1 = new Person()
console.log(p1.__proto__ === Person.prototype); //true
console.log(Person.prototype.__proto__ === Object.prototype); //true
console.log(Person.prototype.__proto__.__proto__); //null

比如我们定义一个foo函数，那么他就存在一个prototype属性指向foo的原型对象，
由于函数又是一个特殊的对象，所以这个函数也有一个__proto__属性指向Function构造函数的原型对象
在这个对象里存在一个constructor属性指向构造函数本身，
并且还有个__proto__属性指向Object的原型对象。

同时如果用new关键字函数构造一个对象，这个对象也有一个__proto__属性指向foo函数的原型对象

Function也是function对象，所以Function.__proto__ === Function.prototype
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/abefe330-0ff4-4f40-8eef-54d8f6672dd2/Untitled.png)

# 实现继承的几种方案

```jsx
/**
 * *实现继承的几种方式
 */

//原型链实现继承：基本思想是利用原型让一个引用类型继承另一个引用类型的属性和方法。
//因为此模式会共享属性和方法，所以当一个实例修改属性和方法时会影响其他实例，而且子类型无法向超类型传递参数。
function father() {
    this.name = "yc";
}
function son() {}

son.prototype = new father();
const instance = new son();
console.log(instance.name);
console.log(son.prototype);

//借用构造函数 为解决原型中包含引用类型值所带来的问题，人们开始用一种叫做借用构造函数的技术来实现继承。
//这种技术的基本思想非常简单，即在子类构造函数内部调用超类构造函数
//只能继承父类的实例属性和方法，不能继承原型属性/方法

function Parent() {
    this.name = "parent1";
}

Parent.prototype.getName = function () {
    return this.name;
};

function Child() {
    Parent1.call(this);
    this.type = "child";
}

let child = new Child();
console.log(child); // 没问题
console.log(child.getName()); // 会报错
console.log(son1.__proto__);

//组合继承组合上述两种方法就是组合继承。
//用原型链实现对原型属性和方法的继承，用借用构造函数技术来实现实例属性的继承。
//组合模式的缺点就是在使用子类创建实例对象时，其原型中会存在两份相同的属性/方法。

function Parent3() {
    this.name = "parent3";
    this.play = [1, 2, 3];
}

Parent3.prototype.getName = function () {
    return this.name;
};
function Child3() {
    // 第二次调用 Parent3()
    Parent3.call(this);
    this.type = "child3";
}

// 第一次调用 Parent3()
Child3.prototype = new Parent3();
// 手动挂上构造器，指向自己的构造函数
Child3.prototype.constructor = Child3;
var s3 = new Child3();
var s4 = new Child3();
s3.play.push(4);
console.log(s3.play, s4.play); // 不互相影响
console.log(s3.getName()); // 正常输出'parent3'
console.log(s4.getName()); // 正常输出'parent3'

//原型式继承
//利用一个空对象作为中介，将某个对象直接赋值给空对象构造函数的原型。
//这里主要借助Object.create方法实现普通对象的继承
//因为Object.create方法实现的是浅拷贝，多个实例的引用类型属性指向相同的内存，存在篡改的可能

let parent4 = {
    name: "parent4",
    friends: ["p1", "p2", "p3"],
    getName: function () {
        return this.name;
    },
};

let person4 = Object.create(parent4);
person4.name = "tom";
person4.friends.push("jerry");

let person5 = Object.create(parent4);
person5.friends.push("lucy");

console.log(person4.name); // tom
console.log(person4.name === person4.getName()); // true
console.log(person5.name); // parent4
console.log(person4.friends); // ["p1", "p2", "p3","jerry","lucy"]
console.log(person5.friends); // ["p1", "p2", "p3","jerry","lucy"]

//寄生式继承
//核心：在原型式继承的基础上，增强对象，封装一层clone函数，返回构造函数
//其优缺点也很明显，跟上面讲的原型式继承一样
let parent5 = {
    name: "parent5",
    friends: ["p1", "p2", "p3"],
    getName: function () {
        return this.name;
    },
};

function clone(original) {
    let clone = Object.create(original);
    clone.getFriends = function () {
        return this.friends;
    };
    return clone;
}

let person5 = clone(parent5);

console.log(person5.getName()); // parent5
console.log(person5.getFriends()); // ["p1", "p2", "p3"]

//寄生组合式继承
//寄生组合式继承，借助解决普通对象的继承问题的Object.create 方法，在前面几种继承方式的优缺点基础上进行改造，这也是所有继承方式里面相对最优的继承方式
function clone(parent, child) {
    // 这里改用 Object.create 就可以减少组合继承中多进行一次构造的过程
    child.prototype = Object.create(parent.prototype);
    child.prototype.constructor = child;
}
function father() {
    this.name = "yc";
    this.play = ["1", "2"];
}
father.prototype.getName = function () {
    console.log(this.name + " is running");
};

function son() {
    father.call(this);
    this.friends = "child5";
}

clone(father, son);

son.prototype.getFriends = function () {
    return this.friends;
};
let person6 = new son();
console.log(person6); //{friends:"child5",name:"child5",play:[1,2,3],__proto__:Parent6}
console.log(person6.getName()); // parent6
console.log(person6.getFriends()); // child5
```

# 数组使用代理拦截操作

```jsx
const lessons = [{
        name: 'a',
        age: 1
    },
    {
        name: 'b',
        age: 2
    }
];

let proxy = new Proxy(lessons, {
    get(array, key) {
        console.log(array[key - 1]);
    },
    set(array, key, value) {
        array[key] = value;
    }
})

proxy[1] = {
    name: 'c',
    age: 3
}
console.log(proxy);
```

# 创建对象的几种方式

```jsx
// 工厂模式
function Person(name) {
    var o = new Object()
    o.name = name
    o.bar = function () {
        console.log(o.name);
    }
    return o

}
const p1 = Person('YC')
p1.bar()
// 构造函数
function Person(name) {
    this.name = name
    this.bar = function () {
        console.log(this.name);
    }
}
const p2 = new Person('YC')
p2.bar()
//原型创建模式
function Person() {

}
Person.prototype.name = 'Yc'
const p3 = new Person()
console.log(p3.name);
//原型创建+构造函数
function Person(name) {
    this.name = name
}
Person.prototype.bar = function () {
    console.log(this.name);
}
const p4 = new Person('YC')
p4.bar()
```

# **精度丢失**

`0.1 + 0.2 === 0.3 // false`

0.1和0.2转换成二进制后会无限循环

```
0.1 -> 0.0001100110011001...(无限循环)
0.2 -> 0.0011001100110011...(无限循环)
```

但是由于IEEE 754尾数位数限制，需要将后面多余的位截

然后在进行对阶运算得到

```jsx
0.0100110011001100110011001100110011001100110011001100
```

结果转换成十进制之后就是0.30000000000000004，这样就有了前面的“秀”操作：0.1 + 0.2 != 0.3

**如何解决精度丢失问题**

1. 把计算数字转成整数
    1. 第三方库

# 数组的方法

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1024c906-896c-4dad-8504-1c6da0545d99/Untitled.png)

```jsx
创建数组方法
let arr1 = new Array()
let arr2 = ['1', '2', '3']

检测数组
console.log(Array.isArray(arr1)); //true
console.log(arr1 instanceof Array); //true

转换方法
console.log(arr2.valueOf());
console.log(arr2.toString());

push、 pop、 unshift、 shift
push接受任意数量的参数， 添加到数组末尾， 返回新数组的长度
pop删除数组最后一项， 返回删除的项
unshift接受任意数量的参数， 添加到数组头部， 返回新数组的长度
shift删除数组第一项， 返回删除的项

find() / findIndex()（查找到就停止查找）
find() 方法返回数组中满足提供的测试函数的第一个元素的值。 否则返回 undefined
findIndex() 方法返回数组中满足提供的测试函数的第一个元素的索引。 若没有找到对应元素则返回 - 1

查找元素indexOf()
验证数组中是否含有某个元素， 返回第一个匹配到的元素在数组中所在的位置， 如果没有， 则返回 - 1

includes() 方法用来判断一个数组是否包含一个指定的值， 根据情况， 如果包含则返回 true， 否则返回 false。

reverse、 sort、 concat、 slice
reverse反转数组的顺序， 并返回重新排序之后的数组， 原数组会被改变
sort排序， 可根据升序或者降序
concat没有传递参数， 那么只是复制当前数组并返回副本， 原数组不变， 传递一个元素（ 数组） 或多个元素（ 数组）, 会将其合并到arr中， 返回新数组， 原数组不变
slice剪切数组， 返回剪切之后的数组， **元素不会改变（返回剪切的数组，原数组不改变）**

splice(返回剪切的元素，原元素改变)
删除： arr.splice(index, num)
传入两个参数， 第一个为位置（ 数组下标）， 第二个为删除的项数， 可以删除任意项， 返回删除元素组成的数组， **原数组变了**
插入： arr.splice(index, 0, item)
传入3个参数，[起始位置（ 数组下标） | 要删除的项数 为0 | 要插入的元素]， 最终返回删除掉的元素组成的数组， 因为这里删除项数为0， 因此会返回空数组
替换： arr.splice(index, num, item)
传入3个参数，[起始位置 | 要删除的项数 | 要插入的任意项数]， 最终返回删除掉的元素组成的数组

高阶函数map, filter, reduce, forEach
**map返回一个新数组， 数组中的元素为原始数组元素调用函数处理后的值**。 映射一一对应 n => n元素个数不会少
console.log(arr2.map(item => item >= 2));
filter把传入的函数依次作用于每个元素， **然后根据返回值是true还是false决定保留还是丢弃该元素**。 n => ? 元素个数会少
console.log(arr2.filter(item => item >= 2));
reduce三个参数, **数组中的每个值（ 从左到右） 开始缩减， 最终计算为一个值。** n => 1 **做一些计算**
const arr3 = arr2.reduce((pre, cur, index) => {
    return parseInt(pre) + parseInt(cur)
})
console.log(arr3);

```

# 静态属性和静态方法

静态属性和静态方法是描述类的，都只能通过类来访问，构造的函数对象也访问不了

```jsx
class Person {
    static age = 'Yc'
}
Person.getAge = function () {
    console.log('123');
}
const p1 = new Person()
console.log(Person.age); //YC
console.log(p1.age); //undefined
Person.getAge() //123
p1.getAge() //p1.getAge is not a function
```

# 迭代器与生成器

**迭代器：**

迭代器是帮助我们对某个数据结构进行遍历的对象

迭代器是一个具体的对象，这个对象符合迭代器协议（iterator protocol），在这里协议就是有返回next函数。next返回一个对象里面有两个属性：**done**和**value。**

done（boolen）：

如果迭代器可以产生序列中的下一个值，则为 false。

如果迭代器已将序列迭代完毕，则为 true

**value**

迭代器返回的任何 JavaScript 值。done 为 true 时可省略。

**迭代器：**

```jsx
const friends = ['wang', 'kobe', 'james']
let index = 0
const friendsIterator = {
    next: function () {
        if (index < friends.length) {
            return {
                done: false,
                value: friends[index++]
            }
        } else {
            return {
                done: true
            }
        }
    }
}
console.log(friendsIterator.next());
console.log(friendsIterator.next());
console.log(friendsIterator.next());
console.log(friendsIterator.next());
console.log(friendsIterator.next());
```

**可迭代对象：**

当一个对象实现了迭代器协议时，它就是一个可迭代对象，这个对象要实现@@iterator方法，在代码中使用Symbol.iterator访问该属性（**把迭代器和迭代对象放在一个对象里称这个对象为可迭代对象**）

**可迭代对象应用**：

for...of... ， yield, 展开语法， 结构赋值，就是调用内部@@iterator方法

创建对象：new Map（）， new WeakMap（）， new Set（）， new WeakSet（）

方法调用：Promise.all(iterable), Promise.race(iterable), Array.from(iterable)

```jsx
const info = {
    friends: ['wang', 'kobe', 'james'],
    index: 0,
    [Symbol.iterator]: function () {
        return {
            friends: this.friends,
            index: this.index,
            
: function () {
                return (this.index < this.friends.length) ? {
                    done: false,
                    value: this.friends[this.index++]
                } : {
                    done: true,
                    value: undefined
                }
            }
        }
    }

}
const iterable = info[Symbol.iterator]()
console.log(iterable.next());
console.log(iterable.next());
console.log(iterable.next());
nsole.log(iterable.next());
console.log(iterable.next());

for (let item of info) {
    console.log(item);
}
```

# this的指向

**默认绑定**：独立函数调用

下面代码输出`Jenny`，原因是调用函数的对象在`window`中（在严格模式下就是找不到引用）

```jsx
var name = 'Jenny';
function person() {
    return this.name;
}
console.log(person());  //Jenny
```

**隐式绑定**：通过某个对象发起的函数调用，绑定上一级对象。（隐式丢失：将一个函数给到另一个）

```jsx
	
function test() {
  console.log(this.x);
}

var obj = {};
obj.x = 1;
obj.m = test;

obj.m(); // 1
```

这个函数中包含多个对象，尽管这个函数是被最外层的对象所调用，`this`指向的也只是它上一级的对象

下面的代码中，`this`的上一级对象为`b`，`b`并内部没有`a`的定义，所以输出`undefined`

```jsx

var o = {
    a:10,
    b:{
        fn:function(){
            console.log(this.a); //undefined
        }
    }
}
o.b.fn();
```

```jsx
var o = {
    a:10,
    b:{
        a:12,
        fn:function(){
            console.log(this.a); //undefined
            console.log(this); //window
        }
    }
}
var j = o.b.fn;
j();

```

此时，这里的所有人需要记住，`this`永远指向的是它没有指向对象，虽然是对象的方法，但最终给了时间并执行，最终指向指向`window`

**显示绑定**：明确this绑定的对象 call/bind

**new绑定:** 

**优先级：new>bind>**隐式绑定>默认绑定

箭头函数内的this对象，就是定义该函数时所在作用域指向的对象，匿名函数this指向window

# new操作时内部有什么操作

1. 在内存创建一个新的对象
2. 这个对象的[[prototype]]属性被赋值为该构造函数的prototype属性（obj.__proto__= Function.prototype ）
3. 构造函数内部的this，会指向创建出来的新对象
4. 执行函数内部的代码
5. 如果构造函数没返回对象，则返回创建出的对象
    
    ```jsx
    function _new(fn, ...arg) {
        const obj = Object.create(fn.prototype);
        const ret = fn.apply(obj, arg);
        return ret instanceof Object ? ret : obj;
    }
    ```
    

# 箭头函数和普通函数区别

1. 箭头函数没有prototype属性。
2. 箭头函数也不绑定this，定义的时候就定义好了
3. 因为没有prototype和this就不能当成构造函数
4. 箭头函数不绑定arguments，可以使用剩余参数解决

# 原型方法和对象方法优先级

**结论：自己的优先级更高**

```jsx
let obj = {
    render() {
        console.log('自己的render');
    }
};
obj.__proto__.render = function () {
    console.log('长辈的render');
};

obj.render() //自己的render
```

# ****前端离线化几种常用的方案****

1. ****Application Cache（已废弃）****
2. ****service-worker****

[Service Worker API - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Service_Worker_API)

它提供独立的后台 JS 线程，是一种特殊的 worker 上下文访问环境。我们可以在 ****service-worker**** 安装激活的生命周期中，按需填充缓存资源，然后在 发送网络请求的事件中，拦截 http 请求，将缓存资源或者自定义消息返回给页面。

出于安全考量，Service workers只能由HTTPS承载，毕竟修改网络请求的能力暴露给中间人攻击会非常危险

注册**[下载、安装和激活](https://developer.mozilla.org/zh-CN/docs/Web/API/Service_Worker_API#%E4%B8%8B%E8%BD%BD%E3%80%81%E5%AE%89%E8%A3%85%E5%92%8C%E6%BF%80%E6%B4%BB)**

# call/apply/bind

1. **bind**
    
    bind方法和call很相似，第一参数也是`this`的指向，后面传入的也是一个参数列表(但是这个参数列表可以分多次传入)
    
    改变`this`指向后不会立即执行，而是返回一个永久改变`this`
    
    ```jsx
    function.bind(thisArg[, arg1[, arg2[, ...]]])
    ```
    
    **返回值**：返回一个原函数的拷贝，并拥有指定的 **`this`** 值和初始参数。
    
2. **call**
    
    **`call()`**方法使用一个指定的 `this`值和单独给出的一个或多个参数来调用一个函数。
    
    该方法的语法和作用与 `[apply()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)`方法类似，只有一个区别，就是 `call()`方法接受的是**一个参数列表**，而 `apply()`方法接受的是**一个包含多个参数的数组**。
    
    ```jsx
    function.call(thisArg, arg1, arg2, ...)
    ```
    
3. apply
    
    `apply`接受两个参数，第一个参数是`this`的指向，第二个参数是函数接受的参数，以数组的形式传入
    
    改变`this`指向后原函数会立即执行
    

**小结：**

从上面可以看到，`apply`、`call`、`bind`三者的区别在于：

- 三者都可以改变函数的`this`对象指向
- 三者第一个参数都是`this`要指向的对象，如果如果没有这个参数或参数为`undefined`或`null`，则默认指向全局`window`
- 三者都可以传参，但是`apply`是数组，而`call`是参数列表，且`apply`和`call`是一次性传入参数，而`bind`可以分为多次传入
- `bind`是返回绑定this之后的函数，`apply`、`call` 则是立即执行，**call的性能比apply好一些**

# ****DataView视图****

**`DataView`**视图是一个可以从 二进制`[ArrayBuffer](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)`对象中读写多种数值类型的底层接口，使用它时，不用考虑不同平台的[字节序](https://developer.mozilla.org/zh-CN/docs/Glossary/Endianness)
问题。

```jsx
new DataView(buffer [, byteOffset [, byteLength]])
//buffer一个 已经存在的**ArrayBuffer** 或 **SharedArrayBuffer**  对象，DataView 对象的数据源。
//byteOffset 可选 此 DataView 对象的第一个字节在 buffer 中的字节偏移。如果未指定，则默认从第一个字节开始。
//byteLength 可选此 DataView 对象的字节长度。如果未指定，这个视图的长度将匹配buffer的长度。
```

返回值： 指定数据缓存区的新`DataView`对象

你可以把返回的对象想象成一个二进制字节缓存区 array buffer 的“解释器”——它知道如何在读取或写入时正确地转换字节码。这意味着它能在二进制层面处理整数与浮点转化、字节顺序等其他有关的细节问题。

# 数组去重

1. 利用ES6 Set去重（ES6chang yon）
    
    ```jsx
    function unique(arr) {
        return Array.from(new Set(arr))
    }
    let a = [1, 1, 2, 3, 4, 4]
    console.log(unique(a));
    ```
    
2. 利用includes
    
    ```jsx
    function unique(arr) {
        let b = []
        arr.forEach(item => {
            if (!b.includes(item)) {
                b.push(item)
            }
        });
        return b
    }
    ```
    
3. indexOf
4. filter     

# 防抖节流

防抖和节流本质上都是为了优化体验，所以需要对这类事件（浏览器的 `resize`、`scroll`、`keypress`、`mousemove` 等事件）进行调用次数的限制

**定义：**

- 防抖: n 秒后再执行该事件，若在 n 秒内被重复触发，则重新计时
    - 搜索框搜索输入。只需用户最后一次输入完，再发送请求
    - 手机号、邮箱验证输入检测
    - 窗口大小`resize`。只需窗口调整完成后，计算窗口大小。防止重复渲染。
- 节流: n 秒内只运行一次，若在 n 秒内重复触发，只有一次生效
    - 滚动加载，加载更多或滚到底部监听
    - 搜索框，搜索联想功能
    - 屏幕自适应resize

1. 防抖
    
    ```jsx
    function debounce(fn, delay) {
        let timer = null
        return function() {
            let args = arguments
            let _this = this
            if(timer) clearTimeout(timer)
            timer = setTimeout(() => {
                fn.call(_this, args)
            }, delay)
        }
    }
    ```
    
2. 节流
    
    ```jsx
    function throttled(fn, delay) {
        let timer = null
        return function(...args) {
            let _this = this
            if(!timer) {
                timer = setTimeout(() => {
                    fn.call(_this, args)
                    timer = null
                }, delay)
            }
        }
    }
    ```
    

# 柯里化函数

[柯里化（Currying）](https://en.wikipedia.org/wiki/Currying)是一种关于函数的高阶技术。它不仅被用于 JavaScript，还被用于其他编程语言。

柯里化是一种函数的转换，它是指将一个函数从可调用的 `f(a, b, c)` 转换为可调用的 `f(a)(b)(c)`。

柯里化不会调用函数。它只是对函数进行转换。

让我们先来看一个例子，以更好地理解我们正在讲的内容，然后再进行一个实际应用。

我们将创建一个辅助函数 `curry(f)`，该函数将对两个参数的函数 `f` 执行柯里化。换句话说，对于两个参数的函数 `f(a, b)` 执行 `curry(f)` 会将其转换为以 `f(a)(b)` 形式运行的函数：

```jsx
function curry(f) { // curry(f) 执行柯里化转换
  return function(a) {
    return function(b) {
      return f(a, b);
    };
  };
}

// 用法
function sum(a, b) {
  return a + b;
}

let curriedSum = curry(sum);

alert( curriedSum(1)(2) ); // 3
```

正如你所看到的，实现非常简单：只有两个包装器（wrapper）。

- `curry(func)` 的结果就是一个包装器 `function(a)`。
- 当它被像 `curriedSum(1)` 这样调用时，它的参数会被保存在词法环境中，然后返回一个新的包装器 `function(b)`。
- 然后这个包装器被以 `2` 为参数调用，并且，它将该调用传递给原始的 `sum` 函数。

# 模拟for...of...

```jsx
const obj = [1, 2, 3, 4, 5, 6, 7]
function forOf(obj) {
    let it = obj[Symbol.iterator]()
    console.log(it);
    result = it.next()
    while (!result.done) {
        console.log(result.value);
        result = it.next()
    }
}
forOf(obj)//1 2 3 4 5 6 7
```

# super关键字

`super` 有两种用法：

- 在子类构造函数中直接 `super()` 调用父类构造器，且在子类构造器中出现 this 引用之前必须先调用 `super()`;
- 在方法中使用 `super.XXX` 调用父类方法/属性

[超级深入理解 JS 中的 super： A super deep dive into JS super - 掘金](https://juejin.cn/post/6867393929361227789)

# 用reduce实现promise队列

```jsx
var dayjs = require('dayjs')

function methodThatReturnsAPromise(nextID) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log(`Resolve! ${dayjs().format('hh:mm:ss')}`);
            resolve();
        }, 1000);
    });
}
[1, 2, 3].reduce((accumulatorPromise, nextID) => {
    console.log(`Loop! ${dayjs().format('hh:mm:ss')}`);
    return accumulatorPromise.then(() => {
        return methodThatReturnsAPromise(nextID);
    });
}, Promise.resolve());
```

# **JavaScript深入之词法作用域和动态作用域**

**作用域**

作用域是指程序源代码中定义变量的区域。

作用域规定了如何查找变量，也就是确定当前执行代码对变量的访问权限。

JavaScript 采用**词法作用域(lexical scoping)**，也就是**静态作用域**。

**静态作用域与动态作用域**

因为 JavaScript 采用的是词法作用域，**函数的作用域在函数定义的时候就决定了**。

而与词法作用域相对的是动态作用域，函数的作用域是在函数调用的时候才决定的。

让我们认真看个例子就能明白之间的区别

```jsx
var value = 1;

function foo() {
    console.log(value);
}

function bar() {
    var value = 2;
    foo();
}

bar();

// 结果是 ???
```

假设JavaScript采用静态作用域，让我们分析下执行过程：

执行 foo 函数，先从 foo 函数内部查找是否有局部变量 value，如果没有，就根据书写的位置，查找上面一层的代码，也就是 value 等于 1，所以结果会打印 1。

假设JavaScript采用动态作用域，让我们分析下执行过程：

执行 foo 函数，依然是从 foo 函数内部查找是否有局部变量 value。如果没有，就从调用函数的作用域，也就是 bar 函数内部查找 value 变量，所以结果会打印 2。

前面我们已经说了，JavaScript采用的是静态作用域，所以这个例子的结果是 1。

**动态作用域**

也许你会好奇什么语言是动态作用域？

bash 就是动态作用域，不信的话，把下面的脚本存成例如 scope.bash，然后进入相应的目录，用命令行执行 `bash ./scope.bash`，看看打印的值是多少。

# ****说说JavaScript中的事件模型****

****一、事件与事件流****

`javascript`中的事件，可以理解就是**在`HTML`文档或者浏览器中发生的一种交互操作**，**使得网页具备互动性**， 常见的有加载事件、鼠标事件、自定义事件等

由于`DOM`是一个树结构，如果在父子节点绑定事件时候，当触发子节点的时候，就存在一个顺序问题，这就涉及到了事件流的概念

事件流都会经历三个阶段：

- 事件捕获阶段(capture phase)
- 处于目标阶段(target phase)
- 事件冒泡阶段(bubbling phase)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b45b8e14-8437-4a90-bab6-dbc7193f38d9/Untitled.png)

事件冒泡是一种**从下往上**的传播方式，由最**具体的元素（触发节点）**然后逐渐向上传播到最不具体的那个节点，也就是`DOM`
中最高层的父节点

事件捕获与事件冒泡相反，事件最开始由不太具体的节点最早接受事件, 而最具体的节点（触发节点）最后接受事件

二 **标准事件模型：**

在该事件模型中，一次事件共有三个过程:

- 事件捕获阶段：事件从`document`一直向下传播到目标元素, 依次检查经过的节点是否绑定了事件监听函数，如果有则执行
- 事件处理阶段：事件到达目标元素, 触发目标元素的监听函数
- 事件冒泡阶段：事件从目标元素冒泡到`document`, 依次检查经过的节点是否绑定了事件监听函数，如果有则执行

事件绑定监听函数的方式如下:

`addEventListener(eventType, handler, useCapture)`

事件移除监听函数的方式如下:

`removeEventListener(eventType, handler, useCapture)`

参数如下：

- `eventType`指定事件类型(不要加on)
- `handler`是事件处理函数
- `useCapture`是一个`boolean`用于指定是否在捕获阶段进行处理，一般设置为`false`

举个例子：

```jsx
var btn = document.getElementById('.btn');
btn.addEventListener(‘click’, showMessage, false);
btn.removeEventListener(‘click’, showMessage, false);
```

# ****解释下什么是事件代理？应用场景？****

1. 事件代理是什么
    
    事件代理，俗地来讲，就是把一个元素响应事件（`click`、`keydown`......）的函数委托到另一个元素
    
    前面讲到，事件流的都会经过三个阶段： 捕获阶段 -> 目标阶段 -> 冒泡阶段，而事件委托就是在**冒泡阶段**完成
    
    **事件委托，会把一个或者一组元素的事件委托到它的父层或者更外层元素上，真正绑定事件的是外层元素，而不是目标元素**
    
    当事件响应到目标元素上时，会通过事件冒泡机制从而触发它的外层元素的绑定事件上，然后在外层元素上去执行函数
    
2. 应用场景
    
    如果我们有一个列表，列表之中有大量的列表项，我们需要在点击列表项的时候响应一个事件
    
    ```jsx
    <ul id="list">
      <li>item 1</li>
      <li>item 2</li>
      <li>item 3</li>
      ......
      <li>item n</li>
    </ul>
    ```
    
    如果给每个列表项一一都绑定一个函数，那对于内存消耗是非常大的
    
    ```jsx
    // 获取目标元素
    const lis = document.getElementsByTagName("li")
    // 循环遍历绑定事件
    for (let i = 0; i < lis.length; i++) {
        lis[i].onclick = function(e){
            console.log(e.target.innerHTML)
        }
    }
    ```
    
    这时候就可以事件委托，把点击事件绑定在父级元素`ul`上面，然后执行事件的时候再去匹配目标元素
    
    ```jsx
    // 给父层元素绑定事件
    document.getElementById('list').addEventListener('click', function (e) {
        // 兼容性处理
        var event = e || window.event;
        var target = event.target || event.srcElement;
        // 判断是否匹配目标元素
        if (target.nodeName.toLocaleLowerCase === 'li') {
            console.log('the content is: ', target.innerHTML);
        }
    });
    ```
    
    还有一种场景是上述列表项并不多，我们给每个列表项都绑定了事件
    
    但是如果用户能够随时动态的增加或者去除列表项元素，那么在每一次改变的时候都需要重新给新增的元素绑定事件，给即将删去的元素解绑事件
    
    如果用了事件委托就没有这种麻烦了，因为事件是绑定在父层的，和目标元素的增减是没有关系的，执行到目标元素是在真正响应执行事件函数的过程中去匹配的
    
    举个例子：
    
    下面`html`结构中，点击`input`可以动态添加元素
    
    ```jsx
    <input type="button" name="" id="btn" value="添加" />
    <ul id="ul1">
        <li>item 1</li>
        <li>item 2</li>
        <li>item 3</li>
        <li>item 4</li>
    </ul>
    ```
    
    使用事件委托
    
    ```jsx
    const oBtn = document.getElementById("btn");
    const oUl = document.getElementById("ul1");
    const num = 4;
    
    //事件委托，添加的子元素也有事件
    oUl.onclick = function (ev) {
        ev = ev || window.event;
        const target = ev.target || ev.srcElement;
        if (target.nodeName.toLowerCase() == 'li') {
            console.log('the content is: ', target.innerHTML);
        }
    
    };
    
    //添加新节点
    oBtn.onclick = function () {
        num++;
        const oLi = document.createElement('li');
        oLi.innerHTML = `item ${num}`;
        oUl.appendChild(oLi);
    };
    ```
    
    可以看到，使用事件委托，在动态绑定事件的情况下是可以减少很多重复工作的
    
    3. ****总结****
        
        适合事件委托的事件有：`click`，`mousedown`，`mouseup`，`keydown`，`keyup`，`keypress`
        
        从上面应用场景中，我们就可以看到使用事件委托存在两大优点：
        
        - 减少整个页面所需的内存，提升整体性能
        - 动态绑定，减少重复工作
        
        但是使用事件委托也是存在局限性：
        
        - `focus`、`blur`这些事件没有事件冒泡机制，所以无法进行委托绑定事件
        - `mousemove`、`mouseout`这样的事件，虽然有事件冒泡，但是只能不断通过位置去计算定位，对性能消耗高，因此也是不适合于事件委托的
        
        如果把所有事件都用事件代理，可能会出现事件误判，即本不该被触发的事件被绑定上了事件
    
# 事件冒泡和事件捕获

`element.addEventListener(event, function, useCapture)`

第一个参数是需要绑定的事件。第二个参数是触发事件后要执行的函数。第三个参数默认值是false，表示在**事件冒泡阶段**调用事件处理函数;如果参数为true，则表示在**事件捕获阶段**调用处理函数。

**事件冒泡**： 

当给父子元素的同一事件绑定方法的时候，触发子元素身上的事件，执行完毕之后，也会触发父级元素相同的事件。 注意： addEventListener中有三个属性，第三个属性是布尔值。false为事件冒泡，true为事件捕获

因此在事件冒泡的概念下在p元素上发生click事件的顺序应该是**`p -> div -> body -> html -> document`**

**阻止事件冒泡:**

1. **给子级加 event.stopPropagation( )**
2. **在事件处理函数中返回 false(阻止冒泡外，还会阻止事件本身)**
3. **e.target === e.currentTarget**

**事件捕获**：

从上至下到指定元素。当触发子元素身上的事件时，先触发父元素，然后在传递给子元素

**`document -> html -> body -> div -> p`**
    
# 本地存储的几种方式

- cookie
- sessionStorage
- localStorage
- indexedDB

1. cookie
    
    小型文本文件，辨别用户身份而储存在用户本地终端上的数据，是为了解决 `HTTP`无状态导致的问题。
    
    它的大小不超过4kb，内容由key-value组成，不安全，容易被窃取
    
    关于`cookie`常用的属性如下：
    
    - `Domain`指定了 `Cookie` 可以送达的主机名
    - `Path`指定了一个 `URL`路径，这个路径必须出现在要请求的资源的路径中才可以发送 `Cookie` 首部
    - `Expires` 用于设置 Cookie 的过期时间
    - `Max-Age` 用于设置在 Cookie 失效之前需要经过的秒数（优先级比`Expires`高）
        
            
        
        

# class和function的区别

1. `class` 声明会提升，但不会初始化赋值。`Foo` 进入暂时性死区，类似于 `let`、`const` 声明变量。
    
    ```jsx
    const bar = new Bar(); // it's ok
    function Bar() {
        this.bar = 42;
    }
    
    const foo = new Foo(); // ReferenceError: Foo is not defined
    class Foo {
        constructor() {
        this.foo = 42;
        }
    }
    ```
    
2. `class` 声明内部会启用严格模式
    
    ```jsx
    // 引用一个未声明的变量
    function Bar() {
        baz = 42; // it's ok
    }
    const bar = new Bar();
    
    class Foo {
        constructor() {
        fol = 42; // ReferenceError: fol is not defined
        }
    }
    const foo = new Foo();
    ```
    
3. `class` 的所有方法（包括静态方法和实例方法）都是不可枚举的。
    
    ```jsx
    class Foo {
        constructor() {
        this.foo = 42;
        }
        static answer() {
        return 42;
        }
        print() {
        console.log(this.foo);
        }
    }
    const fooKeys = Object.keys(Foo); // []
    const fooProtoKeys = Object.keys(Foo.prototype); // []
    ```
        
# 常用的object对象方法

1. **Object.keys**
2. **Object.values**
3. **Object.entries**
4. **Object.freezen**
5. ****Object.create****
    
    Object.create()方法创建一个新对象，使用现有的对象来提供新创建的对象的__proto__。、
    
6. ****Object.hasOwnProperty****
    
    判断对象自身属性中是否具有指定的属性。
    
7. ****Object.getOwnPropertyNames****
    
    Object.getOwnPropertyNames()方法返回对象的所有自身属性的属性名（包括不可枚举的属性）组成的数组，但不会获取原型链上的属性。
    
8. ****Object.getOwnPropertySymbols****
9. **Object.getPrototypeOf**
    
    **`Object.getPrototypeOf()`**方法返回指定对象的原型（内部`[[Prototype]]`属性的值）
    
10. ****Object.assign****
    
    Object.assign方法用于对象的合并，将源对象（ source ）的所有可枚举属性，复制到目标对象（ target ）
    
11. ****Object.defineProperty****
    
    Object.defineProperty可以用来定义新属性或修改原有的属性