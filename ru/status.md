# status

## status

Метод **status** позволяет получать данные ордера по его идентификатору.

## endpoint

Запросы на работу метода отправляются на endpoint **`https://api.any.money/`**.

??? abstract "Входящие параметры метода"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| externalid | yes | "123" | уникальный идентификатор ордера, заданный мерчантом |

!!! success "Данные ответа метода" [репрезентация параметров ордера](add_order.md) с переданным **externalid**

??? failure "Возможные возвращаемые ошибки" **EParamInvalid**не передан \*\*externalid\*\*, передано неверное значение \*\*externalid\*\***EParamNotFound**ордер не найден у данного мерчанта

