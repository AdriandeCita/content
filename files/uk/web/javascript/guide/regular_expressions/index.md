---
title: Регулярні вирази
slug: Web/JavaScript/Guide/Regular_Expressions
tags:
  - Guide
  - Intermediate
  - JavaScript
  - Reference
  - RegExp
  - Regular Expressions
  - regex
---

{{jsSidebar("JavaScript Guide")}} {{PreviousNext("Web/JavaScript/Guide/Text_formatting", "Web/JavaScript/Guide/Indexed_collections")}}

Регулярні вирази — це патерни, які застосовуються для пошуку збігів з комбінаціями символів у рядках. У JavaScript регулярні вирази також є об'єктами. Ці патерни використовуються методами об'єкта {{jsxref("RegExp")}}: {{jsxref("RegExp.exec", "exec()")}} і {{jsxref("RegExp.test", "test()")}}, а також методами {{jsxref("String")}}: {{jsxref("String.match", "match()")}}, {{jsxref("String.matchAll", "matchAll()")}}, {{jsxref("String.replace", "replace()")}}, {{jsxref("String.replaceAll", "replaceAll()")}}, {{jsxref("String.search", "search()")}} і {{jsxref("String.split", "split()")}}.
Цей розділ описує регулярні вирази у JavaScript.

## Створення регулярного виразу

Сконструювати регулярний вираз можна одним із двох способів:

- Використавши літерал регулярного виразу, який складається з патерну, оточеного скісними рисками, як показано нижче:

  ```js
  const re = /ab+c/;
  ```

  Літерали регулярного виразу забезпечують компіляцію виразу під час завантаження сценарію.
  Якщо регулярний вираз залишається незмінним, такий варіант може покращити швидкодію.

- Або ж викликавши конструктор об'єкту {{jsxref("RegExp")}}, як показано:

  ```js
  const re = new RegExp('ab+c');
  ```

  Використання конструктора забезпечує компіляцію регулярного виразу під час виконання програми.
  Конструктор слід використовувати, якщо наперед невідомо, чи патерн буде змінюватись, або ж якщо патерн іще невідомий і його буде отримано пізніше з інших джерел (наприклад, введено користувачем).

## Написання патерну регулярного виразу

Патерн регулярного виразу складається з простих символів, як от `/abc/`, або ж з поєднання простих і спеціальних символів, як `/ab*c/` чи `/Chapter (\d+)\.\d*/`.
Останній приклад містить дужки, які використовуються як вказівка до запам'ятовування.
Зіставлення, виконане з цією частиною патерну, буде запам'ятоване для пізнішого використання, як це описано в частині [Застосування груп](/uk/docs/Web/JavaScript/Guide/Regular_Expressions/Groups_and_Ranges#zastosuvannia-hrup).

> **Примітка:** Якщо ви вже знайомі з формами регулярних виразів, зверніть увагу на [шпаргалку](/uk/docs/Web/JavaScript/Guide/Regular_Expressions/Cheatsheet), якщо потрібно швидко знайти якийсь конкретний патерн чи конструкцію.

### Застосування простих патернів

Прості патерни складаються з символів, яким треба знайти прямі відповідники. Наприклад, патерн `/abc/` збігається з комбінацією символів у рядках тільки коли трапляється саме ця послідовність – `"abc"` (всі букви разом, і саме в такому порядку). Такий збіг трапився б у двох рядках: `"Hi, do you know your abc's?"` і `"The latest airplane designs evolved from slabcraft."`. В обох випадках трапляється збіг з підрядком `"abc"`. Не існує збігів у рядку `"Grab crab"`, і хоча він містить підрядок `"ab c"`, в ньому не зустрічається точна послідовність `"abc"`.

### Застосування спеціальних символів

