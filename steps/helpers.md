# **Директория helpers/**

# **Содержание**

- [**Файловая структура модуля**](structure.md)
  - [**assets/**](assets.md)
  - [**helpers/**](helpers.md)
  - [**tests/**](tests.md)
  - [**Слой отображения (components/)**](component.md)
- [**Сущности (entity)**](entity.md)
- [**Абстракции (interfaces)**](interfaces.md)
- [**Адаптер (service)**](service.md)
- [**Репозиторий (API)**](api.md)
- [**Глобальное хранилище**](store.md)

#

>
  **UPD:** Данная директория, возможно, не имеет больших перспектив. Поэтому, если вы спросите меня: "А хуле не в глобальные хелперы сразу?", то я лишь достану сигарету и закурю, смотря вдаль.
>

Бывают такие ситуации, когда какая-то команда или конкретный модуль работают с специфичным функционалом, который может быть переиспользован в разных компонентах, бизнес-логике и тд. 

Для этого и нужна *helpers/* директория, которая послужит пристанищем для чистых функций внутри конкретного отдела команды или модуля, который нет необходимости распологать в глоабльном уровне.

Например:

```yml
- myModule/
  - helpers/
    - getMyUniqueHelper.ts
```

```typescript
// getMyUniqueHelper.ts
import { v1 as uuid } from 'uuid';

export default getMyUniqueHelper(): boolean {
  return uuid();
}
```

Сразу остановимся на том, как определена функция:

1. Определён `export default`;
2. Функция в формате `function declaration`.

Это 2 основных принципа, если ваш хелпер состоит из одной функции. Если ваш хелпер реализует несколько функций, то 1-ым правилом можно пренебречь.

## **Доп. правила именования**

Есть несколько кейсов, которые определяют формат именования файла:

1. Если внутри файла распологается и будет распологаться 1 функция, то файл должен называться с приставкой в виде **глагола**, по типу: `get | set | format | parse` и т.д.
2. Если хелпер реализует внутри себя набор методов для работы с конкретным функционалом, то он должен называться через **существительное**, по типу: `date.ts | text.ts | validator.ts` и т.д.