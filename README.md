# Simple Tinkoff bank acquiring library
Простая библиотека для приема платежей через интернет.

### Возможности

 * Генерация URL для оплаты товаров
 * Подттверждение платежа
 * Просмотр статуса платжа
 * Отмена платежа

### Установка

С помощью [Composer](https://getcomposer.org/):

```php
composer require kenvel/laravel-tinkoff
```

Подключение в контроллере:

```php
use Kenvel\Tinkoff;
```

## Примеры использования
### 1 Инициализация

```php
$api_url    = 'https://securepay.tinkoff.ru/v2/';
$terminal   = '152619634343';
$secret_key = 'terminal_secret_password';

$tinkoff = new Tinkoff($api_url, $terminal, $secret_key);
```

### 2 Получить URL для оплаты
```php
//Подготовка массива с данными об оплате
$payment['OrderId']     = '123456';         //Ваш идентификатор платежа
$payment['Amount']      = '100';            //сумма всего платежа в рублях
$payment['Language']    = 'ru';             //язык - используется для локализации страницы оплаты
$payment['Description'] = 'Some buying';    //описание платежа
$payment['Email']       = 'user@email.com'; //email покупателя
$payment['Phone']       = '89099998877';    //телефон покупателя
$payment['Name']        = 'Customer name';  //Имя покупателя
$payment['Taxation']    = 'usn_income';     //Налогооблажение

//подготовка массива с покупками
$items[] = [
    'Name'  => 'Название товара',
    'Price' => '100',    //цена товара в рублях
    'NDS'   => 'vat20',  //НДС
];

//Получение url для оплаты
$paymentURL = $tinkoff->paymentURL($payment, $items);

//Контроль ошибок
if(!$paymentURL){
  echo($tinkoff->error);
} else {
  $payment_id = $tinkoff->payment_id;
  header('Location: ' . $paymentURL);
}
```

### 3 Получить статус платежа
```php
//$payment_id Идентификатор платежа банка (полученый в пункте "2 Получить URL для оплаты")

$status = $tinkoff->getState($payment_id)

//Контроль ошибок
if(!$status){
  echo($tinkoff->error);
} else {
  echo($status);
}
```

### 4 Отмена платежа
```php
$status = $tinkoff->cencelPayment($payment_id)

//Контроль ошибок
if(!$status){
  echo($tinkoff->error);
} else {
  echo($status);
}
```

### 5 Подтверждение платежа
```php
$status = $tinkoff->confirmPayment($payment_id)

//Контроль ошибок
if(!$status){
  echo($tinkoff->error);
} else {
  echo($status);
}
```

