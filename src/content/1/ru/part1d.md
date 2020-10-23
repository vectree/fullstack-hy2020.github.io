---
mainImage: ../../../images/part-1.svg
part: 1
letter: d
lang: ru
---

<div class="content">

### Сложное состояние

<!-- In our previous example the application state was simple as it was comprised of a single integer. What if our application requires a more complex state? -->
В нашем предыдущем примере состояние приложения было простым, поскольку оно хранило только счетчик. Что, если нашему приложению требуется более сложное состояние?

<!-- In most cases the easiest and best way to accomplish this is by using the _useState_ function multiple times to create separate "pieces" of state. -->
В большинстве случаев самый простой и лучший способ добиться этого - использовать функцию _useState_ несколько раз для создания отдельных "частей" состояния.

<!-- In the following code we create two pieces of state for the application named _left_ and _right_ that both get the initial value of 0: -->
В следующем коде мы создаем два отдельные части состояния с именами _left_ и _right_, которые получают начальное значение 0:

```js
const App = (props) => {
  const [left, setLeft] = useState(0)
  const [right, setRight] = useState(0)

  return (
    <div>
      {left}
      <button onClick={() => setLeft(left + 1)}>
        left
      </button>
      <button onClick={() => setRight(right + 1)}>
        right
      </button>
      {right}
    </div>
  )
}
```

<!-- The component gets access to the functions _setLeft_ and _setRight_ that it can use to update the two pieces of state. -->
Компонент получает доступ к функциям _setLeft_ и _setRight_, которые он может использовать для обновления двух частей нашего состояния.

<!-- The component's state or a piece of its state can be of any type. We could implement the same functionality by saving the click count of both the <i>left</i> and <i>right</i> buttons into a single object: -->
Состояние компонента или часть его состояния может быть любого типа. Мы могли бы реализовать ту же функцию, сохранив количество нажатий кнопок <i>left</i> и <i>right</i> в одном объекте:

```js
{
  left: 0,
  right: 0
}
```

В этом случае приложение будет выглядеть так:

```js
const App = (props) => {
  const [clicks, setClicks] = useState({
    left: 0, right: 0
  })

  const handleLeftClick = () => {
    const newClicks = {
      left: clicks.left + 1,
      right: clicks.right
    }
    setClicks(newClicks)
  }

  const handleRightClick = () => {
    const newClicks = {
      left: clicks.left,
      right: clicks.right + 1
    }
    setClicks(newClicks)
  }

  return (
    <div>
      {clicks.left}
      <button onClick={handleLeftClick}>left</button>
      <button onClick={handleRightClick}>right</button>
      {clicks.right}
    </div>
  )
}
```

<!-- Now the component only has a single piece of state and the event handlers have to take care of changing the <i>entire application state</i>. -->
Теперь компонент имеет единое состояние, и обработчики события должны позаботиться об изменении <i>всего состояния приложения</i>.

<!-- The event handler looks a bit messy. When the left button is clicked, the following function is called: -->
Обработчик события выглядит немного запутанным. При нажатии левой кнопки вызывается следующая функция:

```js
const handleLeftClick = () => {
  const newClicks = {
    left: clicks.left + 1,
    right: clicks.right
  }
  setClicks(newClicks)
}
```

<!-- The following object is set as the new state of the application: -->
Следующий объект устанавливается как новое состояние приложения:
```js
{
  left: clicks.left + 1,
  right: clicks.right
}
```

<!-- The new value of the <i>left</i> property is now the same as the value of <i>left + 1</i> from the previous state, and the value of the <i>right</i> property is the same as value of the <i>right</i> property from the previous state. -->
Значение свойства <i>left</i> теперь равно значению <i>left + 1</i> предыдущего состояния, а значение свойства <i>right</i> такое же, как значение свойства <i>right</i> предыдущего состояния.

