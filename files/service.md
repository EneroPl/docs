# **Реализация Service**

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

Наш адаптер, который решала всех вопросов и помошник слою отображения. В нём реализована вся работа с бизнес-логикой. Пример инициализации адаптера:

```typescript
export default class UserService implements IUserServiceInstance {
  api: IUserApi;
  store: IUserStorage;
  
  constructor ({ api, store }: IUserServiceConstructor) {
    this.api = api;
    this.store = store;
  }

  async getUsers(): Promise<TUser[]> {
    return await this.api.getUsers();
  }
}
```

Адаптер, как и сущность, не имеет возможности хранить на уровне файла какие-либо внешние пакеты, поэтому они определяются в `constructor` в качестве зависимости как `entity` (см. [**Entity**](files/entity.md)).

Также, адаптер не умеет хранить в себе промежуточные данные (просто у него лапки). Поэтому, **НЕ** допускается использования `constructors` в качестве хранилища промежуточных данных\состояний. Абсолютно по этой же причине **НЕ** допускается хранение состояния компонентов на уровне адаптера.

```typescript
// Плохо. В конструкторе какие-то данные о состоянии.
export default class UserService implements IUserServiceInstance {
  api: IUserApi;
  store: IUserStorage;
  
  constructor ({ api, store }: IUserServiceConstructor) {
    this.api = api;
    this.store = store;
    this.isLoading = false; <-- НЕЛЬЗЯ
  }
}
```

Чтобы обеспечить компонента данными таким образом, чтобы у вас не возникали вопросы по типу: "А где тогда определить стейт, если это мне нужно в нескольких местах?" Ответ: "На странице".