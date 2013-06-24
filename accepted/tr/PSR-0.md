Aşağıdakiler otomatik yükleyici ile birlikte çalışabilmesi için 
zorunlu gereksinimleri açıklamaktadır

Zorunluluklar
-------------

* Tam nitelikli namespace ve sınıf yapısı aşağıdaki gibi olmalıdır.
  `\<Vendor Name>\(<Namespace>\)*<Class Name>`
* Herbir namespace'in bir üst seviye namespace'i olmalıdır. 
  ("Vendor Name")
* Her bir namespace'in istediğin gibi bir çok alt namespace'i olabilir.
* Dosya sistemi yüklenirken her namespace ayracı `DIRECTORY_SEPARATOR`e 
  dönüştürülür.
* Sınıf adındaki her bir `_` karakteri `DIRECTORY_SEPARATOR`e dönüştürülür.
  Namespace'deki `_` karakterinin özel bir anlamı yoktur.
* Tam nitelikli namespace ve sınıf dosya sisteminden yüklenirken sonuna 
  `.php` eklenir.
* Vendor adı, namespace ve sınıflardaki alfabetik karakterler büyük 
  küçük harflerin herhangi bir şekilde biraraya gelmesinden oluşabilir.

Örnekler
--------

* `\Doctrine\Common\IsolatedClassLoader` => `/path/to/project/lib/vendor/Doctrine/Common/IsolatedClassLoader.php`
* `\Symfony\Core\Request` => `/path/to/project/lib/vendor/Symfony/Core/Request.php`
* `\Zend\Acl` => `/path/to/project/lib/vendor/Zend/Acl.php`
* `\Zend\Mail\Message` => `/path/to/project/lib/vendor/Zend/Mail/Message.php`

Sınıf ve Namespace İsimlerinde Altçizgi
---------------------------------------

* `\namespace\package\Class_Name` => `/path/to/project/lib/vendor/namespace/package/Class/Name.php`
* `\namespace\package_name\Class_Name` => `/path/to/project/lib/vendor/namespace/package_name/Class/Name.php`

Belirlenen standartlar otomatik yükleyici ile birlikte çalışabilirlik 
için en düşük ortak paydada olmalıdır. Örnek SplClassLoader uygulamasını 
kullanarak bu standartları test edebilirsiniz.

Örnek Uygulama
--------------

Aşağıda basit örnek bir fonksiyon ile yukarıdaki standartlara uygun bir otomatik 
yükleyicinin nasıl olduğunu görebilirsiniz.

```php
<?php
function autoload($className)
{
    $className = ltrim($className, '\\');
    $fileName  = '';
    $namespace = '';
    if ($lastNsPos = strrpos($className, '\\')) {
        $namespace = substr($className, 0, $lastNsPos);
        $className = substr($className, $lastNsPos + 1);
        $fileName  = str_replace('\\', DIRECTORY_SEPARATOR, $namespace) . DIRECTORY_SEPARATOR;
    }
    $fileName .= str_replace('_', DIRECTORY_SEPARATOR, $className) . '.php';

    require $fileName;
}
?>
```

SplClassLoader Uygulaması
-------------------------

Aşağıdaki gist basit bir SplClassLoader uygulamasını içermektedir. Bu 
uygulama sayesinde kendi sınıflarınızı daha önceki belirtilen standartlara 
uyduğunuz takdirde otomatik yükleyebilirsiniz. PHP 5.3 sınıflarınızı 
otomatik yüklemek için bu standartları takip edin. 

* [http://gist.github.com/221634](http://gist.github.com/221634)

