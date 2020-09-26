# exc

In case of an error, a JSON object is raised. Key `error` indicates its code `code`, message `message` - its type and additional data `data` specifying the reasons for its occurrence.

Example:

```javascript
{
  "error": {
     "code": -32034,
     "message": "EStateUnauth",
     "data": {
         "field": "token",
         "value": "1JMLaZieUat...teNG_Tph5RXcd"
     }
  },
  "jsonrpc": "2.0",
  "id": "100234sdf500"
}
```

??? failure "Exception codes according to JSON-RPC specification"

| code | message | description |
| ---: | :---: | :--- |
| -32600 | Invalid Request | transmitted JSON is not a valid query object |
| -32601 | Method not found | transmitted method does not exist or does not available |
| -32602 | InvalidInputParams | required input argument not transmitted or extra input argument transmitted |
| -32603 | Internal error | internal JSON-RPC error |
| -32700 | Parse error | the server received invalid JSON. Error in JSON text recognizing by the server |

The service uses error codes in the range from -32000 to -32099

??? failure "Exception codes raised by a server"

| code | message | description |
| ---: | :---: | :--- |
| -32001 | EParam | basic query input arguments error |
| -32002 | EAuth | basic user authorization error |
| -32003 | EParamInvalid | one of the method input arguments was transmitted in the wrong format or has an invalid value |
| -32004 | EParamType | wrong type of one method's input arguments |
| -32005 | EParamMethod | wrong method name in query |
| -32006 | EParamModel | wrong model name in query |
| -32010 | EParamSignInvalid | wrong query signature |
| -32011 | EParamTokenInvalid | invalid query token or object token |
| -32012 | EParamHeadersInvalid | one of the required headers missed, transmitted header contains an empty value or an empty string |
| -32013 | EParamRequestWindow | query timed out |
| -32014 | EParamCurrencyInvalid | currency does not present in a system |
| -32015 | EParamMerchantInvalid | the merchant is inactive or the merchant with the transmitted key is not found |
| -32016 | EParamFieldInvalid | incorrect argument field filling, one of the input arguments of the method was transmitted in the wrong format or has an invalid value |
| -32017 | EParamPassLength | invalid password length |
| -32018 | EParamPassPattern | invalid password pattern |
| -32020 | EParamPeriodInvalid | wrong time format transmitted |
| -32021 | EParamPeriodTooShort | time period too short |
| -32022 | EParamPeriodTooLong | time period too long |
| -32023 | EParamMerchantIPInvalid | the query was sent from an IP address not in the merchant's allowed list |
| -32031 | EStateMerchantInactive | specified merchant is inactive |
| -32032 | EStateForbidden | action is prohibited |
| -32033 | EStateCurrencyInactive | the transmitted currency is not active \(disabled\) |
| -32034 | EStateUnauth | no rights to perform the operation |
| -32035 | EStateUnavailable | operation is not possible due to external factors |
| -32036 | EStateOutPayUnavailable | payments in the entire Any.Money system or for current merchant are blocked |
| -32037 | EStateSessionLimit | authorization attempt limit exceeded |
| -32038 | EStateReqIPLimit | exceeded query-per-time limit for IP address |
| -32039 | EStateReqLimit | exceeded query-per-time limit for current session |
| -32043 | Req403 | query from an unknown domain or an invalid router |
| -32055 | EStateCurrencyUnavail | the currency is unavailable |
| -32056 | EStateInsufficientFunds | insufficient funds to complete the requested action |
| -32060 | EParamWrongCaptcha | authorization failed-per-hour exceeded |
| -32071 | EParamAmountInvalid | incorrect amount transferred |
| -32072 | EParamAmountOffLimits | the amount is out the transaction limits |
| -32073 | EParamAmountTooBig | the amount exceeds the transaction limit |
| -32074 | EParamAmountTooSmall | the amount is less than the transaction limit |
| -32076 | EParamCodeInvalid | invalid code value transmitted |
| -32079 | EParamLoginType | login type error |
| -32081 | EParamPaywayInvalid | transmited non-existent payway name |
| -32082 | EParamAmountFormatInvalid | invalid amount format |
| -32083 | EStatePaywayInactive | inactive payway |
| -32084 | EStatePaywayUnavail | payway is unavailabe for current merchant \(payway is blocked administratively\) |
| -32085 | EStateExchangeUnavail | convertation direction is not available |
| -32086 | EAuth2FARequired | two-factor authorization confirmation is required |
| -32087 | EAuth2Wrong | invalid two-factor authentication type |
| -32088 | EAuth2Fail | two-factor authentication fails limit for this IP address is exceeded |
| -32089 | EAuth2Drop | two-factor authentication not passed |
| -32090 | EParamNotFound | object with transmitted parameters not found |
| -32091 | EParamUnique | an object with the same name/identifier already exists |

