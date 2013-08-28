Hata Günlükleme Arayüzü
=======================

Bu döküman hata günlükleme kütüphaneleri için bir genel arayüzdür.

The main goal is to allow libraries to receive a `Psr\Log\LoggerInterface`
object and write logs to it in a simple and universal way. Frameworks
and CMSs that have custom needs MAY extend the interface for their own
purpose, but SHOULD remain compatible with this document. This ensures
that the third-party libraries an application uses can write to the
centralized application logs.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119][].

The word `implementor` in this document is to be interpreted as someone
implementing the `LoggerInterface` in a log-related library or framework.
Users of loggers are refered to as `user`.

[RFC 2119]: http://tools.ietf.org/html/rfc2119

1. Özellikler
-----------------

### 1.1 Temelleri

- `LoggerInterface` logları yazmak için sekiz metodu sekiz seviyede [RFC 5424][] 
(debug, info, notice, warning, error, critical, alert, emergency) göstermelidir.

- Dokuzuncu metod olan `log`, ilk parametresinde hata günlüğünün seviyesini 
almalıdır. BU metdo günlük seviyeleri sabitlerinden birisini çağırarak günlük 
seviyesine ait metodlar ile aynı sonucu çağırmalıdır. Eğer uygulamada bilinmeyen 
seviyesi tanımlanmamış bir method çağırılırsa `Psr\Log\InvalidArgumentException` 
hatası vermelidir. Kullanıcılar uygulamanın desteklediği ve emin olmadıkları özel 
bir seviye kullanmamalıdır

[RFC 5424]: http://tools.ietf.org/html/rfc5424

### 1.2 Mesaj

- Her metod mesaj olarak bir string, veya `__toString()` metdouna sahip bir 
  nesne alır. Uygulayıcılar parametre göndermek için özel bir metdo kullanabilir.
  Böyle bir durum yoksa, string kullanmak zorundadırlar.

- Mesaj uygulayıcıların bir değer ile değiştirebileceği bir placeholder (yer tutucu)
  içerebilir. 

  Yer tutucu adları dizinin içeriği ile eşleşmelidir. 
  
  Yer tutucu isimleri bir `{` açma parantezi ve bir `}` kapama parantezi ile ayrılmış 
  olmalıdır. Parantezler ile yer turucu isimleri arasında boşluk olmamalıdır. 

  Yer tutucu isimleri `A-Z`, `a-z`, `0-9`, alttan çizgi `_`, ve nokta `.` karakterlerinde 
  oluşmalıdır. Dİğer karakterlerin kullanımı yer tutucu tanımlamalarının gelecekteki
  düzenlemeleri için ayrılmıştır. 
  
  Uygulayıcılar(implementors) yer tutucuları(placeholder) çeşitli stratejileri atlamak ve günlüklerin
  gösterimini değiştirmek için kullanabilir. Users SHOULD NOT pre-escape placeholder
  values since they can not know in which context the data will be displayed. 

  Aşağıdaki örnekte yer tutucu uygulamasının örneğini görebilirsiniz :

  ```php
  /**
   * Interpolates context values into the message placeholders.
   */
  function interpolate($message, array $context = array())
  {
      // build a replacement array with braces around the context keys
      $replace = array();
      foreach ($context as $key => $val) {
          $replace['{' . $key . '}'] = $val;
      }

      // interpolate replacement values into the message and return
      return strtr($message, $replace);
  }

  // a message with brace-delimited placeholder names
  $message = "User {username} created";

  // a context array of placeholder names => replacement values
  $context = array('username' => 'bolivar');

  // echoes "Username bolivar created"
  echo interpolate($message, $context);
  ```

### 1.3 Context

- Her method context verisi olarak bir dizi kabul eder. Bunun anlamı bir
  bir string'de tutulamayacan ekstra veriler bulunmasıdır. Bir dizi herşeyi
  barındırabilir. _Implementors MUST ensure they treat context data with
  as much lenience as possible. A given value in the context MUST NOT throw
  an exception nor raise any php error, warning or notice._

- Bir `Exception` nesnesi context verinin içerisine geldiğinde, o verinin 
  anahtar kelimesi `'exception'` olmalı. _Logging exceptions is a common 
  pattern and this allows implementors to extract a stack trace from the 
  exception when the log backend supports it._ Uygulayıcılar yine de burada
  `'exception'` anahtarına sahip bir elemanın gerçekten de `Exception` objesi
  olup olmadığını kontrol etmeliler. Onun içeriği herşey olabilir. 

