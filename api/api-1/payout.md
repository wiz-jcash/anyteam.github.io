# payout

Using the **payout** model methods, a merchant can:

1. Create payout payment order - method [payout.create](payout.md#payout_create)
2. Calculate preliminary parameters of a payout order - method [payout.calc](payout.md#payout_calc)
3. Get information about the selected payment transaction - method [payout.get](payout.md#payout_get)

## endpoint

Queries for the model's methods are sent to endpoint **`https://api.any.money/`**.

## payout.create

The method creates a payment transaction order and returns its representation.

If **in\_curr** argument is not transmitted than its value will be taken equal to **out\_curr** value.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| amount | yes | "128" | payout amount |
| callback\_url | no | "[https://any\_money.redirect.com](https://any_money.redirect.com)" | url-address for order status change messages. [Messages format](../../test/en/add_order.md#order_repr) |
| externalid | yes | "123" | unique ID specified by the merchant |
| in\_curr | no | "USD" | credit currency name |
| out\_curr | yes | "UAH" | debit currency name |
| payway | yes | "anycash" | payment payway |

In the list of method input arguments, depending on the used payway the query **must** contain the following values:

??? info "Required arguments for the payways" **advcash, nixmoney, qiwi, payeer, perfect, webmoney** **btc, bchabc, ltc, usdt, eth** **privat24, monobank, sberbank, alfa\_bank, tinkoff\_cs, vtb24, card, visamc\_p2p** **mobile** **cash**

!!! warning "Caution!" For typed merchants, a transmitted payway **must** match the merchant type, and the **in\_curr** must be equal to **out\_curr** or absent, otherwise there will be exceptions raised

!!! warning "Caution!" Right after the query receival - order is created and the its processing is called. After the method execution, funds will be immediately **debited from the merchant's balance**

!!! success "Method response data" payout transaction [order representation](../../test/en/add_order.md)

??? failure "Exceptions" **EParamAmountFormatInvalid**\*\*amount\*\* argument wrong volume or format**EParamAmountTooBig**\*\*amount\*\* exceeds the operation maximum**EParamAmountTooSmall**\*\*amount\*\* is less than the operation minimum**EParamCurrencyInvalid**no currency in Any.Money**EParamFieldInvalid**invalid \*\*callback\_url\*\* argument**EParamInvalid**

## payout.calc

Method for calculating payment parameters taking into account the amount.

If the parameter **in\_curr** is not transmitted, the payment in **out\_curr** currency  
and withdrawals in the **out\_curr** balance except for the payway fee will be calculated.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| amount | yes | "238" | payout amount |
| in\_curr | no | "UAH" | credit currency name |
| out\_curr | yes | "RUB" | debit currency name |
| payway | yes | "anycash" | payment payway |

!!! warning "Caution!" For typed merchants, a transmitted payway **must** match the merchant type, and the **in\_curr** must be equal to **out\_curr** or absent, otherwise there will be exceptions raised

!!! success "Method response data" Payment order parameters **account\_amount**debit amount except for the fee**in\_amount**credit amount**in\_fee\_amount**fee amount in the credit currency**orig\_amount**original payment amount, was ordered when creating the order**out\_amount**debit amount**out\_fee\_amount**fee amount in the debit currency**rate**currency convertation rate, when performed. In the \`"rate": \["25.30301", "1"\]\` format

??? failure "Exceptions" **EParamAmountFormatInvalid**\*\*amount\*\* wrong format**EParamAmountTooBig**\*\*amount\*\* exceeds the operation maximum**EParamAmountTooSmall**\*\*amount\*\* is less than the operation minimum**EParamCurrencyInvalid**no currency in Any.Money**EParamInvalid**

## payout.get

The method for obtaining the parameter representation of the payment order by its identifier \(**externalid**\).

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| externalid | yes | "221" | unique order's ID specified by the merchant |

!!! success "Method response data" [order representation](../../test/en/add_order.md) of payout transaction

??? failure "Exceptions" **EParamInvalid**wrong \*\*externalid\*\* argument value**EParamNotFound**no order was found with the corresponding \*\*externalid\*\***EParamType**wrong \*\*externalid\*\* format

