# **Сущности (entity)**

# **Содержание**

- [**Файловая структура модуля**](structure.md)
  - [**assets/**](assets.md)
  - [**helpers/**](helpers.md)
  - [**tests/**](tests.md)
  - [**Слой отображения (components/)**](component.md)
- [**Сущности (entity)**](entity.md)
- [**Абстракции (interfaces)**](interfaces.md)
- [**Адаптер (service)**](service.md)
- [**Глобальное хранилище**](store.md)

#

Слой сущности описывают бизнес-логику и типы данных, с которыми мы работаем внутри модуля. Сущность может быть реализована в разных форматах:

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

>
  **Соглашение:**
  `Type` начинается с префикса `T`;
>

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
>
  **Соглашение:**
  - При определении параметров `constructor` мы определяем все свойства, описанные типом `TUser` (даже если они не нужны);
  - Запрещено передавать в `constructor` сторонние свойства, не имеющих отношения к описываемой сущности. **Исключение: Зависимости от внешних пакетов, читайте дальше**
>

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