### 1.4 Helper sınıfları ve arayüzleri

- `Psr\Log\AbstractLogger` sınıfı `LoggerInterface` arayüzünü kolayca ve genel 
  bir `log` metodu oluşturmanızı sağlar. _The other eight methods are 
  forwarding the message and context to it._

- Benzer şekilde `Psr\Log\LoggerTrait` ile sadece genel `log` metodu oluşturmanız
  gerekir. Traits size arayüz oluşturmaz, bu durumda sizin bir `LoggerInterface`
  oluşturmanız gerekmektedir.

- The `Psr\Log\NullLogger` is provided together with the interface. It MAY be
  used by users of the interface to provide a fall-back "black hole"
  implementation if no logger is given to them. However conditional logging
  may be a better approach if context data creation is expensive.

- The `Psr\Log\LoggerAwareInterface` only contains a
  `setLogger(LoggerInterface $logger)` method and can be used by frameworks to
  auto-wire arbitrary instances with a logger.

- The `Psr\Log\LoggerAwareTrait` trait can be used to implement the equivalent
  interface easily in any class. It gives you access to `$this->logger`.

- The `Psr\Log\LogLevel` class holds constants for the eight log levels.

2. Paket
----------

Arayüz ve sınıflar ilgili hata sınıflarınca yeterince açıklanmıştır ve 
uygulamanızı onaylamak için bir test arayüzü 
[psr/log](https://packagist.org/packages/psr/log) paketinin bir parçası olarak 
gelmektedir.

3. `Psr\Log\LoggerInterface`
----------------------------

```php
<?php

namespace Psr\Log;

/**
 * Describes a logger instance
 *
 * The message MUST be a string or object implementing __toString().
 *
 * The message MAY contain placeholders in the form: {foo} where foo
 * will be replaced by the context data in key "foo".
 *
 * The context array can contain arbitrary data, the only assumption that
 * can be made by implementors is that if an Exception instance is given
 * to produce a stack trace, it MUST be in a key named "exception".
 *
 * See https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-3-logger-interface.md
 * for the full interface specification.
 */
interface LoggerInterface
{
    /**
     * System is unusable.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function emergency($message, array $context = array());

    /**
     * Action must be taken immediately.
     *
     * Example: Entire website down, database unavailable, etc. This should
     * trigger the SMS alerts and wake you up.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function alert($message, array $context = array());

    /**
     * Critical conditions.
     *
     * Example: Application component unavailable, unexpected exception.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function critical($message, array $context = array());

    /**
     * Runtime errors that do not require immediate action but should typically
     * be logged and monitored.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function error($message, array $context = array());

    /**
     * Exceptional occurrences that are not errors.
     *
     * Example: Use of deprecated APIs, poor use of an API, undesirable things
     * that are not necessarily wrong.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function warning($message, array $context = array());

    /**
     * Normal but significant events.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function notice($message, array $context = array());

    /**
     * Interesting events.
     *
     * Example: User logs in, SQL logs.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function info($message, array $context = array());

    /**
     * Detailed debug information.
     *
     * @param string $message
     * @param array $context
     * @return null
     */
    public function debug($message, array $context = array());

    /**
     * Logs with an arbitrary level.
     *
     * @param mixed $level
     * @param string $message
     * @param array $context
     * @return null
     */
    public function log($level, $message, array $context = array());
}
```

4. `Psr\Log\LoggerAwareInterface`
---------------------------------

```php
<?php

namespace Psr\Log;

/**
 * Describes a logger-aware instance
 */
interface LoggerAwareInterface
{
    /**
     * Sets a logger instance on the object
     *
     * @param LoggerInterface $logger
     * @return null
     */
    public function setLogger(LoggerInterface $logger);
}
```

5. `Psr\Log\LogLevel`
---------------------

```php
<?php

namespace Psr\Log;

/**
 * Describes log levels
 */
class LogLevel
{
    const EMERGENCY = 'emergency';
    const ALERT     = 'alert';
    const CRITICAL  = 'critical';
    const ERROR     = 'error';
    const WARNING   = 'warning';
    const NOTICE    = 'notice';
    const INFO      = 'info';
    const DEBUG     = 'debug';
}
```
