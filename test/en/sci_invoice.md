# sci\_invoice

## endpoint

**`https://sci.any.money/invoice`**.

**invoice** module is intended for processing the merchant's balance replenishment operations by customers. Customers are by generated invoice, which can be paid in two ways:

* fully in the invoice currency;
* or, by arbitary parts and various currencies with convertation to merchant's credit balance currency.

  Customers can return the overpaid amount for the specific invoice. Or, they may reverse the base insuficient payment in

  case they do not plan to proceed with the surcharge by the invoice.

  The balance is replenished only after receiving an amount equal or greater than invoiced. The basic payment fee is paid

  by the merchant, but surcharges or refunds costs are beared by payers only.

Query form requires the next arguments:

!!! warning "Caution!" For typed merchants the transmitted payway have to match the merchant type otherwise the error will be returned

!!! warning "Caution!" Please note that the values of boolean argument **is\_multipay** are transmitted exactly in **string** \(!\) format and in lower case letters. In this case, transmitting the value "true" is equivalent to True, and "false" - to False.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| amount | yes | "1000,25" | invoice payment amount |
| callback\_url | no | "[https://callback\_example.com](https://callback_example.com)" | url-address for server-server messages about oprder's status changes. [Messages format](add_order.md#order_repr) |
| client\_email | no | "customer@domain.com" | payer email-address for invoice-payment status change notifications |
| externalid | yes | "123" | unique ID specified by the merchant |
| in\_curr | yes | "UAH" | credit currency |
| is\_multipay | no | "false" | flag for providing the making a surcharge and receiving a refund possibility \("true" = surcharge and return are possible, "false" = blocked\). default = "false" |
| lifetime | no | "1d12h" | order lifetime, in the seconds or in the **timedelta** format \(the string like "1w1d1h1m1s1ms"\). The valid value range is: "7d".."3600s". default="7d" |
| merchant | yes | "13" | string display of the local merchant's system ID |
| merchant\_payfee | no | "0,35" | the **share** of the total fee amount for incoming payments paid by the merchant \(value in the interval \[0..1\] with an accuracy of two decimal places\) |
| out\_curr | no | "USD" | optional invoice-payment debit currency |
| payway | no | "card" | payway of the replenishment \(possible values: "btc", "perfect", "qiwi"... etc.\). For the typed merchant only linked payway is available |
| redirect\_url | no | "[https://example.com/order\_page/](https://example.com/order_page/)" | url-address for redirecting the user method executing or pressing the "Back" button |
| sign | yes | 128-digits secuence | unique [hash signature of the SCI-request](sci_invoice.md#sign) |

**lifetime** is set to "7d" by default.  
If the **payway** volume was not transmitted in the query, the user can access all payways activated in the merchant's settings.

Time type values in the **timedelta** format are transmitted in pairs: ... **without spaces** in pairs.  
Only **seconds** can be transmitted as a keyless value. If, at least, one key is applied to the **timedelta** line, it must be ended by a key \(not a digit\), otherwise an error wiil be returned.

In the exception event, the user will be redirected to the exception page with the relevant information about it.

## web form

\`\`\`html tab="HTML"

```text
<a name="sign"></a>
### signature

The SCI-requests sent to the **invoice** module are authenticated by a signature, formed by the HEX algorithm 
(HMAC-SHA512 (**data**, **sci_key**)), where:  
- **data** - a string, composed from `parameters` values. They are added to the query in order of the following keys, sorted alphabetically;  
- **sci_key** - merchant's invoice-key.  

*Signature formation example*

``` python tab="Python"
import hashlib
import hmac

def sign_form_data(key: str, data: dict) -> str:
    if data: sorted_data = sorted(data.items())
    else: sorted_data = {}

    msg = ''.join(
        str(v) for k, v in sorted_data
        if not isinstance(v, (dict, list, type(None)))
    )
    msg = msg.lower()

    return hmac.new(key.encode(), msg.encode(), hashlib.sha512).hexdigest()
```

`php tab="PHP" <?php function sign_form_data(string $key, array $data) : string { ksort($data); $s = ''; foreach($data as $k=>$value) { if (in_array(gettype($value), array('array', 'object', 'NULL')) ){ continue; } if(is_bool($value)){ $s .= $value ? "true" : "false"; } else { $s .= $value; } } return hash_hmac('sha512', strtolower($s), $key); } ?>`

