# **Директория tests/**

# **Содержание**

- [**Файловая структура модуля**](structure.md)
  - [**assets/**](assets.md)
  - [**helpers/**](helpers.md)
  - [**tests/**](tests.md)
  - [**Слой отображения (components/)**](component.md)
- [**Сущности (entity)**](entity.md)
- [**Абстракции (interfaces)**](interfaces.md)
- [**Адаптер (service)**](service.md)
- [**Репозиторий (API)**](api.md)
- [**Глобальное хранилище**](store.md)

#

Целью тестирования для разработчика является фиксация ожидаемого и фактического результатов на основе спецификации или макета от дизайнера, в зависимости от направления тестирования.

>
  **Примечание:**
  Каждый из проведённых тестов должен быть независим от результата выполнения других тестов.
>

## **Тест-комплект**

Это такая хуйня, которая определяет внутри себя набор *тест-кейсов* по некому объекту тестирования, например "формы авторизации пользователя".

В виде кода это определяется примерно так:

```javacript
describe("auth form authorization", () => {
  // test cases
})
```
>
  **Примечание:**
  Тест-комплект является единственной входной точкой для "объекта тестирования" внутри файла. 
>

## **Тест-кейс**

Это проверка одного или более ожидаемых результатов. Пример структуры тест-кейса:

```typescript
import { describe, test } from 'jest'

describe('test authorization form', () => {
  test("success validation password", () => {
    // step
  })

  test("incorrect validation password", () => {
    // step
  })
})
```

# **Файловая структура**

Директория для хранения/выполнения тестов текущего модуля.

```yml
- myModule/
  - tests/
    service.spec.ts
    MyTest.spec.ts
```

>
  **Примечание:** Тесты могут быть написаны в суффиксе как `.spec.ts`, так и в `.test.ts`. Это формат одного и того же описания.
>

Внутри нашего `myModule` модуля хранятся несколько файлов, которые несколько отличаются друг от друга - один файл с **З**аглавной буквы, а другой нет.

## **Component тесты**

Возвращаясь к нашему утверждению на счёт **З**аглавной буквы, то почему же так назван файл? Ответ прост: Это файл с тестами слоя отображения, внутри которого определены тест-кейсы визуальных состояний и функционала компонента.

>
 **Примечание:** Если мы тестируем компонент, то файл должен называться также, как и называется компонент, с **З**аглавной буквы. Это явно говорит о целях тестирования.
>

## **Unit тесты**

Юнит тесты, в отличии от компонентов, не имеют прямой привязки к названию файла, поэтому в зивисимости от цели тестирования файл может называться по разному, но в формате `camelCase`.

>
 **Примечание:** Если мы пишем `Unit тесты`, то именование файлов должно быть в формате `camelCase`.
>