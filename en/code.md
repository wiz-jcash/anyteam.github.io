# code

The code model methods are used to provide the merchant with the possibilities of receiving and paying out funds through code payment obligations. Using code model methods, merchant can:

1. Create a payment code - method [code.create](code.md#code_create);
2. Check \(verify\) the payment code - method [code.verify](code.md#code_verify);
3. Use \(accept\) the payment code - method [code.redeem](code.md#code_redeem);
4. Get information about the code used for payment - method [code.get](code.md#code_get).

## endpoint

Queries for the model's methods are sent to endpoint **`https://api.any.money/`**.

## code.create

A method for creating a code for a transmitted check payment system, where the payment amount and currency are counted.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| amount | yes | "655" | payment amount \(the amount fixed in the code, but not took from the merchant's balance\) |
| curr | yes | "UAH" | name of the payment code currency |
| externalid | yes | "235" | unique ID specified by the merchant |
| payway | yes | "anycash" | payment payway name |

!!! warning "Caution!" For typed merchants the **payway** should match with the merchant's type, otherwise, an exception would be raised

!!! warning "Caution!" Right after the query receival - order is created and the its processing is called. After the method execution, funds will be immediately **debited from the merchant's balance**

!!! success "Method response data" **code**generated code

??? failure "Exceptions" **EParamAmountFormatInvalid**wrong \*\*amount\*\* value or format**EParamAmountTooBig**\*\*amount\*\* is more than the maximum transaction amount**EParamAmountTooSmall**\*\*amount\*\* is less than the minimum transaction amount**EParamCurrencyInvalid**debit and/or credit currency not present in the system, the currency is prohibited to use by this merchant in the transmitted payway**EParamInvalid**

## code.verify

Method to obtain the incoming payment code data by its full number.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| code | yes | "3453464745t8t8" | full code number |

!!! success "Method response data" Representation of billing code data **amount**the amount of the not redeemed code \(empty field for the "EXMO" payway\)**can\_be\_accepted**is it possible to redeem the code? \(possible values:"yes", "no", "unknown"\)**currency**payment currency name**fee**code redemption fee representation**add** - fixed fee amount**max** - max fee amount**min** - min fee amount**mult** - fee rate**method** - fee method&lt;/dd&gt; **fee\_value** fee amount in the code payment currency if the payment amount is present in the payway response. Else would set to "unknown" **payway** cheque payway name &lt;/dl&gt;

??? failure "Exceptions" **EParamInvalid**the wrong \*\*code\*\* transmitted**EParamCodeInvalid**

## code.redeem

Method for cashing a code payment by creating a code redeem order.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| code | yes | "3453464745t8t8" | full code of external payway |
| externalid | yes | "123" | unique ID specified by the merchant |

!!! success "Method response data" [Representationt of the code redeem order](add_order.md)&lt;/dd&gt;

??? failure "Exceptions" **EParamAmountTooBig**code payment amount exceeded the max convertation summ**EParamAmountTooSmall**code payment amount is less than the min transaction summ**EParamCodeInvalid**code can't be accepted by a system**EParamCurrencyInvalid**the code is created in a currency that is not available in Any.Money**EParamInvalid**\*\*externalid\*\* is not transmitted**EParamPaywayInvalid**code cannot be redeemed in any of the available payways**EParamType**\*\*externalid\*\* has an incorrect format**EStatePaywayInactive**

## code.get

The method for obtaining information about the code by the transmitted order identifier \(**externalid**\).

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| externalid | yes | "123" | order identifier in the store's system |

!!! success "Method response data" **amount**payment amount**code**generated code**currency**payment currency

??? failure "Exceptions" **EParamInvalid**\*\*externalid\*\* invalid value**EParamNotFound**no order was found with transmitted \*\*externalid\*\***EParamType**invalid data type of \*\*externalid\*\***EStateUnavailable**it is impossible to get information from an external system

