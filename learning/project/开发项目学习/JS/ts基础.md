# TypeScript

## 什么是 TypeScript？

简而言之，TypeScript 是 JavaScript 的**超集**，它继承了 JavaScript 所有的语法，并且可以编译为纯 JavaScript 。它的目的并不是创造一种全新语言，而是**增强 JavaScript 的功能，使其更适合多人合作的企业级项目。**

既然是超集，那么它**超**在哪里呢？

TypeScript 最大的特点就是引入了**类型系统**，这样就可以在编译为 JavaScript 代码之前由编译器进行类型检查。在这样的条件下，TypeScript 中的变量在声明的时候就**可以指定类型**，编译器在将 TypeScript 代码编译为 JavaScript 代码的时候会**进行类型检查**，若有不符合类型声明的情况则会报错：

```
const fun = (name: string): void => {
    console.log("Hello, " + name);
};

fun(2); // Error!
```

TypeScript 有着**静态类型检查**，具有类型系统，可以在开发时捕获许多常见的错误。通过类型检查，可以在**编码阶段就发现潜在的问题**，减少运行时错误。

但是，在实际开发中，选择 TypeScript 还是 JavaScript 取决于项目的具体需求和团队的实际情况。首先，对于大型项目、需要**长期维护**和**多人协作**的项目，TypeScript 的优势尤为明显。它**提供了类型检查和工具支持**，可以提高代码的可靠性和团队的效率。而对于小型项目或快速原型开发，JavaScript 可能**更适合快速迭代和开发**的需求。其次，如果团队已经熟悉 JavaScript 生态系统，并且没有特别需要 TypeScript 的需求，继续使用 JavaScript 也是一个合理的选择。要考虑团队成员的技术水平和学习成本。

## TypeScript，启动！

Node.js 提供了 npm 包管理器，通过`npm install -g typescript` 即可安装 TypeScript 。通过 `tsc` 命令即可将 TypeScript 代码编译为 JavaScript 代码。但是它并不支持直接运行编译后的代码，而 `ts-node` 正好填补了这一空缺，它封装了 TypeScript 的编译过程，使得 TypeScript 代码无需编译成 JavaScript，就能直接运行 TypeScript 代码。由于这是一个教学文档，本文先不使用 `ts-node`

我们在一个文件夹下启动命令行，输入 `npm install -g typescript` 安装 TypeScript ，之后输入 ` `tsc --init``即可在当前文件夹下初始化 TypeScript 项目。我们会发现当前文件夹下多了一个 `tsconfig.json` 文件，这是 `TypeScript` 项目的**配置文件**，它包含了编译器的配置选项，我们暂时先不动它

在当前文件夹下新建 `index.ts` 文件，并编写：

```
let str: string = "TypeScript";
console.log(`Hello, ${str}!`);
```

之后在命令行输入 `tsc index.ts`，即可编译 `index.ts` ，同时编译生成的`index.js` 会被放在同一目录下

在命令行输入 `node index.js` ，如果成功输出了 `Hello, TypeScript!`，证明你的电脑已经成功安装 TypeScript 编译器

## TypeScript 的变量类型

TypeScript 的变量类型与 C 语言相比简单的多，仅有**布尔类型( \*boolean\* )**、**数字类型( \*number\* )**、**字符串类型( \*string\* )**、**未定义类型( \*undefined\* )**、**空类型( \*null\* )**、**大数类型( \*bigint\* )**、**符号类型( \*symbol\* )**、**任意类型( \*any\* )**、**never 类型( \*never\* )**

其中有必要细讲的是未定义与空，还有任意类型。大数和符号不常用，感兴趣可以自行 Google

#### 空类型

未定义和空都表示空，但 `null` 表示**这个元素存在，但是是空的**； `undefined` 表示这个元素**干脆就不存在**。且 `undefined` 要比 `null` 常见的多

这两个类型的相同点是，他们都是**只有一个值的数据类型**， `undefined` 的值只有 `undefined` ， `null` 的值只有 `null`

