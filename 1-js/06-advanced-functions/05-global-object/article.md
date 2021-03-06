
# Глобальный объект

Глобальный объект предоставляет переменные и функции, доступные в любом месте программы. В основном те, что встроены в язык или среду исполнения.

В браузере он называется `window`, в Node.js — `global`, в другой среде исполнения может называться иначе.

Недавно `globalThis` был добавлен в язык как стандартизированное имя для глобального объекта, которое должно поддерживаться в любом окружении. В некоторых браузерах, например Edge не на Chromium, `globalThis` ещё не поддерживается, но легко реализуется с помощью полифила.

Ко всем свойствам глобального объекта можно обращаться напрямую:

```js run
alert("Привет");

// то же самое, что и
window.alert("Привет");
```

В браузере глобальные функции и переменные, объявленные с помощью `var`, становятся свойствами глобального объекта:

```js run untrusted refresh
var gVar = 5;

alert(window.gVar); // 5 (становится свойством глобального объекта)
```

Пожалуйста, не полагайтесь на это. Такое поведение поддерживается для совместимости. В современных проектах, использующих JavaScript-модули, такого не происходит. Мы разберём их позже в главе  [](info:modules).

Кроме того, с более современными объявлениями переменных `let` и `const` такого не происходит:

```js run untrusted refresh
let gLet = 5;

alert(window.gLet); // undefined (не становится свойством глобального объекта)
```

Если свойство настольно важное, что вы хотите сделать его доступным для всей программы, запишите его в глобальный объект напрямую:

```js run
*!*
// сделать информацию о текущем пользователе глобальной, для предоставления доступа к ней всем скриптам
window.currentUser = {
  name: "John"
};
*/!*

// где угодно в коде
alert(currentUser.name); // John

// или, если у нас есть локальная переменная с именем "currentUser",
// получим её из window явно (безопасно!)
alert(window.currentUser.name); // John
```

При этом, обычно не рекомендуется использовать глобальные переменные. Следует применять их как можно реже. Дизайн кода, при котором функция получает входные параметры и выдаёт определённый результат, чище, надёжнее и удобнее для тестирования.

## Использование полифилов

Глобальный объект можно использовать, чтобы проверить поддержку современных возможностей языка.

Например, проверить наличие встроенного объекта `Promise` (такая поддержка отсутствует в очень старых браузерах):

```js run
if (!window.Promise) {
  alert("Ваш браузер очень старый!");
}
```

Если нет (скажем, используется старый браузер), мы можем создать полифил: добавить функции, которые не поддерживаются окружением, но существуют в современном стандарте.

```js run
if (!window.Promise) {
  window.Promise = ... // собственная реализация современной возможности языка
}
```

## Итого

- Глобальный объект хранит переменные, которые должны быть доступны в любом месте программы.

    Это включает в себя как встроенные объекты, например, `Array`, так и характерные для окружения свойства, например, `window.innerHeight` -- высота окна браузера.
- Глобальный объект имеет универсальное имя -- `globalThis`.

    ...Но чаще на него ссылаются по-старому, используя имя, характерное для данного окружения, такое как `window` (браузер) и `global` (Node.js). Так как `globalThis` появился недавно, он не поддерживается в IE и Edge (не-Chromium версия), но можно использовать полифил.
- Следует хранить значения в глобальном объекте, только если они действительно глобальны для нашего проекта. И стараться свести их количество к минимуму.
- В браузерах, если только мы не используем модули, глобальные функции и переменные, объявленные с помощью `var`, становятся свойствами глобального объекта.
- Для того, чтобы код был проще и в будущем его легче было поддерживать, следует обращаться к свойствам глобального объекта напрямую, как `window.x`.
