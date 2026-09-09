[toc]

# 新项目如何运行

```bash
cd <your-project-name>
$ npm install
$ npm run dev
```

# 什么是 Vue？

Vue (发音为 /vjuː/，类似 **view**) 是一款用于构建用户界面的 JavaScript 框架。它基于标准 HTML、CSS 和 JavaScript 构建，并提供了一套声明式的、组件化的编程模型，帮助你高效地开发用户界面。

下面是一个最基本的示例：

```js
import { createApp, ref } from 'vue'

createApp({
  setup() {
    return {
      count: ref(0)
    }
  }
}).mount('#app')
```

```template
<div id="app">
  <button @click="count++">
    Count is: {{ count }}
  </button>
</div>
```

上面的示例展示了 Vue 的两个核心功能：

- **声明式渲染**：Vue 基于标准 HTML 拓展了一套模板语法，使得我们可以**声明式地描述最终输出的 HTML 和 JavaScript 状态之间的关系**。
- **响应性**：Vue 会**自动跟踪 JavaScript** 状态并在其发生**变化时响应式地更新 DOM**。

## 渐进式框架

Vue 是一个框架，也是一个生态。其功能覆盖了大部分前端开发常见的需求。但 Web 世界是十分多样化的，不同的开发者在 Web 上构建的东西可能在形式和规模上会有很大的不同。考虑到这一点，Vue 的设计非常注重**灵活性**和**“可以被逐步集成”**这个特点。根据你的需求场景，你可以用不同的方式使用 Vue：

- 无需构建步骤，渐进式增强静态的 HTML
- 在任何页面中作为 Web Components 嵌入
- **单页应用 (SPA)**
- **全栈 / 服务端渲染 (SSR)**
- Jamstack / 静态站点生成 (SSG)
- 开发桌面端、移动端、WebGL，甚至是命令行终端中的界面

## 单文件组件

在大多数启用了构建工具的 Vue 项目中，我们可以使用一种**类似 HTML 格式的文件来书写 Vue 组件**，它被称为**单文件组件** (也被称为 `*.vue` 文件，英文 **Single-File Components**，缩写为 **SFC**)。顾名思义，Vue 的单文件组件会将一个组件的**逻辑 (JavaScript)，模板 (HTML) 和样式 (CSS) 封装在同一个文件里**。下面我们将用单文件组件的格式重写上面的计数器示例：

```vue
<script setup>
import { ref } from 'vue'
const count = ref(0)
</script>

<template>
  <button @click="count++">Count is: {{ count }}</button>
</template>

<style scoped>
button {
  font-weight: bold;
}
</style>
```

单文件组件是 Vue 的标志性功能。

## API 风格

Vue 的组件可以按两种不同的风格书写：**选项式 API** 和**组合式 API**。

### 选项式 API (Options API)

使用选项式 API，我们可以用**包含多个选项的对象**来描述组件的逻辑，例如 `data`、`methods` 和 `mounted`。选项所定义的**属性都会暴露在函数内部的 `this` 上**，它**会指向当前的组件实例**。

```vue
<script>
// 导出一个默认的Vue组件选项对象
export default {
  // data() 返回的属性将会成为响应式的状态
  // 并且暴露在 `this` 上
  data() {
    return {
      count: 0 // 定义一个初始值为0的响应式数据count
    }
  },

  // methods 是一些用来更改状态与触发更新的函数
  // 它们可以在模板中作为事件处理器绑定
  methods: {
    increment() {
      this.count++
    }
  },

  // 生命周期钩子会在组件生命周期的各个不同阶段被调用
  // 例如这个函数就会在组件挂载完成后被调用
  mounted() {
    console.log(`The initial count is ${this.count}.`)
  }
}
</script>

<template>
<--事件绑定指令,点击时触发increment语句-->
  <button @click="increment">Count is: {{ count }}</button>
</template>
```

## 组合式 API (Composition API)

通过组合式 API，我们可以**使用导入的 API 函数来描述组件逻辑**。在单文件组件中，**组合式 API 通常会与 <script setup> 搭配使用**。这个 setup attribute 是一个标识，告诉 Vue 需要在**编译时进行一些处理**，让我们可以更简洁地使用组合式 API。比如，<script setup> 中的**导入和顶层变量/函数**都能够**在模板中直接使用**。

