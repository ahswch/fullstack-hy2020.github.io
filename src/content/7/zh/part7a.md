---
mainImage: ../../../images/part-7.svg
part: 7
letter: a
lang: zh
---

<div class="content">


The exercises in this seventh part of the course differ a bit from the ones before. In this and the next chapter there is, as usual [exercises related to the theory in the chapter](/en/part7/react_router#exercises-7-1-7-3).
本课程第七部分的练习与以前的有一点不同。 在本章和下一章中，像往常一样有[与本章理论相关的练习](/ en / part7 / react router # exercises-7-1-7-3)。

In addition to the exercises in this and the chapter, there is a series of exercises which revise what we've learned during the whole course by expanding the Bloglist application which we worked on during parts 4 and 5.
除了本章和本章的练习外，还有一系列的练习，通过扩展我们在第4和第5部分中使用的 Bloglist 应用程序来修改我们在整个课程中学到的知识。

### Application navigation structure
# # # 应用程式导航结构

Following part 6, we return to React without Redux.
在第6部分之后，我们回到 Redux 中的 React。

It is very common for web-applications to have a navigation bar, which enables switching the view of the application.
对于 web 应用程序来说，有一个导航条是很常见的，它可以切换应用程序的视图。

Our app could have a main page
我们的应用程序可以有一个主页

![](../../images/7/1ea.png)


and separate pages for showing information on notes and users:
以及显示备注及使用者资料的独立网页:

![](../../images/7/2ea.png)


In an [old school web app](/en/part0/fundamentals_of_web_apps#traditional-web-applications), changing the page shown by the application would be accomplished by the browser making a HTTP GET request to the server and rendering the HTML representing the view that was returned.
在[老式 web 应用程序](/ en / part0 / web 应用程序基础 # traditional-web-applications)中，更改应用程序显示的页面将由浏览器向服务器发出 HTTP GET 请求并显示表示返回视图的 HTML 来完成。

In single page apps, we are, in reality, always on the same page. The Javascript code run by the browser creates an illusion of different "pages". If HTTP requests are made when switching view, they are only for fetching JSON formatted data, which the new view might require for it to be shown.
在单页应用程序中，我们实际上总是在同一页上。 浏览器运行的 Javascript 代码会产生不同“页面”的错觉。 如果 HTTP 请求是在切换视图时发出的，那么它们只用于获取 JSON 格式的数据，新视图可能需要这些数据才能显示出来。

The navigation bar and an application containing multiple views is very easy to implement using React.
导航栏和包含多个视图的应用程序非常容易使用 React 来实现。

Here is one way:
这里有一个方法:

```js
import React, { useState } from 'react'
import ReactDOM from 'react-dom'

const Home = () => (
  <div> <h2>TKTL notes app</h2> </div>
)

const Notes = () => (
  <div> <h2>Notes</h2> </div>
)

const Users = () => (
  <div> <h2>Users</h2> </div>
)

const App = () => {
  const [page, setPage] = useState('home')

 const toPage = (page) => (event) => {
    event.preventDefault()
    setPage(page)
  }

  const content = () => {
    if (page === 'home') {
      return <Home />
    } else if (page === 'notes') {
      return <Notes />
    } else if (page === 'users') {
      return <Users />
    }
  }

  const padding = {
    padding: 5
  }

  return (
    <div>
      <div>
        <a href="" onClick={toPage('home')} style={padding}>
          home
        </a>
        <a href="" onClick={toPage('notes')} style={padding}>
          notes
        </a>
        <a href="" onClick={toPage('users')} style={padding}>
          users
        </a>
      </div>

      {content()}
    </div>
  )
}

ReactDOM.render(<App />, document.getElementById('root'))
```

Each view is implemented as its own component. We store the view component information in the application state called <i>page</i>. This information tells us which component, representing a view, should be shown below the menu bar.
每个视图都作为自己的组件实现。 我们将视图组件信息存储在名为 i page / i 的应用程序状态中。 这个信息告诉我们，表示视图的哪个组件应该显示在菜单栏下面。

However, the method is not very optimal. As we can see from the pictures, the address stays the same even though at times we are in different views. Each view should preferably have its own address, e.g. to make bookmarking possible. The <i>back</i>-button doesn't work as expected for our application either, meaning that <i>back</i> doesn't move you to the previously displayed view of the application, but somewhere completely different. If the application were to grow even bigger and we wanted to, for example, add separate views for each user and note, then this self made <i>routing</i>, which means the navigation management of the application, would get overly complicated.
然而，这种方法并不十分理想。 正如我们从图片中看到的，即使有时我们处于不同的视角，地址仍然保持不变。 每个视图最好都有自己的地址，例如使书签成为可能。 I back / i-button 对于我们的应用程序也不能正常工作，这意味着 i back / i 不会将您移动到以前显示的应用程序视图，而是移动到完全不同的位置。 如果应用程序变得更大，例如，我们希望为每个用户添加单独的视图和注释，那么这个自制的 i routing / i (这意味着应用程序的导航管理)将变得过于复杂。

<!-- Reactissa on onneksi olemassa kirjasto [React router](https://github.com/ReactTraining/react-router) joka tarjoaa erinomaisen ratkaisun React-sovelluksen navigaation hallintaan. -->

Luckily React has the [React router](https://github.com/ReactTraining/react-router)-library, which provides an excellent solution for managing navigation in a React-application.
幸运的是 React 有[ React router ]( https://github.com/reacttraining/React-router )-library，它为管理 React-application 中的导航提供了一个很好的解决方案。

Let's change the above application to use React router. First, we install React router with the command
让我们将上面的应用程序改为使用 React 路由器

```js
npm install --save react-router-dom
```

The routing provided by React Router is enabled by changing the application as follows:
React Router 提供的路由通过更改应用程序启用，如下所示:

```js
import {
  BrowserRouter as Router,
  Switch, Route, Link
} from "react-router-dom"

const App = () => {

  const padding = {
    padding: 5
  }

  return (
    <Router>
      <div>
        <Link style={padding} to="/">home</Link>
        <Link style={padding} to="/notes">notes</Link>
        <Link style={padding} to="/users">users</Link>
      </div>

      <Switch>
        <Route path="/notes">
          <Notes />
        </Route>
        <Route path="/users">
          <Users />
        </Route>
        <Route path="/">
          <Home />
        </Route>
      </Switch>

      <div>
        <i>Note app, Department of Computer Science 2020</i>
      </div>
    </Router>
  )
}
```

Routing, or the conditional rendering of components <i>based on the url</i> in the browser, is used by placing components as children of the <i>Router</i> component, meaning inside <i>Router</i>-tags.
路由，或基于浏览器中的 url / i 的组件的条件呈现，通过将组件放置为 i Router / i 组件的子组件来使用，这意味着在 i Router / i-tags 内部。

Notice that, even though the component is referred to by the name <i>Router</i>, we are in fact talking about [BrowserRouter](https://reacttraining.com/react-router/web/api/BrowserRouter), because here the import happens by renaming the imported object:
注意，即使组件名为 i Router / i，我们实际上讨论的是[ BrowserRouter ]( https://reacttraining.com/react-Router/web/api/BrowserRouter ) ，因为这里的导入是通过重命名导入的对象实现的:

```js
import {
  BrowserRouter as Router, // highlight-line
  Switch, Route, Link
} from "react-router-dom"
```

According to the [manuaal](https://reacttraining.com/react-router/web/api/BrowserRouter) 
根据[ manuaal ]( https://reacttraining.com/react-router/web/api/browserrouter )

> <i>BrowserRouter</i> is a <i>Router</i> that uses the HTML5 history API (pushState, replaceState and the popState event) to keep your UI in sync with the URL.
I BrowserRouter / i 是一个 i Router / i，它使用 HTML5历史 API (pushState、 replaceState 和 popState 事件)保持 UI 与 URL 同步。

Normally the browser loads a new page when the URL in the address bar changes. However, with the help of the [HTML5 history API](https://css-tricks.com/using-the-html5-history-api/) <i>BrowserRouter</i> enables us to use the URL in the address bar of the browser for internal "routing" in a React-application. So, even if the URL in the address bar changes, the content of the page is only manipulated using Javascript, and the browser will not load new content form the server. Using the back and forward actions, as well as making bookmarks, is still logical like on a traditional web page.
通常，当地址栏中的 URL 发生更改时，浏览器会加载一个新页面。 然而，借助于[ HTML5历史 API ]( https://css-tricks.com/using-the-HTML5-history-API/ ) i BrowserRouter / i，我们可以使用浏览器地址栏中的 URL 在 React-application 中进行内部“路由”。 因此，即使地址栏中的 URL 发生了变化，页面的内容也只能通过 Javascript 来操作，浏览器也不会从服务器加载新的内容。 使用后退和前进操作，以及制作书签，仍然像在传统网页上一样合乎逻辑。

Inside the router we define <i>links</i> that modify the address bar with the help of the [Link](https://reacttraining.com/react-router/web/api/Link) component. For example,
在路由器内部，我们定义了 i links / i，这个 i links / i 借助于[ Link ]( https://reacttraining.com/react-router/web/api/Link )组件来修改地址栏,

```js
<Link to="/notes">notes</Link>
```

creates a link in the application with the text <i>notes</i>, which when clicked changes the URL in the address bar to <i>/notes</i>.
在应用程序中创建一个带有文本 i notes / i 的链接，当单击该文本时，会将地址栏中的 URL 更改为 i / notes / i。

Components rendered based on the URL of the browser are defined with the help of the component [Route](https://reacttraining.com/react-router/web/api/Route). For example, 
基于浏览器的 URL 呈现的组件是在组件[ Route ]( https://reacttraining.com/react-router/web/api/Route )的帮助下定义的,

```js
<Route path="/notes">
  <Notes />
</Route>
```

<!-- määrittelee, että jos selaimen osoiteena on <i>/notes</i>, renderöidään komponentti <i>Notes</i>. -->

defines, that if the browser address is <i>/notes</i>, we render the <i>Notes</i> component.
定义，如果浏览器地址是 i / Notes / i，则呈现 i Notes / i 组件。

<!-- Urliin perustuen renderöitävät komponentit on sijoitettu [Swithch](https://reacttraining.com/react-router/web/api/Switch)-komponentin lapsiksi -->

We wrap the components to be rendered based on the url with a [Swithch](https://reacttraining.com/react-router/web/api/Switch)-component
我们用一个[ Swithch ]( https://reacttraining.com/react-router/web/api/switch )-组件包装要基于 url 呈现的组件

```js 
<Switch>
  <Route path="/notes">
    <Notes />
  </Route>
  <Route path="/users">
    <Users />
  </Route>
  <Route path="/">
    <Home />
  </Route>
</Switch>
```

<!-- Switch saa aikaan sen, että renderöitävä komponentti on ensimmäinen, jonka <i>path</i> vastaa osoiterivin polkua. -->
——把 aikan sen 调换一下，让它在 ensimm inen，jonka i path / i vastaa osoiterivin polkua 上显示出来
The switch works so, that we render the first component which's <i>path</i> matches the url in the browser's address bar.
这个开关的工作原理是，我们呈现第一个组件，它的 i path / i 匹配浏览器地址栏中的 url。

<!-- Huomaa, että komponenttien järjestys on tärkeä. Jos laittaisimme ensimmäiseksi komponentin <i>Home</i>, jonka polku on <i> path="/"</i>, ei mitää muuta komponenttia koskaan renderöitäisi, sillä "olematon" polku on minkä tahansa polun alkuosa: -->

Note, that the order of the components is important. If we would put the <i>Home</i>-component, which's path is <i> path="/"</i>, first, nothing else would ever get rendered because the "non existing" path "/" is the start of every path:
注意，组件的顺序很重要。 如果我们使用 i Home / i-component，它的路径是 i path” / ” / i，首先，没有其他东西会被渲染，因为“ non existing” path” / ”是每个路径的开始:

```js 
<Switch>
  <Route path="/"> // highlight-line
    <Home /> // highlight-line
  </Route> // highlight-line
  
  <Route path="/notes">
    <Notes />
  </Route>
  // ...
</Switch>
```

### Parameterized route
# # # 参数化路由

Let's examine the slightly modified version from the previous example. The complete code for the example can be found [here](https://github.com/fullstack-hy2020/misc/blob/master/router-app-v1.js).
让我们检查一下前一个例子中稍微修改过的版本，这个例子的完整代码可以在这里找到( https://github.com/fullstack-hy2020/misc/blob/master/router-app-v1.js )。

The application now contains five different views, the display of which is controlled by the router. In addition to the components from the previous example (<i>Home</i>, <i>Notes</i> and <i>Users</i>), we have <i>Login</i> representing the login view and <i>Note</i> representing the view of a single note.
应用程序现在包含五个不同的视图，其显示由路由器控制。 除了前面示例中的组件(i Home / i、 i Notes / i 和 i Users / i)外，我们还有 i Login / i 表示登录视图，i Note / i 表示单个注释的视图。

<i>Home</i> and <i>Users</i> are unchanged from the previous exercise.  <i>Notes</i> is a bit more complicated. It renders the list of notes passed to it as props in such a way that the name of each note is clickable.
I Home / i 和 i Users / i 与上次练习相同。 I Notes / i 有点复杂。 它以这样一种方式呈现作为道具传递给它的音符列表，即每个音符的名称都是可点击的。

![](../../images/7/3ea.png)


The ability to click a name is implemented with the component <i>Link</i>, and clicking the name of a note whose id is 3 would trigger an event that changes the address of the browser into <i>notes/3</i>:
单击名称的能力是通过组件 i Link / i 实现的，单击 id 为3的注释的名称将触发一个事件，该事件将浏览器地址更改为 i notes / 3 / i:

```js
const Notes = ({notes}) => (
  <div>
    <h2>Notes</h2>
    <ul>
      {notes.map(note =>
        <li key={note.id}>
          <Link to={`/notes/${note.id}`}>{note.content}</Link>
        </li>
      )}
    </ul>
  </div>
)
```

<!-- Parametrisoitu url määritellään komponentissa <i>App</i> olevaan reititykseen seuraavasti: -->

We define parametrized urls in the routing in <i>App</i>-component as follows:
我们在 i App / i-component 的路由中定义参数化 url 如下:

```js
<Router>
  <div>
    <div>
      <Link style={padding} to="/">home</Link>
      <Link style={padding} to="/notes">notes</Link>
      <Link style={padding} to="/users">users</Link>
    </div>

    <Switch>
    // highlight-start
      <Route path="/notes/:id">
        <Note notes={notes} />
      </Route>
      // highlight-end
      <Route path="/notes">
        <Notes notes={notes} />
      </Route>
      <Route path="/">
        <Home />
      </Route>
    </Switch>

</Router>
```

<!-- Yksittäisen muistiinpanon näkymän renderöivä route siis määritellään "expressin tyyliin" merkkaamalla reitin parametrina oleva osa merkinnällä <i>:id</i> -->

We define the route rendering a specific note "express style" by marking the parameter with a colon <i>:id</i>
我们通过用冒号 i: id / i 标记参数来定义呈现特定注释的路由“ express style”

```js
<Route path="/notes/:id">
```

<!-- Kun selain siirtyy muistiinpanon yksilöivään osoitteeseen, esim. <i>/notes/3</i>, renderöidään komponentti <i>Note</i>: -->
-- kun selain siirtyy muistiinpanon yksil iv n osoitteeseen，esim. i / notes / 3 / i，render id n komponentti i Note / i: --
When a browser navigates to the url for a specific note, for example <i>/notes/3</i>, we render the <i>Note</i> component:
当浏览器导航到特定笔记的 url 时，例如 i / notes / 3 / i，我们呈现 i Note / i 组件:

```js
import {
  // ...
  useParams  // highlight-line
} from "react-router-dom"

const Note = ({ notes }) => {
  const id = useParams().id // highlight-line
  const note = notes.find(n => n.id === Number(id)) 
  return (
    <div>
      <h2>{note.content}</h2>
      <div>{note.user}</div>
      <div><strong>{note.important ? 'important' : ''}</strong></div>
    </div>
  )
}
```

<!-- Komponentti _Note_ saa parametrikseen kaikki muistiinpanot propsina <i>notes</i> ja se pääsee urlin yksilöivään osaan, eli näytettävän muistiinpanon id:hen käsiksi  react-routerin funktion [useParams](https://reacttraining.com/react-router/web/api/Hooks/useparams) avulla.  -->

The _Note_ component receives all of the notes as props <i>notes</i>, and it can access the url parameter (the id of the note to be displayed) with the [useParams](https://reacttraining.com/react-router/web/api/Hooks/useparams) function of the react-router.
Note 组件接收所有的笔记作为 props i notes / i，它可以通过 react-router 的[ useParams ]( https://reacttraining.com/react-router/web/api/hooks/useParams )函数访问 url 参数(要显示的笔记的 id)。

### useHistory
使用历史

<!-- Sovellukseen on myös toteutettu erittäin yksinkertainen kirjautumistoiminto. Jos sovellukseen ollaan kirjautuneena, talletetaan tieto kirjautuneesta käyttäjästä komponentin <i>App</i> tilaan <i>user</i>. -->

We have also implemented a simple log in function in our application. If a user is logged in, information about a logged in user is saved to the <i>user</i> field of the state of the <i>App</i> component.
我们还在应用程序中实现了一个简单的登录函数。 如果用户登录，则关于登录用户的信息将保存到 i App / i 组件状态的 i user / i 字段中。

The option to navigate to the <i>Login</i>-view is rendered conditionally in the menu.
导航到 i Login / i-view 的选项在菜单中有条件地呈现。

```js
<Router>
  <div>
    <Link style={padding} to="/">home</Link>
    <Link style={padding} to="/notes">notes</Link>
    <Link style={padding} to="/users">users</Link>
    // highlight-start
    {user
      ? <em>{user} logged in</em>
      : <Link style={padding} to="/login">login</Link>
    }
    // highlight-end
  </div>

  // ...
</Router>
```

So if the user is already logged in, instead of displaying the link <i>Login</i> we show the username of the user:
因此，如果用户已经登录，我们不显示链接 i Login / i，而是显示用户的用户名:

![](../../images/7/4a.png)


The code of the component handling the login functionality is as follows 
处理登录功能的组件代码如下

```js
import {
  // ...
  useHistory // highlight-line
} from 'react-router-dom'

const Login = (props) => {
  const history = useHistory() // highlight-line

  const onSubmit = (event) => {
    event.preventDefault()
    props.onLogin('mluukkai')
    history.push('/') // highlight-line
  }

  return (
    <div>
      <h2>login</h2>
      <form onSubmit={onSubmit}>
        <div>
          username: <input />
        </div>
        <div>
          password: <input type='password' />
        </div>
        <button type="submit">login</button>
      </form>
    </div>
  )
}
```

<!-- Mielenkiinoista komponentissa on react-routerin funktion [useHistory](https://reacttraining.com/react-router/web/api/Hooks/usehistory) käyttö. Funktion avulla komponentti pääsee käsiksi [history](https://reacttraining.com/react-router/web/api/history)-olioon, joka taas mahdollistaa mm. selaimen osoiterivin muokkaamisen ohjelmallisesti. -->

What is interesting about this component is the use of the [useHistory](https://reacttraining.com/react-router/web/api/Hooks/usehistory) function of the react-router.
这个组件的有趣之处在于它使用了反应路由器的[ useHistory ]( https://reacttraining.com/react-router/web/api/hooks/useHistory 路由器)功能。
With this function the component can access a [history](https://reacttraining.com/react-router/web/api/history) object. The history object can be used to i.a modify the browser url programmatically.
有了这个函数，组件就可以访问一个[ https://reacttraining.com/react-router/web/api/history ]对象。 历史记录对象可以用于 i.a 编程修改浏览器的 url。

<!-- Kirjautumisen yhteydessä kutsutaan history-olion metodia push. Komento _history.push('/')_ saa aikaan sen, että selaimen osoiteriville tulee osoitteeksi _/_ ja sovellus renderöi osoitetta vastaavan komponentin <i>Home</i>. -->

With user log in we call the push method of the history object. The  _history.push('/')_ call causes the browser url to change to _/_ and the application renders the corresponding component <i>Home</i>.
对于用户登录，我们调用历史对象的 push 方法。 History.push (’ / ’)调用导致浏览器的 url 更改为 / ，应用程序呈现相应的组件 i Home / i。

<!-- Käyttämämme react-router-kirjaston funktiot [useParams](https://reacttraining.com/react-router/web/api/Hooks/useparams) ja [useHistory](https://reacttraining.com/react-router/web/api/Hooks/usehistory) ovat molemmat hook-funktiota, samaan tapaan kuin esim. moneen kertaan käyttämämme useState ja useEffect. Kuten muistamme osasta 1, hook-funktioiden käyttöön liittyy tiettyjä [sääntöjä](/osa1/monimutkaisempi_tila_reactin_debuggaus#hookien-saannot). Create-react-app on konfiguroitu varoittamaan, jos hookien säännöt rikkoutuvat, esim. jos hook-funktiota yritetään kutsua ehtolauseen sisältä.  -->

Both [useParams](https://reacttraining.com/react-router/web/api/Hooks/useparams) and [useHistory](https://reacttraining.com/react-router/web/api/Hooks/usehistory) are hook-functions, just like useState and useEffect we have used many times now.  As you remember from part 1, there are some [rules](/osa1/monimutkaisempi_tila_reactin_debuggaus#hookien-saannot) to using hook-functions. Create-react-app has been configured to warn you, if you break these rules e.g by calling a hook-function from a conditional statement.
Https://reacttraining.com/react-router/web/api/hooks/useParams 和 https://reacttraining.com/react-router/web/api/hooks/useHistory 都是钩子函数，就像我们已经多次使用的 useState 和 useEffect 一样。 正如您在第1部分中记得的，使用钩函数有一些[规则](/ osa1 / monimutkaisempi tila reactin debuggaus # hookenen-saannot)。 创建-反应-应用程序已经配置为警告你，如果你打破这些规则，例如通过调用一个钩子函数从一个 If判断语句。

### redirect
重新定向

There is one more interesting detail about the <i>Users</i> route: 
关于 i Users / i 路由还有一个有趣的细节:

```js
<Route path="/users" render={() =>
  user ? <Users /> : <Redirect to="/login" />
} />
```

If a user isn't logged in, the <i>Users</i> component is not rendered. Instead the user is <i>redirected</i> using the <i>Redirect</i>-component to the login view
如果用户未登录，则不呈现 i Users / i 组件。 相反，用户 i 使用 i Redirect / i-component 重定向到登录视图

```js
<Redirect to="/login" />
```

In reality it would perhaps be better to not even show links in the navigation bar requiring login if the user is not logged into the application.
实际上，如果用户没有登录到应用程序，甚至不在需要登录的导航栏中显示链接也许会更好。

Here is the <i>App</i> component in its entirety:
下面是 i App / i 组件的全部内容:

```js
const App = () => {
  const [notes, setNotes] = useState([
    // ...
  ])

  const [user, setUser] = useState(null) 

  const login = (user) => {
    setUser(user)
  }

  const padding = { padding: 5 }

  return (
    <div>
      <Router>
        <div>
          <Link style={padding} to="/">home</Link>
          <Link style={padding} to="/notes">notes</Link>
          <Link style={padding} to="/users">users</Link>
          {user
            ? <em>{user} logged in</em>
            : <Link style={padding} to="/login">login</Link>
          }
        </div>

        <Switch>
          <Route path="/notes/:id">
            <Note notes={notes} />
          </Route>
          <Route path="/notes">
            <Notes notes={notes} />
          </Route>
          <Route path="/users">
            {user ? <Users /> : <Redirect to="/login" />}
          </Route>
          <Route path="/login">
            <Login onLogin={login} />
          </Route>
          <Route path="/">
            <Home />
          </Route>
        </Switch>
      </Router>      
      <div>
        <br />
        <em>Note app, Department of Computer Science 2020</em>
      </div>
    </div>
  )
}
```
We define an element common for modern web apps called <i>footer</i>, which defines the part at the bottom of the screen, outside of the <i>Router</i>, so that it is shown regardless of the component shown in the routed part of the application.
我们定义了一个现代 web 应用程序通用的元素 i footer / i，它定义了屏幕底部 i Router / i 之外的部分，因此不管应用程序路由部分显示的是哪个组件，它都会显示出来。


### Parameterized route revisited
# # # 重新访问参数化路线

<!-- Sovelluksessa on eräs hieman ikävä seikka. Komponentti _Note_ saa propseina kaikki muistiinpanot, vaikka se näyttää niistä ainoastaan sen, jonka id vastaa urlin parametroitua osaa: -->

Our application has a flaw. The _Note_ component receives all of the notes, even though it only displays the one which's id matches the url parameter:
我们的申请有一个缺陷。 组件接收所有的笔记，即使它只显示与 url 参数匹配的 id:


```js
const Note = ({ notes }) => { 
  const id = useParams().id
  const note = notes.find(n => n.id === Number(id))
  // ...
}
```

<!-- Olisiko mahdollista muuttaa sovellusta siten, että _Note_ saisi propsina ainoastaan näytettävän komponentin: -->

Would it be possible to modify the application so, that _Note_ receives only the component it should display?
是否有可能修改应用程序，使 Note 只接收它应该显示的组件？

```js
const Note = ({ note }) => {
  return (
    <div>
      <h2>{note.content}</h2>
      <div>{note.user}</div>
      <div><strong>{note.important ? 'important' : ''}</strong></div>
    </div>
  )
}
```

<!-- Eräs tapa muuttaa sovellusta olisi selvittää näytettävän muistiinpanon _id_ komponentissa _App_ react-routerin hook-funktion [useRouteMatch](https://reacttraining.com/react-router/web/api/Hooks/useroutematch) avulla. -->

One way to do this would be to use react-router's [useRouteMatch](https://reacttraining.com/react-router/web/api/Hooks/useroutematch) hook to figure out the id of the note to be displayed in the _App_ component.
一种方法是使用 react-router 的[ useRouteMatch ]( https://reacttraining.com/react-router/web/api/hooks/useRouteMatch )钩子来计算出应用程序组件中显示的注释的 id。

<!-- <i>useRouteMatch</i>-hookin käyttö [ei ole](https://github.com/ReactTraining/react-router/issues/7015)  mahdollista samassa komponentissa, joka määrittelee sovelluksen reititettävän osan. Siirretäänkin _Router_-komponenttien käyttö komponentin _App_ ulkopuolelle: -->

It is not possible to use <i>useRouteMatch</i>-hook in the component which defines the routed part of the application. Let's move the use of the _Router_ components from _App_:
在定义应用程序路由部分的组件中不可能使用 i useRouteMatch / i-hook。 让我们把路由器组件的使用从 App 中移除:

```js
ReactDOM.render(
  <Router> // highlight-line
    <App />
  </Router>, // highlight-line
  document.getElementById('root')
)
```

<!-- Komponentti _App_ muuttuu seuraavasti: -->

The _App_component becomes:
应用程序组件变成:

```js
import {
  // ...
  useRouteMatch  // highlight-line
} from "react-router-dom"

const App = () => {
  // ...

 // highlight-start
  const match = useRouteMatch('/notes/:id')
  const note = match 
    ? notes.find(note => note.id === Number(match.params.id))
    : null
  // highlight-end

  return (
    <div>
      <div>
        <Link style={padding} to="/">home</Link>
        // ...
      </div>

      <Switch>
        <Route path="/notes/:id">
          <Note note={note} /> // highlight-line
        </Route>
        <Route path="/notes">
          <Notes notes={notes} />
        </Route>
         // ...
      </Switch>

      <div>
        <em>Note app, Department of Computer Science 2020</em>
      </div>
    </div>
  )
}    
```

<!-- Joka kerta kun komponentti renderöidään, eli käytännössä myös aina kun sovelluksen osoiterivillä oleva url, vaihtuu suoritetaan komento -->

Every time the component is rendered, so practically every time the browser url changes, the following command is executed
每次呈现组件时，实际上每次浏览器 url 发生更改时，都会执行以下命令

```js
const match = useRouteMatch('/notes/:id')
```

<!-- Jos url on muotoa _/notes/:id_ eli vastaa yksittäisen muistiinpanon urlia, saa muuttuja _match_ arvokseen olion, jonka polun parametroitu osa, eli muistiinpanon id voidaan selvittää, ja näin saadaan haettua renderöitävä muistiinpano -->

If the url matches _/notes/:id_, the match variable will contain an object from which we can access the parametrized part of the path, the id of the note to be displayed, and we can then fetch the correct note to display
如果 url 匹配 / notes / : id，match 变量将包含一个对象，我们可以从该对象访问路径的参数化部分，即要显示的注释的 id，然后我们可以获取要显示的正确注释

```js
const note = match 
  ? notes.find(note => note.id === Number(match.params.id))
  : null
```

<!-- Lopullinen koodi on kokonaisuudessaan [täällä](https://github.com/fullstack-hy2020/misc/blob/master/router-app-v2.js). -->

The completed code can be found from [here](https://github.com/fullstack-hy2020/misc/blob/master/router-app-v2.js).
完成的代码可以在这里找到( https://github.com/fullstack-hy2020/misc/blob/master/router-app-v2.js )。

</div>
/ div
<div class="tasks">
Div 类”任务”

### Exercises 7.1.7.3.
练习7.1.7.3。

Let's return to working with anecdotes. Use the redux-free anecdote app found in the repository <https://github.com/fullstack-hy2020/routed-anecdotes> as the starting point for the exercises.
让我们继续研究奇闻轶事。 使用存储库 https://github.com/fullstack-hy2020/routed-anecdotes 中的 redux-free 轶事应用程序作为练习的起点。

If you clone the project into an existing git repository remember to <i>delete the git configuration of the cloned application:</i>
如果您将该项目克隆到现有的 git 存储库中，请记住我删除克隆应用程序的 git 配置: / i

```bash
cd routed-anecdotes   // go first to directory of the cloned repository
rm -rf .git
```

The application starts the usual way, but first you need to install the dependencies of the application:
应用程序以通常的方式启动，但是首先你需要安装应用程序的依赖项:

```bash
npm install
npm start
```

#### 7.1: routed anecdotes, step1
7.1: 失败的奇闻轶事，第一步

Add React Router to the application so that by clicking links in the <i>Menu</i>-component the view can be changed.
向应用程序添加 React Router，以便通过单击 i Menu / i-component 中的链接可以更改视图。

At the root of the application, meaning the path _/_, show the list of anecdotes:
在应用程序的根部，即路径 / ，显示奇闻异事列表:

![](../../assets/teht/40.png)


The <i>Footer</i>-component should always be visible at the bottom.
I Footer / i-component 应该始终在底部可见。

The creation of a new anecdote should happen e.g. in the path <i>create</i>:
一个新奇闻的创作应该发生在例如我创建 / i 的路径上:

![](../../assets/teht/41.png)


#### 7.2: routed anecdotes, step2
7.2: 失败的奇闻轶事，第二步

Implement a view for showing a single anecdote:
实现一个展示单一事件的视图:

![](../../assets/teht/42.png)


Navigating to the page showing the single anecdote is done by clicking the name of that anecdote
导航到显示单个轶事的页面是通过单击该轶事的名称来完成的

![](../../assets/teht/43.png)


#### 7.3: routed anecdotes, step3
7.3: 失败的奇闻轶事，第三步

The default functionality of the creation form is quite confusing, because nothing seems to be happening after creating a new anecdote using the form.
创建表单的默认功能相当混乱，因为在使用该表单创建一个新奇闻之后，似乎什么都没有发生。

Improve the functionality such that after creating a new anecdote the application transitions automatically to showing the view for all anecdotes <i>and</i> the user is shown a notification informing them of this successful creation for the next 10 seconds:
改进功能，比如在创建一个新的轶事后，应用程序会自动转换为显示所有轶事 i 和 / i 的视图，用户会看到一个通知，告诉他们在接下来的10秒内成功创建了这个视图:

![](../../assets/teht/44.png)


</i>
我
=======END=======
完