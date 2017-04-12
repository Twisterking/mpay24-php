# mpay24-php

[![Packagist](https://img.shields.io/packagist/l/doctrine/orm.svg)]()

Offical PHP SDK for SOAP Bindings

## Documentation

A short demo implementation guide is available at https://docs.mpay24.com/docs/get-started</br>
Documentation is available at https://docs.mpay24.com/docs.

## Configuration

The configuration file is located in `/lib/config/config.php`<br />
The soap username and password should be entered here.<br />
The `CURL_LOG` and `MPAY24_LOG` path starts in `/lib/`

## SDK Overview

First it is necessary to include and initialize the library:
```php
include_once ("./lib/MPAY24.php");
$mpay24 = new MPAY24(); // or with soap username, password if not provided in config
```

#### Create a token for seamless creditcard payments

```php
$tokenizer = $mpay24->createPaymentToken("CC")->getPaymentResponse();
```

#### Create a payment

Creditcard payment with a token
```php
$payment = array(
  "amount" => "100",
  "currency" => "EUR",
  "token" => ""
);
$result = $mpay24->acceptPayment("TOKEN", "123", $payment);
```
Paypal payment
```php
$payment = array(
  "amount" => "100",
  "currency" => "EUR"
);
$result = $mpay24->acceptPayment("PAYPAL", "123", $payment);
```

#### Create a checkout

Initialize a minimum paypage
```php
$mdxi = new ORDER();
$mdxi->Order->Tid = "123";
$mdxi->Order->Price = "1.00";

$checkoutURL = $mpay24->selectPayment($mdxi)->location; // redirect location to the payment page

header('Location: '.$checkoutURL);
```

[How to work with ORDER objects](https://github.com/mpay24/mpay24-php/wiki/How-to-work-with-ORDER-objects)

#### Get current transaction status

```php
$mpay24->transactionStatus(12345); // with mpaytid
$mpay24->transactionStatus(null, "123 TID"); // with tid
```
### Prerequisites

In order for the mPAY24 PHP SDK to work, your installation will have to meet the following prerequisites:

* [PHP >= 5](http://www.php.net/)
* [cURL](http://at2.php.net/manual/de/book.curl.php)
* [DOM](http://at2.php.net/manual/de/book.dom.php)
* [Mcrypt](http://at2.php.net/manual/en/mcrypt)

Please refer to http://www.php.net/phpinfo or consult your systems administrator in order to find out if your system fulfills the prerequisites.
