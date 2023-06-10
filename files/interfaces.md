# **Реализация Interfaces**

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

Это абстракции (порты) для описания сервайсов (адаптеров). Здесь располагается полная коллекция того, как выглядят наши API, Service и т.д, что является адаптером:

```typescript
export interface IUserApi {
  getUsers: () => Promise<TUser[]>
}

export type TUserState = {
  accessToken: string;
  refreshToken: string;
  user: TUser;
}

export interface IUserStorage extends TUserState {
  setUser: (tokens: TAuthTokens) => void;
}

export type TUserServiceConstructor = {
  api: IUserApi,
  store: IUserStorage
}

export interface IUserServiceInstance extends TUserConstructor {
  getUsers: () => Promise<TUser[]>
}
```

Соглашения:

1. Каждый интерфейс описывается отдельно, без наследования от другого интерфейса.
2. Конструкторы, Стейт стора и т.д - не интерфейсы, они относятся к сущностям, которые реализованы здесь локально как типы данных.