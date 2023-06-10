# **Директория `entity/`**

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

В большинстве случаев эта директория будет входной точкой для любого модуля. В этой директории хранятся типы, конфиги, классы для работы с текущей сущностью.

Каждая сущность реализуется независимо от другой. То есть, например, если нам нужно описать сущность для "Пользователя" и "Настройки безопасности", то они должны описываться в отдельных файлах.

```yml
- entity/
    - user.ts
    - security.ts
```

При инициализации сущности, в вашем файле могут находится несколько вараинтов описания сущности (подробнее см. [**Сущности**](../files/entity.md)):

1. Описание типов;
2. Константы/Словарики/Enums;
3. Класс для функционального общения с сущностью.

```typescript
// user.ts
export type TUser = {
    userId: number;
    firstName: string;
    lastName: string;
}

// Ещё какая-то хуйня
export type TEditUserName = Omit<TUser, 'userId'>

// И какие-нибудь константы
export const USER_OPTIONS = {}

// И какой-то функциональный класс для реализации сущности
export class UserEntity {}
```

Соглашения по реализации сущностей:

1. **НЕ** докускается использовать один файл для реализации типов семантически разных сущностей или хранить данные, которые не относятся к описываемой сущности;
2. **НЕ** допускается использовать конструкцию `declare module` для описания сущности;
3. Названия файлов должны именоваться в формате `camelCase`, а сам формат файла - `.ts` (**НЕ** `.d.ts`, a `.ts`).
