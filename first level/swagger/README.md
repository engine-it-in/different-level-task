[Вернуться к оглавлению](https://github.com/engine-it-in/different-level-task/blob/main/README.md)
***
* [Swagger](#swagger)
  * [Конфигурирование](#конфигурирование)
  * [Contract-first](#contract-first)
***

# Swagger

* Инструмент документирования API. 
* Устаревшая спецификация (OAS2). 
* Существует также OAS3 - современная спецификация, которая развивается.

## Конфигурирование

В результате конфигурирования получаем URL /v2/api-docs (/v2/api-docs), 
позволяющий увидеть описание сервиса в спецификации OpenApi. 
Для конфигурации используется класс Docket, в котором задаются теги, пути к контроллерам, 
а также описывается логика мэтча урлов (URL), которые экспонируются в описание. 
Теги используются для соотнесения (группировки) эндпоинтов контроллеров приложения 
по разделам функциональностей, поставляемой самим приложением.

## Contract-first

Существует возможность генерации классов из OpenApi. 
Эта возможность предоставляется плагином к Maven, например. 
Можем генерировать Dto-классы, не описывая их самостоятельно.
Можем добавлять mustache-темплейты для переопределения логики генерации кода.