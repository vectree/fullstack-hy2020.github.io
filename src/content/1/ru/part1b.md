---
mainImage: ../../../images/part-1.svg
part: 1
letter: b
lang: ru
---

<div class="content">

В ходе курса у нас есть цель и необходимость изучить Javascript на достаточно хорошем уровне.

Javascript быстро развивался в последние несколько лет, и в этом курсе мы используем фичи из более новых версий. Официальное название стандарта Javascript - [ECMAScript](https://en.wikipedia.org/wiki/ECMAScript). На данный момент последняя версия выпущена в июне 2019 года под названием [ECMAScript® 2019](http://www.ecma-international.org/ecma-262/10.0/index.html) или ES10.

Браузеры еще не поддерживают все новейшие функции Javascript. В связи с этим большая часть кода, запускаемого в браузерах, <i>транспилирована</i> из более новой версии Javascript в более старую и совместимую.

На сегодняшний день самый популярный способ транспилировать код - это использовать [Babel](https://babeljs.io/). Транспиляция автоматически настроена в React-приложениях, созданных с помощью create-react-app. Мы более подробно рассмотрим конфигурацию транспиляции в [7 части](/ru/part7) этого курса.

[Node.js](https://nodejs.org/en/) - среда выполнения Javascript, основанная на движке Google [chrome V8](https://developers.google.com/v8/), которая работает практически везде - от серверов до мобильных телефонов. Давайте напишем что-то на Javascript с помощью Node. Ожидается, что версия Node.js, установленная на вашем компьютере, имеет версию не ниже <i>10.18.0.</i>. Последние версии Node работаю с последней версией Javascript, поэтому нет необходимости делать [транспайлинг кода](https://ru.wikipedia.org/wiki/%D0%A2%D1%80%D0%B0%D0%BD%D1%81%D0%BF%D0%B0%D0%B9%D0%BB%D0%B5%D1%80).


Код записывается в файлы, заканчивающиеся на <i>.js</i>, и запускается с помощью команды <em>node name\_of\_file.js</em>

Также существует возможность запускать Javascript-код в консоли Node.js (открывается с помощью ввода команды _node_ в терминале), а также в консоли разработчика браузера Chrome. Новейшие версии Chrome часто не требуют транспайлинга кода, так как [довольно хорошо](http://kangax.github.io/compat-table/es2016plus/) поддерживают новые функции Javascript.

Javascript напоминает по названию и по синтаксису Java. Но когда дело доходит до понимания механизма работы языка, вы начинаете понимать насколько разные эти языки. Имея знания Java, поведение Javascript может показаться немного чуждым, особенно если не вникать в его возможности.


В некоторых кругах была популярна попытка "симулировать" или "вживить" особенности и шаблоны Java в Javascript. Мы не рекомендуем делать это.

### Переменные

В Javascript есть несколько способов объявления переменных:

```js
const x = 1
let y = 5

console.log(x, y)   // выводит: 1, 5
y += 10
console.log(x, y)   // выводит: 1, 15
y = 'какой-то текст'
console.log(x, y)   // выводит: 1, какой-то текст
x = 4               // выводит ошибку
```

[const](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Statements/const) фактически объявляет <i>константу</i>, для которой значение не может быть изменено. С другой стороны [let](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Statements/let) объявляет обычную изменяемую переменную.

В этом примере мы также видим, что тип данных, назначенный переменной, может измениться во время выполнения. В начале _y_ хранит целое число, а в конце - строку.

Также возможно объявить переменную в Javascript, используя ключевое слово [var](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Statements/var). Долгое время var был единственным способом объявления переменных. Const и let были только недавно добавлены в версию ES6. В некоторых ситуациях var работает [немного](https://medium.com/craft-academy/javascript-variables-should-you-use-let-var-or-const-394f7645c88f) [не так](http://www.jstips.co/en/javascript/keyword-var-vs-let/), как объявление переменных в большинстве языках. Во время этого курса использование var не рекомендуется, и вы должны придерживаться const и let!

Вы можете найти больше по этой теме, например, на YouTube - [var, let and const - ES6 JavaScript Features](https://youtu.be/sjyJBL5fkp8)

### Массивы

[Массив](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array) и несколько примеров его использования:

```js
const t = [1, -1, 3]

t.push(5)

console.log(t.length) // выводит: 4
console.log(t[1])     // выводит: -1

t.forEach(value => {
  console.log(value)  // выводит числа: 1, -1, 3, 5. каждое с новой строки.
})
```

В этом примере примечателен тот факт, что содержимое массива можно изменить, даже если он объявлен с помощью _const_. Поскольку массив является объектом, переменная всегда ссылается на один и тот же объект. Содержимое массива изменяется по мере добавления в него новых элементов.

Один из способов перебора элементов массива - использование метода _forEach_, как показано в примере. В _forEach_ передается <i>функция</i>, определенная с использованием стрелочного синтаксиса.

```js
value => {
  console.log(value)
}
```

forEach вызывает функцию <i>для каждого элемента в массиве</i>, всегда передавая отдельный элемент в качестве параметра. Функция-параметр, передаваемая в forEach, также может получать [другие параметры](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach).

В примере новый элемент был добавлен в массив с помощью метода [push](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/push). При работе с React часто используются методы функционального программирования. Одной из характеристик парадигмы функционального программирования является использование [immutable](https://ru.wikipedia.org/wiki/%D0%9D%D0%B5%D0%B8%D0%B7%D0%BC%D0%B5%D0%BD%D1%8F%D0%B5%D0%BC%D1%8B%D0%B9_%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82) структур данных. В React предпочтительно использовать метод [concat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat). Этот метод создает новый массив, в который включены содержимое старого массива и новый элемент. По итогу concat не изменяет старый массив.

```js
const t = [1, -1, 3]

const t2 = t.concat(5)

console.log(t)  // выводит: [1, -1, 3]
console.log(t2) // выводит: [1, -1, 3, 5]
```

Вызов метода _t.concat(5)_ не добавляет новый элемент в старый массив. Он возвращает новый массив, который содержит элементы старого массива и новый элемент.

Существует множество полезных методов для работы с массивами. Давайте рассмотрим маленький пример использования метода [map](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/map).

```js
const t = [1, 2, 3]

const m1 = t.map(value => value * 2)
console.log(m1)   // выводит: [2, 4, 6]
```

На основе старого массива map создает <i>новый массив</i>, а переданная функция, заданная в качестве параметра, используется для создания новых элементов. В данном примере каждый элемент старого массива умножается на два.

Map может преобразовать массив в что-то совершенно другое:

```js
const m2 = t.map(value => '<li>' + value + '</li>')
console.log(m2)
// выводит: [ '<li>1</li>', '<li>2</li>', '<li>3</li>' ]
```

В этом примере массив, заполненный целочисленными значениями, преобразуется методом map в массив, содержащий строки HTML. В [2 части](/ru/part2) этого курса мы увидим, что map довольно часто используется в React.

Отдельные элементы массива легко получить с помощью [деструктурирующего присваивания](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment).

```js
const t = [1, 2, 3, 4, 5]

const [first, second, ...rest] = t

console.log(first, second)  // выводит: 1, 2
console.log(rest)          // выводит: [3, 4 ,5]
```

Благодаря присваиванию переменные _first_ и _second_ буду равны первым двум целым числам массива. Остальные целые числа "собираются" в отдельный массив, который присваивается переменной _rest_.

### Объекты

Есть несколько разных способов определения объектов в Javascript. Одним из наиболее распространенных является использование [литерала объекта](https://developer.mozilla.org/ru/docs/Web/JavaScript/Guide/Grammar_and_types#%D0%9B%D0%B8%D1%82%D0%B5%D1%80%D0%B0%D0%BB_%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%B0). В данном случае определение объекта происходит путем перечисления его свойств в фигурных скобках:

```js
const object1 = {
  name: 'Арто Хеллас',
  age: 35,
  education: 'Доктор наук',
}

const object2 = {
  name: 'Разработка Full Stack веб-приложений',
  level: 'промежуточные занятия',
  size: 5,
}

const object3 = {
  name: {
    first: 'Ден',
    last: 'Абрамов',
  },
  grades: [2, 3, 5, 3],
  department: 'Стэнфордский Университет',
}
```

Значения свойств могут быть любого типа: целые числа, строки, массивы, объекты...

Обратиться к свойства объекта можно с помощью точечной нотации или с помощью скобок:

```js
console.log(object1.name) // выводит: Арто Хеллас
const fieldName = 'age'
console.log(object1[fieldName]) // выводит: 35
```

Вы также можете добавлять свойства к объекту на лету, используя точечную нотацию или скобки:

```js
object1.address = 'Хельсинки'
object1['secret number'] = 12341
```

Последнее свойство <i>secret number</i> должно быть добавлено с помощью скобок, так как при использовании точечной нотации <i>secret number</i> является недопустимым именем свойства.

Естественно, объекты в Javascript могут иметь методы. Однако в этом курсе нам не нужно определять какие-либо объекты с методами. Поэтому в ходе курса мы пройдемся по ним кратко.

Объекты еще могут быть определены с помощью функций-конструкторов, что похоже на механизмы других языков программирования, например на классы Java. Несмотря на сходство, в Javascript нет классов в том же смысле, что и в объектно-ориентированных языках программирования. Начиная с версии ES6, был добавлен <i>синтаксис класса</i>, который в определенных случаях имитирует объектно-ориентированные классы.

### Функции

Мы уже познакомились со стрелочными функциями. Полный процесс создания стрелочной функции выглядит следующим образом:

```js
const sum = (p1, p2) => {
  console.log(p1)
  console.log(p2)
  return p1 + p2
}
```

и функция вызывается, как обычно:

```js
const result = sum(1, 5)
console.log(result)
```

Если есть только один параметр, мы можем убрать круглые скобки из определения:

```js
const square = p => {
  console.log(p)
  return p * p
}
```

Если функция содержит только одно выражение, фигурные скобки не нужны. В этом случае функция возвращает результат своего единственного выражения. Теперь, если мы удалим вывод в консоль, мы сможем еще больше сократить определение функции:

```js
const square = p => p * p
```

Эта форма особенно удобна при работе с массивами, например с методом map:

```js
const t = [1, 2, 3]
const tSquared = t.map(p => p * p)
// tSquared равен [1, 4, 9]
```

Стрелочная функция была добавлена в Javascript всего пару лет назад вместе со стандартом [ES6](http://es6-features.org/). До этого единственным способом определения функций было использование ключевого слова _function_.

Есть два способа создать функцию; с помощью [объявления функции](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Statements/function).

```js
function product(a, b) {
  return a * b
}

const result = product(2, 6)
// результат: 12
```

Другой вариант определить функцию - использовать [функциональное выржаение](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/function). В этом случае нет необходимости давать функции имя, и определение может находиться в остальной части кода:


```js
const average = function(a, b) {
  return (a + b) / 2
}

const result = average(2, 5)
// результат: 3.5
```

В этом курсе мы определим все функции, используя стрелочный синтаксис.

</div>

<div class="tasks">
  <h3>Упраженения 1.3.-1.5.</h3>

<i>Продолжим создание приложения, над которым мы начали работать в предыдущих упражнениях. Вы можете писать код в том же проекте, поскольку нас интересует только конечное состояние поданного на проверку приложения.</i>

**Совет:** вы можете столкнуться с проблемами, когда дело касается структуры объекта <i>props</i>, который передается в компоненты. Хороший способ прояснить, что там лежит и не получить ошибку - вывести объект props на консоль, например следующим образом:

```js
const Header = (props) => {
  console.log(props) // highlight-line
  return <h1>{props.course}</h1>
}
```

  <h4>1.3: информация о курсе шаг 3</h4>

Давайте начнем исползовать объекты в нашем приложении. Измените определения переменных компоненты <i>App</i> и выполните рефакторинг приложения, чтобы оно работало:

```js
const App = () => {
  const course = 'Разработка приложения с помощью части стека'
  const part1 = {
    name: 'Основы React',
    exercises: 10
  }
  const part2 = {
    name: 'Использование props для передачи данных',
    exercises: 7
  }
  const part3 = {
    name: 'Состояние компонента',
    exercises: 14
  }

  return (
    <div>
      ...
    </div>
  )
}
```

  <h4>1.4: информация о курсе шаг 4</h4>

Затем поместите объекты в массив. Измените определения переменных <i>App</i> по приведенному примеру и измените другие части приложения соответствующим образом:

```js
const App = () => {
  const course = 'Разработка приложения с помощью части стека'
  const parts = [
    {
      name: 'Основы React',
      exercises: 10
    },
    {
      name: 'Использование props для передачи данных',
      exercises: 7
    },
    {
      name: 'Состояние компонента',
      exercises: 14
    }
  ]

  return (
    <div>
      ...
    </div>
  )
}
```

**NB** на этом этапе <i>вы можете предположить, что всегда будет три элемента</i>, поэтому нет необходимости проходит по массивам с помощью циклов. Мы вернемся к теме рендеринга компонентов на основе элементов массива и разберем ее более подробнов [следующей части курса](../part2).

Но не передавайте разные объекты как отдельные свойства из компонента <i>App</i> в компоненты <i>Content</i> и <i>Total</i>. Вместо этого передайте их как массив:

```js
const App = () => {
  // определение константы

  return (
    <div>
      <Header course={course} />
      <Content parts={parts} />
      <Total parts={parts} />
    </div>
  )
}
```

  <h4>1.5: информация о курсе шаг 5</h4>

Давайте сделаем еще один шаг. Переместите переменные **course** и **parts** в Javascript-объект. Исправьте все появившиеся ошибки.

```js
const App = () => {
  const course = {
    name: 'Разработка приложения с помощью части стека',
    parts: [
      {
        name: 'Основы React',
        exercises: 10
      },
      {
        name: 'Использование props для передачи данных',
        exercises: 7
      },
      {
        name: 'Состояние компонента',
        exercises: 14
      }
    ]
  }

  return (
    <div>
      ...
    </div>
  )
}
```

</div>

<div class="content">

### Методы объекта и "this"

В связи с тем, что в этом курсе мы используем версию React с хуками, нам не нужно определять объекты с помощью методов. **Содержание этой главы не имеет отношения к курсу**, но вам, безусловно, будет полезно это знать. В частности, при использовании более старых версий React необходимо понимать содержание этой главы.

Стрелочные функции и функции, определенные с помощью ключевого слова _function_, существенно отличаются, когда дело доходит до их поведения по отношению к ключевому слову [this](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/this), которое ссылается на сам объект.

Мы можем добавить методы в объект, определив свойства, значением которых являются функции:

```js
const arto = {
  name: 'Арто Хеллас',
  age: 35,
  education: 'PhD',
  // highlight-start
  greet: function() {
    console.log('привет, меня зовут', this.name)
  },
  // highlight-end
}

arto.greet()  // выводит: привет, меня зовут Арто Хеллас
```

Методы могут быть добавлены в объект даже после его создания:

```js
const arto = {
  name: 'Арто Хеллас',
  age: 35,
  education: 'PhD',
  greet: function() {
    console.log('привет, меня зовут', this.name)
  },
}

// highlight-start
arto.growOlder = function() {
  this.age += 1
}
// highlight-end

console.log(arto.age)   // выводит: 35
arto.growOlder()
console.log(arto.age)   // выводит: 36
```

Немного доработаем наш объект:

```js
const arto = {
  name: 'Арто Хеллас',
  age: 35,
  education: 'PhD',
  greet: function() {
    console.log('привет, меня зовут', this.name)
  },
  // highlight-start
  doAddition: function(a, b) {
    console.log(a + b)
  },
  // highlight-end
}

arto.doAddition(1, 4)        // выводит: 5

const referenceToAddition = arto.doAddition
referenceToAddition(10, 15)   // выводит: 25
```

Теперь у объекта есть метод _doAddition_, который вычисляет сумму чисел, переданных в качестве параметров. Метод вызывается обычным способом с использованием объекта <em>arto.doAddition(1, 4)</em> или путем сохранения <i>ссылки на метод</i> в переменной и вызова метода через эту переменную <em>referenceToAddition(10, 15)</em>.

Но, если мы попытаемся сделать то же самое с методом _greet_, мы столкнемся с проблемой:

```js
arto.greet()       // выводит: привет, меня зовут Арто Хеллас

const referenceToGreet = arto.greet
referenceToGreet() // выводит только: привет, меня зовут
```

При вызове метода по ссылке метод потерял информацию о том, на какой объект изначально ссылался _this_. В отличие от других языков, в Javascript значение [this](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/this) определяется на основе <i>того, как метод вызывается</i>. При вызове метода через ссылку значение _this_ становится равным так называемому [глобальному объекту](https://developer.mozilla.org/ru/docs/Glossary/Global_object), и конечный результат часто не такой, как изначально задумал разработчик приложения.

Если при написании кода Javascript путаться с _this_, возникнет несколько потенциальных проблем. Ведь часто React или Node (или более специфичному браузерному движку Javascript) необходимо вызвать метод в объекте, который определил разработчик. Однако в этом курсе у нас не будет проблем за счет игнорирования использования this в коде.

Одна из ситуаций, приводящих к исчезновению _this_, возникает, когда мы просим Арто произнести приветствие через одну секунду с помощью метода [setTimeout](https://developer.mozilla.org/ru/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout).

```js
const arto = {
  name: 'Арто Хеллас',
  greet: function() {
    console.log('привет, меня зовут', this.name)
  },
}

setTimeout(arto.greet, 1000)  // highlight-line
```

Как уже был сказано, значение _this_ в Javascript определяется в зависимости от того, как вызывается метод. Когда <em>setTimeout</em> вызывает метод, на самом деле вызов делает движок Javascript и в этот момент _this_ ссылается на глобальный объект.

There are several mechanisms by which the original _this_ can be preserved. One of these is using a method called [bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind):

Есть несколько способов, с помощью которых можно сохранить изначальное значение _this_. Один из них использует метод под названием [bind](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)

```js
setTimeout(arto.greet.bind(arto), 1000)
```

Вызов <em>arto.greet.bind(arto)</em> создает новую функцию, в которой _this_ указывает на объект arto, независимо от того, где и как вызывается метод.

<!-- Using [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) it is possible to solve some of the problems related to _this_. They should not, however, be used as methods for objects because then _this_ does not work at all. We will come back later to the behavior of _this_ in relation to arrow functions.-->

Используя [стрелочные функции](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Functions/Arrow_functions) можно решить некоторые проблемы, связанные с _this_. Однако их не следует использовать в качестве методов объектов, потому что __this__ в таком случае вообще не работает. Позже мы вернемся к поведению _this_ в отношении стрелочных функций.

<!-- If you want to gain a better understanding of how _this_ works in JavaScript, the internet is full of material about the topic, e.g. the screen cast series [Understand JavaScript's this Keyword in Depth](https://egghead.io/courses/understand-javascript-s-this-keyword-in-depth) by [egghead.io](https://egghead.io) is highly recommended! -->

Если вы хотите лучше понять, как _this_ работает в JavaScript, то в Интернете полно материалов по этой теме, например серия скринкастов [Understand JavaScript's this Keyword in Depth](https://egghead.io/courses/understand-javascript-s-this-keyword-in-depth) от [egghead.io](https://egghead.io). Настоятельно рекомендуем!

### Классы

<!-- As mentioned previously, there is no class mechanism like the ones in object-oriented programming languages. There are, however, features in JavaScript which make "simulating" object-oriented [classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) possible. -->

Как упоминалось ранее, тут не существует механизма классов, подобного тем, которые используются в объектно-ориентированных языках программирования. Однако в JavaScript присутствует синтаксис, позволяющий "моделировать" объектно-ориентированные [классы](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Classes).

<!-- Let's take a quick look at the <i>class syntax</i> that was introduced into JavaScript with ES6, which substantially simplifies the definition of classes (or class-like things) in JavaScript.-->

Давайте кратко рассмотрим <i>синтаксис классов</i>, который был введен в JavaScript со стандартом ES6 и существенно упростил определение классов (или чего-то подобного классам) в JavaScript.

<!--In the following example we define a "class" called Person and two Person objects:-->

В следующем примере мы определяем "класс" с именем Person и два объекта Person:

```js
class Person {
  constructor(name, age) {
    this.name = name
    this.age = age
  }
  greet() {
    console.log('привет, меня зовут ' + this.name)
  }
}

const adam = new Person('Адам Ондра', 35)
adam.greet()

const janja = new Person('Джанджа Гарнбрет', 22)
janja.greet()
```

When it comes to syntax, the classes and the objects created from them are very reminiscent of Java classes and objects. Their behavior is also quite similar to Java objects. At the core they are still objects based on JavaScript's [prototypal inheritance](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Inheritance). The type of both objects is actually _Object_, since JavaScript essentially only defines the types [Boolean, Null, Undefined, Number, String, Symbol, and Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures).

Что касается синтаксиса, классы и созданные с их помощью объекты очень напоминают классы и объекты Java. Их поведение также похоже на Java-объекты. По сути, это все еще объекты, основанные на [прототипном наследовании](https://developer.mozilla.org/ru/docs/Learn/JavaScript/Objects/Inheritance) JavaScript. Типом обоих объектов на самом деле является _Object_, поскольку JavaScript по существу определяет только следующие типы: [Boolean, Null, Undefined, Number, String, Symbol и Object](https://developer.mozilla.org/ru/docs/Web/JavaScript/Data_structures).

<!-- The introduction of the class syntax was a controversial addition. Check out [Not Awesome: ES6 Classes](https://github.com/petsel/not-awesome-es6-classes) or [Is “Class” In ES6 The New “Bad” Part?](https://medium.com/@rajaraodv/is-class-in-es6-the-new-bad-part-6c4e6fe1ee65) for more details.-->

Введение классов было спорным. Ознакомьтесь с [Not Awesome: ES6 Classes](https://github.com/petsel/not-awesome-es6-classes) или [Is “Class” In ES6 The New “Bad” Part?](https://medium.com/@rajaraodv/is-class-in-es6-the-new-bad-part-6c4e6fe1ee65), чтобы понять причины.

<!-- The ES6 class syntax is used a lot in "old" React and also in Node.js, hence an understanding of it being beneficial even in this course. However, since we are using the new [hooks](https://reactjs.org/docs/hooks-intro.html) feature of React throughout this course, we have no concrete use for JavaScripts class syntax.-->

Классы ES6 часто используются в "старом" React, а также в Node.js, поэтому понимание преимуществ классов важно даже в этом курсе. Однако, поскольку мы будем использовать [хуки](https://ru.reactjs.org/docs/hooks-intro.html) React на протяжении всего курса, у нас нет необходимостив использовании классов JavaScripts.

### Материалы поJavaScript

<!-- There exists both good and poor guides for JavaScript on the internet. Most of the links on this page relating to JavaScript features reference [Mozilla's Javascript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript).-->

В Интернете есть как хорошие, так и плохие руководства по JavaScript. Большинство ссылок на этой странице, относящихся к возможностям JavaScript, ведутна [Руководство по Javascript от Mozilla](https://developer.mozilla.org/ru/docs/Web/JavaScript).

<!-- It is highly recommended to immediately read [A re-introduction to JavaScript (JS tutorial)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript) on Mozilla's website. -->

Настоятельно рекомендуется прочитать [Повторное введение в JavaScript (JS учебник)](https://developer.mozilla.org/ru/docs/Web/JavaScript/A_re-introduction_to_JavaScript) на сайте Mozilla.

<!-- If you wish to get to know JavaScript deeply there is a great free book series on the internet called [You-Dont-Know-JS](https://github.com/getify/You-Dont-Know-JS). -->

Если вы хотите глубже изучить JavaScript, в Интернете есть отличная серия бесплатных книг под названием [You-Dont-Know-JS](https://github.com/azat-io/you-dont-know-js-ru).

<!-- Another great resource for learning JavaScript is [javascript.info](https://javascript.info). -->

Еще один отличный ресурс для изучения JavaScript - это [javascript.info](https://javascript.info).

<!-- [egghead.io](https://egghead.io) has plenty of quality screencasts on JavaScript, React, and other interesting topics. Unfortunately, some of the material is behind a paywall. -->

[egghead.io](https://egghead.io) содержит множество качественных скринкастов по JavaScript, React и другим интересным темам. К сожалению, часть материалов платно.

</div>
