## TypeScript 备忘笔记

### 定义类型

#### 通过接口定义

接口的作用是为自定义类型命名

```typescript
interface User {
  readonly age: number; // 只读参数
  name: string;
  company?: string; // 可选性参数
  [propName: string]: any; // 任意参数
};

const user: User = {
  age: 99,
  name: 'ecmadao'，
  aaa: 123 // 因为支持可选参数
};
```

```typescript
interface Equal {
    (a: number, b: number): boolean
};

// 箭头函数
const isEqual: Equal = (a, b) => a === b;
// or
let isEqual: Equal;
isEqual = function(a, b) {
    return a === b;
} 
```

##### 类与接口

```typescript
interface BaseClass {
    val: number
};

class ClassA implements BaseClass {
    val: number // 如果不实现这一行则会报错
}
```

##### 接口继承

```typescript
interface Shape {
    color: string;
}

interface Square extends Shape {
    sideLength: number;
}

// you can
const square = <Square>{};
square.color = "blue";
square.sideLength = 10;

// or
const square: Square = {
    color: 'blue',
    sideLength: 10
};
```

#### 通过 `type` 定义

```typescript
type func = (a: number) => void;

// 如果将函数作为参数传入
function getVal(a: func): number[] {
  return [1, 2];
};
```

### 函数

```typescript
// b 代表接收任意多个参数
type myFunc = (a: number, ...b: number[]) => number;
// 使用
const myFunc: myFunc = (a) => a;
const myFunc: myFunc = (a, b, c) => a + b + c;


// 具有可选参数的函数定义
type myFunc2 = (a: number, b?: string) => number;
// 使用
const myFunc2: myFunc2 = (age) => age;
const myFunc2: myFunc2 = (age, name) => age;
```

### 泛型

#### 泛型函数

对于有的函数而言，我们并不知道它会接受什么类型的参数，这时可以用 `any` 类型来表示。但是如果我们还期待参数和返回值的类型一样，则需要使用`泛型`。

```typescript
// 不知道这个函数会接受什么类型的参数，但可以确定其返回值的类型和参数类型一样
function identity<T>(arg: T): T {
    return arg;
}
// or
const identity = <T>(arg: T): T => arg;

// 使用
const output = identity<string>("myString"); // 确定为 string 类型

// 如果定义参数为 Array
const identity = <T>(arg: T[]): T[] => arg;
```

#### 泛型接口

```typescript
interface identityFn {
    <T>(arg: T): T
}

const func: identityFn = function(a) {
    return a;
}
```

可以再进一步，把参数类型提取到外面：

```typescript
interface identityFn<T> {
    (arg: T): T
}

const func: identityFn<number> = function(a) {
    return a;
}
```

#### 泛型类

```typescript
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };
```

### 迭代

```typescript
const list = [4, 5, 6];

// for..of 迭代键对应的 value
for (let i of list) {
  console.log(i); // 4, 5, 6
}

// for..in 迭代键
for (let i in list) {
  console.log(i); // 0, 1, 2
}
```

