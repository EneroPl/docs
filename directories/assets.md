# Директория **`assets/`**

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

Директория хранения медиа-данных, "статики" и т.д внутри текущего модуля.

```yml
- %module_name%/
  - assets/
    - data.json
    - images/
      - icons/
        - logo.svg
      - preview.png
    - videos/
      - onboarding.mp4
    
```

Соглашения по работе с директорией:

1. Файлы медиа-контента (изображения, иконки, видео и т.д) именуются в формате `cebab-case`;
2. Директории и файлы именуются в формате `camelCase`;
2. Данные не должны дублироваться в других модулях.