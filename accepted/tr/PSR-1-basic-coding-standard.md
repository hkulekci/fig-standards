Basit Kodlama Standartları
==========================

Standartların bu bölümü paylaşılan PHP kodları arasında birlikte çalışabilirliği 
yükseltmek için gerekli olan standart kodlama unsurlarının ne olduğundan 
oluşmaktadır.

(Çeviri için)
"MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", ve "OPTIONAL" kelimeleri 
[RFC 2119][] adresinde açıklandığı gibi yorumlanmıştır.


The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119][].

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md


1. Genel Bakış
--------------

- Dosyalar sadece `<?php` ve `<?=` etiketlerini kullanmalıdır. 

- Dosyalar PHP kodları için sadece `UTF-8 without BOM` formatında olmalıdır. 

- Dosyalar ya sembolleri tanımlamalıdır (sınıfları, fonksiyonları, sabitleri 
declare etmeli) ya da çıktı vermelidir. (çıktı vermeli, .ini ayar dosyasını 
değiştirmeli) İkisini birden yapmamalı. 

- Namespace ve sınıflar [PSR-0][]'ı takip etmeli.

- Sınıf ismi `StudlyCaps` formatında tanımlanmalıdır.

- Sınıf sabitleri tümü büyük harf ve alt çizgi ile tanımlanmalıdır. 

- Metod isimleri `camelCase` formatında olmalıdır. 


2. Dosyalar
-----------

### 2.1. PHP Etiketi

PHP kodu `<?php ?>` uzun etiketi ya da `<?= ?>` kısa etiketi kullanmalı; diğer 
etiket versiyonlarını kullanmamalı.

### 2.2. Karakter Kodlaması(Encoding)

PHP kodu `UTF-8 without BOM` formatında olmalıdır.

### 2.3. Etkileri

Bir dosya bir sembolü (sınıflar, fonksiyonlar, sabitler, ..vs.) tanımlamalıdır 
ve diğer hiçbir etkisi olmaz, ya da bir mantık yürütür. İkisini birden yapması 
tavsiye edilmez.

-------------------------------------------------------------------------------
The phrase "side effects" means execution of logic not directly related to
declaring classes, functions, constants, etc., *merely from including the
file*.

"Side effects" include but are not limited to: generating output, explicit
use of `require` or `include`, connecting to external services, modifying ini
settings, emitting errors or exceptions, modifying global or static variables,
reading from or writing to a file, and so on.

-------------------------------------------------------------------------------
Aşağıdaki örnek etkileri ve tanımlamaları aynı dosyada kullanan bir dosyadır;
i.e, önlemek için bir örnek:

```php
<?php
// side effect: change ini settings
ini_set('error_reporting', E_ALL);

// side effect: loads a file
include "file.php";

// side effect: generates output
echo "<html>\n";

// declaration
function foo()
{
    // function body
}
?>
```

The following example is of a file that contains declarations without side
effects; i.e., an example of what to emulate:

```php
<?php
// declaration
function foo()
{
    // function body
}

// conditional declaration is *not* a side effect
if (! function_exists('bar')) {
    function bar()
    {
        // function body
    }
}
?>
```


3. Namespace and Sınıf İsimleri
-------------------------------

Namespace and sınıflar [PSR-0][] standartlarını takip etmelidir.

Bunun anlamı her sınıf kendi dosyası içinde olur, ve en azından bir 
seviye namespace ve onu içerenbri üst seviye vendor ismi altında olur. 

Sınıf ismi `StudlyCaps` formatında yazılmalıdır.

PHP 5.3 ve sonrası için yazılmış kodlar normal namespace kullanmalıdır.

Örneğin:

```php
<?php
// PHP 5.3 and later:
namespace Vendor\Model;

class Foo
{
}
?>
```

5.2.x ve öncesi için yazılmış kodlarda pseudo-namespacing çevirisini kullanmalıdır.
Bu yöntemde sınıflar `Vendor_` öneki alırlar.

Örneğin:

```php
<?php
// PHP 5.2.x and earlier:
class Vendor_Model_Foo
{
}
?>
```

4. Sınıf Sabitleri, Özellikleri, ve Metodları
---------------------------------------------

"Sınıf" terimi tüm sınıflardan(classes), arayüzlerden(interface) ve 
(traits)[http://php.net/manual/en/language.oop5.traits.php]'lerden bahsetmektedir

### 4.1. Sabitler(Constant)

Sınıf sabitleri tüm harfleri büyük ve alt çizgi ile ayrılmış tanımlanmalıdır.

Örneğin:

```php
<?php
namespace Vendor\Model;

class Foo
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
?>
```

### 4.2. Özellikler

Bu rehber kasten önerilen özellik isimlerinin `$StudlyCaps`, `$camelCase`, veya `$under_score` gibi şekillerde kullanılmasını önler.



Whatever naming convention is used SHOULD be applied consistently within a
reasonable scope. That scope may be vendor-level, package-level, class-level,
or method-level.

### 4.3. Metodlar

Metodlar `camelCase()` formatında bildirilmelidir.

