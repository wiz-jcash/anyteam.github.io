# callback

**Callback** - the service for sending the method's execution results and order status finalizing message to a transmitted URL.

To use the **Callback** service, you must transmit an URL in the method execution query \(in the **callback\_url** argument, if it is provided in the incoming parameters\). After the method is completed, a query containing the results of its work and the status of the order will be sent to the transmitted address.

## Callback query formation

Messages from **Callback** service are formed as queries with the following parameters:

* query method - POST;  
* the query contains the creation time \(**x-utc-now-ms**, in milliseconds\) in the headers and hash 

  signature [**x-signature**](auth.md#x-sign_creation) verifying its authenticity:

??? info "Callback query headers example:" **x-signature**9a45aefaaab16f5414f533b238037a6ddb24c8c1160304a0fa7bd793a26f07a89368f8f103a1f404e9ba03290525b4f7df9d59786177bc0fe5abb3d4deb4fcfe**x-utc-now-ms**1575228754418**x-merchant**1234

* the query body is transmitted in JSON format, the method execution result will be placed in it:

??? info "A method response example installed in a callback query:"

```text
    {
      tp: invoice,
      lid: 135735,
      ref: T7f3RvtjZHxKOABY5aQVtOZeycPPUBDwDIQMSEvhr4GxwlLoy17mHEIshRhpMlrOyB09,
      tgt: null,
      rate: null,
      ctime: 1571402123375,
      ftime: 1575228754418,
      owner: 220,
      token: LSoUUp0KIdfCXrgBsBHMawlSXWS6H8iwz5ZbJP00lMhwMN1bdeTFiN5CQ5Rk8k1oSh9Q,
      status: fail,
      in_curr: USD,
      out_curr: USD,
      uaccount: null,
      userdata: {
      },
      in_amount: 11,
      externalid: in_862021412136317,
      out_amount: 11,
      orig_amount: 11,
      payway_name: perfect,
      renumeration: 0,
      in_fee_amount: 3,
      account_amount: 8,
      out_fee_amount: 3
    }
```

## Callback-message representation

For most types of orders the callback message returned to the specified callback URL contains [standart order params representation](add_order.md#order_repr) of Any.Money system.

For orders replenished by cryptocurrency, the callback message returned to the specified callback URL contains [restricted order params representation](add_order.md#callback_repr).

## Waiting for receiving confirmation, resending the callback messages

After sending **callback** message the service is waiting for a response with an HTTP status of `200 OK` for five seconds.

If a response with the status of `200 OK` is not received in the first five seconds, the **Callback** service will resend the query **only** five times at the following intervals:

* **second** 25 sec after the previous one;
* **third** - after 2 minutes and 5 sec;
* **fourth** - after 10 minutes and 25 sec;
* **fifth** - after 52 minutes and 5 sec.

After each repetition, the **Callback** service will wait for a response with the status of `200 OK`.

