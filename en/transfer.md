# transfer

Using the **transfer** model's methods the merchant can:

1. Create money transfer orders using the method [transfer.create](transfer.md#transfer_create);  
2. Preliminary calculate the transfer parameters - method [transfer.calc](transfer.md#transfer_calc);  
3. Get information about the selected transfer - method [transfer.get](transfer.md#transfer_get)

## endpoint

Queries for the model's methods are sent to endpoint **`https://api.any.money/`**.

## transfer.create

The method for creating a transfer order between the merchant's balances. If the credit currency is not transmitted, the **in\_curr** value is taken equal to query **out\_curr**.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| amount | yes | "435345,34" | transfer amount in the debit currency |
| callback\_url | no | "[https://any\_money.callback.com](https://any_money.callback.com)" | URL-address for automatically message sending about changes of the order status. [Messages format](add_order.md#order_repr) |
| externalid | yes | "123" | unique ID specified by the merchant |
| in\_curr | no | "UAH" | credit currency name |
| out\_curr | yes | "USD" | debit currency name |
| tgt | yes | "1056" | addressee of the funds transfer \(merchant's **lid** that transfer is addressed to\) |

!!! warning "Caution!" For typed merchants, the transmitted recipient type **tgt** have to match the type of merchant-payer, and **in\_curr** must be equal to **out\_curr** or absent. Otherwise, the relevant exceptions will be returned

!!! warning "Caution!" Immediately after the query is received, an order is created and its processing is started. After the method is executed **the money will be debited** from the balance

!!! success "Method response data" **transfer** transaction type [order parameters representation](add_order.md)

??? failure "Exceptions" **EParamAmountFormatInvalid**\*\*amount\*\* argument wrong format**EParamAmountTooBig**\*\*amount\*\* exceeds the transfer transaction maximum amount**EParamAmountTooSmall**\*\*amount\*\* is less than the transfer transaction minimum amount**EParamCurrencyInvalid**\*\*out\_curr\*\* parameter is not transmitted, or nonexistent currency is transmitted**EParamFieldInvalid**invalid \*\*callback\_url\*\* argument transmitted**EParamInvalid**

## transfer.calc

Method for calculating the parameters of a transfer order with a known amount.

If the credit currency is not transmitted, the **in\_curr** value is taken equal to query **out\_curr**.

The method calculates and returns a representation of the transfer transaction parameters.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| amount | yes | "1000" | transfer amount |
| in\_curr | no | "USD" | credit currency |
| out\_curr | yes | "USD" | debit currency |
| tgt | no | "6489" | recipient of the fund transfer \(**lid** of the merchant to whom the transfer is addressed\). If the argument is passed, the calculation will be made with the parameters \(individual fee\) of the recipient |

!!! warning "Caution!" For typed merchants, the transmitted recipient type **tgt** have to match the type of merchant-payer, and **in\_curr** must be equal to **out\_curr** or absent. Otherwise the propper exceptions will be returned

!!! success "Method response data" **account\_amount**amount withdrawn from the payer balance \(\*\*in\_amount\*\*+\*\*in\_fee\_amount\*\*\)**in\_amount**order credit amount**in\_fee\_amount**order fee amount in the credit currency**orig\_amount**original amount \(ordered while order creating\)**out\_amount**order debit amount**out\_fee\_amount**order fee amount in the debit currency

??? failure "Exceptions" **EParamAmountFormatInvalid**\*\*amount\*\* argument wrong format**EParamAmountTooBig**\*\*amount\*\* exceeds the transfer transaction maximum amount**EParamAmountTooSmall**\*\*amount\*\* is less than the transfer transaction minimum amount**EParamCurrencyInvalid**\*\*out\_curr\*\* parameter is not transmitted, or nonexistent currency is transmitted**EParamInvalid**

## transfer.get

The method for search and display the of transferring funds between merchants' balances orders \(**transfer** type orders\) by its identifier.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| externalid | yes | "123" | unique ID specified by the merchant |

!!! success "Method response data" **transfer** type transaction [order parameters representation](add_order.md)

??? failure "Exceptions" **EParamInvalid**wrong \*\*externalid\*\* argument value is transmitted**EParamNotFound**order with transmitted \*\*externalid\*\* not found

