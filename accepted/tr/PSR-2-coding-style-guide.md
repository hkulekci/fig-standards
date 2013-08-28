Basit Kodlama Standartları
==========================

Bu rehber [PSR-1][] basit kodlama standartları hakkındadır.

The intent of this guide is to reduce cognitive friction when scanning code
from different authors. It does so by enumerating a shared set of rules and
expectations about how to format PHP code.

The style rules herein are derived from commonalities among the various member
projects. When various authors collaborate across multiple projects, it helps
to have one set of guidelines to be used among all those projects. Thus, the
benefit of this guide is not in the rules themselves, but in the sharing of
those rules.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119][].

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[PSR-1]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-1-basic-coding-standard.md


1. Genel Bakış
--------------

- Kod [PSR-1][] standartlerını takip etmelidir.

- Kod girintiler için 4 boşluk kullanmalıdır. Tab karakterini değil.

- Satır uzunluğu üzerine zorlayıcı bir sınır yoktur. ama zorunlu olmamakla birlikte 
120 karakter olması gerekir. satırlar 80 karakter veya daha az olabilir.

- `namespace` ve `use` ifadelerinin her birisi yeni bir satırda yapılmalıdır.

- Sınıf ifadelerinde açılan parantez bir sonraki satırda olmalıdır. Kapanan 
parantezde gövdenin bitiminden sonraki satırda olmalıdır. 

- Methodların açılan parantezleri tanımlamalardan sonraki satırda olmalıdır ve 
kapanan parantezleri de gövdenin bitiminden sonraki satırda olmalıdır.

