# status

## status

Method **status** allows you to receive order data by its identifier.

## endpoint

Queries for the method are sent to endpoint **`https://api.any.money/`**.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| externalid | yes | "123" | unique ID specified by the merchant |

!!! success "Данные ответа метода" order with transmitted **externalid** [parameters representation](../test/en/add_order.md)

??? failure "Exceptions" **EParamInvalid**\*\*externalid\*\* not transmitted, wrong \*\*externalid\*\* value transmitted**EParamNotFound**order not found with requested merchant

