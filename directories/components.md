# **Директория `components/`**

Хранилище компонентов, которые необходимы для реализации текущего модуля.

```yml
- modules/
    - %module_name%/
        - components/
            - modals/
            - common/
            - Items/
                - Item.vue
                - index.vue
            - MyComponent.vue
```

Требования по использованию директории:

1. Название директорий **должно** быть в формате `camelCase`, если внутри нет компонента `index.vue`, иначе только с **Б**ольшой буквы:

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
    - ItemsCollection.vue
    - Item.vue

// Хорошо
- components/
    - ItemsCollection/
        - index.vue
        - Item.vue
    - MyComponent.vue
```

>
    UPD: Таким образом при изучении структуры модуля, становятся понятным,
    какие компоненты необходимы для реализации какого-либо сложного компонента,
    разбитого на несколько частей.
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
