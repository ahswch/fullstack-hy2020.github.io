---
mainImage: ../../../images/part-1.svg
part: 1
letter: c
lang: zh
---

<div class="content">


Let's go back to working with React.
让我们回到工作与反应。

We start with a new example:
我们从一个新的例子开始:

```js
const Hello = (props) => {
  return (
    <div>
      <p>
        Hello {props.name}, you are {props.age} years old
      </p>
    </div>
  )
}

const App = () => {
  const name = 'Peter'
  const age = 10

  return (
    <div>
      <h1>Greetings</h1>
      <Hello name="Maya" age={26 + 10} />
      <Hello name={name} age={age} />
    </div>
  )
}
```

### Component helper functions
# # # 组件辅助函数

Let's expand our <i>Hello</i> component so that it guesses the year of birth of the person being greeted:
让我们扩展 i Hello / i 组件，以便它可以猜测被问候者的出生年份:

```js
const Hello = (props) => {
  // highlight-start
  const bornYear = () => {
    const yearNow = new Date().getFullYear()
    return yearNow - props.age
  }
  // highlight-end

  return (
    <div>
      <p>
        Hello {props.name}, you are {props.age} years old
      </p>
      <p>So you were probably born in {bornYear()}</p> // highlight-line
    </div>
  )
}
```

The logic for guessing the year of birth is separated into its own function that is called when the component is rendered.
猜测出生年份的逻辑被分离到它自己的函数中，这个函数在呈现组件时被调用。

The person's age does not have to be passed as a parameter to the function, since it can directly access all props that are passed to the component.
用户的年龄不必作为参数传递给函数，因为它可以直接访问传递给组件的所有道具。

If we examine our current code closely, we'll notice that the helper function is actually defined inside of another function that defines the behavior of our component. In Java-programming, defining a method inside another method is not possible, but in JavaScript, defining functions within functions is a commonly used technique.
如果仔细检查当前代码，我们会注意到 helper 函数实际上是在另一个定义组件行为的函数中定义的。 在 java 编程中，在另一个方法中定义一个方法是不可能的，但在 JavaScript 中，在函数中定义函数是一种常用的技术。

### Destructuring
破坏

