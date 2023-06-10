# **Реализация Entity**

## **Содержание**

- [Главная](README.md)
- [Структура модуля](../directories/README.md)
  - [**assets/**](../directories/assets.md)
  - [**helpers/**](../directories/helpers.md)
  - [**components/**](../directories/components.md)
  - [**entity/**](../directories/entity.md)
  - [**tests/**](../directories/tests.md)
- [Файлы](README.md)
  - [**Component**](component.md)
  - [**API**](api.md)
  - [**Entity**](entity.md)
  - [**Tests**](tests.md)
  - [**Service**](service.md)
  - [**Interfaces**](interfaces.md)
  - [**Store**](store.md)

#

Слой сущности описывают бизнес-логику и типы данных, с которыми мы работаем внутри модуля. Как писалось в [**Директории entity/**](directories/entity.md), у нас есть 4 стула, на которые можно сесть:

1. Типа - описание данных;
2. Enums - описание объектов;
3. Переменные - константы;
4. Классы - реализация сущности.

## **Типы `types`**

```typescript
export type TUser = {
  firstName: string;
  lastName: string;
}
```

Соглашения:

1. `Type` начинается с префикса `T`;

## **Типы `Enums`**

```typescript
export enums StepTypes {
  INSTALL = "install",
  UPLOAD = "upload"
}
```

## **Переменные**

```typescript
export const AVAIABLE_FORMATS = ['.jpg', '.png']
```

## **Классы**

Классы реализуют сущность (к примеру, `TUser`), но каким образом? Когда мы работаем с сущностью в компонентах, то часто используем какие-то getters/methods, которые зачастую являются чистыми функциями и возвращают результат манипуляций с объектом сущности.

Обычно такая логика находится либо в `service.ts`, либо в `store.ts`. Однако, `Entity` позволяет нам избавиться от излишней логики в вышеупомянутых местах и разместить её реализацию внутри сущности:

```typescript
export class UserEntity  {
  firstName: string;
  lastName: string;
  
  constructor ({ firstName, lastName }: TUser) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  getFullName(): string {
    return `${this.firstName} ${this.lastName}`;
  }
}
```

Соглашение:

1. При определении параметров `constructor` мы определяем все свойства, описанные типом `TUser` (даже если они не нужны).
2. Запрещено передавать в `constructor` сторонние свойства, не имеющих отношения к описываемой сущности. **Исключение: Зависимости от внешних пакетов, читайте дальше**

## **Зависимости**

Бывает, что для форматирования мы используем какие-либо сторонние пакеты, по типу `moment`. В таком случае допускается инициалзиация конструктора со сторонним свойством в качестве зависимости класса.

```typescript
type TUser = {
  firstName: string;
  lastName: string;
  createdAt: number;
}

type TUserEntityConstructor = TUser & {
  moment: Moment;
}

export class UserEntity  {
  firstName: string;
  lastName: string;
  createdAt: number;
  moment: Moment;
  
  constructor ({ firstName, lastName, createdAt, moment }: TUserEntityConstructor) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.createdAt = createdAt;
    this.moment = moment;
  }

  getFullName(): string {
    return `${this.firstName} ${this.lastName}`;
  }

  getCreatedDate() {
    return this.moment.unix(this.createdAt).format("DD/MM/YYYY")
  }
}
```

Также, помимо вышеупомянутой ситуции, допускается использование импорта пакета в файл сущности, с гарантией того, что в течении ближайших 180 лет этот пакет не поменяется, но всё же стоит избегать подобных кейсов.