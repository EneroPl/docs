# **Директория `components/`**

## **Содержание**

- [Главная](../README.md)
- [Структура модуля](README.md)
  - [**assets/**](assets.md)
  - [**helpers/**](helpers.md)
  - [**components/**](components.md)
  - [**entity/**](entity.md)
  - [**tests/**](tests.md)
- Файлы
  - [**Component**](../files/component.md)
  - [**API**](../files/api.md)
  - [**Entity**](../files/entity.md)
  - [**Tests**](../files/tests.md)
  - [**Service**](../files/service.md)
  - [**Interfaces**](../files/interfaces.md)
  - [**Store**](../files/store.md)

#

Хранилище компонентов, которые необходимы для реализации текущего модуля.

```yml
- modules/
    - %module_name%/
        - components/
            - Items/
                - Item.vue
                - index.vue
            - MyComponent.vue
```

Соглашения по работе с директорией:

1. Название директорий **должно** быть в формате `camelCase`, если внутри нет компонента `index.vue`, иначе `PascalCase`.

```yml
- components/
    // Плохо
    - exampleComponent/
        - index.vue

    // Хорошо
    - ExampleComponent/
        - index.vue
```

2. Если компонент ссылается на сложный компонент, разбитый на несколько частей (например, `ItemCollection.vue` и `Item.vue`), то они **обязаны** находиться в внутри директории:

```yml
// Плохо
- components/
    - MyComponent.vue
    - ItemCollection.vue
    - Item.vue

// Хорошо
- components/
    - ItemCollection/
        - index.vue
        - Item.vue
    - MyComponent.vue
```

>
    UPD: Так становится понятна структура, из которой состоит какой-либо компонент, разбитый на несколько компонентов.
>

3. Если компонент реализует модальное окно, то **должен** храниться в директории `modals/`:

```yml
// Плохо
- components/
    - MyComponent.vue
    - ConfirmExampleModal.vue
    - EditExampleModal.vue
    - AddExampleModal.vue

// Хорошо
- components
    - modals/
        - index.ts // экспорты модалок
        - ConfirmExample.vue
        - EditExample.vue
        - AddExample.vue
    - MyComponent.vue
```

4. Если компонент является переиспользуемым и/или общим для компонентов текущего модуля, то он **должен** располагаться внутри директории `common/`

```yml
// Плохо
- components/
    - ReUsableComponent.vue

// Отлично
- components/
    - common/
        - ReUsableComponent.vue
```