# add\_order

_Methods return the values of **only** requested keys leaving the rest remain empty_

## order params representation \(full callback message\)

!!! info "Parameters of Any.Money order" **account\_amount**debit/credit sum except for the fee**address**crypto address for cryptocurrency invoice payment. When paying with multy subpay, the address of the last one is returned**amount\_paid**the parameter returned only by SCI-methods contains the amount of payments made as part of the SCI-payment**code**\[code data representation\]\(add\_code.md\)**confirmations**number of confirmations \(confirmed blocks\) for orders with cryptocurrencies**ctime**order creation time \(timestamp\)**email**e-mail address for sending the messages of \*\*invoice\*\*-payment status changes**externalid**unique ID specified by the merchant**front\_params**list of merchant's \[parameters for frontend\]\(\#front\_params\_description\). In JSON object format: {key: value} \(only for \*\*invoice\*\* type orders\)**ftime**order finalizing time \(timestamp\)**in\_amount**credit sum**in\_curr**credit currency**in\_fee\_amount**fee sum in credit currency**is\_multipay**the possibility flag of surcharges, refunds, reimburse for \*\*invoice\*\* payment**lid**local order ID**merchant\_payfee**the share of the incoming payments fee paid by the merchant \(values in the \[0..1\] range, accurate to two decimal places\)**orig\_amount**original order sum \(that is booked during order creation\)**out\_amount**debit amount for the current order**out\_curr**debit currency**out\_fee\_amount**fee amount in a debit currency**owner**order creator merchant ID**payer**payer ID**paylink**field available only for \*\*invoice\*\* orders. Contains link to the payment page**payway\_name**the name of order payway. It is filled only for singlepay payments and remains empty for multipay \(\`"btc", "perfect", "qiwi"...\` etc.\)**rate**currency convertation rate, if performed. In the \`"rate": \["25.30301", "1"\]\` format**redirect\_url**URL, used by the fronted for default redirection**ref**internal order ID. For cryptocurrency transactions and is equal to \*\*tx\_id\*\***renumeration**recalculated sum \(for the \*\*payout\*\* orders, if the order was recalculated\), refund sum \(for \*\*invoice\*\* order with overpay\)**reqdata**original query data in the format {key: value}. For example: \`{amount: 10, externalid: 1556712010408, in\_curr:UAH, out\_curr:UAH, payway:privat24, userdata: {payee:4444444444444448}}\`**status**actual \[order status\]\(\#status\_description\)**tgt**merchant, the payee \(for the \*\*transfer\*\* type transactions\)**token**unique order ID-key**tp**order type \(allowable options: \*\*payin\*\*, \*\*autopayin\*\*, \*\*coderedeem\*\*, \*\*payout\*\*, \*\*convert\*\*, \*\*transfer\*\*, \*\*invoice\*\*, \*\*sci\_subpay\*\*, \*\*sci\_refund\*\*\)**txid**the ID of cryptocurrency transaction \(\*\*true\*\* = cryptocurrency used in transaction, \*\*false\*\* = only fiat currencies\)**uaccount**payee details \(for \*\*invoice\*\*, \*\*payout\*\* type transactions\)**userdata**payee details \(for external payways \`{"payee": 4444444444444448}\`\)

## restricted callback message representation

Restricted callback message returned for the cryptocurrency replenished orders .

!!! info "Restricted callback message representation" **account\_amount**debit sum except for the fee**confirmations**number of confirmations \(confirmed blocks\)**ctime**order creation time \(timestamp\)**ftime**order finalizing time \(timestamp\)**in\_amount**credit sum**in\_curr**credit currency**in\_fee\_amount**fee sum in credit currency**lid**local order ID**out\_amount**debit amount for the current order**out\_curr**debit currency**out\_fee\_amount**fee amount in a debit currency**payway\_name**the name of order payway \(\`"btc", "perfect", "qiwi"...\` etc.\)**status**actual \[order status\]\(\#status\_description\)**tp**order type \(\*\*autopayin\*\*\)**txid**the ID of cryptocurrency transaction \(\*\*true\*\* for the cryptocurrency replenished orders\)

### front\_params fields description

!!! info "**front\_params** fields description" **logo**custom logo for SCI \(the merchant selects the logo itself to be displayed on SCI instead of the standard ANYMONEY logo\). Format: image converted to base64 format. Example: \`logo: "data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVV...." \`**theme**set the day/night mode for SCI \(volumes: “night”, “day”, ”auto”\)**theme\_color**set the color theme for SCI elements selected by the merchant. Format: “violet”, “yellow”, “red” etc.

### order statuses description

!!! info "Possible order states"

| status | final | description |
| ---: | :---: | :--- |
| abnormal | no | not defined \(error\) |
| canceled | yes | canceled by the user |
| done | yes | successfully completed |
| expired | yes | expired **lifetime** or **expiry** order params |
| fail | yes | unsuccessfully completed |
| finalizing | no | in a state of convertation from the receipt currency to the invoice crediting currency |
| new | no | accepted for execution |
| pending | no | waiting for execution |
| reject | yes | denied |
| retry | no | executing after error |
| started | no | processing |
| wait | no | waiting for completion |

