# **Абстракции (interfaces)**

Это абстракции для описания сервайсов. Здесь располагается полная коллекция того, как выглядят наши API, Service и т.д:

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

>
  **Соглашения:**
  - Название интерфейса начинается с префикса `I`;
  - Каждый интерфейс описывается отдельно, без наследования от другого интерфейса;
  - Конструкторы, Стейт стора и т.д - не интерфейсы, они относятся к сущностям, которые реализованы здесь локально как типы данных.
>