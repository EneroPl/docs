# **Реализация Tests**

## **Содержание**

- [Главная](README.md)
- [Структура модуля](../directories/README.md)
  - [**assets/**](../directories/assets.md)
  - [**helpers/**](../directories/helpers.md)
  - [**components/**](../directories/components.md)
  - [**entity/**](../directories/entity.md)
  - [**tests/**](../directories/tests.md)
Файлы
  - [**Component**](component.md)
  - [**API**](api.md)
  - [**Entity**](entity.md)
  - [**Tests**](tests.md)
  - [**Service**](service.md)
  - [**Interfaces**](interfaces.md)
  - [**Store**](store.md)

#

Тесты важны для автоматизации рабского тыкания по кнопке или валидации метода каждый раз, когда мы меняем логику поведения модуля, особенно если изменения достаточно масштабные.

Тесты бывают разными, но способ их описания сводится к более менее единому паттерну: **тест-комплекты** и **тест-кейсы**.

## **Тест-комплекты**

Это блок, который состоит из набора тест-кейсов. Всякий раз, когда мы говорим о том, что нам необходимо протестировать "Такой-то блок на такой-то странице", то мы говорим как-раз таки о тест-комплекте. Выглядит он примерно так:

```typescript
import { describe } from 'jest'

describe('test authorization form', () => {
  // test cases
})
```

Соглашения по работе с тест-комплектами:

1. Каждый тест-комплект должен быть реализован в отдельном файле (т.е никога не будет такой ситуации, когда у вас в 1-м файле есть несколько `describe`).

## **Тест-кейсы**

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

Как и в случае с тест-комплектами, тест-кейсы должны быть независимы друг от друга. Другими словами, мы не должны полагаться на то, что тест "success validation password" создаст благоприятые условия для "incorrect validation password". Они должны быть независимы от порядка выполнения и всего что ещё в качестве зависимости можно придумать.

Соглашения по работе с тест-кейсами:

1. Независимость тест-кейса от других тест-кейсов и тест-комплекта;
2. Тест-кейс должен быть описан максимально просто и понятно.

## **Шаги**

Это проверка внутри тест-кейса на соответствие фактического и ожидаемого результата. Этих проверок может быть бесконечное кол-во, если они соответствуют целям тест-кейса.

```typescript
import { describe, test, expect } from 'jest'
import { AuthService } from 'service.ts'
import { authMockApi } from 'api.ts'

describe('test authorization form', () => {
  test("success signIn", () => {
    const authService = new AuthService({
      api: authMockApi
    })

    expect(authService().login({
      login: "username",
      password: "qwerty123"
    })).toMathObject(['accessToken', 'refreshToken'])
  })

  test("incorrect signIn", () => {
    const authService = new AuthService({
      api: authMockApi
    })

    expect(authService().login({
      login: null,
      password: "qwerty123"
    })).toThrowError("Invalid username")

    expect(authService().login({
      login: "username",
      password: null
    })).toThrowError("Invalid password")

    expect(authService().login({
      login: null,
      password: null
    })).toThrowError("Invalid login & password")
  })
})
```

Соглашения по работе с шагами:

1. Каждый шаг должен выполнять одну инструкцию;
2. Шаг должен выполнять проверки в соответствии с целями тест-кейса.

## **Оптимизация пространства**

В первую очередь вы пишите тесты не для клиента, а для себя, поэтому важно создавать благоприятные условия для того, чтобы другие разработчики понимали, что вообще происходит в ваших тестах. Достич такого понимания можно, используя методы/константы, вынесенные за пределы тест-комплекта.