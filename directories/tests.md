# **Директория `tests/`**

## **Содержание**

- [Главная](README.md)
- [Структура модуля](directories/README.md)
  - [**assets/**](directories/assets.md)
  - [**helpers/**](directories/helpers.md)
  - [**components/**](directories/components.md)
  - [**entity/**](directories/entity.md)
  - [**tests/**](directories/tests.md)
- [Файлы](files/README.md)
  - [**Component**](files/component.md)
  - [**API**](files/api.md)
  - [**Entity**](files/entity.md)
  - [**Tests**](files/tests.md)
  - [**Service**](files/service.md)
  - [**Interfaces**](files/interfaces.md)
  - [**Store**](files/store.md)

#

В этой директории располагаются unit/component тесты для текущего модуля. Пример директории с произвольным файлом `%name%` внутри.

```yml
- tests/
  - %name%.spec.ts
```

Соглашения по работе с директорией:

1. Формат именования имени unit-теста `camelCase`;
2. Формат именования component-теста `PascalCase`;
3. Формат файла не имет строгих рамок, так как там особо и нет как разгуляться, есть `.spec.ts`, а есть `.test.ts` ну и всё в принципе.

Примеры:

```
- tests/
  - useCase.test.ts <- unit-test
  - UseCase.test.ts <- component-test
```