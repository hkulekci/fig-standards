Basit Kodlama Standartları
==========================

Standartların bu bölümü paylaşılan PHP kodları arasında birlikte çalışabilirliği 
yükseltmek için gerekli olan standart kodlama unsurlarının ne olduğundan 
oluşmaktadır.

(Çeviri için)
"MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", ve "OPTIONAL" kelimeleri 
[RFC 2119][] adresinde açıklandığı gibi yorumlanmıştır.

Çeviri:
Bu belgedeki *ZORUNLU*, *ÖNERİ* ve *SEÇİMLİK* imleri RFC 2119'da açıklandığı 
gibi yorumlanır. [Bu konudaki çeviri notu](http://belgeler.gen.tr/rfc/rfc2119.html)

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md


1. Genel Bakış
--------------

- Dosyalar sadece `<?php` ve `<?=` etiketlerini kullanmalıdır. 

- Dosyalar PHP kodları için sadece `UTF-8 without BOM` formatında olmalıdır. 

- Dosyalar ya sembolleri tanımlamalıdır (sınıfları, fonksiyonları, sabitleri 
declare etmeli) ya da çıktı vermelidir. (çıktı vermeli, .ini ayar dosyasını 
değiştirmeli), ikisini birden yapmaması önerilir. 

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

### 2.3. Yan Etkileri

Bir dosya bir yeni sembolü (sınıflar, fonksiyonlar, sabitler, ..vs.) tanımlanması
ve diğer hiçbir yanetkisinin olmaması önerilir, ya da bir mantık yürütebilir. 
İkisini birden yapması tavsiye edilmez.

"Yan Etki(Side Effect)"den kasıt sınıf, fonksiyon, sabit, ...vs tanımlamaları 
ile direk bağlantısı olmayan mantık çalıştırmalarıdır. *sadece dosyadan include 
edilir*.

"Yan Etki(Side Effect)", çıktı üretme, açıkça `require` veya `include` kullanımı, 
dış servislere bağlanma, ini ayar dosyasını değiştirme, hata veya istisnaları 
ifade etme, global ya da statik değişkenleri değiştirme, dosyadan okuma ya da 
yazma ve dahasını içerir ancak bunlarla sınırlı değildir.

Aşağıdaki örnek yan etkileri ve tanımlamaları aynı dosyada kullanan bir dosyadır;
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
```
Aşağıdaki dosya hiçbir yan etki içermeyen bir tanımlama içeren örnektir.

```php
<?php
// tanımlama
function foo()
{
    //  fonksiyon gövdesi
}

// Şartlı tanımlama bir yan etki(Side effect) *değildi*.
if (! function_exists('bar')) {
    function bar()
    {
        // fonksiyon gövdesi
    }
}
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
// PHP 5.3 ve sonrası:
namespace Vendor\Model;

class Foo
{
}
```

5.2.x ve öncesi için yazılmış kodlarda pseudo-namespacing çevirisi yöntemini 
kullanmaları önerilir. Bu yöntemde sınıflar `Vendor_` öneki alırlar.

Örneğin:

```php
<?php
// PHP 5.2.x ve öncesi:
class Vendor_Model_Foo
{
}
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
```

### 4.2. Özellikler

Bu rehber kasten önerilen özellik isimlerinin `$StudlyCaps`, `$camelCase`, 
veya `$under_score` gibi şekillerde kullanılmasını önler.

*Adlandırma kuralı ne kullanılırsa kullanılsın, makul kapsamda ve sürekli 
kullanılabilir olması önerilir. Bu kapsam vendor-seviyesinde, paket-seviyesinde,
sınıf-seviyesinde veya metod-seviyesinde olabilir.*

### 4.3. Metodlar

Metodlar `camelCase()` formatında bildirilmelidir.

