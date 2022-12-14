# Speaker Recording Web App Integration Docs 

## Сообщения об ошибках

Иногда происходит ошибка, *понятное* сообщение о которой можно показать пользователю. 
Тогда возвращается Response Body с `display_message` и в приложении напрямую отображается пользователю.
Если `display_message` не будет предоставлен, то в приложении будет выведено общее сообщение о серверной ошибке. 

```ts
{
  // Optional
  display_message: string,
}
```

## Эндпоинты

### `GET /api/v1/texts`
 
Когда диктор заходит в приложение, дергается этот эндпоинт, чтобы получить информацию о текстах, которые нужно записать. 

Выводом служит список всех текстов.

**Request Body**

_No request body required_

**200 Response**

```ts
{
  "texts": []{
    "id": string, 
    "text": string, 
    "note": string,
    "minDuration": int, // seconds, optional 
    "maxDuration": int, // seconds, optional 
  }
}
```

**4xx Response**

Любой 4xx Response позволяет вывести сообщение об ошибке напрямую в приложении. Смотри [Сообщения об ошибках](#сообщения-об-ошибках)

### `POST /api/v1/speeches`
 
Когда диктор записал текст, на этот эндпоинт отправляется файл с голосовым сообщением. 

Request Body кодируется не в JSON, а в FormData. 

**Request Body**

```ts
{
  "text_id": string,
  "retries": string, 
  "speech": MultipartFile
}
```

### `POST /api/v1/skips`
 
Если диктор решил пропустить текст, то на этот эндпоинт отправляется кол-во попыток перезаписи, как в ТЗ. Бэкенд должен записать это в лог. 

Когда диктор записал текст, на этот эндпоинт отправляется файл с голосовым сообщением. 

**Request Body**

```ts
{
  "text_id": string,
  "retries": int
}
```

**200 Response**

_No response body_

**4xx Response**

Любой 4xx Response позволяет вывести сообщение об ошибке напрямую в приложении. Смотри [Сообщения об ошибках](#сообщения-об-ошибках)
