# Директория **`helpers/`**

## **Содержание**

- [Главная](../README.md)
- [Структура модуля](README.md)
  - [**assets/**](assets.md)
  - [**helpers/**](helpers.md)
  - [**components/**](components.md)
  - [**entity/**](entity.md)
  - [**tests/**](tests.md)
- [Файлы](../files/README.md)
  - [**Component**](../files/component.md)
  - [**API**](../files/api.md)
  - [**Entity**](../files/entity.md)
  - [**Tests**](../files/tests.md)
  - [**Service**](../files/service.md)
  - [**Interfaces**](../files/interfaces.md)
  - [**Store**](../files/store.md)

#

Директория вспомогательных функций текущего модуля. Если вы уверены, что тот или иной функционал будет необходим только внутри вашего модуля (в связи со спецификой продукта и т.д), то функции **рекомендуется** распологать на этом уровне, а после задуматься о том, чтобы вынести в глобальную область видимости.

```yml
- %module_name%/
  - helpers/
    - myHelper.ts
```

Возможно, в вашем модуле будет несколько функций, которые будут по семантике похожими друг на друга, например, работа с датами (функции `parseDate` & `getGmtDate`), то в таком случае **НЕ** стоит инциализировать их по отдельности, а наоборот, расположить в одном файле под названием `date.ts`

```yml
// Плохо
- %module_name%/
  - helpers/
    - parseDate.ts
    - getGmtDate.ts

// Хорошо
- %module_name%/
  - helpers/
    - date.ts
```

Соглашения по работе с директорией:

1. Название файлов и директорий в формате `camelCase`;
2. Инициализация файлов по семантике использования без разделения на несколько семантически одинаковых функций (см. пример выше);
3. Внутри хелпера должны быть **ФУНКЦИИ!**, а не экспортируемые переменные и так далее;
4. Формат определения функции в виде `function declaration`:

```typescript
// Плохо
export getDate = (): Date => new Date();

// Хорошо
export function getDate(): Date {
  return new Date()
}
```

5. Функция должна быть **чистой**:

```typescript
/*
  Плохо. Мы сохраняем ссылочный тип на arr, который при
  работе со сложными структурами может выстрелить в ногу.
*/
export function editItem(arr, id, value) {
  const index = arr.findIndex(item => item.id === id);
  arr[index].value = value

  return arr;
}

/*
  Хорошо. Мы используем метод массива для редактирования
  элемента без сохранения ссылки на входной параметр, а значит
  нельзя неявно отредактировать его в вызываемом файле. 
*/
export function editItem(arr, id, value) {
  return arr.map((item) => {
    if (item.id === id) {
      item.value = value;
    }

    return item;
  })
}
```