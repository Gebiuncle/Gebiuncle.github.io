## 函数使用
``` ts
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
  if(typeof x === 'number') {
    return Number(x.toString().split('').reverse().join(''))
  }else if(typeof x === 'string'){
    return x.split('').reverse().join('')
  }
} // 函数重载

document.body.innerHTML = reverse(123) + '<br>'
document.body.innerHTML += reverse('abc')
```
## 接口使用
``` ts
class Student {
  fullName: String;
  constructor(public firstName, public middle, public lastName) {
    this.fullName = this.firstName + ' ' + this.lastName
  }
}

interface Person {
  firstName: String;
  lastName: String;
}

function sayHello(person: Person) {
  return 'hello' + person.firstName + person.lastName
}

const user = new Student('lei', '', 'run');

document.body.innerHTML = sayHello(user)
```
## 类型断言
``` ts
interface Cat {
  name: string;
  run(): void;
}

interface Fish {
  name: string;
  swim(): void;
}

function isCat(animal: Cat | Fish) {
  if(typeof (animal as Cat).run === 'function') {
    return true;
  }else {
    return false;
  }
}

let run: Cat = {
  name: 'run', 
  run() {return this.name}
};

document.body.innerHTML = isCat(run).toString();

(window as any).foo = 1
```
## 类使用
``` ts
class Person {
  name: String;
  age: Number;
  constructor(theName: String, theAge: Number) {
    this.name = theName
    this.age = theAge
  };
  sayHello() {
    return `My name is ${this.name}, age is ${this.age} <br>`
  }
}

class Man extends Person {
  constructor(name: String, age: Number) {
    super(name, age)
  }
}

class Woman extends Person {
  work: String;
  constructor(theWork: String) {
    super('Jane', 18)
    this.work = theWork
  };
  sayWork() {
    return `${this.name}, work is ${this.work}`
  }
}

let man = new Man('run', 18)
document.body.innerHTML = man.sayHello()

let person = new Man('lei', 20)
document.body.innerHTML += person.sayHello()

let woman = new Woman('teacher')
document.body.innerHTML += woman.sayHello() + woman.sayWork()
```