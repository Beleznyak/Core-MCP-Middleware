# Спецификация протокола обмена данными JSON-RPC 2.0

markdown

Взаимодействие между LLM-агентом и Python Middleware осуществляется по стандарту JSON-RPC 2.0. Все запросы содержат обязательные поля `jsonrpc`, `method`, `params` и `id`.

## 1. Метод `Auth` (Авторизация)

Используется для аутентификации LLM-агента или сессии пользователя в шлюзе.

**Пример запроса:**

```json

{

  "jsonrpc": "2.0",

  "method": "Auth",

  "params": {

    "token": "bearer_mcp_token_xyz123",

    "client_id": "llm-agent-01"

  },

  "id": 1

}

```

**Пример успешного ответа:**

```json

{

  "jsonrpc": "2.0",

  "result": {

    "status": "authenticated",

    "session_expires_at": "2026-06-11T12:00:00Z"

  },

  "id": 1

}

```

## 2. Метод `GetMetadata` (Получение структуры метаданных)

Позволяет LLM-агенту запросить у 1С через шлюз список доступных объектов (документов, справочников) и их структуру, чтобы агент знал, какие поля существуют.

**Пример запроса:**

```json

{

  "jsonrpc": "2.0",

  "method": "GetMetadata",

  "params": {

    "object_type": "Справочник",

    "object_name": "Контрагенты"

  },

  "id": 2

}

```

**Пример успешного ответа:**

```json

{

  "jsonrpc": "2.0",

  "result": {

    "object": "Справочник.Контрагенты",

    "fields": [

      {"name": "Наименование", "type": "String", "length": 150},

      {"name": "ИНН", "type": "String", "length": 12},

      {"name": "КПП", "type": "String", "length": 9}

    ]

  },

  "id": 2

}

```

## 3. Метод `ExecuteTool` (Выполнение функции)

Используется для вызова конкретной бизнес-логики или функции в 1С (например, проведение документа или получение отчета).

**Пример запроса (Получение остатков товара):**

```json

{

  "jsonrpc": "2.0",

  "method": "ExecuteTool",

  "params": {

    "tool_name": "GetStockBalance",

    "arguments": {

      "item_uuid": "e02df352-8706-11e2-8024-00155d050704",

      "warehouse_id": "основной"

    }

  },

  "id": 3

}

```

**Пример успешного ответа:**

```json

{

  "jsonrpc": "2.0",

  "result": {

    "status": "success",

    "balance": 42.000,

    "unit": "шт"

  },

  "id": 3

}

```
