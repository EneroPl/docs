# **Реализация Store**

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

Хранилище данных является местом хранения информации в глобальном, относительно модуля (или проекта), контексте.

Чего **нельзя** делать со стором:

- Навязывать стору работу бизнес-логики (см. [Service](service.md));
- Использовать для хранения состояния, которое можно хранить в `state` компонента.
- Использовать в качестве `[getter]` для вычисления состояния компонента и других слоёв (только состояния данных в хранилище)

```typescript
getters: {
  // Плохо
  isDisabledButton: (s: IStorage) => Object.keys(s.user).length > 0
  // Хорошо
  isAuth: (s: IStorage) => Object.keys(s.user).length > 0
}
```

>
  Спросите, что тут поменялось? Поменялся подход к определению `[getter]`.
>
>
  В первом случае мы использовали название для определения состояния кнопки, что выходит за рамки достимого, ибо стор не должен знать и заботиться о слоях отображения. Его задача в том, чтобы организовать хранение в контексте собственных данных.
>
>
  Во втором случае мы использовали название, которое характеризует состояния свойства из хранилища, поэтому это допустимое название для `[getter]`.
>