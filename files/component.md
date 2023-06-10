# **Реализация Component**

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

Компонент реализует слой отображения и отвечает за то, как ему отобразить данные и никакой бизнес-логики.

## **Зона ответственности**

Проще говоря, компонент получает входные данные от бизнес-логики и форматирует их под необходимый для себя формат отображения. Например, у нас есть таблица, которая получает массив данных, где есть свойство `createdAt` типа `number` - timestamp, который мы форматируем в таблице и приводим к нужному формату отображения.



```
**!НЕПРАВИЛЬНО!**
// service.ts
...
getTableData(): Collection[] {
  return this.api.getTableData().then(res => {
    return res.map(item => ({
      ...item,
      createdAt: moment.unix(item.createdAt).format("DD/MM/YYYY")
    }))
  })
}
```

```
**!ОХУЕННО!**
// Table.vue
...
const props = defineProps({
  data: {
    type: Array,
    default: () => []
  }
})

const filteredData = computed(() => {
  return props.data.map(item => ({
    ...item,
    createdAt: moment.unix(item.createdAt).format("DD/MM/YYYY")
  }))
})
```

Грубо говоря, если вы используете слой отображения, то вся необходимая часть по форматированию данных должны оставаться за компонентом.

## **Определение констант**

Определение констант и опций можно опустить на уровне компонента, не вынося в слой сущностей, если наши данные необходимы только для текущего компонента. При определения таких опций, если они статичны, то они **НЕ** должны находиться в **state**

```vue
// УЖОС
<script setup>
  const state = reactive({
    options: [
      {
        label: "test",
        value: 1
      }
    ]
  })
</script>

// ХОРОШЕЧНО
<script setup>
  const OPTIONS = Object.freeze([
      {
        label: "test",
        value: 1
      }
    ])
</script>
```

## Порядок определения структур (только для setup скриптов)

1. Импорты
2. Инъекции/Инициализация Глоабльных переменных
3. Константы
3. Пропсы
4. State
5. Жизненные циклы
6. Watch
7. Computed
8. Methods