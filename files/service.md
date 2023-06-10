# **Реализация Service**

## **Содержание**

- [Главная](../README.md)
- [Структура модуля](../directories/README.md)
  - [**assets/**](../directories/assets.md)
  - [**helpers/**](../directories/helpers.md)
  - [**components/**](../directories/components.md)
  - [**entity/**](../directories/entity.md)
  - [**tests/**](../directories/tests.md)
- Файлы
  - [**Component**](component.md)
  - [**API**](api.md)
  - [**Entity**](entity.md)
  - [**Tests**](tests.md)
  - [**Service**](service.md)
  - [**Interfaces**](interfaces.md)
  - [**Store**](store.md)

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

## **Конструктор адаптера**

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

## **Методы адаптера**

Методы адаптера по определению схожи с понятием инструкции. Если компонент просит нас выполнить какое-то действие, то мы производим некоторые манипуляции, которые необходимы для получения и отдачи результата. В принципе, здесь не так уж и сложно:

1. Пошёл попросил данные у API's / store;
2. Если нужно, залил что-то актуальное в store;
3. Вернул компоненту то, что он ожидает

Примерно по такому абстрактному принципу будет работь каждый метод, даже если в вашем сервайсе возникнит какая-то специфика.

Например, вам необходимо каждые 5 секунд стучаться к адаптеру, чтобы узнать, что у пользователя поменялся статус учётной записи на "Подтверждённый". Как думаете, где будет реализована логика таймера? Правильно! На стороне компонента.

```vue
// Глобальный сервайс
const { $userService } = useNuxtApp();

const state = reactive({
  interval: null,
})

onBeforeUnmount(() => {
  clearInterval(state.interval)
})

const onCheckUserStatus = () => {
  state.interval = setInterval(() => {
    const user = $userService.checkUserStatus()

    if (user.status === StatusTypes.Verefied) {
      clearInterval(state.interval)
    }
  }, 5000)
}
```

```typescript
export default class UserService implements IUserServiceInstance {
  api: IUserApi;
  store: IUserStorage;
  
  constructor ({ api, store }: IUserServiceConstructor) {
    this.api = api;
    this.store = store;
  }

  async checkUserStatus(): Promise<TUser> {
    const response = await this.api.checkUserStatus();
    this.store.setUser(response)

    return response;
  }
}
```
