## 数组去重
1.es6 Set
``` js
let arr = [1, 1, 2, 2, 7, 5, 6, 6];
    arr = Array.from(new Set(arr));
``` 
2. 先将数组排序，然后遍历不等于当前元素就push进新的数组
``` js
let m = [];
   arr.sort();
   m.push(arr[0]);
   for (let i = 1; i < arr.length; i++) {
      if (arr[i] != m[m.length - 1]) {
        m.push(arr[i]);
      }
   }
```
3.通过array.indexOf()方法
``` js
   let arr = [1, 1, 2, 2, 7, 5, 6, 6];
	let m = []
   for (let i = 0; i<arr.length; i++) {
		if(m.indexOf(arr[i])<0){
			m.push(arr[i])
      }
   }
``` 
## 类数组转数组
``` js 
   function arg2arr() {
      // var arr = Array.prototype.slice.call(arguments);
      var arr = [...arguments];
      console.log(arr);
    }
    arg2arr(1, 2, 3);
```
## 数组求和
``` js 
   let sum = 0;
    for (let i = 0; i < arr.length; i++) {
      sum += arr[i];
    }
    sum = arr.reduce((a, b, i) => {
      console.log(a, b, i);
      return a + b;
    });
```
## 数组随机排列
``` js 
   arr.sort(() => {
        console.log(Math.random() - 0.5);
        return Math.random() - 0.5;
      })
```