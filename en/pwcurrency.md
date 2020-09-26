# pwcurrency

Using the methods of **pwcurrency** model merchant can:

1. Get a list of all currencies available for the merchant in every payway. And also a operations parameters list for the 

   every currency - method [pwcurrency.list](pwcurrency.md#pwcurrency_list)

2. Change the currency use status in the transitted payway - method [pwcurrency.pwcma\_switch](pwcurrency.md#pwcurrency_pwcma_switch)

## endpoint

Queries for the model's methods are sent to endpoint **`https://api.any.money/`**.

## pwcurrency.list

The method returns a list of currencies available for the merchant for every payway. And operation parameters for every currency.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| - | - | - | - |

!!! success "Method response data" **{payway name}, its parameters:** **in** - \*\*credit\*\* currency name with parameters**fee** - fee representation in the \*\*replenishment\*\* currency**add** - fixed \*\*replenishment\*\* fee amount**max** - maximum \*\*replenishment\*\* fee amount**method** - \*\*replenishment\*\* fee rounding method**min** - minimum \*\*replenishment\*\* fee amount**mult** - \*\*replenishment\*\* fee interest rate**is\_active** - \*\*credit\*\* currency activity status in the payway for actual merchant**is\_crypto** - cryptocurrency flag**order** - currency priority in payway**precision** - \*\*credit\*\* minimum grain**tech\_max** - maximum \*\*credit\*\* payment amount \(if available\)**tech\_min** - minimum \*\*credit\*\* payment amount \(if available\) **order** - payway priorioty in Any.Money system **out** - \*\*debit\*\* currency name with parameters **fee** - fee representation in the \*\*debit\*\* currency**add** - fixed \*\*debit\*\* fee amount**max** - maximum \*\*debit\*\* fee amount**method** - \*\*debit\*\* fee rounding method**min** - minimum \*\*debit\*\* fee amount**mult** - \*\*debit\*\* fee interest rate**is\_active** - \*\*debit\*\* currency activity status in the payway for actual merchant**is\_crypto** - cryptocurrency flag**order** - currency priority in payway**precision** - \*\*debit\*\* minimum grain**tech\_max** - maximum \*\*debit\*\* payment amount \(if available\)**tech\_min** - minimum \*\*debit\*\* payment amount \(if available\) **type** - payway type &lt;/dl&gt; &lt;/dd&gt; &lt;/dl&gt; &lt;/dd&gt;

## pwcurrency.pwcma\_switch

The method changes the currency using status by the merchant in the transmitted payway and returns the result of changing.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| curr | yes | "USD" | currency name to change |
| is\_active | yes | True | direction of the currency status changing \(True = activate, False = deactivate\) |
| is\_out | no | False | direction of the currency transaction to be activated \(True = outgoing payments, False = incoming payments \(default\)\) |
| payway | yes | "anycash" | payway name |

!!! success "Method response data" **currency**name of the status changing currency**is\_active**currency activity status in the payment system for the current merchant \(True = active, False = deactivated\)**merchant\_lid**local merchant ID**payway**the payway name where the currency status changes

??? failure "Exceptions" **EParamCurrencyInvalid**the required currency in the transmitted payway for actual merchant was not found**EStateForbidden**changing the currency status in the transmitted payway for actual merchant is prohibited

