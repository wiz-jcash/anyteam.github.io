# address

The model allows merchant to create cryptocurrency addresses and receive their parameters, used to send and receive payments.

Using the **address** model's methods the merchant can:

1. Create the cryptocurrency address - [address.create](address.md#address_create)
2. Receive the details of merchant's cryptoaddress - [address.get](address.md#address_get)
3. Receive the merchant's cryptoaddresses list - [address.list](address.md#address_list)
4. Receive the list of available payways - [address.payways](address.md#address_payways)

## endpoint

Queries for the model's methods are sent to an endpoint **`https://api.any.money/`**.

## address.create

The method creates the cryptocurrency address \(crypto wallet\) and returns the representation of its parameters.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| callback\_url | no | "[https://any\_money.redirect.com](https://any_money.redirect.com)" | URL for sending notifications of payment receipt to the address. [Notification format](add_order.md#callback_repr) |
| comment | no | "Comment" | cryptoaddress comment, less than 50 characters |
| in\_curr | yes | "BTC" | the currency of created cryptoaddress |
| out\_curr | no | "UAH" | out currency for created cryptoaddress \(for cryptoaddress with auto convertation\) |
| payway | no | "btc" | payway name for the created cryptoaddress |

!!! warning "Caution!" For typed merchants transmitted payway must match the merchant's type and **out\_curr** must not be transferred since the currency conversion operation is not available for such merchant type

!!! warning "Caution!" The process of a cryptoaddress creation takes a time, most likely it will be necessary to request **address.get** method several times while the cryptoaddress itself \(value of **name** parameter\) does not appear in the response address representation

!!! success "Method response data" Created cryptoaddress [parameters](add_address.md)

??? failure "Exceptions" **EParamCurrencyInvalid**

## address.get

It is a method for finding and obtaining the information about cryptoaddress that are linked to the merchant. Method returns params representation for the founded cryptoaddress.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| name | yes\* | "3E8sEHMof39RgSHqSZswintqaXi3fSKYJN" | cryptoaddress string |
| id | yes\* | "844422933266350" | cryptoaddress ID |

```text
\*make sure to transmit **only one** argument for address.get. (**id** or **name**)
```

!!! success "Method response data" cryptoaddress [parameters](add_address.md)

??? failure "Exceptions" **EParamInvalid**none of the \*\*name\*\* or \*\*id\*\* arguments was transmitted, the invalid value of the \*\*id\*\* or \*\*name\*\* argument was transmitted, the \*\*name\*\* or \*\*id\*\* were transmitted simultaneously**EParamNotFound**address with transmitted arguments is not found**EParamType**wrong data type for \*\*id\*\* argument

## address.list

Method for displaying a list of cryptoaddresses that are linked to the merchant. Parameters are represented for each of them. Search filters are combined by logical **AND**.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| begin | no | "123456789" | the filter by the beginning of the responses range \(timestamp\). If **rotate** = true - by date and time of the last address using \(timestamp\) If **rotate** = false - by the time of its creation. default = "0" |
| count | no | "136" | the size of the method responses sample \(the value cannot exceed "200"\). default = "20" |
| end | no | "234567890" | the filter by the end of the responses sample range \(timestamp\). If **rotate** = true - by date and time of the last use of the address \(timestamp\) If **rotate** = false - by the time of its creation. default = "now" |
| first | no | "17" | the object number in the received sample of results, from which the addresses list and the scoring of **count** argument starts |
| in\_curr | no | "BTC" | the method response filter by cryptocurrency |
| is\_autoconvert | no | false | the address filter by auto convertation \(**true** - only addresses with auto convertation will get into the selection, **false** - only without auto-convertation\) |
| name | no | "dsf34s5t" | the filter by the cryptoaddress name, including the partial matching |
| ord\_by | no | "ctime" | the filter by timestamps \(“ctime” - creation time, “last\_used” - last used time\) |
| ord\_dir | no | false | the sort direction by timestamps |
| out\_curr | no | "USD" | the filter by out currency for auto convertation cryptoaddresses |
| rotate | no | false | the cryptoaddresses filter by the autorotation \( **true** - only addresses with autorotation "on", **false** - only with "off" autorotation\). default=**false** |

??? info "Feature of the **begin**, **end** argument's pair processing" For the **begin** argument a system will **always** assign the smaller value from the transmitted pair, and for the **end** it will always assign the largest. Regardless of the names they were transferred with

!!! warning "Caution!" For typed merchants **in\_curr** must be equal to **out\_curr** or both arguments are not transmitted, otherwise, an exception will be raised

!!! success "Method response data" **count**current response sample size**data** \[data representations\]\(add\_address.md\) array of cryptoaddresses**first**first address number in the method response sample. It is begins the \*\*count\*\* argument counting**page**current response page**page\_total**response pages total**total**total addresses found matching query filters

??? failure "Exceptions" **EParamCurrencyInvalid**currency not present in a system**EParamInvalid**

## address.payways

The method returns the names of all cryptopayways where the merchant can create the address. If **in\_curr** is transmitted, the response will include **only** payways names that work with given currency.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| in\_curr | no | "BTC" | credit currency name |

!!! success "Method response data" The list of available cryptopayways for the merchant

??? failure "Exceptions" **EParamNotFound**cryptopayway is not found**EParamPaywayInvalid**cryptopayway name is unacceptable**EParamCurrencyInvalid**cryptocurrency name is unacceptable**EStateCurrencyUnavail**could not find a cryptocurrency with the transmitted name \*\*in\_curr\*\*, or the currency is not a crypto