我们先看一张比较经典的图片，该图来自 stackoverflow 的回答

![img](./ts%E5%9F%BA%E7%A1%80/ymjew503t0n000d5qavf4th6on50u5d6DIYvDwY2Ddi2BcxPDqJwDO==.png)

对于一个**未定义的变量**，执行 `typeof` 操作符，那么就会返回 `undefined`

```
let data;
console.log(typeof data); // undefined
console.log(typeof src); // undefined
```

这里我们没有使用 `===` 来判断，因为**对于尚未声明过的变量，我们只能执行使用 typeof 操作符检测其数据类型**这个操作，**使用其他的操作都会报错**

还有使用**对象中不存在的属性**，也会返回 `undefined` ：

```
let obj = {
    prop: "value",
};
console.log(obj.abcdefg); // undefined
```

从逻辑角度来看， `null` 值表示一个空对象指针，指示**变量未指向任何对象**，常在**返回类型是对象，但没关联值**的地方使用，就像下面：

```
let $container = document.getElementById("container"); // container是不存在的
console.log($container); // null
```

如果一个变量**已经定义，并且它的值为 `null`** ，那么他的类型就是：

```
let data = null;
console.log(typeof data); // object
```

你会发现他的类型并不是 `null`， 而是一个对象。这并不奇怪。从逻辑角度来看， **`null` 值表示一个空对象指针，它代表的其实就是一个空对象**。

#### 任意类型

当你**不知道该标记什么类型**，或者你希望可以写任何类型时，可以谨慎使用 `any`

`any` 类型是目前 TypeScript 语言之中具有较大争议的一个设计，因为理论上我们可以将所有的变量声明为 `any` 从而绕过类型检查，这个时候 TypeScript 实际上**退化为 JavaScript**

但是考虑到目前 Web 前端项目会引用大量的第三方库，开发者很多时候无法完全把握某些变量的信息，所以 `any` 类型是必要的。不过我们需要注意其使用，对于能够给定类型的变量则尽量不标记为 `any`

```ts
let num: any = "string";
num = 2333; // OK
num = true; // OK
num = undefined; // OK
```

#### never 类型

TypeScript 支持一种特殊的类型，即 `never` 类型。这种类型常被用于**标注函数返回值**，代表**这个函数永远不会终结或者会抛出异常**：

```
const neverEnd = (): never => {
    while (true) {}
};
```

这种类型的值**永远不能被实例化**，也即尝试声明和使用 `never` 类型的值将会**总是出现错误**，利用个特点，我们可以**检测程序是否考虑了所有的情况**，这被称为**耗尽检查**：

```
type All = number | string | boolean;
// switch 语句用法与 C/C++ 一致
const handler = (value: All) => {
    switch (typeof value) {
        case "number":
            // do something
            break;
        case "string":
            // do something
            break;
        case "boolean":
            // do something
            break;
        default:
            let exhaustiveCheck: never = value;
    }
};
```

根据 `never` 的特点，`default` 分支的代码执行**必然会产生错误**，因此如果该 `switch` 语句未能穷尽 `typeof value` 的可能取值，使得代码落入 `default` 分支，导致 `never` 类型的变量被实例化，进而导致编译器报错

如果修改类型 `All` 为 `number | string | boolean | undefined`，编译器会告诉我们*不能将类型“undefined”分配给类型“never”*，这就是因为当 `value === undefined` 时，会尝试将 `undefined` 赋给 `never` 类型的变量。 这样，`handler` 函数就会因为没有耗尽所有可能而报错

## 类型标注

声明变量时可以在变量后面标注类型，也**可以根据初始值自动推断**，但如果声明变量时**不赋初始值，则必须添加类型标注**，否则在使用时会报错（即**自动推断该变量为 `undefined` 的类型**，因此不能赋其他值）。

#### 普通类型标注

