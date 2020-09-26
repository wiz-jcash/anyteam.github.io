# convert

Using the methods of the **convert** model the merchant can:

1. Perform the currencies convertation - [convert.create](convert.md#convert_create)
2. Calculate the convertation parameters - [convert.calc](convert.md#convert_calc)
3. Receive info about chosen convertation transaction - [convert.get](convert.md#convert_get)

!!! warning "Caution!" Model methods are blocked for typed merchants, trying to use **convert** model methods for **typed merchants** the system will return an exception **EStateForbidden**.

## endpoint

Queries for the model's methods are sent to endpoint **`https://api.any.money/`**.

## convert.create

The method creates a convertation order for the transmitted currency pair and returns the representation of the operation details.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| callback\_url | no | "[https://any\_money.redirect.com](https://any_money.redirect.com)" | url-address for messages about order status changes. [Messages format](add_order.md#order_repr) |
| externalid | yes | "123" | ID specified by the merchant |
| in\_amount | yes\* | "2345" | credit amount |
| in\_curr | yes | "RUB" | credit currency |
| out\_amount | yes\* | "345" | debit amount |
| out\_curr | yes | "UAH" | debit currency |

```text
\* **in_amount** or **out_amount** is required
```

!!! success "Method response data" [order representation](add_order.md) of the **convert** type transaction

??? failure "Exceptions" **EParamAmountFormatInvalid**the transmitted amount does not match the currency parameters. For example, the fractional part does not match**EParamAmountTooBig**the processed amount exceeds the maximum convertation amount**EParamAmountTooSmall**the processed amount is less than minimum convertation amount**EParamCurrencyInvalid**the currency is not present in a system**EParamFieldInvalid**invalid \*\*callback\_url\*\* argment is transmitted**EParamInvalid**wrong values of \*\*in\_amount\*\*, \*\*out amount\*\* are transmitted, no one of \*\*in\_amount\*\*, \*\*out amount\*\* is transmitted or they are transmitted together**EParamUnique**order with transmitted \*\*externalid\*\* allready exists**EStateCurrencyInactive**transmitted \*\*in\_curr\*\*, \*\*out\_curr\*\* currency is not active \(disabled\)**EStateExchangeUnavail**\*\*in\_curr\*\* and \*\*out\_curr\*\* are equial or convertation direction is unavailable**EStateForbidden**attempt to use the method for a typed merchant**EStateInsufficientFunds**the required amount exceeds the merchant balance of the debit currency

## convert.calc

Method for calculating the convertation order parameters taking into account the transaction amount and currencies names.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| in\_amount | yes\* | "100" | credit amount |
| in\_curr | yes | "RUB" | credit currency |
| out\_amount | yes\* | "100" | debit amount |
| out\_curr | yes | "USD" | debit currency |
| validate\_balance | no | true | the key for verification necessity the sufficiency of the the debit currency balance amount to complete current transaction \(**true** = verification is required, **false** = not\) default = **true** |

```text
\* **in_amount** or **out_amount** is required
```

!!! success "Method response data" Parameters of the **convert** type order **account\_amount**the changes amount of the merchant's balance \(for the \*\*convert\*\* type methods it is always equal to zero\)**in\_amount**credit currency amount required for convertation**orig\_amount**original sum, the debit or credit \*\*amount\*\*, booked with the order creation**out\_amount**amount of debit currency resulting after the convertation**rate**convertation rate, if the convertation performed. In \`"rate": \["25.30301", "1"\]\` format

??? failure "Exceptions" **EParamAmountFormatInvalid**the transferred amount does not match the currency parameters. For example, the fractional part does not match**EParamAmountTooBig**the processed amount exceeds the maximum convertation amount**EParamAmountTooSmall**the processed amount is less than minimum convertation amount**EParamCurrencyInvalid**the \*\*in\_curr\*\* and/or \*\*out\_curr\*\* currency are not present in a system**EParamInvalid**invalid argument value is transmitted**EStateCurrencyInactive**transmitted \*\*in\_curr\*\*, \*\*out\_curr\*\* currency is not active \(disabled\)**EStateExchangeUnavail**\*\*in\_curr\*\* and \*\*out\_curr\*\* are equial or convertation direction is unavailable**EStateForbidden**attempt to use the method for a typed merchant**EStateInsufficientFunds**the required amount exceeds the merchant's balance of the debit currency

## convert.get

Method for search and receive the **convert** \(currency exchange\) type order parameters by its ID.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| externalid | yes | "123" | unique ID specified by the merchant |

!!! success "Method response data" [order representation](add_order.md)

??? failure "Exceptions" **EParamInvalid**wrong \*\*externalid\*\* argument value transmitted**EParamNotFound**no order was found with the appropriate \*\*externalid\*\***EStateForbidden**attempt to use the method for a typed merchant

