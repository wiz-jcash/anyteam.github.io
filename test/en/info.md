# info

Using the **info** model methods the merchant can get the following information:

1. List of available currencies - method [info.currencies](info.md#info_currencies)
2. List of available currency exchange directions - method [info.exchanges](info.md#info_exchanges)

## endpoint

Queries for the model's methods are sent to endpoint **`https://api.any.money/`**.

## info.currencies

The method returns a list of **all** active currencies in the system and the base parameter values for each.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| - | - | - | - |

!!! success "Method response data" Active system currencies list  
**Currency params:** **admin\_max** maximum transaction amount for this currency**admin\_min** minimum transaction amount for this currency**is\_crypto** cryptocurrency flag**precision** value of the currency fractional part \(minimum movements grain\)

??? failure "Exceptions" **EParamNotFound** no active currency is found

## info.exchanges

Method for compiling a list of available currencies convertation directions in Any.Money system and representations of their parameters.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| - | - | - | - |

!!! success "Method response data" Directions for "currency-currency" exchange **Parameters of exchange directions**: **fee**external system fee for convertation of the currency pair, in percents**i\_rate**pure actual exchange rate, if the convertation tooks place. In the \`"rate": \["25.30301", "1"\]\` format**in\_curr**replenishment currency for convertation transaction - the one to be converted in the credit currency**in\_max**maximum amount in the credit currency**in\_min**minimum amount in the credit currency**out\_curr**debit currency for convertation transaction - the one in which the credit currency will be converted**out\_min**minimum convertation amount in the debit currency**out\_max**maximum convertation amount in the debit currency**rate**currency pair convertation rate with convertation fee added \(\*\*rate\*\*= \*\*i\_rate\*\* + system fee for the convertation\)**ratesource**the source of \*\*i\_rate\*\* receiving

??? failure "Exceptions" **EParamNotFound**no exchange directions were found