```
let isDone: boolean = false;
let Count: number = 100;
let str: string = "Hello, TypeScript!";
```

也就是在变量名字后面紧接着跟一个冒号，再加上类型就可以了

但需要注意的是 TypeScript **允许使用字面量作为类型标注**，如：

```
let one: 1 = 1;
one = 2; // Error!
```

这里变量 `one` 的类型**被限定为字面量 `1` 而不是所有的 `number`**，这种标注的作用在下面会展示。

#### 对象和数组的标注

对象：

```
let obj: {
    name: string;
    age: number;
    address: string;
} = {
    name: "Alice",
    age: 21,
    address:
        "No. 2, Linggong Road, Ganjingzi District, Dalian City, Liaoning Province",
};
```

所有元素均相同的数组：

```
let learning_direction: string[] = ["Embedded", "Web", "Media"];

learning_direction = "Web"; // Error! 不能将类型“string”分配给类型“string[]”
learning_direction = ["Web"]; // Success
```

固定长度和类型的数组：

```
let arr: [number, boolean] = [1, false];
```

#### 函数的标注

```
function sum(x: number, y: number): number {
    return x + y;
}
```

上面的函数，函数名为 `sum`， 两个参数类型均为 `number` ，**返回值**也为 `number`

这里要注意的是，编译器**会尝试推断函数返回值**，但**不会尝试从函数实现中**推断参数类型，因此**参数列表的类型标注是必不可少的**。

也可以按照声明变量的方式：

```
let sum = function (x: number, y: number): number {
    return x + y;
};
```

这里 `sum` 作为一个变量，它的类型并非前文所提过的原始值，因此**也是一个对象，它的构造函数是 `function`**

如果使用箭头函数：

```
// 以下两种写法的结果相同
const sum = (x: number, y: number) => x + y;

const sum = (x: number, y: number) => {
    return x + y;
};
```

虽然我们函数参数有了类型的限制，但是实际上 TypeScript 在运行时并**不会检查你的调用是否符合参数列表**（尽管在编译器会尝试进行静态检查，但是如果你使用 `any` 或其他一些方法传入其他类型参数，仍然会继续运行）

在 JavaScript 中，甚至不会检查你函数调用的时候传入参数的个数，但是 **TypeScript 会阻止传入个数错误的参数**

在 TypeScript 中可以通过**将参数标为可选或提供默认值来允许不同长度的传入参数**：

```
function sum(x: number, y: number = 1, z?: number): number {
    // y的默认值是1，z为可选参数
    return x + y + (z ?? 0); // 这里 y 不可能为空值，但 z 可能。如果函数调用没有给出z，那么z默认是0
}

console.log(sum(1)); // => 1 + 1 + (undefined ?? 0) = 2
console.log(sum(1, 0)); // => 1 + 0 + (undefined ?? 0) = 1
console.log(sum(1, 2, 3)); // => 1 + 2 + (3 ?? 0) = 6
```

#### 类型别名

以使用 `type` 关键词定义类型别名，在需要实现复杂的类型时非常有用：

```
type numberOne = 1;
let one: numberOne = 1;
one = 2; // Error!
```

#### 联合类型和类型收窄

TypeScript 可以将变量的类型声明为若干个类型之一，这称为联合类型

```
let union: number | string = 7;
union = "Genshin Impact"; // OK
union = 8; // OK
```

上面的 `union` 变量，既可以是 `number` 类型，也可以是 `string` 类型

联合类型最常用的地方是标注函数参数，这样就允许了函数接受多种类型的参数：

```
const addHello = (x: number | string): void => {
    console.log("Hello" + x);
};

addHello(1); // OK
addHello("Dalian"); // OK
```

上文提到字面量可以作为类型标注，那么使用联合类型，就可以实现枚举行为：

```
type oddNumber = 1 | 3 | 5 | 7 | 9;
let a: oddNumber = 5; // OK
let b: oddNumber = 2; // Error!
```

*这就是上面提到的复杂类型之一*

