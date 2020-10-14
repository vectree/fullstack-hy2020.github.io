---
mainImage: ../../../images/part-1.svg
part: 1
letter: c
lang: ru
---

<div class="content">

<!-- Let's go back to working with React. -->
<!--  -->
<!-- We start with a new example: -->

Вернемся к работе с React.

Начнем с нового примера:

```js
const Hello = (props) => {
  return (
    <div>
      <p>
        Привет {props.name}, ваш возраст: {props.age}
      </p>
    </div>
  )
}

const App = () => {
  const name = 'Петя'
  const age = 10

  return (
    <div>
      <h1>Приветствуем</h1>
      <Hello name="Майя" age={26 + 10} />
      <Hello name={name} age={age} />
    </div>
  )
}
```

<!-- ### Component helper functions -->
### Вспомогательные функции компонент

<!-- Let's expand our <i>Hello</i> component so that it guesses the year of birth of the person being greeted: -->

Давайте расширим наш компонент <i>Hello</i>, чтобы он предполагал год рождения человека, которого мы приветствуем:

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
      <p>Привет {props.name}, ваш возраст: {props.age}</p>
      <p>И, вероятно, вы родились в {bornYear()}</p> // highlight-line
    </div>
  )
}
```

<!-- The logic for guessing the year of birth is separated into its own function that is called when the component is rendered. -->

Логика угадывания года рождения выделена в отдельную функцию, которая вызывается при отрисовке компонента.

<!-- The person's age does not have to be passed as a parameter to the function, since it can directly access all props that are passed to the component. -->

Возраст человека не нужно передавать в качестве параметра функции, поскольку она может напрямую обращаться к props - всем свойствам, которые передаются в компонент.

<!-- If we examine our current code closely, we'll notice that the helper function is actually defined inside of another function that defines the behavior of our component. In Java programming, defining a function inside another one is complex and cumbersome, so not all that common. In JavaScript, however, defining functions within functions is a commonly-used technique. -->

Если мы внимательно изучим наш текущий код, мы заметим, что вспомогательная функция фактически определена внутри другой функции, определяющей поведение нашего компонента. В языке программирования Java определение функции внутри другой функции достаточно сложно и громоздко, поэтому не все так просто. В JavaScript, наоборот, часто используется определение функций внутри функций.

### Деструктуризация

<!-- Before we move forward, we will take a look at a small but useful feature of the JavaScript language that was added in the ES6 specification, that allows us to [destructure](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) values from objects and arrays upon assignment. -->

Прежде чем двигаться дальше, мы рассмотрим небольшую, но полезную функцию языка программирования JavaScript, которая была добавлена в спецификацию ES6. Она позволяет нам [деструктурировать](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) значения из объектов и массивов при присваивании.

<!-- In our previous code, we had to reference the data passed to our component as _props.name_ and _props.age_. Of these two expressions we had to repeat _props.age_ twice in our code. -->

В приведенном примере мы ссылались на данные (_props.name_ и _props.age_), переданные нашему компоненту. И дважды нам пришлось повторить выражение _props.age_ в коде.

<!-- Since <i>props</i> is an object -->

Поскольку <i>props</i> является объектом

```js
props = {
  name: 'Арто Хеллас',
  age: 35,
}
```

<!-- we can streamline our component by assigning the values of the properties directly into two variables _name_ and _age_ which we can then use in our code: -->

мы можем упростить наш компонент, присвоив значения свойств непосредственно двум переменным _name_ и _age_ и затем использовать их в нашем коде:

```js
const Hello = (props) => {
  // highlight-start
  const name = props.name
  const age = props.age
  // highlight-end

  const bornYear = () => new Date().getFullYear() - age

  return (
    <div>
      <p>Привет {name}, ваш возраст: {age}</p>
      <p>И, вероятно, вы родились в {bornYear()}</p>
    </div>
  )
}
```

<!-- Note that we've also utilized the more compact syntax for arrow functions when defining the _bornYear_ function. As mentioned earlier, if an arrow function consists of a single expression, then the function body does not need to be written inside of curly braces. In this more compact form, the function simply returns the result of the single expression. -->

Обратите внимание, что мы использовали более компактный синтаксис при определении функции _bornYear_ - стрелочные функции. Как упоминалось ранее, если стрелочная функция состоит из одного выражения, то тело функции не нужно записывать внутри фигурных скобок. В этой более компактной форме функция просто возвращает результат одного выражения.

<!-- To recap, the two function definitions shown below are equivalent: -->

Напомним, что приведенные ниже определения двух функций эквивалентны:

```js
const bornYear = () => new Date().getFullYear() - age

