# React 入门

## 创建和嵌套组件 

React 应用程序是由 **组件** 组成的。一个组件是 UI（用户界面）的一部分，它拥有自己的逻辑和外观。组件可以小到一个按钮，也可以大到整个页面。

React 组件是返回标签的 JavaScript 函数：

```react
function MyButton() {

  return (

    <button>我是一个按钮</button>

  );

}
```

至此，你已经声明了 `MyButton`，现在把它嵌套到另一个组件中：

至此，你已经声明了 `MyButton`，现在把它嵌套到另一个组件中：

```
export default function MyApp() {

  return (

    <div>

      <h1>欢迎来到我的应用</h1>

      <MyButton />

    </div>

  );

}
```

你可能已经注意到 `<MyButton />` 是以**大写字母开头的**。你**可以据此识别 React 组件**。React 组件**必须以大写字母开头**，而 **HTML 标签则必须是小写字母**。

来看下效果：

```js
function MyButton() {
  return (
    <button>
      我是一个按钮
    </button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>欢迎来到我的应用</h1>
      <MyButton />
    </div>
  );
}

```

`export default` 关键字指定了**文件中的主要组件**。

## 使用 JSX 编写标签 

上面所使用的标签语法被称为 ***JSX*。**它是可选的，但**大多数 React 项目会使用 JSX**，主要是它很方便。所有 [我们推荐的本地开发工具](https://zh-hans.react.dev/learn/installation) 都开箱即用地支持 JSX。

**JSX 比 HTML 更加严格**。你**必须闭合标签**，如 `<br />`。你的组件也不能返回多个 JSX 标签。你必须将它们**包裹到一个共享的父级中，比如 `<div>...</div>` 或使用空的 `<>...</>` 包裹**：

```react
function AboutPage() {
  return (
    <>
      <h1>关于</h1>
      <p>你好。<br />最近怎么样？</p>
    </>
  );
}
```

## 添加样式 

在 React 中，你可以**使用 `className` 来指定一个 CSS 的 class。**它与 HTML 的 [`class`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/class) 属性的工作方式相同：

```react
<img className="avatar" />
```

然后，你可以在一个单独的 CSS 文件中为它编写 CSS 规则：

```css
/* 在你的 CSS 文件中修改 */
.avatar {
  border-radius: 50%;
}
```

React 并没有规定你如何添加 CSS 文件。最简单的方式是使用 HTML 的 <link>标签。如果你使用了构建工具或框架，请阅读其文档来了解如何将 CSS 文件添加到你的项目中。

## 显示数据 

JSX 会让你把**标签放到 JavaScript 中**。而**大括号会让你 “回到” JavaScript** 中，这样你就可以从你的代码中嵌入一些变量并展示给用户。例如，这将显示 `user.name`：

```jsx
return (
  <h1>
    {user.name}
  </h1>
);
```

你还可以将 JSX 属性 “转义到 JavaScript”，但你必须使用大括号 **而非** 引号。例如，`className="avatar"` 是将 `"avatar"` 字符串传递给 `className`，作为 CSS 的 class。但 `src={user.imageUrl}` 会**读取 JavaScript 的 `user.imageUrl` 变量**，然后将该值作为 `src` 属性传递：

```js
return (
  <img
    className="avatar"
    src={user.imageUrl}
  />
);
```

你也可以把更为复杂的表达式放入 JSX 的大括号内，例如 [字符串拼接](https://javascript.info/operators#string-concatenation-with-binary)：

```js
const user = {
  name: 'Hedy Lamarr',
  imageUrl: 'https://i.imgur.com/yXOvdOSs.jpg',
  imageSize: 90,
};

export default function Profile() {
  return (
    <>
      <h1>{user.name}</h1>
      <img
        className="avatar"
        src={user.imageUrl}
        alt={'Photo of ' + user.name}
        style={{
          width: user.imageSize,
          height: user.imageSize
        }}
      />
    </>
  );
}

```

在上面示例中，`style={{}}` 并不是一个特殊的语法，而是 `style={ }` **JSX 大括号内的一个普通 `{}` 对象。**当你的**样式依赖于 JavaScript 变量时，你可以使用 `style` 属性**。

## 条件渲染 

React 没有特殊的语法来编写条件语句，因此你**使用的就是普通的 JavaScript 代码**。例如使用 [`if`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/if...else) 语句**根据条件引入 JSX**：

```
let content;

if (isLoggedIn) {

  content = <AdminPanel />;

} else {

  content = <LoginForm />;

}

return (

  <div>

    {content}

  </div>

);
```

如果你喜欢更为紧凑的代码，可以使用 [条件 `?` 运算符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)。与 `if` 不同的是，它**工作于 JSX 内部**：

```
<div>

  {isLoggedIn ? (

    <AdminPanel />

  ) : (

    <LoginForm />

  )}

</div>
```

当你不需要 `else` 分支时，你也可以使用更简短的 [逻辑 `&&` 语法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Logical_AND#short-circuit_evaluation)：

```
<div>

  {isLoggedIn && <AdminPanel />}

</div>
```

所有这些方法也适用于有条件地指定属性。如果你对 JavaScript 语法不熟悉，你可以先使用 `if...else`。

## 渲染列表 

你将依赖 JavaScript 的特性，例如 [`for` 循环](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for) 和 [array 的 `map()` 函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map) 来渲染组件列表。

假设你有一个产品数组：

```react
const products = [

  { title: 'Cabbage', id: 1 },

  { title: 'Garlic', id: 2 },

  { title: 'Apple', id: 3 },

];
```

在你的组件中，使用 `map()` 函数将这个数组转换为 `<li>` 标签构成的列表:

```react
const listItems = products.map(product =>

  <li key={product.id}>

    {product.title}

  </li>

);



return (

  <ul>{listItems}</ul>

);
```

注意， `<li>` 有一个 `key` 属性。对于**列表中的每一个元素**，你都应该传递一个字符串或者数字给 `key`，用于在其兄弟节点中唯一标识该元素。通常 key 来自你的数据，比如数据库中的 ID。如果你在后续插入、删除或重新排序这些项目，React 将依靠你提供的 key 来思考发生了什么。

```js
const products = [
  { title: '卷心菜', isFruit: false, id: 1 },
  { title: '大蒜', isFruit: false, id: 2 },
  { title: '苹果', isFruit: true, id: 3 },
];

export default function ShoppingList() {
  const listItems = products.map(product =>
    <li
      key={product.id}
      style={{
        color: product.isFruit ? 'magenta' : 'darkgreen'
      }}
    >
      {product.title}
    </li>
  );

  return (
    <ul>{listItems}</ul>
  );
}
```

## 响应事件 

你可以通过在组件中声明 **事件处理** 函数来响应事件：

```
function MyButton() {

  function handleClick() {

    alert('You clicked me!');

  }



  return (

    <button onClick={handleClick}>

      点我

    </button>

  );

}
```

注意，`onClick={handleClick}` 的结尾没有小括号！不要 **调用** 事件处理函数：你只需 **把函数传递给事件** 即可。当用户点击按钮时 React 会调用你传递的事件处理函数。

## 更新界面 

通常你会希望你的组件 “记住” 一些信息并展示出来，比如一个按钮被点击的次数。要做到这一点，你需要在你的组件中添加 **state**。

首先，从 React 引入 [`useState`](https://zh-hans.react.dev/reference/react/useState)：

```
import { useState } from 'react';
```

现在你可以在你的组件中声明一个 **state 变量**：

```
function MyButton() {

  const [count, setCount] = useState(0);

  // ...
```

你将从 `useState` 中获得两样东西：**当前的 state（`count`），以及用于更新它的函数（`setCount`）**。你可以给它们**起任何名字，但按照惯例会像 `[something, setSomething]` 这样为它们命名**。

第一次显示按钮时，`count` 的值为 `0`，因为你把 `0` 传给了 `useState()`。当你想改变 state 时，**调用 `setCount()` 并将新的值传递给它**。点击该按钮计数器将递增：

```react
function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}
```

React 将再次调用你的组件函数。第一次 count 变成 1。接着点击会变成 2。继续点击会逐步递增。

如果你**多次渲染同一个组件，每个组件都会拥有自己的 state**。你可以尝试点击不同的按钮：

```react
import { useState } from 'react';

export default function MyApp() {
  return (
    <div>
      <h1>独立更新的计数器</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      点了 {count} 次
    </button>
  );
}

```

## 使用 Hook 

以 **`use` 开头的函数**被称为 **Hook**。`useState` 是 React 提供的一个内置 Hook。你可以在 [React API 参考](https://zh-hans.react.dev/reference/react) 中找到其他内置的 Hook。你也可以通过组合现有的 Hook 来编写属于你自己的 Hook。

Hook **比普通函数更为严格**。你**只能在你的组件（或其他 Hook）**的 **顶层** 调用 Hook。如果你想在一个条件或循环中使用 `useState`，请**提取一个新的组件并在组件内部使用**它。

## 组件间共享数据 

在前面的示例中，每个 `MyButton` 都有自己独立的 `count`，当每个按钮被点击时，只有被点击按钮的 `count` 才会发生改变：

经常需要组件 **共享数据并一起更新**。

为了使得 `MyButton` 组件**显示相同的 `count` 并一起更新**，你需要将各个按钮的 **state “向上” 移动到最接近包含所有按钮的组件之中**。

在这个示例中，它是 `MyApp`：![The same diagram as the previous, with the count of the parent MyApp component highlighted indicating a click with the value incremented to one. The flow to both of the children MyButton components is also highlighted, and the count value in each child is set to one indicating the value was passed down.](./%E8%BF%9B%E8%A1%8Creact%E7%9A%84%E4%B8%80%E4%B8%AA%E4%B8%80%E4%B8%AA%E5%AD%A6/imageurl=%252Fimages%252Fdocs%252Fdiagrams%252Fsharing_data_parent_clicked.png)

此刻，当你点击任何一个按钮时，`MyApp` 中的 `count` 都将改变，同时会改变 `MyButton` 中的两个 count。具体代码如下：

首先，将 `MyButton` 的 **state 上移到** `MyApp` 中：

```react
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>独立更新的计数器</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  // ... 我们正在从这里移动代码...
}
```

接着，将 `MyApp` 中的**点击事件处理函数**以及 **state 一同向下传递到** 每个 `MyButton` 中。你可以使用 **JSX 的大括号向 `MyButton` 传递信息。**就像之前向 `<img>` 等内置标签所做的那样:

```react
export default function MyApp() {

  const [count, setCount] = useState(0);



  function handleClick() {

    setCount(count + 1);

  }



  return (

    <div>

      <h1>共同更新的计数器</h1>

      <MyButton count={count} onClick={handleClick} />

      <MyButton count={count} onClick={handleClick} />

    </div>

  );

}
```

使用这种方式传递的信息被称作 **prop**。此时 **`MyApp` 组件包含了 `count` state 以及 `handleClick` 事件处理函数**，并将它们作为 **prop 传递给** 了每个按钮。

最后，改变 `MyButton` 以 **读取** 从父组件传递来的 prop：

```react
function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      点了 {count} 次
    </button>
  );
}
```

当你点击按钮时，`onClick` 处理程序会启动。每个按钮的 `onClick` prop 会被设置为 `MyApp` 内的 `handleClick` 函数，所以函数内的代码会被执行。该代码会调用 `setCount(count + 1)`，使得 state 变量 `count` 递增。新的 `count` 值会被作为 prop 传递给每个按钮，因此它们每次展示的都是最新的值。这被称为“状态提升”。通过向上移动 state，我们实现了在**组件间共享**它。

```react
import { useState } from 'react';
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>共同更新的计数器</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}
function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      点了 {count} 次
    </button>
  );
}
```

## 井字棋游戏

```react
import { useState } from 'react';

function Square({ value, onSquareClick }) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}

function Board({ xIsNext, squares, onPlay }) {
  function handleClick(i) {
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    const nextSquares = squares.slice();
    if (xIsNext) {
      nextSquares[i] = 'X';
    } else {
      nextSquares[i] = 'O';
    }
    onPlay(nextSquares);
  }

  const winner = calculateWinner(squares);
  let status;
  if (winner) {
    status = 'Winner: ' + winner;
  } else {
    status = 'Next player: ' + (xIsNext ? 'X' : 'O');
  }

  return (
    <>
      <div className="status">{status}</div>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
        <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
        <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
        <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
        <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
      </div>
    </>
  );
}

export default function Game() {
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const [currentMove, setCurrentMove] = useState(0);
  const xIsNext = currentMove % 2 === 0;
  const currentSquares = history[currentMove];

  function handlePlay(nextSquares) {
    const nextHistory = [...history.slice(0, currentMove + 1), nextSquares];
    setHistory(nextHistory);
    setCurrentMove(nextHistory.length - 1);
  }

  function jumpTo(nextMove) {
    setCurrentMove(nextMove);
  }

  const moves = history.map((squares, move) => {
    let description;
    if (move > 0) {
      description = 'Go to move #' + move;
    } else {
      description = 'Go to game start';
    }
    return (
      <li key={move}>
        <button onClick={() => jumpTo(move)}>{description}</button>
      </li>
    );
  });

  return (
    <div className="game">
      <div className="game-board">
        <Board xIsNext={xIsNext} squares={currentSquares} onPlay={handlePlay} />
      </div>
      <div className="game-info">
        <ol>{moves}</ol>
      </div>
    </div>
  );
}

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}

```

#### `App.js` 

`App.js` 的代码创建了一个 **组件**。在 React 中，**组件是一段可重用代码**，它通常作为 **UI 界面的一部分**。组件用于渲染、管理和更新应用中的 UI 元素。让我们逐行查看这段代码，看看发生了什么：

```
export default function Square() {

  return <button className="square">X</button>;

}
```

第一行定义了一个名为 `Square` 的函数。JavaScript 的 `export` 关键字使此函数可以在此文件之外访问。`default` 关键字表明它是文件中的主要函数。

第二行返回一个按钮。JavaScript 的 `return` 关键字意味着后面的内容都作为值返回给函数的调用者。`<button>` 是一个 JSX 元素。JSX 元素是 JavaScript 代码和 HTML 标签的组合，用于描述要显示的内容。`className="square"` 是一个 button 属性，它决定 CSS 如何设置按钮的样式。`X` 是按钮内显示的文本，`</button>` 闭合 JSX 元素以表示不应将任何后续内容放置在按钮内。

#### `styles.css` 

单击 CodeSandbox 中的 `styles.css` 文件。该文件定义了 **React 应用的样式**。前两个 **CSS 选择器**（`*` 和 `body`）定义了**应用大部分**的样式，而 **`.square` 选择器定义了 `className` 属性设置为 `square` 的组件的样式**。这与 `App.js` 文件中的 `Square` 组件中的按钮是相匹配的。

#### `index.js` 

单击 CodeSandbox 中的 `index.js` 的文件。在本教程中我们不会编辑此文件，但它是 `App.js` 文件中创建的组件与 Web 浏览器之间的桥梁。

```
import { StrictMode } from 'react';

import { createRoot } from 'react-dom/client';

import './styles.css';



import App from './App';
```

第 1-5 行将所有必要的部分组合在一起：

- React
- React 与 Web 浏览器对话的库（React DOM）
- 组件的样式
- `App.js` 里面创建的组件

其他文件将它们组合在一起，并将最终成果**注入 `public` 文件夹里面的 `index.html`** 中。

React 组件必须**返回单个 JSX 元素**，不能像两个按钮那样**返回多个相邻**的 JSX 元素。要解决此问题，可以**使用 Fragment（`<>` 与 `</>`）包裹多个相邻的 JSX 元素**，如下所示：

`App.js` 文件中，`Square` 组件看起来像这样：

```
export default function Square() {

  return (

    <>

      <div className="board-row">

        <button className="square">1</button>

        <button className="square">2</button>

        <button className="square">3</button>

      </div>

      <div className="board-row">

        <button className="square">4</button>

        <button className="square">5</button>

        <button className="square">6</button>

      </div>

      <div className="board-row">

        <button className="square">7</button>

        <button className="square">8</button>

        <button className="square">9</button>

      </div>

    </>

  );

}
```

借助 `styles.css` 中定义的 `board-row` 样式，我们将**组件分到每一行的 `div` 中**。最终完成了井字棋棋盘：

![有着数字 1 到 9 的井字棋棋盘](./%E8%BF%9B%E8%A1%8Creact%E7%9A%84%E4%B8%80%E4%B8%AA%E4%B8%80%E4%B8%AA%E5%AD%A6/number-filled-board.png)

### 通过 props 传递数据 

接下来，当用户单击方块时，我们要将方块的值从空更改为“X”。根据目前构建的棋盘，你需要复制并粘贴九次更新方块的代码（每个方块都需要一次）！但是，React 的组件架构可以创建**可重用的组件**，以避免混乱、重复的代码。

首先，要将定义第一个方块（`<button className="square">1</button>`）的这行代码从 `Board` 组件复制到新的 `Square` 组件中：

```
function Square() {

  return <button className="square">1</button>;

}



export default function Board() {

  // ...

}
```

然后，更新 Board 组件并使用 JSX 语法渲染 `Square` 组件：

```
// ...

export default function Board() {

  return (

    <>

      <div className="board-row">

        <Square />

        <Square />

        <Square />

      </div>

      <div className="board-row">

        <Square />

        <Square />

        <Square />

      </div>

      <div className="board-row">

        <Square />

        <Square />

        <Square />

      </div>

    </>

  );

}
```

需要注意的是，这并不像 `div`，这些你自己的组件如 `Board` 和 `Square`，必须以大写字母开头。

让我们来看一看效果：

![都是 1 的方块](./%E8%BF%9B%E8%A1%8Creact%E7%9A%84%E4%B8%80%E4%B8%AA%E4%B8%80%E4%B8%AA%E5%AD%A6/board-filled-with-ones.png)

哦不！你失去了你以前有正确编号的方块。现在每个方块都写着“1”。要解决此问题，需要使用 *props* 将**每个方块应有的值从父组件（`Board`）传递到其子组件（`Square`）。**

更新 `Square` 组件，读取从 `Board` 传递的 `value` props：

```
function Square({ value }) {

  return <button className="square">1</button>;

}
```

`function Square({ value })` 表示可以向 Square 组件传递一个名为 `value` 的 props。

我们需要从组件中渲染名为 `value` 的 JavaScript 变量，而不是“value”这个词。要从 JSX“转义到 JavaScript”，你需要使用大括号。在 JSX 中的 `value` 周围添加大括号，如下所示：

```
function Square({ value }) {

  return <button className="square">{value}</button>;

}
```

现在，你应该会看到一个空的棋盘了：

![空棋盘](./%E8%BF%9B%E8%A1%8Creact%E7%9A%84%E4%B8%80%E4%B8%AA%E4%B8%80%E4%B8%AA%E5%AD%A6/empty-board.png)

我们希望 Square 组件能够“记住”它被单击过，并用“X”填充它。为了“记住”一些东西，组件使用 *state*。

React 提供了一个**名为 `useState` 的特殊函数**，可以从组件中调用它来让它“记住”一些东西。让我们将 `Square` 的当前值存储在 state 中，并在单击 `Square` 时更改它。

在文件的顶部导入 `useState`。从 `Square` 组件中移除 `value` props。在调用 `useState` 的 `Square` 的开头**添加一个新行**。让它返回一个名为 value 的 state 变量：

```
import { useState } from 'react';



function Square() {

  const [value, setValue] = useState(null);



  function handleClick() {

    //...
```

`value` 存储值，而 `setValue` 是可用于更改值的函数。传递给 `useState` 的 `null` 用作这个 state 变量的初始值，因此此处 `value` 的值开始时等于 `null`。

由于 `Square` 组件不再接受 props，我们从 Board 组件创建的所有九个 Square 组件中删除 `value` props：

。现在你的 `Square` 组件看起来像这样：

```
function Square() {

  const [value, setValue] = useState(null);



  function handleClick() {

    setValue('X');

  }



  return (

    <button

      className="square"

      onClick={handleClick}

    >

      {value}

    </button>

  );

}
```

通过从 `onClick` 处理程序调用此 `set` 函数，你告诉 `React` 在单击其 `<button>` 时要重新渲染该 `Square`。更新后，方块的值将为“X”，因此会在棋盘上看到“X”。点击任意方块，“X”应该出现

### React 开发者工具 

React 开发者工具可以检查 **React 组件的 props 和 state**。可以在 CodeSandbox 的 *Browser* 部分底部找到 React DevTools 选项卡：

![CodeSandbox 中的 React 开发者工具](./%E8%BF%9B%E8%A1%8Creact%E7%9A%84%E4%B8%80%E4%B8%AA%E4%B8%80%E4%B8%AA%E5%AD%A6/codesandbox-devtools.png)

`handleClick` 函数使用 JavaScript 数组的 `slice()` 方法创建 **`squares` 数组的副本（`nextSquares`）**。然后，`handleClick` 更新 `nextSquares` 数组，将 `X` 添加到第一个（`[0]` 索引）方块。

**调用 `setSquares` 函数让 React 知道组件的 state 已经改变**。这将触发使用 `squares` state 的组件（`Board`）及其子组件（构成棋盘的 `Square` 组件）的**重新渲染**。

### 注意

JavaScript 支持 [闭包](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures)，这意味着**内部函数（例如 `handleClick`）可以访问外部函数（例如 `Board`）中定义的变量和函数**。`handleClick` 函数可以读取 `squares` state 并调用 `setSquares` 方法，因为它们都是在 `Board` 函数内部定义的。

当你传递 onSquareClick={handleClick} 时，你将 handleClick 函数**作为 props 向下传递**。你不是在调用它

### 注意

DOM `<button>` **元素的 `onClick` props** 对 React 有特殊意义，因为它是一个内置组件。对于像 Square 这样的**自定义组件**，**命名由你决定**。你可以给 `Square` 的 `onSquareClick` props 或 `Board` 的 `handleClick` 函数起**任何名字**，代码还是可以运行的。在 React 中，通常使用 **`onSomething` 命名代表事件的 props**，使用 **`handleSomething` 命名处理这些事件的函数**。

### 为什么不变性很重要 

请注意在 `handleClick` 中，你调用了 `.slice()` 来创建 `squares` 数组的副本而不是修改现有数组。为了解释原因，我们需要讨论不变性以及为什么学习不变性很重要。

通常有两种更改数据的方法。第一种方法是通过直接更改数据的值来改变数据。第二种方法是使用具有所需变化的新副本替换数据。如果你改变 `squares` 数组，它会是这样的：

```
const squares = [null, null, null, null, null, null, null, null, null];

squares[0] = 'X';

// Now `squares` is ["X", null, null, null, null, null, null, null, null];
```

如果你在不改变 `squares` 数组的情况下更改数据，它会是这样的：

```
const squares = [null, null, null, null, null, null, null, null, null];

const nextSquares = ['X', null, null, null, null, null, null, null, null];

// Now `squares` is unchanged, but `nextSquares` first element is 'X' rather than `null`
```

结果是一样的，但通过不直接改变（改变底层数据），你可以获得几个好处。

不变性使复杂的功能更容易实现。在本教程的后面，你将实现一个“时间旅行”功能，让你回顾游戏的历史并“跳回”到过去的动作。此功能并非特定于游戏——撤消和重做某些操作的能力是应用程序的常见要求。避免数据直接突变可以让你保持以前版本的数据完好无损，并在**以后重用它们**。

不变性还有另一个好处。默认情况下，当父组件的 state 发生变化时，所有子组件都会自动重新渲染。这甚至包括未受变化影响的子组件。尽管重新渲染本身不会引起用户注意（你不应该主动尝试避免它！），但出于性能原因，你可**能希望跳过重新渲染显然不受其影响的树的一部分**。不变性使得组件比较其数据是否已更改的成本非常低。你可以在 [`memo` API 参考](https://zh-hans.react.dev/reference/react/memo) 中了解更多关于 React 如何选择何时重新渲染组件的信息。

### 存储落子历史 

如果你**改变了 `squares` 数组，实现时间旅行将非常困难**。

但是，你在每次落子后都使用 `slice()` 创建 `squares` 数组的新副本，并将其视为不可变的。这将允许你**存储 `squares` 数组的每个过去的版本**，并在已经发生的轮次之间“来回”。

把过去的 `squares` 数组存储在另一个名为 `history` 的数组中，把它存储为一个新的 state 变量**。`history` 数组表示所有棋盘的 state**，从第一步到最后一步，其形状如下：

```react
[
  // Before first move
  [null, null, null, null, null, null, null, null, null],
  // After first move
  [null, null, null, null, 'X', null, null, null, null],
  // After second move
  [null, null, null, null, 'X', null, null, null, 'O'],
  // ...
]
```

### 再一次“状态提升” 

你现在将编写一个名为 `Game` 的**新顶级组件**来显示过去的着法列表。这就是放置包含整个游戏历史的 `history` state 的地方。

将 `history` state **放入 `Game` 组件**将使你可以从其 `Board` 子组件中删除 `squares` state。就像你将 state 从 `Square` 组件“提升”到 `Board` 组件一样，你现在将把它从 `Board` 提升到顶层 `Game` 组件。这**使 `Game` 组件可以完全控制 `Board` 的数据**，并使它让 `Board` 渲染来自 `history` 的之前的回合。

首先，添加一个带有 `export default` 的 `Game` 组件。让它渲染 `Board` 组件和一些标签

**强烈建议你在构建动态列表时分配适当的 key**。如果你没有合适的 key，你可能需要考虑重组你的数据，以便你这样做。key 不需要是全局唯一的；它们只需要**在组件及其同级组件之间是唯一**的。

# React 哲学

React 可以改变你对可见设计和应用构建的思考。当你使用 React 构建用户界面时，你首先会把它分解成一个个 **组件**，然后，你需要把这些组件连接在一起，使数据流经它们。

## 从原型开始 

想象一下，你早已从设计者那儿得到了一个 JSON API 和原型。

JSON API 返回如下的数据:

```json
[

  { category: "Fruits", price: "$1", stocked: true, name: "Apple" },

  { category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit" },

  { category: "Fruits", price: "$2", stocked: false, name: "Passionfruit" },

  { category: "Vegetables", price: "$2", stocked: true, name: "Spinach" },

  { category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin" },

  { category: "Vegetables", price: "$1", stocked: true, name: "Peas" }

]
```

原型看起来像是这样:![img](./%E8%BF%9B%E8%A1%8Creact%E7%9A%84%E4%B8%80%E4%B8%AA%E4%B8%80%E4%B8%AA%E5%AD%A6/s_thinking-in-react_ui.png)

仅需跟随下面的五步，即可使用 React 来实现 UI。

## 步骤一：将 UI 拆解为组件层级结构 

一开始，在绘制原型中的每个组件和子组件周围绘制盒子并命名它们。如果你与设计师一起工作，他们可能早已在其设计工具中对这些组件进行了命名。检查一下它们!

取决于你的使用背景，可以考虑通过不同的方式将设计分割为组件:

- **程序设计**——使用同样的技术决定你**是否应该创建一个新的函数或者对象**。这一技术即 [单一功能原理](https://en.wikipedia.org/wiki/Single_responsibility_principle)，也就是说，一个组件**理想情况下应仅做一件事情**。但**随着功能的持续增长，它应该被分解为更小的子组件**。
- **CSS**——思考你将把**类选择器用于何处**。(然而，组件并没有那么细的粒度。)
- **设计**——思考你将**如何组织布局的层级。**

如果你的 JSON 结构非常棒，经常会发现其映射到 UI 中的组件结构是一件自然而然的事情。那是因为 **UI 和原型常拥有相同的信息结构—即，相同的形状**。将你的 UI 分割到组件，每个组件匹配到原型中的每个部分。

以下展示了五个组件:![img](./%E8%BF%9B%E8%A1%8Creact%E7%9A%84%E4%B8%80%E4%B8%AA%E4%B8%80%E4%B8%AA%E5%AD%A6/s_thinking-in-react_ui_outline.png)

1. `FilterableProductTable`（灰色）包含完整的应用。
2. `SearchBar`（蓝色）获取用户输入。
3. `ProductTable`（淡紫色）根据用户输入，展示和过滤清单。
4. `ProductCategoryRow`（绿色）展示每个类别的表头。
5. `ProductRow`（黄色）展示每个产品的行。

看向 `ProductTable`（淡紫色），可以看到表头（包含 “Name” 和 “Price” 标签）并不是独立的组件。这是个人喜好的问题，你可以采取任何一种方式继续。在这个例子中，它是作为 `ProductTable` 的一部分，因为它展现在 `ProductTable` 列表之中。然而，如果这个表头变得复杂（举个例子，如果添加排序），创建独立的 `ProductTableHeader` 组件就变得有意义了。

现在你已经在原型中辨别了组件，并将它们转化为了层级结构。在原型中出现在其他组件内部的组件在层级结构中应作为子项出现:

- ```
	FilterableProductTable
	```

	- `SearchBar`

	- ```
		ProductTable
		```

		- `ProductCategoryRow`
		- `ProductRow`

## 步骤二：使用 React 构建一个静态版本 

现在你已经拥有了你自己的组件层级结构，是时候实现你的应用程序了。最直接的办法是根据你的数据模型，构建**一个不带任何交互的 UI 渲染代码版本**…经常是先构建一个**静态版本**比较简单，然后**再一个个添加交互**。构建一个静态版本需要写大量的代码，并不需要什么思考; 但添加交互需要大量的思考，却不需要大量的代码。

构建应用程序的静态版本来渲染你的数据模型，将构建 [组件](https://zh-hans.react.dev/learn/your-first-component) 并复用其它的组件，然后使用 [props](https://zh-hans.react.dev/learn/passing-props-to-a-component) 进行传递数据。Props 是从父组件向子组件传递数据的一种方式。如果你对 [state](https://zh-hans.react.dev/learn/state-a-components-memory) 章节很熟悉，不要在静态版本中使用 state 进行构建。state 只是为交互提供的**保留功能**，即数据会随着时间变化。因为这是一个静态应用程序，所以并不需要。

你既**可以通过从层次结构更高层组件（如 `FilterableProductTable`）开始“自上而下”构建，也可以通过从更低层级组件（如 `ProductRow`）“自下而上”进行构建**。在**简单**的例子中，**自上而下构建**通常更简单；而在**大型**项目中，**自下而上**构建更简单。

```js
function ProductCategoryRow({ category }) {
  return (
    <tr>
      <th colSpan="2">
        {category}
      </th>
    </tr>
  );
}

function ProductRow({ product }) {
  const name = product.stocked ? product.name :
    <span style={{ color: 'red' }}>
      {product.name}
    </span>;

  return (
    <tr>
      <td>{name}</td>
      <td>{product.price}</td>
    </tr>
  );
}

function ProductTable({ products }) {
  const rows = [];
  let lastCategory = null;

  products.forEach((product) => {
    if (product.category !== lastCategory) {
      rows.push(
        <ProductCategoryRow
          category={product.category}
          key={product.category} />
      );
    }
    rows.push(
      <ProductRow
        product={product}
        key={product.name} />
    );
    lastCategory = product.category;
  });

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
}

function SearchBar() {
  return (
    <form>
      <input type="text" placeholder="Search..." />
      <label>
        <input type="checkbox" />
        {' '}
        Only show products in stock
      </label>
    </form>
  );
}

function FilterableProductTable({ products }) {
  return (
    <div>
      <SearchBar />
      <ProductTable products={products} />
    </div>
  );
}

const PRODUCTS = [
  {category: "Fruits", price: "$1", stocked: true, name: "Apple"},
  {category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit"},
  {category: "Fruits", price: "$2", stocked: false, name: "Passionfruit"},
  {category: "Vegetables", price: "$2", stocked: true, name: "Spinach"},
  {category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin"},
  {category: "Vegetables", price: "$1", stocked: true, name: "Peas"}
];

export default function App() {
  return <FilterableProductTable products={PRODUCTS} />;
}
```

在构建你的组件之后，即拥有一个渲染数据模型的可复用组件库。因为这是一个静态应用程序，**组件仅返回 JSX**。最顶层组件（`FilterableProductTable`）将接收你的数据模型作为其 prop。这被称之为 **单向数据流**，因为数据从树的顶层组件传递到下面的组件。

## 步骤三：找出 UI 精简且完整的 state 表示 

为了使 UI 可交互，你需要用户更改潜在的数据结构。你将可以使用 **state** 进行实现。

考虑将 state 作为**应用程序需要记住改变数据的最小集合**。组织 state 最重要的一条原则是保持它 [DRY（不要自我重复）](https://en.wikipedia.org/wiki/Don't_repeat_yourself)。计算出你应用程序需要的绝对精简 state 表示，按需计算其它一切。举个例子，如果你正在构建一个购物列表，你可将他们在 state 中存储为数组。如果你同时想展示列表中物品数量，不需要将其另存为一个新的 state。而是，可以通过读取你数组的长度来实现。

现在考虑示例应用程序中的每一条数据:

1. 产品原始列表
2. 搜索用户键入的文本
3. 复选框的值
4. 过滤后的产品列表

其中哪些是 state 呢？标记出那些不是的:

- 随着时间推移 **保持不变**？如此，便不是 state。
- 通过 props **从父组件传递**？如此，便不是 state。
- 是否可以基于已存在于组件中的 state 或者 props **进行计算**？如此，它肯定不是state！

剩下的可能是 state。

让我们再次一条条验证它们:

1. 原始列表中的产品 **被作为 props 传递，所以不是 state**。
2. 搜索文本似乎应该是 state，因为它会随着时间的推移而变化，并且无法从任何东西中计算出来。
3. 复选框的值似乎是 state，因为它会随着时间的推移而变化，并且无法从任何东西中计算出来。
4. 过滤后列表中的产品 **不是 state，因为可以通过被原始列表中的产品，根据搜索框文本和复选框的值进行计算**。

这就意味着只有搜索文本和复选框的值是 state！非常好！

在 React 中有两种“模型”数据：props 和 state。下面是它们的不同之处:

- [**props** 像是你传递的参数](https://zh-hans.react.dev/learn/passing-props-to-a-component) 至函数。它们使父组件可以传递数据给子组件，定制它们的展示。举个例子，`Form` 可以传递 `color` prop 至 `Button`。
- [**state** 像是组件的内存](https://zh-hans.react.dev/learn/state-a-components-memory)。它使组件可以**对一些信息保持追踪**，并根据交互来改变。举个例子，`Button` 可以保持对 `isHovered` state 的追踪。

props 和 state 是不同的，但它们可以**共同工作**。父组件将经常在 state 中放置一些信息（以便它可以改变），并且作为子组件的属性 **向下** 传递至它的子组件。如果第一次了解这其中的差别感到迷惑，也没关系。通过大量练习即可牢牢记住！

## 步骤四：验证 state 应该被放置在哪里 

在验证你应用程序中的最小 state 数据之后，你需要验证哪个组件是通过改变 state 实现可响应的，或者 **拥有** 这个 state。记住：React 使用单向数据流，通过组件层级结构从父组件传递数据至子组件。要搞清楚哪个组件拥有哪个 state。如果你是第一次阅读此章节，可能会很有挑战，但可以通过下面的步骤搞定它!

为你应用程序中的每一个 state:

1. 验证每一个基于特定 state 渲染的组件。
2. 寻找它们最近并且共同的父组件——在层级结构中，一个凌驾于它们所有组件之上的组件。
3. 决定 state 应该被放置于哪里:
	1. 通常情况下，你可以直接放置 state 于它们共同的父组件。
	2. 你也可以将 state 放置于它们父组件上层的组件。
	3. 如果你找不到一个合适来放这个 state 的地方，单独创建一个新的组件去管理这个 state，并将它添加到它们父组件上层的某个地方。

在之前的步骤中，你已在应用程序中创建了两个 state：输入框文本和复选框的值。在这个例子中，它们总在一起展示，将其视为一个 state 非常简单。

现在为这个 state 贯彻我们的策略:

1. 验证使用 state 的组件

	：

	- `ProductTable` 需要基于 state (搜索文本和复选框值) 过滤产品列表。
	- `SearchBar` 需要展示 state (搜索文本和复选框值)。

2. **寻找它们的父组件**：它们的第一个共同父组件为 `FilterableProductTable`。

3. **决定 state 放置的地方**：我们将过滤文本和勾选 state 的值放置于 `FilterableProductTable` 中。

所以 state 将被放置在 `FilterableProductTable`。

用 [`useState()` Hook](https://zh-hans.react.dev/reference/react/useState) 为组件添加 state。Hook 可以“钩住”组件的 [渲染周期](https://zh-hans.react.dev/learn/render-and-commit)。在 `FilterableProductTable` 的顶部添加两个 state 变量，用于指定你应用程序的初始 state：

```
function FilterableProductTable({ products }) {

  const [filterText, setFilterText] = useState('');

  const [inStockOnly, setInStockOnly] = useState(false);
```

然后，`filterText` 和 `inStockOnly` 作为 props 传递至 `ProductTable` 和 `SearchBar`：

```
<div>

  <SearchBar

    filterText={filterText}

    inStockOnly={inStockOnly} />

  <ProductTable

    products={products}

    filterText={filterText}

    inStockOnly={inStockOnly} />

</div>
```

你可以查看你应用程序的表现。在下面的沙盒代码中，通过修改 `useState('')` 为 `useState('fruit')` 以编辑 `filterText` 的初始值，你将会发现搜索输入框和表格发生更新

## 步骤五：添加反向数据流 

目前你的应用程序可以带着 props 和 state 随着层级结构进行渲染。但是为了支持通过用户输入来改变 state，你需要让数据反向传输：深层结构的表单组件需要更新 `FilterableProductTable` 的 state。

React 使数据流变得明确，但**比双向数据绑定需要多写一些代码**。如果你尝试在上述的例子中输入或者勾选复选框，发现 React 忽视了你的输入。这点是有意为之的。通过 `<input value={filterText} />`，已经设置了 `input` 的 `value` 属性，使之恒等于从 `FilterableProductTable` 传递的 `filterText` state。只要 `filterText` state 不设置，（输入框的）输入就不会改变。

当用户更改表单输入时，state 将更新以反映这些更改。state 由 `FilterableProductTable` 所拥有，所以只有它可以调用 `setFilterText` 和 `setInStockOnly`。使 `SearchBar` 更新 `FilterableProductTable` 的 state，需要将这些函数传递到 `SearchBar`：

```react
function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);

  return (
    <div>
      <SearchBar
        filterText={filterText}
        inStockOnly={inStockOnly}
        onFilterTextChange={setFilterText}
        onInStockOnlyChange={setInStockOnly} />
```

在 `SearchBar` 中，添加一个 **`onChange` 事件处理器**，使用其设置父组件的 state：

```
function SearchBar({

  filterText,

  inStockOnly,

  onFilterTextChange,

  onInStockOnlyChange

}) {

  return (

    <form>

      <input

        type="text"

        value={filterText}

        placeholder="搜索"

        onChange={(e) => onFilterTextChange(e.target.value)}

      />

      <label>

        <input

          type="checkbox"

          checked={inStockOnly}

          onChange={(e) => onInStockOnlyChange(e.target.checked)}
```

现在应用程序可以完整工作了！

# 安装

## Try React 

### 注意

#### 全栈框架不需要服务器 

本页列出的所有框架都支持客户端渲染（[CSR](https://developer.mozilla.org/en-US/docs/Glossary/CSR)）、单页应用（[SPA](https://developer.mozilla.org/en-US/docs/Glossary/SPA)）和静态站点生成（[SSG](https://developer.mozilla.org/en-US/docs/Glossary/SSG)）。这些应用可以**部署到 [CDN](https://developer.mozilla.org/en-US/docs/Glossary/CDN) 或静态托管服务（无需服务器）**。此外，这些框架允许你根据实际需求，针对特定路由单独启用服务端渲染。

这意味着你可以从纯客户端应用开始构建，后续需求变更时，无需重写整个应用，即可在特定路由上启用服务端功能。具体渲染策略的配置方法，请参考各框架的官方文档。

### Vite 

[Vite](https://vite.dev/) 是一个构建工具，旨在为现代网络项目提供更快更简洁的开发体验。

```bash
npm create vite@latest my-app -- --template react
```

Vite 采用约定式设计，开箱即提供合理的默认配置。它拥有丰富的插件生态系统，能够支持快速热更新、JSX、Babel/SWC 等常见功能。你可以查看 Vite 的 [React 插件](https://vite.dev/plugins/#vitejs-plugin-react) 或 [React SWC 插件](https://vite.dev/plugins/#vitejs-plugin-react-swc) 和 [React 服务器端渲染示例项目](https://vite.dev/guide/ssr.html#example-projects) 来开始使用。

Vite 已经作为构建工具在我们 [推荐的框架](https://zh-hans.react.dev/learn/creating-a-react-app) 之一 [React Router](https://reactrouter.com/start/framework/installation) 中使用。

## 步骤 2: 构建常见的应用程序模式 

上面列出的构建工具从**客户端单页应用程序（SPA）开始**，但不包括路由、数据获取或样式等常见功能的进一步解决方案。

React 生态系统中包含许多用于解决这些问题的工具。我们列出了一些广泛使用的工具作为起点，但如果其他工具更适合你，欢迎选择使用。

### 路由 

路由决定了当**用户访问特定 URL 时显示的内容或页面**。你需要**设置一个路由来将 URL 映射到应用程序的不同部分。**你**还需要处理嵌套路由、路由参数和查询参数。路由可以在代码中配置**，也可以根据组件文件夹和文件结构定义。

路由是现代应用程序的核心部分，通常与**数据获取（包括为整个页面预取数据以加快加载速度**）、**代码拆分（以最小化客户端包的大小）**和**页面渲染方法（决定每个页面的生成方式）**集成在一起。

我们建议使用:

- [React Router](https://reactrouter.com/start/data/custom)

### 数据获取 

从服务器或其他数据源获取数据是大多数应用程序的关键部分。要正确地执行此操作，需要处理加载状态、错误状态以及缓存获取的数据，这可能会很复杂。

专门构建的数据获取库为你完成数据获取和缓存的繁重工作，让你专注于应用程序需要哪些数据以及如何显示这些数据。**这些库通常直接在组件中使用**，但也可以集成到路由加载器中，以实现更快的预取和更好的性能，并且也可以用于服务器渲染。

请注意，直接在组件中获取数据可能会导致加载时间变慢，因为会出现网络请求瀑布效应，因此我们建议**尽可能在路由加载器或服务器上预取数据**！这样可以在页面显示时一次性获取页面的数据。

如果你从大多数后端或 REST 风格的 API 获取数据，我们建议使用：

### 代码拆分 

代码拆分是将应用程序分解为可以按需加载的小型包的过程。随着每个新功能和额外依赖的增加，应用程序的代码体积会增大。应用程序可能会因为需要在使用前发送整个应用程序的所有代码而变得加载缓慢。缓存、减少功能/依赖项以及将部分代码移至服务器运行可以帮助缓解加载缓慢的问题，但如果过度使用，这些都是不完整的解决方案，可能会牺牲功能。

同样，如果你依赖使用框架的应用程序来拆分代码，可能会遇到加载速度比不进行代码拆分时更慢的情况。例如，[懒加载](https://zh-hans.react.dev/reference/react/lazy) 图表会延迟发送渲染图表所需的代码，将图表代码与应用程序的其余部分分开。[Parcel 支持使用 React.lazy 进行代码拆分](https://parceljs.org/recipes/react/#code-splitting)。然而，如果图表在初始渲染后才加载其数据，你就需要等待两次。这就是所谓的瀑布效应：与其同时获取图表的数据和发送渲染代码，你必须等待每个步骤依次完成。

通过路由拆分代码，并与打包和数据获取集成，可以减少应用程序的初始加载时间以及渲染应用程序最大可见内容所需的时间 ([最大内容绘制](https://web.dev/articles/lcp))。

有关代码拆分的说明，请参阅你的构建工具文档:

- [Vite 构建优化](https://vite.dev/guide/features.html#build-optimizations)

### 提高应用程序性能 

由于你选择的构建工具仅支持单页应用程序（SPA），你需要实现其他 [渲染模式](https://www.patterns.dev/vanilla/rendering-patterns) 如服务器端渲染（SSR）、静态站点生成（SSG）和/或 React 服务器组件（RSC）。即使你一开始不需要这些功能，将来也可能有一些路由会从 SSR、SSG 或 RSC 中受益。

- **单页面应用程序 (SPA)** 加载**单个 HTML 页面**，并在用户与应用程序交互时动态更新页面。SPA 更容易入门，但初始加载时间可能较慢。SPA 是大多数构建工具的默认架构。
- **流式服务器端渲染 (SSR)** 在**服务器上渲染页面并将完全渲染的页面发送到客户端**。SSR 可以**提高性能**，但设置和维护比单页应用程序更复杂。随着流式处理的加入，SSR 的设置和维护可能会变得非常复杂。 请参阅 [Vite 的 SSR 指南](https://vite.dev/guide/ssr)。
- **静态站点生成 (SSG)** 在构建时**为你的应用生成静态 HTML 文件**。SSG 可以提高性能，但设置和维护可能比服务器端渲染更复杂。请参阅[Vite 的 SSG 指南](https://vite.dev/guide/ssr.html#pre-rendering-ssg)。
- **React 服务器组件 (RSC)** 允许你**在单个 React 树中混合构建时、仅服务器和交互式组件**。RSC 可以提高性能，但目前需要深入的专业知识来设置和维护。请参阅 [Parcel 的 RSC 示例](https://github.com/parcel-bundler/rsc-examples)。

你的渲染策略需要与路由集成，以便使用你的框架构建的应用程序可以在每个路由级别选择渲染策略。这将使你能够在不重写整个应用程序的情况下使用不同的渲染策略。例如，你的应用程序的登录页面可能会从静态生成 (SSG) 中受益，而具有内容提要的页面可能在服务器端渲染时表现最佳。

使用合适的渲染策略针对合适的路由可以**减少加载第一个内容字节的时间 ([首字节时间](https://web.dev/articles/ttfb))，第一个内容元素渲染的时间 ([首次内容绘制](https://web.dev/articles/fcp))，以及应用程序最大可见内容渲染的时间 ([最大内容绘制](https://web.dev/articles/lcp))。**

# 将 React 添加到现有项目中

如果想**对现有项目添加一些交互**，**不必使用 React 将其整个重写**。只需将 React **添加到已有技术栈中**，就可以**在任何位置渲染交互式的** React 组件。

## 在现有网站的子路由中使用 React 

假设你在 `example.com` 部署了一个其他服务端技术（例如 Rails）构建的 Web 应用，但是你又想在 `example.com/some-app/` 部署一个 React 项目。

以下是推荐的配置方式：

1. 使用一个 [基于 React 的框架](https://zh-hans.react.dev/learn/start-a-new-react-project) 构建 **应用的 React 部分**。
2. **在框架配置中将 `/some-app` 指定为基本路径**（这里有 [Next.js](https://nextjs.org/docs/app/api-reference/config/next-config-js/basePath) 与 [Gatsby](https://www.gatsbyjs.com/docs/how-to/previews-deploys-hosting/path-prefix/) 的配置样例）。
3. **配置服务器或代理**，以便所有位于 `/some-app/` 下的请求都由 React 应用处理。

这可以确保应用的 React 部分可以受益于这些框架中内置的 [最佳实践](https://zh-hans.react.dev/learn/build-a-react-app-from-scratch#consider-using-a-framework)。

许多基于 React 的框架**都是全栈的**，从而可以让你的 React 应用充分利用服务器。但是，即使无法或不想在服务器上运行 JavaScript，也可以使用相同的方法。在这种情况下，将 HTML/CSS/JS 导出（Next.js 的 [`next export` output](https://nextjs.org/docs/advanced-features/static-html-export)，Gatsby 的 default）替换为 `/some-app/`。
