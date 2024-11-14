# Tolk vs FunC: коротко

Tolk гораздо больше похож на TypeScript и Kotlin, чем на C и Lisp.
Но он все еще дает вам полный контроль над сборщиком TVM, так как он имеет ядро FunC внутри.

1. Функции декларируются через `fun`, гет-методы через `get`, переменные через `var` (и `val` для неизменяемых), типы указываются справа; типы параметров обязательны; тип возвращаемого значения может быть опущен (автоматически определяется), а также для локальных переменных; спецификаторы 'inline' и другие являются атрибутами `@`

```tolk
global storedV: int;

fun parseData(cs: slice): cell {
    var flags: int = cs.loadMessageFlags();
    ...
}

@inline
веселая сумма(a: int, b: int) { // auto inferred int
    val both = a + b; // то же самое
    обратно;
}

get currentCounter(): int { ... }
```

2. Нет `impure`, он по умолчанию, компилятор не отменяет вызовы пользовательских функций
3. Вместо `recv_internal` и `recv_external`, используется `onInternalMessage` и `onExternalMessage`
4. `2+2` это 4, а не идентификатор; идентификаторы являются алфавитно-цифровыми; используйте имя `const OP_INCREASE` вместо `const op::increase`
5. Логические операторы AND `&&`, OR `||`, NOT `!` поддерживаются
6. Улучшение синтаксиса:
   - `;; комментарий` → \`// комментарий"
   - `{- комментарий -}` → `/* комментарий */`
   - `#include` → `import`, со строгим правилом "импортировать то, что вы используете"
   - `~ found` → `!found` (только для true/false, разумеется) (истина -1, как в FunC)
   - `v = null()` → `v = null`
   - `null?(v)` → `v == null`, то же самое для `builder_null?` и других
   - `~ null?(v)` → `c !=null`
   - `throw(excNo)` → `throw excNo`
   - `catch(_, _)` → `catch`
   - `catch(_, excNo)` → `catch(excNo)`
   - `throw_unless(excNo, cond)` → `assert(cond, excNo)`
   - `throw_if(excNo, cond)` → `assert(!cond, excNo)`
   - `return ()` → `return`
   - `do ... until (cond)` → `do ... пока (!cond)`
   - `elseif` → `else if`
   - `ifnot (cond)` → `if (!cond)`
7. Функцию можно вызывать, даже если она объявлена ниже; предварительные декларации не требуются; компилятор сначала анализирует, а затем делает вывод символов; теперь AST предоставляет исходный код
8. stdlib функции переименованы в ~~verbose~~ понятное именование, camelCase стиль; теперь он встроен, а не загружен с GitHub; он разделен на несколько файлов; общие функции доступны, более конкретные с помощью `import "@stdlib/tvm-dicts"`, IDE предложит вам; вот [соответствие](/v3/documentation/smart-contracts/tolk/tolk-vs-func/stdlib)
9. Нет методов тильды `~`; `cs.loadInt(32)` изменяет маску и возвращает целое число; `b.storeInt(x, 32)` изменяет билдер; `b = b.storeInt()` также работает, поскольку он не только модифицирует, но и возвращает значение; комбинированные методы работают идентично с JS, они возвращают «self»; все работает точно так же, как ожидается, как и в JS; нет накладных расходов, точно как инструкции в Fift; пользовательские методы с легкостью создаются; в Tolk вообще не существует тильды `~`; [подробнее здесь](/v3/documentation/smart-contracts/tolk/tolk-vs-func/mutability)

#### Инструменты окружения

- Плагин JetBrains создан
- Расширение VS Code [exists](https://github.com/ton-blockchain/tolk-vscode)
- WASM обертка для Blueprint [создана](https://github.com/ton-blockchain/tolk-js)
- И даже конвертер из FunC в Tolk [имеется](https://github.com/ton-blockchain/convert-funct--to-tolk)

#### Читать дальше

[Tolk vs FunC: подробнее](/v3/documentation/smart-contracts/tolk/tolk-vs-func/in-detail)