Якщо пошук збігу потребує чогось більшого, аніж просто прямого відповідника, як от знаходження однієї чи більше літер «b», чи знаходження пробілу, слід включати в патерн спеціальні символи. Наприклад, щоб знайти збіг для _одної літери `"a"`, після якої є нуль або більше літер `"b"`, а за ними літера `"c"`_, використовується патерн `/ab*c/`: зірочка `*` після `"b"` означає "наявність попереднього елемента в кількості 0 чи більше одиниць. В рядку `"cbbabbbbcdebc"` цей патерн покаже збіг з підрядком `"abbbbc"`.

Наступні сторінки містять списки різних спеціальних символів, розбитих на категорії, з описами та прикладами.

- [Перевірки](/uk/docs/Web/JavaScript/Guide/Regular_Expressions/Assertions)
  - : Перевірки включають межі, які позначають початки й закінчення слів та рядків, та інші патерни, котрі якимось чином вказують, що збіг можливий (включно з випереджувальними, ретроспективними та умовними виразами).
- [Класи символів](/uk/docs/Web/JavaScript/Guide/Regular_Expressions/Character_Classes)
  - : Розрізняють різні типи символів. Наприклад, розрізнення літер та цифр.
- [Групи й діапазони](/uk/docs/Web/JavaScript/Guide/Regular_Expressions/Groups_and_Ranges)
  - : Вказують групи й діапазони символів виразу.
- [Квантори](/uk/docs/Web/JavaScript/Guide/Regular_Expressions/Quantifiers)
  - : Вказують кількість символів чи виразів для збігу.
- [Екранування юнікодних полів](/uk/docs/Web/JavaScript/Guide/Regular_Expressions/Unicode_Property_Escapes)
  - : Розрізнення, засноване на властивостях символів юнікоду. Наприклад, літери верхнього та нижнього регістру, математичні та розділові знаки.

Нижче наведено єдину таблицю всіх спеціальних символів, які можна використовувати в регулярних виразах:

