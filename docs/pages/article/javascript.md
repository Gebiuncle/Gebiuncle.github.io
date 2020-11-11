## 写一个深拷贝
### 什么是浅拷贝：  
::: tip 浅拷贝
创建一个新对象，这个对象有着原始对象属性值的一份精确拷贝。如果属性是基本类型，拷贝的就是基本类型的值，如果属性是引用类型，拷贝的就是内存地址 ，所以如果其中一个对象改变了这个地址，就会影响到另一个对象。
:::
实现浅拷贝的方法  
1.for in 
``` js
function clone(target) {
    let cloneTarget = {};
    for (const key in target) {
        cloneTarget[key] = target[key];
    }
    return cloneTarget;
}
```
2.Object.assign()  
3.直接=赋值  
### 什么是深拷贝
::: tip 深拷贝
将一个对象从内存中完整的拷贝一份出来,从堆内存中开辟一个新的区域存放新对象,且修改新对象不会影响原对象
:::
实现深拷贝的方法  
乞丐版：通过JSON.parse()  
手写一个完整的深拷贝  
需要考虑的地方：1.object是数组 2.对象存在循环引用的情况 3.性能问题（使用while代替for in 循环）
* 如果是原始类型，无需继续拷贝，直接返回
* 如果是引用类型，创建一个新的对象，遍历需要克隆的对象，将需要克隆对象的属性执行深拷贝后依次添加到新对象上。
``` js
function clone(target) {
    if (typeof target === 'object') {
        let cloneTarget = {};
        for (const key in target) {
            cloneTarget[key] = clone(target[key]);
        }
        return cloneTarget;
    } else {
        return target;
    }
};
```
``` js
function clone(target) {
    if (typeof target === 'object') {
        let cloneTarget = Array.isArray(target) ? [] : {};
        for (const key in target) {
            cloneTarget[key] = clone(target[key]);
        }
        return cloneTarget;
    } else {
        return target;
    }
};
```
解决循环引用问题，我们可以额外开辟一个存储空间，来存储当前对象和拷贝对象的对应关系，当需要拷贝当前对象时，先去存储空间中找，有没有拷贝过这个对象，如果有的话直接返回，如果没有的话继续拷贝，这样就巧妙化解的循环引用的问题。
这个存储空间，需要可以存储key-value形式的数据，且key可以是一个引用类型，我们可以选择Map这种数据结构：  
* 检查map中有无克隆过的对象  
* 有 - 直接返回  
* 没有 - 将当前对象作为key，克隆对象作为value进行存储  
* 继续克隆
``` js
function clone(target, map = new Map()) {
    if (typeof target === 'object') {
        let cloneTarget = Array.isArray(target) ? [] : {};
        if (map.get(target)) {
            return map.get(target);
        }
        map.set(target, cloneTarget);
        for (const key in target) {
            cloneTarget[key] = clone(target[key], map);
        }
        return cloneTarget;
    } else {
        return target;
    }
};
```
``` js
function forEach(array, iteratee) {
    let index = -1;
    const length = array.length;
    while (++index < length) {
        iteratee(array[index], index);
    }
    return array;
}
function clone(target, map = new WeakMap()) {
    if (typeof target === 'object') {
        const isArray = Array.isArray(target);
        let cloneTarget = isArray ? [] : {};

        if (map.get(target)) {
            return map.get(target);
        }
        map.set(target, cloneTarget);

        const keys = isArray ? undefined : Object.keys(target);
        forEach(keys || target, (value, key) => {
            if (keys) {
                key = value;
            }
            cloneTarget[key] = clone2(target[key], map);
        });

        return cloneTarget;
    } else {
        return target;
    }
}

```
上边只考虑了两种数据类型 object、array
## 判断数据类型的方法
### 1.typeof  
typeof操作符可以准确判断一个变量是否为下面几个原始类型：  
``` js
typeof 'ConardLi'  // string
typeof 123  // number
typeof true  // boolean
typeof Symbol()  // symbol
typeof undefined  // undefined
```
判断函数类型  
``` js
typeof function(){}  // function
```  
当你用typeof来判断引用类型时似乎显得有些乏力了：  
``` js
typeof [] // object
typeof {} // object
typeof new Date() // object
typeof /^\d*$/; // object
typeof null === 'obj'
```
### 2.instanceof
instanceof操作符可以帮助我们判断引用类型具体是什么类型的对象  
``` js
[] instanceof Array // true
new Date() instanceof Date // true
new RegExp() instanceof RegExp // true
```
原型链的几条规则：
* 1.所有引用类型都具有对象特性，即可以自由扩展属性
* 2.所有引用类型都具有一个__proto__（隐式原型）属性，是一个普通对象
* 3.所有的函数都具有prototype（显式原型）属性，也是一个普通对象
* 4.所有引用类型__proto__值指向它构造函数的prototype
* 5.当试图得到一个对象的属性时，如果变量本身没有这个属性，则会去他的__proto__中去找  
instanceof检测数据类型不会很准确，也不能检测基本数据类型。
### 3.使用toString来获取准确的引用类型：
::: tip toString
每一个引用类型都有toString方法，默认情况下，toString()方法被每个Object对象继承。如果此方法在自定义对象中未被覆盖，toString() 返回 "[object type]"，其中type是对象的类型。
:::
![alt 数据类型](/object-type.jpg)
## 防抖与节流
防抖：在规定时间内只触发一次  
防抖的原理就是：你尽管触发事件，但是我一定在事件触发 n 秒后才执行，如果你在一个事件触发的 n 秒内又触发了这个事件，那我就以新的事件的时间为准，n 秒后才执行，总之，就是要等你触发完事件 n 秒内不再触发事件，我才执行，真是任性呐!  
github链接 <https://github.com/mqyqingfeng/Blog/issues/22>
``` js
// 思路:在规定时间内未触发第二次，则执行
function debounce (fn, delay) {
  // 利用闭包保存定时器
  let timer = null
  return function () {
    let context = this
    let arg = arguments
    // 在规定时间内再次触发会先清除定时器后再重设定时器
    clearTimeout(timer)
    timer = setTimeout(function () {
      fn.apply(context, arg)
    }, delay)
  }
}

function fn () {
  console.log('防抖')
}
addEventListener('scroll', debounce(fn, 1000)) 
```
节流： 在规定时间内未触发第二次，则执行
节流的原理：如果你持续触发事件，每隔一段时间，执行一次事件。
``` js
// 思路：在规定时间内只触发一次
function throttle (fn, delay) {
  // 利用闭包保存时间
  let prev = Date.now()
  return function () {
    let context = this
    let arg = arguments
    let now = Date.now()
    if (now - prev >= delay) {
      fn.apply(context, arg)
      prev = Date.now()
    }
  }
}

function fn () {
  console.log('节流')
}
addEventListener('scroll', throttle(fn, 1000)) 
```
## call、apply理解
函数都可以调用 call，说明 call 是函数原型上的方法，所有的实例都可以调用。即: Function.prototype.call。  
* 在 call 方法中获取调用call()函数
* 如果第一个参数没有传入，那么默认指向 window / global(非严格模式)
* 传入 call 的第一个参数是 this 指向的对象，根据隐式绑定的规则，我们知道 obj.foo(), foo() 中的 this 指向 obj;因此我们可以这样调用函数 `thisArgs.func(...args)`
* 返回执行结果
![alt call](/call.jpg)
![alt apply](/apply.jpg)
## jsonp的实现方法
``` js
  // 接收三个参数，要发起请求的URL、请求的参数、回调函数
    function jsonp({ url, params, cb }) {
      // 这里是拼接参数的方法
      let createUrl = () => {
        // 申明一个字符串
        let dataStr = "";
        // for...in循环遍历参数
        for (let k in params) {
          // 进行拼接
          dataStr += `${k}=${params[k]}&`;
        }
        // 最后将回调函数也加上去
        dataStr += `callback=${cb}`;
        // 返回最终的结果
        return `${url}?${dataStr}`;
      };
      // 返回一个Promise对象
      return new Promise(resolve => {
        // 在页面上创建script标签
        let script = document.createElement("script");
        // 将script标签中的src属性设置为上面函数返回的参数
        script.src = createUrl();
        // 将script标签插入到body中
        document.body.appendChild(script);
        // 添加回调
        window[cb] = data => {
          // 成功的回调，会将我们要请求的参数作为参数传入
          resolve(data);
          // 执行完毕后移除script标签
          document.body.removeChild(script);
        };
      });
    }
    jsonp({
      url: "https://sug.so.360.cn/suggest",
      params: { word: "c" },
      cb: "b"
    }).then(res => {
      console.log(res)
    })
```
原理实际上是这样的
``` js
<script src="https://sug.so.360.cn/suggest?callback=back"></script>
<script>
    function back(a) {
        console.log(a)
    }
</srcipt>
```
## 实现一个Promise 
## XSS和CSRF