下面是使用了组合式 API 与 <script setup> 改造后和上面的模板完全一样的组件：

```vue
<script setup>
import { ref, onMounted } from 'vue'

// 响应式状态
const count = ref(0)

// 用来修改状态、触发更新的函数
function increment() {
  count.value++
}

// 生命周期钩子
onMounted(() => {
  console.log(`The initial count is ${count.value}.`)
})
</script>

<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>
```

## 该选哪一个？

两种 API 风格都能够覆盖大部分的应用场景。它们只是同一个底层系统所提供的两套不同的接口。实际上，**选项式 API 是在组合式 API 的基础上实现**的！关于 Vue 的基础概念和知识在它们之间都是通用的。

选项式 API 以“组件实例”的概念为中心 (即上述例子中的 this)，对于有**面向对象**语言背景的用户来说，这通常与基于类的心智模型更为一致。同时，它将响应性相关的细节抽象出来，并强制按照选项来组织代码，从而对初学者而言更为友好。

组合式 API 的核心思想是直接在函数作用域内定义响应式状态变量，并将从多个函数中得到的状态组合起来处理复杂问题。这种形式更加自由，也需要你对 Vue 的响应式系统有更深的理解才能高效使用。相应的，它的**灵活性**也使得组织和重用逻辑的模式变得更加强大。

在组合式 API FAQ 章节中，你可以了解更多关于这两种 API 风格的对比以及组合式 API 所带来的潜在收益。

如果你是使用 Vue 的新手，这里是我们的大致建议：

在学习的过程中，推荐采用**更易于自己理解的风格**。再强调一下，大部分的核心概念在这两种风格之间都是通用的。熟悉了一种风格以后，你也能够很快地理解另一种风格。

在生产项目中：

当你不需要使用构建工具，或者打算主要在低复杂度的场景中使用 Vue，例如**渐进增强**的应用场景，推荐采用**选项式 API**。

当你打算用 Vue 构建**完整的单页应用**，推荐采用**组合式 API + 单文件组件**。

在学习阶段，你不必只固守一种风格。在接下来的文档中我们会为你提供一系列两种风格的代码供你参考，你可以随时通过左上角的 API 风格偏好来做切换。

# 声明式渲染

你在编辑器中看到的是一个 Vue 单文件组件 (Single-File Component，缩写为 SFC)。单文件组件是一种可复用的代码组织形式，它将从属于同一个组件的 HTML、CSS 和 JavaScript 封装在使用 `.vue` 后缀的文件中。

Vue 的核心功能是**声明式渲染**：通过扩展于标准 HTML 的模板语法，我们可以根据 JavaScript 的状态来描述 HTML 应该是什么样子的。当状态改变时，HTML 会自动更新。

能在改变时触发更新的状态被称作是**响应式**的。我们可以使用 **Vue 的 `reactive()` API 来声明响应式状态**。由 `reactive()` 创建的对象都是 JavaScript [Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)，其行为**与普通对象一样**：

```vue
import { reactive } from 'vue'

const counter = reactive({
  count: 0
})

console.log(counter.count) // 0
counter.count++
```

`reactive()` **只适用于对象 (包括数组和内置类型，如 `Map` 和 `Set`)**。而另一个 API `ref()` 则可以**接受任何值类型**。`ref` 会**返回一个包裹对象**，并在 `.value` 属性下暴露内部值。

```vue
import { ref } from 'vue'

const message = ref('Hello World!')

console.log(message.value) // "Hello World!"
message.value = 'Changed'
```

