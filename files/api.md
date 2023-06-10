# **Реализация API**

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

Эта штуку называют по разному - *Gateway*, *Repository* или ещё как-нибудь. Во всяком случае, суть одна - это набор методов, реализующие запросы к серверу и возвращту ответа. Ответ, полученный от сервера **распаковывется** и возвращается в виде набора данных, необходимых для реализации модуля.

## **Инициализация API**

Например, у нас будет API, у которой будет единственный метод - `login()`:

```typescript
import axios from 'axios';

export const authApi: IAuthApi = {
  async login(params: TAuthLogin): Promise<TAuthTokens> {
    return await axios.get('/api/login')
      .then(({ data: response }) => {
        return response.data;
      })
  }
}
```

А теперь разберём, что мы сделали и почему именно так:

1. Создали `named export` переменную типа `Object`, которая зафиксирована собственным интерфейсом;
2. Определили метод и описали типами входящие и выходящие данные;
3. Выполнили запрос, распаковали ответ и вернули объект ответа к вызываемомму методу.

### **Named export**

Можете написать много букв о том, что в данном файле ничего не находится кроме `API` и будете правы. Но, я ничего лучше не придумал, как зафиксировать один единственный способ экспортировать `API` таким образом, чтобы каждый разработчик мог с уверенностью знать, что какой-либо ему `API` не понадобился, он всегда его получит по одному и тому же формату импорта.

### **Типизация**

Это риторический пункт, который всё же стоит описать "на всякий случай" во избежания ситуации, когда разработчик не имеет бекенда и выставляет `any` тип, а потом прикручивает `backend` и забывает о типизации.

### **Распаковка и выходные данные**

Когда мы получаем ответ от `axios`, то мы получаем данные не сразу, а в формате `AxiosResponse`, в котором находится `Response` конфиги, статусы и т.д, и где-то там наша заветная `data`. Как уже писалось выше, каждый `API` запрос должен возвращать только те данные, которые необходимы бизнес-логике для реализации собственного модуля.

## **Вспомогательные инструменты**

Как мы уже поняли, `API` состоит только из набора методов и ничего более, так как не предусмотрено законом. Но, есть некоторые исключения.

UPD: Если этих исключений нет в перечисленном списке, значит они не соответствуют правилам соглашения и недопустимы к реализации в этом слое.

### **AbortController**

Этот класс реализует возможность *отмены* асинхронного запроса. Его можно определить на уровне `API`:

```typescript
import axios from 'axios';

export const authApi: IAuthApi = {
  abortController: undefined,
  abortRequst() {
    if (this.abortController) {
      this.abortController.abort();
    }
  },
  uploadFile(id: number, chunk: TChunk): Promise<TFileData> {
    this.abortController = new AbortController();

    return axios.get('/api/upload', {
      signal: this.abortController.signal,
      params: {
        id,
        chunk
      }
    })
      .then(({ data: response }) => {
        return response.data;
      })
  }
}
```

В данном случае `API` имеет внутренний метод типа `() => void`, который необходим для отмены асинхронного запроса.