Before we move forward, we will take a look at a small but useful feature of the JavaScript language that was added in the ES6 specification, that allows us to [destructure](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) values from objects and arrays upon assignment.
在我们继续前进之前，我们将看一看在 ES6规范中添加的 JavaScript 语言的一个小的但是有用的特性，它允许我们在赋值时从对象和数组中[去结构化]( https://developer.mozilla.org/en-us/docs/web/JavaScript/reference/operators/destructuring_assignment  / 值]。

In our previous code, we had to reference the data passed to our component as _props.name_ and _props.age_. Of these two expressions we had to repeat _props.age_ twice in our code.
在前面的代码中，我们必须将传递给组件的数据引用为 props.name 和 props.age。 在这两个表达式中，我们必须在代码中重复 props.age 两次。

Since <i>props</i> is an object
因为 i props / i 是一个对象

```js
props = {
  name: 'Arto Hellas',
  age: 35,
}
```

we can streamline our component by assigning the values of the properties directly into two variables _name_ and _age_ which we can then use in our code:
我们可以通过将属性值直接赋值为两个变量 name 和 age 来简化我们的组件，然后我们可以在代码中使用这两个变量:

```js
const Hello = (props) => {
  // highlight-start
  const name = props.name
  const age = props.age
  // highlight-end

  const bornYear = () => new Date().getFullYear() - age

  return (
    <div>
      <p>Hello {name}, you are {age} years old</p>
      <p>So you were probably born in {bornYear()}</p>
    </div>
  )
}
```

Note that we've also utilized the more compact syntax for arrow functions when defining the _bornYear_ function. As mentioned earlier, if an arrow function consists of a single command, then the function body does not need to be written inside of curly braces. In this more compact form, the function simply returns the result of the single command.
注意，在定义 bornYear 函数时，我们还为箭头函数使用了更紧凑的语法。 如前所述，如果一个箭头函数由单个命令组成，那么函数体就不需要在花括号中写入。 在这种更紧凑的形式中，函数只返回单个命令的结果。

To recap, the two function definitions shown below are equivalent:
总结一下，下面显示的两个函数定义是等价的:
```js
const bornYear = () => new Date().getFullYear() - age

const bornYear = () => {
  return new Date().getFullYear() - age
}
```

Destructuring makes the assignment of variables even easier, since we can use it to extract and gather the values of an object's properties into separate variables:
析构使变量的赋值变得更加容易，因为我们可以使用它来提取和收集对象属性的值到单独的变量中:

```js
const Hello = (props) => {
    // highlight-start
  const { name, age } = props
    // highlight-end
  const bornYear = () => new Date().getFullYear() - age

  return (
    <div>
      <p>Hello {name}, you are {age} years old</p>
      <p>So you were probably born in {bornYear()}</p>
    </div>
  )
}
```

<!-- Eli koska -->

If the object we are destructuring has the values
如果我们要析构的对象具有值
```js
props = {
  name: 'Arto Hellas',
  age: 35,
}
```

the expression <em>const { name, age } = props</em> assigns the values 'Arto Hellas' to _name_ and 35 to _age_.
表达式 em const { name，age } props / em 将值‘ Arto Hellas’赋值给 name，35赋值给 age。

We can take destructuring a step further:
我们可以进一步破坏:
```js
const Hello = ({ name, age }) => { // highlight-line
  const bornYear = () => new Date().getFullYear() - age

  return (
    <div>
      <p>
        Hello {name}, you are {age} years old
      </p>
      <p>So you were probably born in {bornYear()}</p>
    </div>
  )
}
```

The props that are passed to the component are now directly destructured into the variables _name_ and _age_.
传递给组件的道具现在直接析构为变量 name 和 age。

This means that instead of assigning the entire props object into a variable called <i>props</i> and then assigning its properties into the variables _name_ and _age_
这意味着不需要将整个 props 对象分配到一个名为 i props / i 的变量中，然后将其属性分配到变量 name 和 age 中

```js
const Hello = (props) => {
  const { name, age } = props
```

we assign the values of the properties directly to variables by destructuring the props object that is passed to the component function as a parameter:
我们将 props 对象作为参数传递给组件函数，通过对 props 对象进行析构，直接将属性值赋给变量:

```js
const Hello = ({ name, age }) => {
```

### Page re-rendering
页面重新渲染

So far all of our applications have been such that their appearance remains the same after the initial rendering. What if we wanted to create a counter where the value increased as a function of time or at the click of a button?
到目前为止，我们的所有应用程序都是这样的，即在最初的渲染之后，它们的外观仍然是相同的。 如果我们想要创建一个计数器，在这个计数器中值随着时间的变化而增加，或者点击一个按钮而增加，会怎么样呢？

Let's start with the following body:
让我们从下面的主体开始:

```js
const App = (props) => {
  const {counter} = props
  return (
    <div>{counter}</div>
  )
}

let counter = 1

ReactDOM.render(
  <App counter={counter} />, 
  document.getElementById('root')
)
```

The root component is given the value of the counter in the _counter_ prop. The root component renders the value to the screen. But what happens when the value of _counter_ changes? Even if we were to add the command
根部分量给出了反支柱中计数器的值。 根组件将值呈现到屏幕上。 但是当计数器的值发生变化时会发生什么呢？ 即使我们要添加命令

```js
counter += 1
```

the component won't re-render. We can get the component to re-render by calling the _ReactDOM.render_ method a second time, e.g. in the following way:
部件不会重新渲染。 我们可以通过第二次调用 ReactDOM.render 方法让组件重新呈现，例如:

```js
const App = (props) => {
  const { counter } = props
  return (
    <div>{counter}</div>
  )
}

let counter = 1

const refresh = () => {
  ReactDOM.render(<App counter={counter} />, 
  document.getElementById('root'))
}

refresh()
counter += 1
refresh()
counter += 1
refresh()
```

The re-rendering command has been wrapped inside of the _refresh_ function to cut down on the amount of copy-pasted code.
重新呈现命令包装在刷新函数中，以减少复制粘贴代码的数量。

Now the component  <i>renders three times</i>, first with the value 1, then 2, and finally 3. However, the values 1 and 2 are displayed on the screen for such a short amount of time that they can't be witnessed.
现在组件 i 呈现三次 / i，首先是值1，然后是2，最后是3。 但是，值1和2在屏幕上显示的时间非常短，因此无法看到它们。

We can implement slightly more interesting functionality by re-rendering and incrementing the counter every second by using [setInterval](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setInterval):
我们可以通过使用[ setInterval ]( https://developer.mozilla.org/en-us/docs/web/api/windoworworkerglobalscope/setInterval  :

```js
setInterval(() => {
  refresh()
  counter += 1
}, 1000)
```

Making repeated calls to the _ReactDOM.render_-method is not the recommended way to re-render components. Next, we'll introduce a better way of accomplishing this effect.
重复调用 ReactDOM.render-method 不是重新呈现组件的推荐方法。 接下来，我们将介绍一种更好的实现这种效果的方法。

### Stateful component
# # # 有状态组件

All of our components up till now have been simple in the sense that they have not contained any state that could change during the lifecycle of the component.
到目前为止，我们的所有组件都很简单，因为它们没有包含任何可能在组件生命周期中发生变化的状态。

Next, let's add state to our application's <i>App</i> component with the help of React's [state hook](https://reactjs.org/docs/hooks-state.html).
接下来，让我们在 React’ s [ state hook ]( https://reactjs.org/docs/hooks-state.html )的帮助下向应用程序的 i App / i 组件添加状态。

We will change the application to the following:
我们会把申请改为:

```js
import React, { useState } from 'react' // highlight-line
import ReactDOM from 'react-dom'

const App = (props) => {
  const [ counter, setCounter ] = useState(0) // highlight-line

// highlight-start
  setTimeout(
    () => setCounter(counter + 1),
    1000
  )
  // highlight-end

  return (
    <div>{counter}</div>
  )
}

ReactDOM.render(
  <App />, 
  document.getElementById('root')
)
```

In the first row, the application imports the _useState_-function:
在第一行，应用程序导入 useState-function:

```js
import React, { useState } from 'react'
```

The function body that defines the component begins with the function call:
定义组件的函数体以函数调用开始:

```js
const [ counter, setCounter ] = useState(0)
```

The function call adds <i>state</i> to the component and renders it initialized with the value of zero. The function returns an array that contains two items. We assign the items to the variables _counter_ and _setCounter_ by using the destructuring assignment syntax shown earlier.
函数调用将 i state / i 添加到组件，并将其用值0进行初始化。 该函数返回一个包含两个项的数组。 我们使用前面所示的析构化赋值语法将项分配给变量计数器和 setCounter。

The _counter_ variable is assigned the initial value of <i>state</i> which is zero. The variable _setCounter_ is assigned to a function that will be used to <i>modify the state</i>.
计数器变量被赋予初始值 i state / i 为零。 变量 setCounter 被分配给一个函数，该函数将用于 i 修改 state / i。

The application calls the [setTimeout](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout) function and passes it two parameters: a function to increment the counter state and a timeout of one second:
这个应用程序调用[ setTimeout ]( https://developer.mozilla.org/en-us/docs/web/api/windoworworkerglobalscope/setTimeout )函数并传递给它两个参数: 一个增加计数器状态的函数和一个1秒钟的超时:

```js
setTimeout(
  () => setCounter(counter + 1),
  1000
)
```

The function passed as the first parameter to the _setTimeout_ function is invoked one second after calling the _setTimeout_ function
作为第一个参数传递给 setTimeout 函数的函数在调用 setTimeout 函数后一秒钟被调用

```js
() => setCounter(counter + 1)
```

When the state modifying function _setCounter_ is called, <i>React re-renders the component</i> which means that the function body of the component function gets re-executed:
当状态修改函数 setCounter 被调用时，i React 重新呈现 component / i，这意味着组件函数的函数体被重新执行:

```js
(props) => {
  const [ counter, setCounter ] = useState(0)

  setTimeout(
    () => setCounter(counter + 1),
    1000
  )

  return (
    <div>{counter}</div>
  )
}
```

The second time the component function is executed it calls the _useState_ function and returns the new value of the state: 1. Executing the function body again also makes a new function call to _setTimeout_, which executes the one second timeout and increments the _counter_ state again. Because the value of the _counter_ variable is 1, incrementing the value by 1 is essentially the same as a command setting the state _counter_ value to 2.
第二次执行组件函数时，它调用 useState 函数并返回状态的新值: 1。 再次执行函数体还会对 setTimeout 进行一个新的函数调用，它执行一秒钟的超时并再次递增计数器状态。 因为计数器变量的值是1，所以将该值增加1本质上等同于将状态计数器值设置为2的命令。

```js
() => setCounter(2)
```
Meanwhile, the old value of _counter_,  "1", is rendered to the screen.
同时，计数器的旧值“1”呈现到屏幕上。

Every time the _setCounter_  modifies the state it causes the component to re-render. The value of the state will be incremented again after one second, and this will continue to repeat for as long as the application is running.
每次 setCounter 修改状态时，它都会导致组件重新呈现。 状态的值将在一秒钟后再次递增，并且在应用程序运行期间这将继续重复。

If the component doesn't render when you think it should, or if it renders at the "wrong time", you can debug the application by logging the values of the component's variables to the console. If we make the following additions to our code:
如果组件在您认为应该呈现时没有呈现，或者在“错误的时间”呈现，您可以通过将组件变量的值记录到控制台来调试应用程序。 如果我们在代码中添加以下内容:

```js
const App = (props) => {
  const [ counter, setCounter ] = useState(0)

  setTimeout(
    () => setCounter(counter + 1),
    1000
  )

  console.log('rendering...', counter) // highlight-line

  return (
    <div>{counter}</div>
  )
}
```

It's easy to follow and track the calls made to the _render_ function:
很容易跟踪和跟踪调用的渲染函数:

![](../../images/1/4e.png)


### Event handling
事件处理

We have already mentioned <i>event handlers</i> a few times in [part 0](/en/part0), that are registered to be called when specific events occur. E.g. a user's interaction with the different elements of a web page can cause a collection of various different kinds of events to be triggered.
我们已经在[ part 0](/ en / part0)中多次提到 i 事件处理程序 / i，它们注册为在特定事件发生时调用。 例如，用户与一个网页的不同元素的交互可能会触发一系列不同类型的事件。

Let's change the application so that increasing the counter happens when a user clicks a button, which is implemented with the [button](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button)-element.
让我们修改一下应用程序，这样当用户单击一个按钮时，计数器就会增加，这是通过[ button ]( https://developer.mozilla.org/en-us/docs/web/html/element/button )-元素实现的。

Button-elements support so-called [mouse events](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent), of which [click](https://developer.mozilla.org/en-US/docs/Web/Events/click) is the most common event.
按钮元素支持所谓的[鼠标事件]( https://developer.mozilla.org/en-us/docs/web/api/mouseevent 事件) ，其中[点击]( https://developer.mozilla.org/en-us/docs/web/events/click 事件)是最常见的事件。

In React, registering an event handler function to the <i>click</i> event [happens](https://reactjs.org/docs/handling-events.html) like this:
在 React 中，将一个事件处理函数注册到 i click / i 事件[ happens ]中，如下 https://reactjs.org/docs/handling-events.html  :

```js
const App = (props) => {
  const [ counter, setCounter ] = useState(0)

  // highlight-start
  const handleClick = () => {
    console.log('clicked')
  }
  // highlight-end

  return (
    <div>
      <div>{counter}</div>
      // highlight-start
      <button onClick={handleClick}>
        plus
      </button>
      // highlight-end
    </div>
  )
}
```

We set the value of the button's <i>onClick</i>-attribute to be a reference to the _handleClick_ function defined in the code.
我们将按钮的 i onClick / i-attribute 的值设置为对代码中定义的 handleClick 函数的引用。

Now every click of the <i>plus</i> button causes the _handleClick_ function to be called, meaning that every click event will log a <i>clicked</i> message to the browser console.
现在，每次单击 i plus / i 按钮都会调用 handleClick 函数，这意味着每次单击事件都会将 i clicked / i 消息记录到浏览器控制台。

The event handler function can also be defined directly in the value assignment of the onClick-attribute:
事件处理函数也可以在 onClick-attribute 的值分配中直接定义:

```js
const App = (props) => {
  const [ counter, setCounter ] = useState(0)

  return (
    <div>
      <div>{counter}</div>
      <button onClick={() => console.log('clicked')}> // highlight-line
        plus
      </button>
    </div>
  )
}
```

By changing the event handler to the following form
将事件处理程序更改为以下形式
```js
<button onClick={() => setCounter(counter + 1)}>
  plus
</button>
```

we achieve the desired behavior, meaning that the value of _counter_ is increased by one <i>and</i> the component gets re-rendered.
我们实现了期望的行为，这意味着计数器的值增加了一个 i，而 / i 组件被重新渲染。

Let's also add a button for resetting the counter:
让我们再添加一个重置计数器的按钮:

```js
const App = (props) => {
  const [ counter, setCounter ] = useState(0)

  return (
    <div>
      <div>{counter}</div>
      <button onClick={() => setCounter(counter + 1)}>
        plus
      </button>
      // highlight-start
      <button onClick={() => setCounter(0)}> 
        zero
      </button>
      // highlight-end
    </div>
  )
}
```

Our application is now ready!
我们的应用程序现在已经准备好了！


<!-- ### Tapahtumankäsittelijä on funktio -->


### Event handler is a function
事件处理程序是一个函数

<!-- Nappien tapahtumankäsittelijät on siis määritelty suoraan <i>onClick</i>-attribuuttien määrittelyn yhteydessä seuraavasti: -->

We define the event handlers for our buttons where we declare their <i>onClick</i> attributes:
我们为按钮定义事件处理程序，声明它们的 ionclick / i 属性:

```js
<button onClick={() => setCounter(counter + 1)}> 
  plus
</button>
```

<!-- Entä jos yritämme määritellä tapahtumankäsittelijän hieman yksinkertaisemmassa muodossa: -->
(西班牙语)(西班牙语)
What if we'd try to define the event handlers in a simpler form?
如果我们尝试以更简单的形式定义事件处理程序会怎样？

```js
<button onClick={setCounter(counter + 1)}> 
  plus
</button>
```

<!-- Tämä muutos kuitenkin hajottaa sovelluksemme täysin: -->
-- t m muutos kuitenkin hajottaa sovelluksemme t ysin: -- 
This would completely break our application:
这将完全破坏我们的应用程序:

![](../../images/1/5b.png)


<!-- Mistä on kyse? Tapahtumankäsittelijäksi on tarkoitus määritellä joko <i>funktio</i> tai <i>viite funktioon</i>. Kun koodissa on -->

What's going on? An event handler is supposed to be either a <i>function</i> or a <i>function reference</i>, and when we write
怎么回事？ 事件处理程序应该是一个 i 函数 / i 或一个 i 函数引用 / i，当我们编写时

```js
<button onClick={setCounter(counter + 1)}>
```

<!-- tapahtumankäsittelijäksi tulee määriteltyä <i>funktiokutsu</i>. Sekin on monissa tilanteissa ok, mutta ei nyt. Kun React renderöi metodin ensimmäistä kertaa ja muuttujan <i>counter</i> arvo on 0, se suorittaa kutsun <em>setCounter(0 + 1)</em>, eli muuttaa komponentin tilan arvoksi 1. Tämä taas aiheuttaa komponentin uudelleenrenderöitymisen. Ja sama toistuu uudelleen... -->
!-tapahtumank sittelij ksi tulee m ritelty i funktiokutsu / i. 在莫尼萨，提拉尼萨，好的，非常好。 00，se suorittaa kutsun em setCounter (0 + 1) / em，eli muuttaa komponentin tilan arvoksi 1. 我们需要一个小组来处理这个问题。 [咒语]
the event handler is actually a <i>function call</i>. In many situations this is ok, but not in this particular situation. In the beginning the value of the <i>counter</i> variable is 0. When React renders the method for the first time, it exectues the function call <em>setCounter(0+1)</em>, and changes the value of the component's state to 1. 
事件处理器实际上是一个 i 函数调用 / i。 在很多情况下这是可以的，但在这种特殊情况下就不行了。 一开始 i counter / i 变量的值是0。 当 React 第一次呈现方法时，它执行函数调用 em setCounter (0 + 1) / em，并将组件状态的值更改为1。
This will cause the component to be rerendered, react will execute the setCounter function call again, and the state will change leading to another rerender...
这将导致组件重新运行，react 将再次执行 setCounter 函数调用，并且状态将发生变化，从而导致另一个重新运行..。

<!-- Palautetaan siis tapahtumankäsittelijä alkuperäiseen muotoonsa -->

Let's define the event handlers like we did before
让我们像前面一样定义事件处理程序

```js
<button onClick={() => setCounter(counter + 1)}> 
  plus
</button>
```

<!-- Nyt napin tapahtumankäsittelijän määrittelevä attribuutti <i>onClick</i> saa arvokseen funktion _() => setCounter(counter + 1)_, ja funktiota kutsutaan siinä vaiheessa kun sovelluksen käyttäjä painaa nappia.  -->

Now the button's attribute which defines what happens when the button is clicked, <i>onClick</i>, has the value _() => setCounter(counter +1)_.
现在，按钮的属性定义了单击按钮时发生的事情，i onClick / i 的值为() setCounter (counter + 1)。
The setCounter function is called only when a user clicks the button. 
只有当用户单击按钮时才调用 setCounter 函数。

<!-- Tapahtumankäsittelijöiden määrittely suoraan JSX-templatejen sisällä ei useimmiten ole kovin viisasta. Tässä tapauksessa se tosin on ok, koska tapahtumankäsittelijät ovat niin yksinkertaisia.  -->

Usually defining event handlers within JSX-templates is not a good idea. 
通常在 JSX-templates 中定义事件处理程序并不是一个好主意。
Here it's ok, because our event handlers are so simple. 
这里没问题，因为我们的事件处理程序非常简单。

<!-- Eriytetään kuitenkin nappien tapahtumankäsittelijät omiksi komponentin sisäisiksi apufunktioikseen: -->

Let's separate the event handlers into separate functions anyway: 
无论如何，让我们将事件处理程序分离成单独的函数:

```js
const App = (props) => {
  const [ counter, setCounter ] = useState(0)

// highlight-start
  const increaseByOne = () => setCounter(counter + 1)
  
  const setToZero = () => setCounter(0)
  // highlight-end

  return (
    <div>
      <div>{counter}</div>
      <button onClick={increaseByOne}> // highlight-line
        plus
      </button>
      <button onClick={setToZero}> // highlight-line
        zero
      </button>
    </div>
  )
}
```

<!-- Tälläkin kertaa tapahtumankäsittelijät on määritelty oikein, sillä <i>onClick</i>-attribuutit saavat arvokseen muuttujan, joka tallettaa viitteen funktioon: -->

Here the event handlers have been defined correctly. The value of the <i>onClick</i> attribute is a variable containing a reference to a function:
这里已经正确定义了事件处理程序。 I onClick / i 属性的值是一个包含函数引用的变量:

```js
<button onClick={increaseByOne}> 
  plus
</button>
```

### Passing state to child components
# # # 将状态传递给子组件

It's recommended to write React components that are small and reusable across the application and even across projects. Let's refactor our application so that it's composed of three smaller components, one component for displaying the counter and two components for buttons.
建议编写跨应用程序甚至跨项目的小型且可重用的 React 组件。 让我们重构我们的应用程序，使它由三个较小的组件组成，一个组件用于显示计数器，两个组件用于按钮。

Let's first implement a <i>Display</i> component that's responsible for displaying the value of the counter.
让我们首先实现一个 i Display / i 组件，它负责显示计数器的值。

One best practice in React is to [lift the state up](https://reactjs.org/docs/lifting-state-up.html) high enough in the component hierarchy. The documentation says:
在 React 中的一个最佳实践是将状态提升到组件层次结构中足够高的 https://reactjs.org/docs/lifting-state-up.html:

> <i>Often, several components need to reflect the same changing data. We recommend lifting the shared state up to their closest common ancestor.</i>
通常，几个组件需要反映相同的变化数据。 我们建议将共享状态提升到它们最接近的共同祖先。 我

So let's place the application's state in the <i>App</i> component and pass it down to the <i>Display</i> component through <i>props</i>:
因此，让我们将应用程序的状态放在 i App / i 组件中，并通过 i props / i 将其传递给 i Display / i 组件:

```js
const Display = (props) => {
  return (
    <div>{props.counter}</div>
  )
}
```

We can also utilize [destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) directly in the component function's parameters. Since we're interested in the _counter_ property of the props object, it's possible to simplify the component into the following form:
我们也可以在组件函数的参数中直接使用[ destructuring ]( https://developer.mozilla.org/en-us/docs/web/javascript/reference/operators/destructuring_assignment )。 因为我们对 props 对象的反属性感兴趣，所以可以将组件简化为以下形式:

```js
const Display = ({ counter }) => {
  return (
    <div>{counter}</div>
  )
}
```

Using the component is straightforward, as we only need to pass the state of the _counter_ to component:
使用组件很简单，因为我们只需要将计数器的状态传递给组件:

```js
const App = (props) => {
  const [ counter, setCounter ] = useState(0)

  const increaseByOne = () => setCounter(counter + 1)
  const setToZero = () => setCounter(0)

  return (
    <div>
      <Display counter={counter}/> // highlight-line
      <button onClick={increaseByOne}>
        plus
      </button>
      <button onClick={setToZero}> 
        zero
      </button>
    </div>
  )
}
```

Everything still works. When the buttons are clicked and the <i>App</i> gets re-rendered, all of its children including the <i>Display</i> component are also re-rendered.
一切仍然正常。 当单击按钮并重新呈现 i App / i 时，其所有子元素(包括 i Display / i 组件)也将重新呈现。

Next, let's make a <i>Button</i> component for the buttons of our application. We have to pass the event handler as well as the title of the button through the component's props:
接下来，让我们为应用程序的按钮制作一个 i Button / i 组件。 我们必须通过组件的道具传递事件处理程序以及按钮的标题:

```js
const Button = (props) => {
  return (
    <button onClick={props.handleClick}>
      {props.text}
    </button>
  )
}
```

Our <i>App</i> component now looks like this:
我们的 i App / i 组件现在看起来像这样:

```js
const App = (props) => {
  const [ counter, setCounter ] = useState(0)

  const increaseByOne = () => setCounter(counter + 1)
  const decreaseByOne = () => setCounter(counter - 1)
  const setToZero = () => setCounter(0)

  return (
    <div>
      <Display counter={counter}/>
      // highlight-start
      <Button
        handleClick={increaseByOne}
        text='plus'
      />
      <Button
        handleClick={setToZero}
        text='zero'
      />     
      <Button
        handleClick={decreaseByOne}
        text='minus'
      />           
      // highlight-end
    </div>
  )
}
```

Since we now have an easily reusable <i>Button</i> component, we've also implemented new functionality into our application by adding a button that can be used to decrement the counter.
由于我们现在有一个易于重用的 i Button / i 组件，我们还通过添加一个可用于减少计数器的按钮来实现应用程序中的新功能。

The event handler is passed to the <i>Button</i> component through the _onClick_ prop. The name of the prop itself is not that significant, but our naming choice wasn't completely random, e.g. React's own official [tutorial](https://reactjs.org/tutorial/tutorial.html) suggests this convention.
事件处理程序通过 onClick 道具传递给 i Button / i 组件。 道具的名字本身并不重要，但是我们的命名选择并不是完全随机的，例如 React 自己的官方教程就建议了这个 https://reactjs.org/tutorial/tutorial.html。

### Changes in state cause rerendering
状态的改变导致重新运行

<!-- Kerrataan vielä sovelluksen toiminnan pääperiaatteet.  -->

Let's go over the main principles of how an application works once more.
让我们再次回顾一下应用程序如何工作的主要原则。

<!-- Kun sovellus käynnistyy, suoritetaan komponentin _App_-koodi, joka luo [useState](https://reactjs.org/docs/hooks-reference.html#usestate)-hookin avulla sovellukselle laskurin tilan _counter_. Komponentti renderöi laskimen alkuarvon 0 näyttävän komponentin _Display_ sekä kolme _Button_-komponenttia, joille se asettaa laskurin tilaa muuttavat tapahtumankäsittelijät. -->

When the application starts, the code in _App_ is executed. This code uses an [useState](https://reactjs.org/docs/hooks-reference.html#usestate) - hook to create the application state - value of the counter _counter_.
当应用程序启动时，执行 App 中的代码。 此代码使用[ useState ]( https://reactjs.org/docs/hooks-reference.html#useState )-hook 创建计数器的应用程序状态值。
The component renders the _Display_ component. It displays the counter's value (0), and three _Button_ components. The buttons have event handlers, which are used to change the state of the counter.
该组件呈现 Display 组件。 它显示计数器的值(0)和三个 Button 组件。 这些按钮具有用于更改计数器状态的事件处理程序。

<!-- Kun jotain napeista painetaan, suoritetaan vastaava tapahtumankäsittelijä. Tapahtumankäsittelijä muuttaa komponentin _App_ tilaa funktion _setCounter_ avulla. **Tilaa muuttavan funktion kutsuminen aiheuttaa komponentin uudelleenrenderöitymisen.**  -->

When one of the buttons is clicked, the event handler is executed. The event handler changes the state of the _App_ component with the _setCounter_ function. 
当单击其中一个按钮时，将执行事件处理程序。 事件处理程序使用 setCounter 函数更改 App 组件的状态。
**Calling a function which changes the state causes the component to rerender.**
* * 调用一个改变状态的函数会导致组件重新运行。 * * 

<!-- Eli jos painetaan nappia <i>plus</i>, muuttaa napin tapahtumankäsittelijä tilan _counter_ arvoksi 1 ja komponentti _App_ renderöidään uudelleen. Komponentin uudelleenrenderöinti aiheuttaa sen "alikomponentteina" olevien _Display_- ja _Button_-komponenttien uudelleenrenderöitymisen. _Display_ saa propsin arvoksi laskurin uuden arvon 1 ja _Button_-komponentit saavat propseina tilaa sopivasti muuttavat tapahtumankäsittelijät. -->
1 ja komponentti App render id n uudelleen
So, if a user clicks the <i>plus</i> button, the button's event handler changes the value of _counter_ to 1, and the _App_ component is rerendered. 
因此，如果用户单击 i plus / i 按钮，按钮的事件处理程序将 counter 的值更改为1，并重新运行 App 组件。
This causes its subcomponents _Display_ and _Button_ to also be rerendered. 
这将导致其子组件 Display 和 Button 也被重新运行。
_Display_ receives the new value of the counter, 1, as props. The _Button_ components receive event handlers which can be used to change the state of the counter.
显示接收计数器的新值，1，作为道具。 Button 组件接收可用于更改计数器状态的事件处理程序。

### Refactoring the components
重构组件

<!-- Laskimen arvon näyttävä komponentti on siis seuraava -->

The component displaying the value of the counter is as follows:
显示计数器值的组件如下:

```js
const Display = (props) => {
  return (
    <div>{props.counter}</div>
  )
}
```

<!-- Komponentti tarvitsee ainoastaan <i>propsin</i> kenttää _counter_, joten se voidaan yksinkertaistaa [destrukturoinnin](/osa1/komponentin_tila_ja_tapahtumankasittely#destrukturointi) avulla seuraavaan muotoon: -->

The component only uses the _counter_ field of its <i>props</i>. 
该组件只使用其 i props / i 的计数字段。
This means we can simplify the component by using [destructuring](/en/part1/component_state_event_handlers#destructuring) like so:
这意味着我们可以使用[ destructuring ](/ en / part1 / component state 事件处理程序 # destructuring)简化组件，如下所示:

```js
const Display = ({ counter }) => {
  return (
    <div>{counter}</div>
  )
}
```

<!-- Koska komponentin määrittelevä metodi ei sisällä muuta kuin returnin, voimme määritellä sen hyödyntäen nuolifunktioiden tiiviimpää ilmaisumuotoa -->
——柯斯卡公司让我把他们的钱都还给他们，让他们把我的钱都还给我——
The method defining the component contains only the return statement, so
定义组件的方法只包含 return 语句，因此
we can define the method using the more compact form of arrow functions:
我们可以使用更紧凑的箭头函数来定义方法:

```js
const Display = ({ counter }) => <div>{counter}</div>
```

<!-- Vastaava suoraviivaistus voidaan tehdä myös nappia edustavalle komponentille -->

We can simplify the Button component as well.
我们也可以简化 Button 组件。

```js
const Button = (props) => {
  return (
    <button onClick={props.handleClick}>
      {props.text}
    </button>
  )
}
```

<!-- Eli destrukturoidaan <i>props</i>:ista tarpeelliset kentät ja käytetään nuolifunktioiden tiiviimpää muotoa  -->

We can use destructuring to get only the required fields from <i>props</i>, and use the more compact form of arrow functions:
我们可以使用 destructuring 只从 i props / i 获取所需的字段，并使用更紧凑的箭头函数:

```js
const Button = ({ handleClick, text }) => (
  <button onClick={handleClick}>
    {text}
  </button>
)
```

</div>