`reactive()` 和 `ref()` 的细节在[指南 - 响应式基础](https://cn.vuejs.org/guide/essentials/reactivity-fundamentals.html)一节中有进一步讨论。

在组件的 `<script setup>` 块中**声明的响应式状态**，可以直接在模板中使用。下面展示了我们如何使用**双花括号语法**，根据 `counter` 对象和 `message` ref 的值**渲染动态文本**：

```vue
<h1>{{ message }}</h1>
<p>Count is: {{ counter.count }}</p>
```

注意我们在模板中访问的 `message` ref 时**不需要使用 `.value`：它会被自动解包**，让使用更简单。

在双花括号中的内容并**不只限于标识符或路径**——我们可以使用任何有效的 JavaScript 表达式。

```vue
<h1>{{ message.split('').reverse().join('') }}</h1>
```

# Attribute 绑定

在 Vue 中，mustache 语法 (即双大括号) **只能用于文本插值**。为了给 a**ttribute 绑定一个动态值**，需要使用 **`v-bind` 指令**：

```vue
<div v-bind:id="dynamicId"></div>
```

**指令**是由 `v-` 开头的一种特殊 attribute。它们是 Vue 模板语法的一部分。和文本插值类似，指令的值是可以访问组件状态的 JavaScript 表达式。

冒号后面的部分 (`:id`) 是**指令的“参数**”。此处，**元素的 `id` attribute** 将与**组件状态里的 `dynamicId` 属性保持同步**。

由于 `v-bind` 使用地非常频繁，它有一个专门的简写语法：

```vue
<div :id="dynamicId"></div>
```

# 事件监听

我们可以使用 **`v-on` 指令**监听 **DOM 事件**：

```vue
<button v-on:click="increment">{{ count }}</button>
```

因为其经常使用，`v-on` 也有一个简写语法：

```vue
<button @click="increment">{{ count }}</button>
```

此处，`increment` 引用了一个在 `<script setup>` 中声明的函数：

```vue
<script setup>
import { ref } from 'vue'
const count = ref(0)
function increment() {
  // 更新组件状态
  count.value++
}
</script>
```

在函数中，我们可以通过**修改 ref** 来更新组件状态。

事件处理函数也可以使用内置表达式，并且可以**使用修饰符简化常见任务**。这些细节包含在[指南 - 事件处理](https://cn.vuejs.org/guide/essentials/event-handling.html)。

# 表单绑定

我们可以同时使用 `v-bind` 和 `v-on` 来在表单的输入元素上**创建双向绑定**：

```vue
<input :value="text" @input="onInput">
```

```vue
function onInput(e) {
  // v-on 处理函数会接收原生 DOM 事件
  // 作为其参数。
  text.value = e.target.value
}
```

试着在文本框里输入——你会看到 `**<p>` 里的文本也随着你的输入更新**了。

为了简化双向绑定，Vue 提供了一个 **`v-model` 指令**，它实际上是上述操作的**语法糖**：

```vue
<input v-model="text">
```

`v-model` 会将**被绑定的值**与 **`<input>` 的值自动同步**，这样我们就不必再使用事件处理函数了。

`v-model` 不仅支持文本输入框，也支持诸如**多选框、单选框、下拉框**之类的输入类型。

# 条件渲染

我们可以使用 `v-if` 指令来有条件地渲染元素：

```vue
<h1 v-if="awesome">Vue is awesome!</h1>
```

这个 `<h1>` 标签只会在 `awesome` 的值为[真值 (Truthy)](https://developer.mozilla.org/zh-CN/docs/Glossary/Truthy) 时渲染。若 `awesome` 更改为[假值 (Falsy)](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy)，它将被**从 DOM 中移除**。

我们也可以使用 `v-else` 和 `v-else-if` 来表示其他的条件分支：

```vue
<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>
```

现在，示例程序同时展示了两个 `<h1>` 标签，并且按钮不执行任何操作。尝试给它们添加 `v-if` 和 `v-else` 指令，并**实现 `toggle()` 方法**，让我们可以使用按钮在它们之间切换。

```vue
<script setup>
import { ref } from 'vue'

const awesome = ref(true)

function toggle() {
  awesome.value = !awesome.value//注意awesome在这里是个对象而非单值
}
</script>

<template>
  <button @click="toggle">Toggle</button>
  <h1 v-if="awesome">Vue is awesome!</h1>
  <h1 v-else>Oh no 😢</h1>
</template>
```

# 列表渲染

我们可以使用 `v-for` 指令来渲染一个基于源数组的列表：

```vue
<ul>
  <li v-for="todo in todos" :key="todo.id">
    {{ todo.text }}
  </li>
</ul>
```

这里的 `todo` 是一个局部变量，表示当前正在迭代的数组元素。它**只能在 `v-for` 所绑定的元素上或是其内部访问**，就像函数的**作用域**一样。

注意，我们还给**每个 todo 对象设置了唯一的 `id`**，并且将它作为[特殊的 `key` attribute](https://cn.vuejs.org/api/built-in-special-attributes.html#key) 绑定到每个 `<li>`。`key` 使得 Vue 能够**精确地移动每个 `<li>`**，以匹配对应的对象在数组中的位置。

更新列表有两种方式：

1. 在源数组上调用[变更方法](https://stackoverflow.com/questions/9009879/which-javascript-array-functions-are-mutating)：

	```js
	todos.value.push(newTodo)
	```

2. 使用新的数组**替代**原数组：

	```js
	todos.value = todos.value.filter(/* ... */)
	```

这里有一个简单的 todo 列表

```vue
<script setup>
import { ref } from 'vue'

// 给每个 todo 对象一个唯一的 id
let id = 0

const newTodo = ref('')
const todos = ref([
  { id: id++, text: 'Learn HTML' },
  { id: id++, text: 'Learn JavaScript' },
  { id: id++, text: 'Learn Vue' }
])

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value })
  newTodo.value = ''
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)//满足条件的才不会被放入,否则筛去
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo" required placeholder="new todo">
    <button>Add Todo</button>
  </form>
  <ul>
    <li v-for="todo in todos" :key="todo.id">
      {{ todo.text }}
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
</template>
```

# 计算属性

让我们在上一步的 todo 列表基础上继续。现在，我们已经给每一个 todo 添加了切换功能。这是通过给每一个 todo 对象**添加 `done` 属性来实现的**，并且使用了 `v-model` 将其绑定到复选框上：

```vue
<li v-for="todo in todos">
  <input type="checkbox" v-model="todo.done">
  ...
</li>
```

下一个可以添加的改进是**隐藏**已经完成的 todo。我们已经有了一个能够切换 `hideCompleted` 状态的按钮。但是应该如何基于状态渲染不同的列表项呢？

介绍一个新 API：[`computed()`](https://cn.vuejs.org/guide/essentials/computed.html)。它可以让我们创建一个**计算属性 ref**，这个 ref 会**动态地根据其他响应式数据源来计算其 `.value`**：

```js
import { ref, computed } from 'vue'

const hideCompleted = ref(false)
const todos = ref([
  /* ... */
])

const filteredTodos = computed(() => {
  // 根据 `todos.value` & `hideCompleted.value`
  // 返回过滤后的 todo 项目
      return hideCompleted.value
    ? todos.value.filter((t) => !t.done)
    : todos.value
})
```

```css
- <li v-for="todo in todos">
+ <li v-for="todo in filteredTodos">
```

计算属性会**自动跟踪其计算中所使用的到的其他响应式状态**，并将它们收集为自己的依赖。计算**结果会被缓存**，并只有在其**依赖发生改变时才会被自动更新**。

# 生命周期和模板引用

目前为止，Vue 为我们处理了所有的 DOM 更新，这要归功于响应性和声明式渲染。然而，有时我们也会**不可避免地需要手动操作 DOM**。

这时我们需要使用**模板引用**——也就是**指向模板中一个 DOM 元素的 re**f。我们需要通过[这个特殊的 `ref` attribute](https://cn.vuejs.org/api/built-in-special-attributes.html#ref) 来实现模板引用:

```html
<p ref="pElementRef">hello</p>
```

要访问该引用，我们需要声明一个**同名的 ref：**

```js
const pElementRef = ref(null)
```

注意这个 ref **使用 `null` 值来初始化**。这是因为当 `<script setup>` 执行时，DOM 元素还不存在。模板引用 ref 只能在组件**挂载**后访问。

要在**挂载之后执行代码**，我们可以使用 `onMounted()` 函数：

```js
import { onMounted } from 'vue'

onMounted(() => {
  // 此时组件已经挂载。
})
```

这被称为**生命周期钩子**——它允许我们注册一个**在组件的特定生命周期调用的回调函数**。还有一些其他的钩子如 **`onUpdated` 和 `onUnmounted`**。更多细节请查阅[生命周期图示](https://cn.vuejs.org/guide/essentials/lifecycle.html#lifecycle-diagram)。

现在，尝试添加一个 `onMounted` 钩子，然后通过 `pElementRef.value` 访问 `<p>`，并**直接对其执行一些 DOM 操作**。(例如修改它的 `textContent`)。

```js
pElementRef.value.textContent = 'mounted!'//需要去再带一个textContent
```

# 侦听器

有时我们需要响应性地执行一些“副作用”——例如，当一个**数字改变时将其输出到控制台**。我们可以通过侦听器来实现它：

```vue
import { ref, watch } from 'vue'//需要额外导入这个函数

const count = ref(0)

watch(count, (newCount) => {
  // 没错，console.log() 是一个副作用
  console.log(`new count is: ${newCount}`)
})
```

`watch()` 可以**直接侦听一个 ref**，并且只要 **`count` 的值改变就会触发回调**。`watch()` 也可以侦听其他类型的数据源

一个比在控制台输出更加实际的例子是当 **ID 改变时抓取新的数据**。在右边的例子中就是这样一个组件。该组件被挂载时，会从**模拟 API 中抓取 todo 数据**，同时还有一个按钮可以**改变要抓取的 todo 的 ID**。现在，尝试实现一个侦听器，使得组件能够在按钮被点击时抓取新的 todo 项目。

```vue
<script setup>
import { ref, watch } from 'vue'
// “ref”是用来存储值的响应式数据源。
// 理论上我们在展示该字符串的时候不需要将其包装在 ref() 中，
// 但是在下一个示例中更改这个值的时候，我们就需要它了。
const todoId = ref(1)
const todoData = ref(null)

async function fetchData() {
  todoData.value = null
  const res = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )//浏览器内置的网络请求 API，用于向指定 URL 发送 HTTP 请求（默认是 GET 方法）,模板字符串 `${}` 中的 todoId.value：表示动态拼接 URL,await：等待 fetch 请求完成,并将请求的响应结果（Response 对象）赋值给 res
  todoData.value = await res.json()//将响应的 JSON 格式字符串解析为 JavaScript 对象（同样返回一个 Promise）,再次使用 await 等待解析完成，将解析后的对象赋值给 todoData.value
}

fetchData()

watch(todoId, fetchData)
</script>

<template>
  <p>Todo id: {{ todoId }}</p>
  <button @click="todoId++" :disabled="!todoData">Fetch next todo</button>
  <p v-if="!todoData">Loading...</p>
  <pre v-else>{{ todoData }}</pre>
</template>
```

# 组件

目前为止，我们只使用了单个组件。真正的 Vue 应用往往是由**嵌套组件创建**的。

父组件可以在模板中渲染另一个组件作为子组件。要**使用子组件，我们需要先导入**它：

```js
import ChildComp from './ChildComp.vue'
```

然后我们就**可以在模板中使用组件**，就像这样：

```vue
<ChildComp /><!--这样写内含中止了-->
```

# Props

子组件可以通过 **props** 从**父**组件**接受动态数据**。首先，需要声明它所接受的 props：

```vue
<!-- ChildComp.vue -->
<script setup>
const props = defineProps({
  msg: String
})
</script>
```

注意 `defineProps()` 是一个**编译时宏**，并不需要导入。一旦声明，**`msg` prop 就可以在子组件的模板中使用**。它也可以通过 `defineProps()` 所返回的对象在 JavaScript 中访问。

父组件可以像声明 HTML attributes 一样传递 props。若**要传递动态值，也可以使用 `v-bind` 语法**：

```vue
<ChildComp :msg="greeting" />
```

# Emits

除了**接收 props**，子组件还可以向父组件触发事件：

```vue
<script setup>
// 声明触发的事件
const emit = defineEmits(['response'])
// 带参数触发
emit('response', 'hello from child')
</script>
```

`emit()` 的第一个参数是**事件的名称**。其他**所有参数都将传递给事件监听器**。

父组件**可以使用 `v-on` 监听**子组件触发的事件——这里的处理函数接收了子组件**触发事件时的额外参数**并将它赋值给了本地状态：

```vue
<ChildComp @response="(msg) => childMsg = msg" />
```

# 插槽

除了通过 props 传递数据外，父组件还可以通过**插槽** (slots) 将模板片段传递给子组件：

```vue
<ChildComp>
  This is some slot content!<!--如此写即为传入内容了-->
</ChildComp>
```

在子组件中，可以使用 **`<slot>` 元素作为插槽出口 (slot outlet) 渲染父组件中的插槽内容 (slot content)**：

```vue
<!-- 在子组件的模板中 ,不在父组件中-->
<slot/>
```

`<slot>` 插口中的内容将被当作**“默认”内容**：它会在**父组件没有传递任何插槽内容**时显示：

```vue
<slot>Fallback content</slot>
```

现在我们没有给 `<ChildComp>` 传递任何插槽内容，所以你将看到默认内容。