<table class="standard-table">
  <caption>
    Спеціальні символи в регулярних виразах.
  </caption>
  <thead>
    <tr>
      <th scope="col">Символи / конструкції</th>
      <th scope="col">Відповідна стаття</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <code>\</code>, <code>.</code>, <code>\cX</code>, <code>\d</code>,
        <code>\D</code>, <code>\f</code>, <code>\n</code>, <code>\r</code>,
        <code>\s</code>, <code>\S</code>, <code>\t</code>, <code>\v</code>,
        <code>\w</code>, <code>\W</code>, <code>\0</code>, <code>\xhh</code>,
        <code>\uhhhh</code>, <code>\uhhhhh</code>, <code>[\b]</code>
      </td>
      <td>
        <p>
          <a
            href="/uk/docs/Web/JavaScript/Guide/Regular_Expressions/Character_Classes"
            >Класи символів</a
          >
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <code>^</code>, <code>$</code>, <code>x(?=y)</code>,
        <code>x(?!y)</code>, <code>(?&#x3C;=y)x</code>,
        <code>(?&#x3C;!y)x</code>, <code>\b</code>, <code>\B</code>
      </td>
      <td>
        <p>
          <a
            href="/uk/docs/Web/JavaScript/Guide/Regular_Expressions/Assertions"
            >Перевірки</a
          >
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <code>(x)</code>, <code>(?:x)</code>, <code>(?&#x3C;Name>x)</code>,
        <code>x|y</code>, <code>[xyz]</code>, <code>[^xyz]</code>,
        <code>\<em>Number</em></code>
      </td>
      <td>
        <p>
          <a
            href="/uk/docs/Web/JavaScript/Guide/Regular_Expressions/Groups_and_Ranges"
            >Групи та діапазони</a
          >
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <code>*</code>, <code>+</code>, <code>?</code>,
        <code>x{<em>n</em>}</code>, <code>x{<em>n</em>,}</code>,
        <code>x{<em>n</em>,<em>m</em>}</code>
      </td>
      <td>
        <p>
          <a
            href="/uk/docs/Web/JavaScript/Guide/Regular_Expressions/Quantifiers"
            >Квантори</a
          >
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <code>\p{<em>UnicodeProperty</em>}</code>,
        <code>\P{<em>UnicodeProperty</em>}</code>
      </td>
      <td>
        <a
          href="/uk/docs/Web/JavaScript/Guide/Regular_Expressions/Unicode_Property_Escapes"
          >Екранування поля Unicode</a
        >
      </td>
    </tr>
  </tbody>
</table>

> **Примітка:** [Доступна також більша шпаргалка](/uk/docs/Web/JavaScript/Guide/Regular_Expressions/Cheatsheet) (що містить лише витяги зі статей окремих спецсимволів).

### Екранування

Якщо потрібно вжити будь-який спеціальний вираз буквально (наприклад, шукаючи власне символ `"*"`), його потрібно екранувати шляхом додавання зворотного скосу перед ним.
Зокрема, щоб знайти літеру `"a"`, за якою слідує `"*"`, слідом за яким літера `"b"`, треба вжити патерн `/a\*b/`: зворотний скіс «екранує» `"*"`, роблячи її просто знаком і нейтралізуючи спеціальне значення.

Аналогічно, якщо під час написання літералу регулярного виразу потрібно знайти збіг із символом скосу ("/"), слід його екранувати (інакше він означатиме кінець патерну).
Для прикладу, щоб знайти рядок "/example/", за яким слідує одна чи більше букв алфавіту, слід використати патерн `/\/example\/[a-z]+/i` — зворотна коса перед кожною косою робить їх просто символами.

Щоб знайти збіг зворотного скосу буквально, слід його екранувати.
Наприклад, щоб знайти рядок "C:\\", де «C» може бути будь-якою літерою, слід використати патерн `/[A-Z]:\\/`: перший зворотний скіс екранує наступний після нього, тож вираз шукатиме буквально одну зворотну скісну риску.

Використовуючи конструктор `RegExp` з рядковим літералом, пам'ятайте, що зворотний скіс — це екранувальний символ в рядкових літералах. І щоб застосувати його в регулярному виразі, слід спершу його екранувати на рівні рядкового літерала. `/a\*b/` та `new RegExp("a\\*b")` створюють один і той самий вираз, який шукатиме літеру "a" з символом за нею "\*" і літерою "b" після них.

Якщо екранувальні рядки ще не входять в готовий патерн, їх можна додати за допомогою {{jsxref('String.replace')}}:

```js
function escapeRegExp(string) {
  return string.replace(/[.*+?^${}()|[\]\\]/g, '\\$&'); // $& означає цілий рядок, який збігся з патерном
}
```

Літера "g" після регулярних виразів — це опція або позначка, що виконує глобальний пошук, перебираючи рядок цілком і повертаючи всі можливі збіги.
Це пояснено в деталях нижче, в розділі [Поглиблений пошук з позначками](#pohlyblenyi-poshuk-z-poznachkamy).

_Чому це не вбудовано всередину JavaScript?_ Існує [пропозиція](https://github.com/tc39/proposal-regex-escaping) додати таку функцію до RegExp.

### Застосування дужок

Дужки навколо будь-якої частини патерну регулярного виразу призводять до того, що ця частина знайденого підрядка буде збережена окремо. До збереженого підрядка можна потім знову звернутися і використати для інших цілей. Більше деталей є в розділі [Групи та діапазони](/uk/docs/Web/JavaScript/Guide/Regular_Expressions/Groups_and_Ranges#vykorystannia-hrup).

## Застосування регулярних виразів у JavaScript

Регулярні вирази з методами об'єкту {{jsxref("RegExp")}}, наприклад, {{jsxref("RegExp/test", "test()")}} і {{jsxref("RegExp/exec", "exec()")}}, а також із методами {{jsxref("String")}}: {{jsxref("String/match", "match()")}}, {{jsxref("String/replace", "replace()")}}, {{jsxref("String/search", "search()")}} і {{jsxref("String/split", "split()")}}.

| Метод                                           | Опис                                                                                                                |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| {{jsxref("RegExp.exec", "exec()")}}             | Виконує пошук збігу в рядку. Повертає масив з інформацією або `null`, якщо збігів не знайдено.                      |
| {{jsxref("RegExp.test", "test()")}}             | Перевіряє рядок на збіг. Повертає `true` або `false`.                                                               |
| {{jsxref("String.match", "match()")}}           | Повертає масив, що містить всі знайдені збіги, включно з групами захоплення, або ж `null`, якщо збігів не знайдено. |
| {{jsxref("String.matchAll", "matchAll()")}}     | Повертає ітератор, що містить всі знайдені збіги, включно з групами захоплення.                                     |
| {{jsxref("String.search", "search()")}}         | Перевіряє рядок на наявність збігів. Повертає індекс збігу, або `-1`, якщо пошук завершився невдачею.               |
| {{jsxref("String.replace", "replace()")}}       | Виконує пошук збігу в рядку і заміняє знайдену частину рядка переданим рядком для заміни.                           |
| {{jsxref("String.replaceAll", "replaceAll()")}} | Виконує пошук всіх збігів у рядку і заміняє знайдені частини рядка переданим рядком для заміни.                     |
| {{jsxref("String.split", "split()")}}           | Використовує регулярний вираз або фіксований рядок, щоб розбити початковий рядок на масив підрядків.                |

Якщо потрібно визначити, чи патерн присутній в рядку, використовуйте методи `test()` або `search()`. Для отримання докладнішої інформації (вкупі з повільнішим виконанням) застосовуйте методи `exec()` чи `match()`.
Якщо використовується метод `exec()` чи `match()`, і знаходиться збіг, ці методи повертають масив і оновлюють властивості пов'язаного об'єкта регулярного виразу, а також властивості глобального об'єкта `RegExp`. Якщо збіг знайти не вдалося, то метод `exec()` поверне `null` (який, зрештою, зводиться до `false`).

В наступному прикладі сценарій шукає збіги в рядку за допомогою методу `exec()`.

```js
const myRe = /d(b+)d/g;
const myArray = myRe.exec('cdbbdbsbz');
```

А в цьому сценарії — альтернативний варіант створення масиву `myArray`, на випадок якщо вам не потрібен доступ до властивостей самого регулярного виразу:

```js
const myArray = /d(b+)d/g.exec('cdbbdbsbz');
// так само, як і з "cdbbdbsbz".match(/d(b+)d/g); однак,
// "cdbbdbsbz".match(/d(b+)d/g) виводить [ "dbbd" ]
// /d(b+)d/g.exec('cdbbdbsbz') виводить [ 'dbbd', 'bb', index: 1, input: 'cdbbdbsbz' ]
```

(Дивіться більше інформації цю різницю в поведінці у частині [Застосування позначки глобального пошуку з `exec()`](#zastosuvannia-poznachky-hlobalnoho-poshuku-z-exec).)

Якщо ж потрібно сконструювати регулярний вираз із рядка, ось іще одна альтернатива:

```js
const myRe = new RegExp('d(b+)d', 'g');
const myArray = myRe.exec('cdbbdbsbz');
```

В цих сценаріях пошук збігів успішно завершується, повертає масив результатів і оновлює властивості, наведені в таблиці нижче:

<table class="standard-table">
  <caption>
    Результати виконання регулярних виразів.
  </caption>
  <thead>
    <tr>
      <th scope="col">Об'єкт</th>
      <th scope="col">Властивість або індекс</th>
      <th scope="col">Опис</th>
      <th scope="col">В цьому прикладі</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="4"><code>myArray</code></td>
      <td></td>
      <td>Рядок, що збігся, а також всі запам'ятовані підрядки.</td>
      <td><code>['dbbd', 'bb', index: 1, input: 'cdbbdbsbz']</code></td>
    </tr>
    <tr>
      <td><code>index</code></td>
      <td>Індекс позиції (від 0) збігу у вхідному рядку.</td>
      <td><code>1</code></td>
    </tr>
    <tr>
      <td><code>input</code></td>
      <td>Вхідний рядок.</td>
      <td><code>'cdbbdbsbz'</code></td>
    </tr>
    <tr>
      <td><code>[0]</code></td>
      <td>Символи з останнього збігу.</td>
      <td><code>'dbbd'</code></td>
    </tr>
    <tr>
      <td rowspan="2"><code>myRe</code></td>
      <td><code>lastIndex</code></td>
      <td>Індекс, з якого почнеться пошук наступного збігу.
        (Ця властивість встановлюється лише якщо регулярний вираз використовує опцію «g», описану в частині
        <a href="#pohlyblenyi-poshuk-z-poznachkamy">Поглиблений пошук з позначками</a>.)
      </td>
      <td><code>5</code></td>
    </tr>
    <tr>
      <td><code>source</code></td>
      <td>
        Текст патерну. Оновлюється в момент створення регулярного виразу, не виконання.
      </td>
      <td><code>'d(b+)d'</code></td>
    </tr>
  </tbody>
</table>

Як було показано у другому варіанті цього прикладу, можна використовувати регулярні вирази, створені об'єктним конструктором без присвоєння його змінній. Однак у цьому випадку кожний новий виклик створюватиме новий регулярний вираз. З цієї причини, якщо ця форма використовується без присвоєння виразу змінній, неможливо потім отримати доступ до властивостей цього виразу.
Наприклад, припустімо, у нас є такий сценарій:

```js
const myRe = /d(b+)d/g;
const myArray = myRe.exec('cdbbdbsbz');
console.log('Поле lastIndex має значення ' + myRe.lastIndex);

// "Поле lastIndex має значення 5"
```

Однак, якщо натомість у нас є такий сценарій:

```js
const myArray = /d(b+)d/g.exec('cdbbdbsbz');
console.log('Поле lastIndex має значення ' + /d(b+)d/g.lastIndex);

// "Поле lastIndex має значення 0"
```

Вирази `/d(b+)d/g` у двох інструкціях вище — це насправді різні об'єкти регулярного виразу, і тому вони містять різні значення їхньої властивості `lastIndex`. Якщо потрібно доступитися до властивостей регулярного виразу, створеного об'єктним конструктором, слід спершу присвоїти його змінній.

### Поглиблений пошук з позначками

Регулярні вирази мають необов'язкові опції, що дає змогу використовувати таку функціональність, як глобальний пошук чи пошук без врахування регістру літер. Ці опції можна використовувати окремо або разом, у будь-якому порядку, і вони входять в сам регулярний вираз.

| Прапорець | Опис                                                                                                                                  | Відповідна властивість                    |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------- |
| `d`       | Згенерувати індекси для збігів підрядків.                                                                                             | {{jsxref("RegExp.prototype.hasIndices")}} |
| `g`       | Глобальний пошук.                                                                                                                     | {{jsxref("RegExp.prototype.global")}}     |
| `i`       | Пошук, нечутливий до регістру.                                                                                                        | {{jsxref("RegExp.prototype.ignoreCase")}} |
| `m`       | Багаторядковий пошук.                                                                                                                 | {{jsxref("RegExp.prototype.multiline")}}  |
| `s`       | Дає `.` змогу позначати символи початку нового рядка.                                                                                 | {{jsxref("RegExp.prototype.dotAll")}}     |
| `u`       | "unicode"; сприймає патерн як послідовність кодів юнікоду.                                                                            | {{jsxref("RegExp.prototype.unicode")}}    |
| `y`       | Виконує «липкий» пошук, який починає пошук збігів з поточної позиції цільового рядка. Дивіться {{jsxref("RegExp.sticky", "sticky")}}. | {{jsxref("RegExp.prototype.sticky")}}     |

Щоб включити позначку в регулярний вираз, використовується такий синтаксис:

```js
const re = /pattern/flags;
```

або

```js
const re = new RegExp('pattern', 'flags');
```

Зауважте, що ці опції — це невідокремна частина регулярного виразу. Їх неможливо додати чи прибрати потім.

Наприклад, `re = /\w+\s/g` створює регулярний вираз, що шукає одну або більше літер, за якими слідує пробіл, і він виконує цей пошук по всьому рядку.

```js
const re = /\w+\s/g;
const str = 'fee fi fo fum';
const myArray = str.match(re);
console.log(myArray);

// ["fee ", "fi ", "fo "]
```

Можна замінити цей рядок:

```js
const re = /\w+\s/g;
```

на такий:

```js
const re = new RegExp('\\w+\\s', 'g');
```

і отримати такий самий результат.

Позначка `m` використовується для вказання, що багаторядковий вхідний текст має сприйматись як декілька різних рядків. Якщо використано прапорець `m`, символи `^` і `$` позначають відповідно початок та кінець кожного рядка всередині вхідної стрічки замість початку і кінця стрічки в цілому.

#### Застосування позначки глобального пошуку з exec()

Метод {{jsxref("RegExp.prototype.exec()")}} із позначкою `g` ітеративно поверне кожен збіг та його позицію.

```js
const str = 'fee fi fo fum';
const re = /\w+\s/g;
console.log(re.exec(str)); // ["fee ", index: 0, input: "fee fi fo fum"]
console.log(re.exec(str)); // ["fi ", index: 4, input: "fee fi fo fum"]
console.log(re.exec(str)); // ["fo ", index: 7, input: "fee fi fo fum"]
console.log(re.exec(str)); // null
```

Натомість метод {{jsxref("String.prototype.match()")}} поверне усі збіги відразу, втім, без їх позицій.

```js
console.log(str.match(re)); // ["fee ", "fi ", "fo "]
```

#### Використання юнікодних регулярних виразів

Прапорець "u" використовується для створення "юнікодних" регулярних виразів; це такі регулярні вирази, котрі підтримують пошук збігів у юнікодному тексті. Це здебільшого досягнуто шляхом використання [екранування юнікодних властивостей](/uk/docs/Web/JavaScript/Guide/Regular_Expressions/Unicode_Property_Escapes), котрі підтримуються лише в "юнікодних" регулярних виразах.

Наприклад, наступний регулярний вираз може застосовуватися для пошуку збігу в довільному юнікодному "слові":

```js
/\p{L}*/u;
```

Є низка інших відмінностей між юнікодними та неюнікодними регулярними виразами, про котрі слід пам‘ятати:

- Юнікодні регулярні вирази не підтримують так звані "ідентичні екранування", тобто такі патерни, де зворотна скісна риска екранування не потрібна та фактично ігнорується. Наприклад, `/\a/` – дійсний регулярний вираз, що має збіг із літерою 'a', натомість `/\a/u` – ні.

- Фігурні дужки повинні бути екрановані, коли використовуються не як [квантори](/uk/docs/Web/JavaScript/Guide/Regular_Expressions/Quantifiers). Наприклад, `/{/` – дійсний регулярний вираз, що має збіг з фігурною дужкою '{', а `/{/u` – ні: натомість фігурна дужка повинна бути екранована, слід використовувати `/\\{/u`.

- Символ `-` в класах символів інтерпретується інакше. А саме – в юнікодних регулярних виразах `-` інтерпретується як літерал `-` (а не частина діапазону) лише коли зустрічається на початку чи кінці патерну. Наприклад, `/[\w-:]/` – дійсний регулярний вираз, що має збіг із символом слова, `-` чи `:`, однак `/\w-:/u` – недійсний регулярний вираз, адже діапазон символів від `\w` до `:` не є однозначно визначеним.

## Приклади

> **Примітка:** Декілька прикладів також доступні:
>
> - На довідникових сторінках для {{jsxref("RegExp.exec", "exec()")}}, {{jsxref("RegExp.test", "test()")}}, {{jsxref("String.match", "match()")}}, {{jsxref("String.matchAll", "matchAll()")}}, {{jsxref("String.search", "search()")}}, {{jsxref("String.replace", "replace()")}}, {{jsxref("String.split", "split()")}}
> - У статтях цих настанов: [класи символів](/uk/docs/Web/JavaScript/Guide/Regular_Expressions/Character_Classes), [перевірки](/uk/docs/Web/JavaScript/Guide/Regular_Expressions/Assertions), [групи та діапазони](/uk/docs/Web/JavaScript/Guide/Regular_Expressions/Groups_and_Ranges), [квантори](/uk/docs/Web/JavaScript/Guide/Regular_Expressions/Quantifiers), [екранування полів Unicode](/uk/docs/Web/JavaScript/Guide/Regular_Expressions/Unicode_Property_Escapes)

### Застосування спеціальних символів для перевірки вводу

В цьому прикладі користувач має ввести телефонний номер. Коли користувач натискає кнопку "Check", сценарій перевіряє правильність номера. Якщо номер коректний (збігається з послідовністю символів, заданою регулярним виразом), то сценарій показує повідомлення з подякою користувачеві та підтвердженням номеру. Якщо номер некоректний, то сценарій інформує користувача про те, що номер введено невірно.

Регулярний вираз шукає:

1. Початок рядку з даними: `^`
2. Далі три цифрових знаки `\d{3}` АБО `|` ліва дужка `\(` з трьома цифрами за нею `\d{3}`, закритими правою дужкою `\)`, і це все об'єднано в групу (без захоплення) `(?:)`
3. Далі один дефіс, скісна риска або десяткова крапка, що захоплено в групу `()`
4. Далі іще три цифри `\d{3}`
5. Далі збіг, запам'ятований у першій захопленій групі `\1`
6. Далі іще чотири цифри `\d{4}`
7. І за ними кінець рядка з даними: `$`

Подія `click`, котра спрацьовує під час натискання користувачем клавіші <kbd>Enter</kbd>, встановлює значення `phoneInput.value`.

#### HTML

```html
<p>
  Введіть ваш номер телефону (з кодом населеного пункту) та натисність
  "Перевірити".
  <br />
  Очікуваний формат номера: ###-###-####.
</p>
<form id="form">
  <input id="phone" />
  <button type="submit">Перевірити</button>
</form>
<p id="output"></p>
```

#### JavaScript

```js
const form = document.querySelector('#form');
const input = document.querySelector('#phone');
const output = document.querySelector('#output');
const re = /^(?:\d{3}|\(\d{3}\))([-\/\.])\d{3}\1\d{4}$/;
function testInfo(phoneInput) {
  const ok = re.exec(phoneInput.value);
  if (!ok) {
    output.textContent = `${phoneInput.value} не є телефонним номером із кодом населеного пункту!`;
  } else {
    output.textContent = `Дякую, ваш номер телефону – ${ok[0]}`;
  }
}
form.addEventListener('submit', (event) => {
  event.preventDefault();
  testInfo(input);
});
```

#### Результат

{{ EmbedLiveSample('Using_special_characters_to_verify_input') }}

## Інструменти

- [RegExr](https://regexr.com/)
  - : Онлайн-інструмент для вивчення, побудови та тестування регулярних виразів.
- [Regex tester](https://regex101.com/)
  - : Онлайн-інструмент для побудови та зневадження регулярних виразів
- [Regex interactive tutorial](https://regexlearn.com/)
  - : Онлайнові інтерактивні підручники, шпаргалка та ігровий майданчик.
- [Regex visualizer](https://extendsclass.com/regex-tester.html)
  - : Візуальний онлайн-інструмент для тестування регулярних виразів.

{{PreviousNext("Web/JavaScript/Guide/Text_formatting", "Web/JavaScript/Guide/Indexed_collections")}}
