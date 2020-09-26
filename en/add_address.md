# add\_address

!!! info "Cryptoaddress parameters" **callback\_url**URL address, where notification about the address related operations will be automatically sent. \[Notification format\]\(add\_order.md\#callback\_repr\)**comment**cryptoaddress comment**confirm**the required confirmation number for current cryptoaddress for transaction validation**ctime**cryptoaddress creation time**id**cryptoaddress ID in Any.Money system**in\_curr**cryptoaddress currency**last\_used**number of times a crypto address has been used**link**cryptoaddress link**name**cryptoaddress name \(cryptoaddress itself\)**out\_curr**an address autoconvertation currency, if present**payway**payway name**rotate**autorotation flag for cryptoaddress**status**actual \[cryptoaddress status\]\(\#status\_description\)

## status description

!!! info "Cryptoaddress statuses description"

| status | final | description |
| ---: | :---: | :--- |
| done | yes | created |
| new | no | accepted for executing |
| reject | yes | denied |
| wait | no | waiting for executing |

