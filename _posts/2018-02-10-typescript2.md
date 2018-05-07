---
layout: post
title:  typeScript 类
date:   2018-02-10 19:58:00 +0800
categories: TypeScript
tag: TypeScript
---

* content
{:toc}

ES6 的类(Class)不明白可以看[这里](http://es6.ruanyifeng.com/#docs/class)

TypeScript 中 Class 稍有不同。

### 1.公共，私有与受保护的修饰符

参数属性可以方便地让我们在一个地方定义并初始化一个成员。

注意构造函数中的 `private` 关键字。通过给构造函数参数添加一个访问限定符来声明。 使用 `private` 限定一个参数属性会声明并初始化一个私有成员；对于 `public` 和 `protected` 来说也是一样。

```typescript
class Animals {
    constructor(private name: string) { }
    move(distanceInMeters: number) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}

let p = new Animals("Pandan")
p.move(22)
```

#### 1.1 public

在TypeScript里，成员都默认为 `public`，也可以明确的将一个成员标记成 `public`。

```typescript
class Student {
    fullName: string;
    constructor(public firstName, public middleInitial, public lastName) {
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}

interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person : Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

var user = new Student("Jane", "M.", "User");

document.body.innerHTML = greeter(user);
```

#### 1.2 private

当成员被标记成 `private`时，它就不能在声明它的类的外部访问。

#### 1.3 protected

`protected` 修饰符与 `private` 修饰符的行为很相似，但有一点不同，`protected` 成员在派生类中仍然可以访问。

```typescript
class Person {
    protected name: string;
    constructor(name: string) { this.name = name; }
}

class Employee extends Person {
    private department: string;
    constructor(name: string, department: string) {
        super(name)
        this.department = department;
    }

    public getElevatorPitch() {
        return `Hello, my name is ${this.name} and I work in ${this.department}.`;
    }
}

let howard = new Employee("Howard", "Sales");
console.log(howard.getElevatorPitch());
console.log(howard.name); // 错误
```

构造函数也可以被标记成 `protected`。 这意味着这个类不能在包含它的类外被实例化，但是能被继承。

#### 1.4 readonly

可以使用 `readonly` 关键字将属性设置为只读的。

只读属性必须在声明时或构造函数里被初始化。

#### 1.5 存取器 get 和 set

```typescript
let passcode = "secret passcode";

class Employee {
    private _fullName: string;

    get fullName(): string {
        return this._fullName;
    }

    set fullName(newName: string) {
        if (passcode && passcode == "secret passcode") {
            this._fullName = newName;
        }
        else {
            console.log("Error: Unauthorized update of employee!");
        }
    }
}
```

注意：存取器要求你将编译器设置为输出 `ECMAScript 5` 或更高。 不支持降级到 `ECMAScript 3`。 其次，只带有 `get` 不带有 `set` 的存取器自动被推断为 `readonly`。 这在从代码生成 `.d .ts` 文件时是有帮助的，因为利用这个属性的用户会看到不允许够改变它的值。

### 2.静态属性

我们也可以创建类的静态成员，使用 `static` 关键字，这些属性存在于类本身上面而不是类的实例上。

```typescript
class Grid {
    static origin = {x: 0, y: 0};
    calculateDistanceFromOrigin(point: {x: number; y: number;}) {
        let xDist = (point.x - Grid.origin.x);
        let yDist = (point.y - Grid.origin.y);
        return Math.sqrt(xDist * xDist + yDist * yDist) / this.scale;
    }
    constructor (public scale: number) { }
}

let grid1 = new Grid(1.0);  // 1x scale
let grid2 = new Grid(5.0);  // 5x scale

console.log(grid1.calculateDistanceFromOrigin({x: 10, y: 10}));
console.log(grid2.calculateDistanceFromOrigin({x: 10, y: 10}));
```

### 3.抽象类

抽象类做为其它派生类的基类使用。 它们一般不会直接被实例化。 不同于接口，抽象类可以包含成员的实现细节。 `abstract` 关键字是用于定义抽象类和在抽象类内部定义抽象方法。

```typescript
abstract class Animal {
    abstract makeSound(): void;
    move(): void {
        console.log('roaming the earch...');
    }
}
```

抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。 抽象方法的语法与接口方法相似。 两者都是定义方法签名但不包含方法体。 然而，抽象方法必须包含 `abstract` 关键字并且可以包含访问修饰符。