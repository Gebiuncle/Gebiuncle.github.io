## nodejs学习总结
node.js官方文档：http://nodejs.cn/learn 
### 如何使用 Node.js REPL
``` js
node+回车
```
.exit 退出
### 从命令行接收参数
``` js
node app.js run
```
获取参数值的方法是使用 Node.js 中内置的 process 对象。

它公开了 argv 属性，该属性是一个包含所有命令行调用参数的数组。

第一个参数是 node 命令的完整路径。

第二个参数是正被执行的文件的完整路径。

所有其他的参数从第三个位置开始。
![alt 从命令行接收参数](/node.jpg)
### npm包管理工具
安装工具到指定的软件包
``` js
npm install <package-name>
```
--save 安装并添加条目到 package.json 文件的 dependencies。
--save-dev 安装并添加条目到 package.json 文件的 devDependencies。
区别主要是，devDependencies 通常是开发的工具（例如测试的库），而 dependencies 则是与生产环境中的应用程序相关。
### node语义版本控制
语义版本控制的概念很简单：所有的版本都有 3 个数字：x.y.z。

* 第一个数字是主版本。
* 第二个数字是次版本。
* 第三个数字是补丁版本。

## koa知识点
  koa知识点
## express
  express