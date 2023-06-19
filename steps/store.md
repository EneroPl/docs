# **Глобальное хранилище**

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

Глобальное хранилище определяется как для текущего модуля, так и для всего проекта в целом, так что стоит обращать внимание на целесообразность вынесения каких-то данных, которые могут не иметь смысла.

Собственно, глобальное хранилище - это не что-то, где может располагаться инструкция о том, как выполнить axios запрос, распакавать его и присвоить к себе. Хранилище имеет 2 единственных задачи: Присваивать и Отдавать. Например:

```typescript
const useUserStore: IUserStorage = defineStore('user', {
  state: (): IUserState => ({
    accessToken: "",
    refreshToken: "",
    user: {},
  }),
  getters: {
    isAuth: (s: IUserStorage): boolean => {
      return !!Object.keys(s.user).length;
    },
    fullName: (s: IUserStorage): string => {
      return `${s.user.firstName} ${s.user.lastName}`
    }
  },
  setUser({ accessToken, refreshToken }) {
    const parsedToken = // что-то делаем с токенами
    this.accessToken = accessToken;
    this.refreshToken = refreshToken;
    this.user = parsedToken;
  }
})
```

>
  **Примечание**:
  Хранилище знает о том, с чем оно работает, но не более. Внутри хранилища не допускается использование репозиториев и других функций для реализации бизнес-логики.
>