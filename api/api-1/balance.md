# balance

## balance

The **balance** method returns information about the merchant's balances in the format: `currency: amount`. If the **curr** argument is transmitted in the query, the method will return the balance only of transmitted currency.

## endpoint

Queries for the method are sent to endpoint **`https://api.any.money/`**.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| curr | no | "UAH" | balance currency name |

!!! success "Method response data" Params of the balances in the format: `{"currency1":amount, "currency2":amount,...}`

??? failure "Exceptions" **EParamCurrencyInvalid**\*\*curr\*\* currency is not present in the system