对于“所有元素均相同的数组”的实例代码出现的错误，如果更改为下面的代码：

```
let learning_direction: string | string[] = ["Embedded", "Web", "Media"];

learning_direction = "Web"; // Success
learning_direction = ["Web"]; // Success
```

这里我们声明 `learning_direction` 可能有两种类型，即 `string` 和 `string[]`，所以我们可以将其任意赋值为其中的一种，但这就导致我们在使用这一值时，不能精确判断上面包含的方法，例如我们尝试执行：

```
learning_direction.match(/react/); // Error! 类型“string[]”上不存在属性“match”
```

这里编译器会报错，但如果我们的代码足以让编译器推断出来变量类型：

```
if (typeof learning_direction === "string") {
    console.log(learning_direcion.match(/react/));
} else {
    //
}
```

如果能够进入 `if` 判断的第一个大括号的语句，那 `learning_direction` 就一定是 `string` 类型，编译器自己就能够明白，这么做是安全的，允许调用相应的方法，这种行为被称为**类型收窄**

#### 类型断言

这种语法只用在一种情况：你认为你比编译器还懂这个变量的类型

```
let learning_direction: string | string[] = ["Embedded", "Web", "Media"];

(learning_direction as unknown as string).match(/react/);
```

我们这里使用 `as` 关键字，告诉编译器 `learning_direction` 这个变量**一定为 `string` 类型，不可能是 `string[]` 类型**，这个时候编译器就会听你的，把他它当作 `string` 类型处理

但你不能断言一个变量为明显冲突的类型

比如上述代码，如果是 `(learning_direction as string).match (/react/);` ，那么就会报错，所以我们要**先断言为 `unknown` 再断言为其他类型**

## TypeScript 的复杂类型

#### 泛型

这类似于 C++的模板

```
//函数泛型
function identity<Type>(arg: Type): Type {
    return arg;
}

//还是函数泛型
const identity = <Type,>(arg: Type): Type => arg;

//类泛型
class GenericNumber<NumType> {
    zeroValue: NumType;
    add: (x: NumType, y: NumType) => NumType;
}

//对象泛型
interface Request<ReqBody, ResBody> {
    request: ReqBody;
    response: ResBody;
}

//别名泛型
type MaybeArray<Value> = Value | Value[];

//它们的实例化
identity<number>(1);

const num = new GenericNumber<number>();

const req: Request<{ action: string }, { result: string }> = {
    request: {
        action: "update system",
    },
    response: {
        result: "succeeded",
    },
};

let nums: MaybeArray<number> = 0;
nums = [0, 1, 2];
```

可以为泛型添加限制和默认值，也可以由编译器推断泛型类型：

```
function identity<Type = number>(arg: Type): Type {
    return arg;
}

identity(1);

function identity<Type extends { data: string }>(arg: Type): string {
    return arg.data;
}

identity({ data: "str" });
```

#### typeof 和 keyof 关键字

`typeof` 除了可以作为运算符获取变量类型以外，用作类型标注时，可以获取变量的具体类型（而非作为运算符时的有限种类）：

```
const someObj = {
    foo: 1,
    bar: "2",
};

function f(arg: typeof someObj) {
    // arg: { foo: number; bar: string }
    // do something
}
```

`keyof` 可以**获取类型的键的类型**：

```
const Obj = {
    foo: 1,
    bar: "2",
};

function f(arg: keyof typeof Obj) {
    // arg: 'foo' | 'bar'
    // do something
}
```

#### 三目运算符

与 C 语言的三目运算符语法相同，但是在 TypeScript 中与泛型结合会更有用

```
type MessageOf<T> = T extends { message: any } ? T["message"] : unknown;

interface Email {
    message: string;
}

interface Dog {
    bark(): void;
}

type EmailMessageContents = MessageOf<Email>; // EmailMessageContents = string

type DogMessageContents = MessageOf<Dog>; // DogMessageContents = unknown
```