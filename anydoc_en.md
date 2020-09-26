# anydoc\_en

The model contains methods provided receiving of the payment's receipts in the Any.Money system and receiving the receipt's statuses.

Using the **anydoc** model's methods the merchant can:

1. Request a receipt of payment made in the system - [anydoc.prepare\_receipt](anydoc_en.md#anydoc_prepare_receipt)
2. Get the status of a previously requested receipt - [anydoc.receipt\_status](anydoc_en.md#anydoc_receipt_status)

## endpoint

Requests for the model's methods are sent to an endpoint **`https://api.any.money/`**.

### anydoc.prepare\_receipt

A method for placing a task of the preparing a receipt of a payment made in the system. Payment is idetntified by the ID of its order.

!!! warning "Caution!" A receipt can only be provided for an order in “done“ [status](en/add_order.md#status_description)!

The created receipt lives in the system during the time specified in the **anydoc** server configuration or in the **anydoc** client-service settings. A corresponding notification is sent to the client service when receipt is deleted.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| externalid | yes | "123" | external identifier of the order the receipt is requested of |

!!! success "Method response data" **link**link to the requested receipt**order\_id**order identifier, \*\*order.id\*\***status**\*\*receipt\*\* status \(“new”, “pending”, “done”, “deleted”\)

??? failure "Exceptions" **EParamInvalid**order's \*\*externalid\*\* is not transmitted or wrong value transmitted**EParamNotFound**an order with such \*\*externalid\*\* is not found in the system or the order does not belong to the merchant**EParamType**wrong \*\*externalid\*\* argument data type**EStateUnavailable**the order is not in a "done" \[status\]\(../en/add\_order.md\#status\_description\)

### anydoc.receipt\_status

Method of the status request of the receipt provided by [anydoc.prepare\_receipt](anydoc_en.md#anydoc_prepare_receipt) method.

!!! warning "Caution!" If you transmit the order ID without the previously created or exceeded the specified “Lifetime” receipt the method will return an empty response.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| externalid | yes | "123" | external identifier of the order the receipt status is requested of |

!!! success "Method response data" **link**link to the requested status receipt**order\_id**order identifier, \*\*order.id\*\***status**order's \*\*receipt\*\* status \(“new”, “pending”, “done”, “deleted”\)

??? failure "Exceptions" **EParamInvalid**order's \*\*externalid\*\* is not transmitted or wrong value transmitted**EParamNotFound**an order with such \*\*externalid\*\* is not found in the system or the order does not belong to the merchant**EParamType**wrong \*\*externalid\*\* argument data type**EStateUnavailable**the order is not in a "done" \[status\]\(../en/add\_order.md\#status\_description\)

