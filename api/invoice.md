# invoice

The **invoice** model is intended for invoicing customers and replenishing the merchant's balances by the merchant itself.

Customer's choice of the payment method is restricted by the payway/s that is/are specified by the merchant.

Parameter **is\_multipay** defines the criteria for crediting funds to the merchant:

* **is\_multipay = true** -  funds are credited to the merchant's balance only after receiving the full amount, indicated 

  in the invoice.

  > If the amount of the base payment is less than the invoiced, the client can pay extra or return the funds are already transferred. In case of overpayment - the client can get the change back.

* **is\_multipay = false** - client can't surcharge or return funds. The amount of payment immediately and in full, 

  excepts for the fee, credited to the merchant's balance.

The **share** of the fee total amount for receipts paid by the merchant is determined by the value of the **merchant\_payfee** argument \[0..1\], the rest of the commission is paid by customers. The operating costs of a refunds are **always** beared by customers.

Using the **invoice** model methods the merchant can:

1. Create a payment invoice - method [invoice.create](invoice.md#invoice_create);  
2. Calculate preliminary invoice parameters - method [invoice.calc](invoice.md#invoice_calc);  
3. Get information of the selected invoice - method [invoice.get](invoice.md#invoice_get).  

## endpoint <a id="endpoint1"></a>

Queries for the model's methods are sent to endpoint **`https://api.any.money/`**.

## invoice.create

The method to create an incoming payment order, taking into account the amounts of invoice and payment.

??? abstract "Input method arguments"

{% api-method method="post" host="https://api.any.money/" path="invoice.create" %}
{% api-method-summary %}
invoice.create
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="amount" type="number" required=true %}

{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="" type="string" required=false %}

{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-form-data-parameters %}
{% api-method-parameter name="" type="string" required=false %}

{% endapi-method-parameter %}
{% endapi-method-form-data-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="" type="string" required=false %}

{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| amount | yes | "1000,25" | invoice-payment amount |
| callback\_url | no | "[https://callback\_example.com](https://callback_example.com)" | url-address for order status change messages. [Messages format](../test/en/add_order.md#order_repr) |
| client\_email | no | "customer@domain.com" | email payer's address for notifications about changes in the status of an invoice payment |
| externalid | yes | "123" | unique ID specified by the merchant |
| in\_curr | yes | "UAH" | invoice-payment credit currency |
| is\_multipay | no | false | the flag providing the surcharge and receiving a refund possibility \(true = surcharge and return are possible, false = blocked\). default=false |
| lifetime | no | "1d12h" | "lifetime" of the order, in seconds or in the **timedelta** format \(the string like "1w1d1h1m1s1ms"\). The valid value range is \["7d" .. "3600s"\]. default = "7d" |
| merchant\_payfee | no | "0,35" | the **share** of the incoming payments fee paid by the merchant \(values in the \[0..1\] range, accurate to two decimal places. 0=0%..1=100% |
| out\_curr | no | "USD" | optional debit currency of the invoice-payment |
| payway | no | "card" | payway for the replenishment \(options: "btc", "perfect", "qiwi" ... etc.\). For a merchant linked to payway \(typed\), only a linked payway is available\) |
| redirect\_url | no | "[https://example.com/order\_page/](https://example.com/order_page/)" | url-address for redirecting the user after method executing or pressing the "Back" button |

!!! warning "Caution!" If the payway name is transmitted to the method, **make sure** to transmit the following arguments for the corresponding payment system together with it:

??? info "Required arguments for the payways"  **qiwi** **exmo, kuna, anycash** &lt;/dd&gt;

If the name of the transmitted debit currency \(**out\_curr**\) name differs from the credit currency name \(**in\_curr**\), after successful finalization of the order will be made an attempt to convert the received amount into the debit currency at the actual rate. If successful, the funds will be debited to the debit currency balance, otherwise - to the credit currency balance. If the transmitted debit currency name is equal to the credit currency name the **invoice.create** method will ignore the **out\_curr** argument value.

If the query does not contains the **out\_curr** the currency with the name **in\_curr** will be credited to the balance.

**timedelta** values are transmitted in pairs: `<value><key><value><key> ... <value><key>` **without spaces** between pairs. Only **seconds** can be transmitted as a keyless value. But, if in **timedelta** line presents at least one key, the whole string must ends with a key, not a digit. Otherwise an error will be raised.

!!! warning "Caution!" For typed merchants, the transmitted payway **must** match the type of merchant, otherwise the method will return an error

!!! success "Method response data" [order representation](../test/en/add_order.md) of the incoming payment

??? failure "Exceptions" **EParamAmountFormatInvalid**\*\*amount\*\* argument wrong format**EParamAmountTooBig**\*\*amount\*\* exceeds the operation maximum amount**EParamAmountTooSmall**\*\*amount\*\* is less than the operation minimum amount**EParamCurrencyInvalid**no currency in a system**EParamFieldInvalid**invalid \*\*callback\_url\*\* argument**EParamInvalid**

## invoice.calc

The method of calculating the invoice payment parameters, taking into account the amount of debit sum.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| amount | yes | "500" | Input payment amount |
| in\_curr | yes | "USD" | credit currency |
| payway | yes | "anycash" | paway for the possible payment |

!!! warning "Caution!" For typed merchants, the transmitted payway **must** match the type of merchant, otherwise, the method will return an error

!!! success "Method response data" **account\_amount**debit amount excepts for the fee amount**in\_amount**credit amount**orig\_amount**amount set when creating an order**out\_amount**amount debited to the merchant balance**rate**convertation rate, if convertation tooks place

??? failure "Exceptions" **EParamAmountFormatInvalid**\*\*amount\*\* argument wrong format**EParamAmountTooBig**\*\*amount\*\* exceeds the operation maximum amount**EParamAmountTooSmall**\*\*amount\*\* is less than the operation minimum amount**EParamCurrencyInvalid**no currency in a system**EParamInvalid**

## invoice.get

Method for **invoice**-payment parameters requesting by its external ID.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| externalid | yes | "257" | unique ID specified by the merchant |

!!! success "Method response data" [order representation](../test/en/add_order.md) of incoming payment transaction

??? failure "Exceptions" **EParamInvalid**empty \*\*externalid\*\* or has a wrong type or value**EParamNotFound**order with transmitted \*\*externalid\*\* not found

