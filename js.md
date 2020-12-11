# JS
---

<a name='1'> ## В данном материале собраны основные концепции и принципы современного JavaScript, которые помогут быстро найти и вспомнить необходимую фичу.</a>

1. [Содержание](#1)
2. [Объявление переменных var, const, let](#2)
3. [Стрелочные функции](#3)
4. [Параметры функции по умолчанию](#4)
5. [Деструктурирование объектов и массивов](#5)
6. [Методы массивов map/filter/reduce](#6)
7. [Оператор spread(…)](#7)
8. [Сокращение свойств объекта](#8)
9. [Объекты «промисы»](#9)
10. [Шаблонные литералы](#10)
11. [Теговые шаблоны](#11)
12. [Инструкции импорта и экспорта](#12)
13. [Ключевое слово this](#13)
14. [Классы](#14)
15. [Ключевые слова extends и super](#15)

16. [Функция async и оператор await](#16)
17. [Truthy/Falsy](#17)
18. [Анаморфизмы и катаморфизмы](#18)
19. [Генераторы](#19)
20. [Статические методы](#20)

# <a name='2'>Объявление переменных: var, const, let</a>

В JavaScript переменные объявляют при помощи трёх операторов. При этом заданные через const переменные изменить нельзя, а через var и let — можно. Поэтому если в коде предстоит изменить значение переменной, задавайте её с помощью let, если нет — через const.

Область переменных функции обозначает рамки их использования в коде.

Оператор var. Объявленные с его помощью переменные работают в пределах функции и не доступны вне её.

```javascript
function myFunction() {
  var myVar = "Nick";
  console.log(myVar); // "Nick" - myVar доступна в пределах функции
}
console.log(myVar); // выведет ошибку ReferenceError, myVar не доступна вне функции
```

Другой пример, демонстрирующий рамки области переменных:

```javascript
function myFunction() {
  var myVar = "Nick";
  if (true) {
    var myVar = "John";
    console.log(myVar); // "John"
    // myVar находится в пределах функции, мы изменили значение myVar с "Nick" на "John"
  }
  console.log(myVar); // команды блока if влияют на значение "John"
}
console.log(myVar); // выведет ошибку ReferenceError, myVar не доступна вне функции
```

Переменные при выполнении оператора присваивания перемещаются в начало, что называется «поднятие переменных». Часть кода:

```javascript
console.log(myVar) // не определена, поэтому ошибки нет
var myVar = 2;
```

при выполнении понимается как:

```javascript
var myVar;
console.log(myVar) // не определена, поэтому ошибки нет
myVar = 2;
Оператор let. Переменные, объявленные с его помощью, имеют блочную область видимости. Кроме того, они не доступны, пока не присвоены и пока их значение нельзя изменить в той же области. На примере, приведённом выше:
function myFunction() {
  let myVar = "Nick";
  if (true) {
    let myVar = "John";
    console.log(myVar); // "John"
    // myVar имеет блочную область видимости, поэтому мы задали новое значение
    // оно не доступно вне области и не зависит от предыдущего значения myVar
  }
  console.log(myVar); // команды блока if НЕ влияют на "Nick"
}
console.log(myVar); // выведет ReferenceError, myVar не доступна вне функции
```

Вот как это работает для не доступных до присвоения переменных, объявленных операторами let и const:

```javascript
console.log(myVar) // выведет ReferenceError
let myVar = 2;
```
Переменные, объявленные с помощью var, доступны до присвоения. При использовании let и const — только после него. Этот феномен получил название «Временные мёртвые зоны». Кроме того, с помощью оператора let нельзя объявить другую переменную:

```javascript
let myVar = 2;
let myVar = 3; // выведет SyntaxError
```

Оператор const. Переменные, объявленные с его помощью, также имеют блочную область видимости и не доступны до присвоения. Кроме того, их нельзя объявить или заново присвоить в той же области:

```javascript
const myVar = "Nick";
myVar = "John" // выводит ошибку, присвоить заново нельзя
const myVar = "Nick";
const myVar = "John" // выведет ошибку, объявить другое значение нельзя
```

Однако тонкость заключается в том, что вы можете изменить значение объявленной оператором const переменной для объектов:

```javascript
const person = {
  name: 'Nick'
};
person.name = 'John' // будет работать, поскольку значение изменили, а не присвоили заново
console.log(person.name) // "John"
person = "Sandra" // выведет ошибку, поскольку повторное присвоение не доступно для переменных, объявленных с помощью const
и массивов:

const person = [];
person.push('John'); // будет работать, поскольку переменную person изменили, а не присвоили заново
console.log(person[0]) // "John"
person = ["Nick"] // выведет ошибку, поскольку повторное присвоение не доступно для переменных, объявленных с помощью const
```

# <a name='3'>Стрелочные функции</a>

Стрелочные функции ввели в обновлении ES6 как альтернативный способ объявления и использования функций.

Краткость. Стрелочные функции короче традиционных, и каждый случай требует отдельного рассмотрения.
Явная/неявная функция return. Явная функция предполагает использование return в коде:

```javascript
function double(x) {
    return x * 2; // функция возвращает x * 2 с использованием слова return в коде
  }
```

Традиционный код отличается наличием ключевого слова:

```javascript
  const double = (x) => {
    return x * 2; // явная функция
  }
```

Поскольку перед return нет команд, стрелочные функции позволяют избежать дополнительной строки:

```javascript
const double = (x) => x * 2; // верно, возвращает x*2
```

Мы убрали скобки и ключевое слово, но вывод всё равно x * 2. Если функция не возвращает значение (имеет побочные эффекты), это происходит ни явно, ни неявно. При необходимости возврата объекта, его заключают в скобки:

```javascript
const getPerson = () => ({ name: "Nick", age: 24 })
console.log(getPerson()) // объект { name: "Nick", age: 24 } возвращается при помощи стрелочной функции
```

Единственный аргумент. Если функции присвоено одно значение, скобки можно исключить:

```javascript
const double = (x) => x * 2; // в этой функции один параметр
const double = x => x * 2; // в этой тоже один параметр
```

Отсутствие аргументов. В случае если аргументы не заданы, необходимо поставить скобки, иначе синтаксис будет неверным:

```javascript
  () => { // скобки присутствуют, всё работает
    const x = 2;
    return x;
  }
  => { // скобки отсутствуют, код не будет работать
    const x = 2;
    return x;
  }
```
Привязка ключевого слова this к окружению. Чтобы понять тонкости, необходимо знать о поведении this в JavaScript. Для доступа к переменной в функции внутри функции, например, пришлось бы использовать that = this или self = this. А в стрелочных функциях значение this заимствуется из окружения, а не присваивается при создании нового ключевого слова. Например, используем setTimeout в функции myFunc:

```javascript
function myFunc() {
  this.myVar = 0;
  var that = this; // that = this
  setTimeout(
    function() { // новое this создано из области
      that.myVar++;
      console.log(that.myVar) // 1

      console.log(this.myVar) // не определено — см. объявление функции выше
    },
    0
  );
}
```

Теперь в том же коде используем стрелочные функции:

```javascript
function myFunc() {
  this.myVar = 0;
  setTimeout(
    () => { // this из окружения, предполагает значение myFunc
      this.myVar++;
      console.log(this.myVar) // 1
    },
    0
  );
}
```

# <a name='4'>Параметры функции по умолчанию</a>

С выходом версии ES2015 значение по умолчанию для параметра функции задаётся при помощи следующего синтаксиса:

```javascript
function myFunc(x = 10) {
  return x;
}
console.log(myFunc()) // 10 — не задано значение, поэтому значение x по умолчанию присвоено x в функции myFunc
console.log(myFunc(5)) // 5 — значение задано, поэтому x=5 в функции myFunc

console.log(myFunc(undefined)) // 10 — задано значение undefined, поэтому по умолчанию равно x
console.log(myFunc(null)) // null — величина задана, продолжение ниже
```

Параметр по умолчанию применяется в двух случаях: когда он не задан или задан параметр undefined. При введении null параметр по умолчанию не применится.

# <a name="5">Деструктурирование объектов и массивов</a>
Деструктурирование — создание новых переменных путём извлечения данных из объектов и массивов. Например, извлечение параметров this.props из React-проектов.

Объект. Рассмотрим для всех примеров объект:

```javascript
const person = {
  firstName: "Nick",
  lastName: "Anderson",
  age: 35,
  sex: "M"
}
```

Без деструктурирования:

```javascript
const first = person.firstName;
const age = person.age;
const city = person.city || "Paris";
```

Применяя деструктурирование:

```javascript
const { firstName: first, age, city = "Paris" } = person; // так выглядит деструктурирование одной строкой

console.log(age) // 35 — создана переменная age, которая равна person.age
console.log(first) // "Nick" — создана переменная first, значение которой соответствует person.firstName
console.log(firstName) // ReferenceError — person.firstName существует, но новая переменная называется first
console.log(city) // Paris — создана переменная city, а поскольку person.city не определена, city равна заданному по умолчанию значению "Paris".
```

Параметры функции. Деструктурирование применяется для извлечения параметров объектов в функции.
Без деструктурирования:

```javascript
function joinFirstLastName(person) {
  const firstName = person.firstName;
  const lastName = person.lastName;
  return firstName + '-' + lastName;
}

joinFirstLastName(person); // Nick-Anderson
```

Извлекая параметр person, получим компактную функцию:

```javascript
function joinFirstLastName({ firstName, lastName }) { // Мы создаём переменные firstName и lastName, деструктурируя параметр person
  return firstName + '-' + lastName;
}

joinFirstLastName(person); // Nick-Anderson
```

Со стрелочными функциями код становится существенно меньше:

```javascript
const joinFirstLastName = ({ firstName, lastName }) => firstName + '-' + lastName;

joinFirstLastName(person); // Nick-Anderson
```

Массив. Рассмотрим массив:

```javascript
const myArray = ["a", "b", "c"];
```

Без деструктурирования он выглядит так:

```javascript
const x = myArray[0];
const y = myArray[1];
```

С деструктурированием:

```javascript
const [x, y] = myArray; // вот оно!

console.log(x) // "a"
console.log(y) // "b"
```

# <a name='6'>Методы массивов map/filter/reduce</a>

Методы массивов пришли в JavaScript из функционального программирования. Используя три этих метода, вы избегаете циклов for и forEach в большинстве ситуаций. Попробуйте вместо for использовать совокупность map, filter и reduce. В первый раз будет сложно, потому что придётся научиться мыслить иначе, но дальше привыкнете. Посчитаем для примера сумму оценок студентов с результатом 10 и выше, используя три этих метода:

```javascript
const students = [
  { name: "Nick", grade: 10 },
  { name: "John", grade: 15 },
  { name: "Julia", grade: 19 },
  { name: "Nathalie", grade: 9 },
];

const aboveTenSum = students
  .map(student => student.grade) // сравниваем массив студентов с массивом их оценок
  .filter(grade => grade >= 10) // отбираем оценки выше 10
  .reduce((prev, next) => prev + next, 0); // суммируем каждую оценку выше 10

console.log(aboveTenSum) // 44 -- 10 (Nick) + 15 (John) + 19 (Julia), Nathalie игнорируется, поскольку её оценка ниже 10
```

Для более детального наглядного объяснения возьмём массив:

```javascript
const numbers = [0, 1, 2, 3, 4, 5, 6];
Array.prototype.map().
const doubledNumbers = numbers.map(function(n) {
  return n * 2;
});
console.log(doubledNumbers); // [0, 2, 4, 6, 8, 10, 12]
```

Что мы тут сделали? .map выполняет итерацию каждого элемента массива numbers и перемещает их в функцию. Цель функции — обработать и вернуть новую переменную, чтобы map мог заменить её.

Теперь посмотрим на функцию отдельно, чтобы было понятнее:

```javascript
const doubleN = function(n) { return n * 2; };
const doubledNumbers = numbers.map(doubleN);
console.log(doubledNumbers); // [0, 2, 4, 6, 8, 10, 12]
```
Метод часто используется со стрелочными функциями:

```javascript
const doubledNumbers = numbers.map(n => n * 2);
console.log(doubledNumbers); // [0, 2, 4, 6, 8, 10, 12]
```

Используем numbers.map(doubleN) и получаем

[doubleN(0), doubleN(1), doubleN(2), doubleN(3), doubleN(4), doubleN(5), doubleN(6)]
, что равно [0, 2, 4, 6, 8, 10, 12].

```javascript
Array.prototype.filter().
const evenNumbers = numbers.filter(function(n) {
  return n % 2 === 0; // верно, если значение n равно значению, и ложно, если n не равно значению
});
console.log(evenNumbers); // [0, 2, 4, 6]
```

Метод также часто используется со стрелочными функциями:

```javascript
const evenNumbers = numbers.filter(n => n % 2 === 0);
console.log(evenNumbers); // [0, 2, 4, 6]
.filter осуществляет итерацию каждого элемента массива numbers и добавляет их в функцию. Цель функции — вернуть булево значение и определить, нужно оно или нет. После чего фильтр возвращает массив только с добавленными значениями.

Array.prototype.reduce(). Цель метода — сократить переменные до одной после итерации.
const sum = numbers.reduce(
  function(acc, n) {
    return acc + n;
  },
  0 // значение накопительной переменной на первой ступени итерации
);

console.log(sum) // 21
```

Метод также часто используется со стрелочными функциями:

```javascript
const sum = numbers.reduce((acc, n) => acc + n, 0);
console.log(sum) // 21
```

.reduce применяется к массиву и использует функцию как первый параметр. Однако в данном случае мы сталкиваемся с исключениями:

функция .reduce учитывает 2 параметра: вызываемую на каждом шаге итерации функцию и значение накопительной переменной (acc) на первом шаге итерации.
параметры функции: функция, принятая за значение .reduce, имеет 2 других — накопительную переменную (acc) и текущий элемент (n). Накопительная переменная равна возвращаемому на предыдущем шаге итерации значению функции. На первой ступени acc равна значению, принятому за второй параметр .reduce.
Ступени итерации:

acc = 0, поскольку мы ввели «0» вторым значением для reduce;
n = 0, первый элемент массива number.
Функция возвращает acc + n –> 0 + 0 –> 0
acc = 0, значение, возвращённое функцией на предыдущей ступени итерации;
n = 1, второй элемент массива number.
Функция возвращает acc + n –> 0 + 1 –> 1
acc = 1, значение, возвращённое функцией на предыдущей ступени итерации;
n = 2, третий элемент массива number.
Функция возвращает acc + n –> 1 + 2 –> 3
acc = 3, значение, возвращённое функцией на предыдущей ступени итерации;
n = 3, четвёртый элемент массива number.
Функция возвращает acc + n –> 3 + 3 –> 6
(…действия повторяются до последнего шага)
acc = 15, поскольку это значение функция вернула на предыдущей ступени
n = 6, последний элемент массива number.
На последнем шаге функция возвращает acc + n –> 15 + 6 –> 21

# <a name='7'>Оператор spread(…)</a>

Его добавили в обновлении ES2015, чтобы элементы итерации (например массива) можно было использовать в качестве нескольких элементов в коде.

Объектные литералы.
Возьмём два массива:

```javascript
const arr1 = ["a", "b", "c"];
const arr2 = [arr1, "d", "e", "f"]; // [["a", "b", "c"], "d", "e", "f"]
```

В arr2 первый аргумент — массив, поскольку читает содержимое arr1. Но нам нужно сделать arr2 массивом, состоящим из последовательности букв. Для этого используем spread и получим необходимый результат:

```javascript
const arr1 = ["a", "b", "c"];
const arr2 = [...arr1, "d", "e", "f"]; // ["a", "b", "c", "d", "e", "f"]
```

Оставшиеся параметры функции. С их помощью мы можем создать массив с неучтёнными параметрами функции. Объект arguments относится к функциям, равным массиву переданных функции аргументов.

```javascript
function myFunc() {
  for (var i = 0; i < arguments.length; i++) {
    console.log(arguments[i]);
  }
}

myFunc("Nick", "Anderson", 10, 12, 6);
// "Nick"
// "Anderson"
// 10
// 12
// 6
```

Давайте предположим, что нам нужно добавить студента с его оценками и посчитать средний балл. Не лучше ли сделать два параметра двумя разными значениями, а затем собрать массив из полученных данных для итерации? В этом нам и поможет оставшийся параметр.

```javascript
function createStudent(firstName, lastName, ...grades) {
  // firstName = "Nick"
  // lastName = "Anderson"
  // [10, 12, 6]: "..." собирает оставшиеся параметры и создаёт содержащее их значение массива grades

  const avgGrade = grades.reduce((acc, curr) => acc + curr, 0) / grades.length; // высчитывает средний балл из всех

  return {
    firstName: firstName,
    lastName: lastName,
    grades: grades,
    avgGrade: avgGrade
  }
}

const student = createStudent("Nick", "Anderson", 10, 12, 6);
console.log(student);
// {
//   firstName: "Nick",
//   lastName: "Anderson",
//   grades: [10, 12, 6],
//   avgGrade: 9,33
// }
```

Расширение свойств объекта.
```javascript
const myObj = { x: 1, y: 2, a: 3, b: 4 };
const { x, y, ...z } = myObj; // деструктурируем объект
console.log(x); // 1
console.log(y); // 2
console.log(z); // { a: 3, b: 4 }

// z — оставшаяся часть деструктурированного объекта myObj после вычитания свойств x и y

const n = { x, y, ...z };
console.log(n); // { x: 1, y: 2, a: 3, b: 4 }

// свойства объекта z распространяются на n
```

# <a name='8'>Сокращение свойств объекта</a>

Зададим переменную для свойств объекта. Если её имя совпадёт с названием свойства, можно сделать следующее:

```javascript
const x = 10;
const myObj = { x };
console.log(myObj.x) // 10
```

Если вы объявляли объектный литерал в версиях до ES2015 или хотели использовать переменную в качестве значения свойств объекта, то пришлось бы писать:

```javascript
const x = 10;
const y = 20;

const myObj = {
  x: x, // объявление значения переменной x для myObj.x
  y: y // объявление значения переменной y для myObj.y
};

console.log(myObj.x) // 10
console.log(myObj.y) // 20
```

Слишком много повторений, не так ли? Поэтому с выходом ES2015 при совпадении переменной с названием свойства, достаточно такого кода:

```javascript
const x = 10;
const y = 20;

const myObj = {
  x,
  y
};

console.log(myObj.x) // 10
console.log(myObj.y) // 20
```

# <a name='9'>Объекты «промисы»</a>
Промис — это объект, который используется для упорядочивания синхронных и асинхронных операций. При использовании промисов код становится чище, поэтому они всё чаще встречаются в проектах.

```javascript
const fetchingPosts = new Promise((res, rej) => {
  $.get("/posts")
    .done(posts => res(posts))
    .fail(err => rej(err));
});

fetchingPosts
  .then(posts => console.log(posts))
  .catch(err => console.log(err));
```

AJAX-запрос при выполнении не является синхронным, поскольку ответ от ресурса идёт какое-то время. Он может вообще не прийти, если ресурс не доступен (404). Для решения этой проблемы в ES2015 добавили промисы, которые принимают 3 состояния:

ожидание;
выполнен;
отклонён.
Представим, что нам нужно создать Ajax-запрос до ресурса X. Используем для этого метод jQuery.get():

```javascript
const xFetcherPromise = new Promise( // создаём промис, используя ключевое слово new и записываем его в переменную
  function(resolve, reject) { // конструктор промиса использует аргумент функции с другими аргументами — resolve и reject
    $.get("X") // посылаем AJAX-запрос
      .done(function(X) { // как только запрос завершён
        resolve(X); // промис использует resolve с переменной X в качестве аргумента
      })
      .fail(function(error) { // если запрос отклонён
        reject(error); // промис использует reject с error в качестве аргумента
      });
  }
)
```

Объект Promise выполняет функцию executor с аргументами resolve и reject. Эти аргументы выполняются по завершении операции как и функции, которые переводят промис из состояния ожидания в состояние выполнения или отклонения. Функция executor выполняется сразу после создания промиса в статусе ожидания. Как только аргументами функции становятся resolve или reject, промис использует необходимые методы.

Используем методы промиса, чтобы получить его выполнение или ошибку:

```javascript
xFetcherPromise
  .then(function(X) {
    console.log(X);
  })
  .catch(function(err) {
    console.log(err)
  })
```
В случае сдержанного промиса выполняется resolve и функция с методом .then. Иначе выполняется reject и функция с методом .catch. Также обработчик будет выполнен при сдержанном или нарушенном промисе, что приведёт к отсутствию «состояния гонки» между завершением асинхронной операции и применением обработчика.

# <a name="10">Шаблонные литералы</a>
Допускают использование строковой интерполяции и многострочных литералов. Другими словами, это синтаксис, допускающий использование выражений JavaScript внутри строк.

```javascript
const name = "Nick";
`Hello ${name}, the following expression is equal to four : ${2+2}`;

// Hello Nick, the following expression is equal to four: 4
```

# <a name="11">Теговые шаблоны</a>
Являются расширенной формой шаблонных литералов и позволяют разбирать их с помощью функции. При вызове функции первый аргумент содержит массив строковых значений между интерполированными значениями. Чтобы уместить их все, используйте оператор spread (...). Библиотека styled-components написана с применением теговых шаблонов.

Ниже приведён небольшой пример их работы:

```javascript
function highlight(strings, ...values) {
  const interpolation = strings.reduce((prev, current) => {
    return prev + current + (values.length ? "" + values.shift() + "" : "");
  }, "");

  return interpolation;
}

const condiment = "jam";
const meal = "toast";

highlight`I like ${condiment} on ${meal}.`;
// «мне нравится варенье на тосте»
```

Другой интересный пример:

```javascript
function comma(strings, ...values) {
  return strings.reduce((prev, next) => {
    let value = values.shift() || [];
    value = value.join(", ");
    return prev + next + value;
  }, "");
}

const snacks = ['apples', 'bananas', 'cherries'];
comma`I like ${snacks} to snack on.`;
// "I like apples, bananas, cherries to snack on."
```

# <a name="12">Инструкции импорта и экспорта</a>
Экспорт функций/объектов из модулей ES6 и импорт значений из них.

Именованные импорт/экспорт используют для нескольких величин (ими могут быть лишь объекты первого класса):

```javascript
// mathConstants.js
export const pi = 3.14;
export const exp = 2.7;
export const alpha = 0.35;

// -------------

// myFile.js
import { pi, exp } from './mathConstants.js'; // синтаксис именованного импорта схож с синтаксисом деструктурирования
console.log(pi) // 3.14
console.log(exp) // 2.7

// -------------

// mySecondFile.js
import * as constants from './mathConstants.js'; // подставляем экспортированные значения в переменную constants
console.log(constants.pi) // 3.14
console.log(constants.exp) // 2.7
```

Несмотря на внешнюю схожесть именных импортов с деструктурированием, синтаксис отличается. Они не поддерживают значения по умолчанию или вложенное деструктурирование. Кроме того, можно использовать дополнительное имя, но синтаксис отличается от используемого при деструктурировании:

```javascript
import { foo as bar } from 'myFile.js'; // импортируем foo с новым именем переменной bar
```

Импорт/экспорт по умолчанию.
Для одного модуля доступен один экспорт по умолчанию. Экспорт может быть функцией, классом, объектом и т.д. Значение рассматривается как «основное», поскольку так его проще импортировать:

```javascript
// coolNumber.js
const ultimateNumber = 42;
export default ultimateNumber;

// ------------

// myFile.js
import number from './coolNumber.js';
// экспорт по умолчанию, независимо от названия, подставляется как значение переменной number
console.log(number) // 42
```

Экспорт функции:

```javascript
// sum.js
export default function sum(x, y) {
  return x + y;
}
// -------------

// myFile.js
import sum from './sum.js';
const result = sum(1, 2);
console.log(result) // 3
```

# <a name="13">Ключевое слово this</a>
Поведение этого ключевого слова в JavaScript отличается от других языков и зависит от вызова функции.

```javascript
function myFunc() {
  ...
}

// после каждого выражения выводится аргумент this для функции myFunc

myFunc.call("myString", "hello") // myString — первый аргумент функции .call для this

// В нестрогом режиме
myFunc("hello") // глобальный объект window: myFunc() — это синтаксический сахар для myFunc.call(window, "hello")

// В строгом режиме
myFunc("hello") // объект не определён: myFunc() — это синтаксический сахар для myFunc.call(undefined, "hello")
var person = {
  myFunc: function() { ... }
}

person.myFunc.call(person, "test") // объект person — аргумент первого вызова функции для this
person.myFunc("test") // объект person: person.myFunc() — синтаксический сахар для person.myFunc.call(person, "test")

var myBoundFunc = person.myFunc.bind("hello") // создаёт новую функцию, в которую мы подставляем hello в качестве значения переменной для this
person.myFunc("test") // объект person: привязка не влияет на начальный метод
myBoundFunc("test") // hello: myBoundFunc это функция person.myFunc с привязкой hello к this
```

# <a name="14">Классы</a>

JavaScript — прототип-ориентированный язык программирования. Классы ввели как синтаксический сахар для прототип-ориентированного наследования в ES6. Слово «класс» смутит вас, если вы знакомы с классами в других языках программирования. Попробуйте посмотреть иначе: прочитайте о прототипах и их поведении в JavaScript. До ES6 синтаксис прототипов выглядел следующим образом:

```javascript
var Person = function(name, age) {
  this.name = name;
  this.age = age;
}
Person.prototype.stringSentence = function() {
  return "Привет, я " + this.name + " и мне " + this.age;
}
С синтаксисом классов в ES6:

class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  stringSentence() {
    return `Привет, я ${this.name} и мне ${this.age}`;
  }
}

const myPerson = new Person("Manu", 23);
console.log(myPerson.age) // 23
console.log(myPerson.stringSentence()) // Привет, я Ману и мне 23
```

# <a name="15">Ключевые слова extends и super</a>
Ключевое слово extends используют для объявления классов или в выражениях класса для создания дочерних классов. Они получают свойства родительских классов, а также дают возможность добавить новые свойства и изменить заимствованные.

Ключевое слово super вызывает функции родителя объекта, включая его конструктор. Его следует использовать:

до ключевого слова this в конструкторе;
с вызовом super(arguments) при передаче аргументов конструктору класса;
как вызов дочернего класса super.X() для метода X родительского класса.

```javascript
class Polygon {
  constructor(height, width) {
    this.name = 'Polygon';
    this.height = height;
    this.width = width;
  }

  getHelloPhrase() {
    return `Привет, я — ${this.name}`;
  }
}

class Square extends Polygon {
  constructor(length) {
    // здесь вызван конструктор родительского класса со значением lengths
    // использованным для значений width и height класса Polygon 
    super(length, length);
    // в производных классах super() необходимо вызывать перед this
    // иначе будет выведено сообщение об ошибке
    this.name = 'Square';
    this.length = length;
  }

  getCustomHelloPhrase() {
    const polygonPhrase = super.getHelloPhrase(); // доступ к родительскому методу с синтаксисом super.X()
    return `${polygonPhrase} with a length of ${this.length}`;
  }

  get area() {
    return this.height * this.width;
  }
}

const mySquare = new Square(10);
console.log(mySquare.area) // 100
console.log(mySquare.getHelloPhrase()) // «Привет, я Square»: Square — наследник класса Polygon с доступом к его методам
console.log(mySquare.getCustomHelloPhrase()) // Привет, я Square с длиной 10
```

Если бы мы попытались использовать this перед super() в классе Square, появилась бы ошибка ReferenceError:

```javascript
class Square extends Polygon {
  constructor(length) {
    this.height; // ReferenceError, ключевое слово super необходимо использовать раньше

    // здесь вызван конструктор родительского класса со значением lengths
    // использованным для значений width и height класса Polygon
    super(length, length);

    // в производных классах super() необходимо вызывать перед this
    // иначе будет выведено сообщение об ошибке
    this.name = 'Square';
  }
}
```

# <a name="16">Функция async и оператор await</a>
Для написания асинхронного кода в JavaScript появился синтаксис async/await. Цель нововведения — упростить использование промисов и расширить рамки действий с ними. Для лучшего понимания этого синтаксиса, рекомендуем сначала ознакомиться с промисами. await может использоваться только внутри асинхронной функции.

```javascript
async function getGithubUser(username) { // ключевое слово async позволяет использовать await в функции, которая возвращает промис
  const response = await fetch(`https://api.github.com/users/${username}`); // выполнение приостановлено до момента получения ответа от переданного промиса
  return response.json();
}

getGithubUser('mbeaudru')
  .then(user => console.log(user)) // ответ пользователя, проходящего авторизацию: синтаксис await невозможно использовать, поскольку код не является асинхронной функцией
  .catch(err => console.log(err)); // в случае ошибки асинхронной функции, мы увидим её
```

async/await используется с промисами, но предполагают более императивный стиль кода. Оператор async определяет асинхронную функцию и всегда возвращает промис. Оператор await приостанавливает выполнение функции async, пока промис не выполнен или отклонён:

```javascript
async function myFunc() {
  // допустимо использование await, поскольку функция асинхронная
  return "hello world";
}

myFunc().then(msg => console.log(msg)) // hello world: возвращённое значение myFunc стало промисом из-за асинхронной функции
```

Если в асинхронной функции достигается значение return, промис приобретает возвращённое значение. При выводе ошибки промис переходит в статус «отклонён». В то же время при отсутствии возвращённого значения асинхронной функции, промис возвращается без значения по завершению выполнения асинхронной функции.

Оператор await ожидает выполнения промиса.

Функция fetch позволяет выполнить AJAX-запрос.

Выберем пользователя GitHub с помощью промисов и функции fetch:

```javascript
function getGithubUser(username) {
  return fetch(`https://api.github.com/users/${username}`).then(response => response.json());
}

getGithubUser('mbeaudru')
  .then(user => console.log(user))
  .catch(err => console.log(err));
```

А теперь эквивалент с использованием async/await:

```javascript
async function getGithubUser(username) { // можем использовать промис с ключевым словом await
  const response = await fetch(`https://api.github.com/users/${username}`); // выполнение останавливается, пока не выполнен промис
  return response.json();
}

getGithubUser('mbeaudru')
  .then(user => console.log(user))
  .catch(err => console.log(err));
```

Async/await подходят в случае с цепочкой взаимосвязанных промисов. Например можно использовать их для получения токена, чтобы выделить пост блога в базе данных, а также информацию об авторе:

```javascript
async function fetchPostById(postId) {
  const token = (await fetch('token_url')).json().token;
  const post = (await fetch(`/posts/${postId}?token=${token}`)).json();
  const author = (await fetch(`/users/${post.authorId}`)).json();

  post.author = author;
  return post;
}

fetchPostById('gzIrzeo64')
  .then(post => console.log(post))
  .catch(err => console.log(err));
```

Обработка ошибок.
Если не добавить блоки try/catch к выражению await, неперехваченные исключения будут отклонять промис, возвращённый асинхронной функцией. При этом неважно, находятся ли они внутри асинхронной функции или возникли во время await. Использование throw внутри асинхронной функции равноценно отклонённому промису.

Вот так можно устранить ошибки при помощи промисов:

```javascript
function getUser() { // этот промис будет отклонён!
  return new Promise((res, rej) => rej("User not found !"));
}

function getAvatarByUsername(userId) {
  return getUser(userId).then(user => user.avatar);
}

function getUserAvatar(username) {
  return getAvatarByUsername(username).then(avatar => ({ username, avatar }));
}

getUserAvatar('mbeaudru')
  .then(res => console.log(res))
  .catch(err => console.log(err)); // "User not found !"
```

то же самое, но с async/await:

```javascript
async function getUser() { // возвращённый промис будет отклонён!
  throw "User not found !";
}

async function getAvatarByUsername(userId) => {
  const user = await getUser(userId);
  return user.avatar;
}

async function getUserAvatar(username) {
  var avatar = await getAvatarByUsername(username);
  return { username, avatar };
}

getUserAvatar('mbeaudru')
  .then(res => console.log(res))
  .catch(err => console.log(err)); // "User not found !"
```

# <a name="17">Truthy/Falsy</a>
Значения, похожие на правду и на ложь в JavaScript, относятся к логическим выражениям. Пример логического выражения — проверка значения оператора if. Значению присваивается true, если оно не равно:

false;
0;
" " (пустая строка);
null;
undefined;
NaN.
Примеры логических выражений:

проверка значения оператора if.
if (myVar) {}
myVar может быть объектом первого класса (переменной, функцией, логическим значением), но будет считаться логическим выражением, поскольку проверяется в его контексте.

после логического оператора NOT !:
Оператор возвращает false, если одиночный операнд может стать истинным; иначе возвращает true.

```javascript
!0 // true: 0 ложно, поэтому возвращает true
!!0 // false: 0 ложно, поэтому !0 возвращает true, а !(!0) возвращает false
!!"" // false: пустая строка ложна, поэтому NOT (NOT false) равно false
```

C конструктором объектов логических значений:

```javascript
new Boolean(0) // false
new Boolean(1) // true
```

В трёхкомпонентном сравнении:

```javascript
myVar ? "truthy" : "falsy"
```

myVar проверяется в контексте логического значения.

Будьте осторожны, сравнивая 2 значения. Значения объекта (которые должны стать истинными) не считаются логическими, но конвертируются в примитивный тип данных. При сравнении объекта с логическим значением, например [] == true, он преобразуется в [].toString() == true:

```javascript
let a = [] == true // a ложно, [].toString() возвращает ""
let b = [1] == true // b истинно, [1].toString() возвращает "1"
let c = [2] == true // c ложно, [2].toString() возвращает "2"
```

# <a name="18">Анаморфизмы и катаморфизмы</a>
Анаморфизмы. Функции, с помощью которых объекты разворачиваются в более сложные структуры, содержащие объекты того же типа. Представим преобразование целого числа в ряд чисел:

```javascript
function downToOne(n) {
  const list = [];

  for (let i = n; i > 0; --i) {
    list.push(i);
  }

  return list;
}

downToOne(5)
  //=> [ 5, 4, 3, 2, 1 ]
```

Катаморфизмы. Противоположность анаморфизмов: сворачивают объекты с более сложной структурой в простые. В примере product преображает несколько чисел в одно:

```javascript
function product(list) {
  let product = 1;

  for (const n of list) {
    product = product * n;
  }

  return product;
}

product(downToOne(5)) // 120
```

# <a name="19">Генераторы</a>
Ещё один способ записать функцию downToOne — использовать генератор. Эти функции могут приостанавливать выполнение, вернуть промежуточный результат, а затем продолжить выполнение в любой момент. Чтобы инстанцировать объект-генератор, необходимо объявить функцию function *. Перепишем функцию downToOne с использованием генератора:

```javascript
function * downToOne(n) {
  for (let i = n; i > 0; --i) {
    yield i;
  }
}

[...downToOne(5)] // [ 5, 4, 3, 2, 1 ]
```

Генераторы возвращают итерируемые объекты. Функция next() выполняется до ключевого слова yield, которое возвращает значение во внешний код. Либо до функции yield *, которая передаёт её другой функции-генератору. При возврате результата return функция-генератор считается завершённой. Дальнейшие вызовы функции next() не вернут новых значений.

```javascript
// пример yield *
function * idMaker() {
  var index = 0;
  while (index < 2) {
    yield index;
    index = index + 1;
  }
}

var gen = idMaker();

gen.next().value; // 0
gen.next().value; // 1
gen.next().value; // не определено
```

Ключевое слово yield * активирует следующую функцию-генератор во время итерации:

```javascript
// пример yield *
function * genB(i) {
  yield i + 1;
  yield i + 2;
  yield i + 3;
}

function * genA(i) {
  yield i;
  yield* genB(i);
  yield i + 10;
}

var gen = genA(10);

gen.next().value; // 10
gen.next().value; // 11
gen.next().value; // 12
gen.next().value; // 13
gen.next().value; // 20
// пример возврата функции-генератора
function* yieldAndReturn() {
  yield "Y";
  return "R";
  yield "unreachable";
}

var gen = yieldAndReturn()
gen.next(); // { значение: "Y", завершено: false }
gen.next(); // { значение: "R", завершено: true }
gen.next(); // { значение: не определено, завершено: true }
```

# <a name="20">Статические методы</a>
Ключевое слово static используется в классах для определения статичных методов. Статичные методы функции, принадлежащие объекту класса, но не доступные другим объектам того же класса.

```javascript
class Repo {
  static getName() {
    return "Repo name is modern-js-cheatsheet"
  }
}

// нам не нужно создавать объект класса Repo
console.log(Repo.getName()) // "Repo name is modern-js-cheatsheet"

let r = new Repo();
console.log(r.getName()) // необработанная ошибка TypeError: r.getName не является функцией
```

Вызов статического метода из другого статического метода осуществляется с помощью ключевого слова this. Для нестатических методов этот подход не сработает:

```javascript
class Repo {
  static getName() {
    return "Repo name is modern-js-cheatsheet"
  }

  static modifyName(){
    return this.getName() + '-added-this'
  }
}

console.log(Repo.modifyName()) // название Repo — modern-js-cheatsheet-added-this
```

Вызов статического метода из нестатического метода производится двумя способами:

с использованием названия класса. Мы используем имя класса и вызываем статичный метод как свойство, например ClassName.

StaticMethodName:

```javascript
class Repo {
  static getName() {
    return "Repo name is modern-js-cheatsheet"
  }

  useName(){
    return Repo.getName() + ' and it contains some really important stuff'
  }
}

// для использования статических методов нам нужно создать класс
let r = new Repo()
console.log(r.useName()) // название Repo — modern-js-cheatsheet и в нём содержатся важные данные
С помощью конструктора. Статические методы можно вызвать как свойства объектов конструктора:
class Repo {
  static getName() {
    return "Repo name is modern-js-cheatsheet"
  }

  useName(){
    // вызывает статический метод в качестве свойства конструктора
    return this.constructor.getName() + ' and it contains some really important stuff'
  }
}

// для использования нестатических методов нам нужно создать класс
let r = new Repo()
console.log(r.useName()) // название Repo — modern-js-cheatsheet и в нём содержатся важные данные
```
