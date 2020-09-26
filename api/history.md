# history

Using **history** model methods a merchant can get:

1. Billing history - method [history.invoice](history.md#history_invoice)
2. The history of created incoming payment orders - method [history.code](history.md#history_code)
3. The history of created outpayment orders - method [history.payout](history.md#history_payout)
4. The history of created currency convertation orders - method [history.convert](history.md#history_convert)
5. The history of created funds transfer orders - method [history.transfer](history.md#history_transfer)
6. Merchant transactions history - method [history.statement](history.md#history_statement)
7. The history of merchant crypto addresses - method [history.address](history.md#history_address)

## endpoint

Queries for the model's methods are sent to endpoint **`https://api.any.money/`**.

## history.invoice

A method for displaying a list of **invoice** and **payin** type orders created by the merchant. The result is returned in the form of a sorted in the creation date descending list of orders of the **invoice** type. For each of them, a parameters representation is available.

??? abstract "Input method arguments"

| param | required | description |
| ---: | :---: | :--- |
| count | no | the size of the method responses sample \(the value cannot exceed "200"\). default = "20" |
| first | no | first object number in the method response sample. It begins the **count** argument counting. default="0" |
| in\_curr | no | filter by the credit currency |
| out\_curr | no | filter by the debit currency |
| payway | no | filter by the payment payway |

!!! success "Method response data" **count**current responses sample size**data** \*\*invoice\*\* type \[orders representation\]\(add\_order.md\) array**first**number of the first order in the method responses sample**page**current responses page**page\_total**total response pages**total**total orders found matching filters query

??? failure "Exceptions" **EParamCurrencyInvalid**\*\*in\_curr\*\* and/or \*\*out\_curr\*\* currencies are not found**EParamInvalid**wrong argument value transmitted**EParamPaywayInvalid**wrong payway transmitted**EParamType**incorrect argument data type

## history.code

A method to display a list of incoming code payment orders linked to a merchant.

Search filters are combined by logical **AND**.

The result is returned in the descending form the creation date sorted list for **coderedeem** type orders with parameters representation.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| count | no | "100" | the size of the method responses sample \(the value cannot exceed "200"\). default = "20" |
| first | no | "25" | first object number in the method response sample. It is begins the **count** argument counting. default="0" |
| in\_curr | no | "UAH" | filter by the credit currency |
| out\_curr | no | "GBP" | filter by the debit currency |
| payway | no | "anycash" | filter by the payment payway |

!!! success "Method response data" **count**current responses sample size**data**\*\*coderedeem\*\* type \[order representations\]\(add\_order.md\) array**first**number of the first order in the method responses sample**page**current responses page**page\_total**total response pages**total**total orders found matching filters query

??? failure "Exceptions" **EParamCurrencyInvalid**\*\*in\_curr\*\* and/or \*\*out\_curr\*\* currencies are not found**EParamInvalid**wrong argument value transmitted**EParamPaywayInvalid**wrong payway transmitted**EParamType**incorrect argument data type

## history.payout

A method for displaying a list of out payment orders linked to a merchant.

Search filters are combined by logical **AND**.

The result is returned in the descending form the creation date sorted list of **payout** type orders with parameters representation.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| count | no | "38" | the size of the method responses sample \(the value cannot exceed "200"\). default = "20" |
| first | no | "10" | first object number in the method response sample. It is begins the **count** argument counting. default="0" |
| in\_curr | no | "UAH" | filter by the credit currency |
| out\_curr | no | "USD" | filter by the debit currency |
| payway | no | "anycash" | filter by the payment payway |

!!! success "Method response data" **count**current responses sample size**data**\*\*payout\*\* type \[order representations\]\(add\_order.md\) array**first**number of the first order in the method responses sample**page**current responses page**page\_total**total response pages**total**total orders found matching filters query

??? failure "Exceptions" **EParamCurrencyInvalid**\*\*in\_curr\*\* and/or \*\*out\_curr\*\* currencies are not found**EParamInvalid**wrong argument value transmitted**EParamPaywayInvalid**wrong payway transmitted**EParamType**incorrect argument data type

## history.convert

Method for displaying filtered currency convertation orders \(**convert** type\),

Search filters are combined by logical **AND**. If none of them is set, the method will return a list of merchant's convertation operations in all currencies.

The result is returned in the descending form the creation date sorted list for **convert** type orders with parameters representation.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| count | no | "199" | the size of the method responses sample \(the value cannot exceed "200"\). default = "20" |
| first | no | "03" | first object number in the method response sample. It is begins the **count** argument counting. default="0" |
| in\_curr | no | "UAH" | filter by the credit currency |
| out\_curr | no | "GBP" | filter by the debit currency |

!!! success "Method response data" **count**current responses sample size**data**\*\*convert\*\* type \[order representations\]\(add\_order.md\) array**first**number of the first order in the method responses sample**page**current responses page**page\_total**total response pages**total**total orders found matching filters query

??? failure "Exceptions" **EParamCurrencyInvalid**\*\*in\_curr\*\* and/or \*\*out\_curr\*\* currencies are not found**EParamInvalid**\*\*first\*\*, \*\*count\*\* arguments wrong values**EParamType**transmitted wrong data type for \*\*first\*\*, \*\*count\*\* arguments

## history.transfer

Method for displaying a list of funds transfer orders linked to a merchant.

Search filters are combined by logical **AND**.

The result is returned in the descending form the creation date sorted list of the **transfer** type orders with parameters representation.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :--- | :--- | :--- |
| count | no | "100" | the size of the method responses sample \(the value cannot exceed "200"\). default = "20" |
| first | no | "13" | first object number in the method response sample. It is begins the **count** argument counting. default="0" |
| in\_curr | no | "USD" | filter by the credit currency |
| out\_curr | no | "UAH" | filter by the debit currency |

!!! success "Method response data" **count**current responses sample size**data**\*\*transfer\*\* type \[order representations\]\(add\_order.md\) array**first**number of the first order in the method responses sample**page**current responses page**page\_total**total response pages**total**total orders found matching filters query

??? failure "Exceptions" **EParamCurrencyInvalid**\*\*in\_curr\*\* and/or \*\*out\_curr\*\* currencies are not found**EParamInvalid**\*\*first\*\*, \*\*count\*\* arguments wrong values**EParamType**transmitted wrong data type for \*\*first\*\*, \*\*count\*\* arguments

## history.statement

Method for search and display funds movements at merchant accounts.

Search filters are combined by logical **AND**.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| begin | no | "123456789" | period start filter \(timestamp\). default="0" |
| count | no | "132" | the size of the method responses sample \(the value cannot exceed "200"\). default = "20" |
| curr | no | "UAH" | payment currency name |
| end | no | "234567890" | period end filter \(timestamp\). default="now" |
| externalid | no | "127" | filter by order's **externalid** parameter |
| filter\_amount | no | "&gt;123.4" | filter by the amount of funds movement with the condition operand. Possible operands: \(+, -, &lt;, &lt;=, &gt;, &gt;=, =, !=\). when only a number or a number with an operand is transmitted "+" or "-", a system uses logic "=". Operands "+" or "-" can be transmitted without an amount, it will mean only positive or negative values, accordingly |
| filter\_fee | no | "&gt;13.4" | filter by the fee for funds movement. In its currency with the condition operand. Possible operands: \(+, -, &lt;, &lt;=, &gt;, &gt;=, =, !=\). when only a number or a number with an operand is transmitted "+" or "-", a system uses logic "=". Operands "+" or "-" can be transmitted without an amount, it will mean only positive or negative values, accordingly |
| first | no | "32" | first object number in the method response sample. It begins the **count** argument counting. default="0" |
| group | no | "inpay" | order type filter \(possible types: **inpay**, **outpay**, **convert**, **transfer**\) |
| ord\_by | no | "amount" | sort objects \("amount" - by amount, "ctime" - by creation time\) |
| ord\_dir | no | false | direction of sorting results, if transmitted **ord\_by**: \(**false**=high-&gt;low, **true**=low-&gt;high\). default=**false** |

??? info "Feature of the **begin**, **end** argument's pair processing" For the **begin** argument a system will **always** assign the smaller value from the transmitted pair, and for the **end** it will always assign the largest. Regardless of the names they were transferred with

!!! success "Method response data" Orders list in the JSON array fotmat **count**current responses sample size**data**founded objects representations in the JSON format**amount** - the sum of merchant balance change indicated in the account funds movement**balance** - balance state \*\*right after\*\* execution of requested funds movement**ctime** - creation time \(timestamp\) of funds movement at the merchant account**curr** - balance currency**externalid** - external order ID**fee** - fee amount in the balance currency**model** - the model, which the payment relates to**oid** - ID of the funds movement at the merchant's accounts**other\_curr** - the currency of the order, according to which the funds movement at the merchant accounts is formed**payway** - payway name**tp** - order type, formed the funds movement at the merchant accounts&lt;/dd&gt; **first** number of the first order in the method responses sample **page** current responses page **page\_total** total response pages **total** total orders found matching filters query &lt;/dl&gt;

??? failure "Exceptions" **EParamCurrencyInvalid**\*\*curr\*\* currency is not present in a system**EParamInvalid**wrong argument value transmitted**EParamType**incorrect argument data type**EStateCurrencyInactive**\*\*curr\*\* currency is inactive \(disabled\)

## history.address

Method for displaying a list of cryptoaddress credit payment orders and their parameters.

Search filters are combined by logical **AND**.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| addr\_id | yes | "4356" | cryptoaddress ID |
| begin | no | "123456789" | period start filter \(timestamp\). default = "0" |
| count | no | "99" | the size of the method responses sample \(the value cannot exceed "200"\). default = "20" |
| end | no | "234567890" | period end filter \(timestamp\). default = "now" |
| first | no | "5" | first object number in the method response sample. It begins the addresses JSON array and **count** argument counting. default="0" |
| out\_curr | no | "RUB" | results filter by the debit currency |
| status | no | "done" | filter by current order state \("unfinished", "done", "fail"\) |
| tp | no | "autopayin" | filter by order type \( **payin**, **invoice** and **autopayin**\) |

!!! success "Method response data" Orders list in the JSON array fotmat **count**current responses sample size**data**founded \[order representations\]\(add\_order.md\) in the JSON object format**first**number of the first order in the method responses sample**page**current responses page**page\_total**total response pages**total**total orders found matching filters query

??? failure "Exceptions" **EParamCurrencyInvalid**transmitted \*\*out\_curr\*\* is not found**EParamInvalid**\*\*first\*\*, \*\*count\*\*, \*\*begin\*\*, \*\*end\*\*, \*\*tp\*\*, \*\*status\*\* arguments wrong values transmitted**EParamType**\*\*addr\_id\*\*, \*\*is\_autoconvert\*\*, \*\*first\*\*, \*\*count\*\*, \*\*begin\*\*, \*\*end\*\*, \*\*status\*\* arguments wrong type data transmitted**EStateCurrencyInactive**transmitted \*\*out\_curr\*\* is inactive \(disabled\)