- (Visibility)[http://www.php.net/manual/tr/language.oop5.visibility.php] her 
özellik ve method'da tanımlanmalıdır; `abstract` ve `final` görünürlülükten 
önce tanımlanmalıdır; `static` visibility'den sonra tanımlanmalıdır.

- (Kontrol yapısı)[http://php.net/manual/en/language.control-structures.php] 
kelimelerinden sonra bir karakter boşluk yazılmalıdır; method ve fonksiyon 
çağırmalarında yapılmamalıdır. 

- Kontrol yapılarında açılan gövde parantezi aynı satırda olmalıdır. Kapanan 
gövde parantezi gövdenin bitiminden sonraki satırda olmalıdır. 

- Kontrol parametrelerindeki kontrol parantezinden sonra boşluk olmamalıdır, 
ve kapanma parantezinden önce de boşluk olmamalıdır.


### 1.1. Örnek

BU örnek yukarıdaki bazı kuralları kapsayan bir örnektir: 

```php
<?php
namespace Vendor\Package;

use FooInterface;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class Foo extends Bar implements FooInterface
{
    public function sampleFunction($a, $b = null)
    {
        if ($a === $b) {
            bar();
        } elseif ($a > $b) {
            $foo->bar($arg1);
        } else {
            BazClass::bar($arg2, $arg3);
        }
    }

    final public static function bar()
    {
        // method body
    }
}
```

2. Genel
--------

### 2.1 Basit Kodlama Standartları

Kodlama [PSR-1][] standartlarına uymalıdır.

### 2.2 Dosyalar

Tüm PHP dosyaları Unix LF (linefeed) dosya sonunu kullanmalıdır.

Tüm dosyalar boş bir satır ile bitmelidir. 

`?>` kapatma etiketi sadece php içeren dosyalar için ihmal edilmedir.

### 2.3. Satırlar

Satır uzunluğu konusunda katı bir sınırlama yoktur.

Satır uzunluğu (soft limit) 120 karakter olmalıdır; otomatik stil kontrol 
araçları uyarı verecektir ama hata vermeyecektir.

Satılar 80 karakterden daha uzun olmayabilir; bundan uzun olduğu zamanlarda 
her biri 80 karakterden kısa satırlara ayrılabilir.

Dosyanın son satırında boşluk karakteri kullanılmamalıdır. Satır boş olmalıdır.

Boş satırlar kodun okunabilirliğini artırmak ve ilgili kod bloklarını 
göstermek için eklenebilir.

Satır başına birden fazla ifade olmamalıdır.

### 2.4. Girinti

4 boşluklu girinti kullanılmalıdır, ve girinti için tab kullanılmamalıdır. 

> Dikkat et: Sadece boşluk karakteri kullan, ve boşluk karakteri ve tab 
> karakterini karıştırma, önlemek için diff'lerden, yamalardan, geçmişden 
> ve açıklamalardan yararlanabilirsin. _The use of spaces_
> _also makes it easy to insert fine-grained sub-indentation for inter-line_
> _alignment._


### 2.5. Anahtar Sözcükler and True/False/Null

PHP [anahtar sözcükleri][] küçük karakterli kullanılmalıdır.

The PHP sabitleri olan `true`, `false`, ve `null` küçük karakterli kullanılmalıdır.

[anahtar sözcükleri]: http://php.net/manual/en/reserved.keywords.php



3. Namespace ve Use İfadeleri
-----------------------------

`namespace` ifadesinden sonra bir boşluk olmalıdır.

`use` ifadeleri `namespace` ifadelerinden sonra gelmelidir. 

Her bir tanımlama için bir `use` ifadesi kullanılmalıdır.

`use` bloklarından sonra bir boş satır olmalıdır.

Örneğin:

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

// ... additional PHP code ...
```


4. Sınıflar, Özellikler, ve Methodlar
-------------------------------------

"class" terimi bütün sınıflardan, arayüzlerden(interfaces) ve trait'lerden 
bahsetmektedir.

### 4.1. Extends ve Implements

`extends` ve `implements` anahtar sözcükleri  sınıf adı ile aynı satırda 
tanımlanmalıdır. 

Sınıf için açılış parantezi kendi satırında olmalıdır; ve kapanış parantezi 
gövdenin bitişinden sonraki satırda olmalıdır.

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements \ArrayAccess, \Countable
{
    // constants, properties, methods
}
```

_Lists of `implements` MAY be split across multiple lines, where each
subsequent line is indented once. When doing so, the first item in the list
MUST be on the next line, and there MUST be only one interface per line._

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements
    \ArrayAccess,
    \Countable,
    \Serializable
{
    // constants, properties, methods
}
```

### 4.2. Özellikler

Visibility(Görünürlük) her bir özellik için tanımlanmalıdır.

`var` anahtar sözcüğü özelliği tanımlamak için kullanılmamalıdır.

Her bir statement'da bir özellikden fazla özellik tanımlanmamalıdır. 

Özellik isimleri protected veya private görünürlüklerini göstermek için bir alt 
çizgi ile başlamamalıdır.

Bir özellik tanımı aşağıdaki gibi görünür.

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public $foo = null;
}
```

### 4.3. Metodlar

Her method için görünürlük tanımlanmalıdır.

Method isimlerinin görünürlüklerini göstermek için method isimlerine önek 
olarak altçizgi kullanılmamalıdır.

_Method names MUST NOT be declared with a space after the method name._ 
Açılış parantezi kendi satırında olmalı ve kapanma parantezi gövdeyinin bitişinden 
sonraki satırda olmalıdır. Açılış parantezinden sonra boşluk karakteri olmamalıdır
ve kapanma parantezinden önce boşluk karakteri olmamalıdır.

Bi metod tanımlaması aşağıdaki gibidir. Parantezlerin, virgüllerin, boşlukların ve
metod parantezlerinin yerlerini not ediniz:

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function fooBarBaz($arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
```    

### 4.4. Method Parametreleri

Parametre listesinde, her bir virgülden önce bir boşluk karakteri olmamalıdır, ve
virgülden sonra bir boşluk karakteri olmalıdır. 

Varsayılan değerli metod parametreleri parametre listesinin sonunda olmalıdır.

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function foo($arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
```

Parametre listesi bir kaç satıra bölünebilir, ve her bir satırın başında girintili 
olmalıdır. Bunu yaparken birinci madde bir sonraki satırda olmalıdır ve her parametre 
bir satırda olmalıdır.

Parametreler birden fazla satıra bölündüğünde metodun açılış parantezi parametrelerin 
kapanış parantezi ile aynı satırda olmalıdır ve aralarında bir boşluk karakteri olmalıdır.

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function aVeryLongMethodName(
        ClassTypeHint $arg1,
        &$arg2,
        array $arg3 = []
    ) {
        // method body
    }
}
```

### 4.5. `abstract`, `final`, ve `static`

`abstract` ve `final` ifadeleri görünürlük ifadelerinden önce gelmelidir. 

`static` ifadesi görünürlük tanımlarından sonra gelmelidir.

```php
<?php
namespace Vendor\Package;

abstract class ClassName
{
    protected static $foo;

    abstract protected function zim();

    final public static function bar()
    {
        // method body
    }
}
```

### 4.6. Method ve Fonksiyonları Çağırmak

Bir fonksiyon ya da metod çağrılırken, metod ve fonksiyon ismi ile parametrelerin 
açılış parantezleri arasında boşluk bulunmamalıdır, parametrelerin açılış parantezinden
sonra da boşluk bulunmamalıdır, ve parametrelerin kapanış parantezinden önce de bir 
boşluk karakteri kullanılmamalıdır.Parametre listesinde, her bir virgülden önce 
boşluk olmamalıdır ve her virgülden sonra bir boşluk karakteri olmalıdır. 

```php
<?php
bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
```

Parametreler her birinin başında bir girinti olacak şekilde birden fazla satıra 
bölünebilir. Bunu yaparken, birinci parametre bir sonraki satırda olmalıdır, ve 
her bir satırda sadece bir parametre olmalıdır.

```php
<?php
$foo->bar(
    $longArgument,
    $longerArgument,
    $muchLongerArgument
);
```

5. Kontrol Yapıları
-------------------

Kontrol yapıları için genel kurallar aşağıdaki gibidir: 

- Kontrol yapısı anahtar sözcüğünden sonra bir boşluk gelmelidir.
- Kontrol yapısı açılış parantezinden sonra boşluk bulunmamalıdır.
- Kontrol yapısı kapanış parantezinden önce boşluk bulunmamalıdır.
- Kontrol yapısı kapanış parantezi ile kontrol yapısı gövdesinin 
  açılış parantezi arasında bir boşluk karakteri olmalıdır.
- Kontrol yapısı gövdesi bir girinti boyutunda içerde olmalıdır. 
- Kontrol yapısı gövde kapanış parantezi gövdenin bitişinden sonraki 
  satırda olmalıdır.

Her kontrol yapısının gövdesi parantez içerisinde olmalıdır. _This 
standardizes how the structures look, and reduces the likelihood of 
introducing errors as new lines get added to the body._


### 5.1. `if`, `elseif`, `else`

Bir `if` yapısı aşağıdaki gibi görünür. Parantezlerin, boşlukların yerlerini
not edin; ve `else` ve `elseif` ifadeleri açılış ve önceki ifadenin kapanış 
parantezleri ile aynı satırdadir.  

```php
<?php
if ($expr1) {
    // if body
} elseif ($expr2) {
    // elseif body
} else {
    // else body;
}
```
`elseif` anahtar sözcüğü `else if` yerine kullanılabilir, böylece tüm kontrol 
anahtar sözcükleri tek bir kelime gibi görünür.

### 5.2. `switch`, `case`

Bir `switch` yapısı aşağıdaki gibidir. Parantezlerin, boşlukların yerlerini
not edin. `case` ifadesi `switch` ifadesinden bir tab karakteri kadar içerde 
olmalıdır ve `break` anahtar kelimesi (diğer bir durdurma anahtar kelimesi)
`case` gövdesi ile aynı seviye girintide olmalıdır. Boş olmayan bir `case` 
gövdesinde `// no break` yorum satırı olmalıdır.

```php
<?php
switch ($expr) {
    case 0:
        echo 'First case, with a break';
        break;
    case 1:
        echo 'Second case, which falls through';
        // no break
    case 2:
    case 3:
    case 4:
        echo 'Third case, return instead of break';
        return;
    default:
        echo 'Default case';
        break;
}
```


### 5.3. `while`, `do while`

Bir `while` ifadesi aşağıdaki gibi görünür. Parantezleri ve boşlukları not 
ediniz.

```php
<?php
while ($expr) {
    // structure body
}
```

Aynı şekilde, bir `do while` ifadesi de aşağıdaki gibi görünür. Parantezleri 
ve boşlukları not ediniz.

```php
<?php
do {
    // structure body;
} while ($expr);
```

### 5.4. `for`

Bir `for` ifadesi aşağıdaki gibi görünür. Parantezlere ve boşluklara dikkat 
ediniz.

```php
<?php
for ($i = 0; $i < 10; $i++) {
    // for body
}
```

### 5.5. `foreach`

Bir `foreach` ifadesi aşağıdaki gibi görünür. Parantezlere ve boşluklara dikkat 
ediniz.

```php
<?php
foreach ($iterable as $key => $value) {
    // foreach body
}
```

### 5.6. `try`, `catch`

Bir `try catch` ifadesi aşağıdaki gibi görünür. Parantezlere ve boşluklara dikkat 
ediniz.

```php
<?php
try {
    // try body
} catch (FirstExceptionType $e) {
    // catch body
} catch (OtherExceptionType $e) {
    // catch body
}
```

6. "Closures"lar
-----------

"Closure"lar `function` özel anahtarından bir boşluk sonra belirtilmelidir, ve `use` 
özel anahtarından önce ve sonra bir boşluk bulunmalıdır. 

"Closure" gövdesi açılış parantezi aynı satırda olmalıdır, ve kapatma parantezi gövdenin
bitiminden sonraki yeni satırda olmalıdır.

Parametre ve değişken listesi açılış parantezinden sonra bir boşluk olmamalıdır ve aynı 
listenin kapanış parantezinden önce bir boşluk olmamalıdır.

Parametre ve değişken listesinde, her virgülden önce boşluk olmamalıdır ve her virgülden 
sonra bir boşluk karakteri olmalıdır.

Closure'ın varsayılan değeri olan parametreleri parametre listesinin sonunda olmalıdır.

Bir closure aşağıdaki gibi tanımlanmalıdır. Parantezlere, virgüllere ve boşluklara 
dikkat edin.

```php
<?php
$closureWithArgs = function ($arg1, $arg2) {
    // body
};

$closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {
    // body
};
```
Parametre ve değişken listesi birden fazla satıra bölüne bölünebilir, her bir alt satır 
bir girinti içerde olur. Bunu yaparken, listedeki ilk eleman bir sonraki satırda olmalıdır,
ve her satırda bir parametre ya da değişken olmalıdır.

Liste sonlandığı zaman, parametrelerin kapanış parantezi ile closure gövedesinin açılış 
parantezi aynı satırda ve yeni bir satırda olur. Bu iki parantezin aralarında bir boşluk
karakteri olur.

Bir parametreli, parametresiz, değişkenli ve değişkensiz "closure"ların örnekleri 
aşağıdaki gibi tanımlanmalıdır. Parantezlere, virgüllere ve boşluklara dikkat edin.

```php
<?php
$longArgs_noVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) {
   // body
};

$noArgs_longVars = function () use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_longVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_shortVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use ($var1) {
   // body
};

$shortArgs_longVars = function ($arg) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};
```

Closure metodunu bir fonskiyona parametre olarak gönderirken de düzen 
kurallarına dikkat edin.

```php
<?php
$foo->bar(
    $arg1,
    function ($arg2) use ($var1) {
        // body
    },
    $arg3
);
```


7. Sonuç
--------------

Bir çok elamanın stili ve uygulaması bu rehberde kasıtlı olarak ihmal 
edilmiştir. BUnlar arasında ama ancak bunlarla sınırlı olmayanlardan bazıları:

- Global değişkenler ve global sabitlerin tanımlanması

- Fonksiyonların tanımlanması

- Operatörler ve atamaları

- Inter-line hizalama

- Yorumlar ve dökümantasyon blokları

- Sınıf isimleri önekleri ve sonekleri

- En iyi uygulamalar

Gelecekteki önerilerde bu diğer elemanların stilleri ve uygulamarı revize edilebilir 
ve genişletilebilir.


Ek A. Survey
------------------

In writing this style guide, the group took a survey of member projects to
determine common practices.  The survey is retained herein for posterity.

### A.1. Survey Data

    url,http://www.horde.org/apps/horde/docs/CODING_STANDARDS,http://pear.php.net/manual/en/standards.php,http://solarphp.com/manual/appendix-standards.style,http://framework.zend.com/manual/en/coding-standard.html,http://symfony.com/doc/2.0/contributing/code/standards.html,http://www.ppi.io/docs/coding-standards.html,https://github.com/ezsystems/ezp-next/wiki/codingstandards,http://book.cakephp.org/2.0/en/contributing/cakephp-coding-conventions.html,https://github.com/UnionOfRAD/lithium/wiki/Spec%3A-Coding,http://drupal.org/coding-standards,http://code.google.com/p/sabredav/,http://area51.phpbb.com/docs/31x/coding-guidelines.html,https://docs.google.com/a/zikula.org/document/edit?authkey=CPCU0Us&hgd=1&id=1fcqb93Sn-hR9c0mkN6m_tyWnmEvoswKBtSc0tKkZmJA,http://www.chisimba.com,n/a,https://github.com/Respect/project-info/blob/master/coding-standards-sample.php,n/a,Object Calisthenics for PHP,http://doc.nette.org/en/coding-standard,http://flow3.typo3.org,https://github.com/propelorm/Propel2/wiki/Coding-Standards,http://developer.joomla.org/coding-standards.html
    voting,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,no,no,no,?,yes,no,yes
    indent_type,4,4,4,4,4,tab,4,tab,tab,2,4,tab,4,4,4,4,4,4,tab,tab,4,tab
    line_length_limit_soft,75,75,75,75,no,85,120,120,80,80,80,no,100,80,80,?,?,120,80,120,no,150
    line_length_limit_hard,85,85,85,85,no,no,no,no,100,?,no,no,no,100,100,?,120,120,no,no,no,no
    class_names,studly,studly,studly,studly,studly,studly,studly,studly,studly,studly,studly,lower_under,studly,lower,studly,studly,studly,studly,?,studly,studly,studly
    class_brace_line,next,next,next,next,next,same,next,same,same,same,same,next,next,next,next,next,next,next,next,same,next,next
    constant_names,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper
    true_false_null,lower,lower,lower,lower,lower,lower,lower,lower,lower,upper,lower,lower,lower,upper,lower,lower,lower,lower,lower,upper,lower,lower
    method_names,camel,camel,camel,camel,camel,camel,camel,camel,camel,camel,camel,lower_under,camel,camel,camel,camel,camel,camel,camel,camel,camel,camel
    method_brace_line,next,next,next,next,next,same,next,same,same,same,same,next,next,same,next,next,next,next,next,same,next,next
    control_brace_line,same,same,same,same,same,same,next,same,same,same,same,next,same,same,next,same,same,same,same,same,same,next
    control_space_after,yes,yes,yes,yes,yes,no,yes,yes,yes,yes,no,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes
    always_use_control_braces,yes,yes,yes,yes,yes,yes,no,yes,yes,yes,no,yes,yes,yes,yes,no,yes,yes,yes,yes,yes,yes
    else_elseif_line,same,same,same,same,same,same,next,same,same,next,same,next,same,next,next,same,same,same,same,same,same,next
    case_break_indent_from_switch,0/1,0/1,0/1,1/2,1/2,1/2,1/2,1/1,1/1,1/2,1/2,1/1,1/2,1/2,1/2,1/2,1/2,1/2,0/1,1/1,1/2,1/2
    function_space_after,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no
    closing_php_tag_required,no,no,no,no,no,no,no,no,yes,no,no,no,no,yes,no,no,no,no,no,yes,no,no
    line_endings,LF,LF,LF,LF,LF,LF,LF,LF,?,LF,?,LF,LF,LF,LF,?,,LF,?,LF,LF,LF
    static_or_visibility_first,static,?,static,either,either,either,visibility,visibility,visibility,either,static,either,?,visibility,?,?,either,either,visibility,visibility,static,?
    control_space_parens,no,no,no,no,no,no,yes,no,no,no,no,no,no,yes,?,no,no,no,no,no,no,no
    blank_line_after_php,no,no,no,no,yes,no,no,no,no,yes,yes,no,no,yes,?,yes,yes,no,yes,no,yes,no
    class_method_control_brace,next/next/same,next/next/same,next/next/same,next/next/same,next/next/same,same/same/same,next/next/next,same/same/same,same/same/same,same/same/same,same/same/same,next/next/next,next/next/same,next/same/same,next/next/next,next/next/same,next/next/same,next/next/same,next/next/same,same/same/same,next/next/same,next/next/next

### A.2. Survey Legend

`indent_type`:
The type of indenting. `tab` = "Use a tab", `2` or `4` = "number of spaces"

`line_length_limit_soft`:
The "soft" line length limit, in characters. `?` = not discernible or no response, `no` means no limit.

`line_length_limit_hard`:
The "hard" line length limit, in characters. `?` = not discernible or no response, `no` means no limit.

`class_names`:
How classes are named. `lower` = lowercase only, `lower_under` = lowercase with underscore separators, `studly` = StudlyCase.

`class_brace_line`:
Does the opening brace for a class go on the `same` line as the class keyword, or on the `next` line after it?

`constant_names`:
How are class constants named? `upper` = Uppercase with underscore separators.

`true_false_null`:
Are the `true`, `false`, and `null` keywords spelled as all `lower` case, or all `upper` case?

`method_names`:
How are methods named? `camel` = `camelCase`, `lower_under` = lowercase with underscore separators.

`method_brace_line`:
Does the opening brace for a method go on the `same` line as the method name, or on the `next` line?

`control_brace_line`:
Does the opening brace for a control structure go on the `same` line, or on the `next` line?

`control_space_after`:
Is there a space after the control structure keyword?

`always_use_control_braces`:
Do control structures always use braces?

`else_elseif_line`:
When using `else` or `elseif`, does it go on the `same` line as the previous closing brace, or does it go on the `next` line?

`case_break_indent_from_switch`:
How many times are `case` and `break` indented from an opening `switch` statement?

`function_space_after`:
Do function calls have a space after the function name and before the opening parenthesis?

`closing_php_tag_required`:
In files containing only PHP, is the closing `?>` tag required?

`line_endings`:
What type of line ending is used?

`static_or_visibility_first`:
When declaring a method, does `static` come first, or does the visibility come first?

`control_space_parens`:
In a control structure expression, is there a space after the opening parenthesis and a space before the closing parenthesis? `yes` = `if ( $expr )`, `no` = `if ($expr)`.

`blank_line_after_php`:
Is there a blank line after the opening PHP tag?

`class_method_control_brace`:
A summary of what line the opening braces go on for classes, methods, and control structures.

### A.3. Survey Results

    indent_type:
        tab: 7
        2: 1
        4: 14
    line_length_limit_soft:
        ?: 2
        no: 3
        75: 4
        80: 6
        85: 1
        100: 1
        120: 4
        150: 1
    line_length_limit_hard:
        ?: 2
        no: 11
        85: 4
        100: 3
        120: 2
    class_names:
        ?: 1
        lower: 1
        lower_under: 1
        studly: 19
    class_brace_line:
        next: 16
        same: 6
    constant_names:
        upper: 22
    true_false_null:
        lower: 19
        upper: 3
    method_names:
        camel: 21
        lower_under: 1
    method_brace_line:
        next: 15
        same: 7
    control_brace_line:
        next: 4
        same: 18
    control_space_after:
        no: 2
        yes: 20
    always_use_control_braces:
        no: 3
        yes: 19
    else_elseif_line:
        next: 6
        same: 16
    case_break_indent_from_switch:
        0/1: 4
        1/1: 4
        1/2: 14
    function_space_after:
        no: 22
    closing_php_tag_required:
        no: 19
        yes: 3
    line_endings:
        ?: 5
        LF: 17
    static_or_visibility_first:
        ?: 5
        either: 7
        static: 4
        visibility: 6
    control_space_parens:
        ?: 1
        no: 19
        yes: 2
    blank_line_after_php:
        ?: 1
        no: 13
        yes: 8
    class_method_control_brace:
        next/next/next: 4
        next/next/same: 11
        next/same/same: 1
        same/same/same: 6