const bornYear = () => {
  return new Date().getFullYear() - age
}
```

<!-- Destructuring makes the assignment of variables even easier, since we can use it to extract and gather the values of an object's properties into separate variables: -->

Деструктуризация упрошает присваивание переменных, поскольку мы можем использовать ее для извлечения значений свойств объекта в отдельные переменные:

```js
const Hello = (props) => {
    // highlight-start
  const { name, age } = props
    // highlight-end
  const bornYear = () => new Date().getFullYear() - age

  return (
    <div>
      <p>Привет {name}, ваш возраст: {age}</p>
      <p>И, вероятно, вы родились в {bornYear()}</p>
    </div>
  )
}
```

<!-- Eli koska -->
<!-- If the object we are destructuring has the values -->

Если объект, который мы деструктурируем, имеет значение

```js
props = {
  name: 'Арто Хеллас',
  age: 35,
}
```

<!-- the expression <em>const { name, age } = props</em> assigns the values 'Arto Hellas' to _name_ and 35 to _age_. -->

выражение <em>const { name, age } = props</em> присвоит значение 'Арто Хеллас' переменной _name_ и 35 переменной _age_.

<!-- We can take destructuring a step further: -->

Мы можем пойти дальше:

```js
const Hello = ({ name, age }) => { // highlight-line
  const bornYear = () => new Date().getFullYear() - age

  return (
    <div>
      <p>Привет {name}, ваш возраст: {age}</p>
      <p>И, вероятно, вы родились в {bornYear()}</p>
    </div>
  )
}
```

<!-- The props that are passed to the component are now directly destructured into the variables _name_ and _age_. -->

Props, которые передаются в компонент, теперь напрямую деструктурируются в переменные _name_ и _age_.

<!-- This means that instead of assigning the entire props object into a variable called <i>props</i> and then assigning its properties into the variables _name_ and _age_ -->

Это означает, что вместо параметра <i>props</i>, внутри которого хранились бы все свойства компонента и вместо последующего присвоения этих свойств переменным _name_ и _age_

```js
const Hello = (props) => {
  const { name, age } = props
```

<!-- we assign the values of the properties directly to variables by destructuring the props object that is passed to the component function as a parameter: -->

мы присваиваем значения свойств непосредственно переменным, деструктурируя объект props, который передается функции компонента в качестве параметра:

```js
const Hello = ({ name, age }) => {
```

### Ререндеринг страницы

<!-- So far all of our applications have been such that their appearance remains the same after the initial rendering. What if we wanted to create a counter where the value increased as a function of time or at the click of a button? -->

До сих пор все наши приложения не изменяли свой внешний вид после первоначального рендеринга. А что, если бы мы захотели создать счетчик, значение которого увеличивалось бы в зависимости от времени или при нажатии кнопки?

<!-- Let's start with the following: -->

Начнем со следующего кода:

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

<!-- The App component is given the value of the counter via the _counter_ prop. This component renders the value to the screen. What happens when the value of _counter_ changes? Even if we were to add the following -->

Компоненте App передается значение счетчика с помощью свойства _counter_. Этот компонент отображает значение на экране. Что произойдет, если значение _counter_ изменится? Например, если мы добавим следующий код:

```js
counter += 1
```

the component won't re-render. We can get the component to re-render by calling the _ReactDOM.render_ method a second time, e.g. in the following way:

Перерисовка компонента не произойдет. Мы можем заставить компонент повторно отрисоваться, вызвав метод _ReactDOM.render_ второй раз, например следующим образом:

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

<!-- The re-rendering command has been wrapped inside of the _refresh_ function to cut down on the amount of copy-pasted code. -->

Команда ререндеринга была заключена в функцию _refresh_, чтобы сократить количество одинакового кода.

<!-- Now the component  <i>renders three times</i>, first with the value 1, then 2, and finally 3. However, the values 1 and 2 are displayed on the screen for such a short amount of time that they can't be noticed. -->

Теперь компонент <i>отрисовывается трижды</i>, сначала со значением 1, затем 2 и, наконец, со значением 3. Однако значения 1 и 2 отображаются на экране в течение такого короткого промежутка времени, что их сложно заметить.

<!-- We can implement slightly more interesting functionality by re-rendering and incrementing the counter every second by using [setInterval](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setInterval): -->

Мы можем реализовать более интересный функционал, перерисовывая компонент и увеличивая счетчик каждую секунду с помощью [setInterval](https://developer.mozilla.org/ru/docs/Web/API/WindowOrWorkerGlobalScope/setInterval):

```js
setInterval(() => {
  refresh()
  counter += 1
}, 1000)
```

<!-- Making repeated calls to the _ReactDOM.render_ method is not the recommended way to re-render components. Next, we'll introduce a better way of accomplishing this effect. -->

Повторные вызовы метода _ReactDOM.render_ не рекомендуются для ререндеринга компонент. Далее мы покажем лучший способ достижения этого эффекта.

### Stateful компоненты

<!-- All of our components up till now have been simple in the sense that they have not contained any state that could change during the lifecycle of the component. -->

Все наши компоненты до сих пор были простыми в том смысле, что они не содержали никакого состояния, которое могло бы измениться в течение жизненного цикла компонента.

<!-- Next, let's add state to our application's <i>App</i> component with the help of React's [state hook](https://reactjs.org/docs/hooks-state.html). -->

Давайте добавим состояние в компонент <i>App</i> с помощью [state хука React](https://reactjs.org/docs/hooks-state.html).

<!-- We will change the application to the following: -->

Мы изменим приложение на следующее:

```js
import React, { useState } from 'react' // highlight-line
import ReactDOM from 'react-dom'

const App = () => {
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

<!-- In the first row, the application imports the _useState_ function: -->

В первой строке приложение импортирует функцию _useState_:

```js
import React, { useState } from 'react'
```

<!-- The function body that defines the component begins with the function call: -->

Тело функции, определяющей компонент, начинается с вызова этой функции:

```js
const [ counter, setCounter ] = useState(0)
```

<!-- The function call adds <i>state</i> to the component and renders it initialized with the value of zero. The function returns an array that contains two items. We assign the items to the variables _counter_ and _setCounter_ by using the destructuring assignment syntax shown earlier. -->

Вызов функции добавляет <i>состояние (state)</i> в компонент и инициализирует его нулевым значением. Сама функция возвращает массив, содержащий два элемента. Мы присваиваем их двум переменным _counter_ и _setCounter_, используя синтаксис деструктурирующего присваивания, показанный ранее.

<!-- The _counter_ variable is assigned the initial value of <i>state</i> which is zero. The variable _setCounter_ is assigned to a function that will be used to <i>modify the state</i>. -->

Переменной _counter_ присваивается начальное значение <i>состояния</i>, которое равно нулю. Переменной _setCounter_ присваивается функция, которая будет использоваться для <i>изменения значения состояния</i>.

<!-- The application calls the [setTimeout](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout) function and passes it two parameters: a function to increment the counter state and a timeout of one second: -->

Приложение вызывает функцию [setTimeout](https://developer.mozilla.org/ru/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout) и передает ей два параметра: функцию увеличения значения состояния счетчика и тайм-аут в секунду:

```js
setTimeout(
  () => setCounter(counter + 1),
  1000
)
```

<!-- The function passed as the first parameter to the _setTimeout_ function is invoked one second after calling the _setTimeout_ function -->

Функция, переданная в качестве первого параметра _setTimeout_, вызывается через одну секунду после вызова функции _setTimeout_.

```js
() => setCounter(counter + 1)
```

<!-- When the state modifying function _setCounter_ is called, <i>React re-renders the component</i> which means that the function body of the component function gets re-executed: -->

Когда вызывается функция _setCounter_, с помощью которой мы изменим состояние, <i>React ререндерит компонент</i>, что приводит к тому, что тело функции компонента повторно выполняется:

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

<!-- The second time the component function is executed it calls the _useState_ function and returns the new value of the state: 1. Executing the function body again also makes a new function call to _setTimeout_, which executes the one second timeout and increments the _counter_ state again. Because the value of the _counter_ variable is 1, incrementing the value by 1 is essentially the same as an expression setting the value of _counter_ to 2. -->

В результате опять вызывается функция _useState_ и она возвращает новое значение состояния: 1. Также происходит новый вызов функции в _setTimeout_, которая по истечению таймаута в одну секунду увеличивает состояние _counter_. Поскольку значение переменной _counter_ равно 1, увеличение значения на 1 по сути то же самое, что выражение, присваивающее значение 2 переменной _counter_.

```js
() => setCounter(2)
```

<!-- Meanwhile, the old value of _counter_ - "1" - is rendered to the screen. -->

Между тем на экран выводится старое значение _counter_ равное "1".

<!-- Every time the _setCounter_  modifies the state it causes the component to re-render. The value of the state will be incremented again after one second, and this will continue to repeat for as long as the application is running. -->

Каждый раз, когда _setCounter_ изменяет состояние, наш компонент перерисовывается. Значение состояния будет снова увеличено через одну секунду, и это будет повторяться до тех пор, пока приложение работает.

<!-- If the component doesn't render when you think it should, or if it renders at the "wrong time", you can debug the application by logging the values of the component's variables to the console. If we make the following additions to our code: -->

Если компонент не отображается, когда вы думаете, что должен, или если он отображается в "неподходящее время", вы можете отладить приложение, записав значения переменных компонента в консоль. Давайте добавим следующие дополнения в наш код:

```js
const App = () => {
  const [ counter, setCounter ] = useState(0)

  setTimeout(
    () => setCounter(counter + 1),
    1000
  )

  console.log('отрисовка...', counter) // highlight-line

  return (
    <div>{counter}</div>
  )
}
```

<!-- It's easy to follow and track the calls made to the _render_ function: -->

Теперь вам будет легко отслеживать вызовы функции _render_:

![](../../images/1/4e.png)

### Обработка событий

<!-- We have already mentioned <i>event handlers</i> a few times in [part 0](/en/part0), that are registered to be called when specific events occur. E.g. a user's interaction with the different elements of a web page can cause a collection of various different kinds of events to be triggered. -->

Мы уже говорили в [части 0](/ru/part0) об <i>обработчиках событий</i>, которые буду вызваны при определенных событиях. Например, взаимодействие пользователя с различными элементами веб-страницы может вызвать запуск множества различных типов событий.

<!-- Let's change the application so that increasing the counter happens when a user clicks a button, which is implemented with the [button](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button) element. -->

Давайте изменим приложение так, чтобы увеличение счетчика происходило в тот момент, когда пользователь нажимает на кнопку - элемент [button](https://developer.mozilla.org/ru/docs/Web/HTML/Element/button).

<!-- Button elements support so-called [mouse events](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent), of which [click](https://developer.mozilla.org/en-US/docs/Web/Events/click) is the most common event. -->

Элементы button поддерживает так называемые [события мыши](https://developer.mozilla.org/ru/docs/Web/API/MouseEvent), одно из которых [click](https://developer.mozilla.org/ru/docs/Web/Events/click) - наиболее распространенное событие.

<!-- In React, registering an event handler function to the <i>click</i> event [happens](https://reactjs.org/docs/handling-events.html) like this: -->

В React регистрация функции обработчика для события <i>click</i> [происходит](https://ru.reactjs.org/docs/handling-events.html) следующим образом:

```js
const App = () => {
  const [ counter, setCounter ] = useState(0)

  // highlight-start
  const handleClick = () => {
    console.log('жмак')
  }
  // highlight-end

  return (
    <div>
      <div>{counter}</div>
      // highlight-start
      <button onClick={handleClick}>
        увеличить
      </button>
      // highlight-end
    </div>
  )
}
```

<!-- We set the value of the button's <i>onClick</i> attribute to be a reference to the _handleClick_ function defined in the code. -->

У button мы присваиваем атрибуту <i>onClick</i> ссылку на функцию _handleClick_.

<!-- Now every click of the <i>plus</i> button causes the _handleClick_ function to be called, meaning that every click event will log a <i>clicked</i> message to the browser console. -->

Теперь каждое нажатие кнопки <i>увеличить</i> вызывает функцию _handleClick_ и каждое событие click будет приводить к выводу сообщения <i>жмак</i> в консоль браузера.

<!-- The event handler function can also be defined directly in the value assignment of the onClick-attribute: -->

Функцию обработчика событий также можно определить непосредственно при присвоении значения атрибуту onClick:

```js
const App = () => {
  const [ counter, setCounter ] = useState(0)

  return (
    <div>
      <div>{counter}</div>
      <button onClick={() => console.log('жмак')}> // highlight-line
        увеличить
      </button>
    </div>
  )
}
```

<!-- By changing the event handler to the following form -->

Изменив обработчик событий следующим образом:

```js
<button onClick={() => setCounter(counter + 1)}>
  увеличить
</button>
```

<!-- we achieve the desired behavior, meaning that the value of _counter_ is increased by one <i>and</i> the component gets re-rendered. -->

мы достигнем желаемого результата - значение _counter_ увеличивается на единицу <i>и</i> компонент повторно перерисовывается.

<!-- Let's also add a button for resetting the counter: -->

Добавим кнопку сброса счетчика:

```js
const App = () => {
  const [ counter, setCounter ] = useState(0)

  return (
    <div>
      <div>{counter}</div>
      <button onClick={() => setCounter(counter + 1)}>
        увеличить
      </button>
      // highlight-start
      <button onClick={() => setCounter(0)}>
        сбросить
      </button>
      // highlight-end
    </div>
  )
}
```

<!-- Our application is now ready! -->

Наше приложение готово!

<!-- ### Tapahtumankäsittelijä on funktio -->

<!-- ### Event handler is a function -->

### Обработчик событий - это функция

<!-- Nappien tapahtumankäsittelijät on siis määritelty suoraan <i>onClick</i>-attribuuttien määrittelyn yhteydessä seuraavasti: -->

<!-- We define the event handlers for our buttons where we declare their <i>onClick</i> attributes: -->

Мы определяем обработчики событий для наших кнопок там, где объявляем их атрибут <i>onClick</i>:

```js
<button onClick={() => setCounter(counter + 1)}>
  увеличить
</button>
```

<!-- Entä jos yritämme määritellä tapahtumankäsittelijän hieman yksinkertaisemmassa muodossa: -->

<!-- What if we'd try to define the event handlers in a simpler form? -->

Что, если бы мы попытались определить обработчики событий в более простой форме?

```js
<button onClick={setCounter(counter + 1)}>
  увеличить
</button>
```

<!-- Tämä muutos kuitenkin hajottaa sovelluksemme täysin: -->

<!-- This would completely break our application: -->

Это полностью сломает наше приложение:

![](../../images/1/5b.png)

<!-- Mistä on kyse? Tapahtumankäsittelijäksi on tarkoitus määritellä joko <i>funktio</i> tai <i>viite funktioon</i>. Kun koodissa on -->

<!-- What's going on? An event handler is supposed to be either a <i>function</i> or a <i>function reference</i>, and when we write -->

Почему? Обработчик событий должен быть либо <i>функцией</i>, либо <i>ссылкой на функцию</i>, и когда мы пишем:

```js
<button onClick={setCounter(counter + 1)}>
```

<!-- tapahtumankäsittelijäksi tulee määriteltyä <i>funktiokutsu</i>. Sekin on monissa tilanteissa ok, mutta ei nyt. Kun React renderöi metodin ensimmäistä kertaa ja muuttujan <i>counter</i> arvo on 0, se suorittaa kutsun <em>setCounter(0 + 1)</em>, eli muuttaa komponentin tilan arvoksi 1. Tämä taas aiheuttaa komponentin uudelleenrenderöitymisen. Ja sama toistuu uudelleen... -->

<!-- the event handler is actually a <i>function call</i>. In many situations this is ok, but not in this particular situation. In the beginning the value of the <i>counter</i> variable is 0. When React renders the method for the first time, it executes the function call <em>setCounter(0+1)</em>, and changes the value of the component's state to 1.
This will cause the component to be re-rendered, react will execute the setCounter function call again, and the state will change leading to another rerender... -->

обработчик событий на самом деле является <i>результатом вызова функции</i>. Во многих ситуациях это нормально, но не в данном конкретном случае. Вначале значение переменной <i>counter</i> равно 0. Когда React отрисовывает компонент в первый раз, он выполняет вызов функции <em>setCounter(0 + 1)</em> и изменяет значение состояния <i>counter</i> на 1.
Это приведет к повторному рендерингу компонента и react снова выполнит вызов функции setCounter, и состояние вновь изменится, что приведет к еще одному повторному рендерингу... И так до бесконечности!

<!-- Palautetaan siis tapahtumankäsittelijä alkuperäiseen muotoonsa -->

<!-- Let's define the event handlers like we did before -->

Давайте определим обработчики событий так, как мы делали это раньше:

```js
<button onClick={() => setCounter(counter + 1)}>
  увеличить
</button>
```

<!-- Nyt napin tapahtumankäsittelijän määrittelevä attribuutti <i>onClick</i> saa arvokseen funktion _() => setCounter(counter + 1)_, ja funktiota kutsutaan siinä vaiheessa kun sovelluksen käyttäjä painaa nappia.  -->

<!-- Now the button's attribute which defines what happens when the button is clicked - <i>onClick</i> - has the value _() => setCounter(counter +1)_.
The setCounter function is called only when a user clicks the button. -->

Теперь атрибут <i>onClick</i>, который определяет, что происходит при нажатии кнопки, имеет значение _() => setCounter (counter +1)_.
Функция setCounter *вызывается только тогда, когда пользователь нажимает кнопку*.

<!-- Tapahtumankäsittelijöiden määrittely suoraan JSX-templatejen sisällä ei useimmiten ole kovin viisasta. Tässä tapauksessa se tosin on ok, koska tapahtumankäsittelijät ovat niin yksinkertaisia.  -->

<!-- Usually defining event handlers within JSX-templates is not a good idea.
Here it's ok, because our event handlers are so simple. -->

Обычно определение обработчика событий в JSX-шаблоне - не лучшая идея.
Здесь все в порядке, потому что наши обработчики событий простые.

<!-- Eriytetään kuitenkin nappien tapahtumankäsittelijät omiksi komponentin sisäisiksi apufunktioikseen: -->

<!-- Let's separate the event handlers into separate functions anyway: -->

В любом случае давайте выделим обработчики событий в отдельные функции:

```js
const App = () => {
  const [ counter, setCounter ] = useState(0)

  // highlight-start
  const increaseByOne = () => setCounter(counter + 1)

  const setToZero = () => setCounter(0)
  // highlight-end

  return (
    <div>
      <div>{counter}</div>
      <button onClick={increaseByOne}> // highlight-line
        увеличить
      </button>
      <button onClick={setToZero}> // highlight-line
        сбросить
      </button>
    </div>
  )
}
```

<!-- Tälläkin kertaa tapahtumankäsittelijät on määritelty oikein, sillä <i>onClick</i>-attribuutit saavat arvokseen muuttujan, joka tallettaa viitteen funktioon: -->

<!-- Here the event handlers have been defined correctly. The value of the <i>onClick</i> attribute is a variable containing a reference to a function: -->

Здесь обработчики событий определены правильно. Значение атрибута <i>onClick</i> - это переменная, содержащая ссылку на функцию:

```js
<button onClick={increaseByOne}>
  увеличить
</button>
```

### Проброс состояния в дочерний компонент

<!-- It's recommended to write React components that are small and reusable across the application and even across projects. Let's refactor our application so that it's composed of three smaller components, one component for displaying the counter and two components for buttons. -->

Рекомендуется писать компоненты React небольшого размера, которые можно использовать повторно в приложении и даже в проектах. Давайте реорганизуем наше приложение так, чтобы оно состояло из трех меньших компонентов: одного компонента для отображения счетчика и двух компонентов для кнопок.

<!-- Let's first implement a <i>Display</i> component that's responsible for displaying the value of the counter. -->

Давайте сначала реализуем компонент <i>Display</i>, который отвечает за отображение значения счетчика.

<!-- One best practice in React is to [lift the state up](https://reactjs.org/docs/lifting-state-up.html) in the component hierarchy. The documentation says: -->


Одна из лучших практик в React - [подъем состояния](https://ru.reactjs.org/docs/lifting-state-up.html) в иерархии компонентов. В документации говорится:

<!-- > <i>Often, several components need to reflect the same changing data. We recommend lifting the shared state up to their closest common ancestor.</i> -->

> <i>Часто несколько компонентов должны отражать одни и те же изменяющиеся данные. В таком случае мы рекомендуем поднять это общее состояние наверх - до их ближайшего общего предка.</i>

<!-- So let's place the application's state in the <i>App</i> component and pass it down to the <i>Display</i> component through <i>props</i>: -->

Итак, давайте поместим состояние приложения в компонент <i>App</i> и передадим его компоненту <i>Display</i> через <i>props</i>:

```js
const Display = (props) => {
  return (
    <div>{props.counter}</div>
  )
}
```

<!-- Using the component is straightforward, as we only need to pass the state of the _counter_ to it: -->

Использовать компонент просто, так как нам нужно только передать ему состояние _counter_:

```js
const App = () => {
  const [ counter, setCounter ] = useState(0)

  const increaseByOne = () => setCounter(counter + 1)
  const setToZero = () => setCounter(0)

  return (
    <div>
      <Display counter={counter}/> // highlight-line
      <button onClick={increaseByOne}>
        увеличить
      </button>
      <button onClick={setToZero}>
        сбросить
      </button>
    </div>
  )
}
```

<!-- Everything still works. When the buttons are clicked and the <i>App</i> gets re-rendered, all of its children including the <i>Display</i> component are also re-rendered. -->

Все работает. Когда нажимаются кнопки и выполняется повторная отрисовка <i>App</i>, все его дочерние элементы, включая компонент <i>Display</i>, также перерисовываются.

<!-- Next, let's make a <i>Button</i> component for the buttons of our application. We have to pass the event handler as well as the title of the button through the component's props: -->

Затем давайте создадим компонент <i>Button</i> для кнопок нашего приложения. Мы должны передать обработчик события, а также заголовок кнопки через свойства компонента:

```js
const Button = (props) => {
  return (
    <button onClick={props.handleClick}>
      {props.text}
    </button>
  )
}
```

<!-- Our <i>App</i> component now looks like this: -->

В итоге, наш компонент <i>App</i> теперь выглядит так:

```js
const App = () => {
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
        text='увеличить'
      />
      <Button
        handleClick={setToZero}
        text='сбросить'
      />
      <Button
        handleClick={decreaseByOne}
        text='уменьшить'
      />
      // highlight-end
    </div>
  )
}
```

<!-- Since we now have an easily reusable <i>Button</i> component, we've also implemented new functionality into our application by adding a button that can be used to decrement the counter. -->

Так как теперь у нас есть переиспользуемый компонент <i>Button</i>, мы легко добавили новую функциональность в наше приложение - кнопку, которая используется для уменьшения счетчика.

<!-- The event handler is passed to the <i>Button</i> component through the _handleClick_ prop. The name of the prop itself is not that significant, but our naming choice wasn't completely random. React's own official [tutorial](https://reactjs.org/tutorial/tutorial.html) suggests this convention. -->

Обработчик события передается компоненту <i>Button</i> через свойство _handleClick_. Само название свойства не так важно, но наш выбор не был случайным. Мы сделали это на основе названий из официального [учебника React](https://ru.reactjs.org/tutorial/tutorial.html).

### Изменения состояния приведут к перерисовке

<!-- Kerrataan vielä sovelluksen toiminnan pääperiaatteet.  -->
<!-- Let's go over the main principles of how an application works once more. -->
Давайте еще раз пройдемся по основным принципам работы приложения.

<!-- When the application starts, the code in _App_ is executed. This code uses a [useState](https://reactjs.org/docs/hooks-reference.html#usestate) hook to create the application state, setting an initial value of the variable _counter_.
This component contains the _Display_ component - which displays the counter's value, 0 - and three _Button_ components. The buttons all have event handlers, which are used to change the state of the counter. -->
Когда приложение запускается, выполняется код в _App_. Этот код использует хук [useState](https://ru.reactjs.org/docs/hooks-reference.html#usestate) для инициализации состояния приложения, устанавливая начальное значение переменной _counter_.
Этот компонент содержит компонент _Display_, который отображает значение счетчика (на старте - это 0), и три компонента _Button_. Все кнопки получают обработчики события, которые используются для изменения состояния счетчика.

<!-- Kun jotain napeista painetaan, suoritetaan vastaava tapahtumankäsittelijä. Tapahtumankäsittelijä muuttaa komponentin _App_ tilaa funktion _setCounter_ avulla. **Tilaa muuttavan funktion kutsuminen aiheuttaa komponentin uudelleenrenderöitymisen.**  -->
<!-- When one of the buttons is clicked, the event handler is executed. The event handler changes the state of the _App_ component with the _setCounter_ function.
**Calling a function which changes the state causes the component to rerender.** -->
При нажатии на одну из кнопок выполняется заданный обработчик события. Обработчик события изменяет состояние компонента _App_ с помощью функции _setCounter_.
**Вызов функции, которая меняет состояние, приводит к повторной отрисовке компонента.**

<!-- Eli jos painetaan nappia <i>plus</i>, muuttaa napin tapahtumankäsittelijä tilan _counter_ arvoksi 1 ja komponentti _App_ renderöidään uudelleen. Komponentin uudelleenrenderöinti aiheuttaa sen "alikomponentteina" olevien _Display_- ja _Button_-komponenttien uudelleenrenderöitymisen. _Display_ saa propsin arvoksi laskurin uuden arvon 1 ja _Button_-komponentit saavat propseina tilaa sopivasti muuttavat tapahtumankäsittelijät. -->
<!-- So, if a user clicks the <i>plus</i> button, the button's event handler changes the value of _counter_ to 1, and the _App_ component is rerendered.
This causes its subcomponents _Display_ and _Button_ to also be re-rendered.
_Display_ receives the new value of the counter, 1, as props. The _Button_ components receive event handlers which can be used to change the state of the counter. -->
В итоге, если пользователь нажимает кнопку <i>плюс</i>, обработчик события кнопки изменяет значение _counter_ на 1, и компонент _App_ отрисовывается повторно.
А это в свою очередь вызывает повторную отрисовку его подкомпонентов _Display_ и _Button_.
_Display_ получает новое значение счетчика равное 1 в качестве свойства. Компоненты _Button_ вновь получают обработчики события, которые можно использовать для изменения состояния счетчика.

### Рефакторинг компонентов

<!-- Laskimen arvon näyttävä komponentti on siis seuraava -->
<!-- The component displaying the value of the counter is as follows: -->

Компонент, отображающий значение счетчика, выглядит следующим образом:

```js
const Display = (props) => {
  return (
    <div>{props.counter}</div>
  )
}
```

<!-- Komponentti tarvitsee ainoastaan <i>propsin</i> kenttää _counter_, joten se voidaan yksinkertaistaa [destrukturoinnin](/osa1/komponentin_tila_ja_tapahtumankasittely#destrukturointi) avulla seuraavaan muotoon: -->
<!-- The component only uses the _counter_ field of its <i>props</i>.
This means we can simplify the component by using [destructuring](/en/part1/component_state_event_handlers#destructuring), like so: -->

Компонент использует только поле _counter_ из <i>props</i>.
Это означает, что мы можем упростить компонент, используя [деструктуризацию](/ru/part1/component_state_event_handlers#destructuring):

```js
const Display = ({ counter }) => {
  return (
    <div>{counter}</div>
  )
}
```

<!-- Koska komponentin määrittelevä metodi ei sisällä muuta kuin returnin, voimme määritellä sen hyödyntäen nuolifunktioiden tiiviimpää ilmaisumuotoa -->
<!-- The method defining the component contains only the return statement, so
we can define the method using the more compact form of arrow functions: -->
Метод, определяющий компонент, содержит только оператор return. Поэтому
мы можем использовать более компактную форму на основе стрелочных функций:

```js
const Display = ({ counter }) => <div>{counter}</div>
```

<!-- Vastaava suoraviivaistus voidaan tehdä myös nappia edustavalle komponentille -->
<!-- We can simplify the Button component as well. -->
Мы также можем упростить компонент Button.

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
<!-- We can use destructuring to get only the required fields from <i>props</i>, and use the more compact form of arrow functions: -->
Мы можем сделать деструктуризацию, чтобы получить только необходимые поля из <i>props</i>. Ну и использовать более компактную форму стрелочных функций:

```js
const Button = ({ handleClick, text }) => (
  <button onClick={handleClick}>
    {text}
  </button>
)
```

</div>