<!-- We can define the new state object a bit more neatly by using the [object spread](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
syntax that was added to the language specification in the summer of 2018: -->
Мы можем определить новый объект состояния немного более аккуратно, используя [оператор расширения объекта](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/Spread_syntax), который был добавлен в спецификацию языка летом 2018 года:

```js
const handleLeftClick = () => {
  const newClicks = {
    ...clicks,
    left: clicks.left + 1
  }
  setClicks(newClicks)
}

const handleRightClick = () => {
  const newClicks = {
    ...clicks,
    right: clicks.right + 1
  }
  setClicks(newClicks)
}
```

<!-- The syntax may seem a bit strange at first. In practice <em>{ ...clicks }</em> creates a new object that has copies of all of the properties of the _clicks_ object. When we specify a particular property - e.g. <i>right</i> in <em>{ ...clicks, right: 1 }</em>, the value of the _right_ property in the new object will be 1. -->
Сначала синтаксис может показаться немного странным. На практике <em>{ ... clicks }</em> создает новый объект, который имеет копии всех свойств объекта _clicks_. Когда мы указываем конкретное свойство - например, <i>right</i> в <em>{ ... clicks, right: 1 }</em>, значение свойства _right_ в новом объекте будет перезаписано на 1.

В приведенном выше примере строчка:

```js
{ ...clicks, right: clicks.right + 1 }
```

<!-- creates a copy of the _clicks_ object where the value of the _right_ property is increased by one. -->
создает копию объекта _clicks_, в которой значение свойства _right_ увеличивается на единицу.

<!-- Assigning the object to a variable in the event handlers is not necessary and we can simplify the functions to the following form: -->
Присваивать объект переменной в обработчиках события не обязательно, и мы можем упростить функции:

```js
const handleLeftClick = () =>
  setClicks({ ...clicks, left: clicks.left + 1 })

const handleRightClick = () =>
  setClicks({ ...clicks, right: clicks.right + 1 })
```

Некоторые читатели могут спросить, почему мы просто не обновили состояние напрямую:

```js
const handleLeftClick = () => {
  clicks.left++
  setClicks(clicks)
}
```

<!-- The application appears to work. However, <i>it is forbidden in React to mutate state directly</i>, since it can result in unexpected side effects. Changing state has to always be done by setting the state to a new object. If properties from the previous state object are not changed, they need to simply be copied, which is done by copying those properties into a new object, and setting that as the new state. -->
Приложение работает. Однако <i>в React запрещено изменять состояние напрямую</i>, так как это может привести к неожиданным побочным эффектам. Изменение состояния всегда должно выполняться путем установки нового объекта в состояние. Если свойства предыдущего объекта состояния не изменяются, их нужно просто скопировать в новый объект и установить в качестве нового состояния.

<!-- Storing all of the state in a single state object is a bad choice for this particular application; there's no apparent benefit and the resulting application is a lot more complex. In this case storing the click counters into separate pieces of state is a far more suitable choice. -->
Хранение всего состояния в одном объекте состояния - плохой выбор для конкретно этого приложения; очевидной выгоды нет, а полученное приложение выглядит намного сложнее. В этом случае гораздо более подходящим выбором является хранение счетчиков кликов в отдельных частях состояния.

<!-- There are situations where it can be beneficial to store a piece of application state in a more complex data structure. [The official React documentation](https://reactjs.org/docs/hooks-faq.html#should-i-use-one-or-many-state-variables) contains some helpful guidance on the topic. -->
Бывают ситуации, когда может быть полезно сохранить часть состояния приложения в более сложной структуре данных. [Официальная документация React](https://ru.reactjs.org/docs/hooks-faq.html#should-i-use-one-or-many-state-variables) содержит некоторые полезные рекомендации по этой теме.

### Обработка массивов

<!-- Let's add a piece of state to our application containing an array _allClicks_ that remembers every click that has occurred in the application. -->
Давайте добавим в наше приложение состояние, содержащее массив _allClicks_, который запоминает каждый щелчок, произошедший в приложении.

```js
const App = () => {
  const [left, setLeft] = useState(0)
  const [right, setRight] = useState(0)
  const [allClicks, setAll] = useState([]) // highlight-line

// highlight-start
  const handleLeftClick = () => {
    setAll(allClicks.concat('L'))
    setLeft(left + 1)
  }
// highlight-end

// highlight-start
  const handleRightClick = () => {
    setAll(allClicks.concat('R'))
    setRight(right + 1)
  }
// highlight-end

  return (
    <div>
      {left}
      <button onClick={handleLeftClick}>left</button>
      <button onClick={handleRightClick}>right</button>
      {right}
      <p>{allClicks.join(' ')}</p> // highlight-line
    </div>
  )
}
```

<!-- Every click is stored into a separate piece of state called _allClicks_ that is initialized as an empty array: -->
Каждый щелчок сохраняется в отдельной части состояния, называемой _allClicks_, которая вначале инициализируется как пустой массив:

```js
const [allClicks, setAll] = useState([])
```

<!-- When the <i>left</i> button is clicked, we add the letter <i>L</i> to the _allClicks_ array: -->
Когда нажата кнопка <i>left</i>, мы добавляем букву <i>L</i> в массив _allClicks_:

```js
const handleLeftClick = () => {
  setAll(allClicks.concat('L'))
  setLeft(left + 1)
}
```

<!-- The piece of state stored in _allClicks_ is now set to be an array that contains all of the items of the previous state array plus the letter <i>L</i>. Adding the new item to the array is accomplished with the [concat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat) method, that does not mutate the existing array but rather returns a <i>new copy of the array</i> with the item added to it. -->
Состояние _allClicks_ теперь содержит все элементы предыдущего массива состояния плюс букву <i>L</i>. Добавление нового элемента в массив выполняется с помощью метода [concat](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/concat), который не изменяет существующий массив, а возвращает <i>новую копию массива</i> с добавленным к нему элементом.

<!-- As mentioned previously, it's also possible in JavaScript to add items to an array with the [push](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push) method. If we add the item by pushing it to the _allClicks_ array and then updating the state, the application would still appear to work: -->
Как упоминалось ранее, в JavaScript есть возможность добавлять элементы в массив с помощью метода [push](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/push) . Если мы добавим элемент в массив _allClicks_ с помощью метода push и затем обновим состояние, приложение будет работать:

```js
const handleLeftClick = () => {
  allClicks.push('L')
  setAll(allClicks)
  setLeft(left + 1)
}
```

<!-- However, __don't__ do this. As mentioned previously, the state of React components like _allClicks_ must not be mutated directly. Even if mutating state appears to work in some cases, it can lead to problems that are very hard to debug. -->
Однако __не делайте этого__. Как упоминалось ранее, состояние компонентов React (такое как _allClicks_) не должно изменяться напрямую. Даже если все работает, это может привести к проблемам, которые очень трудно отладить.

<!-- Let's take a closer look at how the clicking history is rendered to the page: -->
Давайте рассмотрим, как отрисовывается на странице история кликов:

```js
const App = () => {
  // ...

  return (
    <div>
      {left}
      <button onClick={handleLeftClick}>left</button>
      <button onClick={handleRightClick}>right</button>
      {right}
      <p>{allClicks.join(' ')}</p> // highlight-line
    </div>
  )
}
```

<!-- We call the [join](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join) method on the _allClicks_ array that joins all the items into a single string, separated by the string passed as the function parameter, which in our case is an empty space. -->
Мы вызываем метод [join](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/join) у _allClicks_, который объединяет все элементы в одну строку, разделяя параметров, который был передан в функцию. В данном случае этим параметром является пробел.

### Условный рендеринг

<!-- Let's modify our application so that the rendering of the clicking history is handled by a new <i>History</i> component: -->
Давайте изменим наше приложение так, чтобы рендеринг истории кликов выполнялся в новом компоненте <i>History</i>:

```js
const History = (props) => {
  if (props.allClicks.length === 0) {
    return (
      <div>
        the app is used by pressing the buttons
      </div>
    )
  }

  return (
    <div>
      button press history: {props.allClicks.join(' ')}
    </div>
  )
}

const App = () => {
  // ...

  return (
    <div>
      {left}
      <button onClick={handleLeftClick}>left</button>
      <button onClick={handleRightClick}>right</button>
      {right}
      <History allClicks={allClicks} /> // highlight-line
    </div>
  )
}
```

<!-- Now the behavior of the component depends on whether or not any buttons have been clicked. If not, meaning that the <em>allClicks</em> array is empty, the component renders a div element with some instructions instead: -->
Сейчас поведение компонента зависит от того, были ли нажаты какие-либо кнопки. В противном случае, когда массив <em>allClicks</em> пуст, компонент отображает элемент div с поясняющим текстом:

```js
<div>the app is used by pressing the buttons</div>
```

И во всех остальных случаях компонент отображает историю кликов:

```js
<div>
  button press history: {props.allClicks.join(' ')}
</div>
```

<!-- The <i>History</i> component renders completely different React elements depending on the state of the application. This is called <i>conditional rendering</i>. -->
Компонент <i>History</i> отображает совершенно разные элементы React в зависимости от состояния приложения. Это называется <i>условным рендерингом</i>.

<!-- React also offers many other ways of doing [conditional rendering](https://reactjs.org/docs/conditional-rendering.html). We will take a closer look at this in [part 2](/en/part2). -->
React также предлагает множество других способов выполнения [условного рендеринга](https://ru.reactjs.org/docs/conditional-rendering.html). Мы более подробно рассмотрим их во [второй части](/ru/part2).

<!-- Let's make one last modification to our application by refactoring it to use the _Button_ component that we defined earlier on: -->
Давайте сделаем еще одну модификацию нашего приложения, добавив в него компонент _Button_, с которым мы работали до этого:

```js
const History = (props) => {
  if (props.allClicks.length === 0) {
    return (
      <div>
        the app is used by pressing the buttons
      </div>
    )
  }

  return (
    <div>
      button press history: {props.allClicks.join(' ')}
    </div>
  )
}

// highlight-start
const Button = ({ onClick, text }) => (
  <button onClick={onClick}>
    {text}
  </button>
)
// highlight-end

const App = () => {
  const [left, setLeft] = useState(0)
  const [right, setRight] = useState(0)
  const [allClicks, setAll] = useState([])

  const handleLeftClick = () => {
    setAll(allClicks.concat('L'))
    setLeft(left + 1)
  }

  const handleRightClick = () => {
    setAll(allClicks.concat('R'))
    setRight(right + 1)
  }

  return (
    <div>
      {left}
      // highlight-start
      <Button onClick={handleLeftClick} text='left' />
      <Button onClick={handleRightClick} text='right' />
      // highlight-end
      {right}
      <History allClicks={allClicks} />
    </div>
  )
}
```

### Старая версия React

<!-- In this course we use the [state hook](https://reactjs.org/docs/hooks-state.html) to add state to our React components, which is part of the newer versions of React and is available from version [16.8.0](https://www.npmjs.com/package/react/v/16.8.0) onwards. Before the addition of hooks, there was no way to add state to functional components. Components that required state had to be defined as [class](https://reactjs.org/docs/react-component.html) components, using the JavaScript class syntax. -->
В этом курсе мы используем [хук состояния](https://reactjs.org/docs/hooks-state.html) для добавления состояния в наши компоненты React, который является частью новых изменений React доступных с версии [16.8.0](https://www.npmjs.com/package/react/v/16.8.0) и выше. До хуков не существовало возможности добавлять состояние к функциональным компонентам. Компоненты, требующие состояния, должны были быть определены как [class](https://ru.reactjs.org/docs/react-component.html) с использованием синтаксиса класса JavaScript.

<!-- In this course we have made the slightly radical decision to use hooks exclusively from day one, to ensure that we are learning the future style of React. Even though functional components are the future of React, it is still important to learn the class syntax, as there are billions of lines of old React code that you might end up maintaining someday. The same applies to documentation and examples of React that you may stumble across on the internet. -->
В этом курсе было принято несколько радикальное решение использовать хуки с самого начала, чтобы убедиться в том, что мы используем последние практики React. Несмотря на то, что за функциональными компонентами будущее React, все же важно изучить синтаксис определения компонент с помощью класса. Ведь существуют миллиарды строк старого кода React, которые вы, возможно, когда-нибудь будете поддерживать. То же самое относится к документации и примерам React, которые вы можете найти в Интернете.

<!-- We will learn more about React class components later on in the course. -->
Мы расскажем вам больше об определении React-компонент с помощью классов немного позже.

### Отладка приложений React

<!-- A large part of a typical developer's time is spent on debugging and reading existing code. Every now and then we do get to write a line or two of new code, but a large part of our time is spent on trying to figure out why something is broken or how something works. Good practices and tools for debugging are extremely important for this reason. -->
Большая часть времени разработчика тратится на отладку и чтение существующего кода. Время от времени нам приходится писать что-то новое, но все-таки большая часть нашего времени тратится на попытки выяснить, почему что-то сломано или как что-то работает. По этой причине чрезвычайно важны новейшие методы и инструменты отладки.

<!-- Lucky for us, React is an extremely developer-friendly library when it comes to debugging. -->
К счастью для нас, React - чрезвычайно удобная библиотека, когда дело касается отладки.

<!-- Before we move on, let us remind ourselves of one of the most important rules of web development. -->
Прежде чем двигаться дальше, давайте напомним себе об одном из важнейших правил веб-разработки.

<h4>Первое правило веб-разработки</h4>

>  **Всегда держите консоль разработчика браузера открытой.**
>
> В частности, вкладка <i>Console</i> всегда должна быть открыта, если нет особой причины для просмотра другой вкладки.

<!-- Keep both your code and the web page open together **at the same time, all the time**. -->
Держите ваш код и веб-страницу открытыми **одновременно, все время**.

<!-- If and when your code fails to compile and your browser lights up like a Christmas tree: -->
Если ваш код не компилируется и браузер загорается, как рождественская елка:

![](../../images/1/6e.png)

<!-- don't write more code but rather find and fix the problem **immediately**. There has yet to be a moment in the history of coding where code that fails to compile would miraculously start working after writing large amounts of additional code. I highly doubt that such an event will transpire during this course either. -->
не пишите больше кода, а **немедленно** найдите и устраните проблему. В истории программирования еще не было момента, когда код, который не компилируется, чудесным образом начинал работать после написания дополнительного кода. Очень сомневаюсь, что подобное событие произойдет и на этом курсе.

<!-- Old school, print-based debugging is always a good idea. If the component -->
Отладка на основе вывода в консоль, как в старые добрые времена, - хорошая идея. Если компонент

```js
const Button = ({ onClick, text }) => (
  <button onClick={onClick}>
    {text}
  </button>
)
```

<!-- is not working as intended, it's useful to start printing its variables out to the console. In order to do this effectively, we must transform our function into the less compact form and receive the entire props object without destructuring it immediately: -->
работает не так, как задумано, полезно вывести переменные в консоль. Чтобы сделать это, мы должны преобразовать нашу функцию в менее компактную форму и убрать деструктуризацию для объекта props:

```js
const Button = (props) => {
  console.log(props) // highlight-line
  const { onClick, text } = props
  return (
    <button onClick={onClick}>
      {text}
    </button>
  )
}
```

<!-- This will immediately reveal if, for instance, one of the attributes has been misspelled when using the component. -->
Этот кода сразу же покажет нам, что один из атрибутов был неправильно написан при вызове компонента.

<!-- **NB** When you use _console.log_ for debugging, don't combine _objects_ in a Java-like fashion by using the plus operator. Instead of writing: -->
**Обратите внимание:** когда вы используете _console.log_ для отладки, не объединяйте _объекты_ с помощью оператора плюс, как это делается в языке программирования Java. Вместо того, чтобы писать:

```js
console.log('props value is ' + props)
```

<!-- Separate the things you want to log to the console with a comma: -->
Просто разделите запятыми то, что вы хотите вывести в консоль:

```js
console.log('props value is', props)
```

<!-- If you use the Java-like way of concatenating a string with an object, you will end up with a rather uninformative log message: -->
Если вы воспользуетесь Java-подобным способом объединения строки с объектом, вы получите довольно неинформативное сообщение:

```js
props value is [Object object]
```

<!-- Whereas the items separated by a comma will all be available in the browser console for further inspection. -->
В то время как элементы, разделенные запятой, будут представлены в доступном виде в консоли браузера.

<!-- Logging to the console is by no means the only way of debugging our applications. You can pause the execution of your application code in the Chrome developer console's <i>debugger</i>, by writing the command [debugger](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/debugger) anywhere in your code. -->
Вывод в консоль - далеко не единственный способ отладки приложений. Вы можете приостановить выполнение кода вашего приложения в <i>отладчике</i> консоли разработчика Chrome, написав команду [debugger](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Statements/debugger) в любом месте вашего кода.

<!-- The execution will pause once it arrives at a point where the _debugger_ command gets executed: -->
Выполнение кода будет приостановлено, как только достигнет точки, где выполняется команда _debugger_:

![](../../images/1/7a.png)

<!-- By going to the <i>Console</i> tab, it is easy to inspect the current state of variables: -->
Перейдя на вкладку <i>Console</i>, легко проверить текущее состояние переменных:

![](../../images/1/8a.png)

<!-- Once the cause of the bug is discovered you can remove the _debugger_ command and refresh the page. -->
Как только причина ошибки обнаружена, вы можете удалить команду _debugger_ и обновить страницу.

<!-- The debugger also enables us to execute our code line by line with the controls found on the right-hand side of the <i>Source</i> tab. -->
Отладчик также позволяет нам выполнять наш код построчно с помощью панели управления, находящейся в правой части вкладки <i>Source</i>.

<!-- You can also access the debugger without the _debugger_ command by adding breakpoints in the <i>Sources</i> tab. Inspecting the values of the component's variables can be done in the _Scope_-section: -->
Вы можете получить доступ к отладчику без команды _debugger_, добавив точки останова на вкладке <i>Sources</i>. Проверить значения переменных компонента можно в разделе _Scope_:

![](../../images/1/9a.png)

<!-- It is highly recommended to add the [React developer tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi) extension to Chrome. It adds a new _Components_ tab to the developer tools. The new developer tools tab can be used to inspect the different React elements in the application, along with their state and props: -->
Настоятельно рекомендуется добавить расширение [React developer tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi) в Chrome. Оно добавляет новую вкладку _Components_ в консоль разработчика. Эта вкладка может использоваться для проверки различных элементов React приложения, их состояния и свойств:

![](../../images/1/10ea.png)


<!-- The _App_ component's state is defined like so: -->
Состояние компонента _App_ определяется так:

```js
const [left, setLeft] = useState(0)
const [right, setRight] = useState(0)
const [allClicks, setAll] = useState([])
```

<!-- Dev tools shows the state of hooks in the order of their definition: -->
Консоль разработчика показывает состояние хуков в порядке их определения:

![](../../images/1/11ea.png)

<!-- The first <i>State</i> contains the value of the <i>left</i> state, the next contains the value of the <i>right</i> state and the last contains the value of the <i>allClicks</i> state. -->
Первый <i>State</i> содержит значение состояния <i>left</i>, следующий содержит значение состояния <i>right</i> и последний содержит значение состояние <i>allClicks</i>.

### Правила использования хуков

<!-- There are a few limitations and rules we have to follow to ensure that our application uses hooks-based state functions correctly. -->
Есть несколько ограничений и правил, которым мы должны следовать, чтобы гарантировать правильную работу приложения, используя состояния, основанные на хуках.

<!-- The _useState_ function (as well as the _useEffect_ function introduced later on in the course) <i>must not be called</i> from inside of a loop, a conditional expression, or any place that is not a function defining a component. This must be done to ensure that the hooks are always called in the same order, and if this isn't the case the application will behave erratically. -->
Функция _useState_ (а также функция _useEffect_, о которой мы расскажем позже) <i>не должна вызываться</i> внутри цикла, условного выражения или любого места, которое не является функцией, определяющей компонент. Это необходимо сделать, чтобы гарантировать порядок вызова хуков. Если мы не можем это сделать, то приложение будет вести себя беспорядочно.

<!-- To recap, hooks may only be called from the inside of a function body that defines a React component: -->
Напомним, хуки можно вызывать только внутри тела функции, которая определяет компонент React:

```js
const App = () => {
  // так можно
  const [age, setAge] = useState(0)
  const [name, setName] = useState('Juha Tauriainen')

  if ( age > 10 ) {
    // так работать не будет!
    const [foobar, setFoobar] = useState(null)
  }

  for ( let i = 0; i < age; i++ ) {
    // так тоже будет беда
    const [rightWay, setRightWay] = useState(false)
  }

  const notGood = () => {
    // и так тоже нельзя
    const [x, setX] = useState(-1000)
  }

  return (
    //...
  )
}
```

### Вернемся к обработке событий

<!-- Event handling has proven to be a difficult topic in previous iterations of this course. -->
В пройденной части курса обработка событий могла показаться достаточно сложной темой.

<!-- For this reason we will revisit the topic. -->
По этой причине мы повторим ее.

<!-- Let's assume that we're developing this simple application: -->
Предположим, что мы разрабатываем простое приложение:
```js
const App = () => {
  const [value, setValue] = useState(10)

  return (
    <div>
      {value}
      <button>reset</button>
    </div>
  )
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
)
```

<!-- We want the clicking of the button to reset the state stored in the _value_ variable. -->
Мы хотим, чтобы нажатие кнопки сбрасывало состояние, хранящееся в переменной _value_.

<!-- In order to make the button react to a click event, we have to add an <i>event handler</i> to it. -->
Чтобы кнопка реагировала на нажатие, мы должны добавить к ней <i>обработчик события</i>.

<!-- Event handlers must always be a function or a reference to a function. The button will not work if the event handler is set to a variable of any other type. -->
Обработчики события всегда должны быть функцией или ссылкой на функцию. Кнопка не будет работать, если в качестве обработчика событий задана переменная любого другого типа.

<!-- If we were to define the event handler as a string: -->
Если бы мы определили обработчик события как строку:

```js
<button onClick={'crap...'}>button</button>
```

<!-- React would warn us about this in the console: -->
React предупредил бы нас об этом в консоли:

```js
index.js:2178 Warning: Expected `onClick` listener to be a function, instead got a value of `string` type.
    in button (at index.js:20)
    in div (at index.js:18)
    in App (at index.js:27)
```

<!-- The following attempt would also not work: -->
Следующий вариант также не сработает:

```js
<button onClick={value + 1}>button</button>
```

<!-- We have attempted to set the event handler to _value + 1_ which simply returns the result of the operation. React will kindly warn us about this in the console: -->
Мы попытались установить для обработчика события значение _value + 1_, которое просто возвращает результат операции. React любезно предупредит нас об этом в консоли:

```js
index.js:2178 Warning: Expected `onClick` listener to be a function, instead got a value of `number` type.
```

И этот вариант тоже не сработает:

```js
<button onClick={value = 0}>button</button>
```

<!-- The event handler is not a function but a variable assignment, and React will once again issue a warning to the console. This attempt is also flawed in the sense that we must never mutate state directly in React. -->
Тут обработчик события - это не функция, а присваивание переменной значения. React снова выдаст предупреждение в консоль. Этот вариант также ошибочен с точки зрения того, что мы никогда не должны изменять состояние непосредственно в React (не используя setValue).

<!-- What about the following: -->
А как насчет следующего:

```js
<button onClick={console.log('clicked the button')}>
  button
</button>
```

<!-- The message gets printed to the console once but nothing happens when we click the button a second time. Why does this not work even when our event handler contains a function _console.log_? -->
Сообщение выводится на консоль один раз, но когда мы нажимаем кнопку второй раз, ничего не происходит. Почему вывод не сработал, ведь наш обработчик события содержит функцию _console.log_?

<!-- The issue here is that our event handler is defined as a <i>function call</i> which means that the event handler is actually assigned the returned value from the function, which in the case of _console.log_ is <i>undefined</i>. -->
Проблема здесь в том, что в данном варианте наш обработчик события определен как <i>вызов функции</i>. Фактически обработичку события назначается возвращаемое значение вызванной функции, которое в случае _console.log_ равно <i>undefined</i>.

<!-- The _console.log_ function call gets executed when the component is rendered and for this reason it gets printed once to the console. -->
Вызов функции _console.log_ выполняется во время рендеринга компонента, и по этой причине текст всего один раз выводится на консоль.

<!-- The following attempt is flawed as well: -->
Следующий вариант также ошибочен:

```js
<button onClick={setValue(0)}>button</button>
```

<!-- We have once again tried to set a function call as the event handler. This does not work. This particular attempt also causes another problem. When the component is rendered the function _setValue(0)_ gets executed which in turn causes the component to be re-rendered. Re-rendering in turn calls _setValue(0)_ again, resulting in an infinite recursion. -->
Тут мы еще раз попытались установить вызов функции в качестве обработчика события. Так не работает. Этот конкретный вариант также приводит к другой проблеме. Когда компонент отрисовывается первый раз, выполняется функция _setValue(0)_, которая, в свою очередь, вызывает повторную отрисовку компонента. А повторный рендеринг снова вызывает _setValue(0)_, что приводит к бесконечной рекурсии.

<!-- Executing a particular function call when the button is clicked can be accomplished like this: -->
Выполнение вызова конкретной функции при нажатии кнопки может быть выполнено следующим образом:

```js
<button onClick={() => console.log('clicked the button')}>
  button
</button>
```

<!-- Now the event handler is a function defined with the arrow function syntax _() => console.log('clicked the button')_. When the component gets rendered, no function gets called and only the reference to the arrow function is set to the event handler. Calling the function happens only once the button is clicked. -->
В данном примере обработчик события - это функция, определенная с помощью синтаксиса стрелочной функции _() => console.log('clicked the button')_. Когда компонент отрисовывается, функция не вызывается, так как мы установили ссылку на стрелочную функцию. Вызов функции происходит только после нажатия кнопки.

<!-- We can implement resetting the state in our application with this same technique: -->
Мы можем реализовать сброс состояния в нашем приложении с помощью этого подхода:

```js
<button onClick={() => setValue(0)}>button</button>
```

<!-- The event handler is now the function _() => setValue(0)_. -->
Обработчиком события теперь является функция _() => setValue(0)_.

<!-- Defining event handlers directly in the attribute of the button is not necessarily the best possible idea. -->
Определение обработчика события непосредственно в атрибуте кнопки - не лучшая идея.

<!-- You will often see event handlers defined in a separate place. In the following version of our application we define a function that then gets assigned to the _handleClick_ variable in the body of the component function: -->
Вы часто будете видеть обработчики события, определенные в отдельном месте. В следующей версии нашего приложения мы создаем функцию, которая присваивается переменной _handleClick_ в теле функции компонента:

```js
const App = () => {
  const [value, setValue] = useState(10)

  const handleClick = () =>
    console.log('clicked the button')

  return (
    <div>
      {value}
      <button onClick={handleClick}>button</button>
    </div>
  )
}
```

<!-- The _handleClick_ variable is now assigned to a reference to the function. The reference is passed to the button as the <i>onClick</i> attribute: -->
Переменная _handleClick_ теперь является ссылкой на функцию. Эта переменная передается в кнопку с помощью атрибута <i>onClick</i>:

```js
<button onClick={handleClick}>button</button>
```

<!-- Naturally, our event handler function can be composed of multiple commands. In these cases we use the longer curly brace syntax for arrow functions: -->
Естественно, наша функция-обработчик события может состоять из нескольких команд. В этом случае мы используем более длинный синтаксис и объявляем стрелочную функцию с фигурными скобоками:

```js
const App = () => {
  const [value, setValue] = useState(10)

  // highlight-start
  const handleClick = () => {
    console.log('clicked the button')
    setValue(0)
  }
   // highlight-end

  return (
    <div>
      {value}
      <button onClick={handleClick}>button</button>
    </div>
  )
}
```

### Функция, возвращающая функцию

<!-- Another way to define an event handler is to use <i>function that returns a function</i>. -->
Другой способ определить обработчик события - использовать <i>функцию, которая возвращает функцию</i>.

<!-- You probably won't need to use functions that return functions in any of the exercises in this course.  If the topic seems particularly confusing, you may skip over this section for now and return to it later. -->
Вероятно, вам не нужно будет использовать функции, возвращающие функции, в заданиях этого курса. Если эта тема кажется запутанной, вы можете пропустить этот раздел сейчас и вернуться к нему позже.

<!-- Let's make the following changes to our code: -->
Внесем в наш код следующие изменения:

```js
const App = () => {
  const [value, setValue] = useState(10)

  // highlight-start
  const hello = () => {
    const handler = () => console.log('hello world')

    return handler
  }
  // highlight-end

  return (
    <div>
      {value}
      <button onClick={hello()}>button</button>
    </div>
  )
}
```

<!-- The code functions correctly even though it looks complicated. -->
Код работает правильно, хотя выглядит сложным.

<!-- The event handler is now set to a function call: -->
Теперь в атрибуте onClick происходит вызов функции:

```js
<button onClick={hello()}>button</button>
```

<!-- Earlier on we stated that an event handler may not be a call to a function, and that it has to be a function or a reference to a function. Why then does a function call work in this case? -->
Ранее мы заявили, что обработчик события не может быть вызовом функции, а должен быть функцией или ссылкой на функцию. Почему же тогда в этом случае все работает?

<!-- When the component is rendered, the following function gets executed: -->
Когда компонент отрисовывается, выполняется следующая функция:

```js
const hello = () => {
  const handler = () => console.log('hello world')

  return handler
}
```

<!-- The <i>return value</i> of the function is another function that is assigned to the _handler_ variable. -->
<i>Возвращаемое значение</i> этой функции - это другая функция, которая присваивается переменной _handler_.

<!-- When React renders the line: -->
Когда React отображает строку:

```js
<button onClick={hello()}>button</button>
```

<!-- It assigns the return value of _hello()_ to the onClick attribute. Essentially the line gets transformed into: -->
Он присваивает возвращаемое значение _hello()_ атрибуту onClick. По сути, строка трансформируется в:

```js
<button onClick={() => console.log('hello world')}>
  button
</button>
```

<!-- Since the _hello_ function returns a function, the event handler is now a function. -->
Поскольку функция _hello_ возвращает функцию, обработчик события теперь является функцией.

<!-- What's the point of this concept? -->
В чем смысл этой концепции?

<!-- Let's change the code a tiny bit: -->
Немного изменим код:

```js
const App = () => {
  const [value, setValue] = useState(10)

  // highlight-start
  const hello = (who) => {
    const handler = () => {
      console.log('hello', who)
    }

    return handler
  }
  // highlight-end

  return (
    <div>
      {value}
  // highlight-start
      <button onClick={hello('world')}>button</button>
      <button onClick={hello('react')}>button</button>
      <button onClick={hello('function')}>button</button>
  // highlight-end
    </div>
  )
}
```

<!-- Now the application has three buttons with event handlers defined by the _hello_ function that accepts a parameter. -->
Теперь в приложении есть три кнопки с обработчиками событий, которые вернула функция _hello_, вызванная с разыми параметрами.

<!-- The first button is defined as -->
onClick первой кнопки задается как:

```js
<button onClick={hello('world')}>button</button>
```

<!-- The event handler is created by <i>executing</i> the function call _hello('world')_. The function call returns the function: -->
Этот обработчик события создается путем <i>выполнения</i> вызова функции _hello('world')_. Сам вызов функции возвращает новую функцию:

```js
() => {
  console.log('hello', 'world')
}
```

onClick для второй кнопки определяется как:

```js
<button onClick={hello('react')}>button</button>
```

<!-- The function call _hello('react')_ that creates the event handler returns: -->
Вызов функции _hello('react')_, которая создает обработчик события, возвращает:

```js
() => {
  console.log('hello', 'react')
}
```

<!-- Both buttons get their own individualized event handlers. -->
Обе кнопки имеют собственные индивидуализированные обработчики события.

<!-- Functions returning functions can be utilized in defining generic functionality that can be customized with parameters. The _hello_ function that creates the event handlers can be thought of as a factory that produces customized event handlers meant for greeting users. -->
Функции, возвращающие функции, могут использоваться для определения общих функций, настраиваемых с помощью параметров. Функцию _hello_ можно рассматривать как фабрику, которая создает настраиваемые обработчики события, предназначенные для приветствия пользователей.

<!-- Our current definition is slightly verbose: -->
Наше текущее определение достаточно многословно:

```js
const hello = (who) => {
  const handler = () => {
    console.log('hello', who)
  }

  return handler
}
```

<!-- Let's eliminate the helper variables and directly return the created function: -->
Давайте удалим вспомогательную переменную и напрямую вернем созданную функцию:

```js
const hello = (who) => {
  return () => {
    console.log('hello', who)
  }
}
```

<!-- Since our _hello_ function is composed of a single return command, we can omit the curly braces and use the more compact syntax for arrow functions: -->
Поскольку наша функция _hello_ содержит всего одну команду return, мы можем опустить фигурные скобки и использовать более компактный синтаксис стрелочных функций:

```js
const hello = (who) =>
  () => {
    console.log('hello', who)
  }
```

<!-- Lastly, let's write all of the arrows on the same line: -->
Наконец, давайте расположим все стрелки в одной строке:

```js
const hello = (who) => () => {
  console.log('hello', who)
}
```

<!-- We can use the same trick to define event handlers that set the state of the component to a given value. Let's make the following changes to our code: -->
Мы можем использовать тот же трюк, чтобы создавать обработчики события, устанавливающие состояние компонента в заданное значение. Внесем в наш код следующие изменения:

```js
const App = () => {
  const [value, setValue] = useState(10)

  // highlight-start
  const setToValue = (newValue) => () => {
    setValue(newValue)
  }
  // highlight-end

  return (
    <div>
      {value}
      // highlight-start
      <button onClick={setToValue(1000)}>1000</button>
      <button onClick={setToValue(0)}>reset</button>
      <button onClick={setToValue(value + 1)}>increment</button>
      // highlight-end
    </div>
  )
}
```

<!-- When the component is rendered, the <i>thousand</i> button is created: -->
Когда компонент отрисуется, создастся кнопка <i>1000</i>:

```js
<button onClick={setToValue(1000)}>1000</button>
```

<!-- The event handler is set to the return value of _setToValue(1000)_ which is the following function: -->
Обработчиком события становится функция, возвращаемая вызовом _setToValue(1000)_:

```js
() => {
  setValue(1000)
}
```

Кнопка увеличения объявлена следующим образом:

```js
<button onClick={setToValue(value + 1)}>increment</button>
```

<!-- The event handler is created by the function call _setToValue(value + 1)_ which receives as its parameter the current value of the state variable _value_ increased by one. If the value of _value_ was 10, then the created event handler would be the function: -->
Обработчиком события становится функция, возвращаемая вызовом _setToValue(value + 1)_. _setToValue(value + 1)_ получает в качестве параметра текущее значение переменной состояния _value_, увеличенное на единицу. Если бы значение _value_ было 10, то созданный обработчик события был бы функцией:

```js
() => {
  setValue(11)
}
```

<!-- Using functions that return functions is not required to achieve this functionality. Let's return the _setToValue_ function that is responsible for updating state, into a normal function: -->
Использование функций, возвращающих функции, не является обязательным для достижения такого поведения. Вернем функцию _setToValue_, отвечающую за обновление состояния, к нормальной форме:

```js
const App = () => {
  const [value, setValue] = useState(10)

  const setToValue = (newValue) => {
    setValue(newValue)
  }

  return (
    <div>
      {value}
      <button onClick={() => setToValue(1000)}>
        1000
      </button>
      <button onClick={() => setToValue(0)}>
        reset
      </button>
      <button onClick={() => setToValue(value + 1)}>
        increment
      </button>
    </div>
  )
}
```

<!-- We can now define the event handler as a function that calls the _setToValue_ function with an appropriate parameter. The event handler for resetting the application state would be: -->
Теперь мы можем определить обработчик события как функцию, которая вызывает функцию _setToValue_ с соответствующим параметром. Обработчик события для сброса состояния до нуля:

```js
<button onClick={() => setToValue(0)}>reset</button>
```

<!-- Choosing between the two presented ways of defining your event handlers is mostly a matter of taste. -->
Выбор между двумя представленными способами определения обработчиков событий - это в основном дело вкуса.

### Передача обработчиков события дочерним компонентам

<!-- Let's extract the button into its own component: -->
Давайте выделим кнопку в отдельный компонент:

```js
const Button = (props) => (
  <button onClick={props.handleClick}>
    {props.text}
  </button>
)
```

<!-- The component gets the event handler function from the _handleClick_ prop, and the text of the button from the _text_ prop. -->
Этот компонент получает функцию обработчик события с помощью свойства _handleClick_, а текст кнопки - с помощью свойства _text_.

<!-- Using the <i>Button</i> component is simple, although we have to make sure that we use the correct attribute names when passing props to the component. -->
Использование компоненты <i>Button</i> просто, хотя мы должны убедиться в том, что используем правильные имена атрибутов при передаче свойств компоненту.

![](../../images/1/12e.png)

### Не определяйте компоненты внутри компонентов

<!-- Let's start displaying the value of the application into its own <i>Display</i> component.
We will change the application by defining a new component inside of the <i>App</i>-component. -->
Давайте отобразим значение счетчика в компоненте <i>Display</i>, определенной в <i>App</i>.


```js
// This is the right place to define a component
const Button = (props) => (
  <button onClick={props.handleClick}>
    {props.text}
  </button>
)

const App = () => {
  const [value, setValue] = useState(10)

  const setToValue = newValue => {
    setValue(newValue)
  }

  // Не определяйте компоненты внутри компонентов
  const Display = props => <div>{props.value}</div> // highlight-line

  return (
    <div>
      <Display value={value} />
      <Button handleClick={() => setToValue(1000)} text="thousand" />
      <Button handleClick={() => setToValue(0)} text="reset" />
      <Button handleClick={() => setToValue(value + 1)} text="increment" />
    </div>
  )
}
```

<!-- The application still appears to work, but **don't implement components like this!** Never define components inside of other components. The method provides no benefits and leads to many unpleasant problems. The biggest problems are due to the fact that React treats a component defined inside of another component as a new component in every render. This makes it impossible for React to optimize the component. -->
Приложение по-прежнему работает, но **не создавайте подобные компоненты!** Никогда не определяйте компоненты внутри других компонентов. Такой способ определения не приносит никакой пользы и приводит к множеству неприятных проблем. Самые большие проблемы связаны с тем, что React создает этот компонент при каждом рендеринге. Это лишает React возможности оптимизировать этот компонент.

<!-- Let's instead move the <i>Display</i> component function to its correct place, which is outside of the <i>App</i> component function: -->
Вместо этого давайте переместим функцию компонента <i>Display</a> в правильное место, которое находится за пределами функции компонента <i>App</i>:

```js
const Display = props => <div>{props.value}</div>

const Button = (props) => (
  <button onClick={props.handleClick}>
    {props.text}
  </button>
)

const App = () => {
  const [value, setValue] = useState(10)

  const setToValue = newValue => {
    setValue(newValue)
  }

  return (
    <div>
      <Display value={value} />
      <Button handleClick={() => setToValue(1000)} text="thousand" />
      <Button handleClick={() => setToValue(0)} text="reset" />
      <Button handleClick={() => setToValue(value + 1)} text="increment" />
    </div>
  )
}
```

### Полезные материалы

<!-- The internet is full of React-related material. However, we use such a new style of React that a large majority of the material found online is outdated for our purposes. -->
В Интернете полно материалов, связанных с React. Однако мы используем фичи последней версии React и с большой веорятностью часть материалов, найденных в Интернете, устарела.

<!-- You may find the following links useful: -->
Но вам могут быть полезны следующие ссылки:

<!-- - The React [official documentation](https://reactjs.org/docs/hello-world.html) is worth checking out at some point, although most of it will become relevant only later on in the course. Also, everything related to class-based components is irrelevant to us;
- Some courses on [Egghead.io](https://egghead.io) like [Start learning React](https://egghead.io/courses/start-learning-react) are of high quality, and recently updated [The Beginner's Guide to React](https://egghead.io/courses/the-beginner-s-guide-to-reactjs) is also relatively good; both courses introduce concepts that will also be introduced later on in this course. **NB** The first one uses class components but the latter uses the new functional ones. -->

- [Официальная документация React](https://ru.reactjs.org/docs/hello-world.html) стоит прочтения, хотя большая ее часть станет актуальной только позже в курсе. Кроме того, все, что связано с созданием компонентов на основе классов, можно пропустить;
- Много качественных курсов вы можете найти на [Egghead.io](https://egghead.io), например, [Start learning React](https://egghead.io/courses/start-learning-react) или недавно обновленный [The Beginner's Guide to React](https://egghead.io/courses/the-beginner-s-guide-to-reactjs); оба курса знакомят с концепциями, которые будут представлены позже в этом курсе. **Обратите внимание:** первый курс использует классы для создания компонент, а второй использует функциональные компоненты.

</div>

<div class="tasks">
  <h3>Упражнение  1.6.-1.14.</h3>

Упражнения отправляются через GitHub и помечаются как выполненные в [системе сдачи упражнений](https://studies.cs.helsinki.fi/stats/courses/fullstackopen).

<!-- Remember, submit **all** the exercises of one part **in a single submission**. Once you have submitted your solutions for one part, **you cannot submit more exercises to that part any more**. -->
Помните, что нужно отправить **все** упражнения данной части **за один раз**. После того, как вы отправили свои решения упражнений данной части, **вы больше не можете отправлять упражнения для этой части**.

<!-- <i>Some of the exercises work on the same application. In these cases, it is sufficient to submit just the final version of the application. If you wish, you can make a commit after every finished exercise, but it is not mandatory.</i> -->
<i>Некоторые упражнения выполняются в одном приложении. В этом случае достаточно отправить финальную версию приложения. По желанию вы можете делать коммит после каждого завершенного упражнения, но это не обязательно.</i>

<!-- **WARNING** create-react-app will automatically turn your project into a git-repository unless you create your application inside of an existing git repository. **Most likely you do not want each of your projects to be a separate repository**, so simply run the _rm -rf .git_ command at the root of your application. -->
**ПРЕДУПРЕЖДЕНИЕ** create-react-app автоматически превратит ваш проект в репозиторий git, если вы не создадите приложение внутри уже существующего репозитория git. **Скорее всего, вы не хотите, чтобы каждый из ваших проектов был отдельным репозиторием**, поэтому просто запустите команду _rm -rf .git_ в корне вашего приложения.

<!-- In some situations you may also have to run the command below from the root of the project: -->
В некоторых ситуациях вам может потребоваться выполнить приведенную ниже команду из корня проекта:

```
rm -rf node_modules/ && npm i
```

  <h4> 1.6: unicafe шаг1</h4>

<!-- Like most companies, [Unicafe](https://www.unicafe.fi/#/9/4) collects feedback from its customers. Your task is to implement a web application for collecting customer feedback. There are only three options for feedback: <i>good</i>, <i>neutral</i>, and <i>bad</i>. -->
Как и большинство компаний, [Unicafe](https://www.unicafe.fi/#/9/4) собирает отзывы своих клиентов. Ваша задача - реализовать веб-приложение для сбора отзывов клиентов. Есть только три варианта отзыва: <i>good</i>, <i>neutral</i> и <i>bad</i>.

<!-- The application must display the total number of collected feedback for each category. Your final application could look like this: -->
Приложение должно отображать общее количество собранных отзывов по каждой категории. Ваше окончательное приложение может выглядеть следующим образом:

![](../../images/1/13e.png)

<!-- Note that your application needs to work only during a single browser session. Once you refresh the page, the collected feedback is allowed to disappear. -->
Обратите внимание, что ваше приложение должно работать в течении одной сессии браузера. После обновления страницы собранные отзывы могут исчезнуть.

<!-- You can implement the application in a single <i>index.js</i> file. You can use the code below as a starting point for your application. -->
Вы можете реализовать приложение в одном файле <i>index.js</i>. Вы можете использовать приведенный ниже код в качестве отправной точки для своего приложения.

```js
import React, { useState } from 'react'
import ReactDOM from 'react-dom'

const App = () => {
  // сохраняем нажатие каждой кнопки в отдельном состоянии
  const [good, setGood] = useState(0)
  const [neutral, setNeutral] = useState(0)
  const [bad, setBad] = useState(0)

  return (
    <div>
      код тут
    </div>
  )
}

ReactDOM.render(<App />,
  document.getElementById('root')
)
```

<h4>1.7: unicafe шаг2</h4>

<!-- Expand your application so that it shows more statistics about the gathered feedback: the total number of collected feedback, the average score (good: 1, neutral: 0, bad: -1) and the percentage of positive feedback. -->
Расширьте приложение, чтобы оно показывало больше статистики о собранных отзывах: общее количество собранных отзывов, средний балл (хорошо: 1, нейтральный: 0, плохо: -1) и процент положительных отзывов.

![](../../images/1/14e.png)

<h4>1.8: unicafe шаг3</h4>

<!-- Refactor your application so that displaying the statistics is extracted into its own <i>Statistics</i> component. The state of the application should remain in the <i>App</i> root component. -->
Измените ваше приложение так, чтобы отображаемая статистика хранилась в отдельном компоненте <i>Statistics</i>. Состояние приложения должно оставаться в корневом компоненте <i>App</i>.

<!-- Remember that components should not be defined inside other components: -->
Помните, что компоненты не должны определяться внутри других компонентов:

```js
// подходящее место для определения компонента
const Statistics = (props) => {
  // ...
}

const App = () => {
  const [good, setGood] = useState(0)
  const [neutral, setNeutral] = useState(0)
  const [bad, setBad] = useState(0)

  // не определяйте компонент в другом компоненте
  const Statistics = (props) => {
    // ...
  }

  return (
    // ...
  )
}
```

<h4>1.9: unicafe шаг4</h4>

<!-- Change your application to display statistics only once feedback has been gathered. -->
Измените свое приложение так, чтобы статистика отображалась только после сбора отзыва.

![](../../images/1/15e.png)

<h4>1.10: unicafe шаг5</h4>

<!-- Let's continue refactoring the application. Extract the following two components: -->
Продолжим рефакторинг приложения. Извлеките следующие два компонента:

<!-- - <i>Button</i> for defining the buttons used for submitting feedback
- <i>Statistic</i> for displaying a single statistic, e.g. the average score. -->
- <i>Button</i> - создает кнопки, используемые для отправки отзыва;
- <i>Statistic</i> - отображает отдельную статистику, например средний балл.

<!-- To be clear: the <i>Statistic</i> component always displays a single statistic, meaning that the application uses multiple components for rendering all of the statistics: -->
Для ясности: компонент <i>Statistic</i> всегда отображает одну часть статистики. Это значит, что приложение использует несколько компонентов для визуализации всей статистики:

```js
const Statistics = (props) => {
  /// ...
  return(
    <div>
      <Statistic text="good" value ={...} />
      <Statistic text="neutral" value ={...} />
      <Statistic text="bad" value ={...} />
      // ...
    </div>
  )
}

```

<!-- The application's state should still be kept in the root <i>App</i> component. -->
Состояние приложения по-прежнему должно храниться в корневом компоненте <i>App</i>.

<h4>1.11*: unicafe шаг6</h4>

<!-- Display the statistics in an HTML [table](https://developer.mozilla.org/en-US/docs/Learn/HTML/Tables/Basics), so that your application looks roughly like this: -->
Отобразите статистику в [table](https://developer.mozilla.org/ru/docs/Learn/HTML/Tables/Basics), чтобы ваше приложение выглядело примерно так:

![](../../images/1/16e.png)

<!-- Remember to keep your console open at all times. If you see this warning in your console: -->
Не забывайте всегда держать консоль открытой. Если вы видите это предупреждение в консоли:

![](../../images/1/17a.png)

<!-- Then perform the necessary actions to make the warning disappear. Try Googling the error message if you get stuck. -->
Тогда выполните необходимые действия, чтобы предупреждение исчезло. Если вы застряли, попробуйте поискать в Google информацию об ошибке.

<!-- <i>Typical source of an error `Unchecked runtime.lastError: Could not establish connection. Receiving end does not exist.` is Chrome extension. Try going to `chrome://extensions/` and try disabling them one by one and refreshing React app page; the error should eventually disappear.</i> -->
<i>Типичная причина ошибки `Unchecked runtime.lastError: Could not establish connection. Receiving end does not exist.` - это расширение Chrome. Перейдите в `chrome://extensions/` и попробуйте отключить расширения одно за другим. После обновите страницу приложения React; со временем ошибка должна исчезнуть.</i>

<!-- **Make sure that from now on you don't see any warnings in your console!** -->
**Убедитесь, что на текущий момент вы не видите никаких предупреждений в консоли!**

<h4>1.12*: анекдоты шаг1</h4>

<!-- The world of software engineering is filled with [anecdotes](http://www.comp.nus.edu.sg/~damithch/pages/SE-quotes.htm) that distill timeless truths from our field into short one-liners. -->
Мир программной инженерии наполнен [анекдотами](http://www.comp.nus.edu.sg/~damithch/pages/SE-quotes.htm) - короткими однострочниками, содержащими вневременные истины нашей области.

<!-- Expand the following application by adding a button that can be clicked to display a <i>random</i> anecdote from the field of software engineering: -->
Создайте следующее приложение, добавив кнопку, при нажатии на которую можно отобразить <i>случайный</i> анекдот из области разработки программного обеспечения:

```js
import React, { useState } from 'react'
import ReactDOM from 'react-dom'

const App = (props) => {
  const [selected, setSelected] = useState(0)

  return (
    <div>
      {props.anecdotes[selected]}
    </div>
  )
}

const anecdotes = [
  'If it hurts, do it more often',
  'Adding manpower to a late software project makes it later!',
  'The first 90 percent of the code accounts for the first 90 percent of the development time...The remaining 10 percent of the code accounts for the other 90 percent of the development time.',
  'Any fool can write code that a computer can understand. Good programmers write code that humans can understand.',
  'Premature optimization is the root of all evil.',
  'Debugging is twice as hard as writing the code in the first place. Therefore, if you write the code as cleverly as possible, you are, by definition, not smart enough to debug it.'
]

ReactDOM.render(
  <App anecdotes={anecdotes} />,
  document.getElementById('root')
)
```

<!-- Google will tell you how to generate random numbers in JavaScript. Remember that you can test generating random numbers e.g. straight in the console of your browser. -->
Google расскажет вам, как генерировать случайные числа в JavaScript. Помните, что вы можете протестировать генерацию случайных чисел в консоли вашего браузера.

<!-- Your finished application could look something like this: -->
Готовое приложение может выглядеть примерно так:

![](../../images/1/18a.png)

<!-- **WARNING** create-react-app will automatically turn your project into a git-repository unless you create your application inside of an existing git repository. **Most likely you do not want each of your projects to be a separate repository**, so simply run the _rm -rf .git_ command at the root of your application. -->
**ПРЕДУПРЕЖДЕНИЕ** create-react-app автоматически превратит ваш проект в репозиторий git, если вы не создадите приложение внутри уже существующего репозитория git. **Скорее всего, вы не хотите, чтобы каждый из ваших проектов был отдельным репозиторием**, поэтому просто запустите команду _rm -rf .git_ в корне вашего приложения.

<h4>1.13*: анекдоты шаг2</h4>

<!-- Expand your application so that you can vote for the displayed anecdote. -->
Добавьте функционал, с помощью которого вы можете проголосовать за показанный анекдот.

![](../../images/1/19a.png)

<!-- **NB** store the votes of each anecdote into an array or object in the component's state. Remember that the correct way of updating state stored in complex data structures like objects and arrays is to make a copy of the state. -->
**Обратите внимание:** храните голоса каждого анекдота в массиве или объекте в состоянии компонента. Помните, что правильный способ обновления состояния, хранящегося в сложных структурах данных, таких как объекты и массивы, - это сделать копию состояния.

<!-- You can create a copy of an object like this: -->
Вы можете создать копию объекта следующим образом:

```js
const points = { 0: 1, 1: 3, 2: 4, 3: 2 }

const copy = { ...points }
// увеличить значение свойства 2 на единицу
copy[2] += 1
```

<!-- OR a copy of an array like this: -->
ИЛИ копию массива:

```js
const points = [1, 4, 6, 3]

const copy = [...points]
// увеличить значение индекса 2 на единицу
copy[2] += 1
```

<!-- Using an array might be the simpler choice in this case. Googling will provide you with lots of hints on how to create a zero-filled array of a desired length, like [this](https://stackoverflow.com/questions/20222501/how-to-create-a-zero-filled-javascript-array-of-arbitrary-length/22209781). -->
В нашем случае проще использовать массив. Простой гуглинг предоставит вам множество подсказок о том, как создать массив заполненный нулями и желаемой длины. Например, [вы можете почитать эту статью](https://stackoverflow.com/questions/20222501/how-to-create-a-zero-filled-javascript-array-of-arbitrary-length/22209781).

<h4>1.14*: анекдоты шаг3</h4>

<!-- Now implement the final version of the application that displays the anecdote with the largest number of votes: -->
Теперь реализуем финальную версию приложения, которая отображает анекдот с наибольшим количеством голосов:

![](../../images/1/20a.png)

<!-- If multiple anecdotes are tied for first place it is sufficient to just show one of them. -->
Если на первом месте несколько анекдотов, достаточно просто показать один из них.

<!-- This was the last exercise for this part of the course and it's time to push your code to GitHub and mark all of your finished exercises to the [exercise submission system](https://studies.cs.helsinki.fi/stats/courses/fullstackopen). -->
Это последнее упражнение в данной части курса! Пришло время разместить ваш код на GitHub и отметить все завершенные упражнения в [системе сдачи упражнений](https://studies.cs.helsinki.fi/stats/courses/fullstackopen).

</div>
