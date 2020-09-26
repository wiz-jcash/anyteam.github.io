# payway

Using the methods of **payway** model merchant can:

1. Get a list of available payways and their parameters - method [payway.list](payway.md#payway_list)
2. Activate a payway for work with the merchant - method [payway.activate](payway.md#payway_activate)
3. Deactivate a payway for work with the merchant - method [payway.deactivate](payway.md#payway_deactivate)

## endpoint

Queries for the model's methods are sent to endpoint **`https://api.any.money/`**.

## payway.list

The method returns a list of payways, available for the merchant, and their parameters. As well as the list of the payway currencies with their parameters.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| - | - | - | - |

!!! success "Method response data" **{payway name}, its parameters:** **active\_in\_system**is a payway active in Any.Money system **active\_for\_merchant** is a payway active for a current merchant **is\_public** can a merchant activate / deactivate a current payway **payin\_allowed** payway can accept credit payments **tp** payway type. Possible options: "sci", "crypto", "cheque", "cash" **{Currency name in a current payway}, its parameters:** **in\_fee** - fee representation in the \*\*replenishment\*\* currency**add** - fixed \*\*replenishment\*\* fee amount**max** - maximum \*\*replenishment\*\* fee amount**method** - \*\*replenishment\*\* fee rounding method**min** - minimum \*\*replenishment\*\* fee amount**mult** - \*\*replenishment\*\* fee rate **in\_tech\_max** - maximum \*\*replenishment\*\* amount \(if replenishment is available\) **in\_tech\_min** - minimum \*\*replenishment\*\* amount \(if replenishment is available\) **out\_fee** - fee representation in the \*\*debit\*\* currency **add** - fixed \*\*debit\*\* fee amount**max** - maximum \*\*debit\*\* fee amount**method** - \*\*debit\*\* fee rounding method**min** - minimum \*\*debit\*\* fee amount**mult** - \*\*debit\*\* fee rate **out\_tech\_max** - maximum \*\*debiting\*\* amount \(if debiting is available\) **out\_tech\_min** - minimum \*\*debiting\*\* amount \(if debiting is available\) **prec** - minimum currency grain value &lt;/dl&gt;&lt;/dd&gt; &lt;/dl&gt; &lt;/dd&gt;

??? failure "Exceptions" **EParamNotFound** no payways found

## payway.activate

The method changes the status \(activates\) the payway for a merchant, who request it, and returns the result of the change.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| payway | yes | "anycash" | payway name to be activated |

!!! success "Method response data" **is\_active**payway status for actual merchant**m\_lid**local merchant ID**payway**payway name

??? failure "Exceptions" **EParamMerchantInvalid**merchant is inactive**EParamNotFound**merchant not found**EParamPaywayInvalid**transmitted payway name not found**EStateForbidden**it is forbidden to change the linked payway status**EStatePaywayUnavail**merchant cannot deactivate transmitted payway \(it is not public\)

## payway.deactivate

The method deactivates the payway for a merchant, who request it, and returns the result of the change.

??? abstract "Input method arguments"

| param | required | example | description |
| ---: | :---: | :--- | :--- |
| payway | yes | "anycash" | payway name to be deactivated |

!!! success "Method response data" **is\_active**payway status for actual merchant**m\_lid**local merchant ID**payway**deactivated payway name

??? failure "Exceptions" **EParamMerchantInvalid**merchant is inactive**EParamNotFound**merchant not found**EParamPaywayInvalid**transmitted payway name not found**EStateForbidden**it is forbidden to change the linked payway status**EStatePaywayUnavail**merchant cannot deactivate transmitted payway \(it is not public\)

