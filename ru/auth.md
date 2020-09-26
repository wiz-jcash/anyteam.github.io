# auth

Интерфейс **API** системы **Any.Money** выполнен на основе протокола удаленного вызова процедур [JSON-RPC](https://www.jsonrpc.org/specification), но его реализация имеет такие ограничения:

* уведомления \(notifications\) не обрабатываются;  
* не поддерживаются групповые запросы/ответы \(batch request/response\);  
* все значения параметров в запросе передаются **только** в строчном или 

  boolean форматах;  

* в поле **params** поддерживается передача аргументов в формате 

  json-объекта, но не массивом;  

* если для аргумента передано значение `null`, то используется его 

  значение по умолчанию. Когда оно не предусмотрено - возвращается 

  [соответствующая ошибка](exc.md).

## Формирование запроса

Запросы к API отправляются на endpoint **`https://api.any.money/`**.  
_Запрос должен быть подписан. Данные мерчанта передаются в заголовках._

### Заголовки

* **x-merchant**    - идентификатор мерчанта;  
* **x-signature**   - [хеш-подпись запроса](auth.md#x-sign_creation);
* **x-utc-now-ms**  - время создания запроса, в миллисекундах \(utc timestamp\).

### Параметры

* **method** - имя метода запроса;  
* **params** - параметры метода в виде `ключ: значение;`
* **jsonrpc** - версия спецификации \(всегда равна "2.0"\);  
* **id** - идентификатор запроса.  

### Подпись

Для аутентификации запроса его нужно подписать и передать подпись в заголовке **x-signature**.  
Подпись формируется по алгоритму HEX\(HMAC-SHA512\(**data**, **api\_key**\)\), где:

* **data** - строка, составленная из значений объекта **params**. Для 

  формирования строки необходимо:  

  * отсортировать ключи **params** по алфавиту;  
  * составить строку из их значений, за исключением вложенных 

    объектов и `null`;  

  * к концу полученной строки добавить значение хедера **x-utc-now-ms**;  
  * привести строку к нижнему регистру;  

* **api\_key** - ключ мерчанта для взаимодействия по API.  

### Пример запроса по API

\`\`\` python tab="Python" import hashlib import hmac import requests import time

def sign\_data\(key: str, data: dict, utc\_now: str\) -&gt; str: if data: sorted\_data = sorted\(data.items\(\)\) else: sorted\_data = {}

```text
msg = ''.join(
    str(v) for k, v in sorted_data
    if not isinstance(v, (dict, list, type(None)))
) + str(utc_now)
msg = msg.lower()

return hmac.new(key.encode(), msg.encode(), hashlib.sha512).hexdigest()
```

data = { "method": "balance", "params": {"curr": "BTC"}, "jsonrpc": "2.0", "id": "1" }

MERCHANT = '1234' API\_KEY = 'your api\_key here' utc\_now = str\(int\(time.time\(\) \* 1000\)\)

headers = { "x-merchant": MERCHANT, "x-signature": sign\_data\(API\_KEY, data.get\('params'\) or {}, utc\_now\), "x-utc-now-ms": utc\_now }

response = requests.post\(url='[https://api.any.money/](https://api.any.money/)', json=data, headers=headers\)

```text
``` php tab="PHP"
<?php
function sign_data(string $key, array $data, string $utc_now) : string {
    ksort($data);
    $s = '';
    foreach($data as $k=>$value) {
      if (in_array(gettype($value), array('array', 'object', 'NULL')) ){
          continue;
        }
        if(is_bool($value)){
            $s .= $value ? "true" : "false";
        } else {
            $s .= $value;
        }
    }
    $s .= $utc_now;
    return hash_hmac('sha512', strtolower($s), $key);
}


$data = array(
    "method" => "balance", 
    "params" => array("curr" => "BTC"),
    "jsonrpc" => "2.0",
    "id" => "1"
);

$MERCHANT = '1234';
$API_KEY = 'your api_key here';
$utc_now = strval(((int)round(microtime(true) * 1000)));

$data_string = json_encode($data);

$ch = curl_init('https://api.any.money/');                                                                                                                                            
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
    'Content-Type: application/json',
    'Content-Length: ' . strlen($data_string),
    'x-merchant: ' . $MERCHANT,
    'x-signature: ' . sign_data($API_KEY, $data['params'] ?: array(), $utc_now),
    'x-utc-now-ms: ' . $utc_now)
);

$result = curl_exec($ch);
?>
```

## Варианты ответа системы

Если запрос корректен, то по ключу `"result"` будет возвращен результат выполнения метода в виде json-объекта.

!!! success "Ответ метода"

```javascript
    {
      "result": {"UAH": "0"}, 
      "id": "1", 
      "jsonrpc": "2.0",
    }
```

**result**результат выполнения метода**id**идентификатор запроса, на который получен данный ответ**jsonrpc**версия спецификации \(всегда равен "2.0"\)

Иначе, по ключу `"error"` будет возвращен код, сообщение и данные возникшей [ошибки](exc.md):

!!! failure "Ошибка"

```javascript
    {
        "error": {
            "code": -32003,
            "message": "EParamInvalid",
            "data": {
                "field": "externalid",
                "value": "25"
            }
        },
        "jsonrpc": "2.0",
        "id": "R5822"
    }
```

!!! info "Возвращаемые в заголовках данные" **x-utc-now-ms**время ответа, в миллисекундах \(utc timestamp\)**x-signature**хеш-подпись ответа

